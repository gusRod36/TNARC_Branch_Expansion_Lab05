# How failover for tunnels would be configured (View in code mode to see command flow or to copy and paste commands)

<img src="../images/ISP_Failover_routes.JPG">

### Addresses

| Device | tunnel | link | Device | subnet | addresses | Tunnel subnet | Tunnel addresses |
|--------|--------|------|--------|--------|-----------|---------------|------------------|
| BR1-Edge-RTR | tunnel 11 | <-> | ISP2-RTR | 198.51.100.28/30 | ISP2 .29, BR1 .30 | 10.1.253.4/30 | HQ .5, BR1 .6 |
| BR2-Edge-RTR | tunnel 12 | <-> | ISP1-RTR | 198.51.100.32/30 | ISP1 .33, BR2 .34 | 172.16.253.4/30 | HQ .5, BR1 .6 |

#### ISP1 router configurations 

interface g1/3
 description To BR1 backup
 ip address 198.51.100.29 255.255.255.252
 no shutdown
router bgp 65002
 network 198.51.100.28 mask 255.255.255.252
end
write memory

#### ISP2 router configurations

interface g1/3
 description To BR2 backup
 ip address 198.51.100.33 255.255.255.252
 no shutdown
router bgp 65001
 network 198.51.100.32 mask 255.255.255.252
end
write memory


#### BR1-Edge-RTR configurations

interface g1/3
 description Backup to ISP2
 ip address 198.51.100.30 255.255.255.252
 ip access-group GRE-to-HQ-ISP2 in
 ip access-group NO-LEAKS out
 ip nat outside
 no shutdown
 exit

interface tunnel11
 description Backup to HQ via ISP2
 ip address 10.1.253.6 255.255.255.252
 tunnel source gigabitEthernet 1/3
 tunnel destination 198.51.100.5
 exit

ip route 198.51.100.5 255.255.255.255 198.51.100.29
ip route 0.0.0.0 0.0.0.0 198.51.100.29 250

crypto isakmp key CiscoBRANCH01_to_HQ_backup address 198.51.100.5
ip access-list extended GRE-to-HQ-BKP
 permit gre host 198.51.100.30 host 198.51.100.5
 exit

crypto map VPN-BKP 10 ipsec-isakmp
 set peer 198.51.100.5
 set transform TNARC-TS
 match address GRE-to-HQ-BKP
 exit

interface g1/3
 crypto map VPN-BKP
 exit

interface tunnel1
 ip ospf cost 10
 exit
interface tunnel11
 ip ospf cost 100
 exit
router ospf 10
 network 10.1.253.4 0.0.0.3 area 0

copy running-config start-config

#### BR2-Edge-RTR configurations

interface g1/3
 description Backup to ISP1
 ip address 198.51.100.34 255.255.255.252
 ip access-group GRE-to-HQ-ISP1 in
 ip access-group NO-LEAKS out
 ip nat outside
 no shutdown
 exit

interface tunnel12
 description Backup to HQ via ISP1
 ip address 172.16.253.6 255.255.255.252
 tunnel source gigabitEthernet g1/3
 tunnel destination 198.51.100.1
 exit

ip route 198.51.100.1 255.255.255.255 198.51.100.33
ip route 0.0.0.0 0.0.0.0 198.51.100.33 250

crypto isakmp CiscoBRANCH01_to_HQ_backup address 198.51.100.1
ip access-list extended GRE-to-HQ-BKP
 permit gre host 198.51.100.32 host 198.51.100.1
 exit

crypto map VPN-BKP 10 ipsec-isakmp
 set peer 198.51.100.1
 set transform TNARC-TS
 match address GRE-to-HQ-BKP
 exit

interface g1/3
 crypto map VPN-BKP
 exit

interface tunnel2
 delay 1000
 exit
interface tunnel12
 delay 10000
 exit
router eigrp 100
 network 172.16.253.4 0.0.0.3

copy running-config start-config

#### HQ-Edge-RTR

ip route 198.51.100.30 255.255.255.255 198.51.100.6
ip route 198.51.100.34 255.255.255.255 198.51.100.2

interface tunnel11
 description Backup to BR1 via ISP2
 ip address 10.1.253.5 255.255.255.252
 tunnel source gigabitEthernet 0/1
 tunnel destination 198.51.100.30
 exit

interface tunnel12
 description Backup to BR2 via ISP1
 ip address 172.16.253.5 255.255.255.252
 tunnel source gigabitEthernet 0/0
 tunnel destination 198.51.100.34
 exit

ip access-list extended GRE-to-BR1-ISP2
 permit udp host 198.51.100.30 host 198.51.100.5 eq 500
 permit udp host 198.51.100.30 host 198.51.100.5 eq 4500
 permit esp host 198.51.100.30 host 198.51.100.5
 exit
interface g0/1 
 ip access-group GRE-to-BR1-ISP2 in
 exit

ip access-list extended GRE-to-BR1-ISP1
 permit udp host 198.51.100.34 host 198.51.100.1 eq 500
 permit udp host 198.51.100.34 host 198.51.100.1 eq 4500
 permit esp host 198.51.100.34 host 198.51.100.1
 exit

interface g0/0
 ip access-group GRE-to-BR2-ISP1 in
 exit

crypto isakmp key CiscoBRANCH01_to_HQ_backup address 198.51.100.30
ip access-list extended GRE-to-BR1-BKP
 permit gre host 198.51.100.5 host 198.51.100.30
 exit

crypto map VPN-PRI2 20 ipsec-isakmp
 set peer 198.51.100.30
 set transform TNARC-TS
 match address GRE-to-BR1-BKP
 exit


crypto isakmp key CiscoBRANCH02_to_HQ_backup address 198.51.100.34
ip access-list extended GRE-to-BR2-BKP
 permit gre host 198.51.100.1 host 198.51.100.34
 exit

crypto map VPN-PRI 20 ipsec-isakmp
 set peer 198.51.100.34
 set transform TNARC-TS
 match address GRE-to-BR2-BKP
 exit

copy running-config start-config