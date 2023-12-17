# Python

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

## Certificates Issue

Run script from [here](https://github.com/python/cpython/blob/main/Mac/BuildScript/resources/install_certificates.command). Have run into issues with OpenSSL and `brew` before, if so just run:

```bash
brew reinstall openssl@3
```

## Links

- <https://github.com/python/cpython/blob/main/Mac/BuildScript/resources/install_certificates.command>
