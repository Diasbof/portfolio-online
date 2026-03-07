---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-vulnerabilite-gpp-cpassword/"}
---

# Vulnérabilité GPP (cpassword)
> [!ABSTRACT] Définition
> La vulnérabilité GPP (Group Policy Preferences) réside dans une ancienne fonctionnalité permettant aux administrateurs de déployer des mots de passe locaux ou des mappages de lecteurs via des stratégies de groupe. Ces mots de passe étaient stockés de manière chiffrée dans l'attribut `cpassword`, mais Microsoft a publié publiquement la clé de chiffrement AES associée.
## Mécanisme Technique

Les stratégies de groupe sont stockées dans le dossier partagé `SYSVOL` des Contrôleurs de Domaine, qui est lisible par défaut par tout utilisateur authentifié du domaine. Si un administrateur a utilisé les GPP pour configurer un mot de passe (par exemple, pour le compte administrateur local), un fichier XML est généré dans ce répertoire. N'importe quel utilisateur peut lire ce fichier, extraire la valeur `cpassword` et la déchiffrer instantanément à l'aide de la clé connue.
## Méthodologie d'Attaque
1. **Énumération SYSVOL :** L'attaquant recherche des fichiers spécifiques (comme `Groups.xml`, `Services.xml`, `Printers.xml` ou `Drives.xml`) dans le partage `\\Domaine\SYSVOL`.
2. **Extraction :** Localisation de la chaîne chiffrée dans l'attribut `cpassword`.
3. **Déchiffrement :** Utilisation d'outils standards comme `gpp-decrypt` sous Linux ou de scripts automatisés pour obtenir le mot de passe en clair.
## Remédiations & Détection
- **Mise à jour :** Appliquer le correctif de sécurité MS14-025, qui supprime la possibilité de stocker de nouveaux mots de passe dans les GPP.
- **Nettoyage manuel :** Le correctif n'efface pas les anciens fichiers. Il est impératif d'auditer le dossier `SYSVOL` et de supprimer manuellement les fichiers XML vulnérables existants.
- **Alternative sécurisée :** Déployer [[Sheet Cheats/Concept - Microsoft LAPS\|Microsoft LAPS]] pour la gestion des mots de passe administrateurs locaux.
- **Surveillance :** Analyser les accès anormaux ou massifs au partage SYSVOL par des comptes utilisateurs standards.
---
👉 *Vulnérabilité historique démontrant l'importance de l'audit continu des partages systèmes.*