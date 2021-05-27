# Key EVPN Benefits

There are several important EVPN benefits that enable service providers to simplify their networks and offer advanced Ethernet services.

### Integrated Services

Delivering Layer 2 and Layer 3 services over the same interface, VLAN and VPN using a single VPN technology has been cumbersome until now.
While service providers can offer services with VPLS and L3VPNs, it requires multiple technologies and multiple customer service interfaces.
L3VPN-like operation with MP-BGP as the control plane protocol offers more scalability and control over learning and flooding.

### Network Efficiency

Multihoming with all-active forwarding, and load balancing between provider edge routers (PEs) is key to providing redundant and efficient services within the data center, between them for DCI services or for hybrid cloud interconnect services.
All-active forwarding means that all links are active and used in the network, and there is no wasted idle capacity for standby links.
More efficient hybrid service can be delivered over a single interface or VLAN, instead of using multiple interfaces or VLANs for multiple services.

### Design Flexibility

Since the control plane and data plane are separated, several MPLS or IP data plane encapsulation choices are available to meet core network requirements.  Provisioning and management using a single VPN technology is simpler, instead of managing a Layer 2 and Layer 3 VPN technology.

### Greater Control

MAC/IP provisioning from a network management system database enables programmatic network control, and control plane signaling maintains a consistent signaled FDB instead of flooding and learning in the data plane.
Proxy ARP/ND functionality allows PEs to respond to ARP/ND requests locally, which reduces or eliminates flooding.

EVPN also builds on consensus and cooperation between router vendors and service providers working together on a simple and interoperable technology.
Since 2014, Nokia has worked in the IETF to lead the standardization of EVPN (greater than 20 Internet drafts) and committed to interop testing at EANTC with all major EVPN vendors.
Nokia completeness of EVPN feature set is widely recognized in the industry, and has become the basis for delivery of enhanced VPN services by many operators worldwide.
