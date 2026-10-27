# VirtualBox Cybersecurity Lab Setup

## Overview

This project involved building a virtual lab for cybersecurity practice using VirtualBox and Kali Linux. The goal was to configure a dedicated testing machine with a static IP address, internet connectivity, host integration, and a recovery point for future exercises.

## Lab Environment

| Setting | Value |
|---|---|
| Virtualization platform | Oracle VirtualBox |
| Virtual machine | Kali Linux |
| Network type | NAT Network |
| Network name | `NatNetwork` |
| Network subnet | `10.0.0.0/24` |
| Kali IPv4 address | `10.0.0.2/24` |
| Default gateway | `10.0.0.1` |
| DNS servers | `8.8.8.8`, `1.1.1.1` |

### Virtual Network Configuration

I created a custom NAT Network in VirtualBox and connected Kali Linux to it. DHCP was disabled, and the VM was configured with a static IPv4 address.

![VirtualBox NAT Network configuration](screenshots/01-nat-network-configuration.png)

![Kali network adapter configuration](screenshots/02-kali-network-adapter.png)

### Kali Network Configuration

The network settings were applied through NetworkManager. I verified the interface address and default route using:

```bash
ip -br addr
ip route
```

The `eth0` interface showed `10.0.0.2/24`, with the default gateway set to `10.0.0.1`.

![Kali IP address and routing table](screenshots/03-kali-static-ip-and-routing.png)

## Connectivity Verification

I tested the connection to the virtual gateway, checked internet reachability by IP address, and tested DNS resolution.

| Check | Command | Result |
|---|---|---|
| Gateway connectivity | `ping -c 4 10.0.0.1` | 4 replies received; 0% packet loss |
| Internet connectivity | `ping -c 4 8.8.8.8` | Successful |
| DNS resolution | `ping -c 4 google.com` | Successful after DNS configuration |

![Gateway connectivity test](screenshots/04-kali-gateway-connectivity.png)

![Internet and DNS verification](screenshots/05-kali-internet-and-dns.png)

### DNS Troubleshooting

Initially, Kali could reach the gateway, but domain names could not be resolved. I updated the DNS servers in the NetworkManager profile and reactivated the connection.

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dns "8.8.8.8 1.1.1.1"
sudo nmcli connection up "Wired connection 1"
```

After the change, the domain-name test succeeded. This helped me distinguish a DNS resolution problem from basic IP connectivity.

## Virtual Machine Integration

I enabled bidirectional clipboard sharing and drag-and-drop in the Kali VM's VirtualBox settings.

![Clipboard and drag-and-drop settings](screenshots/06-kali-clipboard-drag-drop.png)

I also configured the host's Downloads folder as a shared folder. I verified that it was accessible inside Kali at `/media/sf_downloads`.

![Shared folder configuration or mounted directory](screenshots/07-kali-shared-folder.png)

## Recovery Snapshot

After completing the configuration, I created a VirtualBox snapshot named `Kali-Lab-Setup-Complete`. This preserves a baseline that can be used to return to the configured state if a future lab exercise changes the VM.

![Kali baseline snapshot](screenshots/08-kali-snapshot.png)

## Key Takeaways

- Configured a custom VirtualBox NAT Network and static IPv4 addressing.
- Used gateway, IP-based, and DNS tests to troubleshoot connectivity.
- Integrated a host folder with Kali Linux.
- Enabled clipboard and drag-and-drop functionality.
- Created a VM snapshot for recovery before future practical exercises.

## Responsible Use

This environment is intended for cybersecurity education and authorized testing. Any future assessment of other systems will be performed only with appropriate permission.

## Tools and References

- [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Kali Linux](https://www.kali.org/get-kali/)

## Project Details

- **Program:** Networkwalks Cybersecurity
- **Task:** WK1-PM1
- **Project:** VirtualBox and Kali Linux Lab Setup
- **Author:** Chinemerem Ndubuisi
