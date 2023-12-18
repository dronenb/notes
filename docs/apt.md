# Apt

## Apt with Proxy

```bash
echo 'Acquire::http::Proxy "http://proxy.example.com:83";' >> /etc/apt/apt.conf.d/proxy.conf
echo 'Acquire::https::Proxy "http://proxy.example.com:83";' >> /etc/apt/apt.conf.d/proxy.conf
apt update
```
