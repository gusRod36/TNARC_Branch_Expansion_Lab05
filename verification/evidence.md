# Requirement 1: Every site reaches every other site.

#### HQ-Host pinging BR1 and BR2 hosts

<img src="../images/verifications/R1-HQPC-pings_BR1_and_BR2PCs.JPG" alt="HQ Host pinging BR1 and BR2 hosts" width="450">

#### BR1-Host pinging HQ and BR2 hosts

<img src="../images/verifications/R1-BR1FINPC_pings_HQFINPC_and_BR2PC.JPG" alt="BR1 host pinging HQ and BR2 hosts" width="650">

#### BR2-host pinging HQ and BR2 hosts

<img src="../images/verifications/R1-BR2PC_pings_HQ_and_BR1.JPG" alt="BR2 host pinging HQ and BR1 hosts" width="450">

# Requirement 2: All traffic between sites is encrypted.

#### BR1 to HQ tunnel + Encrypted packet

<img src="../images/verifications/R2-ESP-HQ-to-BR1_GRE_over_IPSEC.JPG" alt="BR1 Encrypted Packet" width="350"><img src="../images/verifications/R2-BR1-crypto-ipsec-sa.JPG" alt="" width="650">

<img src="../images/verifications/R2-BR1-crypto-isakmp.JPG" alt="" width="">

#### BR2 to HQ tunnel + Encrypted packet

<img src="../images/verifications/R2-ESP-HQ-to-BR2_GRE_over_IPSEC.JPG" alt="" width="350"><img src="../images/verifications/R2-BR2-crypto-ipsec-sa.JPG" alt="" width="650">

<img src="../images/verifications/R2-BR2-crypto-isakmp.JPG" alt="" width="">

# Requirement 3: Routes are exchanged dynamically over the tunnels.

#### HQ Routing Table

<img src="../images/verifications/R3-HQ-Edge-RTR_IP-Route_to_BR1_and_BR2_subnets.JPG" alt="HQ Routing Table" width="450">

#### BR1 Routing Table

<img src="../images/verifications/R3-BR1-Edge-RTR_Routes_to_BR2_and_HQ_Subnets.JPG" alt="BR1 Routing Table" width="450">

#### BR2 Routing Table

<img src="../images/verifications/R3-BR2-Edge-RTR_Routes_to_BR2_and_HQ_Subnets.JPG" alt="BR2 Routing Table" width="450">

# Requirement 4: OSPF and EIGRP share routes at exactly one point.

#### HQ dynamic routing OSPF and EIGRP via tunnels.

<img src="../images/verifications/R4-HQ-Edge-RTR_DynamicRouting_to_BR1_and_BR2.JPG" alt="HQ-Edge-RTR dynamic routing to BR1 and BR2" width="">

# Requirement 5: Branch user reach the internet through their own connections.

# Requirement 6: Branch traffic to HQ is inspected by the ASA, with only the needed flows allowed.

[The full list of ACLs can be found here](../ASA_ACLs.md)

# Requirement 7: Branch users get addresses from the HQ DHCP server.

#### BR1 Host DHCP request

<img src="../images/verifications/R7-BR1-PC_DHCP_Req.JPG" alt="DHCP for BR1 host" width="">

#### BR2 Host DHCP request

<img src="../images/verifications/R7-BR2-PC_DHCP_Req.JPG" alt="DHCP for BR2 host" width="">

# Requirement 8: Every new device meets Stage 2 and 3 baselines.

# Requirement 9: Tunnels survive the failure of ISP1 at HQ.