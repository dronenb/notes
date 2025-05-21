# Shell Scripting + `bash` + `zsh` Tricks

Useful one liners that I always end up Googling or obscure things that took me awhile to find or come up with.

## Disable Bash deprecation warning in macOS

```bash
export BASH_SILENCE_DEPRECATION_WARNING=1
```

## Lowercase/Uppercase Conversion

### Lower to upper

```bash
whoami | tr '[:lower:]' '[:upper:]'
```

### Upper to lower

```bash
whoami | tr '[:upper:]' '[:lower:]'
```

## Generate Secure Password

Uses [pwgen](https://linux.die.net/man/1/pwgen) to generate. The following example generates one 64 character password

```bash
pwgen --symbols --secure 64 1
```

> Note: does not come on macOS. Install with `brew install pwgen`.
