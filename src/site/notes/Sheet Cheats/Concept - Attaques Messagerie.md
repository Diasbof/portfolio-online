---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-attaques-messagerie/"}
---

# Attaques de Messagerie (SMTP / POP / IMAP)
> [!ABSTRACT] Définition
> Les protocoles de messagerie gèrent l'acheminement (SMTP) et la consultation (POP3, IMAP) des courriers électroniques. Fréquemment exposés sur Internet, ils constituent une cible privilégiée pour l'ingénierie sociale (hameçonnage), l'usurpation d'identité et la compromission des identifiants de l'annuaire d'entreprise.
## Vecteurs d'Attaque Principaux

1. **Usurpation d'identité (Spoofing) :** Le protocole SMTP originel ne vérifiant pas l'authenticité de l'expéditeur, un attaquant peut forger l'en-tête d'un courrier pour usurper l'identité d'un dirigeant ou d'un administrateur. Cette technique est au cœur des campagnes de hameçonnage ciblé (Spear Phishing).
2. **Relais Ouvert (Open Relay) :** Une mauvaise configuration du serveur SMTP permet à tout utilisateur non authentifié de l'employer pour acheminer des messages. Les attaquants exploitent cette vulnérabilité pour diffuser massivement des courriers indésirables ou malveillants, provoquant le blacklistage de l'adresse IP publique de l'infrastructure.
3. **Interception (Sniffing) et Force Brute :** L'utilisation des ports standards en clair (SMTP 25, POP3 110, IMAP 143) permet l'interception des identifiants réseau. De plus, ces services d'accès distant sont continuellement ciblés par des attaques de pulvérisation de mots de passe (Password Spraying) visant à compromettre des comptes utilisateurs valides.
## Remédiations & Détection
- **Durcissement cryptographique :** Imposer l'utilisation exclusive des protocoles chiffrés (SMTPS 465, IMAPS 993, POP3S 995) ou exiger l'initialisation d'un canal sécurisé via la commande STARTTLS.
- **Désactivation de l'authentification héritée :** L'authentification basique (Basic Authentication) doit être désactivée sur les services de messagerie au profit d'une authentification moderne imposant un mécanisme multi-facteur (MFA).
- **Standards de sécurité DNS :** Déployer rigoureusement les enregistrements SPF (Sender Policy Framework), DKIM (DomainKeys Identified Mail) et DMARC (Domain-based Message Authentication, Reporting, and Conformance) afin d'assurer l'intégrité et l'authenticité des domaines expéditeurs.
- **Supervision Wazuh :** Intégrer la journalisation des serveurs de messagerie aux agents. Configurer des règles d'alerte pour identifier les tentatives de connexion échouées massives ciblant les services IMAP/SMTP, ainsi que l'envoi de volumes de courriels anormaux depuis un compte interne compromis.
---
Note : L'audit de la configuration de la messagerie est une étape critique pour prévenir les vecteurs d'accès initiaux.