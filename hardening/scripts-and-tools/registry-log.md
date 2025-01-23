# Registry Log

This documentation provides an overview of the **Registry Compliance Audit Script**, its purpose, usage, and implementation details to ensure effective auditing of local registry keys across your infrastructure.

{% hint style="info" %}
The primary goal of this script is to **verify compliance** across registry settings on multiple computers. It ensures that all security and operational configurations enforced via GPOs are properly applied and identifies deviations for corrective actions.
{% endhint %}

{% hint style="warning" %}
Without visibility into registry compliance, misconfigurations may go unnoticed, leaving systems vulnerable to exploitation.
{% endhint %}

***

{% code title="RegistryComplianceChecker.ps1" %}
```powershell
# Récupération du nom de l'ordinateur
$Hostname = $env:COMPUTERNAME

# Récupération de la date au format jour-mois-année
$CurrentDate = (Get-Date -Format 'dd-MM-yyyy')

# Chemins de sortie des fichiers CSV et HTML
$OutputCsvPath = "C:\tools\RegistryAuditResults_${Hostname}_${CurrentDate}.csv"
$OutputHtmlPath = "C:\tools\RegistryAuditResults_${Hostname}_${CurrentDate}.html"

# Configuration of registry keys with descriptions and reasons for expected values
$RegistryKeys = @(
@{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa"; 
        ValueName = "RunAsPPL"; 
        ExpectedValue = 2; 
        Description = "Enables Process Protection Level (PPL) for local accounts to enhance LSA security."; 
        ExpectedValueReason = "Setting this to 2 forces LSA to run in a protected process mode, increasing resistance to tampering and credential theft."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa"; 
        ValueName = "DisableRestrictedAdmin"; 
        ExpectedValue = 0; 
        Description = "Prevents the use of Restricted Admin mode, which could expose sensitive credentials."; 
        ExpectedValueReason = "Setting this to 0 disables Restricted Admin mode, preventing attackers from bypassing credential protections during RDP sessions."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa"; 
        ValueName = "NoLMHash"; 
        ExpectedValue = 1; 
        Description = "Disables storage of LM hashes for passwords."; 
        ExpectedValueReason = "Setting this to 1 ensures LM hashes are not stored, reducing the risk of brute-force attacks."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa"; 
        ValueName = "DisableRestrictedAdminOutboundCreds"; 
        ExpectedValue = 1; 
        Description = "Prevents restricted admin credentials from being used for outbound authentication."; 
        ExpectedValueReason = "Setting this to 1 ensures outbound authentication uses only secure methods."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa"; 
        ValueName = "LmCompatibilityLevel"; 
        ExpectedValue = 5; 
        Description = "Sets the level of NTLM authentication compatibility."; 
        ExpectedValueReason = "Setting this to 5 ensures NTLMv2 is used exclusively, enhancing authentication security."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa"; 
        ValueName = "EveryoneIncludesAnonymous"; 
        ExpectedValue = 0; 
        Description = "Disables inclusion of anonymous users in the Everyone group."; 
        ExpectedValueReason = "Setting this to 0 ensures anonymous users are excluded, improving access control."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa"; 
        ValueName = "DisableDomainCreds"; 
        ExpectedValue = 1; 
        Description = "Prevents storage of domain credentials locally."; 
        ExpectedValueReason = "Setting this to 1 ensures domain credentials are not stored on the local machine, reducing attack surface."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest"; 
        ValueName = "UseLogonCredential"; 
        ExpectedValue = 0; 
        Description = "Disables WDigest logon credential caching to reduce risk of credential theft."; 
        ExpectedValueReason = "Setting this to 0 ensures WDigest authentication does not store plaintext credentials in memory, mitigating potential credential dumping attacks."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest"; 
        ValueName = "Negotiate"; 
        ExpectedValue = 0; 
        Description = "Disables negotiation for WDigest."; 
        ExpectedValueReason = "Setting this to 0 ensures negotiation-based authentication methods are disabled, reducing attack surface."
    },
    @{
        Key = "HKLM:\System\CurrentControlSet\Services\Netlogon\Parameters"; 
        ValueName = "RequireSignOrSeal"; 
        ExpectedValue = 1; 
        Description = "Requires signing or sealing for secure Netlogon communication."; 
        ExpectedValueReason = "Setting this to 1 ensures Netlogon traffic is encrypted or signed, preventing tampering."
    },
    @{
        Key = "HKLM:\System\CurrentControlSet\Services\Netlogon\Parameters"; 
        ValueName = "SealSecureChannel"; 
        ExpectedValue = 1; 
        Description = "Enables secure channel encryption for Netlogon."; 
        ExpectedValueReason = "Setting this to 1 ensures secure channels are encrypted, enhancing data confidentiality."
    },
    @{
        Key = "HKLM:\System\CurrentControlSet\Services\Netlogon\Parameters"; 
        ValueName = "SignSecureChannel"; 
        ExpectedValue = 1; 
        Description = "Enables secure channel signing for Netlogon."; 
        ExpectedValueReason = "Setting this to 1 ensures secure channels are signed, preventing tampering."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\System"; 
        ValueName = "FilterAdministratorToken"; 
        ExpectedValue = 1; 
        Description = "Enables filtering of administrator tokens."; 
        ExpectedValueReason = "Setting this to 1 ensures admin accounts use filtered tokens, improving security in UAC."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\System"; 
        ValueName = "LocalAccountTokenFilterPolicy"; 
        ExpectedValue = 0; 
        Description = "Disables token filtering for local accounts."; 
        ExpectedValueReason = "Setting this to 0 ensures local accounts cannot bypass token filtering, improving security."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\LDAP"; 
        ValueName = "LDAPClientIntegrity"; 
        ExpectedValue = 2; 
        Description = "Sets the LDAP client integrity level to require signing."; 
        ExpectedValueReason = "Setting this to 2 ensures LDAP communications are signed, preventing tampering."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services"; 
        ValueName = "SecurityLayer"; 
        ExpectedValue = 2; 
        Description = "Enforces a specific security layer for RDP connections."; 
        ExpectedValueReason = "Setting this to 2 ensures SSL/TLS is used for secure RDP communication."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services"; 
        ValueName = "UserAuthentication"; 
        ExpectedValue = 1; 
        Description = "Enables user authentication for RDP connections."; 
        ExpectedValueReason = "Setting this to 1 ensures that RDP connections require user authentication, preventing unauthorized access."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services"; 
        ValueName = "fEncryptRPCTraffic"; 
        ExpectedValue = 1; 
        Description = "Encrypts RPC traffic in Terminal Services."; 
        ExpectedValueReason = "Setting this to 1 ensures all RPC traffic is encrypted, improving confidentiality."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services"; 
        ValueName = "KeepAliveInterval"; 
        ExpectedValue = 1; 
        Description = "Defines the interval for RDP keep-alive messages."; 
        ExpectedValueReason = "Setting this to 1 ensures frequent keep-alive checks, preventing session timeouts."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services"; 
        ValueName = "DeleteTempDirsOnExit"; 
        ExpectedValue = 1; 
        Description = "Enables deletion of temporary directories upon session exit."; 
        ExpectedValueReason = "Setting this to 1 ensures temporary data is removed after sessions, reducing data leakage."
    }
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services"; 
        ValueName = "MinEncryptionLevel"; 
        ExpectedValue = 1; 
        Description = "Sets the minimum encryption level for RDP connections."; 
        ExpectedValueReason = "Setting this to 1 ensures that all RDP connections use at least the minimum level of encryption, protecting session data."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters"; 
        ValueName = "SMB1"; 
        ExpectedValue = 0; 
        Description = "Disables the SMBv1 protocol."; 
        ExpectedValueReason = "Setting this to 0 ensures SMBv1 is disabled, reducing exposure to outdated and vulnerable protocols like EternalBlue."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters"; 
        ValueName = "EnableSecuritySignature"; 
        ExpectedValue = 1; 
        Description = "Enables SMB signing for server connections."; 
        ExpectedValueReason = "Setting this to 1 ensures that SMB traffic is signed to improve data integrity."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters"; 
        ValueName = "RequireSecuritySignature"; 
        ExpectedValue = 1; 
        Description = "Requires SMB signing for server connections."; 
        ExpectedValueReason = "Setting this to 1 ensures that all SMB connections are signed, preventing tampering."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters"; 
        ValueName = "AutoShareWks"; 
        ExpectedValue = 0; 
        Description = "Disables administrative share creation for workstations."; 
        ExpectedValueReason = "Setting this to 0 prevents the automatic creation of administrative shares, reducing unauthorized access risks."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters"; 
        ValueName = "AutoShareServer"; 
        ExpectedValue = 0; 
        Description = "Disables administrative share creation for servers."; 
        ExpectedValueReason = "Setting this to 0 prevents the automatic creation of administrative shares on servers, reducing unauthorized access risks."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters"; 
        ValueName = "RestrictNullSessAccess"; 
        ExpectedValue = 1; 
        Description = "Restricts null session access to shared resources."; 
        ExpectedValueReason = "Setting this to 1 ensures that null session access is limited, enhancing resource security."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters"; 
        ValueName = "EnablePlainTextPassword"; 
        ExpectedValue = 0; 
        Description = "Disables plaintext password usage for SMB connections."; 
        ExpectedValueReason = "Setting this to 0 ensures that only encrypted passwords are used for SMB authentication, improving security."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\Rdr\Parameters"; 
        ValueName = "EnableSecuritySignature"; 
        ExpectedValue = 1; 
        Description = "Enables SMB signing for redirector connections."; 
        ExpectedValueReason = "Setting this to 1 ensures SMB traffic is signed, protecting data integrity."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\Rdr\Parameters"; 
        ValueName = "RequireSecuritySignature"; 
        ExpectedValue = 1; 
        Description = "Requires SMB signing for redirector connections."; 
        ExpectedValueReason = "Setting this to 1 ensures SMB traffic is always signed, reducing the risk of tampering."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation"; 
        ValueName = "AllowInsecureGuestAuth"; 
        ExpectedValue = 0; 
        Description = "Disables insecure guest authentication."; 
        ExpectedValueReason = "Setting this to 0 ensures guest authentication requires secure methods, preventing unauthorized access."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters"; 
        ValueName = "EnableSecuritySignature"; 
        ExpectedValue = 1; 
        Description = "Enables SMB signing for workstation connections."; 
        ExpectedValueReason = "Setting this to 1 ensures SMB signing is used, protecting data integrity."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters"; 
        ValueName = "RequireSecuritySignature"; 
        ExpectedValue = 1; 
        Description = "Requires SMB signing for workstation connections."; 
        ExpectedValueReason = "Setting this to 1 ensures all SMB connections are signed, enhancing security."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters"; 
        ValueName = "EnablePlainTextPassword"; 
        ExpectedValue = 0; 
        Description = "Disables plaintext password usage in SMB connections."; 
        ExpectedValueReason = "Setting this to 0 ensures only encrypted passwords are used, improving security."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\Netbt\Parameters"; 
        ValueName = "NodeType"; 
        ExpectedValue = 2; 
        Description = "Defines the NodeType for NetBIOS over TCP/IP."; 
        ExpectedValueReason = "Setting this ensures NetBIOS uses a specific mode for name resolution, reducing conflicts."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\DNSClient"; 
        ValueName = "EnableMulticast"; 
        ExpectedValue = 0; 
        Description = "Disables multicast for the DNS client."; 
        ExpectedValueReason = "Setting this to 0 reduces potential attack surface by disabling multicast DNS."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\WinHttpAutoProxySvc"; 
        ValueName = "Start"; 
        ExpectedValue = 4; 
        Description = "Sets the startup type for the WinHTTP Web Proxy Auto-Discovery service."; 
        ExpectedValueReason = "Setting this to 4 disables the service, reducing unnecessary resource usage and potential risks."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\WebClient"; 
        ValueName = "Start"; 
        ExpectedValue = 4; 
        Description = "Sets the startup type for the WebClient service."; 
        ExpectedValueReason = "Setting this to 4 disables the service, reducing unnecessary resource usage and potential vulnerabilities."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\XboxGipSvc"; 
        ValueName = "Start"; 
        ExpectedValue = 4; 
        Description = "Sets the startup type for the Xbox Game Input Protocol service."; 
        ExpectedValueReason = "Setting this to 4 disables the service, reducing unnecessary resource usage and attack surface."
    }
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\XblAuthManager"; 
        ValueName = "Start"; 
        ExpectedValue = 4; 
        Description = "Sets the startup type for the Xbox Live Auth Manager service."; 
        ExpectedValueReason = "Setting this to 4 disables the service, reducing unnecessary resource usage and attack surface."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\XblGameSave"; 
        ValueName = "Start"; 
        ExpectedValue = 4; 
        Description = "Sets the startup type for the Xbox Live Game Save service."; 
        ExpectedValueReason = "Setting this to 4 disables the service, reducing unnecessary resource usage."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\XboxNetApiSvc"; 
        ValueName = "Start"; 
        ExpectedValue = 4; 
        Description = "Sets the startup type for the Xbox Networking API service."; 
        ExpectedValueReason = "Setting this to 4 disables the service, reducing unnecessary resource usage."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\Spooler"; 
        ValueName = "Start"; 
        ExpectedValue = 4; 
        Description = "Sets the startup type for the Print Spooler service."; 
        ExpectedValueReason = "Setting this to 4 disables the service, mitigating risk of PrintNightmare vulnerabilities."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer"; 
        ValueName = "Start"; 
        ExpectedValue = 4; 
        Description = "Sets the startup type for the Server service."; 
        ExpectedValueReason = "Setting this to 4 disables the service, reducing unnecessary resource usage."
    },
    @{
        Key = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\Wpad"; 
        ValueName = "WpadOverride"; 
        ExpectedValue = 0; 
        Description = "Disables Web Proxy Auto-Discovery Protocol (WPAD)."; 
        ExpectedValueReason = "Setting this to 0 ensures WPAD is disabled, mitigating risks of proxy-based attacks."
    },
    @{
        Key = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings"; 
        ValueName = "AutoDetect"; 
        ExpectedValue = 0; 
        Description = "Disables automatic detection of proxy settings."; 
        ExpectedValueReason = "Setting this to 0 prevents the system from automatically detecting proxies, reducing potential attack vectors."
    },
    @{
        Key = "HKLM:\System\currentcontrolset\services\tcpip6\parameters"; 
        ValueName = "DisabledComponents"; 
        ExpectedValue = 32; 
        Description = "Disables certain IPv6 features."; 
        ExpectedValueReason = "Setting this to 32 disables IPv6 tunneling, reducing potential attack surface."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate"; 
        ValueName = "WUServer"; 
        ExpectedValue = ""; 
        Description = "Specifies the Windows Update Server."; 
        ExpectedValueReason = "Leaving this empty ensures the system does not rely on a non-secure Windows Update server."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"; 
        ValueName = "UseWUServer"; 
        ExpectedValue = 1; 
        Description = "Enforces the use of a specific Windows Update server."; 
        ExpectedValueReason = "Setting this to 1 ensures updates are retrieved from a defined, secure server."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters"; 
        ValueName = "SearchList"; 
        ExpectedValue = "suffix-dns.mycorp.local,suffix2.corp.lo"; 
        Description = "Defines the DNS search suffix list."; 
        ExpectedValueReason = "Setting this ensures only specific DNS suffixes are queried, enhancing network security."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\DNSClient"; 
        ValueName = "SearchList"; 
        ExpectedValue = "suffix-dns.mycorp.local,suffix2.corp.lo"; 
        Description = "Defines the DNS search suffix list for the client."; 
        ExpectedValueReason = "Setting this ensures that only specific DNS suffixes are queried, reducing unnecessary DNS queries."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\System"; 
        ValueName = "DontDisplayNetworkSelectionUI"; 
        ExpectedValue = 1; 
        Description = "Disables the network selection UI on the logon screen."; 
        ExpectedValueReason = "Setting this to 1 prevents users from changing network settings at the logon screen, improving security."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters"; 
        ValueName = "EnableMDNS"; 
        ExpectedValue = 0; 
        Description = "Disables multicast DNS (mDNS)."; 
        ExpectedValueReason = "Setting this to 0 ensures mDNS is disabled, reducing exposure to network discovery attacks."
    },
    @{
        Key = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Device Installer"; 
        ValueName = "DisableCoInstallers"; 
        ExpectedValue = 1; 
        Description = "Disables co-installers for device drivers."; 
        ExpectedValueReason = "Setting this to 1 ensures that additional co-installers are not used, reducing the attack surface for driver installation."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service"; 
        ValueName = "AllowBasic"; 
        ExpectedValue = 0; 
        Description = "Disables Basic authentication for WinRM."; 
        ExpectedValueReason = "Setting this to 0 ensures that Basic authentication, which transmits credentials in plaintext, is disabled."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service"; 
        ValueName = "AllowDigest"; 
        ExpectedValue = 0; 
        Description = "Disables Digest authentication for WinRM."; 
        ExpectedValueReason = "Setting this to 0 ensures Digest authentication, which has vulnerabilities, is disabled."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service"; 
        ValueName = "AllowKerberos"; 
        ExpectedValue = 1; 
        Description = "Enables Kerberos authentication for WinRM."; 
        ExpectedValueReason = "Setting this to 1 ensures Kerberos authentication is used for secure communication."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service"; 
        ValueName = "CbtHardeningLevel"; 
        ExpectedValue = "Strict"; 
        Description = "Enforces strict Channel Binding Tokens (CBT) for WinRM."; 
        ExpectedValueReason = "Setting this to 'Strict' ensures that Channel Binding Tokens are enforced, improving session integrity."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service"; 
        ValueName = "AllowNegotiate"; 
        ExpectedValue = 0; 
        Description = "Disables Negotiate authentication for WinRM."; 
        ExpectedValueReason = "Setting this to 0 ensures that Negotiate authentication is disabled, reducing potential attack vectors."
    }
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WinRM\Client"; 
        ValueName = "AllowBasic"; 
        ExpectedValue = 0; 
        Description = "Disables Basic authentication for WinRM client."; 
        ExpectedValueReason = "Setting this to 0 ensures that Basic authentication, which transmits credentials in plaintext, is disabled."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WinRM\Client"; 
        ValueName = "AllowDigest"; 
        ExpectedValue = 0; 
        Description = "Disables Digest authentication for WinRM client."; 
        ExpectedValueReason = "Setting this to 0 ensures Digest authentication, which has vulnerabilities, is disabled."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WinRM\Client"; 
        ValueName = "AllowKerberos"; 
        ExpectedValue = 1; 
        Description = "Enables Kerberos authentication for WinRM client."; 
        ExpectedValueReason = "Setting this to 1 ensures Kerberos authentication is used for secure communication."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WinRM\Client"; 
        ValueName = "CbtHardeningLevel"; 
        ExpectedValue = "Strict"; 
        Description = "Enforces strict Channel Binding Tokens (CBT) for WinRM client."; 
        ExpectedValueReason = "Setting this to 'Strict' ensures that Channel Binding Tokens are enforced, improving session integrity."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\WinRM\Client"; 
        ValueName = "AllowNegotiate"; 
        ExpectedValue = 0; 
        Description = "Disables Negotiate authentication for WinRM client."; 
        ExpectedValueReason = "Setting this to 0 ensures that Negotiate authentication is disabled, reducing potential attack vectors."
    },
    @{
        Key = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings"; 
        ValueName = "SecureProtocols"; 
        ExpectedValue = 10752; 
        Description = "Defines the secure protocols used by Internet Explorer."; 
        ExpectedValueReason = "Setting this ensures only modern, secure protocols (TLS 1.2 and 1.3) are used, improving communication security."
    },
    @{
        Key = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp"; 
        ValueName = "DefaultSecureProtocols"; 
        ExpectedValue = 10752; 
        Description = "Sets the default secure protocols for WinHTTP."; 
        ExpectedValueReason = "Setting this ensures only secure protocols like TLS 1.2 and 1.3 are used for HTTP connections."
    },
    @{
        Key = "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp"; 
        ValueName = "DefaultSecureProtocols"; 
        ExpectedValue = 10752; 
        Description = "Sets the default secure protocols for 32-bit applications using WinHTTP."; 
        ExpectedValueReason = "Setting this ensures secure protocols like TLS 1.2 and 1.3 are used for HTTP connections in 32-bit apps."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.3\Client"; 
        ValueName = "DisabledByDefault"; 
        ExpectedValue = 0; 
        Description = "Enables TLS 1.3 for clients."; 
        ExpectedValueReason = "Setting this to 0 ensures that TLS 1.3 is enabled for secure communication."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.3\Client"; 
        ValueName = "Enabled"; 
        ExpectedValue = 1; 
        Description = "Ensures TLS 1.3 is enabled for client connections."; 
        ExpectedValueReason = "Setting this to 1 enables TLS 1.3, enhancing secure communication."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client"; 
        ValueName = "DisabledByDefault"; 
        ExpectedValue = 0; 
        Description = "Enables TLS 1.2 for clients."; 
        ExpectedValueReason = "Setting this to 0 ensures that TLS 1.2 is enabled for secure communication."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client"; 
        ValueName = "Enabled"; 
        ExpectedValue = 1; 
        Description = "Ensures TLS 1.2 is enabled for client connections."; 
        ExpectedValueReason = "Setting this to 1 enables TLS 1.2, enhancing secure communication."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client"; 
        ValueName = "DisabledByDefault"; 
        ExpectedValue = 1; 
        Description = "Disables TLS 1.1 for clients."; 
        ExpectedValueReason = "Setting this to 1 ensures that TLS 1.1 is disabled, as it is considered outdated and insecure."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client"; 
        ValueName = "Enabled"; 
        ExpectedValue = 0; 
        Description = "Ensures TLS 1.1 is disabled for client connections."; 
        ExpectedValueReason = "Setting this to 0 ensures TLS 1.1 is disabled, improving overall security."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client"; 
        ValueName = "DisabledByDefault"; 
        ExpectedValue = 1; 
        Description = "Disables TLS 1.0 for clients."; 
        ExpectedValueReason = "Setting this to 1 ensures that TLS 1.0 is disabled, as it is outdated and vulnerable."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client"; 
        ValueName = "Enabled"; 
        ExpectedValue = 0; 
        Description = "Ensures TLS 1.0 is disabled for client connections."; 
        ExpectedValueReason = "Setting this to 0 ensures TLS 1.0 is disabled, improving overall security."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.3\Server"; 
        ValueName = "DisabledByDefault"; 
        ExpectedValue = 0; 
        Description = "Enables TLS 1.3 for servers."; 
        ExpectedValueReason = "Setting this to 0 ensures TLS 1.3 is enabled for secure communication."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.3\Server"; 
        ValueName = "Enabled"; 
        ExpectedValue = 1; 
        Description = "Ensures TLS 1.3 is enabled for server connections."; 
        ExpectedValueReason = "Setting this to 1 enables TLS 1.3, enhancing secure communication."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server"; 
        ValueName = "DisabledByDefault"; 
        ExpectedValue = 0; 
        Description = "Enables TLS 1.2 for servers."; 
        ExpectedValueReason = "Setting this to 0 ensures that TLS 1.2 is enabled for secure communication."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server"; 
        ValueName = "Enabled"; 
        ExpectedValue = 1; 
        Description = "Ensures TLS 1.2 is enabled for server connections."; 
        ExpectedValueReason = "Setting this to 1 enables TLS 1.2, enhancing secure communication."
    }
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server"; 
        ValueName = "DisabledByDefault"; 
        ExpectedValue = 1; 
        Description = "Disables TLS 1.1 for servers."; 
        ExpectedValueReason = "Setting this to 1 ensures TLS 1.1 is disabled, as it is considered outdated and insecure."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server"; 
        ValueName = "Enabled"; 
        ExpectedValue = 0; 
        Description = "Ensures TLS 1.1 is disabled for server connections."; 
        ExpectedValueReason = "Setting this to 0 ensures TLS 1.1 is disabled, improving overall security."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server"; 
        ValueName = "DisabledByDefault"; 
        ExpectedValue = 1; 
        Description = "Disables TLS 1.0 for servers."; 
        ExpectedValueReason = "Setting this to 1 ensures TLS 1.0 is disabled, as it is outdated and vulnerable."
    },
    @{
        Key = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server"; 
        ValueName = "Enabled"; 
        ExpectedValue = 0; 
        Description = "Ensures TLS 1.0 is disabled for server connections."; 
        ExpectedValueReason = "Setting this to 0 ensures TLS 1.0 is disabled, improving overall security."
    },
    @{
        Key = "HKLM:\System\CurrentControlSet\Control\Lsa\MSV1_0"; 
        ValueName = "AuditReceivingNTLMTraffic"; 
        ExpectedValue = 2; 
        Description = "Audits NTLM traffic received by the server."; 
        ExpectedValueReason = "Setting this to 2 ensures that all NTLM traffic is logged, enhancing auditing capabilities."
    },
    @{
        Key = "HKLM:\System\CurrentControlSet\Control\Lsa\MSV1_0"; 
        ValueName = "RestrictSendingNTLMTraffic"; 
        ExpectedValue = 1; 
        Description = "Restricts NTLM traffic sent by the server."; 
        ExpectedValueReason = "Setting this to 1 ensures NTLM traffic is restricted to reduce security risks."
    },
    @{
        Key = "HKLM:\System\CurrentControlSet\Services\Netlogon\Parameters"; 
        ValueName = "AuditNTLMInDomain"; 
        ExpectedValue = 7; 
        Description = "Enables auditing of NTLM usage within the domain."; 
        ExpectedValueReason = "Setting this to 7 ensures all NTLM usage is audited, improving monitoring capabilities."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\CredentialsDelegation"; 
        ValueName = "AllowDefaultCredentials"; 
        ExpectedValue = 0; 
        Description = "Disables delegation of default credentials."; 
        ExpectedValueReason = "Setting this to 0 ensures default credentials are not delegated, reducing potential credential theft."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\CredentialsDelegation"; 
        ValueName = "ConcatenateDefaults_AllowDefault"; 
        ExpectedValue = 0; 
        Description = "Disables concatenation of default credentials."; 
        ExpectedValueReason = "Setting this to 0 ensures default credentials are not concatenated, reducing attack surface."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\CredentialsDelegation"; 
        ValueName = "AllowDefCredentialsWhenNTLMOnly"; 
        ExpectedValue = 0; 
        Description = "Disables delegation of credentials when NTLM is used."; 
        ExpectedValueReason = "Setting this to 0 ensures credentials are not delegated when NTLM authentication is used, enhancing security."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\CredentialsDelegation"; 
        ValueName = "ConcatenateDefaults_AllowDefNTLMOnly"; 
        ExpectedValue = 0; 
        Description = "Disables concatenation of default credentials when NTLM is used."; 
        ExpectedValueReason = "Setting this to 0 ensures default credentials are not concatenated during NTLM authentication, improving security."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\CredentialsDelegation"; 
        ValueName = "AllowDefCredentialsWhenNTLMOnly\1"; 
        ExpectedValue = ""; 
        Description = "Specifies no default credentials delegation when NTLM is used."; 
        ExpectedValueReason = "Leaving this empty ensures no delegation occurs during NTLM authentication."
    },
    @{
        Key = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\CredentialsDelegation"; 
        ValueName = "AllowDefaultCredentials\1"; 
        ExpectedValue = ""; 
        Description = "Specifies no delegation of default credentials."; 
        ExpectedValueReason = "Leaving this empty ensures no default credentials are delegated, reducing potential exposure."
    },
    @{
        Key = "HKLM:\Software\Microsoft\PolicyManager\default\WiFi"; 
        ValueName = "AllowAutoConnectToWiFiSenseHotspots\value"; 
        ExpectedValue = 0; 
        Description = "Disables automatic connection to WiFi Sense hotspots."; 
        ExpectedValueReason = "Setting this to 0 ensures no automatic connections occur, improving network security."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS"; 
        ValueName = "BackupDirectory"; 
        ExpectedValue = 2; 
        Description = "Specifies the backup directory for LAPS credentials."; 
        ExpectedValueReason = "Setting this to 2 ensures backups are securely stored in a designated directory."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS"; 
        ValueName = "PasswordAgeDays"; 
        ExpectedValue = 30; 
        Description = "Defines the maximum password age for LAPS."; 
        ExpectedValueReason = "Setting this to 30 ensures passwords are rotated regularly, improving security."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS"; 
        ValueName = "PasswordLength"; 
        ExpectedValue = 20; 
        Description = "Specifies the minimum password length for LAPS."; 
        ExpectedValueReason = "Setting this to 20 ensures passwords are sufficiently strong to resist brute-force attacks."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS"; 
        ValueName = "PasswordComplexity"; 
        ExpectedValue = 4; 
        Description = "Defines the complexity requirements for LAPS passwords."; 
        ExpectedValueReason = "Setting this to 4 ensures passwords include a mix of character types, enhancing strength."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS"; 
        ValueName = "ADPasswordEncryptionEnabled"; 
        ExpectedValue = 0; 
        Description = "Disables encryption for Active Directory passwords in LAPS."; 
        ExpectedValueReason = "Setting this to 0 ensures passwords are stored securely without unnecessary encryption layers."
    }
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS"; 
        ValueName = "ADBackupDSRMPassword"; 
        ExpectedValue = 1; 
        Description = "Enables backup of the DSRM password in LAPS."; 
        ExpectedValueReason = "Setting this to 1 ensures that the DSRM password is securely backed up, improving recoverability."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS"; 
        ValueName = "PostAuthenticationResetDelay"; 
        ExpectedValue = 6; 
        Description = "Defines the delay in minutes for resetting LAPS passwords after authentication."; 
        ExpectedValueReason = "Setting this to 6 ensures a secure delay for password reset post-authentication."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS"; 
        ValueName = "PostAuthenticationActions"; 
        ExpectedValue = 3; 
        Description = "Specifies actions to be taken post-authentication in LAPS."; 
        ExpectedValueReason = "Setting this to 3 ensures appropriate security measures are enforced post-authentication."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\LAPS\Config"; 
        ValueName = "BackupDirectory"; 
        ExpectedValue = 2; 
        Description = "Specifies the backup directory for LAPS credentials."; 
        ExpectedValueReason = "Setting this to 2 ensures backups are securely stored in a designated directory."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\LAPS\Config"; 
        ValueName = "PasswordAgeDays"; 
        ExpectedValue = 30; 
        Description = "Defines the maximum password age for LAPS."; 
        ExpectedValueReason = "Setting this to 30 ensures passwords are rotated regularly, improving security."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\LAPS\Config"; 
        ValueName = "PasswordLength"; 
        ExpectedValue = 20; 
        Description = "Specifies the minimum password length for LAPS."; 
        ExpectedValueReason = "Setting this to 20 ensures passwords are sufficiently strong to resist brute-force attacks."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\LAPS\Config"; 
        ValueName = "PasswordComplexity"; 
        ExpectedValue = 4; 
        Description = "Defines the complexity requirements for LAPS passwords."; 
        ExpectedValueReason = "Setting this to 4 ensures passwords include a mix of character types, enhancing strength."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\LAPS\Config"; 
        ValueName = "ADPasswordEncryptionEnabled"; 
        ExpectedValue = 0; 
        Description = "Disables encryption for Active Directory passwords in LAPS."; 
        ExpectedValueReason = "Setting this to 0 ensures passwords are stored securely without unnecessary encryption layers."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\LAPS\Config"; 
        ValueName = "ADBackupDSRMPassword"; 
        ExpectedValue = 1; 
        Description = "Enables backup of the DSRM password in LAPS."; 
        ExpectedValueReason = "Setting this to 1 ensures that the DSRM password is securely backed up, improving recoverability."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\LAPS\Config"; 
        ValueName = "PostAuthenticationResetDelay"; 
        ExpectedValue = 6; 
        Description = "Defines the delay in minutes for resetting LAPS passwords after authentication."; 
        ExpectedValueReason = "Setting this to 6 ensures a secure delay for password reset post-authentication."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows\CurrentVersion\LAPS\Config"; 
        ValueName = "PostAuthenticationActions"; 
        ExpectedValue = 3; 
        Description = "Specifies actions to be taken post-authentication in LAPS."; 
        ExpectedValueReason = "Setting this to 3 ensures appropriate security measures are enforced post-authentication."
    },
    @{
        Key = "HKLM:\Software\Policies\Microsoft Services\AdmPwd"; 
        ValueName = "AdmPwdEnabled"; 
        ExpectedValue = 1; 
        Description = "Enables the Local Administrator Password Solution (LAPS)."; 
        ExpectedValueReason = "Setting this to 1 ensures LAPS is enabled, providing unique and secure local administrator passwords."
    },
    @{
        Key = "HKLM:\Software\Policies\Microsoft Services\AdmPwd"; 
        ValueName = "PwdExpirationProtectionEnabled"; 
        ExpectedValue = 1; 
        Description = "Enables expiration protection for LAPS passwords."; 
        ExpectedValueReason = "Setting this to 1 ensures expired passwords are replaced promptly, maintaining security."
    },
    @{
        Key = "HKLM:\Software\Policies\Microsoft Services\AdmPwd"; 
        ValueName = "PasswordComplexity"; 
        ExpectedValue = 4; 
        Description = "Defines the complexity requirements for LAPS passwords."; 
        ExpectedValueReason = "Setting this to 4 ensures passwords include a mix of character types, enhancing strength."
    },
    @{
        Key = "HKLM:\Software\Policies\Microsoft Services\AdmPwd"; 
        ValueName = "PasswordLength"; 
        ExpectedValue = 20; 
        Description = "Specifies the minimum password length for LAPS."; 
        ExpectedValueReason = "Setting this to 20 ensures passwords are sufficiently strong to resist brute-force attacks."
    },
    @{
        Key = "HKLM:\Software\Policies\Microsoft Services\AdmPwd"; 
        ValueName = "PasswordAgeDays"; 
        ExpectedValue = 30; 
        Description = "Defines the maximum password age for LAPS."; 
        ExpectedValueReason = "Setting this to 30 ensures passwords are rotated regularly, improving security."
    },
    @{
        Key = "HKLM:\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\GPExtensions\{D76B9641-3288-4f75-942D-087DE603E3EA}"; 
        ValueName = "ExtensionDebugLevel"; 
        ExpectedValue = 2; 
        Description = "Enables debug logging for Winlogon Group Policy Extensions."; 
        ExpectedValueReason = "Setting this to 2 ensures detailed logging for troubleshooting Group Policy issues."
    }
   
)

# Retrieve the hostname of the current machine
$HostName = $env:COMPUTERNAME

# Initialize an array to store results
$Results = @()

# Function to check a local registry key
function Get-RegistryKeyInfo {
    param (
        [string]$KeyPath,
        [string]$ValueName,
        [object]$ExpectedValue,
        [string]$Description,
        [string]$ExpectedValueReason
    )
    try {
        # Read the registry key value
        $Value = Get-ItemProperty -Path $KeyPath -Name $ValueName -ErrorAction Stop
        $CurrentValue = $Value.$ValueName
        $Compliant = ($CurrentValue -eq $ExpectedValue)

        # Return key information as a custom object
        return [PSCustomObject]@{
            hostname            = $HostName
            key                 = $KeyPath
            keyname             = $ValueName
            value               = $CurrentValue
            expected            = $ExpectedValue
            compliant           = if ($Compliant) { "TRUE" } else { "FALSE" }
            description         = $Description
            ExpectedValueReason = $ExpectedValueReason
        }
    }
    catch {
        # Handle errors and return default values
        return [PSCustomObject]@{
            hostname            = $HostName
            key                 = $KeyPath
            keyname             = $ValueName
            value               = "undefined"
            expected            = $ExpectedValue
            compliant           = "FALSE"
            description         = $Description
            ExpectedValueReason = $ExpectedValueReason
        }
    }
}

# Iterate over each registry key and collect results
foreach ($Key in $RegistryKeys) {
    $Result = Get-RegistryKeyInfo -KeyPath $Key.Key -ValueName $Key.ValueName -ExpectedValue $Key.ExpectedValue -Description $Key.Description -ExpectedValueReason $Key.ExpectedValueReason
    $Results += $Result
}

# Calculate compliance statistics
$TotalEntries = $Results.Count
$TotalCompliant = ($Results | Where-Object { $_.compliant -eq "TRUE" }).Count
$TotalNonCompliant = ($Results | Where-Object { $_.compliant -eq "FALSE" }).Count
$ComplianceRate = [math]::Round(($TotalCompliant / $TotalEntries) * 100, 2)

# Export the results to the CSV file
$Results | Export-Csv -Path $OutputCsvPath -NoTypeInformation -Encoding UTF8

# Modern HTML Style with conditional coloring and compliance statistics
$HtmlStyle = @"
<style>
    body {
        font-family: Arial, sans-serif;
        margin: 20px;
        background-color: #f8f9fa;
        color: #343a40;
    }
    h1 {
        text-align: center;
        color: #007bff;
    }
    table {
        width: 100%;
        border-collapse: collapse;
        margin: 20px 0;
        font-size: 14px;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    }
    th, td {
        padding: 12px 15px;
        border: 1px solid #dee2e6;
        text-align: left;
    }
    th {
        background-color: #007bff;
        color: #ffffff;
    }
    tr.true {
        background-color: #d4edda;
        color: #155724;
    }
    tr.false {
        background-color: #f8d7da;
        color: #721c24;
    }
    footer {
        text-align: center;
        margin-top: 20px;
        font-size: 12px;
        color: #6c757d;
    }
    .stats {
        margin: 20px 0;
        font-size: 16px;
        text-align: center;
    }
</style>
"@

# Generate HTML content with compliance statistics
$HtmlContent = @"
<!DOCTYPE html>
<html>
<head>
    <title>Registry Audit Results</title>
    $HtmlStyle
</head>
<body>
<h1>Registry Audit Results</h1>
<div class='stats'>
    <p><strong>Total Entries:</strong> $TotalEntries</p>
    <p><strong>Compliant (TRUE):</strong> $TotalCompliant</p>
    <p><strong>Non-Compliant (FALSE):</strong> $TotalNonCompliant</p>
    <p><strong>Compliance Rate:</strong> $ComplianceRate%</p>
</div>
<table>
<tr>
    <th>Hostname</th>
    <th>Key</th>
    <th>Keyname</th>
    <th>Value</th>
    <th>Expected</th>
    <th>Compliant</th>
    <th>Description</th>
    <th>Expected Value Reason</th>
</tr>
"@

# Add rows to the HTML table
foreach ($Result in $Results) {
    $RowClass = if ($Result.compliant -eq "TRUE") { "true" } else { "false" }
    $HtmlContent += "<tr class='$RowClass'><td>$($Result.hostname)</td><td>$($Result.key)</td><td>$($Result.keyname)</td><td>$($Result.value)</td><td>$($Result.expected)</td><td>$($Result.compliant)</td><td>$($Result.description)</td><td>$($Result.ExpectedValueReason)</td></tr>"
}

$HtmlContent += "</table><footer>Generated by PowerShell Registry Audit Script</footer></body></html>"

# Write the HTML content to file
$HtmlContent | Out-File -FilePath $OutputHtmlPath -Encoding UTF8

Write-Output "Audit completed. Results exported to: $OutputCsvPath and $OutputHtmlPath"

```
{% endcode %}

### What Does This Script Do?

#### Key Functions

1. **Scans and Verifies Registry Keys**:
   * Retrieves the current value of each configured registry key.
   * Compares it against the **expected value** defined in the script.
2. **Calculates Compliance Metrics**:
   * Counts the total number of keys checked.
   * Calculates the number of compliant vs. non-compliant keys.
   * Provides a compliance rate in percentage.
3. **Generates Reports**:
   * **CSV File**: A detailed, tabular report for data analysis.
   * **HTML Report**: A visually appealing, color-coded report highlighting compliant and non-compliant keys.
4. **Supports Security Best Practices**:
   * Ensures critical security configurations (e.g., disabling WDigest or SMBv1) are enforced.

{% hint style="success" %}
By regularly running this script, administrators can ensure that systems are secure, compliant, and aligned with organizational policies.
{% endhint %}

***

## Step-by-Step Implementation

### 1. Deploy the Script

To deploy the script, follow these steps:

#### Create a GPO to Deploy a Scheduled Task

1. Open the **Group Policy Management Console (GPMC)**.
2. Create a new GPO (e.g., `RegistryAuditDeployment`) or edit an existing one.
3.  Navigate to:

    ```
    Computer Configuration > Preferences > Control Panel Settings > Scheduled Tasks
    ```
4. Create a **new immediate scheduled task** with the following settings:
   * **Action**: Create
   * **Name**: `Registry Audit Task`
   * **Run as user**: `S-1-5-18` (Local System account)
   * **Action type**: Start a Program
   * **Program/script**: Path to the PowerShell executable (e.g., `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`).
   *   **Arguments**: Add the path to the script, e.g.:

       ```powershell
       powershell -exec bypass -nop -command "cat file.ps1 | out-string | iex"
       ```
   * **Trigger**: Set the frequency (e.g., daily or as needed).

{% hint style="info" %}
By configuring the scheduled task to run under the **Local System account (S-1-5-18)**, you ensure:

* The task has sufficient privileges to access and audit registry keys.
* There’s no dependency on user credentials, avoiding failures if users log out or change passwords.
{% endhint %}

#### Place the GPO at the Root Level

1. Link the GPO at the **root of your domain** (e.g., `domain.com`).
2. Set the **GPO to "Enforced"** to ensure it applies across all Organizational Units (OUs).

#### Scope of Application

Add **Authenticated Users** to the GPO's security filtering to ensure it applies to all devices in your environment.

***

### 2. Set Up Log Directory

1. Choose a **shared folder** accessible by all machines (e.g., `SYSVOL`).

{% hint style="warning" %}
Always enforce strict permissions on the shared folder to prevent unauthorized access or tampering with logs.
{% endhint %}

#### 3. **Merge and Analyze Logs**

*   After all machines have uploaded their CSV files, follow these steps:

    1. **Merge the files** into a single CSV file.
    2. Use [**Timeline Explorer**](https://ericzimmerman.github.io/#!index.md) to review and browse the data.

    Timeline Explorer is a powerful tool for analyzing time-based data, providing an intuitive interface to explore your merged audit logs.

#### 4. **Search for Issues**

* Use [**Ripgrep**](https://github.com/BurntSushi/ripgrep) to search for specific patterns in the merged logs.
* Example commands:
  *   Search for all non-compliant keys:

      ```bash
      rg -i "compliant: false" merged_audit.csv
      ```
  *   Search for a specific registry key:

      ```bash
      rg -i "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa" merged_audit.csv
      ```
