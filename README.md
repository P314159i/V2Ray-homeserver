# V2Ray-homeserver setup with ZeroTier

## Requirements
- A Linux Mint laptop (server)

---

## Server Setup (Linux Mint laptop)

### 1. Install V2Ray
```bash
sudo apt install curl unzip
curl -O https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh
sudo bash install-release.sh
```

### 2. Generate a UUID
```bash
uuidgen
```
Save the output — this is your client ID. 
Generate as many as the number of your clients.

### 3. Edit the V2Ray config file
```bash
sudo nano /usr/local/etc/v2ray/config.json
```
Paste the following (replace `YOUR-xx-UUID-HERE` with your UUID codes):
```json
{
  "inbounds": [{
    "port": 10086,
    "protocol": "vmess",
    "settings": {
      "clients": [
        { "id": "YOUR-1st-UUID-HERE", "alterId": 0 }
        { "id": "YOUR-2nd-UUID-HERE", "alterId": 0 }
      ]
    }
  }],
  "outbounds": [{
    "protocol": "freedom",
    "settings": {}
  }]
}
```
Save with `Ctrl+X`, then `Y`, then `Enter`.

### 4. Enable and Start V2Ray
```bash
sudo systemctl enable v2ray
sudo systemctl start v2ray
```
(to be continued...)
