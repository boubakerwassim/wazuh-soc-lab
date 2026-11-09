# 02 – Installation & verification

Reference steps for the lab setup. Follow the official Wazuh documentation for the exact installer of your version: <https://documentation.wazuh.com/current/quickstart.html>

## 1. Wazuh server (Ubuntu VM)

Install the central components (Manager, Indexer, Dashboard; Filebeat is configured by the installer) with the Wazuh installation assistant in all-in-one mode (`wazuh-install.sh -a`). Keep the admin credentials printed at the end of the installation.

### Check the services

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
sudo systemctl status filebeat
```

### Useful logs on the server

```bash
# Manager log (agent connections, errors)
sudo tail -f /var/ossec/logs/ossec.log

# Alerts in JSON format
sudo tail -f /var/ossec/logs/alerts/alerts.json
```

## 2. Wazuh agent (Windows 11)

1. Download the Windows agent installer (MSI) matching the Manager version.
2. Install it and set the Manager address (IP of the Ubuntu VM).
3. Start the agent service (PowerShell as administrator):

```powershell
NET START WazuhSvc
```

## 3. Verify the agent

On the server:

```bash
sudo /var/ossec/bin/agent_control -l
```

The Windows endpoint must appear as **Active**. It is also visible in the Dashboard under **Agents**.

## Troubleshooting checklist

- Can the Windows endpoint reach the VM? (`ping <VM-IP>`, `Test-NetConnection <VM-IP> -Port 1514` in PowerShell)
- Is the Manager listening? (`sudo ss -tlnp | grep -E "1514|1515"`)
- Does `ossec.log` on the server show the agent connecting?
- Does the agent log on Windows (`C:\Program Files (x86)\ossec-agent\ossec.log`) show connection errors?
- Is the VM network in a mode the physical laptop can reach (bridged or port forwarding)?
