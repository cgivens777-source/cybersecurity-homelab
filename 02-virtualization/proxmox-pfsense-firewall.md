# Proxmox Deployment and pfSense Firewall

## Objective

Transition the cybersecurity homelab virtualization environment to Proxmox VE and deploy pfSense as a virtual firewall. The objective is to establish a platform for virtual network segmentation, firewall configuration, and controlled security testing.

## Environment

- **Hypervisor:** Proxmox VE 9.2
- **Physical host:** AMD Ryzen 7 3700X, 64 GB RAM
- **Host management IP:** `192.168.4.50`
- **Virtual firewall:** pfSense Community Edition
- **Guest operating system:** Ubuntu Server
- **Virtual networking:** Proxmox Linux bridges

## Architecture

The physical machine runs Proxmox VE, which manages the virtual machines and virtual network interfaces.

Two virtual bridges support the lab network design:

- `vmbr0` — connected to the existing home LAN.
- `vmbr1` — intended for the isolated lab network.

The pfSense VM has two virtual network interfaces, allowing it to serve as the firewall between its WAN and LAN interfaces.

## pfSense Configuration

| Component | Configuration |
|---|---|
| WAN interface | `vtnet0` |
| WAN address | `192.168.4.78/22` |
| LAN interface | `vtnet1` |
| LAN address | `10.10.10.1/24` |
| LAN DHCP range | `10.10.10.100–10.10.10.200` |
| Virtual disk | 16 GiB |

The WAN interface connects to the existing home network. The LAN interface is configured for the private lab subnet.

## Ubuntu Server VM

The Ubuntu Server test VM was created in Proxmox and assigned the reported address `192.168.4.82/22` on its initial network interface.

The lab is being configured to connect guest systems to the pfSense-controlled LAN. The Ubuntu VM's LAN-side interface and end-to-end routing will be documented after verification.

## Security Objectives

- Deploy a virtual firewall with separate WAN and LAN interfaces.
- Establish a dedicated private lab subnet.
- Configure DHCP for lab clients.
- Separate lab network design from the existing home LAN.
- Develop and verify firewall rules.
- Observe network traffic and validate access controls.

## Verification and Evidence

Evidence will include:

- Proxmox node and VM configuration.
- Proxmox bridge configuration.
- pfSense WAN and LAN interface settings.
- DHCP configuration.
- Firewall rules and logs.
- Verified client addressing and routing.

Only verified results will be reported as completed.

## Lessons Learned

This project moves the lab toward centralized virtualization and firewall-based network control. Proxmox provides VM and virtual-network management, while pfSense provides a platform for implementing and testing network security policies.

The project will continue as the isolated lab network, firewall policies, and client connectivity are configured and validated.
