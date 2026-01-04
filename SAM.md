
## SAM - Security Accounts Manager

- built-in Windows **database**
- securely stores credentials for ==local user accounts== on a machine
	- not domain accounts, which use Active Directory
- holds: username and hashed passwords (one way cryptographic hashes)
	- primarily NTML hashes (passwords are never stored in plain text)
		- older LM hashes are disabled in new windows
	- other account details: Relative IDs, group memberships and security descriptors
 
 The SAM database is a ==registry hive== file located at: `C:\Windows\System32\config\SAM` - **mounted in** `HKEY_LOCAL_MACHINE\SAM` 
 - heavily restricted key
 - admins cannot view it directly while windows is running
 - file is locked by kernel during operation - no copy or access
 - part of windows registry hive

**Note**: NTML AuthN is still used widely for legacy compatibility
