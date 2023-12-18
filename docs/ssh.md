# SSH

Various tricks with SSH

## FIDO2 support for macOS

```bash
brew install michaelroosz/ssh/libsk-libfido2-install
```

### Links

- <https://github.com/MichaelRoosz/homebrew-ssh/blob/main/Casks/libsk-libfido2-install.rb>
- <https://github.com/Yubico/libfido2/issues/464#issuecomment-1781855050>


## SSH Port Forward

Forward Remote Port to Local:

```bash
ssh -L <LOCAL IP>:<LOCAL PORT>:<REMOTE IP>:<REMOTE PORT> <SSH GATEWAY IP/HOSTNAME>
```

^This requires `GatewayPorts yes` in `/etc/ssh/sshd_config` on the remote host.
