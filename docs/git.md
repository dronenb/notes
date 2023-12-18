# Git

If there are upstream commits and you are ahead:

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

Enable rebasing by default:

```
git config --global pull.rebase true
```
