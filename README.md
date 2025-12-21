# <br><img width="5120" height="1024" alt="Image" src="https://github.com/user-attachments/assets/7ee8af7a-7bd9-4126-b645-ce2f814c9b8a" />

Dumpster OS is a lightweight, minimal Linux distribution built almost entirely from scratch, weighing in at under 75MB. Designed with extreme resource efficiency in mind, it's perfect for breathing new life into hardware that others have thrown away, literally or figuratively.

## Philosophy

Born from the idea that computing doesn't require bloat, Dumpster OS strips away everything unnecessary to deliver a fast, functional, and surprisingly capable system. Whether you're reviving a decades-old laptop found in a dumpster, building an embedded system, or just want to understand how Linux works at its core, Dumpster OS provides a clean, minimalist foundation.

## What Makes It Different

Unlike mainstream distributions that come bundled with GNU utilities and massive package repositories, Dumpster OS takes a different approach:

- **Pure Linux/Busybox stack**: No GNU bloat, just the Linux kernel and Busybox providing 300+ essential Unix utilities in a single ~1MB binary
- **Sub-75MB footprint**: The entire operating system, bootloader included, fits comfortably on ancient hardware or embedded devices
- **Built from scratch**: Every component compiled and configured manually, giving you complete control and understanding of your system
- **Lightning fast boot**: Optimized init sequence gets you to a working shell in seconds
- **Perfect for learning**: Clean, minimal codebase makes it ideal for understanding how Linux distributions are actually built

## Use Cases

- Rescue old hardware from obsolescence
- Embedded systems and IoT devices  
- Learning platform for understanding Linux internals
- Minimalist computing philosophy
- Portable system on USB drives
- Virtual machine testing environments
- Base for custom specialized distributions

DumpsterOS proves that powerful computing doesn't require gigabytes of dependencies—sometimes all you need is a kernel, a shell, and the will to make it work.

# Using
### Note: Dumpster OS 26 BETA 2 has NOT been tested on any real hardware. Use at your own risk.

## Booting in QEMU
Note: You have to have QEMU installed beforehand.
To boot from ISO in QEMU use: 
```bash
qemu-system-x86_64 -cdrom /path/to/dumpsteros.iso -m 512M
```

## Resources
* [BusyBox 1.36.1 > https://busybox.net/downloads/busybox-1.36.1.tar.bz2](https://busybox.net/downloads/busybox-1.36.1.tar.bz2)
* [Linux Kernel 6.12.4 > https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.12.4.tar.xz](https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.12.4.tar.xz)
