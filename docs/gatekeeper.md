# Gatekeeper

Tricks for working with Apple's Gatekeeper

## Disable gatekeeper quarantine for item

```bash
xattr -d com.apple.quarantine <path_to_binary>
```

## Disable gatekeeper for Homebrew Casks

```bash
export HOMEBREW_CASK_OPTS=--no-quarantine
```
