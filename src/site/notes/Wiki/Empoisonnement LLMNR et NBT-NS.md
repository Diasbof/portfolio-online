---
{"dg-publish":true,"permalink":"/wiki/empoisonnement-llmnr-et-nbt-ns/","tags":["RedTeam","Réseau","ActiveDirectory"]}
---


# Accès Initial : Empoisonnement LLMNR & NBT-NS

> [!abstract] Le concept de base
> **LLMNR** (Link-Local Multicast Name Resolution) et **NBT-NS** (NetBIOS Name Service) sont des protocoles de résolution de noms obsolètes. Ils servent de solution de secours lorsqu'un serveur DNS classique ne parvient pas à trouver l'adresse IP correspondant à un nom de machine. Lorsqu'un utilisateur cherche un serveur inexistant ou tape mal un nom, son ordinateur va "crier" (requête broadcast) sa demande à tout le réseau local.

## 1. La Vulnérabilité Théorique
Dans un réseau bien configuré, seul le serveur DNS devrait répondre aux requêtes. Cependant, avec LLMNR et NBT-NS (souvent activés par défaut sur les anciens réseaux), n'importe quelle machine du réseau local a le droit de répondre à ce "cri". 

Le problème de sécurité majeur réside dans le fait qu'il n'y a **aucune vérification d'identité**. Un attaquant peut donc lever la main et dire à l'ordinateur de la victime : *"C'est moi le serveur que tu cherches, envoie-moi tes identifiants pour te connecter!"*

## 2. L'Exploitation (Attaque Man-in-the-Middle)
Pour exploiter cette faille, la méthode standard en pentest consiste à utiliser l'outil **Responder**.[1]

- **L'Écoute :** L'attaquant se connecte au réseau local, lance Responder et se met en écoute passive.
- **L'Empoisonnement (Spoofing) :** Dès qu'une victime tente d'accéder à un partage réseau qui n'existe pas (ex: `\\SERVEUR-PARTAGE`), Responder intercepte la requête de secours.
- **La Capture :** Responder se fait passer pour le serveur légitime et force la machine de la victime à s'authentifier. La machine victime envoie alors automatiquement le condensat de son mot de passe (Hash) au format **NTLMv2**.[1]
- **Le Cassage :** L'attaquant récupère ce Hash NTLMv2 et utilise des outils de cassage hors-ligne (comme `Hashcat` ou `John The Ripper`) pour retrouver le mot de passe en clair via des attaques par dictionnaire.[1]

## 🛡️ Comment s'en protéger? (Hardening)
Bien qu'elle soit dévastatrice et permette d'obtenir un premier accès au domaine, c'est l'une des failles les plus simples à corriger.[1] Voici les recommandations prioritaires :

1. **Désactiver LLMNR :** Via les Stratégies de Groupe (GPO), naviguer dans `Configuration ordinateur > Modèles d'administration > Réseau > Client DNS` et activer l'option *"Désactiver la résolution de nom multidiffusion"*.
2. **Désactiver NBT-NS :** Directement dans les paramètres avancés de la carte réseau (TCP/IPv4) ou via la configuration du serveur DHCP.
3. **Forcer la signature SMB (SMB Signing) :** Pour empêcher un attaquant de relayer le hash capturé directement vers une autre machine critique sans même avoir besoin de le casser au préalable.[1]

---
**Voir la mise en pratique de cette attaque dans mon projet d'audit :**
[[Base/Pentest Active Directory - Occitanie-IT\|Pentest Active Directory - Occitanie-IT]]