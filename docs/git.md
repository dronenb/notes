# Git

Tips and tricks for working with `git`.

## Rebase when there are upstream changes

```bash
git remote add upstream <upstream-repository-URL>
git fetch upstream
git checkout main # Or master
git rebase upstream/main # or master

# There may be conflicts. Can resolve with:
git add/rm <conflicted_files>
git rebase --continue

# Then force update the branch:
git push -f
```

## Enable rebasing by default

```bash
git config --global pull.rebase true
```

## Amend Commit

With same commit message:

```bash
git commit --amend -m "$(git log -1 --pretty=%B)"
```

## Enabling Commit Signing w/ SSH

Prep work on Windows:

```pwsh
choco upgrade git --params “‘/NoOpenSSH’”
Get-Service ssh-agent | Set-Service -StartupType Automatic -PassThru | Start-Service
New-Item "$env:USERPROFILE\.ssh" -ItemType Directory -ea 0
ssh-keygen -t ecdsa -b 256 -C "$env:USERNAME" -f "$env:USERPROFILE\.ssh\id_ecdsa"
if (-not (Test-Path $PROFILE)) {$null = New-Item -Force $PROFILE}
"ssh-add -l | Select-String `$(ssh-keygen -lf `$env:USERPROFILE\.ssh\id_ecdsa) || ssh-add  `$env:USERPROFILE\.ssh\id_ecdsa" >> $PROFILE
```

Actual process

```bash
ssh-keygen -t ecdsa -b 256 -C "${USER}" -f ~/.ssh/id_ecdsa
git config --global gpg.format ssh
git config --global commit.gpgsign true
git config --global user.signingkey ~/.ssh/id_ecdsa.pub
gh auth login --git-protocol https --scopes "admin:ssh_signing_key"
gh ssh-key add --title "${USER}" --type signing ~/.ssh/id_ecdsa.pub
```
