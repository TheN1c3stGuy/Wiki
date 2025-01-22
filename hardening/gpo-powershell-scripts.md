---
hidden: true
---

# GPO PowerShell Scripts

<pre class="language-powershell"><code class="lang-powershell"><strong>###########################################################################################
</strong># [1mm0rt41][__main__] GlobalScript
###########################################################################################
$IPForInternet=@('1.0.0.0-9.255.255.255',
'11.0.0.0-100.63.255.255',
'100.128.0.0-126.255.255.255',
'128.0.0.0-169.253.255.255',
'169.255.0.0-172.15.255.255',
'172.32.0.0-191.255.255.255',
'192.0.1.0-192.0.1.255',
'192.0.3.0-192.167.255.255',
'192.169.0.0-198.17.255.255',
'198.20.0.0-198.51.99.255',
'198.51.101.0-203.0.112.255',
'203.0.114.0-255.255.255.254')



###########################################################################################
# [1mm0rt41][DomainController_Config] CreateOU
###########################################################################################
$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value) = ( [ADSI]"LDAP://RootDSE" ).defaultNamingContext.Value
$baseDN = 'Corp.lo'
New-ADOrganizationalUnit -Name "$baseDN"                             -Path "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Users"                               -Path "OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Servers"                             -Path "OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Workstations"                        -Path "OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Groups"                              -Path "OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
#                                                                    
New-ADOrganizationalUnit -Name "Admin"                               -Path "OU=Users,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Service"                             -Path "OU=Users,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "External"                            -Path "OU=Users,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Internal"                            -Path "OU=Users,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
#
New-ADOrganizationalUnit -Name "Tier0-CoreSystem"                    -Path "OU=Admin,OU=Users,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Tier1-ServerBusinessCritical"        -Path "OU=Admin,OU=Users,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Tier2-ServerLambda"                  -Path "OU=Admin,OU=Users,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Tier3-WorkstationBusinessCritical"   -Path "OU=Admin,OU=Users,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Tier4-WorkstationLambda"             -Path "OU=Admin,OU=Users,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "External"                            -Path "OU=Users,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Internal"                            -Path "OU=Users,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
#
New-ADOrganizationalUnit -Name "Tier0-CoreSystem"                    -Path "OU=Servers,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Tier1-ServerBusinessCritical"        -Path "OU=Servers,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Tier2-ServerLambda"                  -Path "OU=Servers,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Tier3-WorkstationBusinessCritical"   -Path "OU=Workstations,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Tier4-WorkstationLambda"             -Path "OU=Workstations,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
#
New-ADOrganizationalUnit -Name "Tier0-CoreSystem"                    -Path "OU=Groups,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Tier1-ServerBusinessCritical"        -Path "OU=Groups,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Tier2-ServerLambda"                  -Path "OU=Groups,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Tier3-WorkstationBusinessCritical"   -Path "OU=Groups,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Tier4-WorkstationLambda"             -Path "OU=Groups,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
#
New-ADOrganizationalUnit -Name "Tier3-UserBusinessCritical"          -Path "OU=Internal,OU=Users,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Tier4-UserLambda"                    -Path "OU=Internal,OU=Users,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
#
New-ADOrganizationalUnit -Name "Role-Admins"                         -Path "OU=Groups,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "Role-Computers"                      -Path "OU=Groups,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADOrganizationalUnit -Name "TierSystem"                          -Path "OU=Role-Computers,OU=Groups,OU=$baseDN,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"

#redircmp "OU=_AllComputers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
#redirusr "OU=_AllUsers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"



###########################################################################################
# [1mm0rt41][DomainController_Config] CreateBasicGroups
###########################################################################################
New-ADGroup -Name PRIV_DBA_ADMIN -Path "OU=_CriticalUsers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -GroupCategory Security -GroupScope DomainLocal
New-ADGroup -Name PRIV_INTERACT_LAPTOP -Path "OU=_AllUsers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -GroupCategory Security -GroupScope DomainLocal
New-ADGroup -Name PRIV_INTERACT_WORKSTATION -Path "OU=_AllUsers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -GroupCategory Security -GroupScope DomainLocal
New-ADGroup -Name PRIV_LOCAL_ADM -Path "OU=_CriticalUsers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -GroupCategory Security -GroupScope DomainLocal
New-ADGroup -Name PRIV_ENROLL_MACHINE -Path "OU=_CriticalUsers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -GroupCategory Security -GroupScope DomainLocal



###########################################################################################
# [1mm0rt41][DomainController_Config] Enable the Recycle Bin
###########################################################################################
Import-Module ActiveDirectory
Enable-ADOptionalFeature -Identity "CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -Scope ForestOrConfigurationSet -Target "$domain" -Confirm:$false



###########################################################################################
# [1mm0rt41][DomainController_Config] Install 'BitLocker recovery tab'
###########################################################################################
Install-WindowsFeature RSAT-Feature-Tools-BitLocker-BdeAducExt



###########################################################################################
# [1mm0rt41][DomainController_Config] Password policy
###########################################################################################
$Policies= @{
    Identity=$env:UserDomain;
    LockoutDuration='00:30:00';
    LockoutThreshold=5;
    LockoutObservationWindow='00:20:00';
    ComplexityEnabled=$true;
    ReversibleEncryptionEnabled=$False;
    MaxPasswordAge='180.00:00:00';
    MinPasswordLength=15;
    PasswordHistoryCount=10;
}
Set-ADDefaultDomainPasswordPolicy @Policies



###########################################################################################
# [1mm0rt41][DomainController_Config] Enable-Advenced-AD-Password-Protection
###########################################################################################
# https://blog.lithnet.io/2019/01/lppad-1.html
curl.exe -L https://github.com/lithnet/ad-password-protection/releases/latest/download/Lithnet.ActiveDirectory.PasswordProtection.msi --output Lithnet.ActiveDirectory.PasswordProtection.msi
/quiet /qn ALLUSERS=2 
msiexec.exe /i Lithnet.ActiveDirectory.PasswordProtection.msi /quiet /qn /norestart REBOOT=ReallySuppress ALLUSERS=2 
# Download the latest version of the NTLM passwords from the haveibeenpwned.com pwned password list (scroll to the end). Make sure you get the "NTLM Ordered by hash" version. Use the torrent link if you are able to so, as this helps minimize bandwidth and costs. Uncompress the file, and place it on your server to import later in the process.
curl.exe -L https://downloads.pwnedpasswords.com/passwords/pwned-passwords-ntlm-ordered-by-hash-v8.7z --output hash.7z
7z x hash.7z
Import-Module LithnetPasswordProtection
Open-Store 'C:\Program Files\Lithnet\Active Directory Password Protection\Store'
Import-CompromisedPasswordHashes -Filename hash\pwned-passwords-ntlm-ordered-by-hash-*.txt



###########################################################################################
# [1mm0rt41][DomainController_Config] Split DC FSMO role
###########################################################################################
Write-Host "Doc: https://activedirectorypro.com/transfer-fsmo-roles/"
$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value) = ( [ADSI]"LDAP://RootDSE" ).defaultNamingContext.Value
Write-Warning "Current FSMO"
netdom query fsmo
$dc = Get-ADComputer -Filter * -SearchBase "OU=Domain Controllers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" | where { $_.Enabled -eq $true }
if( $dc.Count -gt 1 ){
    Move-ADDirectoryServerOperationMasterRole -Identity $dc[(0)%$dc.Count].Name PDCEmulator
    Move-ADDirectoryServerOperationMasterRole -Identity $dc[(1)%$dc.Count].Name RIDMaster
    Move-ADDirectoryServerOperationMasterRole -Identity $dc[(2)%$dc.Count].Name Infrastructuremaster
    Move-ADDirectoryServerOperationMasterRole -Identity $dc[(3)%$dc.Count].Name DomainNamingmaster
    Move-ADDirectoryServerOperationMasterRole -Identity $dc[(4)%$dc.Count].Name SchemaMaster
    Write-Warning "New FSMO"
    netdom query fsmo
}else{
    Write-Warning "Unable to change FSMO ! Not Enough server"
}



###########################################################################################
# [1mm0rt41][DomainController_Config](GPO,Computer&#x26;User) Set timezone to W. Europe Standard Time
###########################################################################################
New-GPO -Name "[1mm0rt41][DomainController_Config](GPO,Computer&#x26;User) Set timezone to W. Europe Standard Time" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
tzutil /s 'W. Europe Standard Time'
reg add "HKCU\Control Panel\International" /v sLongDate /d "dddd, MMMM d, yyyy" /t REG_SZ /f
reg add "HKCU\Control Panel\International" /v sShortDate /d "MM/dd/yyyy" /t REG_SZ /f
reg add "HKCU\Control Panel\International" /v sShortTime /d "HH:mm" /t REG_SZ /f
reg add "HKCU\Control Panel\International" /v sTimeFormat /d "HH:mm:ss" /t REG_SZ /f
reg add "HKCU\Control Panel\International" /v sYearMonth /d "MMMM yyyy" /t REG_SZ /f
reg add "HKCU\Control Panel\International" /v iFirstDayOfWeek /d "0" /t REG_SZ /f

reg add "HKEY_USERS\.DEFAULT\Control Panel\International" /v sLongDate /d "dddd, MMMM d, yyyy" /t REG_SZ /f
reg add "HKEY_USERS\.DEFAULT\Control Panel\International" /v sShortDate /d "MM/dd/yyyy" /t REG_SZ /f
reg add "HKEY_USERS\.DEFAULT\Control Panel\International" /v sShortTime /d "HH:mm" /t REG_SZ /f
reg add "HKEY_USERS\.DEFAULT\Control Panel\International" /v sTimeFormat /d "HH:mm:ss" /t REG_SZ /f
reg add "HKEY_USERS\.DEFAULT\Control Panel\International" /v sYearMonth /d "MMMM yyyy" /t REG_SZ /f
reg add "HKEY_USERS\.DEFAULT\Control Panel\International" /v iFirstDayOfWeek /d "0" /t REG_SZ /f
	$_ | Set-GPRegistryValue -Key "HKCU\Control Panel\International" -ValueName "sLongDate" -Value "dddd, MMMM d, yyyy" -Type ExpandString >$null
	$_ | Set-GPRegistryValue -Key "HKCU\Control Panel\International" -ValueName "sShortDate" -Value "MM/dd/yyyy" -Type ExpandString >$null
	$_ | Set-GPRegistryValue -Key "HKCU\Control Panel\International" -ValueName "sShortTime" -Value "HH:mm" -Type ExpandString >$null
	$_ | Set-GPRegistryValue -Key "HKCU\Control Panel\International" -ValueName "sTimeFormat" -Value "HH:mm:ss" -Type ExpandString >$null
	$_ | Set-GPRegistryValue -Key "HKCU\Control Panel\International" -ValueName "sYearMonth" -Value "MMMM yyyy" -Type ExpandString >$null
	$_ | Set-GPRegistryValue -Key "HKCU\Control Panel\International" -ValueName "iFirstDayOfWeek" -Value "0" -Type ExpandString >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Control Panel\International" -ValueName "sLongDate" -Value "dddd, MMMM d, yyyy" -Type ExpandString >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Control Panel\International" -ValueName "sShortDate" -Value "MM/dd/yyyy" -Type ExpandString >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Control Panel\International" -ValueName "sShortTime" -Value "HH:mm" -Type ExpandString >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Control Panel\International" -ValueName "sTimeFormat" -Value "HH:mm:ss" -Type ExpandString >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Control Panel\International" -ValueName "sYearMonth" -Value "MMMM yyyy" -Type ExpandString >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Control Panel\International" -ValueName "iFirstDayOfWeek" -Value "0" -Type ExpandString >$null
	$_
}


###########################################################################################
# [1mm0rt41][DomainController_Config](GPO,Computer) Force DC to switch on Domain Profile Firewall
###########################################################################################
New-GPO -Name "[1mm0rt41][DomainController_Config](GPO,Computer) Force DC to switch on Domain Profile Firewall" -Comment "##################################`r`n`r`nTo force firewall profile to domain for DC" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\nlasvc" -ValueName "Start" -Value 2 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\nlasvc" -ValueName "DelayedAutostart" -Value 1 -Type DWord >$null
	$_
} | New-GPLink -target "OU=Domain Controllers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][DomainController_Config] PingCastle-fix-PreWin2000Other
###########################################################################################
# SamAccountName	: AccÃ¨s compatible prÃ©-Windows 2000
# SID			   : S-1-5-32-554
$preWin200 = Get-ADGroup -Filter * | where { $_.SID -eq 'S-1-5-32-554' }
$preWin200 | get-ADGroupMember | foreach { $preWin200 | Remove-ADGroupMember -Members $_.SamAccountName }



###########################################################################################
# [1mm0rt41][DomainController_Config] Block PetitPotam
###########################################################################################
$rr = (netsh rpc filter show filter).Replace(' ','') ;
if( -not ($rr -Like "*c681d488*" -Or $rr -Like "*df1941c5*") ){
    @'
rpc
filter
add rule layer=um actiontype=permit
add condition field=if_uuid matchtype=equal data=c681d488-d850-11d0-8c52-00c04fd90f7e
add condition field=remote_user_token matchtype=equal data=D:(A;;CC;;;DA)
add filter
add rule layer=um actiontype=block
add condition field=if_uuid matchtype=equal data=c681d488-d850-11d0-8c52-00c04fd90f7e
add filter
add rule layer=um actiontype=permit
add condition field=if_uuid matchtype=equal data=df1941c5-fe89-4e79-bf10-463657acf44d
add condition field=remote_user_token matchtype=equal data=D:(A;;CC;;;DA)
add filter
add rule layer=um actiontype=block
add condition field=if_uuid matchtype=equal data=df1941c5-fe89-4e79-bf10-463657acf44d
add filter
quit
'@ | Out-File -Encoding ASCII C:\Windows\Temp\rr.txt
    netsh -f C:\Windows\Temp\rr.txt
    write-Host 'Patching'
}
write-Host 'Patched'



###########################################################################################
# [1mm0rt41][DomainController_Config] PingCastle(A-RootDseAnonBinding) - RootDse Anonymous Binding is disabled
###########################################################################################
Set-ADObject -Identity "CN=Directory Service,CN=Windows NT,CN=Services,$(([ADSI]'LDAP://RootDSE').configurationNamingContext.Value)" -Add @{'msDS-Other-Settings'="DenyUnauthenticatedBind=1"}



###########################################################################################
# [1mm0rt41][Guardian] Admin account cannot be delegated
###########################################################################################
New-GPO -Name "[1mm0rt41][Guardian] Admin account cannot be delegated" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$gpoDir="$gpoBasePath\Machine\Scripts";
	mkdir "$gpoDir" >$null
	@'
$UID__DOMAIN = (Get-ADDomain).DomainSID.Value
# DomainAdmin=512, EnterpriseAdmin=519, AccountOperator=548, BackupOperator=551, PrintOperator=550, Replicator=552, SchemaAdministrators=518
# Admin account cannot be delegated
@(512,519,548,551,550,552,518,'DnsAdmins') | foreach {
    $grp = Get-ADGroup -Filter "SID -eq '$UID__DOMAIN-$_' -or SID -eq 'S-1-5-32-$_' -or samAccountName -eq '$_'"
    Write-Host "Set account cannot be delegated in >$($grp.Name)&#x3C;"
    $target = $grp | Get-ADGroupMember -Recursive | Get-ADUser -Properties AccountNotDelegated | Where-Object {-not $_.AccountNotDelegated -and $_.objectClass -eq 'user'}
    $target | Select distinguishedName,SamAccountName,AccountNotDelegated
    $target | Set-ADUser -AccountNotDelegated $true
}

'@ | Out-File -Encoding Ascii "$gpoDir\AdminAccountCannotBeDelegated.ps1" 
	$gpoDir="$gpoBasePath\Machine\Preferences\ScheduledTasks";
	mkdir "$gpoDir" >$null
	$taskXml = ( @"
&#x3C;?xml version="1.0" encoding="utf-8"?>
&#x3C;ScheduledTasks clsid="{CC63F200-7309-4ba0-B154-A71CD118DBCC}">&#x3C;TaskV2 clsid="{D8896631-B747-47a7-84A6-C155337F3BC8}" name="[GPO] AdminAccountCannotBeDelegated" image="0" changed="$((Get-Date).ToString("yyyy-MM-dd HH:mm:ss"))" uid="{D98A502B-7563-4A3D-A4EA-5B4EE8E63364}" >&#x3C;Properties action="R" name="[GPO] AdminAccountCannotBeDelegated" runAs="S-1-5-18" logonType="S4U">&#x3C;Task version="1.2">&#x3C;RegistrationInfo>&#x3C;Author>$($env:USERDOMAIN)\$($env:USERNAME)&#x3C;/Author>&#x3C;Description>&#x3C;![CDATA[This task need to run with S-1-5-18 // GPO Id: $gpoId // GPO Name: $gpoName]]>&#x3C;/Description>&#x3C;/RegistrationInfo>&#x3C;Principals>&#x3C;Principal id="Author">&#x3C;UserId>S-1-5-18&#x3C;/UserId>&#x3C;LogonType>S4U&#x3C;/LogonType>&#x3C;RunLevel>HighestAvailable&#x3C;/RunLevel>&#x3C;/Principal>&#x3C;/Principals>&#x3C;Settings>&#x3C;IdleSettings>&#x3C;Duration>PT5M&#x3C;/Duration>&#x3C;WaitTimeout>PT1H&#x3C;/WaitTimeout>&#x3C;StopOnIdleEnd>false&#x3C;/StopOnIdleEnd>&#x3C;RestartOnIdle>false&#x3C;/RestartOnIdle>&#x3C;/IdleSettings>&#x3C;MultipleInstancesPolicy>IgnoreNew&#x3C;/MultipleInstancesPolicy>&#x3C;DisallowStartIfOnBatteries>false&#x3C;/DisallowStartIfOnBatteries>&#x3C;StopIfGoingOnBatteries>false&#x3C;/StopIfGoingOnBatteries>&#x3C;AllowHardTerminate>true&#x3C;/AllowHardTerminate>&#x3C;StartWhenAvailable>true&#x3C;/StartWhenAvailable>&#x3C;AllowStartOnDemand>true&#x3C;/AllowStartOnDemand>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;Hidden>false&#x3C;/Hidden>&#x3C;ExecutionTimeLimit>PT1H&#x3C;/ExecutionTimeLimit>&#x3C;Priority>7&#x3C;/Priority>&#x3C;RestartOnFailure>&#x3C;Interval>PT10M&#x3C;/Interval>&#x3C;Count>3&#x3C;/Count>&#x3C;/RestartOnFailure>&#x3C;RunOnlyIfNetworkAvailable>false&#x3C;/RunOnlyIfNetworkAvailable>&#x3C;/Settings>&#x3C;Actions Context="Author">&#x3C;Exec>&#x3C;Command>powershell&#x3C;/Command>&#x3C;Arguments>-exec bypass -nop -File "%MachineScript%\AdminAccountCannotBeDelegated.ps1"&#x3C;/Arguments>&#x3C;/Exec>&#x3C;/Actions>&#x3C;Triggers>&#x3C;CalendarTrigger>&#x3C;StartBoundary>$((Get-Date).AddDays(1).ToString("yyyy-MM-ddT{0:d2}:00:00" -f 9))&#x3C;/StartBoundary>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;ScheduleByDay>&#x3C;DaysInterval>1&#x3C;/DaysInterval>&#x3C;/ScheduleByDay>&#x3C;RandomDelay>PT15M&#x3C;/RandomDelay>&#x3C;/CalendarTrigger>&#x3C;/Triggers>&#x3C;/Task>&#x3C;/Properties>&#x3C;/TaskV2>
&#x3C;/ScheduledTasks>
"@ ).Trim()
	$taskXml = $taskXml.Replace("%MachineScriptUNC%","\\$($env:USERDNSDOMAIN)\SYSVOL\$($env:USERDNSDOMAIN)\Policies\$gpoId\Machine\Scripts");
	$taskXml = $taskXml.Replace("%MachineScript%","$gpoBasePath\Machine\Scripts");
	$taskXml | Out-File -Encoding ASCII "$gpoDir\ScheduledTasks.xml"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{00000000-0000-0000-0000-000000000000}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}][{AADCED64-746C-4633-A97C-D61349046527}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}]"};
	$_
}


###########################################################################################
# [1mm0rt41][Guardian] DomainBackup
###########################################################################################
New-GPO -Name "[1mm0rt41][Guardian] DomainBackup" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$gpoDir="$gpoBasePath\Machine\Scripts";
	mkdir "$gpoDir" >$null
	@'
mkdir -force C:\Backup
Start-Transcript -Path "C:\Backup\$((Get-Date).ToString('yyyyMMddHH')).txt" -Force
ntdsutil "activate instance ntds" ifm "create full C:\Backup\AD-new" quit quit
try{
    if( (Get-Item 'C:\Backup\AD\Active Directory\ntds.dit').Length -gt 1 ){
        rm -Recurse -ErrorAction SilentlyContinue -Force C:\Backup\AD
        mv C:\Backup\AD-new C:\Backup\AD
    }
}catch{}
Stop-Transcript

'@ | Out-File -Encoding Ascii "$gpoDir\BackupAD.ps1" 
	$gpoDir="$gpoBasePath\Machine\Preferences\ScheduledTasks";
	mkdir "$gpoDir" >$null
	$taskXml = ( @"
&#x3C;?xml version="1.0" encoding="utf-8"?>
&#x3C;ScheduledTasks clsid="{CC63F200-7309-4ba0-B154-A71CD118DBCC}">&#x3C;TaskV2 clsid="{D8896631-B747-47a7-84A6-C155337F3BC8}" name="[GPO] BackupAD" image="0" changed="$((Get-Date).ToString("yyyy-MM-dd HH:mm:ss"))" uid="{D98A502B-7563-4A3D-A4EA-5B4EE8E63364}" >&#x3C;Properties action="R" name="[GPO] BackupAD" runAs="S-1-5-18" logonType="S4U">&#x3C;Task version="1.2">&#x3C;RegistrationInfo>&#x3C;Author>$($env:USERDOMAIN)\$($env:USERNAME)&#x3C;/Author>&#x3C;Description>&#x3C;![CDATA[This task need to run with S-1-5-18 // GPO Id: $gpoId // GPO Name: $gpoName]]>&#x3C;/Description>&#x3C;/RegistrationInfo>&#x3C;Principals>&#x3C;Principal id="Author">&#x3C;UserId>S-1-5-18&#x3C;/UserId>&#x3C;LogonType>S4U&#x3C;/LogonType>&#x3C;RunLevel>HighestAvailable&#x3C;/RunLevel>&#x3C;/Principal>&#x3C;/Principals>&#x3C;Settings>&#x3C;IdleSettings>&#x3C;Duration>PT5M&#x3C;/Duration>&#x3C;WaitTimeout>PT1H&#x3C;/WaitTimeout>&#x3C;StopOnIdleEnd>false&#x3C;/StopOnIdleEnd>&#x3C;RestartOnIdle>false&#x3C;/RestartOnIdle>&#x3C;/IdleSettings>&#x3C;MultipleInstancesPolicy>IgnoreNew&#x3C;/MultipleInstancesPolicy>&#x3C;DisallowStartIfOnBatteries>false&#x3C;/DisallowStartIfOnBatteries>&#x3C;StopIfGoingOnBatteries>false&#x3C;/StopIfGoingOnBatteries>&#x3C;AllowHardTerminate>true&#x3C;/AllowHardTerminate>&#x3C;StartWhenAvailable>true&#x3C;/StartWhenAvailable>&#x3C;AllowStartOnDemand>true&#x3C;/AllowStartOnDemand>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;Hidden>false&#x3C;/Hidden>&#x3C;ExecutionTimeLimit>PT1H&#x3C;/ExecutionTimeLimit>&#x3C;Priority>7&#x3C;/Priority>&#x3C;RestartOnFailure>&#x3C;Interval>PT10M&#x3C;/Interval>&#x3C;Count>3&#x3C;/Count>&#x3C;/RestartOnFailure>&#x3C;RunOnlyIfNetworkAvailable>false&#x3C;/RunOnlyIfNetworkAvailable>&#x3C;/Settings>&#x3C;Actions Context="Author">&#x3C;Exec>&#x3C;Command>powershell&#x3C;/Command>&#x3C;Arguments>-exec bypass -nop -File "%MachineScript%\BackupAD.ps1"&#x3C;/Arguments>&#x3C;/Exec>&#x3C;/Actions>&#x3C;Triggers>&#x3C;CalendarTrigger>&#x3C;StartBoundary>$((Get-Date).AddDays(1).ToString("yyyy-MM-ddT{0:d2}:00:00" -f 1))&#x3C;/StartBoundary>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;ScheduleByDay>&#x3C;DaysInterval>1&#x3C;/DaysInterval>&#x3C;/ScheduleByDay>&#x3C;RandomDelay>PT120M&#x3C;/RandomDelay>&#x3C;/CalendarTrigger>&#x3C;/Triggers>&#x3C;/Task>&#x3C;/Properties>&#x3C;/TaskV2>
&#x3C;/ScheduledTasks>
"@ ).Trim()
	$taskXml = $taskXml.Replace("%MachineScriptUNC%","\\$($env:USERDNSDOMAIN)\SYSVOL\$($env:USERDNSDOMAIN)\Policies\$gpoId\Machine\Scripts");
	$taskXml = $taskXml.Replace("%MachineScript%","$gpoBasePath\Machine\Scripts");
	$taskXml | Out-File -Encoding ASCII "$gpoDir\ScheduledTasks.xml"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{00000000-0000-0000-0000-000000000000}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}][{AADCED64-746C-4633-A97C-D61349046527}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}]"};
	$_
}


###########################################################################################
# [1mm0rt41][Guardian] Deny AuthenticatedUsers to create folders in C:
###########################################################################################
New-GPO -Name "[1mm0rt41][Guardian] Deny AuthenticatedUsers to create folders in C:" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$gpoDir="$gpoBasePath\Machine\Scripts";
	mkdir "$gpoDir" >$null
	@'
#icacls C:\ /remove:g "NT AUTHORITY\Utilisateurs authentifiÃ©s"
#icacls C:\ /remove:g "Utilisateurs authentifiÃ©s"
#icacls C:\ /remove:g "NT AUTHORITY\Authenticated Users"
#icacls C:\ /remove:g "Authenticated Users"
$root = Get-Acl C:\
$sddl = $root.Sddl
$sddl = $sddl.Replace('(A;CIIO;DC;;;BU)','')
$sddl = $sddl.Replace('(A;CI;LC;;;BU)','')
$sddl = $sddl.Replace('(A;;LC;;;AU)','')
$sddl = $sddl.Replace('(A;OICIIO;SDGXGWGR;;;AU)','(A;OICIIO;GXGR;;;AU)')
$root.SetSecurityDescriptorSddlForm($sddl)
Set-Acl C:\ $root

'@ | Out-File -Encoding Ascii "$gpoDir\DenyAuthenticatedUsersCreateFolderInC.ps1" 
	$gpoDir="$gpoBasePath\Machine\Preferences\ScheduledTasks";
	mkdir "$gpoDir" >$null
	$taskXml = ( @"
&#x3C;?xml version="1.0" encoding="utf-8"?>
&#x3C;ScheduledTasks clsid="{CC63F200-7309-4ba0-B154-A71CD118DBCC}">&#x3C;ImmediateTaskV2 clsid="{9756B581-76EC-4169-9AFC-0CA8D43ADB5F}" name="[GPO] DenyAuthenticatedUsersCreateFolderInC" image="0" changed="$((Get-Date).ToString("yyyy-MM-dd HH:mm:ss"))" uid="{D98A502B-7563-4A3D-A4EA-5B4EE8E63364}" userContext="0" removePolicy="0">&#x3C;Properties action="R" name="[GPO] DenyAuthenticatedUsersCreateFolderInC" runAs="S-1-5-18" logonType="S4U">&#x3C;Task version="1.2">&#x3C;RegistrationInfo>&#x3C;Author>$($env:USERDOMAIN)\$($env:USERNAME)&#x3C;/Author>&#x3C;Description>&#x3C;![CDATA[This task need to run with S-1-5-18 // GPO Id: $gpoId // GPO Name: $gpoName]]>&#x3C;/Description>&#x3C;/RegistrationInfo>&#x3C;Principals>&#x3C;Principal id="Author">&#x3C;UserId>S-1-5-18&#x3C;/UserId>&#x3C;LogonType>S4U&#x3C;/LogonType>&#x3C;RunLevel>HighestAvailable&#x3C;/RunLevel>&#x3C;/Principal>&#x3C;/Principals>&#x3C;Settings>&#x3C;IdleSettings>&#x3C;Duration>PT5M&#x3C;/Duration>&#x3C;WaitTimeout>PT1H&#x3C;/WaitTimeout>&#x3C;StopOnIdleEnd>false&#x3C;/StopOnIdleEnd>&#x3C;RestartOnIdle>false&#x3C;/RestartOnIdle>&#x3C;/IdleSettings>&#x3C;MultipleInstancesPolicy>IgnoreNew&#x3C;/MultipleInstancesPolicy>&#x3C;DisallowStartIfOnBatteries>true&#x3C;/DisallowStartIfOnBatteries>&#x3C;StopIfGoingOnBatteries>true&#x3C;/StopIfGoingOnBatteries>&#x3C;AllowHardTerminate>true&#x3C;/AllowHardTerminate>&#x3C;StartWhenAvailable>true&#x3C;/StartWhenAvailable>&#x3C;AllowStartOnDemand>true&#x3C;/AllowStartOnDemand>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;Hidden>false&#x3C;/Hidden>&#x3C;ExecutionTimeLimit>PT1H&#x3C;/ExecutionTimeLimit>&#x3C;Priority>7&#x3C;/Priority>&#x3C;DeleteExpiredTaskAfter>PT0S&#x3C;/DeleteExpiredTaskAfter>&#x3C;RestartOnFailure>&#x3C;Interval>PT10M&#x3C;/Interval>&#x3C;Count>3&#x3C;/Count>&#x3C;/RestartOnFailure>&#x3C;RunOnlyIfNetworkAvailable>true&#x3C;/RunOnlyIfNetworkAvailable>&#x3C;/Settings>&#x3C;Actions Context="Author">&#x3C;Exec>&#x3C;Command>powershell&#x3C;/Command>&#x3C;Arguments>-exec bypass -nop -File "%MachineScriptUNC%\DenyAuthenticatedUsersCreateFolderInC.ps1"&#x3C;/Arguments>&#x3C;/Exec>&#x3C;/Actions>&#x3C;Triggers>&#x3C;TimeTrigger>&#x3C;StartBoundary>%LocalTimeXmlEx%&#x3C;/StartBoundary>&#x3C;EndBoundary>%LocalTimeXmlEx%&#x3C;/EndBoundary>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;/TimeTrigger>&#x3C;/Triggers>&#x3C;/Task>&#x3C;/Properties>&#x3C;/ImmediateTaskV2>
&#x3C;/ScheduledTasks>
"@ ).Trim()
	$taskXml = $taskXml.Replace("%MachineScriptUNC%","\\$($env:USERDNSDOMAIN)\SYSVOL\$($env:USERDNSDOMAIN)\Policies\$gpoId\Machine\Scripts");
	$taskXml = $taskXml.Replace("%MachineScript%","$gpoBasePath\Machine\Scripts");
	$taskXml | Out-File -Encoding ASCII "$gpoDir\ScheduledTasks.xml"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{00000000-0000-0000-0000-000000000000}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}][{AADCED64-746C-4633-A97C-D61349046527}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}]"};
	$_
}


###########################################################################################
# [1mm0rt41][Guardian] Remove Powershellv2
###########################################################################################
New-GPO -Name "[1mm0rt41][Guardian] Remove Powershellv2" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$gpoDir="$gpoBasePath\Machine\Preferences\ScheduledTasks";
	mkdir "$gpoDir" >$null
	$taskXml = ( @"
&#x3C;?xml version="1.0" encoding="utf-8"?>
&#x3C;ScheduledTasks clsid="{CC63F200-7309-4ba0-B154-A71CD118DBCC}">&#x3C;ImmediateTaskV2 clsid="{9756B581-76EC-4169-9AFC-0CA8D43ADB5F}" name="[GPO] RemovePowershellv2" image="0" changed="$((Get-Date).ToString("yyyy-MM-dd HH:mm:ss"))" uid="{D98A502B-7563-4A3D-A4EA-5B4EE8E63364}" userContext="0" removePolicy="0">&#x3C;Properties action="R" name="[GPO] RemovePowershellv2" runAs="S-1-5-18" logonType="S4U">&#x3C;Task version="1.2">&#x3C;RegistrationInfo>&#x3C;Author>$($env:USERDOMAIN)\$($env:USERNAME)&#x3C;/Author>&#x3C;Description>&#x3C;![CDATA[This task need to run with S-1-5-18 // GPO Id: $gpoId // GPO Name: $gpoName]]>&#x3C;/Description>&#x3C;/RegistrationInfo>&#x3C;Principals>&#x3C;Principal id="Author">&#x3C;UserId>S-1-5-18&#x3C;/UserId>&#x3C;LogonType>S4U&#x3C;/LogonType>&#x3C;RunLevel>HighestAvailable&#x3C;/RunLevel>&#x3C;/Principal>&#x3C;/Principals>&#x3C;Settings>&#x3C;IdleSettings>&#x3C;Duration>PT5M&#x3C;/Duration>&#x3C;WaitTimeout>PT1H&#x3C;/WaitTimeout>&#x3C;StopOnIdleEnd>false&#x3C;/StopOnIdleEnd>&#x3C;RestartOnIdle>false&#x3C;/RestartOnIdle>&#x3C;/IdleSettings>&#x3C;MultipleInstancesPolicy>IgnoreNew&#x3C;/MultipleInstancesPolicy>&#x3C;DisallowStartIfOnBatteries>true&#x3C;/DisallowStartIfOnBatteries>&#x3C;StopIfGoingOnBatteries>true&#x3C;/StopIfGoingOnBatteries>&#x3C;AllowHardTerminate>true&#x3C;/AllowHardTerminate>&#x3C;StartWhenAvailable>true&#x3C;/StartWhenAvailable>&#x3C;AllowStartOnDemand>true&#x3C;/AllowStartOnDemand>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;Hidden>false&#x3C;/Hidden>&#x3C;ExecutionTimeLimit>PT1H&#x3C;/ExecutionTimeLimit>&#x3C;Priority>7&#x3C;/Priority>&#x3C;DeleteExpiredTaskAfter>PT0S&#x3C;/DeleteExpiredTaskAfter>&#x3C;RestartOnFailure>&#x3C;Interval>PT10M&#x3C;/Interval>&#x3C;Count>3&#x3C;/Count>&#x3C;/RestartOnFailure>&#x3C;RunOnlyIfNetworkAvailable>true&#x3C;/RunOnlyIfNetworkAvailable>&#x3C;/Settings>&#x3C;Actions Context="Author">&#x3C;Exec>&#x3C;Command>dism&#x3C;/Command>&#x3C;Arguments>/Online /Disable-Feature:MicrosoftWindowsPowerShellV2Root /NoRestart&#x3C;/Arguments>&#x3C;/Exec>&#x3C;/Actions>&#x3C;Triggers>&#x3C;TimeTrigger>&#x3C;StartBoundary>%LocalTimeXmlEx%&#x3C;/StartBoundary>&#x3C;EndBoundary>%LocalTimeXmlEx%&#x3C;/EndBoundary>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;/TimeTrigger>&#x3C;/Triggers>&#x3C;/Task>&#x3C;/Properties>&#x3C;/ImmediateTaskV2>
&#x3C;/ScheduledTasks>
"@ ).Trim()
	$taskXml = $taskXml.Replace("%MachineScriptUNC%","\\$($env:USERDNSDOMAIN)\SYSVOL\$($env:USERDNSDOMAIN)\Policies\$gpoId\Machine\Scripts");
	$taskXml = $taskXml.Replace("%MachineScript%","$gpoBasePath\Machine\Scripts");
	$taskXml | Out-File -Encoding ASCII "$gpoDir\ScheduledTasks.xml"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{00000000-0000-0000-0000-000000000000}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}][{AADCED64-746C-4633-A97C-D61349046527}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}]"};
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) Kerberos Compound Auth
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) Kerberos Compound Auth" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System\KDC\Parameters" -ValueName "EnableCbacAndArmor" -Value 2 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System\KDC\Parameters" -ValueName "RequestCompoundId" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" -ValueName "EnableCbacAndArmor" -Value 1 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) PingCastle(A-HardenedPaths) - Hardened UNC Paths
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) PingCastle(A-HardenedPaths) - Hardened UNC Paths" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\NetworkProvider\HardenedPaths" -ValueName "\\*\NETLOGON" -Value "RequireMutualAuthentication=1, RequireIntegrity=1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\NetworkProvider\HardenedPaths" -ValueName "\\*\SYSVOL" -Value "RequireMutualAuthentication=1, RequireIntegrity=1" -Type String >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) Enable TLS1.3&#x26;TLS1.2&#x26;TLS1.1
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) Enable TLS1.3&#x26;TLS1.2&#x26;TLS1.1" -Comment "##################################`r`n`r`nEnable TLS1.1, TLS 1.2 and TLS 1.3`r`nValues:`r`n    0x00000008	Enable SSL 2.0`r`n    0x00000020	Enable SSL 3.0`r`n    0x00000080	Enable TLS 1.0`r`n    0x00000200	Enable TLS 1.1`r`n    0x00000800	Enable TLS 1.2`r`n    0x00002000	Enable TLS 1.3`r`n`r`nSide effect: None`r`nIf Disabled: an block some SSL/TLS connections" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings" -ValueName "SecureProtocols" -Value 10752 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp" -ValueName "DefaultSecureProtocols" -Value 10752 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp" -ValueName "DefaultSecureProtocols" -Value 10752 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.3\Client" -ValueName "DisabledByDefault" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.3\Client" -ValueName "Enabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client" -ValueName "DisabledByDefault" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client" -ValueName "Enabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client" -ValueName "DisabledByDefault" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client" -ValueName "Enabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.3\Server" -ValueName "DisabledByDefault" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.3\Server" -ValueName "Enabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" -ValueName "DisabledByDefault" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" -ValueName "Enabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" -ValueName "DisabledByDefault" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" -ValueName "Enabled" -Value 1 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) PingCastle() - Enable TLS1.3&#x26;TLS1.2 | Disable others
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) PingCastle() - Enable TLS1.3&#x26;TLS1.2 | Disable others" -Comment "##################################`r`n`r`nEnable TLS 1.2 and TLS 1.3`r`nDisable SSL3, TLS1.0, TLS1.1`r`nValues:`r`n    0x00000008	Enable SSL 2.0`r`n    0x00000020	Enable SSL 3.0`r`n    0x00000080	Enable TLS 1.0`r`n    0x00000200	Enable TLS 1.1`r`n    0x00000800	Enable TLS 1.2`r`n    0x00002000	Enable TLS 1.3`r`n`r`nSide effect: None`r`nIf Disabled: an block some SSL/TLS connections" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings" -ValueName "SecureProtocols" -Value 10240 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp" -ValueName "DefaultSecureProtocols" -Value 10240 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp" -ValueName "DefaultSecureProtocols" -Value 10240 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.3\Client" -ValueName "DisabledByDefault" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.3\Client" -ValueName "Enabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.3\Server" -ValueName "DisabledByDefault" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.3\Server" -ValueName "Enabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client" -ValueName "DisabledByDefault" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client" -ValueName "Enabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" -ValueName "DisabledByDefault" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" -ValueName "Enabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client" -ValueName "DisabledByDefault" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client" -ValueName "Enabled" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" -ValueName "DisabledByDefault" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" -ValueName "Enabled" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" -ValueName "DisabledByDefault" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" -ValueName "Enabled" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client" -ValueName "DisabledByDefault" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client" -ValueName "Enabled" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server" -ValueName "DisabledByDefault" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server" -ValueName "Enabled" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client" -ValueName "DisabledByDefault" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client" -ValueName "Enabled" -Value 0 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) UAC configuration
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) UAC configuration" -Comment "##################################`r`n`r`nFrom CIS 2.3.17.1 UAC - Ensure 'User Account Control: Admin Approval Mode for the Built-in Administrator account' is set to 'Enabled'`r`nFilterAdministratorToken=1=>This value builds a filtered token. It's the default value. The administrator credentials are removed.`r`nLocalAccountTokenFilterPolicy=0=>This value builds a filtered token. It's the default value. The administrator credentials are removed." | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System" -ValueName "FilterAdministratorToken" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System" -ValueName "LocalAccountTokenFilterPolicy" -Value 0 -Type DWord >$null
	$_
} | New-GPLink -target "OU=Domain Controllers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) RDP server configuration
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) RDP server configuration" -Comment "##################################`r`n`r`n- Kill active session after 8h of idle`r`n- Kill process of disconnected sessions after 2h`r`n- Force usage of SSL (18.9.59.3.9.3 (L1) Ensure 'Require use of specific security layer for remote (RDP) connections' is set to 'Enabled: SSL' (Scored))`r`n- Purge temp dir of the user (18.9.59.3.11.1 (L1) Ensure 'Do not delete temp folders upon exit' is set to 'Disabled' (Scored))`r`n- Disable storage of Domain credz`r`n- Force NLA (Network Level Authentication)`r`n- Require secure RPC communication" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -ValueName "KeepAliveInterval" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -ValueName "DeleteTempDirsOnExit" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -ValueName "SecurityLayer" -Value 2 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -ValueName "UserAuthentication" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -ValueName "MaxIdleTime" -Value (1000*60*8) -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -ValueName "MaxDisconnectionTime" -Value (1000*60*2) -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -ValueName "RemoteAppLogoffTimeLimit" -Value (1000*60*2) -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -ValueName "fEncryptRPCTraffic" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -ValueName "MinEncryptionLevel" -Value 3 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -ValueName "AllowEncryptionOracle" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\System\CurrentControlSet\Control\Lsa" -ValueName "DisableDomainCreds" -Value 1 -Type DWord >$null
	$_
} | New-GPLink -target "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) RDP Client configuration
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) RDP Client configuration" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPPrefRegistryValue -Key "HKEY_CLASSES_ROOT\.RDP" -ValueName "(Default)" -Value "txtfile" -Type String -Context User -Action Replace >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) PingCastle(S-FolderOption) - Hardened file extension to notepad
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) PingCastle(S-FolderOption) - Hardened file extension to notepad" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPPrefRegistryValue -Key "HKEY_CLASSES_ROOT\.jse" -ValueName "(Default)" -Value "txtfile" -Type String -Context User -Action Replace >$null
	$_ | Set-GPPrefRegistryValue -Key "HKEY_CLASSES_ROOT\.js" -ValueName "(Default)" -Value "txtfile" -Type String -Context User -Action Replace >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) LogSystem
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) LogSystem" -Comment "##################################`r`n`r`nWindows logs configuration:`r`n- type of logs`r`n- size of logs`r`n`r`nIf disabled: Lost logs information" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$inf =  "[Unicode]`r`n";
	$inf += "Unicode=yes`r`n";
	$audit = "";
	$inf += "[Event Audit]`r`n"
	$inf += 'AuditSystemEvents = 3'+"`r`n";
	$inf += 'AuditLogonEvents = 3'+"`r`n";
	$inf += 'AuditObjectAccess = 3'+"`r`n";
	$inf += 'AuditPrivilegeUse = 3'+"`r`n";
	$inf += 'AuditPolicyChange = 3'+"`r`n";
	$inf += 'AuditAccountManage = 3'+"`r`n";
	$inf += 'AuditProcessTracking = 3'+"`r`n";
	$inf += 'AuditDSAccess = 3'+"`r`n";
	$inf += 'AuditAccountLogon = 3'+"`r`n";
	$audit += ',System,Audit Authentication Policy Change,{0cce9230-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Computer Account Management,{0cce9236-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit DPAPI Activity,{0cce922d-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Kerberos Authentication Service,{0cce9242-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Kerberos Service Ticket Operations,{0cce9240-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Logoff,{0cce9216-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Logon,{0cce9215-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Other Account Logon Events,{0cce9241-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Process Creation,{0cce922b-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Security Group Management,{0cce9237-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Security System Extension,{0cce9211-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Sensitive Privilege Use,{0cce9228-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Special Logon,{0cce921b-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit User Account Management,{0cce9235-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Directory Service Changes,{0cce923c-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Directory Service Replication,{0cce923d-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Account Lockout,{0cce9217-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Certification Services,{0cce9221-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Detailed File Share,{0cce9244-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit File Share,{0cce9224-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$audit += ',System,Audit Filtering Platform Connection,{0cce9226-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$inf += "[Version]`r`n";
	$inf += 'signature="$CHICAGO$"'+"`r`n";
	$inf += "Revision=1`r`n";
	$gpoSecEditPath="$gpoBasePath\Machine\Microsoft\Windows NT\SecEdit"
	mkdir "$gpoSecEditPath" >$null
	$inf > "$gpoSecEditPath\GptTmpl.inf"
	$audit = $audit.Trim("`r`n")
	$gpoAuditPath="$gpoBasePath\Machine\Microsoft\Windows NT\Audit"
	mkdir "$gpoAuditPath" >$null
	"Machine Name,Policy Target,Subcategory,Subcategory GUID,Inclusion Setting,Exclusion Setting,Setting Value`r`n$audit" | Out-File -Encoding ASCII $gpoAuditPath\audit.csv
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{827D319E-6EAC-11D2-A4EA-00C04F79F83A}{803E14A0-B4FB-11D0-A0D0-00A0C90F574B}][{F3CCC681-B74C-4060-9F26-CD84525DCA2A}{0F3F3735-573D-9804-99E4-AB2A69BA5FD4}]"};
	$_ | Set-GPRegistryValue -Key "HKLM\System\CurrentControlSet\Control\Lsa\MSV1_0" -ValueName "AuditReceivingNTLMTraffic" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\System\CurrentControlSet\Services\Netlogon\Parameters" -ValueName "AuditNTLMInDomain" -Value 7 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\PowerShell\ModuleLogging" -ValueName "EnableModuleLogging" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKCU\Software\Policies\Microsoft\Windows\PowerShell\Transcription" -ValueName "EnableTranscripting" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKCU\Software\Policies\Microsoft\Windows\PowerShell\Transcription" -ValueName "EnableInvocationHeader" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKCU\Software\Policies\Microsoft\Windows\PowerShell\Transcription" -ValueName "OutputDirectory" -Value "C:\Windows\Powershell.log" -Type ExpandString >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\PowerShell\Transcription" -ValueName "EnableTranscripting" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\PowerShell\Transcription" -ValueName "EnableInvocationHeader" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\PowerShell\Transcription" -ValueName "OutputDirectory" -Value "C:\Windows\Powershell.log" -Type ExpandString >$null
	$_ | Set-GPRegistryValue -Key "HKCU\Software\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging" -ValueName "EnableScriptBlockLogging" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging" -ValueName "EnableScriptBlockLogging" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Channels\Microsoft-Windows-Dhcp-Client/Operational" -ValueName "Enabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Channels\Microsoft-Windows-Dhcpv6-Client/Operational" -ValueName "Enabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Channels\Microsoft-Windows-DNS-Client/Operational" -ValueName "Enabled" -Value 1 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) Auto lock session after 15min
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) Auto lock session after 15min" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System" -ValueName "InactivityTimeoutSecs" -Value 900 -Type DWord >$null
	$_
} | New-GPLink -target "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) Deny anonymous SMB (Block CobaltStrike from using \\evil.kali\tmp\becon.exe)
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) Deny anonymous SMB (Block CobaltStrike from using \\evil.kali\tmp\becon.exe)" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\LanmanWorkstation" -ValueName "AllowInsecureGuestAuth" -Value 0 -Type DWord >$null
	$_
} | New-GPLink -target "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) WinRM - Configuration
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) WinRM - Configuration" -Comment "##################################`r`n`r`nWinRM configuration:`r`n- Disable basic auth &#x26; Negotiate`r`n- Force kerberos auth`r`n- Enable protection against relay`r`n`r`nIf disabled: Restore WinRM default configuration" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Protocol" -Group "[GPO][1mm0rt41][Hardening](GPO,Computer) WinRM - Configuration" -Action Allow -Direction Inbound -LocalPort TCP -Protocol undefined >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] LocalPort" -Group "[GPO][1mm0rt41][Hardening](GPO,Computer) WinRM - Configuration" -Action Allow -Direction Inbound -LocalPort @(5985,5986) -Protocol undefined >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service" -ValueName "AllowBasic" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service" -ValueName "AllowDigest" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service" -ValueName "AllowCredSSP" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service" -ValueName "AllowKerberos" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service" -ValueName "CbtHardeningLevel" -Value "Strict" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service" -ValueName "AllowNegotiate" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service" -ValueName "AllowUnencryptedTraffic" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Service" -ValueName "DisableRunAs" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Client" -ValueName "AllowBasic" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Client" -ValueName "AllowDigest" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Client" -ValueName "AllowCredSSP" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Client" -ValueName "AllowKerberos" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Client" -ValueName "CbtHardeningLevel" -Value "Strict" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Client" -ValueName "AllowNegotiate" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Client" -ValueName "AllowUnencryptedTraffic" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WinRM\Client" -ValueName "DisableRunAs" -Value 1 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) WSUS - Configuration with HTTPS
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) WSUS - Configuration with HTTPS" -Comment "##################################`r`n`r`nWSUS configuration:`r`n- Force HTTPS`r`n`r`nIf disabled: Restore WSUS default configuration" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "WUServer" -Value "https://xxxxx.corp.lo:8531" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "WUStatusServer" -Value "https://xxxxx.corp.lo:8531" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "UseWUServer" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "NoAutoUpdate" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "AUOptions" -Value 2 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "ScheduledInstallDay" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "ScheduledInstallTime" -Value 3 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) Print spooler configuration
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) Print spooler configuration" -Comment "##################################`r`n`r`nConfigure spooler to avoid priviledge escalation.`r`n`r`nSide effect: Block installation of new printers ! Package your printer drivers in the image or via WSUS/SCCM`r`nIf disabled: Lost logs information" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" -ValueName "RestrictDriverInstallationToAdministrators" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" -ValueName "NoWarningNoElevationOnInstall" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" -ValueName "UpdatePromptSettings" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" -ValueName "InForest" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" -ValueName "TrustedServers" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PackagePointAndPrint" -ValueName "PackagePointAndPrintOnly" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PackagePointAndPrint" -ValueName "PackagePointAndPrintServerList" -Value 1 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) SMB server - FileServer configuration
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) SMB server - FileServer configuration" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" -ValueName "SMB1" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" -ValueName "EnableSecuritySignature" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" -ValueName "RequireSecuritySignature" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" -ValueName "AutoShareWks" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" -ValueName "AutoShareServer" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" -ValueName "AutoDisconnect" -Value 60 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" -ValueName "RestrictNullSessAccess" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" -ValueName "SmbServerNameHardeningLevel" -Value 2 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\System\CurrentControlSet\Services\Rdr\Parameters" -ValueName "EnableSecuritySignature" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\System\CurrentControlSet\Services\Rdr\Parameters" -ValueName "RequireSecuritySignature" -Value 1 -Type DWord >$null
	$_
} | New-GPLink -target "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) SMB client configuration
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) SMB client configuration" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters" -ValueName "EnableSecuritySignature" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters" -ValueName "RequireSecuritySignature" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters" -ValueName "EnablePlainTextPassword" -Value 0 -Type DWord >$null
	$_
} | New-GPLink -target "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) Bitlocker
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) Bitlocker" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$gpoDir="$gpoBasePath\Machine\Scripts";
	mkdir "$gpoDir" >$null
	@'
if( (Get-BitLockerVolume C:).ProtectionStatus -ne 'On' ){
    Enable-BitLocker -MountPoint "C:" -EncryptionMethod XtsAes256 -UsedSpaceOnly -TpmProtector -SkipHardwareTest
    Add-BitLockerKeyProtector -MountPoint "C:" -RecoveryPasswordProtector
    Sleep -Seconds 30
    Get-BitLockerVolume C: | ConvertTo-JSON | Out-File -encoding ascii "\\$($env:USERDNSDOMAIN)\SYSVOL\$($env:USERDNSDOMAIN)\Scripts\Bitlocker\$($env:COMPUTERNAME).json"
}

'@ | Out-File -Encoding Ascii "$gpoDir\BitLockerEnabler.ps1" 
	$gpoDir="$gpoBasePath\Machine\Preferences\ScheduledTasks";
	mkdir "$gpoDir" >$null
	$taskXml = ( @"
&#x3C;?xml version="1.0" encoding="utf-8"?>
&#x3C;ScheduledTasks clsid="{CC63F200-7309-4ba0-B154-A71CD118DBCC}">&#x3C;ImmediateTaskV2 clsid="{9756B581-76EC-4169-9AFC-0CA8D43ADB5F}" name="[GPO] BitLockerEnabler" image="0" changed="$((Get-Date).ToString("yyyy-MM-dd HH:mm:ss"))" uid="{D98A502B-7563-4A3D-A4EA-5B4EE8E63364}" userContext="0" removePolicy="0">&#x3C;Properties action="R" name="[GPO] BitLockerEnabler" runAs="S-1-5-18" logonType="S4U">&#x3C;Task version="1.2">&#x3C;RegistrationInfo>&#x3C;Author>$($env:USERDOMAIN)\$($env:USERNAME)&#x3C;/Author>&#x3C;Description>&#x3C;![CDATA[This task need to run with S-1-5-18 // GPO Id: $gpoId // GPO Name: $gpoName]]>&#x3C;/Description>&#x3C;/RegistrationInfo>&#x3C;Principals>&#x3C;Principal id="Author">&#x3C;UserId>S-1-5-18&#x3C;/UserId>&#x3C;LogonType>S4U&#x3C;/LogonType>&#x3C;RunLevel>HighestAvailable&#x3C;/RunLevel>&#x3C;/Principal>&#x3C;/Principals>&#x3C;Settings>&#x3C;IdleSettings>&#x3C;Duration>PT5M&#x3C;/Duration>&#x3C;WaitTimeout>PT1H&#x3C;/WaitTimeout>&#x3C;StopOnIdleEnd>false&#x3C;/StopOnIdleEnd>&#x3C;RestartOnIdle>false&#x3C;/RestartOnIdle>&#x3C;/IdleSettings>&#x3C;MultipleInstancesPolicy>IgnoreNew&#x3C;/MultipleInstancesPolicy>&#x3C;DisallowStartIfOnBatteries>true&#x3C;/DisallowStartIfOnBatteries>&#x3C;StopIfGoingOnBatteries>true&#x3C;/StopIfGoingOnBatteries>&#x3C;AllowHardTerminate>true&#x3C;/AllowHardTerminate>&#x3C;StartWhenAvailable>true&#x3C;/StartWhenAvailable>&#x3C;AllowStartOnDemand>true&#x3C;/AllowStartOnDemand>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;Hidden>false&#x3C;/Hidden>&#x3C;ExecutionTimeLimit>PT1H&#x3C;/ExecutionTimeLimit>&#x3C;Priority>7&#x3C;/Priority>&#x3C;DeleteExpiredTaskAfter>PT0S&#x3C;/DeleteExpiredTaskAfter>&#x3C;RestartOnFailure>&#x3C;Interval>PT10M&#x3C;/Interval>&#x3C;Count>3&#x3C;/Count>&#x3C;/RestartOnFailure>&#x3C;RunOnlyIfNetworkAvailable>true&#x3C;/RunOnlyIfNetworkAvailable>&#x3C;/Settings>&#x3C;Actions Context="Author">&#x3C;Exec>&#x3C;Command>powershell&#x3C;/Command>&#x3C;Arguments>-exec bypass -nop -File "%MachineScriptUNC%\\BitLockerEnabler.ps1"&#x3C;/Arguments>&#x3C;/Exec>&#x3C;/Actions>&#x3C;Triggers>&#x3C;TimeTrigger>&#x3C;StartBoundary>%LocalTimeXmlEx%&#x3C;/StartBoundary>&#x3C;EndBoundary>%LocalTimeXmlEx%&#x3C;/EndBoundary>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;/TimeTrigger>&#x3C;/Triggers>&#x3C;/Task>&#x3C;/Properties>&#x3C;/ImmediateTaskV2>
&#x3C;/ScheduledTasks>
"@ ).Trim()
	$taskXml = $taskXml.Replace("%MachineScriptUNC%","\\$($env:USERDNSDOMAIN)\SYSVOL\$($env:USERDNSDOMAIN)\Policies\$gpoId\Machine\Scripts");
	$taskXml = $taskXml.Replace("%MachineScript%","$gpoBasePath\Machine\Scripts");
	$taskXml | Out-File -Encoding ASCII "$gpoDir\ScheduledTasks.xml"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{00000000-0000-0000-0000-000000000000}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}][{AADCED64-746C-4633-A97C-D61349046527}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}]"};
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\FVE" -ValueName "ActiveDirectoryBackup" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\FVE" -ValueName "OSActiveDirectoryBackup" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\FVE" -ValueName "FDVActiveDirectoryBackup" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\FVE" -ValueName "RDVActiveDirectoryBackup" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\FVE" -ValueName "OSRecovery" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\FVE" -ValueName "RequireActiveDirectoryBackup" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\FVE" -ValueName "ActiveDirectoryInfoToStore" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\FVE" -ValueName "EncryptionMethodWithXtsOs" -Value 7 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\FVE" -ValueName "EncryptionMethodWithXtsFdv" -Value 7 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\FVE" -ValueName "EncryptionMethodWithXtsRdv" -Value 7 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\FVE" -ValueName "EncryptionMethodNoDiffuser" -Value 4 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\FVE" -ValueName "EncryptionMethod" -Value 2 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) Windows defender
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) Windows defender" -Comment "##################################`r`n`r`n75668C1F-73B5-4CF0-BB93-3ECF5CB7CC84: Block Office applications from injecting code into other processes`r`nD4F940AB-401B-4EFC-AADC-AD5F3C50688A: Block Office applications from creating child processes`r`n92E97FA1-2EDF-4476-BDD6-9DD0B4DDDC7B: Block Win32 API calls from Office macro`r`n3B576869-A4EC-4529-8536-B80A7769E899: Block Office applications from creating executable content`r`n5BEB7EFE-FD9A-4556-801D-275E5FFC04CC: Block execution of potentially obfuscated scripts`r`nBE9BA2D9-53EA-4CDC-84E5-9B1EEEE46550: Block executable content from email client and webmail`r`nD3E037E1-3EB8-44C8-A917-57927947596D: Block JavaScript or VBScript from launching downloaded executable content`r`nC1DB55AB-C21A-4637-BB3F-A12568109D35: Use advanced protection against ransomware`r`nB2B3F03D-6A65-4F7B-A9C7-1C7EF74A9BA4: Block untrusted and unsigned processes that run from USB`r`n7674BA52-37EB-4A4F-A9A1-F0F9A1619A2C: Block Adobe Reader from creating child processes`r`n9E6C4E1F-7D60-472F-BA1A-A39EF669E4B2: Block credential stealing from the Windows local security authority subsystem (lsass.exe)`r`nE6DB77E5-3DF2-4CF1-B95A-636979351E5B: Block persistence through WMI event subscription`r`n56A863A9-875E-4185-98A7-B882C64B5CE5: Block abuse of exploited vulnerable signed drivers`r`nA8F5898E-1DC8-49A9-9878-85004B8A61E6: Block Webshell creation for Servers`r`n01443614-CD74-433A-B99E-2ECDC07BFC25: Block executable files from running unless they meet a prevalence, age, or trusted list criterion`r`nD1E49AAC-8F56-4280-B9BA-993A6D77406C: Block process creations originating from PSExec and WMI commands`r`n26190899-1602-49E8-8B27-EB1D0A1CE869: Block Office communication application from creating child processes" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Signature Updates" -ValueName "DisableUpdateOnStartupWithoutEngine" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender" -ValueName "DisableAntiSpyware" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender" -ValueName "DisableRoutinelyTakingAction" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender" -ValueName "PUAProtection" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Real-Time Protection" -ValueName "DisableBehaviorMonitoring" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Real-Time Protection" -ValueName "DisableIOAVProtection" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Real-Time Protection" -ValueName "DisableOnAccessProtection" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Real-Time Protection" -ValueName "DisableRawWriteNotification" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Real-Time Protection" -ValueName "DisableScanOnRealtimeEnable" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Real-Time Protection" -ValueName "RealtimeScanDirection" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Scan" -ValueName "DisableHeuristics" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Scan" -ValueName "DisablePackedExeScanning" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR" -ValueName "ExploitGuard_ASR_Rules" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "75668C1F-73B5-4CF0-BB93-3ECF5CB7CC84" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "D4F940AB-401B-4EFC-AADC-AD5F3C50688A" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "92E97FA1-2EDF-4476-BDD6-9DD0B4DDDC7B" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "3B576869-A4EC-4529-8536-B80A7769E899" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "5BEB7EFE-FD9A-4556-801D-275E5FFC04CC" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "BE9BA2D9-53EA-4CDC-84E5-9B1EEEE46550" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "D3E037E1-3EB8-44C8-A917-57927947596D" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "C1DB55AB-C21A-4637-BB3F-A12568109D35" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "B2B3F03D-6A65-4F7B-A9C7-1C7EF74A9BA4" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "7674BA52-37EB-4A4F-A9A1-F0F9A1619A2C" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "9E6C4E1F-7D60-472F-BA1A-A39EF669E4B2" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "E6DB77E5-3DF2-4CF1-B95A-636979351E5B" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "56A863A9-875E-4185-98A7-B882C64B5CE5" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "A8F5898E-1DC8-49A9-9878-85004B8A61E6" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "01443614-CD74-433A-B99E-2ECDC07BFC25" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "D1E49AAC-8F56-4280-B9BA-993A6D77406C" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\ASR\Rules" -ValueName "26190899-1602-49E8-8B27-EB1D0A1CE869" -Value "1" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows Defender\Windows Defender Exploit Guard\Network Protection" -ValueName "EnableNetworkProtection" -Value 1 -Type DWord >$null
	$_
} | New-GPLink -target "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) [CVE] Fix-exploit-kerberos-samaccountname-spoofing #CVE-2021-42287 #CVE-2021-42278
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) [CVE] Fix-exploit-kerberos-samaccountname-spoofing #CVE-2021-42287 #CVE-2021-42278" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
#To find any computer accounts that have a invalid SamAccountName property use this query
Get-ADComputer -Filter { samAccountName -notlike "*$" } | Set-ADComputer -Enabled $false
	$_ | Set-GPRegistryValue -Key "HKLM\System\CurrentControlSet\Services\Kdc" -ValueName "PacRequestorEnforcement" -Value 2 -Type DWord >$null
	$_
} | New-GPLink -target "OU=Domain Controllers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) LDAP client configuration
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) LDAP client configuration" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\System\CurrentControlSet\Services\LDAP" -ValueName "LDAPClientIntegrity" -Value 2 -Type DWord >$null
	$_
} | New-GPLink -target "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) LDAP server configuration
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) LDAP server configuration" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\System\CurrentControlSet\Services\NTDS\Parameters" -ValueName "LDAPServerIntegrity" -Value 2 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\System\CurrentControlSet\Services\NTDS\Parameters" -ValueName "LdapEnforceChannelBinding" -Value 2 -Type DWord >$null
	$_
} | New-GPLink -target "OU=Domain Controllers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) LSASS Protection (Mimikatz)
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) LSASS Protection (Mimikatz)" -Comment "##################################`r`n`r`nProtect the process lsass.exe to avoid an attacker to hijack credentials by dumping lsass.exe`r`n`r`nRequire: Do not deploy on computers that require Smartcard/2FA with DLL not signed by Microsoft            `r`nSide effect: Block all DLL not signed by Microsoft.`r`nIf disabled: An attacker can abuse of dumping lsass.exe to grab AD credentials of past connections.`r`nDoc: https://itm4n.github.io/lsass-runasppl/`r`n`r`nRunAsPPL=1 => Enforce with UEFI + SecureBoot`r`nRunAsPPL=2 => Enforce without UEFI + SecureBoot" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest" -ValueName "UseLogonCredential" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest" -ValueName "Negotiate" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\LSA" -ValueName "RunAsPPL" -Value 2 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\LSA" -ValueName "DisableRestrictedAdmin" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\LSA" -ValueName "DisableRestrictedAdminOutboundCreds" -Value 1 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) LSASS Protection (Mimikatz)(tspkg)
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) LSASS Protection (Mimikatz)(tspkg)" -Comment "##################################`r`n`r`nBlock credential delegation to avoid an attacker to hijack credentials from lsass.exe`r`n`r`nRequire: Do not deploy on RDP server that require delegation`r`nSide effect: Can kill RDP server for delegation.`r`nIf disabled: An attacker can abuse of lsass and rdp service to grab AD credentials of past connections.`r`nDoc: https://clement.notin.org/blog/2019/07/03/credential-theft-without-admin-or-touching-lsass-with-kekeo-by-abusing-credssp-tspkg-rdp-sso/" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\CredentialsDelegation" -ValueName "AllowDefaultCredentials" -Value 0 -Type DWORD >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\CredentialsDelegation" -ValueName "ConcatenateDefaults_AllowDefault" -Value 0 -Type DWORD >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\CredentialsDelegation" -ValueName "AllowDefCredentialsWhenNTLMOnly" -Value 0 -Type DWORD >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\CredentialsDelegation" -ValueName "ConcatenateDefaults_AllowDefNTLMOnly" -Value 0 -Type DWORD >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\CredentialsDelegation\AllowDefCredentialsWhenNTLMOnly" -ValueName "1" -Value "" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\CredentialsDelegation\AllowDefaultCredentials" -ValueName "1" -Value "" -Type String >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) WIFI-Protection - AirStrike
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) WIFI-Protection - AirStrike" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\System" -ValueName "DontDisplayNetworkSelectionUI" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\PolicyManager\default\WiFi\AllowAutoConnectToWiFiSenseHotspots" -ValueName "value" -Value 0 -Type DWord >$null
	$_
} | New-GPLink -target "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) Disable print spooler
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) Disable print spooler" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$inf =  "[Unicode]`r`n";
	$inf += "Unicode=yes`r`n";
	$audit = "";
	$inf += "[Service General Setting]`r`n"
	$inf += '"Spooler",4,""'+"`r`n";
	$inf += "[Version]`r`n";
	$inf += 'signature="$CHICAGO$"'+"`r`n";
	$inf += "Revision=1`r`n";
	$gpoSecEditPath="$gpoBasePath\Machine\Microsoft\Windows NT\SecEdit"
	mkdir "$gpoSecEditPath" >$null
	$inf > "$gpoSecEditPath\GptTmpl.inf"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{827D319E-6EAC-11D2-A4EA-00C04F79F83A}{803E14A0-B4FB-11D0-A0D0-00A0C90F574B}]"};
	$_
} | New-GPLink -target "OU=Domain Controllers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) Disable WebClient
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) Disable WebClient" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$inf =  "[Unicode]`r`n";
	$inf += "Unicode=yes`r`n";
	$audit = "";
	$inf += "[Service General Setting]`r`n"
	$inf += '"Webclient",4,""'+"`r`n";
	$inf += "[Version]`r`n";
	$inf += 'signature="$CHICAGO$"'+"`r`n";
	$inf += "Revision=1`r`n";
	$gpoSecEditPath="$gpoBasePath\Machine\Microsoft\Windows NT\SecEdit"
	mkdir "$gpoSecEditPath" >$null
	$inf > "$gpoSecEditPath\GptTmpl.inf"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{827D319E-6EAC-11D2-A4EA-00C04F79F83A}{803E14A0-B4FB-11D0-A0D0-00A0C90F574B}]"};
	$_
} | New-GPLink -target "OU=Domain Controllers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) LLMNR
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) LLMNR" -Comment "##################################`r`n`r`nProtection against Man-In-The-Middle.`r`nSide effect: Check first that dns suffix is deployed everywhere" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Drop LLMNR" -Group "[GPO][1mm0rt41][Hardening](GPO,Computer) LLMNR" -Action Block -Direction Outbound -Protocol UDP -RemotePort 5355 >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\DNSClient" -ValueName "EnableMulticast" -Value 0 -Type DWord >$null
	$_
} | New-GPLink -target "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) NetBios
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) NetBios" -Comment "##################################`r`n`r`nProtection against Man-In-The-Middle.`r`nSide effect: Check first that dns suffix is deployed everywhere" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Drop NetBios" -Group "[GPO][1mm0rt41][Hardening](GPO,Computer) NetBios" -Action Block -Direction Outbound -Protocol UDP -RemotePort 137 >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\Netbt\Parameters" -ValueName "NodeType" -Value 2 -Type DWord >$null
	$_
} | New-GPLink -target "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) mDNS
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) mDNS" -Comment "##################################`r`n`r`nProtection against Man-In-The-Middle.`r`nSide effect: Check first that dns suffix is deployed everywhere" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters" -ValueName "EnableMDNS" -Value 0 -Type DWord >$null
	$_
} | New-GPLink -target "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) IPv6
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) IPv6" -Comment "##################################`r`n`r`nProtection against Man-In-The-Middle.`r`nSide effect: None" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] IPv6" -Group "[GPO][1mm0rt41][Hardening](GPO,Computer) IPv6" -Action Block -Direction Outbound -Protocol IPv6 >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] IPv6-Frag" -Group "[GPO][1mm0rt41][Hardening](GPO,Computer) IPv6" -Action Block -Direction Outbound -Protocol IPv6-Frag >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] IPv6-Route" -Group "[GPO][1mm0rt41][Hardening](GPO,Computer) IPv6" -Action Block -Direction Outbound -Protocol IPv6-Route >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] ICMPv6" -Group "[GPO][1mm0rt41][Hardening](GPO,Computer) IPv6" -Action Block -Direction Outbound -Protocol ICMPv6 >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] IPv6-NoNxt" -Group "[GPO][1mm0rt41][Hardening](GPO,Computer) IPv6" -Action Block -Direction Outbound -Protocol IPv6-NoNxt >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] IPv6-Opts" -Group "[GPO][1mm0rt41][Hardening](GPO,Computer) IPv6" -Action Block -Direction Outbound -Protocol IPv6-Opts >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] DHCPv6" -Group "[GPO][1mm0rt41][Hardening](GPO,Computer) IPv6" -Action Block -Direction Outbound -Protocol UDP -RemotePort 547 >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\Tcpip6\Parameters" -ValueName "DisabledComponents" -Value 32 -Type DWord >$null
	$_
} | New-GPLink -target "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) WPAD
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) WPAD" -Comment "##################################`r`n`r`nProtection against Man-In-The-Middle.`r`n`r`nRequire: Check if corp use automatic proxy settings`r`nSide effect: Can block network communication if WAD proxy is used but not deployed via GPO ou via DNS entry" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$inf =  "[Unicode]`r`n";
	$inf += "Unicode=yes`r`n";
	$audit = "";
	$inf += "[Service General Setting]`r`n"
	$inf += '"WinHttpAutoProxySvc",4,""'+"`r`n";
	$inf += "[Version]`r`n";
	$inf += 'signature="$CHICAGO$"'+"`r`n";
	$inf += "Revision=1`r`n";
	$gpoSecEditPath="$gpoBasePath\Machine\Microsoft\Windows NT\SecEdit"
	mkdir "$gpoSecEditPath" >$null
	$inf > "$gpoSecEditPath\GptTmpl.inf"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{827D319E-6EAC-11D2-A4EA-00C04F79F83A}{803E14A0-B4FB-11D0-A0D0-00A0C90F574B}]"};
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\WinHttpAutoProxySvc" -ValueName "Start" -Value 4 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Wpad" -ValueName "WpadOverride" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Wpad" -ValueName "WpadOverride" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -ValueName "AutoDetect" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -ValueName "AutoDetect" -Value 0 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) Windows-LAPSv2
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) Windows-LAPSv2" -Comment "##################################`r`n`r`nConfiguration for Windows LAPS (new LAPS)`r`n`r`nRequire: Check if local admin is not used by scripts/services/schtask`r`nSide effect: None" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
# Doc: https://www.it-connect.fr/tuto-configurer-windows-laps-active-directory/
# Doc: https://learn.microsoft.com/en-us/windows-server/identity/laps/laps-management-policy-settings
Import-Module LAPS
# Note admx is at C:\Windows\PolicyDefinitions\LAPS.admx
# > Computer Configuration > Policies > Administration template > System > LAPS
Update-LapsADSchema -Verbose
# Will add the following attributes
#    msLAPS-PasswordExpirationTime
#    msLAPS-Password
#    msLAPS-EncryptedPassword
#    msLAPS-EncryptedPasswordHistory
#    msLAPS-EncryptedDSRMPassword
#    msLAPS-EncryptedDSRMPasswordHistory
# Allow computers to update their local admin password
Set-LapsADComputerSelfPermission -Identity "OU=Computers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)"
New-ADGroup -Name PRIV_WLAPS_READER -Path "OU=_CriticalUsers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -GroupCategory Security -GroupScope DomainLocal
Set-LapsADReadPasswordPermission -Identity "OU=Computers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -AllowedPrincipals PRIV_WLAPS_READER
Set-LapsADAuditing -Identity "OU=Computers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -AuditedPrincipals PRIV_WLAPS_READER -AuditType Success
#xcopy C:\Windows\PolicyDefinition\LAPS.admx and \en-US\LAPS.adml
#xcopy C:\Windows\PolicyDefinition\en-US\LAPS.adml \corp.net\SYSVOL\corp.net\Policies\PolicyDefinitions
#
# Event: Applications and Services Logs>Microsoft>Windows>LAPS
#
# delete LAPS.Legacy agent:
# > MsiExec.exe /x {EA8CB806-C109-4700-96B4-F1F268E5036C} /qn
# | Policy name	                | Policy registry key root
# |=============================|===============================================================
# | LAPS CSP	                | HKLM\Software\Microsoft\Policies\LAPS
# | LAPS Group Policy	        | HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS
# | LAPS Local Configuration	| HKLM\Software\Microsoft\Windows\CurrentVersion\LAPS\Config
# | Legacy Microsoft LAPS	    | HKLM\Software\Policies\Microsoft Services\AdmPwd
#
# TO force LAPS refresh on a computer:
# > Invoke-LapsPolicyProcessing
#
# Get Password
# > Get-LapsADPassword "PC-01" -AsPlainText -IncludeHistory
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS" -ValueName "BackupDirectory" -Value 2 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS" -ValueName "PasswordAgeDays" -Value 30 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS" -ValueName "PasswordLength" -Value 20 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS" -ValueName "PasswordComplexity" -Value 4 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS" -ValueName "ADPasswordEncryptionEnabled" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS" -ValueName "ADBackupDSRMPassword" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS" -ValueName "PostAuthenticationResetDelay" -Value 6 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\LAPS" -ValueName "PostAuthenticationActions" -Value 3 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\LAPS\Config" -ValueName "BackupDirectory" -Value 2 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\LAPS\Config" -ValueName "PasswordAgeDays" -Value 30 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\LAPS\Config" -ValueName "PasswordLength" -Value 20 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\LAPS\Config" -ValueName "PasswordComplexity" -Value 4 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\LAPS\Config" -ValueName "ADPasswordEncryptionEnabled" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\LAPS\Config" -ValueName "ADBackupDSRMPassword" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\LAPS\Config" -ValueName "PostAuthenticationResetDelay" -Value 6 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\LAPS\Config" -ValueName "PostAuthenticationActions" -Value 3 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) LAPS.Legacy
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) LAPS.Legacy" -Comment "##################################`r`n`r`nConfiguration for LAPS.Legacy: Every 30 days change local administrator password with a random one with a length of 16 random chars [A-Za-z0-9-+*/*`$=)]`r`nSide effect: None`r`nNote: Debug mode is enabled for tracking LAPS error" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
# Installation of LAPS &#x26; LAPS UI on the DC
choco install laps --params='/ALL' -y
# Update the schema of the AD with new attributes:
#   ms-MCS-AdmPwd
#   ms-MCS-AdmPwdExpirationTime
Import-module AdmPwd.PS
Update-AdmPwdADSchema
# Set ACLs to set passwords in ms-Mcs-AdmPwd by SELF (computers)
Set-AdmPwdComputerSelfPermission -Identity _AllComputers # &#x3C;Base OU with computers>
# Create LAPS auto deployement
New-GPOSchTask -GPOName "[SD][Choco] LAPS" -TaskName "[SD][Choco] LAPS" -TaskType ImmediateTask -Command 'C:\ProgramData\chocolatey\bin\choco.exe' -CommandArguments 'install -y laps'
# One line full deploy New-GPOSchTask -GPOName "[SD][Choco] LAPS" -TaskName "[SD][Choco] LAPS" -TaskType ImmediateTask -Command "powershell.exe" -CommandArguments '-exec bypass -nop -Command "[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12;iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex; C:\ProgramData\chocolatey\bin\choco.exe install -y laps"'
# Grant a group for LAPS of computers of an OU
##dsacls "OU=SecretComputers,OU=azerty,DC=corp,DC=local" /G "LAPS_READER_4_SecretComputers:CA;ms-Mcs-AdmPwd"
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft Services\AdmPwd" -ValueName "AdmPwdEnabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft Services\AdmPwd" -ValueName "PwdExpirationProtectionEnabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft Services\AdmPwd" -ValueName "PasswordComplexity" -Value 4 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft Services\AdmPwd" -ValueName "PasswordLength" -Value 16 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft Services\AdmPwd" -ValueName "PasswordAgeDays" -Value 30 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\GPExtensions\{D76B9641-3288-4f75-942D-087DE603E3EA}" -ValueName "ExtensionDebugLevel" -Value 2 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) WindowsUpdate for users
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) WindowsUpdate for users" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "SetAutoRestartDeadline" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "AutoRestartDeadlinePeriodInDays" -Value 3 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "AutoRestartDeadlinePeriodInDaysForFeatureUpdates" -Value 3 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "SetComplianceDeadline" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "ConfigureDeadlineForQualityUpdates" -Value 3 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "ConfigureDeadlineGracePeriod" -Value 2 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "ConfigureDeadlineForFeatureUpdates" -Value 3 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "ConfigureDeadlineGracePeriodForFeatureUpdates" -Value 2 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "ConfigureDeadlineNoAutoReboot" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "SetActiveHoursMaxRange" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "ActiveHoursMaxRange" -Value 18 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "AUOptions" -Value 4 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "AutomaticMaintenanceEnabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "ScheduledInstallDay" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "ScheduledInstallTime" -Value 12 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "ScheduledInstallEveryWeek" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "AllowMUUpdateService" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "AutoInstallMinorUpdates" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "IncludeRecommendedUpdates" -Value 1 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Hardening](GPO,Computer) WindowsUpdate for servers
###########################################################################################
New-GPO -Name "[1mm0rt41][Hardening](GPO,Computer) WindowsUpdate for servers" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "SetActiveHoursMaxRange" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate" -ValueName "ActiveHoursMaxRange" -Value 18 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "AUOptions" -Value 4 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "AutomaticMaintenanceEnabled" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "ScheduledInstallDay" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "ScheduledInstallTime" -Value 3 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "ScheduledInstallEveryWeek" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "AllowMUUpdateService" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "AutoInstallMinorUpdates" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU" -ValueName "IncludeRecommendedUpdates" -Value 1 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Log](GPO,Computer) LSA &#x26; NTLM Audit Mode
###########################################################################################
New-GPO -Name "[1mm0rt41][Log](GPO,Computer) LSA &#x26; NTLM Audit Mode" -Comment "##################################`r`n`r`nWindows logs configuration:`r`n- Audit LSA protection (RunAsPPL)`r`n- Audit incoming NTLM traffic for all accounts:`r`n    to view =>`r`n    Get-WinEvent -Filterxml @'`r`n    &#x3C;QueryList>`r`n     &#x3C;Query Id=`"0`" Path=`"security`">`r`n      &#x3C;Select Path=`"security`">`r`n       *[System[(EventID=4624)]]`r`n        and`r`n        (`r`n         *[EventData[Data[@Name='AuthenticationPackageName']!='Kerberos']]`r`n         and`r`n         *[EventData[Data[@Name='LmPackageName']!='NTLM V2']]`r`n       )`r`n      &#x3C;/Select>`r`n     &#x3C;/Query>`r`n    &#x3C;/QueryList>`r`n    '@`r`n    and also Get-WinEvent -FilterHashtable @{ LogName = 'Microsoft-Windows-NTLM/Operational' ; Id = 8001,8002 }            `r`n`r`nIf disabled: Lost logs information" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\services\Netlogon\Parameters" -ValueName "AuditNTLMInDomain" -Value 7 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\Lsa\MSV1_0" -ValueName "AuditReceivingNTLMTraffic" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\Lsa\MSV1_0" -ValueName "RestrictSendingNTLMTraffic" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe" -ValueName "AuditLevel" -Value 8 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Priv] Groups allowed to link new computers to the domain (PRIV_ENROLL_MACHINE)
###########################################################################################
New-ADGroup -Name PRIV_ENROLL_MACHINE -Path "OU=_CriticalUsers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -GroupCategory Security -GroupScope Universal
# Avoid machine added by users
Set-ADDomain (Get-ADDomain).distinguishedname -Replace @{"ms-ds-MachineAccountQuota"="0"}
# List all creators
Get-ADComputer -Filter * -Properties ms-DS-CreatorSID | Where-Object -FilterScript { $_."ms-DS-CreatorSID" -ne $Null } | Format-Table -AutoSize -Property Name,@{Label='User';Expression={(New-Object System.Security.Principal.SecurityIdentifier($_."mS-DS-CreatorSID".Value)).Translate([System.Security.Principal.NTAccount]).Value}}
dsacls.exe "OU=_AllComputers,$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" /G "PRIV_ENROLL_MACHINE:CC;computer"
# Check if MachineAccountQuota=0           
Get-ADObject ((Get-ADDomain).distinguishedname) -Properties ms-DS-MachineAccountQuota
### Add manualy the group or users allowed to add machine
### Group Policy Management Console (gpmc.msc) > Domain Controllers OU > Domain Controllers Policy > Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > User Rights Assigments > Add workstations to domain
### And https://www.appuntidallarete.com/you-have-exceeded-the-maximum-number-of-computer-accounts/
### https://www.moderndeployment.com/correct-domain-join-account-permissions/



###########################################################################################
# [1mm0rt41][Priv](GPO,Computer) Allow session for groups PRIV_INTERACT_WORKSTATION,PRIV_LOCAL_ADM
###########################################################################################
New-GPO -Name "[1mm0rt41][Priv](GPO,Computer) Allow session for groups PRIV_INTERACT_WORKSTATION,PRIV_LOCAL_ADM" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$inf =  "[Unicode]`r`n";
	$inf += "Unicode=yes`r`n";
	$audit = "";
	$inf += "[Privilege Rights]`r`n"
	$inf += "SeInteractiveLogonRight = "+"*S-1-5-32-544,"+"*"+((New-Object System.Security.Principal.NTAccount($env:USERDOMAIN, "PRIV_LOCAL_ADM")).Translate([System.Security.Principal.SecurityIdentifier]).Value)+","+"*"+((New-Object System.Security.Principal.NTAccount($env:USERDOMAIN, "PRIV_INTERACT_WORKSTATION")).Translate([System.Security.Principal.SecurityIdentifier]).Value)+","+"`r`n";
	$inf += "[Version]`r`n";
	$inf += 'signature="$CHICAGO$"'+"`r`n";
	$inf += "Revision=1`r`n";
	$gpoSecEditPath="$gpoBasePath\Machine\Microsoft\Windows NT\SecEdit"
	mkdir "$gpoSecEditPath" >$null
	$inf > "$gpoSecEditPath\GptTmpl.inf"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{827D319E-6EAC-11D2-A4EA-00C04F79F83A}{803E14A0-B4FB-11D0-A0D0-00A0C90F574B}]"};
	$_
}


###########################################################################################
# [1mm0rt41][Priv](GPO,Computer) Allow session for groups PRIV_INTERACT_LAPTOP,PRIV_LOCAL_ADM
###########################################################################################
New-GPO -Name "[1mm0rt41][Priv](GPO,Computer) Allow session for groups PRIV_INTERACT_LAPTOP,PRIV_LOCAL_ADM" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$inf =  "[Unicode]`r`n";
	$inf += "Unicode=yes`r`n";
	$audit = "";
	$inf += "[Privilege Rights]`r`n"
	$inf += "SeInteractiveLogonRight = "+"*S-1-5-32-544,"+"*"+((New-Object System.Security.Principal.NTAccount($env:USERDOMAIN, "PRIV_LOCAL_ADM")).Translate([System.Security.Principal.SecurityIdentifier]).Value)+","+"*"+((New-Object System.Security.Principal.NTAccount($env:USERDOMAIN, "PRIV_INTERACT_LAPTOP")).Translate([System.Security.Principal.SecurityIdentifier]).Value)+","+"`r`n";
	$inf += "[Version]`r`n";
	$inf += 'signature="$CHICAGO$"'+"`r`n";
	$inf += "Revision=1`r`n";
	$gpoSecEditPath="$gpoBasePath\Machine\Microsoft\Windows NT\SecEdit"
	mkdir "$gpoSecEditPath" >$null
	$inf > "$gpoSecEditPath\GptTmpl.inf"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{827D319E-6EAC-11D2-A4EA-00C04F79F83A}{803E14A0-B4FB-11D0-A0D0-00A0C90F574B}]"};
	$_
}


###########################################################################################
# [1mm0rt41][Priv](GPO,Computer) Allow RDP for group PRIV_REMOTE_TS
###########################################################################################
New-GPO -Name "[1mm0rt41][Priv](GPO,Computer) Allow RDP for group PRIV_REMOTE_TS" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$inf =  "[Unicode]`r`n";
	$inf += "Unicode=yes`r`n";
	$audit = "";
	$inf += "[Privilege Rights]`r`n"
	$inf += "SeInteractiveLogonRight = "+"*S-1-5-32-544,"+"*"+((New-Object System.Security.Principal.NTAccount($env:USERDOMAIN, "PRIV_REMOTE_TS")).Translate([System.Security.Principal.SecurityIdentifier]).Value)+","+"*"+((New-Object System.Security.Principal.NTAccount($env:USERDOMAIN, "PRIV_LOCAL_ADM")).Translate([System.Security.Principal.SecurityIdentifier]).Value)+","+"`r`n";
	$inf += "SeNetworkLogonRight = "+"*S-1-5-32-544,"+"*"+((New-Object System.Security.Principal.NTAccount($env:USERDOMAIN, "PRIV_REMOTE_TS")).Translate([System.Security.Principal.SecurityIdentifier]).Value)+","+"*"+((New-Object System.Security.Principal.NTAccount($env:USERDOMAIN, "PRIV_LOCAL_ADM")).Translate([System.Security.Principal.SecurityIdentifier]).Value)+","+"`r`n";
	$inf += "[Version]`r`n";
	$inf += 'signature="$CHICAGO$"'+"`r`n";
	$inf += "Revision=1`r`n";
	$gpoSecEditPath="$gpoBasePath\Machine\Microsoft\Windows NT\SecEdit"
	mkdir "$gpoSecEditPath" >$null
	$inf > "$gpoSecEditPath\GptTmpl.inf"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{827D319E-6EAC-11D2-A4EA-00C04F79F83A}{803E14A0-B4FB-11D0-A0D0-00A0C90F574B}]"};
	$_
}


###########################################################################################
# [1mm0rt41][Priv](GPO,Computer) AdminLocal for group PRIV_LOCAL_ADM
###########################################################################################
New-GPO -Name "[1mm0rt41][Priv](GPO,Computer) AdminLocal for group PRIV_LOCAL_ADM" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$inf =  "[Unicode]`r`n";
	$inf += "Unicode=yes`r`n";
	$audit = "";
	$inf += "[Group Membership]`r`n"
	$inf += "S-1-5-32-544__Memberof = "+"*"+((New-Object System.Security.Principal.NTAccount($env:USERDOMAIN, "")).Translate([System.Security.Principal.SecurityIdentifier]).Value)+","+"`r`n";
	$inf += "S-1-5-32-544__Members = "+"*"+((New-Object System.Security.Principal.NTAccount($env:USERDOMAIN, "PRIV_LOCAL_ADM")).Translate([System.Security.Principal.SecurityIdentifier]).Value)+","+"`r`n";
	$inf += "[Version]`r`n";
	$inf += 'signature="$CHICAGO$"'+"`r`n";
	$inf += "Revision=1`r`n";
	$gpoSecEditPath="$gpoBasePath\Machine\Microsoft\Windows NT\SecEdit"
	mkdir "$gpoSecEditPath" >$null
	$inf > "$gpoSecEditPath\GptTmpl.inf"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{827D319E-6EAC-11D2-A4EA-00C04F79F83A}{803E14A0-B4FB-11D0-A0D0-00A0C90F574B}]"};
	$_
}


###########################################################################################
# [1mm0rt41][Priv](GPO,Computer) DomainAdmin not allowed to connect
###########################################################################################
New-GPO -Name "[1mm0rt41][Priv](GPO,Computer) DomainAdmin not allowed to connect" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
$UID__DOMAIN = (Get-ADDomain).DomainSID.Value
	$inf =  "[Unicode]`r`n";
	$inf += "Unicode=yes`r`n";
	$audit = "";
	$inf += "[Privilege Rights]`r`n"
	$inf += "SeDenyNetworkLogonRight = "+"*S-1-5-32-546,"+"*$UID__DOMAIN-514,"+"*$UID__DOMAIN-501,"+"*$UID__DOMAIN-512,"+"*$UID__DOMAIN-519,"+"`r`n";
	$inf += "SeDenyInteractiveLogonRight = "+"*S-1-5-32-546,"+"*$UID__DOMAIN-514,"+"*$UID__DOMAIN-501,"+"*$UID__DOMAIN-512,"+"*$UID__DOMAIN-519,"+"`r`n";
	$inf += "SeDenyServiceLogonRight = "+"*S-1-5-32-546,"+"*$UID__DOMAIN-514,"+"*$UID__DOMAIN-501,"+"*$UID__DOMAIN-512,"+"*$UID__DOMAIN-519,"+"`r`n";
	$inf += "SeDenyBatchLogonRight = "+"*S-1-5-32-546,"+"*$UID__DOMAIN-514,"+"*$UID__DOMAIN-501,"+"*$UID__DOMAIN-512,"+"*$UID__DOMAIN-519,"+"`r`n";
	$inf += "SeDenyRemoteInteractiveLogonRight = "+"*S-1-5-32-546,"+"*$UID__DOMAIN-514,"+"*$UID__DOMAIN-501,"+"*$UID__DOMAIN-512,"+"*$UID__DOMAIN-519,"+"`r`n";
	$inf += "[Version]`r`n";
	$inf += 'signature="$CHICAGO$"'+"`r`n";
	$inf += "Revision=1`r`n";
	$gpoSecEditPath="$gpoBasePath\Machine\Microsoft\Windows NT\SecEdit"
	mkdir "$gpoSecEditPath" >$null
	$inf > "$gpoSecEditPath\GptTmpl.inf"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{827D319E-6EAC-11D2-A4EA-00C04F79F83A}{803E14A0-B4FB-11D0-A0D0-00A0C90F574B}]"};
	$_
}


###########################################################################################
# [1mm0rt41][Firewall] FW-TEST
###########################################################################################
New-GPO -Name "[1mm0rt41][Firewall] FW-TEST" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] toto" -Group "[GPO][1mm0rt41][Firewall] FW-TEST" -Action Allow -Direction Outbound -RemoteAdress $IPForInternet -Protocol TCP -Program "C:\Program Files\Internet Explorer\iexplore.exe" >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] toto" -Group "[GPO][1mm0rt41][Firewall] FW-TEST" -Action Allow -Direction Outbound -RemoteAdress $IPForInternet -Protocol TCP -Program "C:\Program Files (x86)\Internet Explorer\iexplore.exe" >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_
}


###########################################################################################
# [1mm0rt41][Firewall](GPO,Computer) ALLOW admin IP &#x26; ICMP &#x26; Audit
###########################################################################################
New-GPO -Name "[1mm0rt41][Firewall](GPO,Computer) ALLOW admin IP &#x26; ICMP &#x26; Audit" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
$IP_VPN_ADMIN = '10.10.10.0/24'
	$inf =  "[Unicode]`r`n";
	$inf += "Unicode=yes`r`n";
	$audit = "";
	$inf += "[Event Audit]`r`n"
	$audit += ',System,Audit Filtering Platform Connection,{0cce9226-69ae-11d9-bed3-505054503030},Success and Failure,,3'+"`r`n";
	$inf += "[Version]`r`n";
	$inf += 'signature="$CHICAGO$"'+"`r`n";
	$inf += "Revision=1`r`n";
	$gpoSecEditPath="$gpoBasePath\Machine\Microsoft\Windows NT\SecEdit"
	mkdir "$gpoSecEditPath" >$null
	$inf > "$gpoSecEditPath\GptTmpl.inf"
	$audit = $audit.Trim("`r`n")
	$gpoAuditPath="$gpoBasePath\Machine\Microsoft\Windows NT\Audit"
	mkdir "$gpoAuditPath" >$null
	"Machine Name,Policy Target,Subcategory,Subcategory GUID,Inclusion Setting,Exclusion Setting,Setting Value`r`n$audit" | Out-File -Encoding ASCII $gpoAuditPath\audit.csv
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{827D319E-6EAC-11D2-A4EA-00C04F79F83A}{803E14A0-B4FB-11D0-A0D0-00A0C90F574B}][{F3CCC681-B74C-4060-9F26-CD84525DCA2A}{0F3F3735-573D-9804-99E4-AB2A69BA5FD4}]"};
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow ICMP for *" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) ALLOW admin IP &#x26; ICMP &#x26; Audit" -Action Allow -Direction Outbound -Protocol ICMPv4 >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow IPSec" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) ALLOW admin IP &#x26; ICMP &#x26; Audit" -Action Allow -Direction Outbound -RemotePort (500,4500) -Protocol UDP >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow IPSec ESP" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) ALLOW admin IP &#x26; ICMP &#x26; Audit" -Action Allow -Direction Outbound -Protocol 50 >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow IPSec AH" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) ALLOW admin IP &#x26; ICMP &#x26; Audit" -Action Allow -Direction Outbound -Protocol 51 >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow * for admin" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) ALLOW admin IP &#x26; ICMP &#x26; Audit" -Action Allow -Direction Inbound -RemoteAddress $IP_VPN_ADMIN >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow ICMP for *" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) ALLOW admin IP &#x26; ICMP &#x26; Audit" -Action Allow -Direction Inbound -Protocol ICMPv4 >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow IPSec" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) ALLOW admin IP &#x26; ICMP &#x26; Audit" -Action Allow -Direction Inbound -LocalPort (500,4500) -Protocol UDP >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow IPSec ESP" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) ALLOW admin IP &#x26; ICMP &#x26; Audit" -Action Allow -Direction Inbound -Protocol 50 >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow IPSec AH" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) ALLOW admin IP &#x26; ICMP &#x26; Audit" -Action Allow -Direction Inbound -Protocol 51 >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_
}


###########################################################################################
# [1mm0rt41][Firewall](GPO,Computer) Open DHCP role for *
###########################################################################################
New-GPO -Name "[1mm0rt41][Firewall](GPO,Computer) Open DHCP role for *" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] DHCP for *" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) Open DHCP role for *" -Action Allow -Direction Inbound -LocalPort (67,68,2535) -Protocol UDP -RemoteAddress */ >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_
}


###########################################################################################
# [1mm0rt41][Firewall](GPO,Computer) Open FileServer role for *
###########################################################################################
New-GPO -Name "[1mm0rt41][Firewall](GPO,Computer) Open FileServer role for *" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] SMB for *" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) Open FileServer role for *" -Action Allow -Direction Inbound -LocalPort 445 -Protocol TCP -RemoteAddress */ >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_
}


###########################################################################################
# [1mm0rt41][Firewall](GPO,Computer) Open NPS role for *
###########################################################################################
New-GPO -Name "[1mm0rt41][Firewall](GPO,Computer) Open NPS role for *" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] NPS for *" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) Open NPS role for *" -Action Allow -Direction Inbound -LocalPort (1812,1813,1645,1646) -Protocol UDP -RemoteAddress */ >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_
}


###########################################################################################
# [1mm0rt41][Firewall](GPO,Computer) Open RDP role for *
###########################################################################################
New-GPO -Name "[1mm0rt41][Firewall](GPO,Computer) Open RDP role for *" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] RDP (udp) for *" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) Open RDP role for *" -Action Allow -Direction Inbound -LocalPort 3389 -Protocol UDP -RemoteAddress */ >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] RDP (tcp) for *" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) Open RDP role for *" -Action Allow -Direction Inbound -LocalPort 3389 -Protocol TCP -RemoteAddress */ >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_
}


###########################################################################################
# [1mm0rt41][Firewall](GPO,Computer) Open RCP role for *
###########################################################################################
New-GPO -Name "[1mm0rt41][Firewall](GPO,Computer) Open RCP role for *" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] RPC (Dynamic RPC) for *" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) Open RCP role for *" -Action Allow -Direction Inbound -LocalPort RPC -Protocol TCP -RemoteAddress */ >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] RDP (EMapper) for *" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) Open RCP role for *" -Action Allow -Direction Inbound -LocalPort RPCEPMap -Protocol TCP -RemoteAddress */ >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_
}


###########################################################################################
# [1mm0rt41][Firewall](GPO,Computer) DROP Default Inbound&#x26;Outbound &#x26; ENABLE FW
###########################################################################################
New-GPO -Name "[1mm0rt41][Firewall](GPO,Computer) DROP Default Inbound&#x26;Outbound &#x26; ENABLE FW" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	Set-NetFirewallProfile -PolicyStore $GpoSessionName -All -Enabled true -DefaultOutboundAction Block -DefaultInboundAction Block -AllowLocalFirewallRules false -AllowLocalIPsecRules true >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_
}


###########################################################################################
# [1mm0rt41][Firewall] Global configuration
###########################################################################################
New-GPO -Name "[1mm0rt41][Firewall] Global configuration" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	Set-NetFirewallProfile -PolicyStore $GpoSessionName -All -LogFileName "%windir%\system32\logfiles\pfirewall.log" -NotifyOnListen false -AllowUnicastResponseToMulticast true -LogAllowed true -LogBlocked true -LogIgnored false -LogMaxSizeKilobytes 32767 >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_
}


###########################################################################################
# [1mm0rt41][Firewall](GPO,Computer) Default rules for DC
###########################################################################################
New-GPO -Name "[1mm0rt41][Firewall](GPO,Computer) Default rules for DC" -Comment "##################################`r`n`r`n- Drop 445 outgoing connection to avoid Coercing`r`n- Allow full sync between DC" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
$domainControllerList = @((Get-AdTrust -Filter "*").Name)+@($env:USERDNSDOMAIN) | foreach {
    Resolve-DnsName -Type A -Name $_
} | foreach {
    $_.IP4Address
} | sort -unique
	$gpoDir="$gpoBasePath\Machine\Scripts";
	mkdir "$gpoDir" >$null
	@'
$domainControllerList = @((Get-AdTrust -Filter "*").Name)+@($env:USERDNSDOMAIN) | foreach {
    Resolve-DnsName -Type A -Name $_
} | foreach {
    $_.IP4Address
} | sort -unique
if( $domainControllerList.Count -gt 1 ){
    Write-Host "Found DC: $($domainControllerList -Join ',')"
    $scriptPath = split-path -parent $MyInvocation.MyCommand.Definition
    $scriptPath = $scriptPath.ToLower()
    $gpoId = ($scriptPath -Split 'policies\\')[1].Split('\\')[0]
    $prop = Get-ADObject -Filter "name -eq '$gpoId'" -Properties DisplayName
    $gpoDisplayName = $prop.DisplayName
    $gpoLdapPath = $prop.DistinguishedName
    $gpoDomainName = (Get-ADDomain).DNSRoot
    $GpoSessionName = ""
    Write-Host "Updating GPO named $gpoDisplayName"
    try{
        $GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore "$gpoDomainName\$gpoDisplayName"
    }catch{
        $GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
    }
    Get-NetFirewallRule -PolicyStore $GpoSessionName | ?{ $_.DisplayName.EndsWith("Allow * for DC") } | Set-NetFirewallRule -RemoteAddress $domainControllerList
}

'@ | Out-File -Encoding Ascii "$gpoDir\UpdateDcIpFw.ps1" 
	$gpoDir="$gpoBasePath\Machine\Preferences\ScheduledTasks";
	mkdir "$gpoDir" >$null
	$taskXml = ( @"
&#x3C;?xml version="1.0" encoding="utf-8"?>
&#x3C;ScheduledTasks clsid="{CC63F200-7309-4ba0-B154-A71CD118DBCC}">&#x3C;TaskV2 clsid="{D8896631-B747-47a7-84A6-C155337F3BC8}" name="[GPO] UpdateDCList" image="0" changed="$((Get-Date).ToString("yyyy-MM-dd HH:mm:ss"))" uid="{D98A502B-7563-4A3D-A4EA-5B4EE8E63364}" >&#x3C;Properties action="R" name="[GPO] UpdateDCList" runAs="S-1-5-18" logonType="S4U">&#x3C;Task version="1.2">&#x3C;RegistrationInfo>&#x3C;Author>$($env:USERDOMAIN)\$($env:USERNAME)&#x3C;/Author>&#x3C;Description>&#x3C;![CDATA[This task need to run with S-1-5-18 // GPO Id: $gpoId // GPO Name: $gpoName]]>&#x3C;/Description>&#x3C;/RegistrationInfo>&#x3C;Principals>&#x3C;Principal id="Author">&#x3C;UserId>S-1-5-18&#x3C;/UserId>&#x3C;LogonType>S4U&#x3C;/LogonType>&#x3C;RunLevel>HighestAvailable&#x3C;/RunLevel>&#x3C;/Principal>&#x3C;/Principals>&#x3C;Settings>&#x3C;IdleSettings>&#x3C;Duration>PT5M&#x3C;/Duration>&#x3C;WaitTimeout>PT1H&#x3C;/WaitTimeout>&#x3C;StopOnIdleEnd>false&#x3C;/StopOnIdleEnd>&#x3C;RestartOnIdle>false&#x3C;/RestartOnIdle>&#x3C;/IdleSettings>&#x3C;MultipleInstancesPolicy>IgnoreNew&#x3C;/MultipleInstancesPolicy>&#x3C;DisallowStartIfOnBatteries>false&#x3C;/DisallowStartIfOnBatteries>&#x3C;StopIfGoingOnBatteries>false&#x3C;/StopIfGoingOnBatteries>&#x3C;AllowHardTerminate>true&#x3C;/AllowHardTerminate>&#x3C;StartWhenAvailable>true&#x3C;/StartWhenAvailable>&#x3C;AllowStartOnDemand>true&#x3C;/AllowStartOnDemand>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;Hidden>false&#x3C;/Hidden>&#x3C;ExecutionTimeLimit>PT1H&#x3C;/ExecutionTimeLimit>&#x3C;Priority>7&#x3C;/Priority>&#x3C;RestartOnFailure>&#x3C;Interval>PT10M&#x3C;/Interval>&#x3C;Count>3&#x3C;/Count>&#x3C;/RestartOnFailure>&#x3C;RunOnlyIfNetworkAvailable>false&#x3C;/RunOnlyIfNetworkAvailable>&#x3C;/Settings>&#x3C;Actions Context="Author">&#x3C;Exec>&#x3C;Command>powershell&#x3C;/Command>&#x3C;Arguments>-exec bypass -nop -File "%MachineScript%\\UpdateDcIpFw.ps1"&#x3C;/Arguments>&#x3C;/Exec>&#x3C;/Actions>&#x3C;Triggers>&#x3C;CalendarTrigger>&#x3C;StartBoundary>$((Get-Date).AddDays(1).ToString("yyyy-MM-ddT{0:d2}:00:00" -f 12))&#x3C;/StartBoundary>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;ScheduleByDay>&#x3C;DaysInterval>1&#x3C;/DaysInterval>&#x3C;/ScheduleByDay>&#x3C;RandomDelay>PT60M&#x3C;/RandomDelay>&#x3C;/CalendarTrigger>&#x3C;/Triggers>&#x3C;/Task>&#x3C;/Properties>&#x3C;/TaskV2>
&#x3C;/ScheduledTasks>
"@ ).Trim()
	$taskXml = $taskXml.Replace("%MachineScriptUNC%","\\$($env:USERDNSDOMAIN)\SYSVOL\$($env:USERDNSDOMAIN)\Policies\$gpoId\Machine\Scripts");
	$taskXml = $taskXml.Replace("%MachineScript%","$gpoBasePath\Machine\Scripts");
	$taskXml | Out-File -Encoding ASCII "$gpoDir\ScheduledTasks.xml"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{00000000-0000-0000-0000-000000000000}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}][{AADCED64-746C-4633-A97C-D61349046527}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}]"};
	$GpoSessionName = ""
	try{
		$GpoSessionName = Open-NetGPO -ErrorAction Stop -PolicyStore ("{0}\{1}" -f $_.DomainName,$_.DisplayName)
	}catch{
		$GpoSessionName = "LDAP://$($env:COMPUTERNAME).$gpoDomainName/$gpoLdapPath"
	}
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow * TCP except 445" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) Default rules for DC" -Action Allow -Direction Outbound -RemotePort ('0-444','446-65535') -Protocol TCP >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow * UDP except 445" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) Default rules for DC" -Action Allow -Direction Outbound -RemotePort ('0-444','446-65535') -Protocol UDP >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow * for DC" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) Default rules for DC" -Action Allow -Direction Outbound -LocalAddress $domainControllerList >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow * TCP except 3389" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) Default rules for DC" -Action Allow -Direction Inbound -LocalPort ('0-3388','3390-65535') -Protocol TCP >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow * UDP except 3389" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) Default rules for DC" -Action Allow -Direction Inbound -LocalPort ('0-3388','3390-65535') -Protocol UDP >$null
	New-NetFirewallRule -Enabled True -Profile Any -ErrorAction Continue -PolicyStore $GpoSessionName -DisplayName "[GPO] Allow * for DC" -Group "[GPO][1mm0rt41][Firewall](GPO,Computer) Default rules for DC" -Action Allow -Direction Inbound -RemoteAddress $domainControllerList >$null
	#Save-NetGPO -GPOSession $GpoSessionName -ErrorAction SilentlyContinue >$null
	$_
}


###########################################################################################
# [1mm0rt41][NiceToHave](GPO,User) ExplorerUI
###########################################################################################
New-GPO -Name "[1mm0rt41][NiceToHave](GPO,User) ExplorerUI" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced" -ValueName "HideFileExt" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced" -ValueName "LaunchTo" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced" -ValueName "HidePeopleBar" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\People" -ValueName "PeopleBand" -Value 0 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][NiceToHave](GPO,Computer) Unlimited Path length
###########################################################################################
New-GPO -Name "[1mm0rt41][NiceToHave](GPO,Computer) Unlimited Path length" -Comment "##################################`r`n`r`nAllow long path in Windows, usefull for SMB share for users.`r`n`r`nSide effect: None`r`nIf disabled: Can block access to some ressource on SMB that have long path" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Control\FileSystem" -ValueName "LongPathsEnabled" -Value 1 -Type DWord >$null
	$_
} | New-GPLink -target "$(([ADSI]'LDAP://RootDSE').defaultNamingContext.Value)" -LinkEnabled Yes


###########################################################################################
# [1mm0rt41][NiceToHave](GPO,Computer) DNS Suffix
###########################################################################################
New-GPO -Name "[1mm0rt41][NiceToHave](GPO,Computer) DNS Suffix" -Comment "##################################`r`n`r`nThe typical name resolution process for Microsoft Windows 2000 uses the primary DNS suffix and any connection-specific DNS suffixes. If these suffixes do not work, the devolution of the primary DNS suffix is attempted by the name resolution process.`r`n`r`nWhen a domain suffix search list is configured on a client, only that list is used. The primary DNS suffix and any connection-specific DNS suffixes are not used, nor is the devolution of the primary suffix attempted. The domain suffix search list is an administrative override of all standard Domain Name Resolver (DNR) look-up mechanisms.`r`n`r`nSide effect: None`r`nIf disabled: Can block access to some ressource that doesn't known AD Suffix" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters" -ValueName "SearchList" -Value "suffix-dns.mycorp.local,suffix2.corp.lo" -Type String >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\DNSClient" -ValueName "SearchList" -Value "suffix-dns.mycorp.local,suffix2.corp.lo" -Type String >$null
	$_
}


###########################################################################################
# [1mm0rt41][Azure](GPO&#x26;Pref,Computer) Deny AzureAD autojoin via Teams
###########################################################################################
New-GPO -Name "[1mm0rt41][Azure](GPO&#x26;Pref,Computer) Deny AzureAD autojoin via Teams" -Comment "##################################`r`n`r`nDeny Teams&#x26;co to autojoin device to AzureAD`r`n`r`nTo detect if the computer/user is AAD auto join:`r`n1) Check:`r`n    dsregcmd.exe /debug /status`r`n2) Unenroll:`r`n    dsregcmd.exe /debug /leave (as SYSTEM)`r`n3) Reinstall `"Microsoft.AAD.BrokerPlugin`" in the context of the user session`r`n    Add-AppxPackage -Register `"C:\Windows\SystemApps\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\Appxmanifest.xml`" -DisableDevelopmentMode -ForceApplicationShutdown`r`n4) Clear all `"Microsoft.AAD.BrokerPlugin`" cache (as local ADMIN)`r`n    Get-ItemProperty -Path `"C:\Users\*\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin*`" | %{ rmdir /q /s `"`$(`$_.FullName)`" } | Out-Null `r`n`r`nWARNING! Do not disable DisableAADWAM or EnableADAL, it will kill the MFA on Azure, all account with WFA will not work anymore`r`n`r`nTo avoid device to auto join in AAD, configure AAD. In AAD/Microsoft-Entra go to Identity > Devices > All devices > Device settings`r`n    - `"Users may join devices to Microsoft Entra ID`": SELECTED (only dedicated user)`r`n    - `"Additional local administrators on Microsoft Entra joined devices`": NONE`r`n    - `"Require multifactor authentication (MFA) to join devices`": YES`r`n`r`nSide effect: Take the computer out of AAD but not from AD`r`nIf disabled: Will allow Teams &#x26; co to auto-enroll the device. Will not autojoin to AAD`r`nDoc: https://techpress.net/how-to-unjoin-a-hybrid-azure-ad-join-device/`r`nDoc: To fix the unpredictable freez of Office apps (Outlook/Teams/OneDrive): http://aldrid.ge/W10MU-AAD-Auth" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$gpoDir="$gpoBasePath\Machine\Preferences\ScheduledTasks";
	mkdir "$gpoDir" >$null
	$taskXml = ( @"
&#x3C;?xml version="1.0" encoding="utf-8"?>
&#x3C;ScheduledTasks clsid="{CC63F200-7309-4ba0-B154-A71CD118DBCC}">&#x3C;ImmediateTaskV2 clsid="{9756B581-76EC-4169-9AFC-0CA8D43ADB5F}" name="[GPO] Unjoin AAD" image="0" changed="$((Get-Date).ToString("yyyy-MM-dd HH:mm:ss"))" uid="{D98A502B-7563-4A3D-A4EA-5B4EE8E63364}" userContext="0" removePolicy="0">&#x3C;Properties action="R" name="[GPO] Unjoin AAD" runAs="S-1-5-18" logonType="S4U">&#x3C;Task version="1.2">&#x3C;RegistrationInfo>&#x3C;Author>$($env:USERDOMAIN)\$($env:USERNAME)&#x3C;/Author>&#x3C;Description>&#x3C;![CDATA[This task need to run with S-1-5-18 // GPO Id: $gpoId // GPO Name: $gpoName]]>&#x3C;/Description>&#x3C;/RegistrationInfo>&#x3C;Principals>&#x3C;Principal id="Author">&#x3C;UserId>S-1-5-18&#x3C;/UserId>&#x3C;LogonType>S4U&#x3C;/LogonType>&#x3C;RunLevel>HighestAvailable&#x3C;/RunLevel>&#x3C;/Principal>&#x3C;/Principals>&#x3C;Settings>&#x3C;IdleSettings>&#x3C;Duration>PT5M&#x3C;/Duration>&#x3C;WaitTimeout>PT1H&#x3C;/WaitTimeout>&#x3C;StopOnIdleEnd>false&#x3C;/StopOnIdleEnd>&#x3C;RestartOnIdle>false&#x3C;/RestartOnIdle>&#x3C;/IdleSettings>&#x3C;MultipleInstancesPolicy>IgnoreNew&#x3C;/MultipleInstancesPolicy>&#x3C;DisallowStartIfOnBatteries>true&#x3C;/DisallowStartIfOnBatteries>&#x3C;StopIfGoingOnBatteries>true&#x3C;/StopIfGoingOnBatteries>&#x3C;AllowHardTerminate>true&#x3C;/AllowHardTerminate>&#x3C;StartWhenAvailable>true&#x3C;/StartWhenAvailable>&#x3C;AllowStartOnDemand>true&#x3C;/AllowStartOnDemand>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;Hidden>false&#x3C;/Hidden>&#x3C;ExecutionTimeLimit>PT1H&#x3C;/ExecutionTimeLimit>&#x3C;Priority>7&#x3C;/Priority>&#x3C;DeleteExpiredTaskAfter>PT0S&#x3C;/DeleteExpiredTaskAfter>&#x3C;RestartOnFailure>&#x3C;Interval>PT10M&#x3C;/Interval>&#x3C;Count>3&#x3C;/Count>&#x3C;/RestartOnFailure>&#x3C;RunOnlyIfNetworkAvailable>true&#x3C;/RunOnlyIfNetworkAvailable>&#x3C;/Settings>&#x3C;Actions Context="Author">&#x3C;Exec>&#x3C;Command>dsregcmd&#x3C;/Command>&#x3C;Arguments>/debug /leave&#x3C;/Arguments>&#x3C;/Exec>&#x3C;/Actions>&#x3C;Triggers>&#x3C;TimeTrigger>&#x3C;StartBoundary>%LocalTimeXmlEx%&#x3C;/StartBoundary>&#x3C;EndBoundary>%LocalTimeXmlEx%&#x3C;/EndBoundary>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;/TimeTrigger>&#x3C;/Triggers>&#x3C;/Task>&#x3C;/Properties>&#x3C;/ImmediateTaskV2>
&#x3C;/ScheduledTasks>
"@ ).Trim()
	$taskXml = $taskXml.Replace("%MachineScriptUNC%","\\$($env:USERDNSDOMAIN)\SYSVOL\$($env:USERDNSDOMAIN)\Policies\$gpoId\Machine\Scripts");
	$taskXml = $taskXml.Replace("%MachineScript%","$gpoBasePath\Machine\Scripts");
	$taskXml | Out-File -Encoding ASCII "$gpoDir\ScheduledTasks.xml"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{00000000-0000-0000-0000-000000000000}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}][{AADCED64-746C-4633-A97C-D61349046527}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}]"};
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WorkplaceJoin" -ValueName "BlockAADWorkplaceJoin" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SOFTWARE\Policies\Microsoft\Windows\WorkplaceJoin" -ValueName "autoWorkplaceJoin" -Value 0 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Audit](GPO,Computer) Audit LDAP SASL
###########################################################################################
New-GPO -Name "[1mm0rt41][Audit](GPO,Computer) Audit LDAP SASL" -Comment "##################################`r`n`r`nLog missing LDAP SASL.`r`n=> Event ID of 2889 in the Directory Service log.`r`n`r`nMonitoring for LDAP Binding without Channel Binding.`r`n=> Event ID 3039 in the Directory Service event log.`r`n`r`nPump the size of Directory Service log to 32MB. The default size is 1MB`r`n`r`n`$Hours = 24`r`n`$DCs = Get-ADDomainController -filter *`r`n`$InsecureLDAPBinds = @()`r`nForEach (`$DC in `$DCs) {`r`n`$Events = Get-WinEvent -ComputerName `$DC.Hostname -FilterHashtable @{Logname='Directory Service';Id=2889; StartTime=(Get-Date).AddHours('-`$Hours')}`r`nForEach (`$Event in `$Events) {`r`n   `$eventXML = [xml]`$Event.ToXml()`r`n   `$Client = (`$eventXML.event.EventData.Data[0])`r`n   `$IPAddress = `$Client.SubString(0,`$Client.LastIndexOf(':'))`r`n   `$User = `$eventXML.event.EventData.Data[1]`r`n   Switch (`$eventXML.event.EventData.Data[2])`r`n      {`r`n      0 {`$BindType = 'Unsigned'}`r`n      1 {`$BindType = 'Simple'}`r`n      }`r`n   `$Row = '' | select IPAddress,User,BindType`r`n   `$Row.IPAddress = `$IPAddress`r`n   `$Row.User = `$User`r`n   `$Row.BindType = `$BindType`r`n   `$InsecureLDAPBinds += `$Row`r`n   }`r`n}`r`n`$InsecureLDAPBinds | Out-Gridview" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Diagnostics" -ValueName "16 LDAP Interface Events" -Value 2 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\EventLog\Directory Service" -ValueName "MaxSize" -Value 33685504 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\EventLog\Directory Service" -ValueName "MaxSizeUpper" -Value 0 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Policies\Microsoft\FeatureManagement\Overrides" -ValueName "1775223437" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Policies\Microsoft\FeatureManagement\Overrides" -ValueName "2654580365" -Value 1 -Type DWord >$null
	$_ | Set-GPRegistryValue -Key "HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters" -ValueName "LdapEnforceChannelBinding" -Value 1 -Type DWord >$null
	$_
}


###########################################################################################
# [1mm0rt41][Audit] Syslog
###########################################################################################
New-GPO -Name "[1mm0rt41][Audit] Syslog" | %{
	$gpoId="{{{0}}}" -f $_.Id.ToString();
	$gpoName=$_.DisplayName
	$gpoDomainName=$_.DomainName
	$gpoLdapPath=$_.Path
	$gpoBasePath="C:\Windows\SYSVOL\domain\Policies\$gpoId"
	$gpoDir="$gpoBasePath\Machine\Preferences\ScheduledTasks";
	mkdir "$gpoDir" >$null
	$taskXml = ( @"
&#x3C;?xml version="1.0" encoding="utf-8"?>
&#x3C;ScheduledTasks clsid="{CC63F200-7309-4ba0-B154-A71CD118DBCC}">&#x3C;TaskV2 clsid="{D8896631-B747-47a7-84A6-C155337F3BC8}" name="[GPO] Syslog" image="0" changed="$((Get-Date).ToString("yyyy-MM-dd HH:mm:ss"))" uid="{D98A502B-7563-4A3D-A4EA-5B4EE8E63364}" >&#x3C;Properties action="R" name="[GPO] Syslog" runAs="S-1-5-18" logonType="S4U">&#x3C;Task version="1.2">&#x3C;RegistrationInfo>&#x3C;Author>$($env:USERDOMAIN)\$($env:USERNAME)&#x3C;/Author>&#x3C;Description>&#x3C;![CDATA[This task need to run with S-1-5-18 // GPO Id: $gpoId // GPO Name: $gpoName]]>&#x3C;/Description>&#x3C;/RegistrationInfo>&#x3C;Principals>&#x3C;Principal id="Author">&#x3C;UserId>S-1-5-18&#x3C;/UserId>&#x3C;LogonType>S4U&#x3C;/LogonType>&#x3C;RunLevel>HighestAvailable&#x3C;/RunLevel>&#x3C;/Principal>&#x3C;/Principals>&#x3C;Settings>&#x3C;IdleSettings>&#x3C;Duration>PT5M&#x3C;/Duration>&#x3C;WaitTimeout>PT1H&#x3C;/WaitTimeout>&#x3C;StopOnIdleEnd>false&#x3C;/StopOnIdleEnd>&#x3C;RestartOnIdle>false&#x3C;/RestartOnIdle>&#x3C;/IdleSettings>&#x3C;MultipleInstancesPolicy>IgnoreNew&#x3C;/MultipleInstancesPolicy>&#x3C;DisallowStartIfOnBatteries>false&#x3C;/DisallowStartIfOnBatteries>&#x3C;StopIfGoingOnBatteries>false&#x3C;/StopIfGoingOnBatteries>&#x3C;AllowHardTerminate>true&#x3C;/AllowHardTerminate>&#x3C;StartWhenAvailable>true&#x3C;/StartWhenAvailable>&#x3C;AllowStartOnDemand>true&#x3C;/AllowStartOnDemand>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;Hidden>false&#x3C;/Hidden>&#x3C;ExecutionTimeLimit>PT1H&#x3C;/ExecutionTimeLimit>&#x3C;Priority>7&#x3C;/Priority>&#x3C;RestartOnFailure>&#x3C;Interval>PT10M&#x3C;/Interval>&#x3C;Count>3&#x3C;/Count>&#x3C;/RestartOnFailure>&#x3C;RunOnlyIfNetworkAvailable>true&#x3C;/RunOnlyIfNetworkAvailable>&#x3C;/Settings>&#x3C;Actions Context="Author">&#x3C;Exec>&#x3C;Command>powershell&#x3C;/Command>&#x3C;Arguments>-exec bypass -nop -Command \\dc01.corp.lo\sysvol\dc01.corp.lo\scripts\logger.ps1&#x3C;/Arguments>&#x3C;/Exec>&#x3C;/Actions>&#x3C;Triggers>&#x3C;CalendarTrigger>&#x3C;StartBoundary>$((Get-Date).AddDays(1).ToString("yyyy-MM-ddT{0:d2}:00:00" -f 9))&#x3C;/StartBoundary>&#x3C;Enabled>true&#x3C;/Enabled>&#x3C;ScheduleByDay>&#x3C;DaysInterval>1&#x3C;/DaysInterval>&#x3C;/ScheduleByDay>&#x3C;RandomDelay>PT10M&#x3C;/RandomDelay>&#x3C;/CalendarTrigger>&#x3C;/Triggers>&#x3C;/Task>&#x3C;/Properties>&#x3C;/TaskV2>
&#x3C;/ScheduledTasks>
"@ ).Trim()
	$taskXml = $taskXml.Replace("%MachineScriptUNC%","\\$($env:USERDNSDOMAIN)\SYSVOL\$($env:USERDNSDOMAIN)\Policies\$gpoId\Machine\Scripts");
	$taskXml = $taskXml.Replace("%MachineScript%","$gpoBasePath\Machine\Scripts");
	$taskXml | Out-File -Encoding ASCII "$gpoDir\ScheduledTasks.xml"
	Get-AdObject -Filter "(objectClass -eq 'groupPolicyContainer') -and (name -eq '$gpoId')" | Set-ADObject -Replace @{gPCMachineExtensionNames="[{00000000-0000-0000-0000-000000000000}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}][{AADCED64-746C-4633-A97C-D61349046527}{CAB54552-DEEA-4691-817E-ED4A4D1AFC72}]"};
	$_
}

</code></pre>
