# Wireguard

## Use a wg client as a gateway for LAN

On the client:

```bash
wg_net="10.9.6.0/255.255.255.0"
export DEBIAN_FRONTEND=noninteractive
apt-get install -y iptables-persistent

# If not using systemd-sysctl
sysctl -w net.ipv6.conf.all.forwarding=1
sysctl -w net.ipv4.ip_forward=1
sudo sysctl -p

# If using systemd-sysctl
echo "net.ipv4.ip_forward=1" >> /etc/sysctl.d/99-sysctl.conf
echo "net.ipv6.conf.all.forwarding=1" >> /etc/sysctl.d/99-sysctl.conf
systemctl restart systemd-sysctl

iptables -A INPUT -i wg0 -j ACCEPT
iptables -A FORWARD -i wg0 -j ACCEPT
iptables -A FORWARD -o wg0 -j ACCEPT
iptables -A OUTPUT -o wg0 -j ACCEPT
iptables -A FORWARD -i wg0 -o eth0 -s "${wg_net}" -j ACCEPT
iptables -t nat -A POSTROUTING -o eth0 -s "${wg_net}" -j MASQUERADE

iptables-save > /etc/iptables/rules.v4
```

On the server, add the following to the `wg0.conf`, example CIDR: `10.91.1.0/24`:

```text
# For client peer:
AllowedIPs = ...,10.91.1.0/24
```

On other `wg` clients, add the following to the server peer:

```text
AllowedIPs = ...,10.91.1.0/24
```
