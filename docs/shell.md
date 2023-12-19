# Shell Scripting

Useful one liners that I always end up Googling or obscure things that took me awhile to come up with.

## Disable Bash deprecation warning in macOS

```bash
export BASH_SILENCE_DEPRECATION_WARNING=1
```

## Lowercase/Uppercase Conversion

Lower to upper:

```bash
whoami | tr '[:lower:]' '[:upper:]'
```

Upper to lower:

```bash
whoami | tr '[:upper:]' '[:lower:]'
```
