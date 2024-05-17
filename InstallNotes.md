# Installation Guide
This project uses qemu as an emulator for the virtual machine. The base OS used is xv6 for RISC-V architecture which is available here [xv6-riscv](https://github.com/mit-pdos/xv6-riscv)
**What cross-compilers are needed?** 

## Install Qemu
**still trying to figure this one out myself**
This follows the steps outlined in [OSTEP Projects](https://github.com/remzi-arpacidusseau/ostep-projects/blob/master/INSTALL-xv6.md) which only provides instructions for MacOS and use [xv6-public](https://github.com/mit-pdos/xv6-public) which uses x86 architecture. 

1. Download qemu for your operating system. [Qemu Download](https://www.qemu.org/download)
2. Have your cross compiler?
3. Configure and build qemu [Qemu Build System](https://www.qemu.org/docs/master/devel/build-system.html)

### Quick Start for Qemu
Introduction: [Qemu Instructions](https://www.qemu.org/docs/master/system/introduction.html#running)

A generic form of a QEMU command line

```
$ qemu-system-x86_64 [machine opts] \
                [cpu opts] \
                [accelerator opts] \
                [device opts] \
                [backend opts] \
                [interface opts] \
                [boot opts]
```
                
Note: examples of the 'help' command for qemu include
`$ qemu-system-x86_64 -M help` & `$ qemu-system-x86_64 -device scsi-hd,help`

Specific Explanation from [Drew DeVault: Getting Started](https://drewdevault.com/2018/09/10/Getting-started-with-qemu.html)
```
$ qemu-system-x86_64 \
    -enable-kvm \
    -m 2048 \
    -nic user,model=virtio \
    -drive file=alpine.qcow2,media=disk,if=virtio \
    -cdrom alpine-standard-3.8.0-x86_64.iso \
    -sdl
```
- enable-kvm: This enables use of the KVM (kernel virtual machine) subsystem to use hardware accelerated virtualisation on Linux hosts.

- m 2048: This specifies 2048M (2G) of RAM to provide to the guest.

- nic user,model=virtio: Adds a virtual network interface controller, using a virtual LAN emulated by qemu. This is the most straightforward way to get internet in a guest, but there are other options (for example, you will probably want to use -nic tap if you want the guest to do networking directly on the host NIC). model=virtio specifies a special virtio NIC model, which is used by the virtio kernel module in the guest to provide faster networking.

- drive file=alpine.qcow2,media=disk,if=virtio: This attaches our virtual disk to the guest. It’ll show up as /dev/vda. We specify if=virtio for the same reason we did for -nic: it’s the fastest interface, but requires special guest support from the Linux virtio kernel module.

- cdrom alpine-standard-3.8.0-x86_64.iso connects a virtual CD drive to the guest and loads our install media into it.

- sdl finally specifies the graphical configuration. We’re using the SDL backend, which is the simplest usable graphical backend. It attaches a display to the guest and shows it in an SDL window on the host.
