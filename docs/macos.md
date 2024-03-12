# macOS

## Removing Duplicates from "Open With" Menu

```bash
/System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -kill -r -domain local -domain system -domain user
killall Finder 
```
