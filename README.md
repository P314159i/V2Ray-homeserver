# V2Ray-homeserver setup with ZeroTier

## Requirements
- A Linux Mint laptop (server)
- A ZeroTier account (sign up at [zerotier.com](https://zerotier.com) with temp mail)

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

### 5. Install ZeroTier
 (or Tailscale, but Tailscale doesn't allow anonymous sign up and ZeroTier is as opSec safe as Tailscale afaik)
```bash
curl -s https://install.zerotier.com | sudo bash
```

### 6. Join your ZeroTier network 
 Go to my.zerotier.com, log in, and your network ID is shown right there on the main page under Networks.
 It's the 16-character code like a1b2c3d4e5f6g7h8
 Replace 'YOUR_NETWORK_ID' with your own ZeroTier code
```bash
sudo zerotier-cli join YOUR_NETWORK_ID
```
Then go to your ZeroTier dashboard and authorize your laptop under **Members**.

### 7. Get your ZeroTier IP
```bash
sudo zerotier-cli listnetworks
```
Write down the IP for yourself (looks like `10.x.x.x` or `172.x.x.x`).

---

## Client Setup (family devices)

### 1. Install ZeroTier
- Download from [zerotier.com/download] or from AppStore or PlayStore
- Join the same network ID
- Get authorized by the server owner in the dashboard

### 2. Install a V2Ray client app from AppStore or PlayStore

### 3. Add a new server in the app
| Field | Value |
|---|---|
| Address | Your ZeroTier IP |
| Port | `10086` |
| ID (UUID) | Your generated UUID |
| AlterID | `0` |
| Security | `auto` |
| Network | `tcp` |

---

## Managing the Server

| Action | Command |
|---|---|
| Start V2Ray | `sudo systemctl start v2ray` |
| Stop V2Ray | `sudo systemctl stop v2ray` |
| Check status | `sudo systemctl status v2ray` |
| Disable autostart | `sudo systemctl disable v2ray` |


Voila!
