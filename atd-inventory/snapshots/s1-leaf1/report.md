# s1-leaf1 Commands Output

## Table of Contents

- [show lldp neighbors](#show-lldp-neighbors)
- [show ip interface brief](#show-ip-interface-brief)
- [show interfaces description](#show-interfaces-description)
- [show version](#show-version)
- [show running-config](#show-running-config)
## show interfaces description

```
Interface                      Status         Protocol           Description
Et1                            up             up                 MLAG_s1-leaf2_Ethernet1
Et2                            up             up                 P2P_s1-spine1_Ethernet2
Et3                            up             up                 P2P_s1-spine2_Ethernet2
Et4                            up             up                 SERVER_s1-host1_Eth1
Et6                            up             up                 MLAG_s1-leaf2_Ethernet6
Lo0                            up             up                 ROUTER_ID
Lo1                            up             up                 VXLAN_TUNNEL_SOURCE
Lo100                          up             up                 DIAG_VRF_Tenant_A_OP_Zone
Ma0                            up             up                 OOB_MANAGEMENT
Po1                            up             up                 MLAG_s1-leaf2_Port-Channel1
Po4                            down           lowerlayerdown     PortChannel
Vl110                          up             up                 Tenant_A_OP_Zone_1
Vl1199                         up             up                 
Vl3009                         up             up                 MLAG_L3_VRF_Tenant_A_OP_Zone
Vl4093                         up             up                 MLAG_L3
Vl4094                         up             up                 MLAG
Vx1                            up             up                 s1-leaf1_VTEP
```
## show ip interface brief

```
Address
Interface       IP Address          Status     Protocol          MTU    Owner  
--------------- ------------------- ---------- ------------- ---------- -------
Ethernet2       172.30.255.1/31     up         up               1500           
Ethernet3       172.30.255.3/31     up         up               1500           
Loopback0       192.0.255.3/32      up         up              65535           
Loopback1       192.0.254.3/32      up         up              65535           
Loopback100     10.255.1.3/32       up         up              65535           
Management0     192.168.0.12/24     up         up               1500           
Vlan110         10.1.10.1/24        up         up               1500           
Vlan1199        unassigned          up         up               9164           
Vlan3009        10.255.251.0/31     up         up               1500           
Vlan4093        10.255.251.0/31     up         up               1500           
Vlan4094        10.255.252.0/31     up         up               1500
```
## show lldp neighbors

```
Last table change time   : 1:03:07 ago
Number of table inserts  : 5
Number of table deletes  : 0
Number of table drops    : 0
Number of table age-outs : 0

Port          Neighbor Device ID       Neighbor Port ID    TTL
---------- ------------------------ ---------------------- ---
Et1           s1-leaf2.atd.lab         Ethernet1           120
Et2           s1-spine1.atd.lab        Ethernet2           120
Et3           s1-spine2.atd.lab        Ethernet2           120
Et4           s1-host1.atd.lab         Ethernet1           120
Et6           s1-leaf2.atd.lab         Ethernet6           120
```
## show running-config

```
! Command: show running-config
! device: s1-leaf1 (cEOSLab, EOS-4.33.0F-39050855.4330F (engineering build))
!
no aaa root
!
username admin privilege 15 role network-admin secret 5 $1$5O85YVVn$HrXcfOivJEnISTMb6xrJc.
username arista privilege 15 role network-admin secret 5 $1$4VjIjfd1$XkUVulbNDESHFzcxDU.Tk1
username arista ssh-key ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCyut6vmeyYq3uPXAhQsye2YsSIeKoONAw9Y0/mY7cKdJkkWdq83GZAkiqGTb8W5THHYbGNqJTHfCm7oM2+qp3AcDZbNs2i1v09FNz7U2uLqeyZx1UZTtD+ww/qQr/u0+Y0HnOxb8QoXBX9YkvLA4xJ4vlVk4QsKf4ikLPHJzRVMwvI8BjlkL3RB/y6NvZKt3WqU7G2q+8Gwl+R2V35oygfFrzDVBPhvkHkBtgdHYaz7ML5Sg6E+RU6DyubfKTTt7WmoXheRRaKrrbabmSoS5Nk/07DSYYxR1ei4XEx8aciaCjJ4UrF3IearfydjeYqI3x4gXSGrHn+L+Cc0KVsZp4R arista@avd-gerry-1-f3158c0b-eos
!
management api http-commands
   no shutdown
   !
   vrf default
      no shutdown
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvcompression=gzip -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -cvaddr=192.168.0.5:9910 -cvauth=token,/tmp/token -cvvrf=default -taillogs -disableaaa
   no shutdown
!
vlan internal order ascending range 1006 1199
!
no service interface inactive port-id allocation disabled
!
transceiver qsfp default-mode 4x10G
!
service routing protocols model multi-agent
!
hostname s1-leaf1
ip name-server vrf default 8.8.8.8
ip name-server vrf default 192.168.2.1
dns domain atd.lab
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 16384
!
system l1
   unsupported speed action error
   unsupported error-correction action error
!
vlan 110
   name Tenant_A_OP_Zone_1
!
vlan 160
   name Tenant_A_VMOTION
!
vlan 3009
   name MLAG_L3_VRF_Tenant_A_OP_Zone
   trunk group MLAG
!
vlan 4093
   name MLAG_L3
   trunk group MLAG
!
vlan 4094
   name MLAG
   trunk group MLAG
!
vrf instance Tenant_A_OP_Zone
!
radius-server host 192.168.0.1 key 7 0207165218120E
!
aaa group server radius atds
   server 192.168.0.1
!
aaa authentication login default group atds local
aaa authorization exec default group atds local
aaa authorization commands all default local
!
interface Port-Channel1
   description MLAG_s1-leaf2_Port-Channel1
   switchport mode trunk
   switchport trunk group MLAG
!
interface Port-Channel4
   description PortChannel
   switchport trunk allowed vlan 110-112
   switchport mode trunk
   mlag 4
!
interface Ethernet1
   description MLAG_s1-leaf2_Ethernet1
   channel-group 1 mode active
!
interface Ethernet2
   description P2P_s1-spine1_Ethernet2
   mtu 1500
   no switchport
   ip address 172.30.255.1/31
!
interface Ethernet3
   description P2P_s1-spine2_Ethernet2
   mtu 1500
   no switchport
   ip address 172.30.255.3/31
!
interface Ethernet4
   description SERVER_s1-host1_Eth1
   channel-group 4 mode active
!
interface Ethernet6
   description MLAG_s1-leaf2_Ethernet6
   channel-group 1 mode active
!
interface Loopback0
   description ROUTER_ID
   ip address 192.0.255.3/32
!
interface Loopback1
   description VXLAN_TUNNEL_SOURCE
   ip address 192.0.254.3/32
!
interface Loopback100
   description DIAG_VRF_Tenant_A_OP_Zone
   vrf Tenant_A_OP_Zone
   ip address 10.255.1.3/32
!
interface Management0
   description OOB_MANAGEMENT
   ip address 192.168.0.12/24
!
interface Vlan110
   description Tenant_A_OP_Zone_1
   vrf Tenant_A_OP_Zone
   ip address virtual 10.1.10.1/24
!
interface Vlan3009
   description MLAG_L3_VRF_Tenant_A_OP_Zone
   mtu 1500
   vrf Tenant_A_OP_Zone
   ip address 10.255.251.0/31
!
interface Vlan4093
   description MLAG_L3
   mtu 1500
   ip address 10.255.251.0/31
!
interface Vlan4094
   description MLAG
   mtu 1500
   no autostate
   ip address 10.255.252.0/31
!
interface Vxlan1
   description s1-leaf1_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 110 vni 10110
   vxlan vlan 160 vni 55160
   vxlan vrf Tenant_A_OP_Zone vni 10
!
ip virtual-router mac-address 00:1c:73:00:dc:01
ip address virtual source-nat vrf Tenant_A_OP_Zone address 10.255.1.3
!
ip routing
ip routing vrf Tenant_A_OP_Zone
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 192.0.255.0/24 eq 32
   seq 20 permit 192.0.254.0/24 eq 32
!
ip prefix-list PL-MLAG-PEER-VRFS
   seq 10 permit 10.255.251.0/31
!
mlag configuration
   domain-id pod1
   local-interface Vlan4094
   peer-address 10.255.252.1
   peer-link Port-Channel1
   reload-delay mlag 300
   reload-delay non-mlag 330
!
ip route 0.0.0.0/0 192.168.0.1
!
ntp server 192.168.0.1 iburst source Management0
!
ip radius source-interface Management0
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-CONN-2-BGP-VRFS deny 10
   match ip address prefix-list PL-MLAG-PEER-VRFS
!
route-map RM-CONN-2-BGP-VRFS permit 20
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
!
router bfd
   multihop interval 1200 min-rx 1200 multiplier 3
!
router bgp 65101
   router-id 192.0.255.3
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS password 7 q+VNViP5i4rVjW1cxFv2wA==
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS password 7 AQQvKeimxJu+uGQ/yYvv9w==
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65101
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description s1-leaf2
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor MLAG-IPv4-UNDERLAY-PEER password 7 vnEaG8gMeQf3d3cN6PktXQ==
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor 10.255.251.1 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.255.251.1 description s1-leaf2_Vlan4093
   neighbor 172.30.255.0 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.30.255.0 remote-as 65001
   neighbor 172.30.255.0 description s1-spine1_Ethernet2
   neighbor 172.30.255.2 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.30.255.2 remote-as 65001
   neighbor 172.30.255.2 description s1-spine2_Ethernet2
   neighbor 192.0.255.1 peer group EVPN-OVERLAY-PEERS
   neighbor 192.0.255.1 remote-as 65001
   neighbor 192.0.255.1 description s1-spine1_Loopback0
   neighbor 192.0.255.2 peer group EVPN-OVERLAY-PEERS
   neighbor 192.0.255.2 remote-as 65001
   neighbor 192.0.255.2 description s1-spine2_Loopback0
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan-aware-bundle Tenant_A_OP_Zone
      rd 192.0.255.3:10
      route-target both 10:10
      redistribute learned
      vlan 110
   !
   vlan-aware-bundle Tenant_A_VMOTION
      rd 192.0.255.3:55160
      route-target both 55160:55160
      redistribute learned
      vlan 160
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf Tenant_A_OP_Zone
      rd 192.0.255.3:10
      route-target import evpn 10:10
      route-target export evpn 10:10
      router-id 192.0.255.3
      neighbor 10.255.251.1 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.255.251.1 description s1-leaf2_Vlan3009
      redistribute connected route-map RM-CONN-2-BGP-VRFS
!
router multicast
   ipv4
      routing
      software-forwarding sfe
   !
   ipv6
      software-forwarding kernel
!
end
```
## show version

```
Arista cEOSLab
Hardware version: 
Serial number: s1-leaf1
Hardware MAC address: 001c.73c0.c612
System MAC address: 001c.73c0.c612

Software image version: 4.33.0F-39050855.4330F (engineering build)
Architecture: i686
Internal build version: 4.33.0F-39050855.4330F
Internal build ID: ff38b52c-4b4f-4a3f-b591-ef310b5ac8ca
Image format version: 1.0
Image optimization: None

Kernel version: 5.14.0-503.21.1.el9_5.x86_64

Uptime: 2 hours and 4 minutes
Total memory: 49062204 kB
Free memory: 2651516 kB
```
