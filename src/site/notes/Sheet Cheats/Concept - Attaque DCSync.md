---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-attaque-dc-sync/"}
---

# L'Attaque DCSync
> [!ABSTRACT] Définition
> L'attaque DCSync est une technique permettant à un attaquant de simuler le comportement d'un Contrôleur de Domaine (DC) afin de demander la réplication des données de l'Active Directory. Cela permet d'extraire les condensés cryptographiques (hashs) des mots de passe de n'importe quel utilisateur, y compris les comptes critiques comme `krbtgt`.
## Mécanisme Technique

L'Active Directory utilise le protocole MS-DRSR (Directory Replication Service Remote Protocol) pour synchroniser les données entre les différents Contrôleurs de Domaine. L'attaque DCSync exploite ce fonctionnement légitime en envoyant une requête `GetNCChanges` au DC cible. Pour que l'attaque réussisse, le compte compromis par l'attaquant doit posséder des privilèges de réplication spécifiques sur la racine du domaine, notamment `Replicating Directory Changes` et `Replicating Directory Changes All` (généralement détenus par les Administrateurs du Domaine ou les Contrôleurs de Domaine).
## Exploitation Opérationnelle
1. **Prérequis :** Obtention préalable de privilèges élevés ou compromission d'un compte ayant des droits de réplication mal configurés via l'analyse des ACL.
2. **Exécution :** Utilisation d'outils comme Mimikatz (`lsadump::dcsync`) ou Impacket (`secretsdump.py`) pour interroger le Contrôleur de Domaine via l'API DRSUAPI.
3. **Extraction :** Récupération du hash NTLM ou des clés Kerberos du compte ciblé (par exemple, le compte de service de distribution de clés pour forger un ticket d'or).
## Remédiations & Détection
- **Audit des ACL (Remédiation) :** Vérifier régulièrement les permissions à la racine du domaine pour s'assurer que seuls les groupes légitimes (Domain Controllers, Enterprise Admins) possèdent les droits de réplication de l'annuaire. Les permissions excessives (vulnérabilités de type misconfiguration) doivent être révoquées.
- **Supervision Wazuh (Détection) :** Configurer les règles SIEM pour surveiller l'Event ID 4662 (Accès à un objet du service d'annuaire) sur les Contrôleurs de Domaine Windows Server 2019. L'alerte doit se déclencher si l'accès implique les GUID de réplication (notamment `1131f6aa-9c07-11d1-f79f-00c04fc2dcd2`) et provient d'une adresse IP ou d'un utilisateur n'appartenant pas à la liste des Contrôleurs de Domaine autorisés.
- **Analyse Réseau :** Détecter le trafic RPC atypique lié au protocole MS-DRSR provenant de postes de travail standards au lieu de serveurs d'infrastructure.
---
👉 *Technique de post-exploitation illustrant le risque lié aux permissions excessives dans l'annuaire.*