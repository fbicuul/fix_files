
Dynamic routing protocols are commonly used in the following scenarios:

• In networks consisting of more than just a few routers
• When a change in the network topology requires the network to automatically determine another path
• For scalability. As the network grows, the dynamic routing protocol automatically learns about any new networks.

The table shows a comparison of some the differences between dynamic and static routing.


| Feature | Dynamic Routing | Static Routing |
| Configuration complexity | Independent of network size | Increases with network size |
| Topology changes | Automatically adapts to topology changes | Administrator intervention required |
| Scalability | Suitable for simple to complex network topologies | Suitable for simple topologies |
| Security | Security must be configured | Security is inherent |
| Resource Usage | Uses CPU, memory, and link bandwidth | No additional resources needed |
| Path Predictability | Route depends on topology and routing protocol used | Explicitly defined by the administrator |





|  | Interior Gateway Protocols |  |  |  | Exterior Gateway Protocols |
|  | Distance Vector |  | Link-State |  | Path Vector |
| IPv4 | RIPv2 | EIGRP | OSPFv2 | IS-IS | BGP-4 |
| IPv6 | RIPng | EIGRP for IPv6 | OSPFv3 | IS-IS for IPv6 | BGP-MP |




