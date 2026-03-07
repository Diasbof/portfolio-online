---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-attaques-ftp/"}
---

# Attaques FTP (Port 21)
> [!ABSTRACT] Définition
> Le protocole FTP (File Transfer Protocol), opérant par défaut sur le port TCP 21, est un standard historique dédié au transfert de fichiers sur un réseau. Sa conception originelle ne prévoyant aucun mécanisme de chiffrement, il est aujourd'hui considéré comme vulnérable et expose l'infrastructure à de multiples risques de compromission.
## Vecteurs d'Attaque Principaux

1. **Interception réseau (Sniffing) :** L'intégralité des échanges, incluant les identifiants de connexion et le contenu des fichiers, circule en clair. Un attaquant en position d'intercepteur (Man-in-the-Middle) peut capturer ces informations à l'aide d'un analyseur de trames réseau.
2. **Authentification Anonyme :** Une erreur de configuration fréquente permet la connexion au serveur via le compte "Anonymous" sans exiger de mot de passe. Cela offre un accès direct au système de fichiers, parfois avec des droits d'écriture, facilitant le dépôt de charges malveillantes.
3. **Force Brute et Pulvérisation de mots de passe :** L'absence fréquente de politique de verrouillage de compte (Account Lockout) sur les services FTP standards permet aux attaquants de tester massivement des combinaisons d'identifiants via des scripts automatisés.
4. **FTP Bounce Attack :** Vulnérabilité historique permettant à un attaquant d'exploiter la commande PORT d'un serveur FTP mal configuré pour s'en servir de relais. Cela permet de scanner d'autres machines sur le réseau interne ou d'exfiltrer des données en contournant les restrictions du pare-feu.
## Remédiations & Détection
- **Transition cryptographique :** Remplacer le protocole FTP standard par des alternatives robustes telles que SFTP (qui s'appuie sur le protocole SSH) ou FTPS (qui intègre une couche de chiffrement TLS/SSL).
- **Configuration stricte :** Sur des environnements comme IIS sous Windows Server 2019, désactiver explicitement les accès anonymes et imposer l'isolation des utilisateurs (Chroot) afin de restreindre la navigation au seul répertoire autorisé.
- **Supervision Wazuh :** Déployer des règles de détection spécifiques pour identifier les tentatives de force brute (échecs d'authentification multiples et rapprochés sur le port 21) et monitorer les transferts de fichiers volumineux ou inhabituels.
---
Note : Document de synthèse justifiant la dépréciation des protocoles en clair au sein du système d'information.