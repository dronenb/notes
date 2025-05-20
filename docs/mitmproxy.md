# `mitmproxy`

`mitmproxy` is a useful man-in-the-middle proxy that can be used to reverse engineer TLS connections

```bash
brew install mitmproxy
```

Launch web UI with:

```bash
mitmweb
```

Install CA cert on macOS with:

```bash
sudo security add-trusted-cert -d -p ssl -p basic -k /Library/Keychains/System.keychain ~/.mitmproxy/mitmproxy-ca-cert.pem
```

ref: <https://docs.mitmproxy.org/stable/concepts/certificates/#installing-the-mitmproxy-ca-certificate-manually>

Use with CLI applications
```bash
export HTTPS_PROXY=http://localhost:8080
export HTTP_PROXY=http://localhost:8080
export https_proxy=http://localhost:8080
export http_proxy=http://localhost:8080
```
