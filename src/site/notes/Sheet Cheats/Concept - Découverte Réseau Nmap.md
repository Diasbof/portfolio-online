---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-decouverte-reseau-nmap/"}
---

# Découverte Réseau et Balayage de Ports
> [!ABSTRACT] Définition
> La découverte réseau est la première étape active d'une évaluation technique. Elle vise à identifier les équipements en fonctionnement sur un segment réseau ainsi que les services applicatifs exposés (ports ouverts) afin d'établir la surface d'attaque interne de l'infrastructure.
## Mécanisme Technique
Des utilitaires de balayage, tels que Nmap, émettent des paquets réseau spécifiques (requêtes ICMP, segments TCP SYN) vers des plages d'adresses IP définies. L'analyse des réponses TCP/IP permet de déduire l'état des ports (ouverts, fermés, ou filtrés par un dispositif de sécurité), d'inférer le système d'exploitation de la cible (OS Fingerprinting) et de lister les versions précises des services en cours d'exécution.
## Exploitation Opérationnelle
1. **Balayage de détection (Ping Sweep) :** Identification rapide des hôtes actifs au sein du réseau local.
2. **Scan de ports (SYN Scan) :** Découverte furtive des services d'infrastructure critiques (ex: TCP 53 pour le DNS, TCP 88 pour Kerberos, TCP 389 pour LDAP, TCP 445 pour le partage de fichiers).
3. **Analyse de vulnérabilités :** Utilisation de moteurs de scripts automatisés (ex: Nmap Scripting Engine) pour détecter des configurations par défaut ou des vulnérabilités publiques non corrigées (CVE) associées aux versions de services identifiées.
## Remédiations & Détection
- **Segmentation réseau :** Isoler les serveurs d'infrastructure critiques au sein de réseaux virtuels (VLAN) dédiés, protégés par un filtrage strict des flux entrants.
- **Principe de moindre privilège applicatif :** Désactiver rigoureusement l'ensemble des services non essentiels sur les serveurs d'entreprise.
- **Supervision Wazuh :** Intégrer les journaux des dispositifs de pare-feu au SIEM. Configurer des alertes de corrélation pour identifier les balayages de ports (détection de requêtes de connexion multiples ciblant des ports distincts sur une fenêtre de temps réduite en provenance d'une IP source unique).
---
Note : Technique fondamentale permettant de cibler précisément les services vulnérables avant la phase d'exploitation.