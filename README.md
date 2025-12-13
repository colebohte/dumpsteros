# Dumpster OS
DumpsterOS is a tiny <50MB Linux distrobution designed to run on PCs you pull out of the dumpster.

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
DUMPSTEROS="/path/to/git/"
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
### To Create, Format, and Mount an Empty Img:
```bash
cd $DUMPSTEROS

dd if=/dev/zero of=dumpsteros.img bs=1M count=100

mkfs.ext4 dumpsteros.img

mkdir -p mnt
sudo mount -o loop dumpsteros.img mnt
```
### Copy rootfs to the Image
```bash
sudo cp -a rootfs/* mnt/
```
### Install extlinux Bootloader
```bash
sudo extlinux --install mnt/boot
```
### Create a Bootloader Config:
```bash
sudo tee mnt/boot/extlinux.conf << 'EOF'
DEFAULT dumpsteros
PROMPT 0
TIMEOUT 50

LABEL dumpsteros
  LINUX /boot/vmlinuz
  APPEND root=/dev/sda rw console=tty0
EOF
```
### Copy the Kernel to Boot
```bash
sudo cp linux-6.12.4/arch/x86/boot/bzImage mnt/boot/vmlinuz
```
### Unmount
```bash
sudo umount mnt
```
### Install the MBR
```
sudo dd bs=440 count=1 conv=notrunc if=/usr/lib/syslinux/mbr/mbr.bin of=dumpsteros.img
```

## 5. Boot with QEMU
We now have a bootable img at `$DUMPSTEROS/dumpsteros.img`, so lets boot it.
```bash
qemu-system-x86_64 -drive file=dumpsteros.img,format=raw -m 512M
```


## Resources
* [BusyBox 1.36.1 > https://busybox.net/downloads/busybox-1.36.1.tar.bz2](https://busybox.net/downloads/busybox-1.36.1.tar.bz2)
* [Linux Kernel 6.12.4 > https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.12.4.tar.xz](https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.12.4.tar.xz)
