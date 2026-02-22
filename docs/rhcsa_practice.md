# [RH124](https://www.redhat.com/en/services/training/rh124-red-hat-system-administration-i) - Red Hat System Administration I (v10.0)

## 1. Introduction to Red Hat Enterprise Linux

- Fedora - Upstream project. New features, latest updates, and wild experiments
- CentOS Stream - Rolling preview of what is coming to the next minor RHEL release
  - More stable than Fedora
  - More adventurous than RHEL
- Red Hat Enterprise Linux - Polished, enterprise grade distribution for production environments where security, stability, and support are critical

This is for the purposes of innovation without sacrificing reliability. By running features through Fedora/CentOS first, ensures RHEL stability for paying customers.

All three distros use RPM packages

Red Hat contributes back - `systemd`, KVM, and `TuneD` all started at Red Hat.

Choice - GUI (Gnome) and headless are both options in RHEL.

Developer subscription - allows for 16 machines running RHEL

Red Hat releases a new major version every three years. LEAPP allows upgrading between major versions.

Phases of RHEL support:

- Full support phase - first 5 years
- Maintenance support phase- next 5 years
- Extended Life Phase - extra addon after the first 10 years

[EPEL](https://docs.fedoraproject.org/en-US/epel/) - Extrap packages for enterprise Linux - provided by Fedora, compatible with Fedora, CentOS Stream, RHEL, but not supported by Red Hat.

## 2. Accessing the Command Line

TTY = teletype(writer)

- `tty1` - GUI login (ctrl + alt + f1)
- `tty2` - Logged in GUI session (ctrl + alt + f2)
- `tty3`-`tty6` - text login

If no GUI, exlusively text terminals

In forums, if `#` is in front of the command, that indicates the command should be run as a root user. Otherwise, `$` would be used.

Useful shortcuts (probably only works in Gnome terminal).

| Shortcut        | Description                                                     |
| --------------- | --------------------------------------------------------------- |
| Ctrl+A          | Jump to the beginning of the command line.                      |
| Ctrl+E          | Jump to the end of the command line.                            |
| Ctrl+U          | Clear from the cursor to the beginning of the command line.     |
| Ctrl+K          | Clear from the cursor to the end of the command line.           |
| Ctrl+LeftArrow  | Jump to the beginning of the previous word on the command line. |
| Ctrl+RightArrow | Jump to the end of the next word on the command line.           |
| Ctrl + R        | Search the history list of commands for a pattern.              |

Apparently, `ctrl` + `L` is a common keyboard shortcut to clear a terminal. Seems to work in VSCodium and iTerm2 as well.

Apparently `ctrl` + `D` disconnects an SSH connection. This seems to work on macOS too.

Running `history` then `!<linenum>` will run that command from your history (in `bash`).

## 3. Getting Help from Local Documentation

Search for keywords within documentation with:

`man -k <keyword>`

Man pages have different sections:

| Section | Content Type                                  | Description                                      |
| ------- | --------------------------------------------- | ------------------------------------------------ |
| 1       | User commands                                 | Both executable and shell programs               |
| 2       | System calls                                  | Kernel routines that are invoked from user space |
| 3       | Library functions                             | Provided by program libraries                    |
| 4       | Special files                                 | For example, device files                        |
| 5       | File formats                                  | For many configuration files and structures      |
| 6       | Games and screensavers                        | Historical section for amusing programs          |
| 7       | Conventions, standards, and miscellaneous     | Protocols and file systems                       |
| 8       | System administration and privileged commands | Maintenance tasks                                |
| 9       | Linux kernel API                              | Internal kernel calls                            |

## 4. Registering Systems for Red Hat Support

Why register? - Access to CDN (content, security updates, bug fixes, improvements, lightspeed, etc.)

Red Hat Satellite (recommended when running more than 20 installs of RHEL) does the following:

- Mirrors content
- Proxies lightspeed
- remote mangementment
- policy enforcement

Entitlements - rights that come with your Red Hat subscription.

`rhc` - tool used to connect a server to Red Hat (RHEL 8.8+)
`subscription-manager` - the older way to manage RHEL connections. Has some more verbose info than `rhc` in some cases.
`insight-clients` for using lightspeed with RHEL. Needed if using `subscription-manager` to register.

## 5. Getting AI-assisted Help with Red Hat Enterprise Linux Lightspeed

## 6. Navigating the File-system Hierarchy

Significant Red Hat Enterprise Linux Directories

| Location | Purpose                                                                                                                                                                                                                                                                                                                   |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| /boot    | Files to start the boot process                                                                                                                                                                                                                                                                                           |
| /dev     | special device files that the system uses to access hardware                                                                                                                                                                                                                                                              |
| /etc     | System-specific configuration files                                                                                                                                                                                                                                                                                       |
| /home    | Home directory, where regular users store their data and configuration files.                                                                                                                                                                                                                                             |
| /root    | Home directory for the administrative superuser, root                                                                                                                                                                                                                                                                     |
| /run     | Runtime data for processes that started since the last boot. This data includes process ID files and lock files. The contents of this directory are re-created on reboot. This directory consolidates the /var/run and /var/lock directories from earlier versions of Red Hat Enterprise Linux                            |
| /tmp     | A world-writable space for temporary files. Files taht are not accessed, changed, or modified for 10 days are deleted from this directory automatically. The /var/tmp directory is also a temporary directory, in which files that are not accessed, changed, or modified in more than 30 days are deleted automatically. |
| /usr     | Installed software, shared libraries, including files, and read-only program data. Significant subdirectories in the `/usr` directory include the following: `/usr/bin`, for user commands, `/usr/sbin/` for system administration commands, and `/usr/local/`, for locally customized software                           |
| /var     | System-specific variable data should persist between boots. Files taht dynamically change, such as databases, cache directories, log files, printer-spooled documents, and website content, might be found under the /var directory                                                                                       |

## 7. Managing Files from the Command Line

## 10. Managing Local Users and Groups

## 11. Controlling Access to Files

Types of files

| Symbol | Meaning               |
| ------ | --------------------- |
| `-`    | Regular file          |
| `d`    | Directory             |
| `l`    | Symbolic link         |
| `c`    | Character file device |
| `b`    | Block device          |
| `p`    | Named pipe file       |
| `s`    | Local socket file     |

Ownership is evaluated in the following order:

1. Owning user
2. Owning group
3. Others

Unix permissions do not "fall-through". IE, if a file has no permissions for the owning user, and rwx permissions for others, if the user is the owning user, the owning user will not be able to do anything with the file, since evaluation stops once the user is identified as being the owning user.

To list files in a directory, you need read and execute bits on that directory. To delete files, you need write and execute (you don't need read. This is called a "blind" operation).

### 11.5 Special Permissions

In addition to `rwx`, there are three special permissions: `setuid`, `setgid`, `sticky`.

| Permission | Effect on Files | Effect on Directories | Command to Set |
| - | - | - | - |
| `setuid` | The file executes with the permissions of the owning user of the file, and not with the permissions of the user running the executable. | NA | `chmod u+s` |
| `setgid` | A file executes with the permissions of the owning group of the file inst3ead of with the permissions of the group for the user | A newly creawted file inherits the owning group from the owning group of the directory, and not the primary group of the creator | `chmod g+s` |
| `sticky` | NA | Only the owning user of a file can delete a file from the directory. | `chmod o+t` |

with `setuid` and `setgid`, an `s` will show up in the execute position in `ls` output. If it is lowercase, it means it has `setuid` or `setgid` AND executable. If uppercase, it does not have executable permissions.

By default, directories have `777` and files have `666`. The tools that create these files can be changed with `umask`, which has its defaults set in `/etc/login.defs`

## 12. Installing and Updating Software with RPM

`rpm` command lets you transact against the `rpm` database (keeps a record of installed RPM's) and against `.rpm` files directly.

`.rpm` files follow the following naming convention `name-version-release.architecture.rpm`
