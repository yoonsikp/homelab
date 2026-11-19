# Install usbmuxd and iphone udev rules
sudo apt install usbmuxd ipheth-utils

# Install Python tooling
curl -LsSf https://astral.sh/uv/install.sh | sh

# Install pymobiledevice3
uv tool install --managed-python pymobiledevice3

# On iPhone:
# Settings > Personal Hotspot
# Enable Allow Others to Join
# Connect USB-C cable
# Unlock iPhone
# Tap Trust This Computer if prompted

# Check USB device detection
lsusb

# Check iPhone through pymobiledevice3
pymobiledevice3 usbmux list
pymobiledevice3 lockdown info

# Add to /etc/network/interfaces 
```
allow-hotplug enx82b364582913
iface enx82b364582913 inet6 auto
    pre-up sysctl -w net.ipv6.conf.enx82b364582913.ra_defrtr_metric=2048
```

ra_defrtr_metric --> deprioritize this ipv6 route compared to others (1024)



