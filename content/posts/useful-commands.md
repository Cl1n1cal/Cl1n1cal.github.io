---
title: "Useful commands"
date: 2025-01-14
draft: false
---

### Useful commands
See a list of addresses and symbols defined in the linux kernel
```bash
head /proc/kallsyms
```
See a specific address for a symbol
```bash
cat /proc/kallsyms | grep commit_creds
```

View kernel modules:
```bash
cat /proc/modules
```

To use function names for breakpoints you can do the following in gdb:
```bash
add-symbol-file <driver-path> <base-address>
```

See if SMEP is enabled from inside of the machine (requires root)(extend terminal width):
```bash
cat /proc/cpuinfo | grep smep
```

Enable SMEP in qemu
```bash
-cpu kvm64,+smep
```

See if SMAP is enabled from inside of the machine (requires root)(extend terminal width):
```bash
cat /proc/cpuinfo | grep smap
```

Enable SMAP in qemu:
```bash
-cpu kvm64,+smap
```

Enabling KPTI in qemu:
```bash
-append "... pti=on ..."
```

If KPTI is enabled we need to use the following macro when returning to user mode. Otherwise we will still have the kernel page table and segfault.
```bash
cat /proc/kallsyms | grep swapgs_restore_regs_and_return_to_usermode
```

Extract a bzImage:
```bash
extract-vmlinux bzImage > vmlinux
```

Extract .cpio file:
```bash
mkdir root   
cd root; cpio -idv < ../rootfs.cpio
```

Repackage into .cpio (from inside the root folder):
```bash
find . -print0 | cpio -o --format=newc --null > ../rootfs_updated.cpio
```
Grepping gadgets that contain two important things but not necessarily right after the other:
```bash
grep -E "push rdx.*pop rsp" gadgets.txt
```
Add this to your GDB to view kernel addresses:
```
echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
```
KPTI trampoline macro:
```bash
cat /proc/kallsyms | grep swapgs_restore_regs_and_return_to_usermode
```

Find modprobe address using ptrlib (remember to activate env):
```bash
$ python   
>>> from ptrlib import ELF   
>>> kernel = ELF("./vmlinux")   
>>> hex(next(kernel.search("/sbin/modprobe\0")))   
0xffffffff81e38180
```
Qemu monitor (nographic):
```bash
Ctrl+a
c
```
Se resten her: https://github.com/Myldero/kernelinit/blob/master/tricks.md
