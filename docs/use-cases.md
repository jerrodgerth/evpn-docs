# EVPN Data Center Use Cases

### Layer 2 or Layer 3 Inter Data Center Services


### Layer 2 or Layer 3 Intra Data Center Services


### Layer 2 or Layer 3 Data Center Interconnect

 
Figure caption: Layer 2 or Layer 3 Data Center Interconnect

This DCI application enables scalable Layer 2 or Layer 3 services for virtualized data centers with control plane signaling of IP/MAC mobility for VMs that move between data centers.
Local IP gateways at each PE optimize routing, so that external traffic is sent to the closest exit instead of over the internal network to a remote gateway.
Integrated Layer 2 switching and Layer 3 routing over the same interface or VLAN enables flexible service delivery to VMs.


### Data Center to Internet/WAN VPNs/Cloud Interconnect 

 
Figure caption: Layer 2 and Layer 3 Overlay VPNs

EVPN is use to provide integrated Layer 2 and Layer 3 VPN services on a single interface and single VLAN to customers.
There’s only one VPN technology for both services, and no need for multiple VPN protocols.
All-active or single-active PE to CE connections can be supported depending on the requirements for redundancy and load-balancing.
The EVPN services can be provided over any core network as described above, MPLS cores can use EVPN-MPLS and IP cores can use EVPN-VXLAN.

Figure caption: Flexible Layer 2 and Layer 3 VPN Solution

EVPN-VXLAN works over any IP network to provide a flexible Layer 2 and Layer 3 VPN.
This application just requires IP connectivity between sites, no MPLS or any special configuration by the IP service provider is needed.
There could even be several different IP networks in the path, for example if a network operator bought IP service from different providers at different locations.
The service provider network is completely transparent to EVPN, and the EVPN overlay is completely transparent to service providers – it’s just IP traffic.
Routing and MAC/IP advertisement within EVPN are controlled via IBGP or eBGP between PEs.
