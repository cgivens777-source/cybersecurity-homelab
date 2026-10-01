# Linux System Hardening Lab

## Objective

Apply and document security-hardening techniques on a clean Ubuntu Linux installation.

The goal was to reduce unnecessary attack surface, understand Linux privilege boundaries, and verify the security configuration through command-line investigation.

## Environment

* Ubuntu Linux
* AMD Ryzen 7 3700X
* 64 GB RAM
* Dedicated NVMe storage
* UEFI boot environment
* Home laboratory network

## 1. User and Privilege Management

Linux users and privilege boundaries were examined to understand which accounts could perform administrative operations.

The following command was used to determine the current user's available sudo privileges:

```bash
sudo -l
```

This demonstrated the importance of verifying administrative permissions rather than assuming that membership in a particular group automatically provides unrestricted access.

## 2. SUID Binary Enumeration

SUID-enabled executables were enumerated to identify programs that can execute with the permissions of their file owner.

Command used:

```bash
find / -perm -4000 -type f 2>/dev/null
```

Example results included:

```text
/usr/bin/mount
/usr/bin/umount
/usr/bin/passwd
/usr/bin/su
/usr/bin/sudo
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/pkexec
```

### Security Consideration

SUID programs are important from a security perspective because they can create privilege-escalation opportunities when improperly configured or vulnerable.

Enumeration provides visibility into the system's privileged executable surface.

## 3. Privilege Boundary Testing

Administrative access was tested from both privileged and unprivileged user contexts.

The purpose was to verify that users could not automatically perform administrative operations without the appropriate authorization.

This provided practical experience with:

* Root privileges
* Sudo
* User accounts
* Group-based permissions
* Privilege boundaries
* Least privilege

## 4. File and Directory Permissions

Linux file permissions were examined to understand how read, write, and execute permissions are applied to:

* Owner
* Group
* Other users

Commands used during administration included:

```bash
ls -l
chmod
chown
```

These commands were used to inspect and modify permissions and ownership where appropriate.

## 5. System and Service Investigation

The system was examined from the command line to identify running processes, services, and network activity.

Commands included:

```bash
ps
top
ss
```

The purpose was to understand what software was running and what network services were exposed.

## 6. Host-Based Firewall Configuration

Ubuntu's Uncomplicated Firewall (UFW) was configured to establish a default-deny inbound security posture while permitting outbound connections.

### Default Firewall Policy

Incoming traffic was denied by default:

```bash
sudo ufw default deny incoming
```

Outgoing traffic was allowed by default:

```bash
sudo ufw default allow outgoing
```

Forwarded/routed traffic was also denied by default:

```bash
sudo ufw default deny routed
```

The firewall was then enabled:

```bash
sudo ufw enable
```

### SSH Access Control

Because SSH is required for remote administration, an explicit exception was created for the local network:

```bash
sudo ufw allow from 192.168.4.0/22 to any port 22 proto tcp
```

This produced the rule:

```text
22/tcp ALLOW IN 192.168.4.0/22
```

Rather than exposing SSH to all network sources, the rule restricts SSH access to the specified local network range.

### Security Considerations

This configuration demonstrates the principle of **default deny**:

* Unsolicited inbound connections are blocked by default.
* Outbound connections are permitted.
* Routed traffic is denied by default.
* SSH is explicitly permitted only from the authorized network range.

The configuration was then verified using:

```bash
sudo ufw status verbose
```

This provided practical experience with host-based firewall configuration, network access control, service-specific exceptions, and least-privilege network policy.

## 7. SSH Key-Based Authentication

SSH remote administration was configured using an asymmetric cryptographic key pair generated on a Microsoft Surface device.

The key pair consisted of:

* **Private key** — retained on the client device and kept confidential
* **Public key** — placed on the Ubuntu system for authentication

The private key was not copied to the Ubuntu host.

### Security Model

The SSH connection uses asymmetric cryptography to authenticate the client.

The client proves possession of the private key without transmitting the private key to the server.

The public key can be stored on the Ubuntu system because it does not provide the ability to authenticate as the user by itself.

### Authentication Flow

```text
Microsoft Surface
       |
       | Private Key
       |
       v
    SSH Client
       |
       | Authentication
       v
 Ubuntu Linux
       |
       | Public Key
       v
Authorized Key
```

### Security Considerations

The private key is sensitive and must be protected from unauthorized access.

The SSH configuration was combined with the UFW firewall rules documented above, restricting inbound SSH access to the authorized local network.

This created multiple security layers:

1. Network-level restriction through UFW
2. SSH service authentication
3. Asymmetric key-based authentication
4. Protection of the private key on the client

### Skills Demonstrated

* SSH administration
* Public-key authentication
* Asymmetric cryptography
* Linux remote administration
* Secure key management
* Firewall-based access restriction
* Defense in depth


## 8. Security Philosophy

The hardening process followed several core security principles:

### Least Privilege

Users and processes should have only the permissions required to perform their intended functions.

### Attack Surface Reduction

Unnecessary services, applications, ports, and privileges increase the number of potential paths an attacker could exploit.

### Visibility

Security controls are more effective when administrators can identify what is running, which users have privileges, and which services are exposed.

### Verification

Security configuration should be tested rather than assumed to be correct.

## 9. Skills Demonstrated

* Linux user administration
* Privilege management
* Sudo administration
* SUID enumeration
* File permissions
* File ownership
* Process investigation
* Service investigation
* Network service enumeration
* Least-privilege implementation
* Linux security hardening

## 10. Lessons Learned

Linux security depends heavily on understanding the relationship between users, groups, permissions, processes, services, and privileged executables.

A system cannot be considered secure simply because security settings have been configured. The configuration must also be examined and tested to verify that the intended security boundaries actually exist.

## 11. Next Steps

* Investigate listening network services
* Review authentication configuration
* Configure logging and monitoring
* Perform controlled vulnerability assessment
* Document security findings and remediation
