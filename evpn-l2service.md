# EVPN L2 Service

The basic SR Linux configuration of EVPN-VXLAN for L2 consists of:
1.	A bridged sub-interface, which defines the port, VLAN, and policy associated with traffic entering the L2 EVPN service
2.	A vxlan-interface, that contains the ingress VNI of the VXLAN tunnel used when transmitting VXLAN packets.
3.	A MAC-VRF network-instance where, in addition to other sub-interfaces, the vxlan-interface is attached. 
4.	BGP-EVPN is also enabled in the same MAC-VRF with a minimum configuration of the EVI and the network-instance vxlan-interface associated to it.

 

EVPN L2 Basic Routes
The basic EVPN Layer-2 model requires the implementation of the BGP-EVPN address family and the support for two route types that are illustrated in the following Figure.
•	EVPN MAC/IP route (or type 2, RT2)
•	EVPN Inclusive Multicast Ethernet Tag route (IMET or type 3, RT3)
Figure – EVPN-VXLAN Basic L2 routes
 
The MAC/IP route is used to convey the MAC and IP information of hosts connected to sub-interfaces in the MAC-VRF. The IMET route is advertised as soon as bgp-evpn is enabled in the MAC-VRF and it has a twofold purpose:
1.	The auto-discovery of the remote VTEPs attached to the same EVI.
2.	The creation of a default flooding list in the MAC-VRF so that BUM frames are replicated.
As in SROS, the MAC Mobility extended community is required along with the RT2s for mobility procedures (Sequence Number) and for the Static bit that will be used to advertised static MACs and learned-static MACs. Both RT2 and RT3 are advertised along with the bgp encapsulation extended community.
The advertisement of the MAC/IP and IMET routes can be controlled on a per MAC-VRF basis.




EVPN L2 Packet Walkthrough
The following figures illustrate a basic packet walkthrough for the communication between two hosts of the same broadcast domain, including the initial ARP resolution. LEAF-1/2/3 use the information conveyed in the IMET routes from the remote NVEs to create a flooding list that consists of the [VTEP:VNI] pairs of the two remote Leafs. In addition, the leafs populate their Bridge Table with the MAC addresses contained in the received MAC/IP routes.
Figure – EVPN-VXLAN L2 model – Packet walkthrough
 
The steps depicted in Figure "EVPN-VXLAN L2 model – BUM packet walkthrough" can be summarized as:
1.	Initial Control Plane exchange: as soon as the network-instances for the same route-target are enabled on the three leafs, Inclusive Multicast Ethernet Tag (IMET) routes are exchanged. Leaf nodes create their flood list for the mac-vrf upon processing the received IMET routes.
2.	Frames are identified for a mac lookup in the bridge-table based on the ingress sub-interface. The sub-interface will also provide the ingress vlan manipulation action.
3.	The mac lookup is twofold:
a.	A mac destination address lookup yields no entry (since the MAC DA is broadcast), hence the frame is identified to forward to the flooding list.
b.	A mac source address lookup yields no entry, hence the frame is sent to CPM for mac learning.
4.	The frame is flooded, and the MAC SA advertised in EVPN.
a.	The frame to be flooded is encapsulated in a VXLAN packet with the source-vtep address and VNI for the service. Two copies (each one with a different tunnel IP DA) are sent to the default network-instance to be routed to the remote vteps LEAF-2 and LEAF-3. ECMP and LAG spraying are applied to each packet. Also, if the outbound sub-interface is tagged, a vlan tag will be pushed into the egress packet.
b.	In parallel, the MAC SA is learned by the l2maclearn_mgr, selected/installed by the l2mac_mgr and advertised by evpn_mgr in a MAC/IP route.
5.	At the egress Leafs, the flooded VXLAN packet is identified for mac-vrf 1 processing, based on the outer VID and VXLAN VNI. At the same time, the MAC/IP routes containing M1 are received, imported and installed in LEAF-2/3 bridge tables.
6.	The inner MAC DA is identified for mac lookup in the bridge table. Since the MAC DA lookup fails to find an entry, the decapsulated frame will be flooded based on the local flood list and observing the split-horizon rules (that avoid forwarding back to vxlan), thus providing loop prevention. Vlan manipulation is applied based on the egress sub-interfaces configuration.

Figure – EVPN-VXLAN L2 model – Unicast packet walkthrough
 

7. The ARP Request eventually gets to the CE with MAC M2. It replies with a unicast ARP reply destined to M1. The frame is classified for mac lookup as in steps 2 and 3.
8. This time the mac destination lookup yields an entry in the bridge table. The source mac lookup follows step 3. The process of learning, advertising and installing M2 is the same as for M1.
9. The unicast VXLAN packet is forwarded to LEAF-1 using its VTEP.
10. The packet is classified for mac-vrf processing based on the outer VID and VNI.
11. Now the mac destination lookup yields an entry pointing at sub-interface eth-1/1-1. The inner frame is forwarded to the sub-interface, after going through the egress vlan manipulation process.

Loop prevention:
	As is highlighted above, when an unknown MAC-DA is received from a host, the packet is flooded to the local flood list, as well as to the remote devices learnt via the IMET routes sharing the same VNI. When unknown MAC-DAs are received from a VXLAN tunnel (ie flooded from a remote endpoint), again the packet is flooded to the local flood list. 
In these cases though, as well as when dual-homing of CEs occurs with the associated EVPN Segment Identifier (ESI) definition, split-horizon rules within SR LINUX ensure that the flooded packet is NOT flooded back from where it was received, this being either a host, ESI, or VXLAN tunnel. The result of this is effective loop prevention in an EVPN L2 service. 
 

