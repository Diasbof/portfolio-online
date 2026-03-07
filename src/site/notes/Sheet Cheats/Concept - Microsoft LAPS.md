---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-microsoft-laps/"}
---

# Microsoft LAPS (Local Administrator Password Solution)
> [!ABSTRACT] Définition
> Microsoft LAPS est une solution de durcissement (hardening) qui gère, centralise et randomise les mots de passe des comptes administrateurs locaux des postes de travail et serveurs membres d'un domaine Active Directory.
## Mécanisme Technique

En l'absence de LAPS, les entreprises déploient souvent un mot de passe administrateur local identique sur l'ensemble du parc informatique. LAPS déploie une extension côté client (CSE) via les stratégies de groupe (GPO). Chaque machine génère localement un mot de passe complexe et aléatoire, puis le transmet de manière chiffrée au Contrôleur de Domaine. Ce secret est stocké dans l'attribut confidentiel `ms-Mcs-AdmPwd` de l'objet ordinateur correspondant au sein de l'annuaire.
## Exploitation Opérationnelle (Déploiement)
1. **Extension du Schéma :** Mise à jour du schéma Active Directory (via la commande PowerShell `Update-AdmPwdADSchema`) afin d'intégrer les attributs nécessaires au stockage des mots de passe et de leur date d'expiration (`ms-Mcs-AdmPwdExpirationTime`).
2. **Gestion des Permissions :** Révocation des droits de lecture étendus par défaut et délégation stricte de la consultation de ces attributs à un groupe de sécurité défini (ex: Administrateurs locaux ou équipe support).
3. **Application via GPO :** Déploiement d'une stratégie de groupe définissant la longueur requise du mot de passe, sa complexité et la fréquence de rotation automatique (par exemple, 30 jours).
## Remédiations & Détection
- **Prévention des Mouvements Latéraux (Remédiation) :** L'implémentation de LAPS neutralise l'impact des attaques de type [[Sheet Cheats/Concept - Pass-the-Hash\|Pass-the-Hash]] sur les comptes locaux. La compromission d'une station de travail ne fournit plus d'identifiants réutilisables sur le reste du réseau.
- **Audit des Accès :** Activer l'audit de l'accès aux objets d'annuaire sur l'infrastructure Windows Server 2019.
- **Supervision Wazuh (Détection) :** Configurer les agents pour cibler l'Event ID 4662 (Opération sur un objet d'annuaire) associé au GUID de l'attribut de mot de passe LAPS. La lecture de cet attribut par des comptes utilisateurs non listés dans le groupe de délégation autorisé, ou en dehors des horaires de maintenance, doit déclencher une alerte de sécurité critique.
---
Note : Implémentation prioritaire lors de la phase de remédiation d'un audit de sécurité interne.