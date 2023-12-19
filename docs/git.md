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

## Enable rebasing by default:

```
git config --global pull.rebase true
```
