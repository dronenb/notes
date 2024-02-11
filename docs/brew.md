# Homebrew

Homebrew is a package manager for macOS

## Forgetting an installed cask

Sometimes there might be a case where you want to leave an application installed but not
have it be managed by `brew`. It is possible to do this via the following:

```bash
cd "${HOMEBREW_PREFIX}/Caskroom"
rm -rf <cask_name>
```

Ref:

- https://stackoverflow.com/a/61164725/5050637
