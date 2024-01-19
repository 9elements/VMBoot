# VMBoot concept

VMBoot presents a PoC of booting into [TianoCore/EDK2](https://github.com/tianocore/edk2) firmware with only Open Source Firmware, namely [coreboot](https://www.coreboot.org/) and Linuxboot/[u-root](https://u-root.org/), on the flash chip .
It utilizes [gokvm](https://github.com/bobuhiro11/gokvm), a small Linux-KVM hypervisor written in pure Go, which is integrated into u-root as VMBoot and it's able to execute an EDK2 firmware image.
For loading the firmware and basic setup of the virtual machine, the [PVH Boot Protocol](https://github.com/mirage/xen/blob/master/docs/misc/pvh.markdown) and [HMV direct boot ABI](https://github.com/mirage/xen/blob/master/docs/misc/hvmlite.markdown) are used.

### Demo
[![asciicast](https://asciinema.org/a/785rLfVhSdpnGsfY13fIJi5ke.svg)](https://asciinema.org/a/785rLfVhSdpnGsfY13fIJi5ke)

## Status gokvm
 - [gokvm](https://github.com/bobuhiro11/gokvm)
 - gokvm is able to boot into [EDK2/CloudHV](https://github.com/cloud-hypervisor/edk2/tree/ch) for [Cloud-Hypervisor](https://github.com/cloud-hypervisor/cloud-hypervisor) until the EFI-Shell.
 - device passthrough via VirtIO is limited to block devices and network

## Status vmbootloader in u-root
 - [vmboot](https://github.com/u-root/u-root/tree/main/cmds/exp/vmboot)
 - iterates over block devices and mounts partition with EDK2 image
 - loads EDK2 image from mounted block device (only XFS file system)
 - runs EDK2 in gokvm until EFI-Shell
 - experimental state to show that it is possible to start a vm from u-root and execute EDK2 in the VM.

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

## Procedure
- build linux kernel with example config
- build u-root initrd with vmboot
- build coreboot for desired platform and use linux kernel and u-root initrd as payload
- flash coreboot image on device
- place EDK2/CloudHv image on block device attached to machine (XFS filesystem on block device required)
- boot machine and execute vmboot

## Further work
 - More platforms need to be testes.
 - Extension and improvments of gokvm and vmboot is required

## Blog posts
- [VMBoot Proof of Concept](https://9esec.io/blog/vmboot-poc/)

## _References_:

- [Interview with Ron Minich](https://archive.fosdem.org/2007/interview/ronald+g+minnich.html)
- [UEFI Spec 2.10](https://uefi.org/sites/default/files/resources/UEFI_Spec_2_10_Aug29.pdf)
- [ACPI Spec 6.5](https://uefi.org/sites/default/files/resources/ACPI_Spec_6_5_Aug29.pdf)
- [gokvm](https://github.com/bobuhiro11/gokvm)
- [u-root](https://github.com/u-root/u-root)
- [u-root/vmboot](https://github.com/u-root/u-root/tree/main/cmds/exp/vmboot)
- [coreboot](https://www.coreboot.org/)
- [HMV direct boot ABI](https://github.com/mirage/xen/blob/master/docs/misc/hvmlite.markdown)
- [HMV Structures](https://github.com/torvalds/linux/blob/master/include/xen/interface/hvm/start_info.h)
- [PVH Boot Protocol](https://github.com/mirage/xen/blob/master/docs/misc/pvh.markdown)
- [Cloud Hypervisor](https://github.com/cloud-hypervisor/cloud-hypervisor)
- [EDK2/CloudHV](https://github.com/cloud-hypervisor/edk2/tree/ch)


## Funding

This project is funded through the [NGI Assure Fund](https://nlnet.nl/assure), a fund established by [NLnet](https://nlnet.nl) with financial support from the European Commission's [Next Generation Internet](https://ngi.eu) program. Learn more at the [NLnet project page](https://nlnet.nl/project/UEFI-isolation).

[<img src="https://nlnet.nl/logo/banner.png" alt="NLnet foundation logo" width="20%" />](https://nlnet.nl)
[<img src="https://nlnet.nl/image/logos/NGIAssure_tag.svg" alt="NGI Assure Logo" width="20%" />](https://nlnet.nl/assure)