# EVPN L3 - Asymmetric IRB Model 

The asymmetric model is considered to be the basic L3 forwarding model when the ip-vrf interfaces are all IRB based. The asymmetric model assumes that all the subnets of a tenant are local in the ip-vrf route-table, hence there is no need to advertise EVPN RT5s. Figure – EVPN-VXLAN L3 model – Asymmetric forwarding illustrates this model. 
Figure – EVPN-VXLAN L3 model – Asymmetric forwarding
 
 
In Figure – EVPN-VXLAN L3 model – Asymmetric forwarding: 
1.	The host with IP address 10.1 sends a unicast packet with destination 20.1 (host in a different subnet and remote leaf). Since the IP DA is in a remote subnet, the MAC DA will be resolved to the local default gateway, mac M-IRB1. The frame is classified for mac lookup on mac-vrf 1, and the result is IRB.1, which indicates that an IP DA lookup is required in ip-vrf red. 
2.	An IP DA longest prefix match in the route-table yields IRB.2, a local IRB interface, hence an ARP and MAC DA lookups are required in the corresponding IRB interface and mac-vrf bridge table. 
3.	The ARP lookup yields M2 on mac-vrf 2, and M2 lookup yields vxlan destination [VTEP:VNI]=[2.2.2.2:2]. The routed packet is encapsulated with the corresponding inner MAC header and VXLAN encapsulation before being sent to the wire. 
4.	At the egress leaf, the frame is classified for a mac lookup on mac-vrf 2, which yields a local sub- interface where the frame is sent to. 
Note that the “asymmetric” name refers to the fact that there are more lookups performed at the ingress leaf than at the egress leaf (as opposed to “symmetric” which implies the same number of lookups at ingress and egress). 
While this asymmetric model allows inter-subnet-forwarding in EVPN-VXLAN networks in a very simple way, it has some scale implications: 
•	It requires the instantiation of all the mac-vrfs of all the tenant subnets on all the leafs attached to the tenant. This is a considerable configuration burden that is only needed when all the leafs have hosts in all the subnets of the tenant. 
Since all the mac-vrfs of the tenant are instantiated, FDB and ARP entries are consumed for all the hosts in all the leafs of the tenant. E.g., in Figure – EVPN-VXLAN L3 model – Asymmetric forwarding, leaf-1 needs to install the ARP and FDB entries for host with IP address 20.1, even if the host is not attached to leaf-1. Likewise, for host 10.1 in leaf-2. 
Those scale implications make a symmetric model needed in any DC deployment. 
 
