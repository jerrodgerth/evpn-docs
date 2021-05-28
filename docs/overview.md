# EVPN Overview

Now that we’ve discussed the motivation and key benefits of EVPN, let’s dive into how the technology is developing.
EVPN technology is driven in the IETF L2VPN WG, comprised of a few mature base internet drafts (I-Ds) as well as a plethora of new I-Ds that extend EVPN functionality (over 20 I-Ds in total).
No more changes are expected on the base specification, and there are several shipping EVPN implementations.
The authors of the EVPN requirements and base specification I-Ds come from a diverse set of router vendors (Nokia (Alcatel-Lucent at the time), Cisco, Juniper) and network operators (Arktan, AT&T, Bloomberg, Verizon).
EVPN isn’t just another bright idea that someone came up with in a dark cave by themselves, it’s a collaboration between multiple router vendors and network operators that are working together to define this new technology.

FIGURE

EVPN introduces the concept of a separate control plane and data plane, which enables IP/MAC learning to be performed in the control plane instead of the data plane.
MP-BGP is used as the control plane protocol, which brings proven and inherent BGP control plane scalability to MAC routes, and can even be extended with hierarchy and route reflection.
Using control plane learning provides a consistent signaled FDB in any size network instead of relying on flooding and learning. Control plane learning also offers greater control over MAC learning, what is signaled, from where and to whom, and maintains virtualization and isolation of EVPN instances.
MP-BGP advertises MACs and IPs for next hop resolution with an EVPN NLRI, which fully supports IPv4 and IPv6 in the control and data plane.
Yes that’s right, there are no extensions to EVPN for IPv6, it’s completely integrated and supported just like IPv4 from the very beginning.

For reference, here are the BGP route types defined for EVPN, and their uses:

| Route Type | Description | RFC | Use |
| --- | --- | --- | --- |
| 1 | Ethernet auto-discovery route	| RFC 7432 | Advertises list of EVPN Instances (EVIs) per Ethernet segment (ES) |
| 2 | MAC/IP advertisement route | RFC 7432 | Advertises MACs, reachability, and IP/MAC bindings |
| 3 | Inclusive multicast route | RFC 7432 | Advertises per VLAN and per ESI tunnel endpoint discovery for BUM traffic |
| 4 | Ethernet segment route | RFC 7432 | Redundancy group discovery and designated forwarder (DF) election |
| 5 | IP prefix route | Draft-ietf-bess-evpn-prefix-advertisement-11 | Advertises IP prefixes

Within EVPN, the separation of the control plane and data plane is key to providing flexibility, control, and efficiency in the network.

While we’ll explain the various dataplane encapsulating options supported by EVPN in the next section, the fact that there is an encapsulation dictates that there is both overlay and underlay routing and switching possible.
Overlay routing/switching being performed at the edge on ingress of packets into the EVPN services, and underlay routing/switching occurring based on the encapsulated packet in the service-unaware core of the network.
The overlays and underlay are making independent routing and switching decisions based on advertisements by their separate control planes.
In the underlay section of this document, the use of various control plane options, include eBGP, iBGP, ISIS, and OSPF are explained and compared.

For the advertisement of overlay MACs and IPs, EVPN supports the use of either eBGP or iBGP with the Multi-Protocol (MP) BGP extension.
The choice between these may be based on the operator’s preference, operations team’s skills set, feature support by the vendor, or based on the configuration requirement differences between them.
Both options support very large fabrics, high scale MAC/IP advertisements, tuning options for fast convergence, and similar troubleshooting options.
For Nokia, SR Linux supports both models, with the following operational considerations identified for operators to make their choice.

#### eBGP

* Configuration of an AS per overlay terminating node, and per spine switch
* AS reuse possible per overlay terminating node (or pair) 
* eBGP peering between all directly connected nodes in the network 
* Each non overlay terminating node (spine, super-spine), must act as a route server and re-advertise reachability of all overlay terminating nodes without modifying the next-hop
* This requires support for the eBGP feature - next-hop-unchanged
* Advantages
	* Overlay path captured in eBGP AS-path attributes
	* No requirement for route reflectors  
* Challenges
	* AS assignment and tracking
	* Additional configuration on spine nodes


#### iBGP

* Configuration of a single AS across the overlay terminating nodes
* iBGP peering:
	* Directly between all overlay terminating nodes (full mesh)
	* Indirectly between overlay terminating nodes via >=1 route reflectors
	* Route reflector must support EVPN family of route types
* Advantages
	* Simplicity of single AS design
* Challenges
	* Additional requirement for route reflectors 
	* Less path tracking capabilities in the overlay, but can still be done via underlay

While there are advantages and challenges for either eBGP or iBGP for the EVPN overlay control plane, the majority of vendors have been advocating iBGP at EANTC interop testing sessions in recent years.