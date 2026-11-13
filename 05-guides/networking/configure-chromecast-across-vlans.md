---
Created: 2026-11-13
Modified:
---

# Configure Chromecast Across VLANs

Google Cast allows phones, tablets, and computers to stream or control media on compatible devices, including Chromecast, Google TV, and other Cast-enabled receivers.

By default, Google Cast devices are easiest to discover and access when the sending device and receiver are connected to the same local network. In networks segmented using virtual local area networks (VLANs), discovery traffic generally cannot cross VLAN boundaries without additional configuration.

This guide covers configuring Google Cast across VLANs using OPNsense, including multicast DNS (mDNS) discovery, firewall rules, and testing. The casting devices (phones, tablets) are on a different network from the receiving devices such as Nest speakers, Google TV, etc. Adjust according to your network configuration.

> [!NOTE]
> Some devices may not support discovery or control across VLANs without additional configuration. When cross-VLAN connectivity is unsupported or unreliable, placing the device on the same VLAN as its controller may be necessary.
> 
> For example, my Bose Smart Soundbar 900 must be on the same network as my phone for casting and Bose app access to work reliably.

## 📋 Prerequisites

- OPNsense configured as the firewall and router between the participating VLANs.
- Static IP addresses or DHCP reservations for Cast receivers.
- At least two configured VLANs.
    - One containing devices initiating casting.
    - Another containing Cast receivers.

## 🏠 My Environment

| VLAN | Role | Example devices |
|---|---|---|
| Server | Casting | Home Assistant |
| Trusted | Casting | Personal phones, tablets, laptops |
| Guest | Casting | Guest users, work laptops |
| IoT | Receiving | Chromecast, Google TV, other smart devices |

Inter-VLAN communication is blocked by default. Specific firewall rules permit authorized devices to communicate across VLANs when necessary.

## Overview

1. [Understanding Google Cast Across VLANs](#understanding-google-cast-across-vlans)
2. [Install the mDNS Repeater](#install-the-mdns-repeater)
3. [Configure the mDNS Repeater](#configure-the-mdns-repeater)
4. [OPNsense Configuration](#opnsense-configuration)
5. [Testing & Verification](#testing--verification)
6. [Related Documentation](#related-documentation)
7. [Official Documentation](#official-documentation)

## Understanding Google Cast Across VLANs

Google Cast uses several network mechanisms to discover devices, establish connections, and deliver media. When devices reside on separate VLANs, these mechanisms must be supported without unnecessarily weakening network segmentation.

### Device Discovery

Multicast DNS (mDNS) allows devices to discover services on a local network without requiring a conventional DNS server to maintain records for those services. mDNS is designed for local-link communication. Standard inter-VLAN routing does not automatically forward these discovery messages.

Google Cast commonly advertises its availability through the DNS Service Discovery (DNS-SD) service type `_googlecast._tcp.local`.

The relevant IPv4 discovery traffic uses:

| Component | Value | Purpose |
|---|---|---|
| Protocol | UDP | Transports mDNS messages |
| Multicast address | `224.0.0.251` | IPv4 mDNS multicast destination |
| Port | `5353` | mDNS service discovery |
| Service type | `_googlecast._tcp.local` | Identifies Google Cast receivers |

### Casting Connections

Discovery only identifies available receivers. Once a sender discovers a receiver, it must establish a direct network connection to the receiver's IP address. Not every casting application uses all these ports.

Common Google Cast communication ports include:

| Protocol | Port | Purpose |
|---|---|---|
| TCP | `8008` | Receiver communication and device services |
| TCP | `8009` | Cast control and session communication |
| TCP | `8443` | Additional receiver services on some devices |
| UDP | Dynamic | Local media transport and screen mirroring, depending on device and casting method |

[🔼 Back to Overview](#overview)

## Install the mDNS Repeater

An mDNS repeater receives discovery traffic on one interface and retransmits it onto other configured interfaces, allowing compatible devices to discover services across multiple local networks.

OPNsense provides the `os-mdns-repeater` plugin for repeating mDNS traffic between selected interfaces.

1. Sign in to the **OPNsense** web interface.
2. Navigate to **System → Firmware → Plugins**.
3. Locate the **os-mdns-repeater** plugin.
4. Click `+` to install.
5. Once installed, **mDNS Repeater** becomes available under **Services**. 
6. If **mDNS Repeater** does not appear under **Services**, refresh the web interface or log out and sign back in.

[🔼 Back to Overview](#overview)

## Configure the mDNS Repeater

Configure the repeater to exchange mDNS discovery traffic between VLANs. The standard OPNsense mDNS repeater forwards mDNS traffic rather than providing a Chromecast-only service filter. Other mDNS services advertised on the selected interfaces may also become discoverable. Consider this when selecting VLANs.

1. Navigate to **Services → mDNS Repeater**.
2. Configure the following settings:

    | Setting | Value |
    |---|---|
    | Enabled | ✅ |
    | Listen Interfaces | `Server VLAN`, `Trusted VLAN`, `IoT VLAN`, `Guest VLAN` |

3. Click **Apply**.
4. If mDNS Repeater is not running, click `▶` to start the service.

[🔼 Back to Overview](#overview)

## OPNsense Configuration

Firewall rules control which devices can communicate across VLANs after discovery. Successful mDNS discovery does not guarantee that casting will work. Firewall rules must also permit the communication required by the selected receiver and casting method.

For this configuration, authorized devices on the Trusted, Server, and Guest VLANs should be able to initiate Google Cast connections to specific receivers on the IoT VLAN without granting unrestricted access between networks.

This section covers:

1. [Back Up OPNsense](#back-up-opnsense)
2. [Create Firewall Aliases](#create-firewall-aliases)
3. [Firewall Rules](#firewall-rules)
4. [Allow mDNS Discovery Traffic](#allow-mdns-discovery-traffic)

### Back Up OPNsense

> [!IMPORTANT]
> Back up your OPNsense configuration before modifying firewall rules.

1. Open the **OPNsense web interface**.
2. Navigate to **System → Configuration → Backups**.
3. Click **Download configuration**.
4. Save the backup somewhere secure.

### Create Firewall Aliases

Firewall aliases are reusable collections of IP addresses, networks, and ports. They allow us to update multiple firewall or NAT rules by modifying a single alias instead of editing each rule individually.

1. Navigate to **Firewall → Aliases**.
2. Click `+` to add a new alias.
3. Create the aliases below.
4. Click **Save** after creating each alias.
5. Click **Apply**.

| Setting | Cast Receivers | Cast Ports | 
|---|---|---|
| Name | `Cast_Receivers` | `Cast_Ports` |
| Type | Host(s) | Port(s) |
| Content | `<ALL_CAST_RECEIVERS>` | `8008:8009`, `8443` |
| Description | Google Cast receivers | Google Cast ports | 

### Firewall Rules

Firewall rules allow authorized devices on the casting VLANs to communicate with Google Cast receivers on the IoT VLAN. These rules permit casting connections without allowing unrestricted access between VLANs.

Create a rule on the interface where the sending devices originate. OPNsense interface rules are normally evaluated against traffic entering the selected interface. Therefore, a connection initiated by a trusted client is usually permitted through a rule on the Trusted VLAN, not a matching rule on the IoT VLAN. Review existing firewall rules before creating new ones to avoid unnecessary duplication.

> [!IMPORTANT]
> If only specific devices on a VLAN require casting access, use their IP addresses or a host alias as the **Source** instead of allowing the entire VLAN. For example, Home Assistant is the only device on my Server VLAN that requires Google Cast access.

For the example network, this is the Trusted VLAN.

1. Navigate to **Firewall → Rules**.
2. Select the **Trusted VLAN** interface.
3. Click `+` to add a new firewall rule.
4. Add a new rule with the following settings:

    | Setting | Value |
    |---|---|
    | Enabled | ✅ | 
    | Description | Allow Google Cast |
    | Interface | Trusted VLAN |
    | Quick | ✅ | 
    | Action | Pass |
    | Direction | In |
    | TCP/IP Version | IPv4 |
    | Protocol | TCP |
    | Source | Trusted VLAN net |
    | Source Port | Any |
    | Destination | `Cast_Receivers` |
    | Destination Port | `Cast_Ports` |

5. Click **Save**.
6. Position the rule above any existing rules that would block Google Cast traffic.
7. Click **Apply**.
8. To allow casting from additional VLANs:
    - Click `⧉` to clone the existing rule.
    - Change the **Interface** to the VLAN containing the casting devices.
    - Update the **Source** to the appropriate network, host, or alias.
    - Click **Save**.
    - Position the rule above any applicable blocking rules.
    - Click **Apply**.

### Allow mDNS Discovery Traffic

> [!NOTE]
> A separate mDNS firewall rule is not always required when using the OPNsense mDNS Repeater. If device discovery works across VLANs, no additional rule is necessary.

If device discovery fails:

1. Verify that the mDNS Repeater is enabled and running.
2. Confirm that the casting and receiving VLAN interfaces are selected.
3. Review **Firewall → Log Files → Live View** for blocked UDP `5353` traffic.
4. If firewall filtering is preventing discovery, investigate the affected interface and create an appropriately scoped exception.

If discovery traffic is being blocked, add the following firewall aliases and rule using the settings below. Adjust the interface and source to match the affected VLAN.

#### Firewall Aliases

| Setting | mDNS Address | mDNS Port | 
|---|---|---|
| Name | `Multicast_mDNS` | `Ports_mDNS` |
| Type | Host(s) | Port(s) |
| Content | `224.0.0.251` | `5353` |
| Description | mDNS address | mDNS port |

#### Firewall Rule

| Setting | Value |
|---|---|
| Enabled | ✅ | 
| Description | Allow mDNS discovery |
| Interface | Trusted VLAN |
| Quick | ✅ | 
| Action | Pass |
| Direction | In |
| TCP/IP Version | IPv4 |
| Protocol | UDP |
| Source | Trusted VLAN net |
| Source Port | Any |
| Destination | `Multicast_mDNS` |
| Destination Port | `Ports_mDNS` |

[🔼 Back to Overview](#overview)

## Testing & Verification

Verify that Google Cast receivers can be discovered and controlled from each VLAN configured for casting. Repeat the steps for each of the VLANs.

1. Connect a phone or tablet to a VLAN configured for casting, such as the **Trusted VLAN** or **Guest VLAN**.
2. Open a Google Cast-compatible application.
3. Verify that the available Cast receivers appear in the device list.
4. Cast audio to a compatible speaker or soundbar.
5. Cast media to a Google TV or Chromecast device.
6. Verify that playback controls, including pause, resume, and volume, work correctly.

[🔼 Back to Overview](#overview)

## Related Documentation

## Official Documentation

- [OPNsense Multicast DNS Proxy](https://docs.opnsense.org/manual/how-tos/multicast-dns.html) — Install and configure the OPNsense mDNS repeater.
- [OPNsense Firewall Rules](https://docs.opnsense.org/manual/firewall.html) — Understand stateful firewall policies, interface rules, and rule processing.
- [Google Cast Discovery Troubleshooting](https://developers.google.com/cast/docs/discovery) — Understand Cast receiver discovery and investigate connection problems.

[🔼 Back to Guides](../README.md)