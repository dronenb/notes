# Homebrew

Homebrew is a package manager for macOS

## Bumping a Formula or Cask Version

This will automatically detect if a formula or cask is out of date and submit a PR if it is out of date.

```bash
brew bump --open-pr <package>
```

## LiveCheck

```bash
# A tap
brew lc --tap dronenb/tap
```

## Forgetting an installed cask

Sometimes there might be a case where you want to leave an application installed but not
have it be managed by `brew`. It is possible to do this via the following:

```bash
cd "${HOMEBREW_PREFIX}/Caskroom"
rm -rf <cask_name>
```

Ref:

- https://stackoverflow.com/a/61164725/5050637
