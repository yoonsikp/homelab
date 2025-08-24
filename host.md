# host
LAN Network IP: 172.16.0.2/24
VM Network IP: 172.16.1.1/24

## Hardware
- Intel E3-1245v6
- 32GB DDR4 ECC
- 128GB Crucial SATA SSD (MLC)
- 2 x 6TB WD Red HDD (CMR)
- 2 x 512GB Samsung NVMe SSD (MLC)

## ZFS Storage
- /sand/
  - ./scratch/: Scratch VM + Files
  - ./vms/defs: VM Definitions
  - ./vms/media: VM Disk Drives
- /rust/backups
  - ./iphone: passed through to ibackup VM
  - ./macbook: chown backup1 server
  - ./turtle: turtle backup
  - ./bear: bear backup

## Software
- Alpine Linux
- ZFS, ZFS-zed
- apcupsd
- libvirtd
- libvirt-guests
- ssmtp

## Alpine Linux
https://wiki.alpinelinux.org/wiki/Installation

```
# Useful Tools
apk add doas doas-sudo-shim sudo
apk add nano pv 
apk add logrotate
apk add intel-ucode amd-ucode
```

```
# cat /etc/modules
tun
vfio
vfio_pci
```

```
# cat /etc/sudoers
...
## Uncomment to allow members of group wheel to execute any com>
# %wheel ALL=(ALL:ALL) ALL

## Same thing without a password
# %wheel ALL=(ALL:ALL) NOPASSWD: ALL

yoonsik ALL=(ALL:ALL) NOPASSWD: ALL

```

## Setup Timezone
```
sudo setup-timezone
```

## Configure Networking
https://wiki.alpinelinux.org/wiki/Configure_Networking

```
# cat /etc/networking/interfaces

allow-hotplug eth1
auto eth1
iface eth1 inet manual
    # disable pause frames
    post-up ethtool -A eth1 autoneg off tx off rx off
    # offloading causes crashes on i219
    post-up ethtool -K eth1 tx off rx off gso off gro off tso off

allow-hotplug eth0
auto eth0
iface eth0 inet manual
    # disable pause frames
    post-up ethtool -A eth0 autoneg off tx off rx off
    # disable tx rx checksum
    post-up ethtool -K eth0 tx off rx off
#    post-up ethtool -K eth0 gro off lro off
#    post-up ethtool --set-eee eth0 eee off

allow-hotplug br0
auto br0
iface br0 inet static
    bridge_ports eth0 eth1
    bridge_stp off
    bridge_fd 0
    bridge_maxwait 0
    address 172.16.0.2
    netmask 255.255.255.0
    gateway 172.16.0.1

allow-hotplug br1
auto br1
iface br1 inet manual
    bridge_ports none
    bridge_stp off
    bridge_fd 0
    bridge_maxwait 0
```

```
# cat /etc/resolv.conf
search .lan
nameserver 172.16.0.1
```

## ZFS Notes
https://wiki.alpinelinux.org/wiki/ZFS

See zfs.md

```
apk add zfs zfs-lts
modprobe zfs
zpool import ...
```

Automatically import pools
```
rc-update add zfs-import default
```

See https://github.com/yoonsikp/os-tips-tricks

SSH restriction
```
command="borg serve --append-only --restrict-to-repository /rust/backups/macbook",restrict ssh-rsa AAAA...9fQ== yoonsik@macair.lan.naut.ca
```

## KVM + Disable GPU Bringup
https://wiki.archlinux.org/title/PCI_passthrough_via_OVMF#Setting_up_IOMMU
https://wiki.alpinelinux.org/wiki/KVM
```
apk add edk2 ovmf
apk add virtiofs
apk add pciutils
apk add ethtool
```

Enable auto shutdown of guests
```
rc-update add libvirt-guests
```

```
# cat /etc/mkinitfs/features.d/vfio.modules; sudo mkinitfs
kernel/drivers/vfio/vfio.ko.*
kernel/drivers/vfio/vfio_virqfd.ko.*
kernel/drivers/vfio/vfio_iommu_type1.ko.*
kernel/drivers/vfio/pci/vfio-pci.ko.*
```

```
# cat /etc/default/grub; sudo update-grub
GRUB_CMDLINE_LINUX_DEFAULT="fsck.mode=force fsck.repair=yes rootflags=data=journal intel_iommu=on iommu=pt preempt=full pcie_acs_override=downstream,multifunction initcall_blacklist=sysfb_init video=simplefb:off video=vesafb:off video=efifb:off video=vesa:off disable_vga=1 modprobe.blacklist=radeon,nouveau,nvidia,nvidiafb,nvidia-gpu,snd_hda_intel,snd_hda_codec_hdmi,i915"
```

```
# cat /etc/modprobe.d/vfio.conf
options vfio
options vfio_iommu_type1
options vfio_virqfd
options vfio_pci ids=8086:591d disable_vga=1
```

```
# cat /etc/modprobe.d/i915.conf 
blacklist i915
```

Note: libvirt will run an instance of dnsmasq which will appear to be listening on all IP addresses - however it is bound strictly to the virtual network devices.

## UPS Notes

```
sudo apk add apcupsd
sudo rc-update add apcupsd
```
Most USB UPSs will support this configuration
```
# cat apcupsd.conf

UPSCABLE smart
UPSTYPE usb
# DEVICE 

ONBATTERYDELAY 6
BATTERYLEVEL 25
MINUTES 5
TIMEOUT 0
NETSERVER off

```
Run custom scripts if desired

```
# cat apccontrol

   onbattery)
	echo "Power failure on UPS ${2}. Running on batteries." | ${WALL}
	ssh -o BatchMode=yes -o ConnectTimeout=10 -i /home/yoonsik/.ssh/id_rsa administrator@winnie shutdown /s
    ;;
    offbattery)
	echo "Power has returned on UPS ${2}..." | ${WALL}
	ssh -o BatchMode=yes -o ConnectTimeout=10 -i /home/yoonsik/.ssh/id_rsa administrator@winnie shutdown /a
    ;;
```

### Security Hardening
Lock root account:
```
passwd -l root
```

SSH Disable Password Authentication
```
# cat /etc/ssh/sshd_config

PasswordAuthentication no
```

### Scheduled Tasks
- Backup from SSD to HDD
- Backup from HDD to Offsite
- Upgrade packages?

### Update to latest release
https://wiki.alpinelinux.org/wiki/Include:Upgrading_to_latest_release#Using_latest-stable_instead_of_version_number
