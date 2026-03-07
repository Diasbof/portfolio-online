---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-pass-the-hash/"}
---

# L'Attaque Pass-the-Hash (PtH)
> [!ABSTRACT] Définition
> Le Pass-the-Hash (PtH) est une technique de mouvement latéral permettant à un attaquant de s'authentifier sur un système ou un réseau distant en utilisant le condensé cryptographique (hash NTLM) du mot de passe d'un utilisateur, sans jamais avoir besoin d'en connaître la valeur en clair.
## Mécanisme Technique
Le protocole d'authentification NTLM, encore présent par rétrocompatibilité sur de nombreux réseaux, ne requiert pas le mot de passe en clair pour valider une session, mais demande simplement de prouver la possession du hash. Si un attaquant parvient à compromettre une machine et à extraire les hashs stockés en mémoire (particulièrement dans le processus système LSASS), il peut injecter ces hashs dans sa propre session pour usurper l'identité de l'utilisateur légitime sur d'autres serveurs du domaine.
## Exploitation Opérationnelle
1. **Extraction :** Utilisation d'outils spécialisés (ex: Mimikatz) ou de techniques de vidage mémoire (dump) du processus `lsass.exe` pour récupérer les hashs NTLM des utilisateurs ayant des sessions actives ou récentes sur la machine compromise.
2. **Injection :** Utilisation de scripts (ex: `psexec.py` ou `wmiexec.py` de la suite Impacket) pour présenter ce hash à une machine cible.
3. **Accès :** Obtention d'une exécution de commandes à distance ou d'un accès aux partages avec les privilèges de l'utilisateur usurpé.
## Remédiations & Détection
- **Groupe Protected Users :** Dans un Active Directory sous Windows Server 2019, l'ajout des comptes à hauts privilèges (comme les administrateurs du domaine) au groupe "Protected Users" empêche la mise en cache de leurs hashs NTLM en mémoire.
- **Windows Defender Credential Guard :** Activer cette fonctionnalité de sécurité basée sur la virtualisation pour isoler le processus LSASS et empêcher l'extraction des secrets.
- **Supervision Wazuh :** Configurer les agents pour remonter et alerter sur l'Event ID 4624 (Logon) avec un type de connexion réseau (Logon Type 3) associé à des processus d'authentification suspects. Il est également critique de surveiller l'Event ID 4656 ou 4663 concernant les tentatives d'accès en lecture au processus LSASS.
---
👉 *Technique fondamentale justifiant la mise en place d'une administration en modèle de silos (Tiering Model).*