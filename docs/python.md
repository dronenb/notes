# Python

Tips and tricks when working with Python that I'm inclined to forget.

## pyenv

Using `pyenv` on Mac:

```bash
# Install pyenv
brew install pyenv pyenv-virtualenv
# Get version currently used by pyenv
pyenv version-name

# Get list of versions available to pyenv, not including system
pyenv versions --bare | sort -rt "." -k1,1n -k2,2n -k3,3n

# Get list of available Python versions
pyenv install --list

# One-liner to get most recent version
PYTHON_VERSION=$( \
  pyenv install --list | \
    awk '{print $1}' | \
    grep ^3 | \
    grep -v "dev" | \
    grep -v a | \
    sort -t "." -k1,1n -k2,2n -k3,3n | \
    tac | \
    head -n 1 \
)

# Install a particular version
pyenv install "${PYTHON_VERSION}"

# May get TK error, in which case may need to install python-tk for that version
brew install "python-tk@${PYTHON_VERSION}"

# Switch to globally use that version
pyenv global "${PYTHON_VERSION}"
```

## macOS Cert Issue

On macOS, might be necessary to run [this script](https://github.com/python/cpython/blob/main/Mac/BuildScript/resources/install_certificates.command). Have run into issues with OpenSSL and `brew` before, if so just run:

```bash
brew reinstall openssl@3
```

Some answers also suggest installing or upgrading `certifi`

```bash
pip3 install --upgrade certifi
```

- <https://github.com/python/cpython/blob/main/Mac/BuildScript/resources/install_certificates.command>
