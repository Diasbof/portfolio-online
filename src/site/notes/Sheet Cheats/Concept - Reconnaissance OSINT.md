---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-reconnaissance-osint/"}
---

# Reconnaissance OSINT (Open Source Intelligence)
> [!ABSTRACT] Définition
> L'OSINT regroupe les techniques de collecte et d'analyse d'informations publiquement accessibles concernant une organisation cible. Cette phase passive s'effectue sans interaction directe avec l'infrastructure de la victime, la rendant de fait indétectable par les systèmes défensifs du système d'information.
## Mécanisme Technique
L'objectif est d'alimenter les phases offensives ultérieures (ingénierie sociale, pulvérisation de mots de passe) en accumulant des données stratégiques. L'analyse s'appuie sur les moteurs de recherche (Google Dorks), les réseaux sociaux professionnels, les bases de données d'enregistrements DNS (Whois) et les dépôts de code publics.
## Exploitation Opérationnelle
1. **Génération de dictionnaires d'utilisateurs :** Extraction des noms et prénoms des collaborateurs pour modéliser le format des adresses courriel de l'entreprise (ex: prenom.nom@domaine.com). Cette liste est indispensable pour tester les accès externes.
2. **Fuites de données (Data Breaches) :** Recherche des adresses de l'organisation dans les bases de données d'identifiants compromis (Credential Leaks). La réutilisation des mots de passe par les utilisateurs constitue un vecteur d'accès initial majeur.
3. **Cartographie de la surface d'attaque externe :** Identification des sous-domaines, des serveurs de messagerie, des portails VPN ou des interfaces de supervision technique exposés par inadvertance sur Internet.
## Remédiations & Sécurisation
- **Sensibilisation des collaborateurs :** Former le personnel aux risques inhérents à la divulgation d'informations professionnelles sur des plateformes publiques.
- **Surveillance de l'empreinte numérique :** Maintenir une veille continue sur les fuites de données impliquant le nom de domaine de l'organisation.
- **Politique d'accès robuste :** Imposer systématiquement l'authentification multi-facteurs (MFA) sur les services exposés afin d'annihiler l'exploitabilité des identifiants compromis découverts lors de l'investigation OSINT.
---
Note : Première étape systématique d'un audit de sécurité structuré, visant à évaluer l'exposition publique de l'organisation.