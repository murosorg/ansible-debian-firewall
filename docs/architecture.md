# Architecture

The deployment uses two Debian firewalls in an active-passive HA pair. Keepalived provides the virtual IP, conntrackd synchronizes connection state and WireGuard provides encrypted administration and service connectivity.

```mermaid
flowchart LR
  Internet((Internet)) --> FW1[fw1 Debian 13 or 14]
  Internet --> FW2[fw2 Debian 13 or 14]
  FW1 <-->|VRRP and conntrackd| FW2
  FW1 <-->|WireGuard| Admin[Administration network]
  FW2 <-->|WireGuard| Admin
  FW1 --> LAN[LAN and service networks]
  FW2 --> LAN
  FW1 --> Routing[FRRouting OSPF or BGP]
  FW2 --> Routing
```

The Ansible roles manage the network layer, nftables policy, VPN services, routing, high availability and connection tracking. The virtual IP is owned by the active node and moves to the standby node during failover.
