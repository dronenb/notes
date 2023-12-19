# SSH

Various tricks with SSH

## SSH Config

Typically lives in ~/.ssh/config.

### Base Config

Some of the options listed below are for macOS only.

```text
Host *
  User dronenb
  UseKeychain yes # I think for macOS only
  AddKeysToAgent yes
  ServerAliveInterval 30
  LogLevel ERROR
```

### Conditional Host

Sometimes it may be useful to have a `ProxyCommand` or `ProxyJump` directive if you meet a certain condition (IE, on or off a particular network). For example, the following could be used to access github.com through an internet proxy:

```text
Match Originalhost github.com Exec "condition"
  ProxyCommand nc -X connect -x <proxyhost>:<proxyport> %h %p
```

`condition` can be a `bash` pipeline.

### SSH Agent + Environment Variable forwarding

Probably not a good idea to enable for all hosts, but can be useful, particularly for bastion hosts.

```text
Host myhost
  ForwardAgent yes
  SendEnv GH_TOKEN
  SendEnv GH_HOST
```

For environment variables, the server must have the appropriate `AcceptEnv GH_TOKEN`, etc. on the other end in the `/etc/ssh/sshd_config` file.


## SSH Port Forward

### Forward Remote Port to Local:

```bash
ssh -L <LOCAL IP>:<LOCAL PORT>:<REMOTE IP>:<REMOTE PORT> <SSH GATEWAY IP/HOSTNAME>
```

^This requires `GatewayPorts yes` in `/etc/ssh/sshd_config` on the remote host.

## FIDO2 support for macOS

For using a Yubikey or similar with SSH on macOS Sonoma or higher.

```bash
brew install michaelroosz/ssh/libsk-libfido2-install
```

- <https://github.com/MichaelRoosz/homebrew-ssh/blob/main/Casks/libsk-libfido2-install.rb>
- <https://github.com/Yubico/libfido2/issues/464#issuecomment-1781855050>

## Hushlogin

Disable useless output when logging in via SSH:

```bash
touch $HOME/.hughlogin
```
