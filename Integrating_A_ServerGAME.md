## OVERVIEW
### JOB: NETWORK OPERATIONS TEAM
### TOPIC: INTEGRATING A SERVER ONTO THE NETWORK FOR THE PURPOSES OF TESTING
### ASSIGNED BY: DEVELOPMENT TEAM
### TASKS:Assign two temporary IP addresses to the server's main network. 
### WHY: For A/B Testing
## KEY HARDWARE
    **MODEM** connects to Internet Service Provider (**ISP**) and tranlates internet signals. 
        -Think of as translator between ISP and my home network. 
    **ROUTER** Assigns **IPs** and routes traffic between devices on the network and the internet
        -Think of Router as traffic control, managing communication 
       *MODEM V ROUTER*: modem brings it to the home, router distriutes it to devices. 
    **SWITCH** Handles multiple wired (Ethernet) connections
    **ACCESS POINT** Provides WiFi to wireless **clients**
    **NIC** Network Access Card. Hardware component within both server and client devices. It's what actually connects the device to the network
    *Most household out-of-the-box or ISP provided devices are combo mmodem/routers that handle:*
        -WAN connection     
        -LAN routing
        -WiFi Access
        -Firewalling. 
## Terms
### SERVER
     A computer or software application that provides servies, or resources to other computers (**clients**) over a **network** 
     common types include
        -web servers
        -file servers
        -database servers
     **Client**
        -a computer or software application that requests and uses services or resources from a **server** over a **network**
### NETWORK
    A group of interconnected computers and devices that can communicate and share resources. This includes both physical (wired/wireless/bluetooth) connections **AND** logical (IP addressing, protocols) components

### A/B TESTING 
    Method by testing two versions (A and B) of a system (website, service, or configuration) by directing a portion of traffic to each version and measuring which performs better according to certain metrics
### NIC (Network Interface Card)
    Computer hardware or virtualized interface that enables a computer to connect to a network. It handles communication at the **data link layer**, has a **MAC adress** and supports **IP configuration**. 
    **Primary NIC** 
        -the main network interface used by the system for default outbound traffic and communication. Often associated with the system's default gateway and primary **IP address**

### MAC ADRESS  
     Media Access Control address. Hardware identifier assigned to a NIC
     It is a unique 48-bit address (`00:1A:2B:3C:4D:5E`)
### IP ADDRESS
     A numerical label assigned to a device on a network, used for identifying the source and destination of data. 
### IP ALIASING
     The process of assigning multiple IP addresses to a single network interface
### Outbound Traffic 
     Data sent FROM a server or device TO other systems on the network or internet
### GATEWAY
     The device (typically a **router**) that connects a local netwrok to external networks, such as the internet. 
### DEFAULT GATEWAY
     The router or network node that a devie uses to send traffic distined fr outside its local network -- essentall the "next hop" for non-local IP addresses. 
     where a device sends traffic when the destnation IP is outside the local subnet.
### SUBNET
     Short for `subnetwork`. A logically defined define section of a larger network, created by dividing an IP network into smaller segments. Subnets group devices with similar addresses, which improves routing efficiency and network organization. 
    **Example**
        If the network is `192.168.1.0/24`
             The **subnet** includes all IPs from `192.168.1.1` to `192.168.1.254`
             Devices within the subnet can commiunicate directly with each other. 
             Anything outside thi subnet must go through a **gateway**.
### SUBNET MASK
     32-bit number that works alongside an IP address to define which portion of the address identifies the **netowrk** and which part identifies the **host**
### Interface Names
    **NAMING CONVENTION**
    `eth0`, `eth1` traditional naming 
    STRUCTURE: **zero indexed**--`first ethernet interface, second ethernet int`
### DHCP
     Dynamic Host Configuration Protocol. A network protocol that automaticallly assigns IP addresses and other network configuration settings such as **subnet mask** **gateway** **DNS** to devices on the network so they can communicate without manual setup. 

### Manual Setup
    Also called 'strategic configuration', means assigning a device's network settings such as IP address, subnet mask, gateway, DNS, without using automatic system like DHCP
## THE HANDS ON 
    Hey, Hilde. It's me, you! Your favorite haha. Okay, so my idea here is I am going to jounal out this portion. This is pretty cool, and I think it's a very valuable resource. Not to mention, this is a very well gamified exersize. So, just to get us all caught up...this is an exercise I found on a government Cyber Sercurity careers page. I think my dad is onto something. Security is the way to go. 
So this is some baby steps stuff. 

The first "job" is to assign two temporary **IP addresses** to the server for the purpose of **A/B testing**. 

These are all terms I have looked up above. 
**IP address**: numerical label assigned to a device on a network
**A/B Testing**: putting two different versions of the same software or process through similar tests to see which performs better according to certain metrics.

In the scenario, this is a request from the **Development Team** to the **Network Operations Team**. IIiii'm the Network Operations Team. SO!!The Network operations team's job is to makee sure things get indexed into the network propely and maintaining the network. We all gotta start somewhere. 

Whatcha gotta do is first is review the exisitng IP addresses configured on the **server's** **primary NIC**
**Server**: a software applicaton or computer thhat provides serrvices and resources to other devices on the network. 
**NIC**: Network Interface Card--hardware or virtualized program that enables a computer to connet to a network. If you remember the stuff from like over a year ago about the network layers, this one lives at the data link layer. It's a *HARDWARE girl*, and **both** the **client** && **server** hardware have her. She is important. NIC network interface card. Hardware, both client and server have their own. The NIC is what allows the device to connect to the network, be it wired or wireless. Examples would be my laptop, which has a NIC for wireless, wired (ethernet), or my internet home intnernet router, which has a NIC for ethernet connections and wifi connections. 
PRIMARY NIC: The main used by the system for outbound traffic and communication. 

The way that we REVIEW EXISTING IP ADDRESSES CONFIGURED ON THE SERVER'S PRIMARY NIC
`~$ ip -4 -br address show`
COMMAND:
    `ip` show and manipulate routing, network device, interfaces, and tunnels. 
OPTIONS
    `-4` 'shortcut for `-family inet`'. Limits output to only include IPv4 addresses.
    `-br` stands for "brief". "Print only basic infomaton in a tabular format fo better readability. ONLY SUPPORTED BY `addr show`, `ip link show`, `ip neigh show` commands"
OBJECT
    `addresss` protocol (IP or IPv6) address on the device
OPTIONS
    `show` one of the options available to the OBJECT. Most OBJECTS have ability to **show**, **list**, **add**, or **delete** but some OBJECTS do not support each of these. 


When I run it on my machine, I get this

```
lo      UNKNOWN 127.0.0.1/8
wlpls0  UP      192.168.0.24/24
```
On my machine, what this means is 

PRIMARY NIC
    `wlpsl0`, which has only one IP address assigned to it which is...
IP ADDRESS ASSIGNED TO NIC
    `192.168.0.24` and has 
SUBNET MASK
    `/24`

SIDE BAR ON IP conventions
IPs within the range 
    `192.168.0.0` 
    `192.168.255.255`
are **non-routable on the public internet**
are used inside private networks like home/office

Within the exercise, the output we wanted to find was 

`ens3 UP 192.168.100.200/24`

This confirms there is only one IP assigned to the server's main network interface (Primary NIC).

ADDING THE TEMP IPS

*note the IPs will disappear after reboot*

`~$ sudo ip addr add 192.168.100.210/24 dev ens3`
`~$ sudo ip addr add 192.168.100.220/24 dev ens3`

confirm the addition with 
    `ip -4 -br address show`

KEY KNOWLEDGE AREAS
-Teams and roles
    -Dev Team made  request to 
    -Network Ops team. In this case, me. 
-Why am I doing this?
    -the Dev Team is an a testing phase of their process and they have two versions of the same software. They are going to conduct **A/B testing** *on the versions by running both of them under similar conditions and seeing which one performs better according to certain metrics*

-How do I play into this?
    -as a member of the Network Ops team, my purview covers 
