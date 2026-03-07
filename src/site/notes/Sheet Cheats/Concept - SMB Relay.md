---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-smb-relay/"}
---

# L'Attaque SMB Relay
> [!ABSTRACT] Définition
> L'attaque SMB Relay consiste à intercepter une tentative d'authentification réseau (généralement NTLM) et à la transférer (relayer) en temps réel vers une autre machine cible. Cela permet à l'attaquant de s'authentifier sur le système cible avec les privilèges de l'utilisateur intercepté, sans avoir besoin de compromettre le mot de passe.
## Mécanisme Technique

Cette attaque fait souvent suite à un empoisonnement de la résolution de noms (voir [[Sheet Cheats/Concept - Empoisonnement LLMNR-NBTNS\|Empoisonnement LLMNR / NBT-NS]]). Lorsqu'un client tente de se connecter légitimement à une ressource, il envoie une requête d'authentification. L'attaquant se place en position d'intercepteur (Man-in-the-Middle) et transfère cette requête vers le port 445 (SMB) d'un serveur cible. Si l'authentification aboutit et que la cible n'exige pas de signature cryptographique, l'attaquant obtient une session valide.
## Exploitation Opérationnelle
1. **Préparation :** Identification des cibles sur le réseau dont le paramètre de signature SMB est désactivé ou défini sur "Optionnel".
2. **Relais :** Utilisation de scripts spécialisés (ex: `ntlmrelayx.py` de la suite Impacket) pour écouter les requêtes entrantes et les rediriger vers les cibles vulnérables.
3. **Exécution :** Dès la réussite du relais, l'outil peut extraire la base SAM locale de la cible, exécuter des commandes système ou établir un shell interactif avec les droits de la session relayée.
## Remédiations & Détection
- **Signature SMB :** La protection principale consiste à forcer la signature SMB (SMB Signing) via les stratégies de groupe (GPO) sur les serveurs Windows Server 2019 et l'ensemble du parc. Cette configuration invalide les sessions relayées.
- **Désactivation des protocoles obsolètes :** Couper LLMNR et NBT-NS pour limiter les capacités d'interception initiales sur le segment réseau.
- **Filtrage réseau :** Restreindre les communications de poste à poste (isolation) pour empêcher la propagation latérale via le port 445.
- **Supervision Wazuh :** Analyser les événements de connexion réseau (Event ID 4624, Logon Type 3). Une anomalie caractéristique d'un relais est l'observation d'une adresse IP source (l'attaquant) ne correspondant pas au nom de la machine habituelle de l'utilisateur authentifié.
---
👉 *Technique démontrant la criticité de l'authentification mutuelle et de la signature des flux dans un domaine Active Directory.*