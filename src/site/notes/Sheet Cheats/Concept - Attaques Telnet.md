---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-attaques-telnet/"}
---

# Attaques Telnet (Port 23)
> [!ABSTRACT] Définition
> Telnet (Teletype Network), opérant par défaut sur le port TCP 23, est un protocole client-serveur historique d'émulation de terminal. Conçu avant l'émergence des cybermenaces modernes, il transmet l'intégralité des communications en clair, ce qui en fait l'un des protocoles les plus critiques à éradiquer d'une infrastructure réseau contemporaine.
## Vecteurs d'Attaque Principaux

1. **Interception réseau (Sniffing) :** L'absence totale de chiffrement permet à tout attaquant situé sur le même segment réseau de capturer les identifiants de connexion et les commandes administratives saisies via une simple analyse de trames.
2. **Man-in-the-Middle (MitM) :** Telnet ne proposant aucun mécanisme d'authentification du serveur, un attaquant peut aisément usurper l'identité de l'équipement cible via des techniques d'empoisonnement réseau (ex: ARP Spoofing) et détourner la session d'administration.
3. **Force Brute et Identifiants par défaut :** Les équipements réseau hérités ou les anciens serveurs exposant encore Telnet sont fréquemment configurés avec des mots de passe d'usine, rendant les attaques par dictionnaire et l'automatisation des accès extrêmement efficaces pour les attaquants.
## Remédiations & Détection
- **Éradication du protocole :** La mesure corrective absolue consiste à désactiver définitivement le service Telnet sur l'ensemble des équipements du système d'information (serveurs Windows/Linux, commutateurs, routeurs, objets connectés).
- **Déploiement d'alternatives sécurisées :** Remplacer les accès Telnet par des protocoles chiffrés tels que SSH (Port 22) ou, dans le cadre d'une infrastructure Microsoft, par WinRM / PowerShell Remoting sur HTTPS.
- **Filtrage et Supervision Wazuh :** Bloquer les flux à destination du port TCP 23 au niveau des pare-feu internes. Configurer les agents Wazuh pour générer une alerte de sévérité haute dès qu'un trafic réseau impliquant le port 23 est détecté ou si le service Telnet est démarré sur un hôte supervisé.
---
Note : La découverte du protocole Telnet lors d'un audit interne constitue une vulnérabilité critique nécessitant une remédiation immédiate de la part des équipes d'infrastructure.