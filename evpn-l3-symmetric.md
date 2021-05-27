# EVPN L3 - Symmetric IRB - Prefix Routing Model 

Figure – EVPN-VXLAN L3 model – Symmetric forwarding illustrates the forwarding in a symmetric model. 
 

In Figure – EVPN-VXLAN L3 model – Symmetric forwarding: 
1.	As in the asymmetric model, the frame is classified for bridge table lookup on mac-vrf and processed for routing in ip-vrf red. 
2.	Contrary to the asymmetric model, a longest prefix match does not yield a local subnet, but a remote subnet reachable via [VTEP:VNI]=[2.2.2.2:3] and inner MAC DA R-MAC2. Depending on the sub-model implemented, Interface-less (IFL) or Interface-ful (IFF), that information is found in the ip- vrf route-table directly (IFL) or via recursive lookup into the ip-vrf route-table and a core SBD mac- vrf. NOTE: refer to Annex to see the details and differences between the IFL and IFF sub-models. 
3.	At the egress PE, the packet is classified for an ip lookup on the ip-vrf red. Again, the way the ip-vrf red is found, depends on whether the implemented sub-model is IFL or IFF. The ip lookup yields a local IRB interface. 
4.	Subsequent ARP and MAC lookups provide the information to send the routed frame onto the wire to sub-interface 2. 
Note that the “symmetric” name refers to the fact that a mac and ip lookups are needed at ingress, while ip and mac lookups are performed at egress. 
Compared to the asymmetric model, this model scales much better since hosts’ ARP and FDB entries will be installed only on the directly attached leafs and not on all the leafs of the tenant.

CE connectivity options – single and dual homed:
	EVPN covers a variety of connectivity options for CE devices into EVPN L2 and L3 services. These include single and dual homing with various support forwarding behaviours and parameters, including Active/Standby and All Active. These will be covered in their own section of this document.
