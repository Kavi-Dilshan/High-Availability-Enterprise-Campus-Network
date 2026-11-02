# High-Availability Enterprise Campus Network (Cisco Packet Tracer)

A fault-tolerant, 3-Tier Enterprise Campus Network architecture designed and configured in Cisco Packet Tracer. This project demonstrates core enterprise routing, switching, redundancy, WAN connectivity, and network security principles.

![Network Topology](topology.png)

## Key Technologies Implemented
* **High Availability & Redundancy:** HSRP (Hot Standby Router Protocol) for default gateway failover and LACP (IEEE 802.3ad) Layer 2 EtherChannel.
* **Switching & Loop Prevention:** VLAN Segmentation, 802.1Q Trunking, Rapid-PVST+ (Root Primary & Secondary alignment), PortFast, and BPDU Guard.
* **Routing & WAN:** Multi-path OSPF (Area 0) dynamic routing, Default Route redistribution, and NAT/PAT (Overload) for external internet access.
* **Network Security:** Extended Access Control Lists (ACLs) for inter-department isolation, Switchport Port-Security (Sticky MAC), and Secure Remote Management via SSHv2 with VTY ACL restrictions.

## VLAN & IP Addressing Scheme

| VLAN ID | Department | Subnet | HSRP Virtual Gateway | DSW1 (Active) | DSW2 (Standby) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **VLAN 10** | HR & Admin | 192.168.10.0/24 | 192.168.10.1 | 192.168.10.2 | 192.168.10.3 |
| **VLAN 20** | Finance | 192.168.20.0/24 | 192.168.20.1 | 192.168.20.2 | 192.168.20.3 |
| **VLAN 30** | IT Department | 192.168.30.0/24 | 192.168.30.1 | 192.168.30.2 | 192.168.30.3 |
| **VLAN 40** | Server Farm | 192.168.40.0/24 | 192.168.40.1 | 192.168.40.2 | 192.168.40.3 |
| **VLAN 99** | Management | 192.168.99.0/24 | 192.168.99.1 | 192.168.99.2 | 192.168.99.3 |

## Security Policies Enforced
1. **Department Isolation (Extended ACL):** HR Department (`VLAN 10`) is blocked from accessing the Finance Subnet (`VLAN 20`).
2. **Restricted Server Access:** HR users are only permitted HTTP (`Port 80`) access to the Internal Web Server (`192.168.40.10`); all other traffic to the Server Farm is denied.
3. **Secure Management:** SSHv2 (`admin` / `Admin@123`) is restricted strictly to the IT Department subnet (`192.168.30.0/24`) via VTY access-class filtering.
4. **Access Layer Protection:** Sticky MAC Port Security (maximum 2 MAC addresses per port, restrict violation mode) enabled on all staff access ports.

## How to Test This Topology
1. Clone or download this repository and open `Enterprise_Campus_Network.pkt` in **Cisco Packet Tracer**.
2. **HSRP Failover Test:** Run `ping -t 8.8.8.8` from any PC, then shut down interface `Fa0/24` or `Vlan10` on `DSW1-Primary` to observe seamless failover to `DSW2-Backup`.
3. **ACL Verification:** Ping `192.168.20.10` from `HR-PC1` (Blocked) and browse `http://192.168.40.10` from the `HR-PC1` Web Browser (Permitted).
4. **SSH Access:** Open Command Prompt on `IT-PC` and run `ssh -l admin 192.168.99.2` (Password: `Admin@123`).
