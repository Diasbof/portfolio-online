---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-pass-the-ticket/"}
---

# L'Attaque Pass-the-Ticket (PtT)
> [!ABSTRACT] Définition
> L'attaque Pass-the-Ticket (PtT) consiste à extraire un ticket Kerberos (TGT ou TGS) de la mémoire d'un système compromis, puis à l'injecter dans la session de l'attaquant. Cela permet de s'authentifier sur des ressources distantes en usurpant l'identité du propriétaire du ticket, sans avoir besoin de son mot de passe ni de son hash NTLM.
## Mécanisme Technique

Contrairement au protocole NTLM, Kerberos repose sur un système de tickets géré par l'autorité de sécurité locale (LSASS). Lorsqu'un utilisateur s'authentifie ou accède à un service, les tickets correspondants sont mis en cache en mémoire pour éviter de solliciter continuellement le Contrôleur de Domaine. Si un attaquant dispose des privilèges d'administration locaux sur une machine où un utilisateur à hauts privilèges (ex: Administrateur du Domaine) s'est récemment connecté, il peut exporter ces tickets (généralement au format `.kirbi`) avant leur expiration.
## Exploitation Opérationnelle
1. **Extraction :** Utilisation d'outils comme Mimikatz (`sekurlsa::tickets /export`) ou Rubeus (`dump`) pour vider le cache Kerberos de la machine compromise.
2. **Injection :** Importation du ticket ciblé dans la session en cours de l'attaquant (via `kerberos::ptt` sous Mimikatz ou `ptt` sous Rubeus).
3. **Mouvement :** Accès immédiat aux ressources réseau (partages administratifs, exécution de commandes à distance) autorisées par le ticket, de manière totalement transparente pour les systèmes cibles.
## Remédiations & Détection
- **Windows Defender Credential Guard :** L'activation de cette fonctionnalité de virtualisation (VBS) isole le processus LSASS dans un conteneur sécurisé, empêchant l'extraction des tickets Kerberos en clair, même par un administrateur local.
- **Principe du Moindre Privilège (Tiering) :** Restreindre strictement les connexions des comptes à hauts privilèges sur les postes de travail standards (Tier 2). Si un administrateur ne se connecte pas sur une machine compromise, aucun ticket n'y sera laissé en cache.
- **Supervision Wazuh :** Détecter les accès suspects à la mémoire LSASS (Event ID 4656/4663). Sur les Contrôleurs de Domaine, surveiller les anomalies Kerberos (Event ID 4769 - Demande de TGS) où l'adresse IP source ne correspond pas à l'adresse IP habituelle de l'utilisateur associé au ticket. L'Event ID 4624 (Logon) avec le "Logon Type 9" (NewCredentials) est également un indicateur fort d'injection de jeton ou de ticket.
---
Note : Technique de choix pour la compromission d'infrastructures modernes où le protocole NTLM a été désactivé au profit exclusif de Kerberos.