---
{"dg-publish":true,"permalink":"/wiki/durcissement-avec-microsoft-laps/","tags":["BlueTeam","Hardening","ActiveDirectory"]}
---


# 🧱 Durcissement : Déploiement de Microsoft LAPS

> [!info] Concept de base **LAPS** (Local Administrator Password Solution) est un outil gratuit fourni par Microsoft. Son rôle est de générer automatiquement un mot de passe aléatoire, complexe et unique pour le compte Administrateur local de chaque poste de travail ou serveur membre du domaine. Ce mot de passe est ensuite stocké de manière sécurisée directement dans un attribut caché de l'Active Directory.

## 1. La Vulnérabilité : Le Mouvement Latéral

Dans de nombreuses entreprises (comme c'était le cas initialement sur l'infrastructure d'Occitanie-IT), lors du déploiement des ordinateurs, le compte Administrateur local possède exactement le **même mot de passe** sur toutes les machines du parc.

- **Le risque critique :** Si un attaquant parvient à compromettre un seul ordinateur standard (via un phishing par exemple) et à voler l'empreinte (Hash) de ce compte administrateur local, il peut utiliser la technique du **Pass-The-Hash**.
    
- **L'impact :** Puisque le mot de passe est identique partout, l'attaquant peut rebondir de machine en machine (mouvement latéral) jusqu'à atteindre un serveur critique, sans jamais avoir besoin de connaître le mot de passe en clair.
    

## 2. La Remédiation avec LAPS

Lors de ma phase de durcissement (Hardening), j'ai déployé LAPS pour neutraliser définitivement ce vecteur d'attaque.

### Étape A : Mise à jour du Schéma Active Directory

LAPS a besoin de nouveaux attributs pour stocker les mots de passe et leurs dates d'expiration. J'ai donc étendu le schéma AD via la commande PowerShell suivante :

`Update-AdmPwdADSchema`

**Résultat de l'opération :**

| **Operation**      | **DistinguishedName**                                           | **Status** |
| ------------------ | --------------------------------------------------------------- | ---------- |
| AddSchemaAttribute | cn=ms-Mcs-AdmPwdExpirationTime,CN=Schema,CN=Configuration...    | Success    |
| AddSchemaAttribute | cn=ms-Mcs-AdmPwd,CN=Schema,CN=Configuration,DC=occitanie,DC=lan | Success    |
| ModifySchemaClass  | cn=computer,CN=Schema,CN=Configuration,DC=occitanie,DC=lan      | Success    |
### Étape B : Déploiement par Stratégie de Groupe (GPO)

Une fois le schéma mis à jour et l'extension client déployée, j'ai configuré une GPO dédiée pour forcer la gestion des mots de passe :

- **Activation :** Paramètre `Enable local admin password management` activé.
    
- **Sécurité :** Configuration de mots de passe longs (14 caractères minimum) avec une rotation automatique définie.

## 💡 L'avantage opérationnel

Désormais, si un technicien du support informatique doit intervenir sur une machine spécifique, il interroge l'Active Directory pour récupérer le mot de passe unique de _cette_ machine précise. Si la machine venait à être compromise, l'attaquant resterait bloqué et ne pourrait pas propager son attaque aux autres postes de l'entreprise.

---

**Voir la mise en pratique globale du durcissement dans mon projet :** [[Base/Pentest Active Directory - Occitanie-IT\|Pentest Active Directory - Occitanie-IT]]