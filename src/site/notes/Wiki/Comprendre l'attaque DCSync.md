---
{"dg-publish":true,"permalink":"/wiki/comprendre-l-attaque-dc-sync/","tags":["RedTeam","PostExploitation","ActiveDirectory"]}
---


# Post-Exploitation : L'attaque DCSync

> [!abstract] Le concept de base
> **DCSync** n'est pas une vulnérabilité logicielle (un bug), mais l'exploitation malveillante d'une fonctionnalité légitime de Microsoft. Dans un environnement Active Directory, les Contrôleurs de Domaine (DC) doivent constamment se synchroniser entre eux pour partager les mots de passe et les configurations. L'attaque DCSync consiste à se faire passer pour un Contrôleur de Domaine afin de demander poliment au serveur principal de nous envoyer tous les secrets de l'annuaire.

## 1. Les prérequis de l'attaque
Contrairement aux attaques de type LLMNR Poisoning qui peuvent être exécutées sans aucun droit, DCSync est une technique de **Post-Exploitation**. 

Pour l'exécuter, l'attaquant doit déjà avoir compromis un compte possédant des privilèges très élevés. Plus précisément, le compte doit posséder les droits de réplication d'annuaire (généralement réservés aux groupes *Administrateurs du domaine* ou *Administrateurs de l'entreprise*).

## 2. L'Exploitation et l'exfiltration de la base NTDS.dit
Lors de mon audit d'infrastructure (Pentest), une fois les droits d'Administrateur du Domaine obtenus, j'ai simulé cette attaque pour garantir une persistance totale sur le SI.[1]

- **L'outil utilisé :** Le script `secretsdump.py` de la suite **Impacket**.[1]
- **L'action :** L'outil utilise le protocole *Directory Replication Service (DRS) Remote Protocol* (MS-DRSR).
- **Le résultat :** Le serveur cible est persuadé de parler à un autre serveur légitime et transmet l'intégralité du fichier **NTDS.dit** (le cœur de l'Active Directory).[1]
- **L'impact critique :** L'attaquant récupère le hachage NTLM du compte `krbtgt` (le compte de service de distribution des clés). Avec ce hash, il est possible de forger des *Golden Tickets*, offrant un accès persistant et indétectable au domaine, même si le mot de passe de l'administrateur est changé.

##  Comment détecter et bloquer DCSync? (Blue Team)
Puisqu'il s'agit d'une fonctionnalité légitime et nécessaire au bon fonctionnement d'un réseau multi-serveurs, **on ne peut pas simplement désactiver la réplication**. 

La stratégie de défense repose entièrement sur la **gouvernance des droits** et la **détection** :
1. **Principe de moindre privilège :** Auditer rigoureusement les listes de contrôle d'accès (ACL) pour s'assurer qu'aucun compte de service ou utilisateur standard ne possède les droits `Replicating Directory Changes` et `Replicating Directory Changes All`.
2. **Détection SIEM :** Superviser le réseau pour détecter les requêtes de réplication provenant d'adresses IP qui n'appartiennent pas aux Contrôleurs de Domaine connus. Lors de mes travaux de remédiation, j'ai proposé la création de règles personnalisées sur le SIEM **Wazuh** pour repérer spécifiquement ce comportement anormal.[1]

---
**Voir la mise en pratique de l'exfiltration NTDS.dit dans mon projet de Pentest :**
[[Base/Pentest Active Directory - Occitanie-IT\|Pentest Active Directory - Occitanie-IT]]