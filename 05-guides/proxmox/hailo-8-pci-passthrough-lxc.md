---
Created: 2026-10-09
Modified:
---

# Hailo-8 PCI Passthrough to LXC

Use Hailo's official HailoRT installation documentation and the prebuilt `.deb` package provided through the Hailo Developer Zone. This guide uses HailoRT PCIe driver version `4.24.0` with the Hailo-8.

1. Refresh the Proxmox package index.
   - `apt update` tells APT to refresh its package index by checking configured repositories for available package versions. It does not install any updates.

```bash
apt update
```

2. Download the Hailo PCIe driver package from the [Hailo Developer Zone](https://hailo.ai/developer-zone/) to the local computer, then copy it to the Proxmox host using **PowerShell**. A Hailo Developer Zone account is required to access the download, and because the download requires authentication, download the `.deb` package through a web browser first.

```powershell
scp "$HOME\Downloads\hailort-pcie-driver_4.24.0_all.deb" root@<PROXMOX_IP>:/tmp/
```

3. Install the Hailo PCIe driver package on the Proxmox host.
   - `dpkg --install` is Debian's low-level package manager for installing, removing, and inspecting `.deb` packages. `--install` tells `dpkg` to install the specified package.

```bash
dpkg --install /tmp/hailort-pcie-driver_4.24.0_all.deb
```

4. Reboot the Proxmox host if the driver installer instructs you to do so. A newly installed kernel driver may require a reboot before Linux loads it and binds it to the PCI device.

```bash
reboot
```

## Related Documentation

- [Proxmox - PCI Passthrough to LXC](../pci-passthrough-lxc.md)

## Official Documentation

- [Hailo AI](https://hailo.ai/)
- [Hailo Developer Zone](https://hailo.ai/developer-zone/)
- [HailoRT 4.24.0 Documentation](https://hailo.ai/developer-zone/documentation/hailort-v4-24-0/)