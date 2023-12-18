# Networking Troubeshooting tricks

Get external IP address using curl

```bash
curl http://ipecho.net/plain
```

Rolling packet capture

```bash
tcpdump -vvv -i any not port 22 -w /tmp/rolling_dump0.pcap -W 4 -C 600 -n
```

Test connection with `nc`:

```bash
nc -w 1 -vz  <HOST> <PORT>
```

Repeated `nc` connection test:

```bash
while :; do nc -w 1 -vz  <HOST> <PORT>; done
```
