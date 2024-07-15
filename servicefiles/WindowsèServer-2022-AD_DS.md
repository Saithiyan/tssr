# Windows Server 2022 - Active Directory :

sconfig
15 pour sortir / mode cmd
start PowerShell

pour faire des execution de script, il faut dire qu'on approuve la politique etc
Get-ExecutionPolicy  Set-ExecutionPolicy

Lister les interfaces réseaux
Voici la commande pour lister les interfaces réseaux à partir de PowerShell.
Cela permet de récupérer le nom, la description, l'index if, le statut, l'adresse Mac et la vitesse.
- Get-NetAdapter

- Get-NetIPAddress
- Get-NetIPInterface : de verification
- Get-NetIPConfiguration

Affectation :
- New-NetIPAddress -InterfaceIndex 5 -IPAdress 192.168.100.250 -PreFixLenght 24 -DefaultGateway 192.168.100.254

DNS ajout:
- Set-DnsClientDerverAddress -InterfaceInfex 5 -ServerAddresses 8.8.8.8

-----------
- Get-NetRoute : voir les route

pour reaffecter les routes:
- New-netroute -interfaceIndex N -nexthop "192.168.100.254" -destinationprefix 0.0.0.0/0


-----Verifier ipV6
- Get-NetAdapterBinding
- Disable-NetAdapterBinding -Name nom -ComponentID ms_tcpip6 -PassThru
ex:
Disable-NetAdapterBinding -Name Ethernet -ComponentID ms_tcpip6 -PassThru


-------functionnalités
- Get-WindowsFeatures
- Instaall-WindowsFeature -Name DNS -InclurdeManagementTools ( -InclurdeManagementToolsles outils de managment pas besoin car outils RSAT sur le client)

Créer une domaine:
zone directe
- Add-DnsServerPrimaryZone -Name "chat.on" -Zonefile "chat.dns"
- Get-DnsServerZone
zone inverse
- Add-DnsServerPrimaryZone -NetworkId 192.168.100.0/24 -zoneFile "100.168.192.in-addr.arpa"

- Add-DnsServerResourceRecordA -Name "srv1" -ZoneName "chat.on" -IpAddress "192.168.100.250"
- Add-DnsServerResourceRecordPtr -name "250" -ZoneName "100.168.192.in-addr.arpa" -PtrDomainName "srv1.chat.on"

- Add-DnsServerForwarder -IPAddress 8.8.8.8,1.1.1.1,1.0.0.1

Pour mettre en memoire / recharger la zone 
- Sync-DnsServerZone name "chat.on" -passthru

- Get-DnsServerResourceRecord -ZoneName "chat.on"


-----DHCP-------
- Install-WindowsFeature -Name dhcp
- Set-DhcpServerv4DnsSetting -ComputerName "srv1.chat.on" -DynamicUpdates "Always" -DeleteDnsRROnLeaseExpiry $true

- Add-DhcpServerv4Scope -Name "etendue" -StartRange 192.168.100.10 -EndRange 192.18.100.100 -SubnetMask 255.255.255.0 -Description "Plage pour le LAN"
- Set-DhcpServerv4Optionvalue -ScopeId 192.168.100.0 -Option 3 -Value 192.168.100.254
- Set-DhcpServerv4Optionvalue -ScopeId 192.168.100.0 -Option 6 -Value 192.168.100.250
- Set-DhcpServerv4Optionvalue -ScopeId 192.168.100.0 -Option 15 -Value "chat.on"
- Set-DhcpServerv4Scope -ScopeId 192.168.100.0 -State active
pour verifier: - Get-DhcpServerv4Scope

Terminer la conf DHCP:
- New-ADGroup -Name "admins DHCP" -DisplayName "adins DHCP" -Decription "Les administrateur du DHCP" -GroupCategory Security -GroupScope DomainLocal -Path "CN=Users,DC=chat,DC=lab"
Autoriser le server DHCP dans AD DS
- Add-DhcpServerInDC -DnsName "srv1.chat.on" -IPAddres 192.168.100.250




-----Pare-feu------
- New-NetFirewallRule -DisplayName "PING entrant" -Drection Inbund -Protocol ICMPv4 -IcmpType 8 -Action Allow



------AD DS----------
- Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
On peut ajouter les outils d'admin : avec l'option -IncludeManagementTools
- Import-Module ADDSDeployment : c'est pour povoir pousuivre l'operation en powershell, lib qui permet de gérer ADDS.
- Get-Module : pour voir si le module est bien installer
---
Foret - Domain
- $password = ConverTo-SecureString 0Poseidon -AsPlaintText -Force
- Install-ADDSForest -DomainName chat.on -CreateDnsDelegation:$false -InstalDNS:$true -DomainNetbiosName CHAT -DomainMode Win2012R2 -ForestMode Win2012R2 -SafeModeAdministratorPassword $password -Confirm -Force

[Install-ADDSForest -DomainName jeep.labo -CreateDnsDelegation:$false -InstallDns:$true -DomainNetBIOSName JEEP  -DomainMode Win2012r2 -ForestMode Win2012r2 -Confirm -SafeModeAdministratorPassword $password -force]


-----winrm----

sur server:
-winrm get winrm/config/client
- Enable-PsRemoting
sur client:
- Enter-PSSession -ComputerName "srv-1.chat.on" -Credential administrateur@chat.on
