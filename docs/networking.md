# Networking Troubeshooting tricks

Tips, common flags, etc. so I don't have to Google it every time.

## Get external IP address using curl

```bash
curl http://ipecho.net/plain
```

## Rolling Packet Capture (`tcpdump`)

```bash
tcpdump -vvv -i any not port 22 -w /tmp/rolling_dump0.pcap -W 4 -C 600 -n
```

## Test outbound connection with `nc`:

```bash
nc -w 1 -vz  <HOST> <PORT>
```

Repeated `nc` connection test:

```bash
while :; do nc -w 1 -vz  <HOST> <PORT>; done
```

Send file with `nc`:

```bash
# Send
nc -w 3 <IP> 8000 < out.file

# Recieve (Mac syntax)
nc -lk 8000 > uploaded_file.txt
```
