---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-attaques-dns/"}
---

# Attaques DNS (Port 53)
> [!ABSTRACT] Définition
> Le service DNS (Domain Name System), fonctionnant sur le port 53 (TCP/UDP), est le système de résolution de noms central d'Internet et le pilier de l'architecture Active Directory. Il permet aux postes clients de localiser les Contrôleurs de Domaine et les services essentiels. Sa compromission met en péril l'intégrité de l'ensemble du système d'information.
## Vecteurs d'Attaque Principaux

1. **Transfert de Zone (AXFR) :** Une mauvaise configuration du serveur peut autoriser un client non légitime à demander une copie complète de la zone DNS. Cela offre à l'attaquant une cartographie précise de l'infrastructure interne (noms de serveurs, adresses IP associées).
2. **Empoisonnement de Cache (DNS Spoofing) :** L'attaquant insère de faux enregistrements dans le cache du résolveur DNS. Les requêtes légitimes des utilisateurs sont alors redirigées vers des serveurs contrôlés par l'attaquant, facilitant l'interception d'identifiants ou la distribution de charges malveillantes.
3. **Tunneling DNS :** Utilisation du protocole DNS pour encapsuler d'autres flux de données. Cette technique est fréquemment employée par les codes malveillants pour exfiltrer des informations ou communiquer avec un serveur de commande et contrôle (C2) en contournant les restrictions de filtrage des pare-feux.
4. **Élévation de privilèges (DNSAdmins) :** Dans un environnement Windows Server, un compte membre du groupe "DNSAdmins" peut être exploité pour forcer le service DNS à charger une bibliothèque (DLL) arbitraire. Cela conduit à une exécution de code avec les privilèges SYSTEM sur le Contrôleur de Domaine.
## Remédiations & Détection
- **Restriction des transferts :** Configurer les serveurs (y compris le rôle DNS de Windows Server 2019) pour n'autoriser les transferts de zone qu'aux adresses IP des serveurs secondaires explicitement approuvés.
- **Intégrité des enregistrements :** Déployer le protocole DNSSEC (Domain Name System Security Extensions) afin de signer cryptographiquement les résolutions et de prévenir l'empoisonnement de cache.
- **Audit des permissions d'annuaire :** Restreindre strictement l'appartenance au groupe DNSAdmins et appliquer le principe de moindre privilège.
- **Supervision Wazuh :** Intégrer la journalisation Sysmon (Event ID 22 - DNS Query) aux agents Wazuh. Créer des règles de détection pour identifier les requêtes suspectes (par exemple, des enregistrements TXT anormalement longs ou massifs, caractéristiques du tunneling) et alerter sur toute modification non autorisée des clés de registre liées au service DNS.
---
Note : La sécurisation du service DNS est un prérequis absolu pour garantir la fiabilité de l'authentification Kerberos et la disponibilité de l'annuaire au sein du domaine.