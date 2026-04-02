# `mitmproxy`

`mitmproxy` is a useful man-in-the-middle proxy that can be used to reverse engineer TLS connections

```bash
brew install mitmproxy
```

Launch web UI with:

>NOTE: some things don't seem to work when http/2 is enabled - disable it.

```bash
mitmweb --no-http2
```

Install CA cert on macOS with:

```bash
sudo security add-trusted-cert -d -p ssl -p basic -k /Library/Keychains/System.keychain ~/.mitmproxy/mitmproxy-ca-cert.pem
```

Python apps don't seem to respect system CA's - can use this env var to override:

ref: <https://docs.mitmproxy.org/stable/concepts/certificates/#installing-the-mitmproxy-ca-certificate-manually>

```bash
export REQUESTS_CA_BUNDLE=~/.mitmproxy/mitmproxy-ca-cert.pem
export SSL_CERT_FILE=~/.mitmproxy/mitmproxy-ca-cert.pem
```

Use with CLI applications

```bash
export HTTPS_PROXY=http://localhost:8080
export HTTP_PROXY=http://localhost:8080
export https_proxy=http://localhost:8080
export http_proxy=http://localhost:8080
```
