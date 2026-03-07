---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-as-rep-roasting/"}
---

# AS-REP Roasting
> [!ABSTRACT] Définition
> L'AS-REP Roasting est une technique d'accès initial ou d'élévation de privilèges ciblant une erreur de configuration spécifique des comptes Active Directory. Elle exploite l'absence de pré-authentification Kerberos pour récupérer un message chiffré contenant le secret de l'utilisateur, en vue d'un cassage hors-ligne.
## Mécanisme Technique
Par défaut, le protocole Kerberos exige une étape de pré-authentification : l'utilisateur doit chiffrer un horodatage avec son mot de passe pour prouver son identité avant que le Contrôleur de Domaine ne lui délivre un Ticket d'Octroi de Ticket (TGT). Si l'attribut de compte "Ne pas exiger la pré-authentification Kerberos" est activé (souvent pour des raisons de rétrocompatibilité applicative), n'importe quel attaquant peut formuler une requête d'authentification (AS-REQ) pour cet utilisateur. Le serveur répondra par un message (AS-REP) contenant une partie chiffrée avec le mot de passe de la cible.
## Exploitation Opérationnelle
1. **Identification :** Requête LDAP visant à lister les comptes dont la propriété `DONT_REQ_PREAUTH` est définie dans l'attribut `userAccountControl`.
2. **Récupération :** Envoi d'une requête Kerberos factice (via Impacket `GetNPUsers.py` ou Rubeus) pour obtenir le hash AS-REP. Cette étape ne nécessite aucun accès préalable au domaine si l'attaquant est en mesure de joindre le port TCP/UDP 88 du Contrôleur de Domaine.
3. **Cassage :** Attaque par dictionnaire hors-ligne (Hashcat mode 18200) pour dériver le mot de passe en clair.
## Remédiations & Détection
- **Audit des configurations :** Identifier et décocher systématiquement l'option "Ne pas exiger la pré-authentification Kerberos" dans les propriétés des comptes de l'annuaire.
- **Supervision Wazuh :** Analyser les événements de sécurité du Contrôleur de Domaine. Surveiller l'Event ID 4768 (Un ticket TGT a été demandé) en corrélant l'absence de données de pré-authentification (Pre-Authentication Type : 0) avec les comptes sensibles de l'infrastructure.
---
Note : Vecteur d'attaque direct exploitant une simple case à cocher dans les paramètres de gestion de l'annuaire.