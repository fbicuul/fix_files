
An ACL is a series of IOS commands that are used to filter packets  based on information found in the packet header. By default, a router  does not have any ACLs configured. However, when an ACL is applied to an  interface, the router performs the additional task of evaluating all  network packets as they pass through the interface to determine if the  packet can be forwarded.

An ACL uses a sequential list of permit or deny statements, known as access control entries (ACEs).

Note: ACEs are also commonly called ACL statements.
Note: ACLs do not act on packets that originate from the router itself.




Several  tasks performed by routers require the use of ACLs to identify traffic.  The table lists some of these tasks with examples.


| Task | Example |
| Limit network traffic to increase network performance | • A corporate policy prohibits video traffic on the network to reduce the network load.
• A policy can be enforced using ACLs to block video traffic. |
| Provide traffic flow control | • A corporate policy requires that routing protocol traffic be limited to certain links only.
• A policy can be implemented using ACLs to restrict the delivery of routing updates to only those that come from a known source. |
| Provide a basic level of security for network access | • Corporate policy demands that access to the Human Resources network be restricted to authorized users only.
• A policy can be enforced using ACLs to limit access to specified networks. |
| Filter traffic based on traffic type | • Corporate policy requires that email traffic be permitted into a network, but that Telnet access be denied.
• A policy can be implemented using ACLs to filter traffic by type. |
| Screen hosts to permit or deny access to network services | • Corporate policy requires that access to some file types (e.g., FTP or HTTP) be limited to user groups.
• A policy can be implemented using ACLs to filter user access to services. |
| Provide priority to certain classes of network traffic | • Corporate traffic specifies that voice traffic be forwarded as fast as possible to avoid any interruption.
• A policy can be implemented using ACLs and QoS services to identify voice traffic and process it immediately. |



Cisco routers support two types of ACLs:
• Standard ACLs - ACLs only filter (permit or deny packets) at Layer 3, based only on the source IPv4 address.
• Extended ACLs - ACLs filter (permit or deny packets) at Layer 3 based on the source and / or destination IPv4 address. They can also filter at Layer 4 using TCP, UDP ports, and optional protocol type information for finer control.




Wildcard Mask Keywords
• host - This keyword substitutes for the 0.0.0.0 mask. This mask states that all IPv4 address bits must match to filter just one host address.
• any - This  keyword substitutes for the 255.255.255.255 mask. This mask says to ignore the entire IPv4 address or to accept any addresses.

For example,  in the command output, two ACLs are configured. The ACL 10 ACE permits  only the 192.168.10.10 host and the ACL 11 ACE permits all hosts.

R1(config)# access-list 10 permit 192.168.10.10  0.0.0.0
R1(config)# access-list 11 permit  0.0.0.0 255.255.255.255
R1(config)#

Alternatively, the keywords host and any could have been used to replace the highlighted output.

R1(config)# access-list 10 permit  host  192.168.10.10
R1(config)# access-list 11 permit  any
R1(config)#






ACL Best Practices

Using  ACLs requires attention to detail and great care. Mistakes can be  costly in terms of downtime, troubleshooting efforts, and poor network  service. Basic planning is required before configuring an ACL.

The table presents guidelines that form the basis of an ACL best practices list.


| Guideline | Benefit |
| Base ACLs on the organizational security policies. | This will ensure you implement organizational security guidelines. |
| Write out what you want the ACL to do. | This will help you avoid inadvertently creating potential access problems. |
| Use a text editor to create, edit, and save all of your ACLs. | This will help you create a library of reusable ACLs. |
| Document the ACLs using the remark command. | This will help you (and others) understand the purpose of an ACE. |
| Test the ACLs on a development network before implementing them on a production network. | This will help you avoid costly errors. |





Numbered and Named ACLs 
-------------------------------------------- NUMBERED
R1(config)# 	access-list  ?
  <1-99>       			IP standard access list
  <100-199>    		IP extended access list
  <1100-1199> 		Extended 48-bit MAC address access list
  <1300-1999>  	IP standard access list (expanded range)
  <200-299>    		Protocol type-code access list
  <2000-2699> 	IP extended access list (expanded range)
  <700-799>    		48-bit MAC address access list
  rate-limit   			Simple rate-limit specific access list
  template     			Enable IP template acls
R1(config)# access-list



-------------------------------------------- NAMED
R1(config)# ip access-list extended FTP-FILTER
R1(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq ftp
R1(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq ftp-data
R1(config-ext-nacl)#





Where to Place ACLs



Assume the objective to prevent traffic originating in the 192.168.10.0/24 network from reaching the 192.168.30.0/24 network.
Extended ACLs should be located as close as possible to the source of  the traffic to be filtered. This way, undesirable traffic is denied  close to the source network without crossing the network infrastructure.
Standard  ACLs should be located  as close to the destination as possible. If a  standard ACL was placed at the source of the traffic, the "permit" or  "deny" will occur based on the given source address no matter where the  traffic is destined.






REMARK a.k.a ACL's Description
R1(config)# access-list  10  remark  ACE permits ONLY host 192.168.10.10 to the internet
R1(config)# access-list 10 permit host 192.168.10.10
R1(config)# do show access-lists
		Standard IP access list 10
    	10 permit 192.168.10.10
R1(config)#


Notice that the output of the show access-lists command does not display the remark statements. ACL remarks are displayed in the running configuration file. Although the remark command is not required to enable the ACL, it is strongly suggested for documentation purposes.





CLEARING ACL COUNTERS
Use the clear access-list counters command to clear the ACL statistics.

R1# show access-lists
	Standard IP access list NO-ACCESS
    10 deny   192.168.10.10  (20 matches) 
    20 permit 192.168.10.0, wildcard bits 0.0.0.255  (64 matches) 
R1# clear access-list counters NO-ACCESS
R1# show access-lists
	Standard IP access list NO-ACCESS
    10 deny   192.168.10.10
    20 permit 192.168.10.0, wildcard bits 0.0.0.255
R1#






COMMAND SYNTAX FOR EXTENDED ACLs
Although  there are many keywords and parameters for extended ACLs, it is not  necessary to use all of them when configuring an extended ACL. The table  provides a detailed explanation of the syntax for an extended ACL.

Router(config)# access-list access-list-number {deny | permit | remark text} protocol source source-wildcard [operator {port}] destination destination-wildcard [operator {port}] [established] [log]



| Parameter | Description |
| access-list-number | • This is the decimal number of the ACL. 
• Extended ACL number range is 100 to 199 and 2000 to 2699. |
| deny | This denies access if the condition is matched. |
| permit | This permits access if the condition is matched. |
| remark text | • (Optional) Adds a text entry for documentation purposes.
• Each remark is limited to 100 characters. |
| protocol | • Name or number of an internet protocol. 
• Common keywords include ip, tcp, udp, and icmp. 
• The ip keyword matches all IP protocols. |
| source | • This identifies the source network or host address to filter.
• Use the any keyword to specify all networks.
• Use the host ip-address keyword or simply enter an ip-address (without the host keyword) to identify a specific IP address. |
| source-wildcard | (Optional) A 32-bit wildcard mask that is applied to the source. |
| destination | • This identifies the destination network or host address to filter.
• Use the any keyword to specify all networks.
• Use the host ip-address keyword or ip-address. |
| destination-wildcard | (Optional) This is a 32-bit wildcard mask that is applied to the destination. |
| operator | • (Optional) This compares source or destination ports. 
• Some operators include lt (less than), gt (greater than), eq (equal), and neq (not equal). |
| port | (Optional) The decimal number or name of a TCP or UDP port. |
| established | • (Optional) For the TCP protocol only.
• This is a 1st generation firewall feature. |
| log | • (Optional) This keyword generates and sends an informational message whenever the ACE is matched.
• This message includes ACL number, matched condition (i.e., permitted or denied), source address, and number of packets. 
• This message is generated for the first matched packet.
• This keyword should only be implemented for troubleshooting or security reasons. |




ACL Protocol Options
The four highlighted protocols are the most popular options.

Note: Use the  ?  to get help when entering a complex ACE.
Note:  If an internet protocol is not listed, then the IP protocol number  could be specified. For instance, the ICMP protocol number 1, TCP is 6,  and UDP is 17.


R1(config)# access-list  100  permit  ? 
  <0-255>       		An IP protocol number
  ahp           				Authentication Header Protocol
  dvmrp         			dvmrp
  eigrp         				Cisco's EIGRP routing protocol
  esp           				Encapsulation Security Payload
  gre           				Cisco's GRE tunneling
  icmp          				Internet Control Message Protocol
  igmp          				Internet Gateway Message Protocol
  ip            					Any Internet Protocol
  ipinip        				IP in IP tunneling
  nos           				KA9Q NOS compatible IP over IP tunneling
  object-group  	Service object group
  ospf          				OSPF routing protocol
  pcp          		 			Payload Compression Protocol
  pim           				Protocol Independent Multicast
  tcp           					Transmission Control Protocol
  udp           				User Datagram Protocol
R1(config)# access-list 100 permit 



ACL Port Keyword Options
Selecting a protocol influences port options. For instance, selecting the:
• tcp protocol would provide TCP related ports options
• udp protocol would provide UDP specific ports options
• icmp protocol would provide ICMP related ports (i.e., message) options

Again, notice how many TCP port options are available. The highlighted ports are popular options.
Port  names or number can be specified. However, port names make it easier to  understand the purpose of an ACE. Notice how some common ports names  (e.g., SSH and HTTPS) are not listed. For these protocols, port numbers  will have to be specified.


R1(config)# access-list 100 permit tcp any any eq  ?
  <0-65535>    		Port number
  bgp          				Border Gateway Protocol (179)
  chargen      			Character generator (19)
  cmd          				Remote commands (rcmd, 514)
  daytime      			Daytime (13)
  discard      			Discard (9)
  domain       			Domain Name System (53) 
  echo         				Echo (7)
  exec         				Exec (rsh, 512)
  finger       				Finger (79)
  ftp          					File Transfer Protocol (21) 
  ftp-data     			FTP data connections (20) 
  gopher       			Gopher (70)
  hostname     		NIC hostname server (101)
  ident        				Ident Protocol (113)
  irc          					Internet Relay Chat (194)
  klogin       				Kerberos login (543)
  kshell       				Kerberos shell (544)
  login        				Login (rlogin, 513)
  lpd          					Printer service (515)
  msrpc       	 			MS Remote Procedure Call (135)
  nntp         				Network News Transport Protocol (119)
  onep-plain   		Onep Cleartext (15001)
  onep-tls     			Onep TLS (15002)
  pim-auto-rp  		PIM Auto-RP (496)
  pop2         				Post Office Protocol v2 (109)
  pop3         				Post Office Protocol v3 (110) 
  smtp         				Simple Mail Transport Protocol (25) 
  sunrpc       				Sun Remote Procedure Call (111)
  syslog       				Syslog (514)
  tacacs       				TAC Access Control System (49)
  talk         					Talk (517)
  telnet       				Telnet (23) 
  time         				Time (37)
  uucp         				Unix-to-Unix Copy Program (540)
  whois        				Nicname (43)
  www          				World Wide Web (HTTP, 80) 
R1(config)# access-list 100 permit tcp any any eq 




