---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-blood-hound-and-analyse-de-graphes/"}
---

# BloodHound & Analyse des Chemins d'Attaque
> [!ABSTRACT] Définition
> BloodHound est un outil d'analyse de données qui utilise la théorie des graphes pour cartographier les relations au sein d'un Active Directory. Il permet d'identifier visuellement les chemins d'attaque les plus courts pour devenir "Domain Admin".
## Pourquoi c'est indispensable ?
Dans un réseau complexe, les relations de confiance sont invisibles à l'œil nu. BloodHound révèle des liens critiques : par exemple, un utilisateur standard qui a le droit de réinitialiser le mot de passe d'un groupe, lequel a des droits d'admin sur un serveur où une session "Domain Admin" est ouverte.
## Fonctionnement Technique
1. **Collecte (SharpHound) :** Un collecteur (ingestor) est exécuté sur le domaine pour extraire les utilisateurs, groupes, ordinateurs, sessions et surtout les ACL (Access Control Lists).
2. **Base de Données (Neo4j) :** Les données sont importées dans Neo4j, une base de données orientée graphe qui calcule les relations entre les objets.
3. **Interface (BloodHound GUI) :** L'attaquant ou l'auditeur utilise des requêtes pré-enregistrées comme "Shortest Paths to Domain Admins" pour visualiser les cibles.
## Remédiations & Détection
- **Nettoyage des ACL :** Supprimer les permissions dangereuses (GenericAll, WriteDacl) accordées à des utilisateurs non privilégiés.
- **Principe du Moindre Privilège :** Limiter le nombre d'administrateurs et isoler les sessions via le Tiering Model de Microsoft.
- **Détection (Wazuh/SIEM) :** Surveiller les requêtes LDAP massives provenant d'une seule machine en peu de temps, caractéristiques d'une énumération SharpHound.
---
👉 *Outil majeur pour comprendre l'escalade de privilèges par analyse de relations.*