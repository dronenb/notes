# Serial Access on Linux

## Kernal Options

### Fedora CoreOS

```
    coreos-installer iso customize \
        --live-ignition "${WORKDIR}/ignition.ign" \
        --live-karg-append console=ttyS0,115200n8 \
        --live-karg-append serial \
        -o "${WORKDIR}/output_iso.iso" \
        "${WORKDIR}/fedora-coreos.iso"
```

## Console Size

```
# Show existing size
stty size

# Set rows
stty rows 116

# Set columns
stty columns 69
```
