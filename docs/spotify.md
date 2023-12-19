# Spotify

Notes related to configuring Spotify client.

## 

## Spotify w/ Proxy

Spotiy does not resolve DNS properly when using an HTTP proxy. Can use the following command to add DNS entry for Spotify to `/etc/hosts` for Spotify and configure the Spotify client settings.

```bash
#!/bin/bash

# Run as root
[[ "$UID" -eq 0 ]] || exec sudo "$0" "$@"

spotify_configpath="$HOME/Library/Application Support/Spotify/prefs"
dns_name=spotify.com

spotify_proxyoff_config='network.proxy.mode=1'
spotify_proxyon_config='network.proxy.mode=2'

function spotify_proxon {
    ip_addr=$(curl -skH "accept: application/dns-json" "https://1.1.1.1/dns-query?name=${dns_name}&type=A" | jq -r '.Answer[0].data')
    cp /etc/hosts ~/hosts.beforeproxy
    for name in "www.${dns_name}" "${dns_name}"; do
        if ! grep -qE "\s${name}" /etc/hosts; then
            echo "${ip_addr} ${name}" >> /etc/hosts
        fi
    done
    spotify_proxyline='network.proxy.addr="YOUR_PROXY_SERVER_HERE"'
    if [[ -f "${spotify_configpath}" ]]; then
        if ! grep -qF "${spotify_proxyline}" "${spotify_configpath}"; then
            echo "${spotify_proxyline}" >> "${spotify_configpath}"
        fi
        if ! grep -qF 'network.proxy.mode=' "${spotify_configpath}"; then
            echo "${spotify_proxyon_config}" >> "${spotify_configpath}"
        else
            sed -i '' -E 's/network\.proxy\.mode=[0-9]/'"${spotify_proxyon_config}"'/' "${spotify_configpath}"
        fi
    fi
}

function spotify_proxoff {
    cp /etc/hosts ~/hosts.afterproxy
    sed -i '' '/'"${dns_name}"'/d' /etc/hosts
    if [[ -f "${spotify_configpath}" ]]; then
        if grep -qF 'network.proxy.mode=' "${spotify_configpath}"; then
            sed -i '' -E 's/network\.proxy\.mode=[0-9]/'"${spotify_proxyoff_config}"'/' "${spotify_configpath}"
        fi
    fi
}

function restart_spotify {
    spotify_pid=$(pgrep Spotify | sort | head -n 1)
    if [[ -n "${spotify_pid}" ]]; then
        kill -9 "${spotify_pid}"
        sleep 1
    fi

    open /Applications/Spotify.app
}
```
