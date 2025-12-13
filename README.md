# Dumpster OS

DumpsterOS is a lightweight, minimal Linux distribution built entirely from scratch, weighing in at under 50MB. Designed with extreme resource efficiency in mind, it's perfect for breathing new life into hardware that others have thrown away, literally or figuratively.

## Philosophy

Born from the idea that computing doesn't require bloat, DumpsterOS strips away everything unnecessary to deliver a fast, functional, and surprisingly capable system. Whether you're reviving a decades-old laptop found in a dumpster, building an embedded system, or just want to understand how Linux works at its core, DumpsterOS provides a clean, minimalist foundation.

## What Makes It Different

Unlike mainstream distributions that come bundled with GNU utilities and massive package repositories, DumpsterOS takes a different approach:

- **Pure Linux/Busybox stack**: No GNU bloat, just the Linux kernel and Busybox providing 300+ essential Unix utilities in a single ~1MB binary
- **Sub-50MB footprint**: The entire operating system, bootloader included, fits comfortably on ancient hardware or embedded devices
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

# Building
This segment will run you through how to build a working version of DumpsterOS and run it in an emulator like QEMU.

## 1. Installing Dependencies

### Dependency List
- `gcc` - GNU C Compiler
- `make` - Build automation tool
- `binutils` - Binary utilities (ld, as, etc.)
- `bison` - Parser generator
- `flex` - Lexical analyzer
- `libssl-dev` - SSL development libraries
- `libelf-dev` - ELF library for kernel modules
- `bc` - Basic calculator (kernel build scripts)
- `libncurses-dev` - Terminal handling library
- `libncurses5-dev` - Ncurses compatibility
- `libncursesw5-dev` - Wide character ncurses support
- `wget` - File downloader
- `qemu-system-x86` - x86 system emulator for testing
- `isolinux` - Bootloader for ISO images
- `syslinux` - Bootloader for USB drives
- `syslinux-utils` - Syslinux utilities
- `xorriso` - ISO 9660 image creation tool
- `extlinux` - Extlinux bootloader installer

### Debian Based Distros (apt)
```bash
sudo apt update && sudo apt install -y build-essential libncurses-dev libncurses5-dev libncursesw5-dev bison flex libssl-dev libelf-dev bc wget qemu-system-x86 isolinux syslinux-utils xorriso syslinux extlinux git
```

### Arch Based Distros (pacman)
```bash
sudo pacman -Syu --needed base-devel ncurses bison flex openssl elfutils bc wget qemu-system-x86 syslinux libisoburn git
```

### Solus (eopkg)
```bash
sudo eopkg install -y -c system.devel ncurses-devel bison flex openssl-devel elfutils-devel bc wget qemu syslinux xorriso git
```

### Red Hat Based Distros (dnf)
```bash
sudo dnf install -y gcc make ncurses-devel bison flex openssl-devel elfutils-libelf-devel bc wget qemu-system-x86 syslinux xorriso git
```

### openSUSE (zypper)
```bash
sudo zypper install -y gcc make ncurses-devel bison flex libopenssl-devel libelf-devel bc wget qemu-x86 syslinux xorriso git
```

### Alpine Linux (apk)
```bash
sudo apk add build-base ncurses-dev bison flex openssl-dev elfutils-dev bc wget qemu-system-x86_64 syslinux xorriso git
```

### Void Linux (xbps)
```bash
sudo xbps-install -Sy base-devel ncurses-devel bison flex openssl-devel elfutils bc wget qemu syslinux libisoburn git
```

### Gentoo (emerge)
```bash
sudo emerge --ask sys-devel/gcc sys-devel/make sys-libs/ncurses sys-devel/bison sys-devel/flex dev-libs/openssl virtual/libelf sys-devel/bc net-misc/wget app-emulation/qemu sys-boot/syslinux app-cdr/xorriso dev-vcs/git
```

## 2. Cloning
You will need Git to clone the repository.<br>
Git was put in the dependencies list; no need to install.
```bash
git clone --depth 1 https://github.com/colebohte/dumpsteros.git dumpsteros
cd dumpsteros
```
#### Optional: Set it as an environment variable:
```bash
export DUMPSTEROS="/path/to/git/"
```

## 3. Cleaning and Building
### This will clean all build data from previous builds:
```bash
cd "$DUMPSTEROS/busybox-1.36.1"
make clean
```
### Compile BusyBox:
```bash
make -j$(nproc)
```

## 4. Build a Disk Image
### Build the initramfs
```bash
cd $DUPMSTEROS/rootfs
find . | cpio -H newc -o | gzip > ../initramfs.img
```

## 5. Boot with QEMU
We now have a bootable img at `$DUMPSTEROS/initramfs.img`, so lets boot it.
```bash
cd ..

qemu-system-x86_64 \
  -kernel linux-6.12.4/arch/x86/boot/bzImage \
  -initrd initramfs.img \
  -append "console=ttyS0" \
  -nographic
```
### To have a GUI window:
1. Remove `-nographic   -append "console=ttyS0"`
2. Add the following:
  ```
   -append "console=tty1" \
   -m 512M \
   -vga std
   ```



## Resources
* [BusyBox 1.36.1 > https://busybox.net/downloads/busybox-1.36.1.tar.bz2](https://busybox.net/downloads/busybox-1.36.1.tar.bz2)
* [Linux Kernel 6.12.4 > https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.12.4.tar.xz](https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.12.4.tar.xz)
