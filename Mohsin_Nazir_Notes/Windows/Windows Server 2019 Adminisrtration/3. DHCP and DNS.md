# DHCP 

## 1. DHCP Overview

- Automates management of IP configuration on clients and devices
- DHCP process
	![[DHCP Process.png]]
- DHCP lease renewal is attempted at:
	- Startup
	- 50% of lease time  --> day 4/8
	- 87.5% of lease time. day 7/8
	- 100% lease expire --> 8 days --> redo the DHCP process

## 2. Analyzing DHCP process using wireshark

- Start the wireshark --> select the network adapter --> Start Capture

- **Release the IP address.**
	- Windows  --> `ipconfig /release`
	- Linux --> `sudo dhclient -r`

- **After release command renew .**
	- Windows --> `ipconfig renew`
	- Linux --> `sudo dhclient` 

- **Stop the capture and analyze in Wireshark (Filter Protocol)**
	- Discover  | Client --> Server
	- Offer        | Client <-- Server
	- Request   | Client -->Server
	- Ack           | Client <--Server
## 3. Install and configure the DHCP role

1. Install
2. Addsecuirty groups
3. Add to the domains (authentications)
4. Scope

- **Multiple Ways**
	1. Windows admin center --> roles and features
	2. Server Manager --> add roles and features.
	3. Powershell command 
		- `Get-WindowFeature` --> to get the name.
		- `Add-WindowsFeature DHCP -IncludeManagementTools`
		- `netsh dhcp add securitygroup` --> add security groups 

Note: to manage a DHCP server by using window server admin center must install the DHCP powershell tools.


- **DHCP Local Security Groups**
	- DHCP administrators --> Full control over the DHCP service
	- DHCP users --> Read only access

- **To create the DHCP local security groups**
	- Server managers --> Post install configuration wizard
	- PowerShell command

```
Add-DhcpServerSecurityGroup - Computer DhcpServerName
```
## 4. DHCP Authorization and Scope

#### DHCP AD DS Authorization

- DHCP server must be authorized in the AD DS to lease IP addresses.
- This will prevent from rogue IP addresses.
- PowerShell command to do this 

```
Add-DHCPServerinDC <name or IP address of the DHCP server> (svr1.socintern.com)
``` 

- A standalone sever with DHCP can not lease IP address if an authorized DHCP is detected.

- Non-windows DHCP server (router / switches etc) and devices will function regardless of the authorization.
#### Properties of a DHCP scope

- Name (Mandatory ) --> Location
- IP address range (mandatory)
- Subnet mask (mandatory)
- Exclusion --> IP address not to be leased
- Delay --> for back DHCP server.
- Lease Duration --> default 8 days
- Option 
	- 3 --> Router(Default Gateway)
	-  6 --> DNS servers
	- 15 --> DNS domain name
- Activation --> DHCP server lease addresses or not.

## 5. DHCP Scope Practical

#### With Server Manager

- Tools --> DHCP
- Green Tick represent authorization
- Red Down arrow unauthorized.
- Under DHCP --> rts.dc1.scointern.com
- Right click on IPv4 --> New Scope
- New Scope Wizard Opens
	- Give Name
	- Set Range of IP addresses
	- Set exclusion if any
	- Set lease duration
	- Configure DHCP Option --> Yes
	- Give (Router) Default Gateway 
	- DNS Name and DNS Server
	- WINS server --> next (leave it not used to today)
	- Activate Scope
	- Finish.
- Now can verify from SVR1 that it really works
	- change the static IP to DHCP
	- check IP config
	- check in the DC1 dhcp its shows one IP leased.
	- reset static IP.
	- ![[DHCP Practical Verification.png]]

#### With PowerShell

**Add Scope**

- Start PowerShell ISE as administrator
- Use the right pane commands module for dhcp server
- choose the commend `Add-DhcpServerv4Scope` --> a form will appear fill the form and copy or insert.
- Benefits --> use it in the test environment , save and runs in the production environment.


**Remove Scope**

- get the scope id with commands

```
Get-DhcpServer4Scope
```

- scope id is the network id
```
Remove-DhcpServerv4Scope -ScopeId 192.168.1.0
```
## 6. DHCP High Availability

- **Split Scopes**
	- two dhcp servers that care configured with non-overlapping scopes

- **DHCP failover**
	- Scopes are replicated from one DHCP to another DHCP partner.
	- Preferred option
	- Failover modes
		- Load Balance  --> Active/ Active --> Both servers run.
		- Host Standby --> Active / Passive --> one working other waiting.
## 7. Configuring DHCP Failover

- Sever manager --> Tools --> DHCP.
- Navigate to scopes under IPv4.
- Right click scope --> configure fail-over --> Wizard opens.
	- Select the scopes list from list
	- select partner server.
	- select the relation mode (Load Balance or Hot Standby)
	- Finish
- Deconfigure scope form one server it will also delete the scope from another as well.


# DNS

## 1. DNS Components

- **DNS Domain Names**
	- Portion of the DNS namespace
	- Can be public / private
```
<domainname>.<top level domian name> (socintern.com)
```

- **DNS Serves**
	- respond to the name resolution request.
	- stores resources record locally in a database on DNS servers.

- **DNS Zones and resource record**
	- Zone is the local copy of DNS namespace on DNS servers.
	- resources records are created and stored in a zone.

- **DNS resolvers**
	- request DNS information from DNS serveres. (clients)
	- cache results

## 2. DNS Resolution Process

- **Check DNS cache**
```
ipconfig/displaydns
```

- **Flush DNS cache**
```
ipconfig/flushdns
```

- **Recursive query**
	- help me out with this  do you know this , if not then you know do you who knows it, query that and ask that servers.

- **Iterative Query**
	- can you help or point out to someone that can

- **DNS Process Diagram**
![[DNS Resolution Process.png]]

## 3. DNS Records

- Forward Lookup zones in include:
	- **Host (A)** --> Name to >> IPv4
	- **Host (AAAA)** --> Name to >> IPv6
	- **Alisa(CNAME)** --> made-up name (www.googl.com) >> IP address 
	- **Service Location (SRV)** --> resolve a service >> IP address
	- **Pointer(PTR)** --> Reverse lookup >> IP name.
#### Create Records in DNS

- **Manual Methods**
	- Windows Admin Center
	- DNS Manager
	- Windows Powershell

- **Dynamic Creation**
	- Clients register name and IP address in a zone.

## 4. DNS Zones

- **Primary Zones**
	- Authoritative for a portion of a DNS namespace
	- Are where the resource records are created

- **Secondary Zones**
	- Read-only copies of primary zones

- **Stub Zones**
	- contain only the records required to locate and contain with name servers(DNS)
	- roles --> DNS of same compamy (with in the company)

- **Active Directory-integrated zones**
	- can only reside on domain controllers
	- replicates with active directory
	- encrypted 
	- secure dynamic updates (options)
		- record for domain integrated device as well.

## 5. Installing DNS Role

- **Through Server Managers**
	- Use Manage --> Add roles and select
	- Follow the wizard --> Add DNS in the service and Finish

- **Through PowerShell**
	- Get windows feature name first
	```
	get-windowsfeature|more
	```
	- Now install the feature with commands

```
	install-windowsfeature DNS -IncludemanagementTools
```

## 6. DNS Zones Creation 

#### Primary Zones Creation

- Server Manager --> Tools --> DNS
- In DNS managers --> DNS --> server name (RTS-DC1) --> Forward lookup Zones --> New Zone -->Wizard Opens.
- Select the primary zone radio button (unchecked the Active directory check)
- Give the zone name in format (name.damin) Islamabad.local
- Create a new or use an old for updating the from old dns.
- Allow both secure and non secure dynamic updates.
- Finish


#### Secondary Zones Creation


- Can add the server from DNS manager and create or login to other server and create.
- Server Manager --> Tools --> DNS
- In DNS managers --> DNS --> server name (RTS-DC1) --> Forward lookup Zones --> New Zone -->Wizard Opens.
- Select the secondary zone radio button ( Active directory check is grayed out because it is not a DC)
- give the same name as primary
- give the IP or name of master address.
- finish

#### Stub Zones Creation

- same process just select the stub zone radio button.
- same process as secondary , it just don't replicate the all records it just keep the Name Servers records
#### AD Integrated Zones Creation

- Server Manager --> Tools --> DNS
- In DNS managers --> DNS --> server name (RTS-DC1) --> Forward lookup Zones --> New Zone -->Wizard Opens.
- Select the primary zone radio button (checked the Active directory check)
- Select Replication option to forest or domian.
- Give name
- Allow only secure dynamic updates.
- can change zone type or replication or dynamic updates from property

## 7. Reverse Lookup Zones

- Resolves IP address to name 
- Pointer record (PTR)
#### Creation of Reverse Lookup

- Server Manager --> Tools --> DNS
- In DNS managers --> DNS --> server name (RTS-DC1) --> Reverse lookup Zones --> New Zone -->Wizard Opens.
- Select Zone type
- If AD integrated then replication option.
- Select IPv4 or IPv6
- Give the network ID like (192.168.1)
- Select dynamic updates options
- Finish
- Have to create a reverse lookup for each sub-net on network (like 192.168.2 / 192.168.3 etc)

- **PTR Records**
	- Right click --> New pointer record
	- Give IP address
	- Browse for host name --> Ok.
	- PTR records created


## 8. DNS Forwarding and Root Hints

**Forwarder**
 - Receive requests, and forward request for zones for which it is not authoritative.
 - common for external name resolution

**Conditional Forwarder**
- Forward request for a specific domain names.
- typical between partners and trusted organization.

**Stub Zones Similarity** 
- Have a similar role to conditional forwarders.
- Stub zones are used with in the same company
- Stub zones configuration on both DNS Servers.

![[DNS Forwarding.png]]

**Roots Hints**
- List of 13 public DNS root servers
- If Root zones created by using name (.) only and it will removes all the roots hints and forwarders. --> anythings outside defined zones will not be resolved.


## 9.Creating Forwarders 

**Forwarders**

- Server Manager --> Tools --> DNS
- In DNS managers --> DNS --> server name (RTS-DC1)
- Right click server name (RTS-DC1) --> Properties
- Forwarders tabs --> edit --> add IP --> OK --> OK
- Root hints tab --> view all the 13 root hints public servers.

**Conditional Forwarders**

- Server Manager --> Tools --> DNS
- In DNS managers --> DNS --> server name (RTS-DC1) --> Conditional Forwarders
- Right click --> new conditional forwarders
- Give domain name and IP Address 
- OK.

****



