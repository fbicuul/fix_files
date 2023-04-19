
OSPFv2
Types of OSPF Packets


| Type | Packet Name | Description |
| 1 | Hello | Discovers neighbors and builds adjacencies between them |
| 2 | Database Description (DBD) | Checks for database synchronization between routers |
| 3 | Link-State Request (LSR) | Requests specific link-state records from router to router |
| 4 | Link-State Update (LSU) | Sends specifically requested link-state records |
| 5 | Link-State Acknowledgment (LSAck) | Acknowledges the other packet types |






To  maintain routing information, OSPF routers complete a generic  link-state routing process to reach a state of convergence. The figure  shows a five router topology. Each link between routers is labeled with a  cost value. In OSPF, cost is used to determine the best path to the  destination. The following are the link-state routing steps that are  completed by a router:

1. Establish Neighbor Adjacencies
2. Exchange Link-State Advertisements
3. Build the Link State Database
4. Execute the SPF (shortest path first) Algorithm
5. Choose the Best Route




Link-State Operation

1. Establish Neighbor Adjacencies
OSPF-enabled  routers must recognize each other on the network before they can share  information. An OSPF-enabled router sends Hello packets out all  OSPF-enabled interfaces to determine if neighbors are present on those  links. If a neighbor is present, the OSPF-enabled router attempts to  establish a neighbor adjacency with that neighbor.





2. Exchange Link-State Advertisements
After  adjacencies are established, routers then exchange link-state  advertisements (LSAs). LSAs contain the state and cost of each directly  connected link. Routers flood their LSAs to adjacent neighbors. Adjacent  neighbors receiving the LSA immediately flood the LSA to other directly  connected neighbors, until all routers in the area have all LSAs.





3. Build the Link State Database
After  LSAs are received, OSPF-enabled routers build the topology table (LSDB)  based on the received LSAs. This database eventually holds all the  information about the topology of the area.





4. Execute the SPF Algorithm
Routers  then execute the SPF algorithm. The gears in the figure for this step  are used to indicate the execution of the SPF algorithm. The SPF  algorithm creates the SPF tree.




5. Choose the Best Route
After  the SPF tree is built, the best paths to each network are offered to  the IP routing table. The route will be inserted into the routing table  unless there is a route source to the same network with a lower  administrative distance, such as a static route. Routing decisions are  made based on the entries in the routing table.














Single-Area and Multiarea OSPF

To  make OSPF more efficient and scalable, OSPF supports hierarchical  routing using areas. An OSPF area is a group of routers that share the  same link-state information in their LSDBs. OSPF can be implemented in  one of two ways, as follows:

• Single-Area OSPF - All routers are in one area. Best practice is to use area 0.



• Multiarea OSPF - OSPF is implemented using multiple areas, in a hierarchical fashion. All areas must connect to the backbone area (area 0). Routers interconnecting the areas are referred to as Area Border Routers (ABRs).



The focus of this module is on single-area OSPFv2.









OSPFv3
OSPFv3 is the OSPFv2 equivalent for exchanging IPv6 prefixes. Recall  that in IPv6, the network address is referred to as the prefix and the  subnet mask is called the prefix-length.
Similar to its IPv4 counterpart, OSPFv3 exchanges routing information to populate the IPv6 routing table with remote prefixes.

Note:  With the OSPFv3 Address Families feature, OSPFv3 includes support for  both IPv4 and IPv6. OSPF Address Families is beyond the scope of this  curriculum.
OSPFv2 runs over the IPv4 network layer, communicating with other OSPF IPv4 peers, and advertising only IPv4 routes.
OSPFv3  has the same functionality as OSPFv2, but uses IPv6 as the network  layer transport, communicating with OSPFv3 peers and advertising IPv6  routes. OSPFv3 also uses the SPF algorithm as the computation engine to  determine the best paths throughout the routing domain.









OSPF Operational States

When an OSPF router is initially connected to a network, it attempts to:
• Create adjacencies with neighbors
• Exchange routing information
• Calculate the best routes
• Reach convergence

The table details the states OSPF progresses through while attempting to reach convergence:


| State | Description |
| Down State | • No Hello packets received = Down.
• Router sends Hello packets.
• Transition to Init state. |
| Init State | • Hello packets are received from the neighbor.
• They contain the Router ID of the sending router.
• Transition to Two-Way state. |
| Two-Way State | • In this state, communication between the two routers is bidirectional.
• On multiaccess links, the routers elect a DR and a BDR.
• Transition to ExStart state. |
| ExStart State | On point-to-point networks, the two routers decide which router will initiate the DBD packet exchange and decide upon the initial DBD packet sequence number. |
| Exchange State | • Routers exchange DBD packets.
• If additional router information is required then transition to Loading; otherwise, transition to the Full state. |
| Loading State | • LSRs and LSUs are used to gain additional route information.
• Routes are processed using the SPF algorithm.
• Transition to the Full state. |
| Full State | The link-state database of the router is fully synchronized. |





Verify DR/BDR Adjacencies
To verify the OSPFv2 adjacencies, use the show ip ospf neighbor command, as shown in the example for R1. The state of neighbors in multiaccess networks can be as follows:

• FULL/DROTHER - This is a DR or BDR router that is fully adjacent with a non-DR or BDR router. These two neighbors can exchange Hello packets, updates, queries, replies, and acknowledgments.
• FULL/DR - The router is fully adjacent with the indicated DR neighbor. These two neighbors can exchange Hello packets, updates, queries, replies, and acknowledgments.
• FULL/BDR - The router is fully adjacent with the indicated BDR neighbor. These two neighbors can exchange Hello packets, updates, queries, replies, and acknowledgments.
• 2-WAY/DROTHER - The non-DR or BDR router has a neighbor relationship with another non-DR or BDR router. These two neighbors exchange Hello packets.

The  normal state for an OSPF router is usually FULL. If a router is stuck  in another state, it is an indication that there are problems in forming  adjacencies. The only exception to this is the 2-WAY state, which is  normal in a multiaccess broadcast network. For examples, DROTHERs will  form a 2-WAY neighbor adjacency with any DROTHERs that join the network.  When this happens, the neighbor state displays as 2-WAY/DROTHER.




OSPF Priority
• R1 should be the DR and will be configured with a priority of 255.
• R2 should be the BDR and will be left with the default priority of 1.
• R3 should never be a DR or BDR and will be configured with a priority of 0.


R1(config)# interface GigabitEthernet 0/0/0 
R1(config-if)# ip ospf priority 255 
R1(config-if)# end 
R1#
R1# clear ip ospf process
Reset ALL OSPF processes? [no]:  y
R1#


PATH COST VALUE
R1(config)# interface g0/0/1
R1(config-if)# ip ospf cost 30
R1(config-if)# interface lo0
R1(config-if)# ip ospf cost 10
R1(config-if)# end
R1#









Modify OSPFv2 Intervals
Note: The default Hello and Dead intervals are based on best practices and should only be altered in rare situations.


R1(config)# interface g0/0/0 
R1(config-if)# ip ospf hello-interval 5 
R1(config-if)# ip ospf dead-interval 20 
R1(config-if)# 
*Jun  7 04:56:07.571: %OSPF-5-ADJCHG: Process 10, Nbr 2.2.2.2 on GigabitEthernet0/0/0 from
FULL to DOWN, Neighbor Down: Dead timer expired 
R1(config-if)# end 
R1# show ip ospf neighbor 
Neighbor ID     Pri   	State           Dead Time   Address         Interface
3.3.3.3          		 0   		FULL/  -        00:00:37    10.1.1.13       GigabitEthernet0/0/1
R1#






Propagate a Default Static Route in OSPFv2
Your network users will need to send packets out of your network to non-OSPF networks, such as the internet. This is where you will need to have a default static route that they can use. In the topology in the figure, R2 is connected to the internet and should propagate a default route to R1 and R3. The router connected to the internet is sometimes called the edge router or the gateway router. However, in OSPF terminology, the router located between an OSPF routing domain and a non-OSPF network is called the autonomous system boundary router (ASBR).



All that is required for R2 to reach the internet is a default static route to the service provider.

Note: In this example, a loopback interface with IPv4 address 64.100.0.1 is used to simulate the connection to the service provider.
To propagate a default route, the edge router (R2) must be configured with the following:
• A default static route using the ip route 0.0.0.0 0.0.0.0 [next-hop-address | exit-intf] command.
• The default-information originate router configuration command. This instructs R2 to be the source of the default route information and propagate the default static route in OSPF updates.

In the following example, R2 is configured  with a loopback to simulate a connection to the internet. Then a default  route is configured and propagated to all other OSPF routers in the  routing domain.

Note: When configuring static  routes, best practice is to use the next-hop IP address. However, when  simulating a connection to the internet, there is no next-hop IP  address. Therefore, we use the exit-intf argument



R2(config)# interface lo1
R2(config-if)# ip address 64.100.0.1 255.255.255.252 
R2(config-if)# exit
R2(config)# ip route 0.0.0.0 0.0.0.0 loopback 1
Default route without gateway, if not a point-to-point interface, may impact performance
R2(config)# router ospf 10
R2(config-router)# default-information originate
R2(config-router)# end
R2#



