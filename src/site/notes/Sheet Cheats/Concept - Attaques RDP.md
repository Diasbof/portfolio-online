---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-attaques-rdp/"}
---

# Attaques RDP (Port 3389)
> [!ABSTRACT] Définition
> Le protocole RDP (Remote Desktop Protocol), opérant par défaut sur le port TCP/UDP 3389, est la solution native de Microsoft pour l'accès graphique à distance. En raison de sa large adoption pour l'administration des serveurs, il constitue l'un des vecteurs d'accès initial privilégiés par les groupes criminels, notamment pour le déploiement de ransomwares.
## Vecteurs d'Attaque Principaux

1. **Force Brute et Pulvérisation de mots de passe :** L'exposition directe du port 3389 sur Internet ou sur un réseau local non segmenté permet aux attaquants d'automatiser des tentatives de connexion massives (via des outils comme Hydra ou Crowbar) afin de compromettre des comptes administrateurs faiblement protégés.
2. **Exploitation de vulnérabilités (ex: BlueKeep) :** Des failles critiques historiques, telles que BlueKeep (CVE-2019-0708), permettent une exécution de code à distance (RCE) sans authentification préalable, en exploitant la gestion des canaux virtuels du protocole RDP.
3. **Détournement de session (Session Hijacking) :** Un attaquant ayant obtenu des privilèges locaux (SYSTEM) sur un serveur peut utiliser des utilitaires natifs (comme `tscon.exe`) pour s'attacher à la session RDP active ou déconnectée d'un autre utilisateur (par exemple, un Administrateur du Domaine), sans connaître son mot de passe.
4. **Attaques Man-in-the-Middle (MitM) :** Si l'authentification au niveau du réseau (NLA) n'est pas imposée, un attaquant peut intercepter la négociation de la connexion et potentiellement forcer une rétrogradation (downgrade) du niveau de chiffrement.
## Remédiations & Détection
- **Durcissement de la configuration :** Imposer systématiquement l'authentification au niveau du réseau (NLA - Network Level Authentication) via les stratégies de groupe (GPO) pour exiger l'authentification avant l'établissement de la session RDP.
- **Restriction des accès :** Ne jamais exposer le port 3389 directement sur Internet. Utiliser une passerelle RDP (RD Gateway), un VPN, et filtrer les accès au niveau du pare-feu pour n'autoriser que les adresses IP d'administration légitimes.
- **Délégation stricte :** Limiter l'appartenance au groupe local "Utilisateurs du Bureau à distance" et interdire aux administrateurs de domaine l'utilisation du RDP sur des postes de travail standards pour prévenir le vol d'identifiants.
- **Supervision Wazuh :** Configurer les agents pour monitorer les événements de connexion réseau. Cibler l'Event ID 4625 (Échec de connexion) pour la détection des attaques par force brute. L'Event ID 4624 avec le "Logon Type 10" (Interactive Remote) ou "Logon Type 7" (Reconnect) permet de tracer les accès réussis. Une alerte critique doit également être levée lors de l'exécution suspecte du binaire `tscon.exe`.
---
Note : La sécurisation des accès RDP est une priorité absolue dans la protection contre les mouvements latéraux et le déploiement de codes malveillants de type rançongiciel.