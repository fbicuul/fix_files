
Static routes are useful for smaller networks with only one path to an outside network. They also provide security in a larger network for certain types of traffic, or links to other networks that need more control.


Static routes can be configured for IPv4 and IPv6. Both protocols support the following types of static routes:
• Standard static route
• Default static route
• Floating static route
• Summary static route



When  configuring a static route, the next hop can be identified by an IP  address, exit interface, or both. How the destination is specified  creates one of the three following types of static route:
• Next-hop route - Only the next-hop IP address is specified
• Directly connected static route - Only the router exit interface is specified
• Fully specified static route - The next-hop IP address and exit interface are specified




Next-Hop Static Route
In a next-hop static route, only the next-hop IP address is specified. The exit interface is derived from the next hop. For example, three next-hop IPv4 static routes are configured on R1 using the IP address of the next hop, R2.



R1(config)# ip route 172.16.1.0 		255.255.255.0 	172.16.2.2
R1(config)# ip route 192.168.1.0 	255.255.255.0 	172.16.2.2
R1(config)# ip route 192.168.2.0 	255.255.255.0 	172.16.2.2

R1(config)# ipv6 unicast-routing
R1(config)# ipv6 route 2001:db8:acad:1::/64 		2001:db8:acad:2::2
R1(config)# ipv6 route 2001:db8:cafe:1::/64 		2001:db8:acad:2::2
R1(config)# ipv6 route 2001:db8:cafe:2::/64 		2001:db8:acad:2::2






Directly Connected Static Route




R1(config)# ip route 172.16.1.0 		255.255.255.0 	s0/1/0
R1(config)# ip route 192.168.1.0 	255.255.255.0 	s0/1/0
R1(config)# ip route 192.168.2.0 	255.255.255.0 	s0/1/0





Fully Specified Static Route
In a fully specified static route, both the exit interface and the next-hop IP address are specified. This form of static route is used when the exit interface is a multi-access interface and it is necessary to explicitly identify the next hop. The next hop must be directly connected to the specified exit interface. Using an exit interface is optional, however it is necessary to use a next-hop address.



R1(config)# ip route 172.16.1.0 		255.255.255.0 	GigabitEthernet 0/0/1 	172.16.2.2
R1(config)# ip route 192.168.1.0 	255.255.255.0 	GigabitEthernet 0/0/1 	172.16.2.2
R1(config)# ip route 192.168.2.0 	255.255.255.0 	GigabitEthernet 0/0/1 	172.16.2.2





















