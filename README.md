SoftEther VPN Cascade Connection Setup for Bypassing GFW
This repository provides instructions for installing and configuring SoftEther VPN on two servers to create a cascade connection, allowing for more robust bypassing of the Great Firewall (GFW) and restricted network environments.

Overview
First Server (Outside Iran): Acts as the primary SoftEther VPN server.
Second Server (Inside Iran): Configured with SoftEther and connected to the first server in cascade mode.
Prerequisites
Set up two servers, one outside and one inside Iran, both running Linux. Follow the steps below to configure each server.

1. Setup Instructions for Both Servers

1. System Update
```bash

apt update && apt upgrade -y
```
2. Install Prerequisites
```bash

apt-get -y install build-essential wget make curl gcc wget zlib1g-dev tzdata git libreadline-dev libncurses-dev libssl-dev
```
Installation
1. Download SoftEther
Replace (download link) with the official SoftEther download link.

```bash

wget (download link)
```
2. Extract the Downloaded File
Replace (download file name) with the actual name of the downloaded file.
```bash

ls   # Verify file name
tar xzf (download file name)
```

3. Compile SoftEther
```bash

cd vpnserver
make
cd ..
```
4. Move to /usr/local
```bash

mv vpnserver /usr/local
cd /usr/local/vpnserver/
```
5. Set Permissions
```bash

chmod 600 *
chmod 700 vpnserver vpncmd
```
6. Start the VPN Server
```bash

./vpnserver start
```
Configuration
1. Access the Management Panel
```bash

./vpncmd
```
2. Set the Server Password
```bash

ServerPasswordSet
```
3. Configure Systemd Service
```bash

sudo cat >> /lib/systemd/system/vpnserver.service << EOF
[Unit]
Description=SoftEther VPN Server
After=network.target
[Service]
Type=forking
ExecStart=/usr/local/vpnserver/vpnserver start
ExecStop=/usr/local/vpnserver/vpnserver stop
[Install]
WantedBy=multi-user.target
EOF
```
4. Enable IP Forwarding
```bash

echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf
sysctl -p

systemctl enable vpnserver
systemctl start vpnserver
systemctl status vpnserver
```
Firewall Settings
Allow necessary ports for VPN connections:

```bash
ufw allow 443
ufw allow 500,4500/udp
ufw allow 1701
ufw allow 1194
ufw allow 5555
```

After that on first server make cascade connection to iran server and its done!! enjoy

