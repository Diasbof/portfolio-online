---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-attaques-ssh/"}
---

# Attaques SSH (Port 22)
> [!ABSTRACT] Définition
> Le protocole SSH (Secure Shell), opérant par défaut sur le port TCP 22, permet d'établir une communication chiffrée de bout en bout entre un client et un serveur. Bien qu'il soit conçu pour sécuriser l'administration à distance, de mauvaises configurations ou une gestion inadéquate des identifiants en font une cible privilégiée lors des audits de sécurité.
## Vecteurs d'Attaque Principaux

1. **Force Brute et Pulvérisation de mots de passe :** L'attaque la plus courante consiste à tester des listes de mots de passe par défaut ou faibles sur des comptes standards ou privilégiés (ex: root, admin) à l'aide d'outils automatisés.
2. **Vol de clés privées :** Lorsqu'un attaquant compromet le poste de travail d'un administrateur, il recherche systématiquement les clés privées asymétriques (fichiers `id_rsa`). Si ces clés ne sont pas protégées par une phrase secrète (passphrase), elles permettent un accès direct aux serveurs cibles.
3. **Détournement d'Agent SSH (Agent Forwarding Hijacking) :** Si un administrateur utilise la fonctionnalité de transfert d'agent vers un serveur rebond préalablement compromis, l'attaquant possédant les privilèges locaux peut usurper la socket de l'agent pour rebondir vers d'autres machines du système d'information.
4. **Faiblesses cryptographiques :** Le maintien de la rétrocompatibilité avec SSHv1 ou l'autorisation d'algorithmes d'échange de clés obsolètes expose les sessions à des attaques d'interception ou de dégradation (downgrade attacks).
## Remédiations & Détection
- **Durcissement de la configuration :** Modifier le fichier `sshd_config` pour interdire l'authentification par mot de passe (`PasswordAuthentication no`) au profit exclusif de l'authentification par clés publiques. L'accès direct au compte super-utilisateur doit être prohibé (`PermitRootLogin no`).
- **Gestion des secrets :** Imposer le chiffrement des clés privées clientes via une phrase secrète robuste.
- **Supervision Wazuh :** Déployer le module de réponse active (Active Response) de Wazuh pour analyser les journaux d'authentification (`/var/log/auth.log` ou `/var/log/secure`) et bannir dynamiquement au niveau du pare-feu les adresses IP à l'origine de tentatives de connexion échouées multiples.
---
Note : L'audit de l'infrastructure SSH est une étape systématique lors de l'évaluation de la sécurité des accès distants d'un système d'information.