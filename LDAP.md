
## LDAP - Lightweight Directory Access Protocol
AuthN Similar to [NTML authN](/NTLM.md)

**LDAP authN** 
    - application directly verifies the user's credentials 
    - app has a pair of AD creds that it can use first to query LDAP and then verify the AD user's credentials
    - popular mechanism with 3rd party (non-microsoft) apps that integrate with AD, like
        - Jenkins
        - custom web apps
        - printers
        - VPNs

### Risk
- Risky if app or service is internet exposed 
- Risky since LDAP using service/app requires a set of AD creds to be stored with it
- Creds/config file often stored in plain text (relies on keeping the location and storage configuration file secure rather than its contents)

### Attacks
- [LDAP Pass-back attack](https://raxis.com/blog/ldap-passback/) 
    - common against network devices like printer
    - gain access to a device's config where the LDAP parameters are specified (e.g., web interface of a network printer)
    - since passwords are generally hidden (if default not changed), we can alter the LDAP configuration, such as the IP or hostname of the LDAP server. 
    - In an LDAP Pass-back attack, we can modify this IP to our IP and then test the LDAP configuration, which will force the device to attempt LDAP authentication to our rogue device. We can intercept this authentication attempt to recover the LDAP credentials.

Use OpenLDAP to host a rogue LDAP server:
`sudo apt-get update && sudo apt-get -y install slapd ldap-utils && sudo systemctl enable slapd`

Configure it:
`sudo dpkg-reconfigure -p low slapd`

use the ldif file to patch our LDAP server using:
`sudo ldapmodify -Y EXTERNAL -H ldapi:// -f ./olcSaslSecProps.ldif && sudo service slapd restart`

Verify the LDAP server config:
`ldapsearch -H ldap:// -x -LLL -s base -b "" supportedSASLMechanisms`

Note:
**olcSaslSecProps**: Specifies the SASL security properties
**noanonymous**: Disables mechanisms that support anonymous login
**minssf**: Specifies the minimum acceptable security strength with 0, meaning no protection.

## Mitigation
Enforce Network Access Control (NAC) - NAC can prevent attackers from connecting rogue devices on the network. 
However, it will require quite a bit of effort since legitimate devices will have to be allowlisted.