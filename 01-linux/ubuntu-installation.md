# Ubuntu Installation & Dual-Boot Lab

## Objective

Install Ubuntu Linux on a dedicated NVMe drive while maintaining a separate Windows installation, creating a dual-boot environment for cybersecurity training and simulated network administration.

## Lab Environment

* CPU: AMD Ryzen 7 3700X
* RAM: 64 GB
* GPU: NVIDIA RTX 3070
* Existing storage: Windows NVMe drive
* Additional storage: Dedicated NVMe drive for Ubuntu
* Firmware: UEFI

## Installation Process

### 1. Prepared Ubuntu Installation Media

Created a bootable Ubuntu USB drive with rufus and booted the system from the USB through the UEFI boot menu.

The Ubuntu installer provided the option to either test Ubuntu or begin the installation.

### 2. Installed Ubuntu on Dedicated NVMe

The new NVMe drive was selected as the target for the Ubuntu installation.

The Windows drive was kept in its original state.

### 3. Configured UEFI Boot

After installation, the system's UEFI boot configuration contained separate boot entries for:

* Ubuntu
* Windows Boot Manager

Ubuntu was configured as the primary boot entry while retaining the ability to boot Windows.

### 4. Troubleshooting Windows Boot

After the initial installation, selecting Windows from the UEFI boot menu resulted in a black screen.

The system was subsequently able to boot Windows after an automatic disk check completed.

This demonstrated the importance of understanding the relationship between:

* UEFI
* Boot entries
* EFI System Partitions
* Operating-system bootloaders
* Disk integrity

## Verification

After installation, Ubuntu was successfully booted and configured.

## Skills Demonstrated

* Linux installation
* UEFI boot configuration
* Dual-boot configuration
* NVMe storage identification
* Windows/Linux interoperability
* Network connectivity testing
* Troubleshooting boot issues
* Command-line troubleshooting

## Lessons Learned

The installation reinforced the importance of understanding the difference between physical storage devices, partitions, EFI boot partitions, and operating-system bootloaders.

It also demonstrated that troubleshooting a dual-boot environment requires separating the problem into hardware, firmware, storage, bootloader, operating-system, and network layers.

## Next Steps

* Document Linux system administration
* Harden the Ubuntu installation
* Configure and test firewall rules
* Analyze running services
* Review file and directory permissions
