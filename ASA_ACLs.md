## HQ-ASA-1

| Line | ACL | P/D | Protocol | Source | Port | Destination | Port |
|------|-----|-----|----------|--------|------|-------------|------|
| 1 | OUTSIDE-IN | permit | tcp | host 10.0.250.1 |  | host 10.0.200.20 | eq 49 |
| 2 | OUTSIDE-IN | permit | udp | host 10.0.250.1 |  | host 10.0.200.35 | eq 123 |
| 3 | OUTSIDE-IN | permit | udp | host 10.0.250.1 |  | host 10.0.200.35 | eq 514 |
| 4 | OUTSIDE-IN | permit | udp | host 10.0.250.1 |  | host 10.0.200.35 | eq 69 |
| 5 | OUTSIDE-IN | permit | udp | any |  | host 10.0.210.10 | eq 53 |
| 6 | OUTSIDE-IN | permit | udp | any |  | host 10.0.210.11 | eq 80 |
| 7 | OUTSIDE-IN | permit | tcp | any |  | host 10.0.210.11 | eq 443 |
| 8 | OUTSIDE-IN | permit | tcp | object BR1-MGMT |  | object HQ-NET | eq 22 |
| 9 | OUTSIDE-IN | permit | tcp | object BR2-MGMT |  | object HQ-NET | eq 22 |
| 10 | OUTSIDE-IN | permit | icmp | object BR1 |  | object HQ-NET | echo |
| 11 | OUTSIDE-IN | permit | icmp | object BR2 |  | object HQ-NET | echo |
| 12 | OUTSIDE-IN | permit | udp | 10.1.0.0/16 |  | host 10.0.200.10 | eq 67 |
| 13 | OUTSIDE-IN | permit | udp | 172.16.0.0/16 |  | host 10.0.200.10 | eq 67 |
| 14 | OUTSIDE-IN | permit | udp | 10.1.0.0/16 |  | host 10.0.200.35 | eq 53 |
| 15 | OUTSIDE-IN | permit | udp | 172.16.0.0/16 |  | host 10.0.200.35 | eq 53 |
| 16 | OUTSIDE-IN | permit | udp | 10.1.0.0/16 |  | host 10.0.200.35 | eq 514 |
| 17 | OUTSIDE-IN | permit | udp | 172.16.0.0/16 |  | host 10.0.200.35 | eq 514 |
| 18 | OUTSIDE-IN | permit | udp | 10.1.0.0/16 |  | host 10.0.200.35 | eq 123 |
| 19 | OUTSIDE-IN | permit | udp | 172.16.0.0/16 |  | host 10.0.200.35 | eq 123 |
| 20 | OUTSIDE-IN | permit | udp | 10.1.0.0/16 |  | host 10.0.200.35 | eq 69 |
| 21 | OUTSIDE-IN | permit | udp | 172.16.0.0/16 |  | host 10.0.200.35 | eq 69 |
| 22 | OUTSIDE-IN | permit | udp | 10.1.253.0/30 |  | host 10.0.200.35 | eq 514 |
| 23 | OUTSIDE-IN | permit | udp | 10.1.253.0/30 |  | host 10.0.200.35 | eq 123 |
| 24 | OUTSIDE-IN | permit | udp | 10.1.253.0/30 |  | host 10.0.200.35 | eq 69 |
| 25 | OUTSIDE-IN | permit | tcp | 10.1.253.0/30 |  | host 10.0.200.20 | eq 49 |
| 26 | OUTSIDE-IN | permit | udp | 172.16.253.0/30 |  | host 10.0.200.35 | eq 514 |
| 27 | OUTSIDE-IN | permit | udp | 172.16.253.0/30 |  | host 10.0.200.35 | eq 123 |
| 28 | OUTSIDE-IN | permit | udp | 172.16.253.0/30 |  | host 10.0.200.35 | eq 69 |
| 29 | OUTSIDE-IN | permit | tcp | 172.16.253.0/30 |  | host 10.0.200.20 | eq 49 |
| 30 | OUTSIDE-IN | permit | tcp | 10.1.0.0/16 |  | host 10.0.200.20 | eq 49 |
| 31 | OUTSIDE-IN | permit | tcp | 172.16.0.0/16 |  | host 10.0.200.20 | eq 49 |
| 32 | OUTSIDE-IN | deny | ip | any |  | any | U
| 1 | DMZ-IN | permit | tcp | any |  | host 10.0.210.11 | eq 80 |
| 2 | DMZ-IN | permit | tcp | any |  | host 10.0.210.11 | eq 443 |
| 3 | DMZ-IN | permit | udp | any |  | host 10.0.210.10 | eq 53 | 
| 4 | DMZ-IN | deny | ip | any |  | any |

## BR1-ASA-1

| Line | ACL | P/D | Protocol | Source | Port | Destination | Port |
|------|-----|-----|----------|--------|------|-------------|------|
| 1 | OUTSIDE-IN | permit | icmp | object HQ-NET |  | object BR1 | echo |
| 2 | OUTSIDE-IN | permit | icmp | object BR2 |  | object BR1 | echo |
| 3 | OUTSIDE-IN | permit | tcp | object HQ-MGMT1 |  | object BR1-MGMT | eq 22 |
| 4 | OUTSIDE-IN | permit | tcp | object HQ-MGMT2 |  | object BR1-MGMT | eq 22 |
| 5 | OUTSIDE-IN | permit | tcp | object BR2-MGMT |  | object BR1-MGMT | eq 22 |
| 6 | OUTSIDE-IN | permit | udp | object DHCP-SRV | eq 67 | object BR1 | eq 67 |
| 7 | OUTSIDE-IN | deny | ip | any |  | any |

## BR2-ASA-1
| Line | ACL | P/D | Protocol | Source | Port | Destination | Port |
|------|-----|-----|----------|--------|------|-------------|------|
| 1 | OUTSIDE-IN | permit | icmp | object HQ-NET |  | object BR2 | echo |
| 2 | OUTSIDE-IN | permit | icmp | object BR1 |  | object BR2 | echo |
| 3 | OUTSIDE-IN | permit | tcp | object HQ-MGMT1 |  | object BR2-MGMT | eq 22 |
| 4 | OUTSIDE-IN | permit | tcp | object HQ-MGMT2 |  | object BR2-MGMT | eq 22 |
| 5 | OUTSIDE-IN | permit | tcp | object BR1-MGMT |  | object BR2-MGMT | eq 22 |
| 6 | OUTSIDE-IN | permit | icmp | host 172.16.250.1 |  | host 172.16.250.2 | echo-reply |
| 7 | OUTSIDE-IN | permit | udp | object DHCP-SRV | eq 67 | object BR2 | eq 67 |
| 8 | OUTSIDE-IN | deny | ip | any |  | any |