---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-attaques-smb-et-net-bios/"}
---

# Attaques SMB et NetBIOS (Ports 139/445)
> [!ABSTRACT] Définition
> Server Message Block (SMB) et NetBIOS sont des protocoles fondamentaux dans les environnements Microsoft, gérant le partage de fichiers, d'imprimantes et la communication inter-processus (IPC). Exposés sur les ports TCP 139 et 445, ils constituent la cible prioritaire pour l'énumération de l'annuaire et la propagation de codes malveillants.
## Vecteurs d'Attaque Principaux
[Image of SMB protocol architecture and NetBIOS over TCP/IP]
1. **Énumération via Sessions Nulles (Null Sessions) :** Sur les anciennes configurations ou par défaut sur certains partages, un attaquant peut s'authentifier sans identifiants (connexion anonyme) pour extraire la liste des utilisateurs, des groupes, des partages réseau et des politiques de mots de passe de l'Active Directory.
2. **Exploitation de Vulnérabilités Critiques :** Les failles de type exécution de code à distance (RCE), comme MS17-010 (EternalBlue) ciblant la version 1 du protocole SMB, permettent la compromission totale du système sans aucune authentification préalable.
3. **Mouvement Latéral (PsExec / SMBExec) :** Avec des identifiants valides ou un condensé NTLM (Pass-the-Hash), un attaquant utilise les partages administratifs cachés (`C$`, `ADMIN$`) pour transférer et exécuter des binaires ou des scripts sur des machines distantes, facilitant la propagation au sein du domaine.
## Remédiations & Détection
- **Désactivation de SMBv1 :** Éradiquer définitivement le protocole SMBv1 sur l'ensemble du parc informatique (serveurs et postes de travail) au profit de SMBv2 ou SMBv3.
- **Signature SMB (SMB Signing) :** Rendre la signature des paquets obligatoire via les stratégies de groupe (GPO) pour prévenir les attaques de type [[Sheet Cheats/Concept - SMB Relay\|SMB Relay]].
- **Restriction de NetBIOS :** Désactiver NetBIOS sur TCP/IP au niveau des cartes réseau des serveurs Windows Server 2019, l'infrastructure moderne s'appuyant exclusivement sur le DNS (Port 445 direct).
- **Supervision Wazuh :** Analyser les événements de sécurité (Event ID 4624 - Logon Type 3) pour détecter des connexions réseau inhabituelles. Corréler ces événements avec les journaux Sysmon (Event ID 1 - Création de processus) ou System (Event ID 7045 - Installation de service) pour identifier l'apparition de services associés aux outils de mouvement latéral comme `PSEXESVC.exe`.
---
Note : La sécurisation des flux SMB est le point névralgique de la lutte contre la prolifération des attaques de type rançongiciel.