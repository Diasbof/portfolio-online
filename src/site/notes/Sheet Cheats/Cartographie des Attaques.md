---
{"dg-publish":true,"permalink":"/sheet-cheats/cartographie-des-attaques/"}
---

# Cartographie des Attaques et Veille Technologique
> [!INFO] Présentation
> Cette page référence l'ensemble des techniques, vulnérabilités et protocoles explorés lors de mes recherches. Ils constituent le socle théorique de ma démarche d'audit d'infrastructure et de supervision sécurité.
## 1. Reconnaissance et Énumération
- [[Sheet Cheats/Concept - Reconnaissance OSINT\|OSINT (Intelligence en sources ouvertes)]]
- [[Sheet Cheats/Concept - Découverte Réseau Nmap\|Découverte Réseau et Balayage de ports (Nmap)]]
- [[Sheet Cheats/Concept - Enumeration LDAP et RPC\|Énumération Active Directory (LDAP et RPC)]]
- [[Sheet Cheats/Concept - BloodHound & Analyse de Graphes\|BloodHound (Analyse des chemins d'attaque)]]
## 2. Accès Initial et Élévation de Privilèges
- [[Sheet Cheats/Concept - Empoisonnement LLMNR-NBTNS\|Empoisonnement LLMNR / NBT-NS (Interception réseaux)]]
- [[Sheet Cheats/Concept - Vulnérabilité GPP (cpassword)\|Exploitation GPP (cpassword)]]
- [[Sheet Cheats/Concept - AS-REP Roasting\|AS-REP Roasting (Contournement de pré-authentification)]]
- [[Sheet Cheats/Concept - Kerberoasting\|L'Attaque Kerberoasting (Cassage de tickets de service)]]
- [[Sheet Cheats/Concept - Elevation SeImpersonate\|Élévation Locale (Abus de SeImpersonatePrivilege)]]
## 3. Mouvements Latéraux
- [[Sheet Cheats/Concept - Pass-the-Hash\|Pass-the-Hash (Mouvement latéral NTLM)]]
- [[Sheet Cheats/Concept - Pass-the-Ticket\|Pass-the-Ticket (Manipulation de tickets Kerberos)]]
- [[Sheet Cheats/Concept - SMB Relay\|Attaque SMB Relay (Interception et relais d'authentification)]]
- [[Sheet Cheats/Concept - Mouvement Lateral WMI\|Exécution furtive via WMI / DCOM (Living off the Land)]]
## 4. Persistance et Compromission Totale
- [[Sheet Cheats/Concept - Attaque DCSync\|L'Attaque DCSync (Réplication AD)]]
- [[Sheet Cheats/Concept - Attaque Golden Ticket\|Golden Ticket (Compromission krbtgt)]]
- [[Sheet Cheats/Concept - Zerologon (CVE-2020-1472)\|Zerologon (Faille Netlogon)]]
## 5. Protocoles Réseau (Infrastructure)
- [[Sheet Cheats/Concept - Attaques SMB et NetBIOS\|Attaques SMB et NetBIOS (Ports 139/445)]]
- [[Sheet Cheats/Concept - Attaques DNS\|Attaques DNS (Port 53)]]
- [[Sheet Cheats/Concept - Attaques RDP\|Attaques RDP (Port 3389)]]
- [[Sheet Cheats/Concept - Attaques DHCP\|Attaques DHCP (Attribution d'IP)]]
- [[Sheet Cheats/Concept - Attaques SSH\|Attaques SSH (Port 22)]]
- [[Sheet Cheats/Concept - Attaques SNMP\|Attaques SNMP (Supervision réseau)]]
- [[Sheet Cheats/Concept - Attaques FTP\|Attaques FTP (Port 21)]]
- [[Sheet Cheats/Concept - Attaques Telnet\|Attaques Telnet (Port 23)]]
- [[Sheet Cheats/Concept - Attaques Messagerie\|Attaques SMTP / POP / IMAP (Mails)]]
## 6. Défense et Méthodologie
- [[Sheet Cheats/Concept - Microsoft LAPS\|Microsoft LAPS (Hardening AD)]]
- [[Sheet Cheats/Concept - Méthodologies MITRE ATT&CK et PTES\|Frameworks d'Audit MITRE ATT&CK et PTES]]

