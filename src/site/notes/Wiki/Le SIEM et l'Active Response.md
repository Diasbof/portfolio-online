---
{"dg-publish":true,"permalink":"/wiki/le-siem-et-l-active-response/","tags":["BlueTeam","SIEM","Détection"]}
---


# 👁️ Détection et Supervision : SIEM & Active Response

> [!abstract] Concept de base
> Un **SIEM** (Security Information and Event Management) est le véritable "cerveau" de la cyberdéfense d'une entreprise. Il collecte, centralise et corrèle en temps réel les journaux d'événements (logs) de tous les équipements du réseau pour détecter des comportements malveillants. L'**Active Response** est sa capacité à réagir de manière autonome et automatique face à une menace confirmée, avant même l'intervention d'un administrateur humain.[1]

## 1. La collecte de journaux (Les Agents)
Pour qu'un SIEM soit efficace, il ne doit pas être aveugle. Il repose sur le déploiement de petits programmes appelés **Agents** sur les machines cibles (serveurs et postes de travail).[1] 

- **Le rôle de l'agent :** L'agent surveille l'activité locale du système (comme les ouvertures de session Windows ou les modifications de registre) et transmet ces journaux de manière chiffrée au serveur central du SIEM.[1]
- **La nécessité de l'audit :** Par défaut, un système d'exploitation ne journalise pas tout. Il est crucial d'activer des stratégies d'audit (GPO sous Windows) pour forcer la machine à générer des alertes spécifiques, comme l'**Event ID 4625** qui signale un échec de connexion au réseau.[1]

## 2. Le File Integrity Monitoring (FIM)
La surveillance d'intégrité des fichiers (FIM) est un module critique du SIEM utilisé pour protéger les données sensibles contre l'altération (ex: Ransomwares ou vol de données).

- **Comment ça fonctionne?** Le SIEM calcule l'empreinte numérique (Hash MD5/SHA) d'un fichier sain. S'il détecte que cette empreinte change ou que le fichier est supprimé, une alerte de niveau critique est immédiatement levée.[1]
- Lors de mes laboratoires, j'ai configuré ce module sur **Wazuh** pour sécuriser un répertoire hautement confidentiel (`C:\DossierSecret`), me permettant d'être alerté à la seconde près en cas de compromission de l'intégrité des fichiers ciblés.[1]

## 3. L'automatisation de la Défense (Active Response)
La détection seule ne suffit pas face à des attaques automatisées rapides (comme les attaques par Force Brute). Le concept d'Active Response transforme le SIEM en IPS (Système de Prévention des Intrusions).[1]

- **Le déclencheur :** On définit une règle stricte. Par exemple : "Si le SIEM reçoit 5 événements d'échec de connexion (ID 4625) en moins d'une minute depuis la même IP distante".[1]
- **La réaction :** Le serveur SIEM envoie un ordre immédiat à l'agent installé sur la machine ciblée d'exécuter un script d'isolation (ex: `route-null` ou une commande `netsh`), bloquant ainsi l'adresse IP de l'attaquant au niveau du pare-feu local sans aucune interaction humaine.[1]

---
**Voir la mise en pratique du déploiement de Wazuh dans mon projet d'architecture :**
[[Base/Pentest Active Directory - Occitanie-IT\|Pentest Active Directory - Occitanie-IT]]