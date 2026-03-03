---
{"dg-publish":true,"permalink":"/architecture-et-securisation-active-directory/","tags":["gardenEntry"]}
---


# 🛡️ Architecture, Durcissement et Supervision d'Infrastructure (SIEM) : Occitanie-IT

## 📋 1. Contexte du Projet et Enjeux

Prélude de mon intervention offensive (Pentest), ce projet de "Blue Teaming" a consisté à bâtir, auditer et sécuriser de l'intérieur l'infrastructure de la PME **Occitanie-IT**.

Suite à une forte croissance de l'entreprise, le système d'information central, basé sur Windows Server 2022 et Active Directory, avait accumulé une dette technique dangereuse. L'objectif de ma mission, réalisée en approche "White-Box" (avec accès administrateur), était d'identifier les vulnérabilités structurelles, de durcir le système via les GPO, et de déployer une supervision proactive.

## 🔍 2. Phase 1 : Audit Interne et Cartographie (PingCastle)

Avant toute modification, j'ai procédé à un état des lieux complet de l'Active Directory à l'aide de l'outil **PingCastle**. Le rapport initial a révélé un score de risque critique de **57/100** dû à des configurations "par défaut" :

- **Dette Technique :** Présence de protocoles obsolètes (SMBv1, NTLMv1) exposant l'entreprise aux ransomwares (type WannaCry) et à l'interception réseau.
    
- **Identités :** Politique de mots de passe trop permissive (autorisant moins de 8 caractères) facilitant les attaques par force brute.
    
- **Surface d'attaque :** Service "Spouleur d'impression" actif sur le Contrôleur de Domaine (vulnérabilité PrintNightmare).
    

## 🧱 3. Phase 2 : Plan de Remédiation et Durcissement (Hardening)

Pour réduire drastiquement la surface d'attaque sans impacter la production, j'ai modélisé et déployé une architecture de sécurité stricte via les **Stratégies de Groupe (GPO)** en m'appuyant sur les guides d'hygiène de l'ANSSI :

- **Sécurisation des protocoles :** Désinstallation pure et simple de la fonctionnalité SMBv1 et forçage de l'authentification NTLMv2.
    
- **Gestion des Identités :** Obligation de mots de passe de 14 caractères minimum, ajout des administrateurs au groupe restreint _Protected Users_, et déploiement de la solution **LAPS** (Local Administrator Password Solution) pour éviter les mouvements latéraux.
    
- **Résilience :** Activation de la Corbeille Active Directory (Recycle Bin) pour prévenir les suppressions accidentelles ou malveillantes.
    

## 👁️ 4. Phase 3 : Déploiement du SIEM et Supervision (Wazuh & VirusTotal)

Un système durci reste "aveugle" s'il n'est pas surveillé. J'ai donc conçu et déployé un serveur de supervision sous **Ubuntu Server** hébergeant la solution SIEM **Wazuh**.

### Onboarding de l'Agent et Stratégie d'Audit

Après configuration de l'adresse IP statique du serveur Linux, j'ai déployé l'agent Wazuh sur le Contrôleur de Domaine Windows via PowerShell. Parallèlement, une GPO d'"Audit Avancé" a été activée pour forcer Windows à journaliser les événements critiques (connexions, modifications de groupes, accès aux objets).

_(Extrait du script de déploiement de l'agent)_

PowerShell

```
Invoke-WebRequest -Uri [https://packages.wazuh.com/4.x/windows/wazuh-agent-4.9.2-1.msi](https://packages.wazuh.com/4.x/windows/wazuh-agent-4.9.2-1.msi) -OutFile $env:tmp\wazuh-agent; msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER="192.168.1.77"
```

### File Integrity Monitoring (FIM) & Active Response

Pour protéger les données sensibles (ex: RH, Brevets), j'ai configuré le module de surveillance d'intégrité des fichiers (FIM) de Wazuh pour alerter en temps réel de toute modification. De plus, j'ai automatisé la défense du serveur (Active Response). En cas de détection d'une attaque par Force Brute (multiples Event ID 4625), le SIEM déclenche de manière autonome un script de bannissement d'IP.

_(Configuration de la réponse active dans ossec.conf)_

XML

```
<active-response>
  <command>win_route-null</command>
  <location>local</location>
  <level>10</level>
  <timeout>60</timeout>
</active-response>
```

### Intégration VirusTotal

Pour la qualification en temps réel des fichiers malveillants, l'API de VirusTotal a été intégrée au Manager. Le système a été testé avec succès via le dépôt d'une charge virale inoffensive (EICAR), immédiatement classifiée comme malveillante par le Dashboard.

## 💡 5. Bilan du Projet

Ce projet complet m'a permis d'assimiler la méthodologie de sécurisation globale d'un Système d'Information : **Auditer, Durcir, Surveiller**. Nous sommes passés d'une infrastructure "muette" et vulnérable à une architecture résiliente, capable d'identifier les failles, de réagir de manière autonome aux menaces (IPS) et d'offrir une véritable visibilité aux équipes d'administration.