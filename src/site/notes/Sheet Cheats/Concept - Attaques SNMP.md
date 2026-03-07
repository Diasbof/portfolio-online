---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-attaques-snmp/"}
---

# Attaques SNMP (Supervision réseau)
> [!ABSTRACT] Définition
> Le protocole SNMP (Simple Network Management Protocol), utilisant principalement les ports UDP 161 (requêtes) et 162 (traps), est le standard de supervision des équipements réseau et serveurs. Historiquement conçu sans sécurité robuste, il représente une source majeure de fuites de données techniques lors de la phase de reconnaissance d'un audit.
## Vecteurs d'Attaque Principaux

1. **Énumération exhaustive (Information Disclosure) :** L'interrogation de l'arborescence MIB (Management Information Base) d'un équipement permet à un attaquant d'extraire sa table de routage, son cache ARP, la liste des processus en cours d'exécution, les logiciels installés, et parfois les comptes utilisateurs du domaine. Des outils comme `snmpwalk` automatisent ce processus.
2. **Force Brute des chaînes de communauté :** Les versions SNMPv1 et SNMPv2c s'appuient sur un mot de passe, appelé "chaîne de communauté" (Community String), transmis en clair sur le réseau. Les attaquants ciblent systématiquement les valeurs par défaut de l'industrie (telles que "public" pour la lecture et "private" pour l'écriture).
3. **Modification de configuration (Accès RW) :** Si un attaquant parvient à compromettre la chaîne de communauté disposant de droits en lecture et écriture (Read-Write), il acquiert la capacité de modifier la configuration de l'équipement, d'altérer les règles de routage ou de provoquer un redémarrage (Déni de Service).
4. **Interception (Sniffing) :** Le trafic des versions 1 et 2c n'étant pas chiffré, une capture de trames sur le réseau local permet de récupérer les chaînes de communauté légitimes utilisées par les serveurs de supervision.
## Remédiations & Détection
- **Migration vers SNMPv3 :** Imposer l'utilisation exclusive de la version 3 du protocole, configurée en mode "authPriv". Ce mode garantit l'authentification forte des requêtes et le chiffrement intégral des flux de supervision.
- **Filtrage réseau strict :** Restreindre l'accès au port UDP 161 via les pare-feux locaux et les listes de contrôle d'accès (ACL) matérielles. Seules les adresses IP des serveurs de supervision légitimes doivent être autorisées à interroger les agents SNMP.
- **Révocation des droits d'écriture :** Si l'utilisation de SNMPv2c est maintenue pour des raisons de compatibilité, remplacer les valeurs par défaut par des chaînes complexes et désactiver systématiquement l'accès en écriture (RW).
- **Supervision Wazuh :** Configurer les agents pour monitorer les balayages réseau ciblant le port UDP 161 depuis des adresses IP non identifiées comme serveurs de supervision. Sur Windows Server 2019, l'installation ou le démarrage non documenté du service SNMP doit déclencher une alerte d'investigation.
---
Note : La sécurisation de l'infrastructure de supervision est critique pour empêcher la cartographie passive du système d'information par un attaquant.