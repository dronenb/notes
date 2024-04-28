# macOS

## Removing Duplicates from "Open With" Menu

```bash
/System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -kill -r -domain local -domain system -domain user
killall Finder 
```

## Cleaning Macbook Tips

- How to take off arrow keys - <https://www.youtube.com/watch?v=xc__-jWXInU>
- Disabling keyboard input for cleaning - <https://github.com/leafac/configuration--hammerspoon/blob/main/init.lua#L5-L29>
