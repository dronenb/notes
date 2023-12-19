# Python

Tips and tricks when working with Python that I'm inclined to forget.

## pyenv

Using `pyenv` on Mac:

```bash
# Get list of available Python versions
pyenv install --list

# Install a particular version
pyenv install 3.12.1

# May get TK error, in which case may need to install python-tk for that version
brew install python-tk@3.12

# Switch to globally use that version
python global 3.12.1
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
