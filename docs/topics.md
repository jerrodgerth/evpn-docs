# Suggested Topics

!!!note
	**This was the original brainstorming list of topics within EVPN**


#### EVPN services:

* Intro – what problems EVPN is solving
	* Single protocol for all overlay services – what RFCs, definition of each Route Type 1-5
		* Table of route types?
	* Transport agnostic
	* Customer separation
	* re-uses BGP control plane – for advertisement per tenant with MP-BGP
	* Interop (EANTC)
* Overlay and Underlay
* EVPN and VXLAN
* Overlay Control Plane 
	* MP-BGP and use of RRs for simplicity
	* Peering considerations and options – iBGP? EBGP? Jorge question.
	* Complicating with eBGP? New next-hops

#### SR LINUX EVPN

* L2 EVPN
	* We offer VXLAN tunneling
	* Diagram of L2 encap and advertisement via BGP
	* Handling of BUM and known unicast traffic
	* All Active, MH, with mobility
	* Loop prevention
* L3 EVPN 
	* EVPN-IRB-unicast
	* We offer VXLAN tunneling
	* Diagram of L3 encap and advertisement via BGP
	* ARP Proxy
	* What we support for inter-subnet forwarding
		* Symmetric and Asymmetric IRB
		* Interface-ful with SBD.
		* Interface-ful with unnumbered SBD IRB.
		* Interface-less.
	* All Active, MH, with mobility
	* Good default versus optimizations for inter/intra-DC etc. 


#### Overlay Overview

* What and Why do we need them?
* Layer 2 vs Layer 3 
	* Differences between them in design and where gateways exists etc  Life of a packet in each case.
* BGP EVPN
	* Peering Design
		* iBGP peering, EVPN RRs etc  Suggest best practice from Nokia
		* Tuning BGP for EVPN
* Loop Prevention


#### Overlay and underlay separation

* EVPN overlays with MP-BGP
* Underlay with IGP (OSPF or ISIS) or BGP
	* Distance vector versus link state considerations
	* Resiliency mechanism (detection with BFD?)
* SRL – underlay – OSPF/ISIS or eBGP
	* RFC for hyperscale DC design uses eBGP


#### RFC 7938 - Use of BGP for Routing in Large-Scale Data Centers

* Simplicity with ISIS for autodiscovery already possible 
* No need to learn something else 
* AS-path traceability in the underlay – know the path because each POD has a different AS
	* Not possible with IGP 
* More route policy possibilities with BGP
* Auto-discovery options with BGP – lldp w/ extensions
	* LSOE – LSVR working group is another option
		* Auto-creates BGP ptop sessions
* Convergence – IGP is typically faster, but loots of work make BGP fast as well. 
* BFD for BGP for sure, ask bruce about BFD for IGP. 


#### Control Plane BGP peering options
 
* eBGP or iBGP
	* typically iBGP – with router reflectors
		* simple
	* if eBGP – next hop unchanged allows spines to be route servers
		* common across vendors
* EANTC – eBGP in underlay, and iBGP in overlay
	* Arista pushed it

#### which is better for:

|                               | eBGP            | iBGP               |  
| ----------------------------- | --------------- | ------------------ |  
| Big fabric                    | no advantage    | no advantage       |  
| Small fabric                  | no advantage    | no advantage       |  
| Lots of /32s                  | no advantage    | no advantage       |  
| Simplicity of ASN assignments | more ASNs       | single ASN         |  
| Troubleshooting               | path attributes | slightly less info |  
