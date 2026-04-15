# personal_VPN_with_AWS_EC2
Do you think a VPN is important when you use public Wi-Fi? Do you fully trust the network provider at a coffee shop, airport, or hotel? If not, why rely on third-party VPN services when you can build your own secure, private VPN in AWS?

This project demonstrates how to design and deploy a cloud-based VPN gateway using WireGuard on AWS EC2, giving you full control over your data, encryption, and network path. It allows your laptop or phone to securely connect to the internet through an encrypted tunnel, protecting your traffic from potential risks on public networks.

Step 1 — Create VPC Network
Summary

    Create a dedicated AWS network with internet access.
    
    Key Actions
    Create VPC (e.g., 10.0.0.0/16)
    Create Public Subnet
    Attach Internet Gateway
    Configure Route Table → 0.0.0.0/0 → IGW
    Benefit
    Full control over network isolation and routing (AWS best practice)

Step 2 — Launch EC2 Instance
Summary

    Deploy Ubuntu server to host VPN.
    
    Configuration
    Instance type: t3.micro
    OS: Ubuntu 22.04
    Security Group:
    SSH (22) → Your IP
    UDP 51820 → Anywhere

Step 3 : Assign a static public IP.

Step 4 : EC2 Set up

  Connect to EC2 (SSH)
  ssh -i your-key.pem ubuntu@<your-ec2-public-ip>
  sudo apt update
  sudo apt install wireguard -y

cat /proc/sys/net/ipv4/ip_forward

sudo sysctl -w net.ipv4.ip_forward=1

sudo nano /etc/sysctl.conf
# Add:
net.ipv4.ip_forward=1

sudo sysctl -p

Step 5 : Generate server keys

sudo mkdir -p /etc/wireguard
cd /etc/wireguard

umask 077
wg genkey | tee server_private.key | wg pubkey > server_public.key
  
Step 8 : Create WireGuard Server Config
sudo nano /etc/wireguard/wg0.conf

Step 9 — Start WireGuard
  sudo systemctl start wg-quick@wg0
  sudo systemctl enable wg-quick@wg0

Step 10 : Config Ubuntu firewall

  sudo ufw allow OpenSSH
  sudo ufw allow 51820/udp
  sudo ufw enable
  sudo ufw status

Step 11 — Generate Laptop Client Keys
  cd /etc/wireguard
  sudo systemctl restart wg-quick@wg0



  umask 077
  wg genkey | tee laptop_private.key | wg pubkey > laptop_public.key

Step 12 — Add Laptop Peer to Server
  sudo nano /etc/wireguard/wg0.conf


Step 13 : Create Client Config (Laptop) and download WireGuard for windows or phone
https://www.wireguard.com/install/

Step : Step 14 — Import to WireGuard App
  Install app (Windows / iOS / Android)
  Import config file or QR code
  Activate tunnel
