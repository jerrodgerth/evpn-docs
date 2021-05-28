# EVPN Data Planes

There are several I-Ds for data plane encapsulation, MPLS (RFC-7432), PBB (RFC-7623), and NVO (RFC 8365).
A summary of the currently supported NVO VXLAN encapsulation SR-Linux supports is provided below.
The data plane is separated from the control plane, so the EVPN functionality is the same over any data plane encapsulation.


### Network Virtualization Overlay (NVO)

NVO defines 3 encapsulation types; VXLAN, NVGRE, and MPLSoGRE. While each could be used for a variety of use cases, the industry has gravitated towards use of VXLAN primarily in the data center.
For this reason, Nokia’s SR Linux has implemented the VXLAN data plane (RFC 7348) for EVPN L2 and L3 services.

FIGURE

VXLAN addresses the data plane needs for overlay networks within virtualized data centers accommodating multiple tenants.
The main attributes of the VXLAN encapsulation are: 

* VXLAN is an overlay network encapsulation used to carry MAC traffic between VMs over a logical Layer 3 tunnel. 
* Avoids the Layer 2 MAC explosion, because VM MACs are only learned at the edge of the network.
Core nodes simply route the traffic based on the destination IP (which is the system IP address of the remote PE or VTEP-VXLAN Tunnel End Point). 
* Supports multi-path scalability through ECMP (to a remote VTEP address, based on source UDP port entropy) while preserving the Layer 2 connectivity between VMs.
xSTP is no longer needed in the network. 
* Supports multiple tenants, each with their own isolated Layer 2 domain.
The tenant identifier is encoded in the VNI field (VXLAN Network Identifier) and allows up to 16M values, as opposed to the 4k values provided by the 802.1q VLAN space. 

As shown in Figure XXX, VXLAN encapsulates the inner Ethernet frames into VXLAN + UDP/IP packets.
The main pieces of information encoded in this encapsulation are: 

* VXLAN header (8 bytes)
* Flags (8 bits) where the I flag is set to 1 to indicate that the VNI is present and valid. The rest of the flags (“Reserved” bits) are set to 0
* Includes the VNI field (24-bit value) or VXLAN network identifier.
It identifies an isolated Layer 2 domain within the DC network. 
* The rest of the fields are reserved for future use. 

FIGURE
