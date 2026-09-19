# nftables configuration

The firewall policy should be expressed with reusable service groups, host groups and network zones instead of raw nftables strings.

## Services

```yaml
nftables_services:
  web:
    protocols:
      tcp: [80, 443]
  mail:
    protocols:
      tcp: [25, 465, 587, 993]
  wireguard:
    protocols:
      udp: [51820]
```

## Hosts and groups

```yaml
nftables_hosts:
  web01: 10.20.10.10
  mail01: 10.20.10.80

nftables_host_groups:
  public_web:
    hosts: [web01]
  mail_servers:
    hosts: [mail01]
```

## Rules

```yaml
nftables_rules:
  forward:
    - name: lan_to_internet
      source: lan
      destination: wan
      source_networks: [10.20.10.0/24]
      action: accept
    - name: internet_to_web
      source: wan
      destination: public_web
      services: [web]
      action: accept
    - name: internet_to_mail
      source: wan
      destination: mail_servers
      services: [mail]
      action: accept
```

The role should resolve service groups and host groups, validate every reference and render the final nftables policy. Raw rules should remain available as an escape hatch, but not be the default configuration format.
