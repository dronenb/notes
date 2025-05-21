# macOS

## Removing Duplicates from "Open With" Menu

```bash
/System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -kill -r -domain local -domain system -domain user
killall Finder 
```

## Flush DNS Cache

```bash
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
```

## Unquarantine a file

```bash
xattr -d com.apple.quarantine <file>
```

## Rotate MAC address

```bash
#!/usr/bin/env bash
set -o errexit  # Exit on any failure. Same as set -e
set -o nounset  # Exit on undeclared variables. Same as set -u
set -o pipefail # Return value of all commands in a pipe
# set -o xtrace  # command tracing, same as set -x
set -o noglob # Disable globs. Messes up the * in curl's noproxy option

cat <<EOF > "${TMPDIR}/disconnectwifi.swift"
import Foundation
import CoreWLAN

if let wifiInterface = CWWiFiClient.shared().interface() {
  wifiInterface.disassociate()
  print("ok")
} else {
  print("error")
}
EOF
swift "${TMPDIR}/disconnectwifi.swift"
sleep 3
random_mac=$(openssl rand -hex 6 | sed 's/\(..\)/\1:/g; s/.$//')
sudo ifconfig en0 ether "${random_mac}"
```

- <https://github.com/dronenb/notes/blob/main/docs/macos.md#rotate-mac-address>

## TFTP Server

Enable with:

```bash
sudo launchctl load -F /System/Library/LaunchDaemons/tftp.plist
```

Disable with:

```bash
sudo launchctl unload /System/Library/LaunchDaemons/tftp.plist
```

TFTP directory is in `/private/tftpboot`, will need to pre-create files w/ 777 permissions to copy files to TFTP server, will need to make sure files in that folder are 777 to copy from.

## Environment Variables

All GUI apps can read environment variables set by launchctl

```bash
launchctl setenv ALLOW_BROWSER_INTEGRATION_OVERRIDE true
```

<https://stackoverflow.com/a/3756686>

## Cleaning Macbook Tips

- How to take off arrow keys - <https://www.youtube.com/watch?v=xc__-jWXInU>
- Disabling keyboard input for cleaning - <https://github.com/leafac/configuration--hammerspoon/blob/main/init.lua#L5-L29>
