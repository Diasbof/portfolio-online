---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-enumeration-ldap-et-rpc/"}
---

# Énumération LDAP et RPC
> [!ABSTRACT] Définition
> L'énumération cible les protocoles natifs de l'infrastructure, notamment LDAP (Lightweight Directory Access Protocol) et RPC (Remote Procedure Call), dans le but d'extraire une cartographie exhaustive des objets de l'annuaire (utilisateurs, groupes de sécurité, ordinateurs, politiques de domaine) préalablement à toute tentative de compromission.
## Mécanisme Technique
Le rôle central d'un annuaire d'entreprise est de fournir des informations aux clients du domaine pour garantir le fonctionnement du système d'information. Les auditeurs et attaquants détournent ce comportement légitime. Si l'accès anonyme (Null Bind) est toléré, ou suite à la compromission d'un compte utilisateur standard, il devient possible d'interroger la base de données de manière massive et automatisée.
## Exploitation Opérationnelle
1. **Extraction de la politique de sécurité :** Identification de la longueur minimale des mots de passe, de la complexité exigée et du seuil de verrouillage des comptes (Account Lockout Threshold). Ces données sont indispensables pour calibrer silencieusement les attaques par force brute.
2. **Cartographie des accès :** Extraction de la liste intégrale des comptes et identification des membres appartenant aux groupes à hauts privilèges (ex: Administrateurs du domaine), définissant ainsi les cibles d'élévation de privilèges.
3. **Identification des comptes de service :** Recherche des attributs SPN (Service Principal Names) associés aux comptes utilisateurs, étape préparatoire indispensable pour l'exécution d'une attaque de type Kerberoasting.
## Remédiations & Détection
- **Désactivation des accès anonymes :** Garantir que les requêtes LDAP anonymes sont strictement prohibées au niveau des Contrôleurs de Domaine.
- **Restriction des requêtes RPC :** Déployer des stratégies de groupe (GPO) pour restreindre l'énumération des comptes locaux (SAM) exclusivement aux administrateurs approuvés.
- **Supervision Wazuh :** Activer l'audit avancé de l'accès aux services d'annuaire. Configurer les règles de détection pour générer une alerte de sécurité lors de requêtes LDAP volumineuses ou atypiques initiées depuis des postes de travail standards (indicateur de compromission caractéristique des outils d'énumération automatisés).
---
Note : L'audit des permissions de lecture sur l'annuaire est essentiel pour entraver la progression latérale d'un attaquant disposant d'un accès initial.