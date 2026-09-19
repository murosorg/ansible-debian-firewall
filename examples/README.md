# Examples

These examples show the recommended variable structure. Copy the relevant files into an inventory environment and adapt addresses, interfaces and secrets before deployment.

```text
examples/
├── group_vars/grp1/nftables.yml
├── host_vars/fw1/keepalived.yml
├── host_vars/fw2/keepalived.yml
└── inventory.yml
```

Never commit private keys or production passwords. Store sensitive values with Ansible Vault.
