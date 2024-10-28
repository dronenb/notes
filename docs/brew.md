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

## Brew Style

```bash
# Check style of everything in a tap:
brew style dronenb/tap
```

Ref:

- https://stackoverflow.com/a/61164725/5050637


## `brew update` or `brew upgrade` problems

For issues similar to:

```text
<internal:/opt/homebrew/Library/Homebrew/vendor/portable-ruby/3.3.5/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:37:in `require': cannot load such file -- sorbet-runtime (LoadError)
	from <internal:/opt/homebrew/Library/Homebrew/vendor/portable-ruby/3.3.5/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:37:in `require'
	from /opt/homebrew/Library/Homebrew/standalone/sorbet.rb:4:in `<top (required)>'
	from /opt/homebrew/Library/Homebrew/startup.rb:10:in `require_relative'
	from /opt/homebrew/Library/Homebrew/startup.rb:10:in `<top (required)>'
	from /opt/homebrew/Library/Homebrew/global.rb:4:in `require_relative'
	from /opt/homebrew/Library/Homebrew/global.rb:4:in `<top (required)>'
	from /opt/homebrew/Library/Homebrew/brew.rb:18:in `require_relative'
	from /opt/homebrew/Library/Homebrew/brew.rb:18:in `<main>'
```

```bash
brew update-reset
```
