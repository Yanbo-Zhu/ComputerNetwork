
![](image/Pasted%20image%2020241105191436.png)


# 1 IP_Datagram_format


![](image/Pasted%20image%2020241105191502.png)


# 2 IP-HEADER UND IP-ADRESSEN

![](image/Pasted%20image%2020250106125557.png)

- IP-Adressen ermöglichen die Adressierbarkeit von Geräten und die Zustellung von Paketen
- Jedes Paket enthält die IP-Adressen des Empfängers und Absenders
- Bei der Weiterleitung von Paketen entscheiden Router auf welchem Ausgang ein Paket weitergeleitet werden muss, um den Empfänger zu erreichen
- Unterteilt sich in einen Präfix für die Adresse des Netzes und einen Postfix für den Rechner innerhalb des Netzes
- Länge des Präfix wird durch die Netzmaske angegeben
- IP-Version 4: 32-Bit-Adressen, d.h. 4.294.967.296 verschiedene Adressen
- IP-Version 6: 128-Bit-Adressen, d.h. 340.282.366.920.938.463.463.374.607.431.768.456 Adressen (oder 6,65*10^17 Adressen pro Quadratmillimeter der Erdoberfläche)


# 3 IP address and interface 

![](image/Pasted%20image%2020241105191632.png)


One Router's have multiple interfaces, because it connects with multiple subdomain 



# 4 Subnets


![](image/Pasted%20image%2020241105192127.png)

![](image/Pasted%20image%2020241105192444.png)


> Subnet 的意义是 Device interfaces that can physically reach each other without passing through an intervening router 

detach each interface from its host or router, creating “islands” of isolated networks
each isolated network is called a subnet


223.1.3.0/24
subnet mask: /24
the first 24 bit are subnet part 
25-32: tbhe last 8 bit are host part 

# 5 CIDR

![](image/Pasted%20image%2020241105192522.png)


# 6 DHCP: How does host get IP address?

Dynamic Host Configuration Protocol:  host dynamically  obtains IP address from network server when it "joins" network 

DHCP 的意义是获取 ip address of xx from network server 

![](image/Pasted%20image%2020241105192657.png)

DHCP server 被部署在 router 


![](image/Pasted%20image%2020241105192716.png)


第一部中 Arriving client broadcast in the domain to finde DHCP server 
第二步中 DHCP server 还不知道 arrving client 的 ip adress, 所以他只能通过 broadcast 去找 arrving client, 而不是 通过 ip addresse of arriving client  直接发送给 这个 client 
之后 arrving client 将自己的 ip address 登记在 dhcp server 中 

![](image/Pasted%20image%2020241105193127.png)


![](image/Pasted%20image%2020241105193626.png)


## 6.1 example 


client 从 dhcp 中获取一些信息的过程 

![](image/Pasted%20image%2020241105193650.png)


![](image/Pasted%20image%2020241105193709.png)


![](image/Pasted%20image%2020241112210457.png)




# 7 special IP address types

On most Operating Systems, there are several ways to find out your own IP addresses. On the command line, the ip address command (or ip a for short) usually works on Linux and Mac OS, ipconfig /all usually works on Windows.

Loopback
Address used for communication that does not leave device

Multicast
Addresses a group of hosts at once. Useful for streaming and conferencing applications

Link-local
Non-unique, non-routable addresses. Can only be used within a network
# 8 command `ip a`

![](image/Pasted%20image%2020241129234400.png)




## 8.1 Loopback IP addresses
The loopback IP address (commonly `127.0.0.1` for IPv4 and `::1` for IPv6) is used by a device to refer to itself. It is primarily used for testing and debugging local network applications without requiring external communication.

## 8.2 Multicast IP addresses
Multicast IP addresses are used to send data to multiple devices simultaneously, often in the same network segment. IPv4 multicast addresses range from `224.0.0.0` to `239.255.255.255`, while IPv6 multicast addresses start with `ff00::/8`.

## 8.3 Link-local IP addresses
Link-local IP addresses are automatically assigned to network interfaces for ==communication within the same physical or logical link==. IPv4 link-local addresses range from `169.254.0.0/16`, and IPv6 link-local addresses start with `fe80::/10`

- Prefix: `fe80::/10`
- Example: `fe80::1`  
    These addresses are automatically assigned to interfaces and used only within the local link.

## 8.4 Private Unicast IP address 
Private IP ranges are designated for use within private networks (LANs) and are not routable on the public internet. They include:
- `10.0.0.0/8` (10.0.0.0 – 10.255.255.255)
- `172.16.0.0/12` (172.16.0.0 – 172.31.255.255)
- `192.168.0.0/16` (192.168.0.0 – 192.168.255.255)
Example: `192.168.1.1` (a common private IP used in home routers).

Unique Local Addresses (ULA):
ipv6的形式
- Prefix: `fc00::/7`
- Example: `fd00::1`  
    ULAs are equivalent to private IPv4 addresses, used within private networks.
## 8.5 Global Unicast Addresses

ipv4 的形式 
Any IPv4 address not in the private ranges above or reserved ranges (e.g., multicast `224.0.0.0/4`) is a global unicast address. These are routable on the internet.
Example: `8.8.8.8` (Google DNS).

ipv6 的形式 
Global unicast addresses in IPv6 are the equivalent of public IP addresses in IPv4. These are routable on the internet and used to ==uniquely identify devices globally across networks.== They are assigned by the Internet Assigned Numbers Authority (IANA) or regional registries.
- Prefix: `2000::/3`
- These addresses are routable on the internet.
- Example: `2001:db8::1` (documentation address).

**Key Features:**
- They begin with the prefix `2000::/3`. For example, `2001:db8::` is a commonly used documentation address.
- Unlike link-local addresses (which operate only within the local network), global unicast addresses can reach devices worldwide if routing is properly configured.
- Each device with a global unicast address can be uniquely identified on the internet.
Global unicast addresses are primarily used for communication between devices across different networks and the public internet. This includes web browsing, remote access, and communication between servers.


## 8.6 Link-local 和 Global Unicast Addresses 的区别
Devices with a global unicast address can reach hosts on the Internet. Link-local addresses only allow communication within the same local link or subnet.



