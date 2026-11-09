# 01 – Architecture

## Personal lab (since 08/2026)

| Machine | Platform | Role |
|---|---|---|
| MacBook Air M1 (16 GB RAM) | macOS + UTM / QEMU | Virtualization host |
| Ubuntu VM | Ubuntu Linux in UTM | Wazuh server: Manager, Indexer, Dashboard, Filebeat |
| Dell laptop | Windows 11 (physical) | Monitored endpoint with Wazuh agent |

Wazuh version: **4.14.9**

## Master's project (2024)

- Ubuntu virtual machine hosting the Wazuh components (Manager, Indexer, Dashboard, Filebeat)
- Windows 11 client monitored by a Wazuh agent

## Data flow

```
Windows 11 endpoint ──(agent)──► Wazuh Manager ──► Filebeat ──► Wazuh Indexer ──► Wazuh Dashboard
```

## Network ports used by Wazuh

| Port | Protocol | Purpose |
|---|---|---|
| 1514 | TCP | Agent → Manager event traffic |
| 1515 | TCP | Agent enrollment |
| 443 | TCP | Wazuh Dashboard (HTTPS) |
| 9200 | TCP | Wazuh Indexer API |
| 55000 | TCP | Wazuh server API |

## Lab-specific challenge: VM ↔ physical endpoint

The Wazuh server runs inside a UTM virtual machine while the endpoint is a separate physical laptop. The agent can only reach the Manager if the VM is reachable from the local network (bridged networking or port forwarding for 1514/1515). Checking this connectivity was a key part of getting the agent online.
