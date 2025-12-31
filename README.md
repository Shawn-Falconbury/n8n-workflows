# n8n Workflows

A collection of n8n automation workflows for home lab and network management.

## Workflows

### 1. UniFi Daily Health Report
**File:** `workflows/unifi-daily-health-report.json`

Automated daily network health report sent via Telegram at 6 AM. Monitors your UniFi network infrastructure and sends a formatted summary including:

- **Network Infrastructure Status** - Device counts and online/offline status for gateways, access points, and switches
- **Connected Clients** - Total count with wireless vs wired breakdown
- **CyberSecure Alerts** - Security events from the last 24 hours (IPS/IDS threats, intrusions, blocked attacks)

#### Sample Output
```
🌐 UniFi Network Health Report
📅 Wednesday, January 1, 2025
🕐 06:00 AM

📡 Network Infrastructure
✅ Devices: 14/14 online
   └ Gateways: 1
   └ Access Points: 3
   └ Switches: 10

👥 Connected Clients
✅ Total: 50
   └ 📶 Wireless: 21
   └ 🔌 Wired: 29

🛡️ CyberSecure Alerts (Last 24h)
✅ No threats detected

🟢 All Systems Operational
```

#### Prerequisites
- UniFi OS controller (UDM, UDM Pro, Cloud Key Gen2+)
- UniFi Integration API key
- Telegram bot

#### Setup Instructions

1. **Import the workflow** into n8n via *Workflows → Import from File*

2. **Create UniFi API Credential:**
   - Go to *Settings → Credentials → Add Credential*
   - Select **Header Auth**
   - Name: `UniFi API Key`
   - Header Name: `X-API-KEY`
   - Header Value: `YOUR_UNIFI_API_KEY`

3. **Create Telegram Credential:**
   - Add Credential → **Telegram API**
   - Access Token: Your bot token from [@BotFather](https://t.me/botfather)

4. **Configure the workflow:**
   - Replace `YOUR_UNIFI_IP` with your controller IP in all HTTP Request nodes
   - Replace `YOUR_TELEGRAM_CHAT_ID` in the Telegram node
   
   To get your Chat ID, message your bot and visit:
   ```
   https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates
   ```

5. **Activate** the workflow

#### API Endpoints Used
- `GET /proxy/network/integration/v1/sites` - List sites
- `GET /proxy/network/integration/v1/sites/{siteId}/clients` - Get connected clients
- `GET /proxy/network/integration/v1/sites/{siteId}/devices` - Get network devices
- `GET /proxy/network/integration/v1/sites/{siteId}/events` - Get security events

## License

MIT

## Contributing

Feel free to submit issues and pull requests for new workflows or improvements.
