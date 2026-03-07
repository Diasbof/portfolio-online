---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-attaque-golden-ticket/"}
---

# L'Attaque Golden Ticket
> [!ABSTRACT] Définition
> L'attaque Golden Ticket consiste à forger un ticket d'octroi de tickets (TGT - Ticket Granting Ticket) Kerberos valide. Cette compromission totale permet à un attaquant de s'octroyer des privilèges d'administrateur du domaine de manière persistante, en contournant les mécanismes d'authentification standard du Contrôleur de Domaine.
## Mécanisme Technique
Le protocole Kerberos repose sur un compte de service clé au sein de l'Active Directory : le compte `krbtgt`. Ce compte détient la clé cryptographique utilisée pour signer et chiffrer tous les TGT émis dans le domaine. Si un attaquant parvient à extraire le condensé (hash NTLM) ou les clés AES de ce compte (généralement via une attaque [[Sheet Cheats/Concept - Attaque DCSync\|DCSync]]), il devient capable de générer ses propres TGT. Le Contrôleur de Domaine acceptera ces tickets forgés comme légitimes puisqu'ils sont signés avec la clé cryptographique officielle.
## Exploitation Opérationnelle
1. **Extraction :** Récupération du hash du compte `krbtgt` suite à une élévation de privilèges (obtention préalable des droits Administrateur du Domaine).
2. **Forge :** Création d'un ticket TGT hors ligne à l'aide d'outils tels que Mimikatz (`kerberos::golden`). L'attaquant définit arbitrairement l'identité de l'utilisateur (y compris un compte inexistant), les groupes d'appartenance (ex: Administrateurs de l'entreprise) et la durée de validité du ticket (pouvant s'étendre sur plusieurs années).
3. **Injection :** Le ticket est chargé en mémoire (technique du Pass-the-Ticket) pour accéder à n'importe quelle ressource du système d'information de manière indétectable par les protections locales.
## Remédiations & Détection
- **Rotation des clés (Remédiation) :** La seule méthode pour invalider un Golden Ticket est de modifier le mot de passe du compte `krbtgt`. Cette opération doit être effectuée **deux fois consécutives** (en respectant le délai de réplication inter-sites) afin de purger l'historique des mots de passe conservé par Windows Server 2019.
- **Supervision Wazuh (Détection) :** La détection d'un ticket d'or est complexe car le trafic réseau généré s'apparente à du trafic légitime. Il est nécessaire de configurer des règles de corrélation avancées. Celles-ci doivent cibler l'Event ID 4624 (Logon) et l'Event ID 4634 (Logoff) en recherchant des incohérences : un nom de compte n'existant pas dans l'annuaire LDAP, ou une durée de validité du TGT dépassant la politique par défaut du domaine (habituellement 10 heures).
- **Audit des privilèges :** Restreindre strictement l'accès aux Contrôleurs de Domaine (Tier 0) pour empêcher l'extraction initiale du secret de distribution de clés.
---
👉 *Technique de persistance avancée démontrant la nécessité d'une rotation régulière des secrets d'infrastructure.*