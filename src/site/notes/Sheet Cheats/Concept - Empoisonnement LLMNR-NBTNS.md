---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-empoisonnement-llmnr-nbtns/"}
---

# Empoisonnement LLMNR / NBT-NS
> [!ABSTRACT] Définition
> LLMNR (Link-Local Multicast Name Resolution) et NBT-NS (NetBIOS Name Service) sont des protocoles de secours utilisés par Windows pour la résolution de noms lorsque le DNS échoue. L'empoisonnement consiste à répondre frauduleusement à ces requêtes pour intercepter des flux d'authentification.
## Mécanisme Technique
Lorsqu'un utilisateur tape une adresse erronée (ex: `\\serveur_compta` au lieu de `\\serveur-compta`), la machine émet une requête multidiffusion (multicast) sur le réseau local. L'attaquant, via un outil dédié, répond immédiatement en affirmant être la ressource recherchée. La victime tente alors de s'authentifier, envoyant ainsi son hash NTLMv2 à l'attaquant.
## Vecteurs d'Attaque
1. **Interception de Hashs :** Capture des condensés (hashs) NTLMv2 pour une tentative de cassage hors-ligne (Brute Force / Dictionnaire).
2. **Relais d'authentification :** Si la signature SMB est désactivée, le flux intercepté peut être redirigé en temps réel vers une autre cible pour en prendre le contrôle (voir [[Sheet Cheats/Concept - SMB Relay\|Concept - SMB Relay]]).
3. **Outil de référence :** **Responder** est l'outil standard pour automatiser cette écoute et ces réponses frauduleuses.
## Remédiations & Détection
- **Désactivation des protocoles :** La mesure la plus efficace consiste à désactiver LLMNR et NetBIOS via GPO (Group Policy Object) sur l'ensemble du parc.
- **Sécurité réseau :** Mettre en place des mécanismes comme le DHCP Snooping ou l'inspection ARP pour limiter les attaques de type Man-in-the-Middle.
- **Surveillance SIEM (Wazuh) :** Analyser les Event ID **4624** (Logon) associés à des méthodes d'authentification NTLM inhabituelles ou des pics de trafic sur les ports UDP 5355 (LLMNR) et 137 (NetBIOS).
---
👉 *Technique critique illustrant les dangers des protocoles de résolution de noms hérités.*