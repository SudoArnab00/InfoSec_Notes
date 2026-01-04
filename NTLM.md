
## NTLM - New Technology LAN Manager

- **NetNTLM** is a challenge-response-based scheme used in NTLM - forwarded to Domain Controller - heavily used by the services on a network - can be exposed to internet.
- Often refered to as Windows Authentication / NTLM Authentication
- Plays the role of a middle man between the client and AD
- Application is authenticating on behalf of the users 
- Prevents the app from storing AD creds - will only be stored on the Domain Controller

- Some popular examples:
    - Internally-hosted Exchange (Mail) servers that expose an Outlook Web App (OWA) login portal.
    - Remote Desktop Protocol (RDP) service of a server being exposed to the internet
    - Exposed VPN endpoints that were integrated with AD.
    - Web applications that are internet-facing and make use of NetNTLM.

### Attack
To extract NTLM hashes, we can either use mimikatz to read the local SAM or extract hashes directly from LSASS memory. [Pass-the-Hash Attack](/Attack-PtH.md)

Since there are always Auto locked account is configured in almost all AD setups, so [BruteForce attack]() is useless. 

Attack to consider [Password Spray attack](https://owasp.org/www-community/attacks/Password_Spraying_Attack).

