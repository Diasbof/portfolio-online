---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-elevation-se-impersonate/"}
---

# Élévation Locale (SeImpersonatePrivilege)
> [!ABSTRACT] Définition
> L'élévation de privilèges via `SeImpersonatePrivilege` consiste à exploiter un droit légitime accordé à certains comptes de service sous Windows. Cette technique permet à un attaquant, ayant initialement compromis un service web ou une base de données, de forcer le système à lui fournir un jeton d'accès (Token) de l'autorité suprême, devenant ainsi "NT AUTHORITY\SYSTEM".
## Mécanisme Technique

Le privilège "Emprunter l'identité d'un client après l'authentification" (`SeImpersonatePrivilege`) est requis par des services comme IIS (Internet Information Services) ou SQL Server pour agir au nom de l'utilisateur qui s'y connecte. L'attaque (souvent appelée "Potato Attack" ou "PrintSpoofer") consiste à forcer un processus privilégié (comme le service Spooler d'impression ou un service RPC) à s'authentifier auprès d'un canal nommé (Named Pipe) contrôlé par l'attaquant. Ce dernier intercepte le jeton d'authentification SYSTEM et l'utilise pour créer un nouveau processus avec ces privilèges maximaux.
## Exploitation Opérationnelle
1. **Accès Initial :** Obtention d'une exécution de code (ex: via un Web Shell) avec un compte de service restreint tel que `IIS APPPOOL\DefaultAppPool` ou `NETWORK SERVICE`.
2. **Vérification :** Utilisation de la commande `whoami /priv` pour confirmer la présence du droit `SeImpersonatePrivilege`.
3. **Exploitation :** Exécution d'un outil tel que PrintSpoofer, RoguePotato ou GodPotato. L'outil manipule les appels système pour intercepter le jeton SYSTEM et lancer une invite de commande ou une balise de commande et contrôle (C2) avec les droits d'administration locaux totaux.
## Remédiations & Détection
- **Isolation des services :** Exécuter les services applicatifs sous des comptes d'utilisateurs virtuels (Virtual Accounts) ou des comptes gMSA ne disposant pas du privilège `SeImpersonatePrivilege` si l'application ne le justifie pas strictement.
- **Désactivation des vecteurs de contrainte :** Sur les serveurs n'ayant pas vocation à imprimer (ex: serveurs web, serveurs de bases de données), désactiver systématiquement le service "Spouleur d'impression" (Print Spooler) qui est le vecteur principal de l'outil PrintSpoofer.
- **Supervision Wazuh :** Intégrer les journaux Sysmon. Détecter l'Event ID 1 (Création de processus) lorsque le processus parent est un compte de service (ex: `w3wp.exe` ou `sqlservr.exe`) et que le processus enfant est un interpréteur de commandes (`cmd.exe` ou `powershell.exe`) s'exécutant sous l'identité SYSTEM.
---
Note : Étape incontournable lors de la compromission d'un serveur applicatif pour obtenir le contrôle total du système d'exploitation sous-jacent.