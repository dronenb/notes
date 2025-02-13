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

```
launchctl setenv ALLOW_BROWSER_INTEGRATION_OVERRIDE true
```

<https://stackoverflow.com/a/3756686>

## Cleaning Macbook Tips

- How to take off arrow keys - <https://www.youtube.com/watch?v=xc__-jWXInU>
- Disabling keyboard input for cleaning - <https://github.com/leafac/configuration--hammerspoon/blob/main/init.lua#L5-L29>
