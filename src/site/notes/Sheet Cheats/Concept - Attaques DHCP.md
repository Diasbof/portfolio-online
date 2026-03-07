---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-attaques-dhcp/"}
---

# Attaques DHCP (Attribution d'IP)
> [!ABSTRACT] Définition
> Le protocole DHCP (Dynamic Host Configuration Protocol), opérant sur les ports UDP 67 et 68, permet l'attribution dynamique des adresses IP et de la configuration réseau (passerelle, serveurs DNS) aux postes clients. Sa compromission permet à un attaquant de manipuler le routage du trafic légitime et de faciliter des attaques d'interception globales.
## Vecteurs d'Attaque Principaux

1. **DHCP Starvation (Famine) :** L'attaquant inonde le serveur DHCP légitime de requêtes d'attribution en générant continuellement de nouvelles adresses MAC spoofées. L'objectif est d'épuiser le pool d'adresses IP disponibles, provoquant un déni de service (DoS) pour les nouveaux équipements tentant de rejoindre le réseau.
2. **Rogue DHCP (Serveur Illicite) :** Souvent consécutive à une attaque par famine, cette technique consiste à introduire un serveur DHCP non autorisé sur le segment réseau. Ce serveur distribue des configurations réseau altérées, désignant la machine de l'attaquant comme passerelle par défaut ou serveur DNS, établissant ainsi une position de Man-in-the-Middle (MitM).
3. **IPv6 Spoofing (ex: mitm6) :** Sur les systèmes d'exploitation modernes, l'IPv6 est activé et priorisé par défaut, même s'il n'est pas explicitement configuré par les administrateurs. Un attaquant peut répondre aux requêtes DHCPv6 pour s'imposer comme serveur DNS principal, forçant ainsi les clients Windows à s'authentifier auprès de lui (facilitant le relais NTLM ou l'usurpation de l'intranet).
## Remédiations & Détection
- **Sécurisation des commutateurs (DHCP Snooping) :** Déployer cette fonctionnalité sur les équipements réseau afin de définir les ports de confiance exclusifs autorisés à diffuser des offres DHCP, bloquant ainsi matériellement l'apparition de serveurs illicites.
- **Contrôle d'accès réseau :** Activer la limitation des adresses MAC par port (Port Security ou 802.1X) pour atténuer instantanément les attaques par famine.
- **Désactivation d'IPv6 :** Si le protocole IPv6 n'est pas géré de manière sécurisée par l'infrastructure, il doit être désactivé sur les cartes réseau des serveurs et des postes de travail via les stratégies de groupe (GPO).
- **Supervision Wazuh :** Analyser les journaux du rôle Serveur DHCP sous Windows Server 2019. Configurer des alertes spécifiques sur l'Event ID 1020 (Pool d'adresses IP épuisé). Il est également pertinent de monitorer le trafic réseau local pour détecter la présence de requêtes UDP 67/68 provenant d'adresses IP n'appartenant pas aux serveurs d'infrastructure approuvés.
---
Note : La sécurisation du service d'attribution IP est fondamentale pour prévenir le détournement des flux réseau dès l'initialisation de la connexion du poste client.