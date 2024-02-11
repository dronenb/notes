# `pre-commit`

`pre-commit` is used to manage the lifecycle of `git` pre-commit hooks for you.

## Usage

Install on macOS with:

```bash
brew install pre-commit
```

Once a repository with a `.pre-commit-config.yaml` is cloned, use the following to install:

```bash
pre-commit install
```

## Updating

Update `pre-commit` hooks and pin them to their release SHA using:

```bash
pre-commit autoupdate --freeze
```

## Homebrew issue with `python-setuptools`

Issue: https://github.com/pre-commit/pre-commit/issues/2122

Resolution:

```bash
brew install python-setuptools
```
