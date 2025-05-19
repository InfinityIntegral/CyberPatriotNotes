Singaporean Tech Support
README for network building in Cisco Packet Tracer

You have been asked by AFA Singapore Network Technologies to bring up a new network infrastructure. This Infrastructure consists of three sites:
Central Site which is abbreviated to CS
Remote Site 1 which is abbreviated to RS1
Remote Site 2 which is abbreviated to RS2
Communication between these sites is through the internet. Each site has two ISP connections for redundancy.
When you are finished with the configurations the PCs in all three sites should be able to ping each other. The PCs should also be able to browse to the AFA server.

Instructions
SAVE YOUR PACKET TRACER FILE OFTEN.
Common Configuration/ Hint:
Be sure to power on equipment that is not already on. This is done via the "Physical Tab" on a device.
On all Routers and Switches configure the enable secret password to be "SGNetworkTech"
Exact commands will be in quotation marks to help you know when the command starts and stops. Please do not type the quotation marks when you are instructed to type a command.
Note that the default mode for the distribution router interfaces is switchport and ip routing is not enabled
- In each section PC addresses are provided. You will have to determine subnet mask and gateway by looking at the distribution router vian configurations. DNS is provided below.
DNS server - 16.34.33.2
AFA server - www.afa.com

CS Internet Router:
- Host name - CSInternetRouter
- IP Addressing:
- Interface Gig 0/0 - 16.34.34.2/24
- Interface Gig 0/1 - 155.74.10.2/30
- Interface Gig 0/2 - 172.16.0.1/30
- Routing:
- OSPF should be configured under process 1 and all network statements in area O
- Connection to ISP 1 should have OSPF enabled
- Connection to ISP 2 should have OSPF enabled
- Interface Gig 0/2 should have OSPF enabled
- ISP 1 and 2 would like us to use values that are half the default value for hello and dead intervals. Internal networks should stay at default.
- Use the network address for the subnet and matching wildcard mask in the network statement for the connection to ISP 2. Use the specific interface address and the required mask in the network statement for the connection to ISP 1 and on Gig 0/2.
- Configure the link to ISP2 with an OSPF cost value of 30 so the ISP1 path is preferred for internet traffic when both links are up.

CS Core Router:
- Host name - CSCoreRouter
- IP addressing:
- Interface Gig 0/0 - 172.16.0.2/30
- Interface Gig 0/1 - 172.16.0.5/30
- Interface gig 0/2 - 172.16.0.9/30
- Routing:
- OSPF should be configured under process 1 and all network statements in area 0
- OSPF should be enabled for int Gig 0/0
- Use the specific interface address and the required mask in the network statement for Gig 0/0
- Configure the following command in the OSPF process "redistribute static metric 1 subnets"
- Routing to the downstream networks on the distribution routers should be via static routes (These networks are the VLAN interfaces on the CS distribution routers. You can find these networks in the instructions for the CS distribution routers). Because VLAN 3 has many more hosts than the other VLANs we want to dedicate one of the two paths to that VLAN.
The other two VLANs will use the other link. However, we want redundancy in case of a failure so we will use floating static routes.
VLAN 3 main route next hop should point to the directly connected interface on Distribution Router 2
VLAN 3 secondary route next hop should point to the directly connected interface on Distribution Router 1
VLAN 1 and 2 main route next hop should point to the directly connected interface on Distribution Router 1
VLAN 1 and 2 secondary route next hop should point to the directly connected interface on Distribution Router 2
Secondary routes should have distance of 254

CS Distribution Router 1:
- Host name - CSDistributionRouter1
- IP addressing:
- Interface Gig 1/0/1 - 172.16.0.6/30
- Interface Gig 1/0/2 - 172.16.0.13/30
- Interface VLAN 1:
Network address - 172.16.0.16
Number of hosts available - 14
This interface should use the first available address in the subnet
- Interface VLAN 2:
Network address - 172.16.0.32
Number of hosts available - 6
This interface should use the first available address in the subnet
- Interface VLAN 3:
Interface address 192.168.0.1
Number of hosts available - 2046
- Routing:
- Routing to the other company networks and the internet is accomplished by two static floating default routes.
Main route should have a default gateway of 172.16.0.5
Secondary route should have a default gateway of 172.16.0.14 and distance of 254
- Switching:
- VLANs 1-3 should exist on the switch
- Interfaces Gig 1/0/3 and Gig 1/0/4 should be bundled together as an LACP etherchannel named port-channel 1
- Interfaces Gig 1/0/5 and Gig 1/0/6should be bundled together as an LACP etherchannel named port-channel 2
- Both Port Channels should be dotiq trunks

CS Distribution Router 2:
- Host Name - CSDistributionRouter2
- IP addressing:
- Interface Gig 1/0/1 - 172.16.0.10/30
- Interface Gig 1/0/2 - 172.16.0.14/30
- Interface VLAN 1:
Network address - 172.16.0.16
Number of hosts available - 14
This interface should use the second available address in the subnet
- Interface VLAN 2:
Network address - 172.16.0.32
Number of hosts available - 6
This interface should use the second available address in the subnet
- Interface VLAN 3:
Interface address 192.168.0.2
Number of hosts available - 2046
- Routing:
- Routing to the other company networks and the internet is accomplished by two static floating default routes.
Main route should have a default gateway of 172.16.0.9
Secondary route should have a default gateway of 172.16.0.13 and distance of 254
- Switching:
- VLANs 1-3 should exist on the switch
- Interfaces Gig 1/0/3 and Gig 1/0/4 should be bundled together as an LACP etherchannel named port-channel 1
- Interfaces Gig 1/0/5 and Gig 1/0/6 should be bundled together as an LACP etherchannel named port-channel 2
- Both Port Channels should be dotiq trunks

CS Switch 1:
- Host name - CSSwitch1
- Switching:
- Interfaces Fast 0/1 and Fast 0/2 should be bundled together as an LACP etherchannel named port-channel 1
- Interfaces Fast 0/3 and Fast 0/4 should be bundled together as an LACP etherchannel named port-channel 2
- Interfaces Fast 0/5 and Fast 0/6 should be bundled together as an LACP etherchannel named port-channel 3
- All port channels should be trunks
- All PC ports should be in access mode
- Interface Fast 0/7 - VLAN 1
- Interface Fast 0/8 - VLAN 2
- Interface Fast 0/8 - VLAN 3

CS Switch 2:
- Host name CSSwitch2
- Switching:
- Interfaces Fast 0/1 and Fast 0/2 should be bundled together as an LACP etherchannel named port-channel 1
- Interfaces Fast 0/3 and Fast 0/4 should be bundled together as an LACP etherchannel named port-channel 2
- Interfaces Fast 0/5 and Fast 0/6 should be bundled together as an LACP etherchannel named port-channel 3
- All port channels should be trunks
- All PC ports should be in access mode
- Interface Fast 0/7 - VLAN 1
- Interface Fast 0/8 - VLAN 2
- Interface Fast 0/9 - VLAN 3

CS PCs:
- PC1 - 172.16.0.19
- PC2 - 172.16.0.35
- PC3 - 192.168.0.3
- PC4 - 192.168.0.4
- PC5 - 172.16.0.20
- PC6 - 172.16.0.36

RS1 Internet Router:
- Host name - RS1InternetRouter
- IP addressing:
- Interface Gig 0/0 - 16.34.35.2/24
- Interface Gig 0/1 - 155.74.10.6/30
- Interface Gig 0/2 - 172.16.1.1/30
- Routing:
- OSPF should be configured under process 1, and all network statements in area 0
- Connection to ISP 1 should have OSPF enabled.
- Connection to ISP 2 should have OSPF enabled.
- Interface Gig 0/2 should have OSPF enabled.
- ISP 1 and 2 would like us to use values that are half the default value for hello and dead intervals. Internal networks should stay at default.
- Use the network address for the subnet and matching wildcard mask
in the network statement for the connection to ISP 2. Use the specific interface address and the required mask in the network statement for the connection to ISP 1 and Gig 0/2.
- Using a cost value of 30 configure the router so that it will always prefer ISP 1 when both ISP connections are up.

RS1 Core Router:
- Host name RS1CoreRouter
- IP addressing:
- Interface Gig 0/0 - 172.16.1.2/30
- Interface Gig 0/1 - 172.16.1.5/30
- Interface gig 0/2 - 172.16.1.9/30
- Routing:
- OSPF should be configured under process 1 and all network statements in area 0
- OSPF should be enabled on int Gig 0/0
- Use the specific interface address and the required mask in the network statement for Gig 0/0
- Configure the following command in the OSPF process "redistribute static metric 1 subnets"
- Routing to the downstream networks on the distribution routers should be via static routes (These networks are the VLAN interfaces on the RS1 distribution routers. You can find these networks in the instructions for the RS1 distribution routers). There should be two routes for each network. One with a next hop of the directly connected interface on Distribution Router 1 and the second with a next hop of the directly connected interface on Distribution Router 2.

RS1 Distribution Router 1:
- Host name RS1DistributionRouter1
- IP addressing:
Interface Gig 1/0/1 - 172.16.1.6/30
- Interface Gig 1/0/2 - 172.16.1.13/30
- Interface VLAN 1:
Network address - 172.16.1.16
Number of hosts available - 14
This interface should use the first available address in the subnet
- Interface VLAN 2:
Network address - 172.16.1.32
Number of hosts available - 30
This interface should use the first available address in the subnet
- Interface VLAN 3:
Network address - 172.16.1.64
Number of hosts available - 6
This interface should use the first available address in the subnet
- Routing:
- Routing to the other company networks and the internet is accomplished by two static floating default routes.
Main route should have a next hop of 172.16.1.5
The secondary route should have a next hop of 172.16.1.14 and distance of 254
- Switching:
- VLANs 1-3 should exist on the switch
- Interfaces Gig 1/0/3 and Gig 1/0/4 should be bundled together as an LACP etherchannel named port-channel 1
- Interfaces Gig 1/0/5 and Gig 1/0/6 should be bundled together as an LACP etherchannel named port-channel 2
- Both Port Channels should be dotiq trunks

RS1 Distribution Router 2:
- Host name - RS1DistributionRouter2
- IP addressing:
- Interface Gig 1/0/1 - 172.16.1.10/30
- Interface Gig 1/0/2 - 172.16.1.14/30
- Interface VLAN 1:
- Network address - 172.16.1.16
Number of hosts available - 14
- This interface should use the second available address in the subnet
- Interface VLAN 2:
Network address - 172.16.1.32
- Number of hosts available - 30
This interface should use the second available address in the subnet
- Interface VLAN 3:
Network address - 172.16.1.64
Number of hosts available - 6
This interface should use the second available address in the subnet
- Routing:
- Routing to the other company networks and the internet is accomplished by two static floating default routes.
Main route should have a next hop of 172.16.1.9
Secondary route should have a next hop of 172.16.1.13 and distance of 254
- Switching:
- VLANs 1-3 should exist on the switch
- Interfaces Gig 1/0/3 and Gig 1/0/4 should be bundled together as an LACP etherchannel named port-channel 1
- Interfaces Gig 1/0/5 and Gig 1/0/6 should be bundled together as an LACP etherchannel named port-channel 2
- Both Port Channels should be dotiq trunks

RS1 Switch 1:
- Host name
Host name - RS1Switch1
- Switching:
- Interfaces Fast 0/1 and Fast 0/2 should be bundled together as an LACP etherchannel named port-channel 1.
- Interfaces Fast 0/3 and Fast 0/4 should be bundled together as an LACP etherchannel named port-channel 2.
- Interfaces Fast 0/5 and Fast 0/6 should be bundled together as an LACP etherchannel named port-channel 3.
- All port channels should be trunks
- All PC ports should be in access mode
- Interface Fast 0/7 - VLAN 1
- Interface Fast 0/8 - VLAN 2
- Interface Fast 0/8 - VLAN 3

RS1 Switch 2:
- Host name - RS1Switch2
- Switching:
- Interfaces Fast 0/1 and Fast 0/2 should be bundled together as an LACP etherchannel named port-channel 1.
- Interfaces Fast 0/3 and Fast 0/4 should be bundled together as an LACP etherchannel named port-channel 2.
- Interfaces Fast 0/5 and Fast 0/6 should be bundled together as an LACP etherchannel named port-channel 3.
- All port channels should be trunks
- All PC ports should be in access mode
- Interface Fast 0/7 - VLAN 1
- Interface Fast 0/8 - VLAN 2
- Interface Fast 0/9 - VLAN 3

RS1 PCs:
- PC1 - 172.16.1.19
- PC2 - 172.16.1.35
- PC3 - 172.16.1.67
- PC4 - 172.16.1.20
- PC5 - 172.16.1.36
- PC6 - 172.16.1.68

