# VMBoot concept

VMBoot presents a conecpt of booting into (TianoCore/EDK2)[https://github.com/tianocore/edk2] firmware with only Open Source Firmware, namely [coreboot](https://www.coreboot.org/) and Linuxboot/[u-root](https://u-root.org/), on the flash chip .
It utilizes [gokvm](https://github.com/bobuhiro11/gokvm), a small Linux-KVM Hypervisor written in pure Go, which is integrated into u-root and is able to execute an EDK2 firmware image.

## Status gokvm
 - [gokvm](https://github.com/bobuhiro11/gokvm)
 - gokvm is able to boot into [EDK2/CloudHV](https://github.com/cloud-hypervisor/edk2/tree/ch) for [Cloud-Hypervisor](https://github.com/cloud-hypervisor/cloud-hypervisor) until the EFI-Shell.
 - device passthrough via VirtIO is limited to block devices and network

## Status vmbootloader in u-root
 - [vmboot](https://github.com/u-root/u-root/tree/main/cmds/exp/vmboot)
 - iterates over block devices and mounts partition with EDK2 image
 - loads EDK2 image from mounted block device
 - no further device pass-through implemented
 - experimental state to show that it is possible to start a vm from u-root and execute EDK2 in the VM.

To advance VMBoot improvements of gokvm and vmboot are required.

## Prerequisites
### Platform
- Platform CPUs must support AMD-V or Intel VT-x
- Platform is supported by coreboot
- coreboot+Linuxboot/u-root requires at least 10MiB free space to use on the flashchip

### Linux kernel
- build with AMD-V or Intel-VT support
- must be build with KVM support
- reduce size by remove unused drivers and features

## Platform support

Vendor | Product name | coreboot support | Status |
|------|--------------|-----------|---------------|
| Supermicro | X11SCH-F | [wip](https://review.coreboot.org/c/coreboot/+/37441) | WIP  |

## Example linux kernel configs
|Platform|
|--------|
| [Supermicro X11SCH-F](./platforms/supermicro/x11sch-f/linux_intel.config)|

## Blog posts
No blog posts yet.

### Sponsor
We thank the [NLnet Foundation](https://nlnet.nl/) for funding the research and development of this project.