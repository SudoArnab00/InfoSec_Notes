
## About
Dumping SAM + SYSTEM is a core persistence/escalation technique - Lateral movement
Authenticate to remote services (WinRM, SMB, RDP, PSSessions) using stolen NTML hash of a user's password.
**No need to know or crack the password**
Hash is the 'ticket' for NTLM rides"

## MITRE ATT&CK
T1550.002  (Use Alternate Authentication Material: Pass the Hash)

## Core vulnerability
Windows stores passwords as hashes (one-way cryptographic representations) in the [SAM database](/SAM.md) (for local accounts) or NTDS.dit (for domain accounts).
### Protocol flaw
- Client doesnt send the password - ==sends a challenge-response derived from the hash==
	- Server sends random challenge
	- Client computes response using the NTLM hash
	- Server verifies using its stored hash
 
- ==If you have the hash - you can compute valid responses - no plain text needed==

## Requirements
To extract local password hashes, you need both:
- **SAM hive** → Contains the encrypted hashes.
- **SYSTEM hive** → Contains the boot key (syskey) needed to decrypt the hashes.
**Privileges**: Backup Operators Group
To extract the backup of the hives

## Common tools
- evil-winrm
- Impacket

## Detection & Mitigation

### Logs
Event ID 4624 Logon with logon type Network + no password; unexpected NTLM authN

### Mitigate
- Force Kerberos (Disable NTLM)
- Credential Guard (Windows 10+ Enterprise ) isolates hashes
- Restricted Admin Mode for RDP
- Monitor hash dumping (e.g., reg save on hives)