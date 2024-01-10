# gpg

# Setup `pinentry-touchid`

```bash
brew tap jorgelbg/tap
brew install pinentry-mac jorgelbg/tap/pinentry-touchid
mkdir -p "$HOME/.gnupg"
chmod 600 "$HOME/.gnupg"
echo "pinentry-program ${HOMEBREW_PREFIX}/bin/pinentry-touchid" > ~/.gnupg/gpg-agent.conf
pinentry-touchid -check
```

## Create a key

```bash
gpg --full-generate-key
```

## Export public key

```bash
gpg --list-secret-keys --keyid-format=long
gpg --armor --export <key_id>
```

## GitHub

### Add to GitHub with `gh` CLI

```bash
# Auth with scope to add GPG key
gh auth refresh -s write:gpg_key

# Add key
gpg --armor --export <key_id> | gh gpg-key add -
```

### Configure Commit Signing

```bash
git config --global user.signingkey <key_id>
git config --global commit.gpgsign true
```

## Refs

- <https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key>
