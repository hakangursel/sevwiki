# sevwiki

#  Unified Communication Deployment
![enter image description here](https://17ihe82xpmj33g4l3s1s5x6r-wpengine.netdna-ssl.com/wp-content/uploads/2018/06/CII_Logo_Color_preview-3.png)

**Milestones**
- [ ] Sangoma Portal
	- [ ] Assign phones to CII Deployment  
	- [ ] Zero-touch provisioning redirection for phones
 - [ ] PBX-1
	 - [x] ~~Install Sangoma OS~~
		 - [x]  ~~Activate system~~
	 - [ ] System settings
		 - [x] ~~Static IP~~
		 - [x] ~~DNS servers~~
		 - [x] ~~Hostname~~
		 - [ ] Email Setup
		 - [x] ~~Notification settings~~
		 - [x] ~~Intrusion detection settings~~
		 - [x] ~~Time zone~~
	- [x] ~~Asterisk Settings~~
		- [x] ~~Change default SIP ports~~
		- [x] ~~Configure codecs~~
	- [ ] Certificate Management
		- [x] ~~Let`s Encrypt Acme service~~
		- [ ] Integrate wildcard certificate
	- [x] ~~Firewall~~
		- [x] ~~Services~~
	- [x] ~~Endpoint Manager~~
		- [x] ~~Global settings~~
		- [x] ~~Extension mapping~~
		- [x] ~~Firmware configuration~~
		- [x] ~~Template configuration~~
	- [ ] Connectivity
		- [x] ~~Trunks~~
			- [x] ~~Spectrum SIP Trunk~~
			- [x] ~~FreePBX IAX2 Trunk~~
		- [ ] Inbound Routes
		- [x] ~~Outbound Routes~~
			- [x] ~~Spectrum~~
			- [x] ~~FreePBX~~
	- [ ] Applications
		- [x] ~~Zulu 3 Server~~
		- [x] ~~XMPP Server~~
		- [x] ~~User Control Panel~~


## Devices
|Item Name|Quantity|
|--|--|
|PBXact Appliance 2000 |2 |
|Sangoma S705 Phone | 190|
|Sangoma S505 Phone| 340 |
|Sangoma Phones Power Adapter | 130 |

## Hardware Specs

### PBXact2000
* **Licensed Extensions  :** 2000
* **Supported Call Capacity  :** 1500*
* **Storage Capacity :** 2 x 1 TB (RAID 1)
* **Processor :** Dual Intel Xeon Six-Core
* **Memory :** 32 GB

>Call Capacity is for normal inbound and outbound calling from a SIP Phone through a SIP Trunk or to another SIP Phone.

>Queue Calls from a system load perspective as it relates to Call Capacity is similar to having 4 calls concurrent calls per queue call on the system.

>Conference Bridge calls from a system load perspective as it relates to Call Capacity is similar to 2 concurrent calls per caller in the conference room on the system. Max users per conference room is 250 Callers.

>   Call Recording calls from a system load perspective as it relates to Call Capacity is similar to 2 concurrent calls per caller that is being recorded on the system.  

### S505 Phones
### S705 Phones
## Locations
### Otis Booth  (UC01)
2121 W Temple St, Los Angeles, CA 90026

**UC01**

**IP Address :** 192.168.16.80  
**Netmask :** 255.255.252.0  
**Gateway :** 192.168.16.10  
**Admin User:** admin  
**Hostname :**  uc01.childrensinstitute.org  
**BMC IP :** 192.168.16. 81  
**Deployment ID :** 83579195  

### CoreSite (UC02)
CoreSite LA2. 900 N Alameda St, Los Angeles
**IP Address :** 192.168.1.80
**Netmask :** 255.255.255.0
**Gateway :** 192.168.1.10

# Installation
* Download ISO file [Downloads - FreePBX](https://www.freepbx.org/downloads/)
`Used Version : STABLE SNG7-PBX-64bit-1904 Release Date: May 2019`
* Boot ISO than install FreePBX
* Active Freepbx to change it to PBXact
`fwconsole sysadmin activate [deployment-id]`
* Wait 5 to 10 minutes than use this to check installation
`fwconsole sa eol`

# Configuration
## System Admin Settings
### Static IP  
```
Network Interface : eth0
Static IP : 192.168.16.80
Netmask : 22
Gateway : 192.168.16.10
```
### DNS servers

`192.168.16.29, 192.168.1.63`

### Hostname

`uc01`

### Email setup
```
SMTP Server : Use built in SMTP server
My Hostname : uc01
My Origin : uc01.childrensinstitute.org
My Domain : childrensinstitute.org
```
### Notification settings

`cii@seviniti.com`

### Intrusion detection settings
```
Ban Time : 3600 sec
Max Retry : 3
Find Time : 300
E-mail : cii@seviniti.com`
Whitelist : 127.0.0.1
```
### Time zone

`America/Los_Angeles`

## Asterisk Settings
### Change default SIP ports
* Set local networks from general sip settings

`192.168.0.0/16`
* Change default chan_sip pjchan_sip ports.

`chan_sip : 5060, chan_sip tsl: 5061, pjchan_sip : 5160 `
### Codec Management
* Put codecs correct order
## Certificate Management
* Forward port 80 to pbx from these addresses

`outbound1.letsencrypt.org, outbound2.letsencrypt.org, mirror1.freepbx.org, mirror2.freepbx.org`

* Create a new Let`s Encrypt certificate from certificate management.
```
Certificate Host Name : uc01.childrensinstitute.org
Owners Email : cii@seviniti.com
Common Name uc01.childrensinstitute.org
Certificate Policies :
		Policy: 2.23.140.1.2.1
		Policy: 1.3.6.1.4.1.44947.1.1.1
  		CPS: http://cps.letsencrypt.org
HTTP : Port 80
Country : United States
State/Province/Region : California
```
* Setup https from system admin with using this certificate.

## Firewall Settings
* add all local networks as trusted zone from firewall > networks
* enable firewall set eth0 interface as  internet zone from firewall > interfaces
* create a new zone for management like ssh, https

## Endpoint Manager Settings
### Zero Touch provisioning
* Find devices and edit from portal.sangoma.com > products > phones > list
* Enable redirection and put pbx ip_addr:port
### Global Settings
* Setup internal and external ip : 10.111.111.25:84
* Change admin password
* Enable extension mapping ip addresses and phone status
### Extension Mapping
* Assign extensions to phones with using Mac add from Extension Mapping (barcode scanner is good idea)
* Save and rebuild configs
* Factory reset phones..
* Check peers from Reports > Asterisk info
### Firmware
* Drag&drop latest firmware on slot1
### Template Configuration
* Brands > Sangoma
* change provisioning server mode to http
* select default internal template
* select a phone and put xml-api > voicemail > REST-voicemail
* Save, rebuild configs and update phones
* check phones from extension mapping.

## Connectivity
### IAX Trunk
#### UC01
#### Adding new iax2 trunk.
* Add new trunk from connections > trunk
**Trunk name :** iaxtrunk
#### Outgoing settings
**trunk name :** freepbx
**peer details :**
```
host=ciipbx.childrensinstitute.org
username=freepbx
secret=1AXtrunkS3cret
type=friend
qualify=yes
```				
#### Incoming settings
**USER Context :**  pbxact
**USER Details :**
```
host=uc01.childrensinstitute.org
username=pbxact
secret=1AXtrunkS3cret
context=from_internal
type=friend
qualify=yes
```
* add outgoing route with using dial patterns

### CIIPBX

#### Adding new iax2 trunk.
* Add new trunk from connections > trunk
**Trunk name :** iaxtrunk
#### Outgoing settings
**trunk name :** freepbx
**peer details :**
```
host=uc01.childrensinstitute.org
username=freepbx
secret=1AXtrunkS3cret
type=friend
qualify=yes
```
#### Incoming settings
**USER Context :**  pbxact
**USER Details :**
				host=ciipbx.childrensinstitute.org
				username=pbxact
				secret=1AXtrunkS3cret
				context=from_internal
				type=friend
				qualify=yes
* add outgoing route with using dial patterns

## Zulu 3

* Browse to Settings,**Advanced Settings**
	* Change **Display Readonly Settings** to **YES**
	* Change **Override Readonly Settings** to **YES**
	* Search for **Enable the mini-HTTP Server** and change it to **YES**
	* Search for **Enable TLS for the mini-HTTP Server** and change it to **YES**
	* Search for **Force WebSocket Mode** and change it to **PJSIP**
	* Search for **Enable the Asterisk REST Interface** and change it to **YES**
	* Search for **SIP Channel Driver** and change it to**BOTH**only

* Browse to Admin,**Certificate Management**
	* ensure you have at least one certificate and that there is a default certificate selected (green check)

* Browse to Settings,**Asterisk SIP Settings**,**PJSIP tab**
	* enable both “ws - 0.0.0.0 - All”and “wss - 0.0.0.0 - All”. Leave ws and wss disabled for individual interfaces.

* After completing these steps, SSH to your PBX and restart asterisk by:
`fwconsole restart`
## XMPP Server
* **domain :** childrensinstitute.org
## User Control Panel
* **port :** 4433
* **address :** https://uc01.childrensinstitute.org:4433/ucp/

# Troubleshooting
## Asterisk commands


|Command Title|Command|Alias(es)|description
|--|--|--|--|
| Bulk Import           	| bulkimport       		| bi         | This command is used to import extensions and dids   |
| Certificates          	| certificates        |            | Certificate Management                               |
| Chown                 		| chown               |            | Change ownership of files                            |
| Context               		| context             | cx         | Shows the specified context from the dialplan        |
| Doctrine              		| doctrine            |            | Specific database documentation (for Development)    |
| Debug                 			| dbug                | debug      | Stream log files for debugging                       |
| EPM                   			| epm                 | endpoint   | Endpoint Manager                                     |
| External IP           		| extip               | externalip | Get External IP                                      |
| Firewall              			| firewall            |            | Firewall functions                                   |
| help                  			| help                |            | Displays help for a command                          |
| list                  			| list                |            | Lists commands                                       |
| Localization          		| localization        |            | Localization Utilities                               |
| Database              		| mysql               |            | Runs MySQL Client using freepbx credentials          |
| Module Administration | moduleadmin         | ma         | Module Administration                                |
| Notification          | notification        |            | Manage notifications                                 |
| Module System Manager | modulesystemmanager | msm        | View and change Update/Notification Manager Settings |
| Qxact Reports         | qxact               |            |                                                      |
| Reload                | reload              | r          | Reload Configs and Asterisk                          |
| Paging Pro            | pagingpro           |            | Paging Pro Interface                                 |
| Phone Apps            |                     |            | Debug Phone App Problems (For XML Style BLFs)        |
| Process Management    | pm2                 |            | Process Management                                   |
| Setting               | setting             | set        |                                                      |
| Sound Languages       | sound               |            | Sound Languages Management                           |
| System Admin          | sysadmin            | sa         | System Admin Functions                               |
| System Update         | systemupdate        | sysup,sys  |                                                      |
| Trunks                | trunks              |            | Enable and disable trunks from the command line      |
| Unlock                | unlock              | un         | Unlock Session                                       |
| User Manager          | userman             |            | User Manager                                         |
| Utilities             | util                |            | Common Utilities                                     |
| Validate              | validate            |            | Validate the PBX against hacks                       |
| vqplus                | vqplus              |            | VQPlus functions                                     |
| zulu                  | zulu                |            | Zulu functions                                       |

## Reinstallation
* Download  and install latest freepbx iso from sangoma web page.
* Activation
`fwconsole sysadmin activate [deployment id]` (after activating, wait 10 minutes for background processes)
* End of line testing
`fwconsole sa eol`  (checks necessary modules.)
* installing ssh-keys and xact view
`yum -y install ssh_keys XactViewServer*`
* list modules
`fwconsole ma listonline`

# Seviniti - 2019
