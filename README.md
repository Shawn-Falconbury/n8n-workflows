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

---

### 2. Daily Weather Email
**File:** `workflows/daily-weather-email.json`

Automated daily weather report sent via Gmail at 6 AM. Fetches current conditions and 5-day forecast from OpenWeatherMap and delivers a beautifully formatted HTML email.

#### Features
- **Current Conditions** - Temperature, feels like, humidity, wind speed
- **Sunrise/Sunset Times** - Localized to your timezone
- **5-Day Forecast** - Daily high/low temps with weather icons
- **Weather Icons** - Dynamic icons from OpenWeatherMap
- **Clickable Link** - Direct link to full forecast on OpenWeatherMap

#### Sample Email Preview
The email includes:
- Gradient header with location and date
- Large current temperature with weather icon
- Current conditions cards (feels like, humidity, wind)
- Sunrise and sunset times
- 5-day forecast cards with high/low temps
- "View Full Forecast" button linking to OpenWeatherMap

#### Prerequisites
- [OpenWeatherMap API key](https://openweathermap.org/api) (free tier works)
- Gmail account with OAuth2 configured in n8n

#### Setup Instructions

1. **Import the workflow** into n8n via *Workflows → Import from File*

2. **Create OpenWeatherMap Credential:**
   - Go to *Settings → Credentials → Add Credential*
   - Select **OpenWeatherMap API**
   - API Key: Your OpenWeatherMap API key

3. **Create Gmail OAuth2 Credential:**
   - Add Credential → **Gmail OAuth2**
   - Follow the OAuth2 setup flow with your Google account

4. **Configure the workflow:**
   - Update `YOUR_ZIP_CODE` in both OpenWeatherMap nodes
   - Update `YOUR_EMAIL@example.com` in the Gmail node
   - Adjust the `timeZone` in the Code node to match your location (default: `America/New_York`)
   
   Common US timezones:
   - `America/New_York` (Eastern)
   - `America/Chicago` (Central)
   - `America/Denver` (Mountain)
   - `America/Los_Angeles` (Pacific)

5. **Activate** the workflow

#### Customization Options
- **Schedule**: Modify the cron expression in the Schedule Trigger (default: `0 6 * * *` for 6 AM daily)
- **Units**: Change `imperial` to `metric` in OpenWeatherMap nodes for Celsius
- **Styling**: Edit the HTML template in the Code node to customize colors and layout

---

## License

MIT

## Contributing

Feel free to submit issues and pull requests for new workflows or improvements.
