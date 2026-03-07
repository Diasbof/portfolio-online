---
{"dg-publish":true,"permalink":"/sheet-cheats/concept-methodologies-mitre-att-and-ck-et-ptes/"}
---

# Méthodologies d'Audit : MITRE ATT&CK et PTES
> [!ABSTRACT] Définition
> L'évaluation de la sécurité d'un système d'information exige des cadres standardisés pour garantir la rigueur, la légalité et l'exhaustivité des tests. Le PTES structure le déroulement d'un audit offensif, tandis que le framework MITRE ATT&CK cartographie le comportement des adversaires pour structurer la défense.
## Le Standard PTES

Le Penetration Testing Execution Standard (PTES) définit les sept phases fondamentales d'un test d'intrusion. Il encadre la prestation d'un point de vue technique et juridique.
1. **Pre-engagement Interactions :** Définition du périmètre de l'audit, des règles d'engagement et contractualisation légale.
2. **Intelligence Gathering :** Collecte d'informations (OSINT) et reconnaissance réseau.
3. **Threat Modeling :** Modélisation des menaces ciblant spécifiquement l'infrastructure ou le secteur d'activité.
4. **Vulnerability Analysis :** Identification des failles applicatives ou de configuration (ex: services obsolètes ou permissions excessives sur un annuaire).
5. **Exploitation :** Tentatives d'intrusion réelles basées sur les vulnérabilités validées lors de la phase précédente.
6. **Post-Exploitation :** Maintien de l'accès, élévation de privilèges (par exemple via [[Sheet Cheats/Concept - Attaque DCSync\|DCSync]]) et démonstration de l'impact métier.
7. **Reporting :** Rédaction des livrables techniques et exécutifs, incluant le plan de remédiation.
## Le Framework MITRE ATT&CK

Le MITRE ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge) est une base de connaissances mondiale recensant les tactiques et techniques exploitées par les cyberattaquants.
- **Tactiques :** Représentent l'objectif à haut niveau de l'attaquant (le "Pourquoi"). Par exemple, la tactique "Lateral Movement" (TA0008).
- **Techniques :** Décrivent la méthode opérationnelle utilisée pour atteindre cet objectif (le "Comment"). Par exemple, l'attaque [[Sheet Cheats/Concept - Pass-the-Hash\|Pass-the-Hash]] est référencée sous la technique T1550.002.
## Intégration et Supervision (SIEM)
L'utilisation conjointe de ces référentiels est primordiale pour le déploiement d'une stratégie de détection (Blue Team) :
- **Cartographie Wazuh :** Lors de la configuration de Wazuh sur une infrastructure Windows Server, chaque règle d'alerte personnalisée doit intégrer le tag de la technique MITRE ATT&CK correspondante.
- **Évaluation de Couverture :** Cela permet aux analystes d'évaluer la maturité défensive de l'Active Directory en identifiant visuellement les angles morts dans la matrice de détection.
---
Note : La maîtrise de ces référentiels est indispensable pour structurer une démarche d'audit, de remédiation et de supervision de niveau professionnel.