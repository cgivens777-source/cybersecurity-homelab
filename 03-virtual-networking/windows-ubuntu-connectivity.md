# Windows–Ubuntu Network Connectivity Lab

## Objective

Establish and troubleshoot network connectivity between Windows and Ubuntu systems within the homelab.

The objective was to verify basic IP connectivity, identify configuration issues, and use command-line networking tools to troubleshoot communication between hosts.

## Environment

* Windows 11 Pro
* Ubuntu Linux
* Home LAN
* Ethernet/Wi-Fi network connectivity
* IPv4

## 1. Initial Connectivity Test

Connectivity between the Ubuntu and Windows systems was tested using ICMP echo requests.

From Ubuntu:

```bash
ping <Windows-IP-address>
```

From Windows:

```cmd
ping <Ubuntu-IP-address>
```

The initial testing identified a connectivity issue that required troubleshooting before successful communication could be established. Windows was able to ping Ubuntu without issue but vice versa was not functioning.

## 2. Troubleshooting

The troubleshooting process involved verifying:

* IP addresses
* Network interfaces
* Network connectivity
* Firewall configuration
* Host availability
* ICMP traffic

Ubuntu network configuration was examined using:

```bash
ip addr
```

Connectivity was tested with:

```bash
ping <Windows-IP-address>
```

Windows network configuration was verified using:

```cmd
ipconfig
```

Connectivity was then tested using:

```cmd
ping <Ubuntu-IP-address>
```

To verify the Ubuntu machine was functioning properly we pinged google.com

```bash
ping <google.com>
```
With a successful response it was determined that a windows configuration was the issue. A quick google search lead me to believe that the Windows Firewall may be the culprit. I verified the status of the firewall using:

```cmd
netsh advfirewall show allprofiles
```

This returned an "ON" state. I then configured a rule to allow ICMP Echo requests:

```cmd
netsh advfirewall firewall add rule name="Lab ICMPv4 Echo" protocol=icmpv4:8,any dir=in action=allow
```

## 3. Resolution

After troubleshooting the network configuration and connectivity issue, ICMP communication between the Windows and Ubuntu systems was successfully established.

Successful ping responses confirmed Layer 3 connectivity between the hosts.

## 4. Security Considerations

ICMP connectivity demonstrates network reachability but does not by itself establish that application-layer services are accessible.

Firewall rules and listening services must be evaluated separately when determining whether a host is reachable for a particular service.

This distinction is important when troubleshooting network security:

```text
Network Reachability
        ↓
Firewall Rules
        ↓
Listening Service
        ↓
Application Connectivity
```

## Skills Demonstrated

* IPv4 networking
* Linux network configuration
* Windows network configuration
* ICMP troubleshooting
* Host-to-host connectivity testing
* Firewall troubleshooting
* Network-layer troubleshooting
* Cross-platform administration

## Lessons Learned

Successful network troubleshooting requires isolating the problem by layer rather than assuming that a failed connection is caused by a single component.

Testing IP configuration, basic reachability, firewall behavior, and application services separately makes it easier to identify where communication is failing.

## Next Steps

* Capture the traffic with Wireshark
* Identify ICMP packets
* Examine packet headers
* Analyze TCP/UDP traffic
* Identify network services and ports
