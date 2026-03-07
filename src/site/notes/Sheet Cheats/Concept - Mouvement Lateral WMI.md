---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-mouvement-lateral-wmi/"}
---

# Mouvement Latéral via WMI
> [!ABSTRACT] Définition
> L'utilisation de WMI (Windows Management Instrumentation) et DCOM (Distributed Component Object Model) pour le mouvement latéral est une technique d'exécution de code à distance furtive. Elle s'appuie sur les composants d'administration natifs de Windows, évitant ainsi le déploiement d'outils malveillants ou la création de nouveaux services détectables par les antivirus.
## Mécanisme Technique

WMI est l'infrastructure de Microsoft destinée à la gestion des données et des opérations sur les systèmes d'exploitation basés sur Windows. Elle permet aux administrateurs (et aux attaquants possédant les identifiants adéquats) d'interroger le système, de modifier des configurations ou de lancer des processus à distance. La communication s'initialise via le port TCP 135 (RPC Endpoint Mapper) puis négocie un port dynamique pour le transfert des instructions DCOM. Le processus lancé sur la machine cible est généralement un enfant du service légitime `WmiPrvSE.exe` (WMI Provider Host).
## Exploitation Opérationnelle
1. **Prérequis :** L'attaquant doit disposer d'un compte membre du groupe des Administrateurs locaux sur la machine cible (obtenu via un Pass-the-Hash, Pass-the-Ticket, ou mot de passe en clair).
2. **Exécution :** Utilisation de scripts d'attaque tels que `wmiexec.py` (Suite Impacket) ou des cmdlets PowerShell natifs (`Invoke-WmiMethod`).
3. **Furtivité :** Contrairement à l'outil classique PsExec qui installe un service temporaire sur la cible (générant des journaux système très audités), WMI exécute les commandes de manière quasi "Fileless", rendant la détection beaucoup plus complexe pour les équipes de sécurité.
## Remédiations & Détection
- **Filtrage Pare-feu (Remédiation) :** Restreindre les flux RPC (port 135 et ports dynamiques élevés) entre les postes de travail (isolation). Seuls les serveurs d'administration et de supervision doivent être autorisés à initier des connexions WMI entrantes.
- **Restriction des accès WMI :** Modifier les descripteurs de sécurité WMI via la console `wmimgmt.msc` pour limiter explicitement les droits d'exécution à distance (Remote Enable) aux seuls groupes administratifs requis.
- **Supervision Wazuh :** Intégrer les journaux Sysmon pour une détection comportementale avancée. Analyser l'Event ID 1 (Process Creation) pour repérer la création de processus suspects (comme `cmd.exe` ou `powershell.exe`) ayant pour processus parent `WmiPrvSE.exe`.
---
Note : Illustration de la technique "Living off the Land" (LotL), nécessitant une supervision granulaire des processus systèmes légitimes.