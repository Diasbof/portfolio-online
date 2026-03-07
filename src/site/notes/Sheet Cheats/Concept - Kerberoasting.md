---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-kerberoasting/"}
---

# L'Attaque Kerberoasting
> [!ABSTRACT] Définition
> Le Kerberoasting est une technique d'élévation de privilèges ciblant les comptes de service de l'Active Directory. Elle permet à un utilisateur standard, déjà authentifié sur le domaine, de demander un ticket de service (TGS) et d'en extraire le condensé cryptographique pour le casser hors-ligne, révélant ainsi le mot de passe en clair du compte de service.
## Mécanisme Technique

Dans l'architecture Kerberos, lorsqu'un utilisateur souhaite accéder à un service (ex: base de données SQL), il demande au Contrôleur de Domaine un ticket TGS (Ticket Granting Service). Ce ticket est chiffré avec le condensé (hash) du mot de passe du compte exécutant ce service, identifié par son attribut SPN (Service Principal Name). L'attaquant interroge l'annuaire LDAP pour lister tous les comptes possédant un SPN, demande des tickets TGS pour chacun d'eux, puis les exporte pour exécuter une attaque par force brute ou par dictionnaire hors-ligne.
## Exploitation Opérationnelle
1. **Énumération :** Utilisation d'outils (ex: BloodHound, PowerView) pour identifier les comptes utilisateurs ayant un attribut `servicePrincipalName` non nul.
2. **Extraction :** Demande légitime de tickets TGS via des modules comme Invoke-Kerberoast ou Rubeus, puis enregistrement des tickets en mémoire au format `.kirbi` ou directement sous forme de hash.
3. **Cassage (Cracking) :** Utilisation d'outils spécialisés (ex: Hashcat, John the Ripper) sur une machine contrôlée par l'attaquant pour déchiffrer le ticket et obtenir le mot de passe.
## Remédiations & Détection
- **Mots de passe robustes :** Imposer des mots de passe générés aléatoirement et d'une longueur supérieure à 25 caractères pour tous les comptes de service, rendant le cassage hors-ligne irréalisable.
- **Group Managed Service Accounts (gMSA) :** Remplacer les comptes de service standards par des comptes gMSA, dont la rotation des mots de passe complexes est gérée automatiquement par l'Active Directory.
- **Supervision Wazuh :** Configurer les agents pour monitorer l'Event ID 4769 (Demande de ticket de service Kerberos). L'émission d'une alerte est nécessaire si le ticket utilise un algorithme de chiffrement faible (ex: Ticket Encryption Type 0x17 correspondant à RC4) ou si un utilisateur standard demande un nombre anormalement élevé de TGS en peu de temps.
---
Note : Technique critique démontrant le risque lié aux mots de passe faibles sur des comptes possédant souvent des privilèges d'administration sur les serveurs cibles.