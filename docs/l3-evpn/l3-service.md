# EVPN L3 Services

MAC-VRF, IRB, IP-VRF

The basic EVPN Layer-3 model builds upon the model of the EVPN routes described in section EVPN-VXLAN L2 and extends it with a new route type to support inter-subnet-forwarding: 

### EVPN IP Prefix route (or type 5, RT5) 

The IP Prefix route is used to convey IP Prefixes of any length and family that need to be installed in the ip- vrfs of remote leaf nodes.
The EVPN Layer-3 model is also known in the standards as the EVPN IRB Model (because it uses IRB interfaces in ip-vrfs connected to mac-vrfs), and it has two required modes of operation:

* Asymmetric IRB - see [IETF-EVPN-IRB] 
* Symmetric IRB, which can be classified as: 
	* Host routing model using RT2s - see [IETF-EVPN-IRB] 
	* Prefix routing model using RT5s - see [IETF-EVPN-L3]. This one can be classified as: 
		* Interface-less ip-vrf-to-ip-vrf model (IFL) 
		* Interface-ful ip-vrf-to-ip-vrf model (IFF) 

The asymmetric IRB mode is required in SRLinux, since it is considered a basic mode of operation for IRB interfaces.

As for the symmetric IRB model, the “Host routing model using RT2s” uses RT2s with an L2 _and_ an L3 VNI, so that the IP address conveyed in the route is installed in the ip-vrf route-table as a host route, associated to the L3 VNI. This model is NOT REQUIRED in SRLinux.
The Prefix routing model is REQUIRED in SRLinux. The following table summarizes the models and requirements.

EVPN L3 Models:

| Model | | | Value | Industry Support | SR Linux Support |
| --- | --- | --- | --- | --- | --- |
| Asymmetric | | | Basic IRB forwarding | All vendors | Yes |
| Symmetric | Host Routing with RT2s | | The same RT2 is used to populate FDB, ARP, and route-table (as opposed to RT2 for FDB/ARP, and RT5 for route-table) | Cisco only | No |
| | Prefix Routing with RT5s | Interface Less (IFL)	| Uses RT5 in a similar way to IPVPN routes | All vendors | Yes |
| | | Interface-Full (IFF) - numbered | Uses RT5 with recursive resolution to RT2 |	SROS, Juniper MX | Yes |
| | | Interface-Full (IFF) - unnumbered | Same as above but does not use IP addresses in the core IRB interfaces | SROS, Nuage, Cisco Nexus | Yes |

The standard SR Linux configuration components of L3 services are illustrated below. 

FIGURE - EVPN-VXLAN L3 INTER-SUBNET FORWARDING IN OVERLAY DCS

To understand the EVPN L3 model which will be most suitable to a deployment, each option must be explained.