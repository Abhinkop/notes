# Linux for arm qemu virt

## 1. Build the kernel for ARM64 virt platform with debug info
### 1.1 Prepare the environment
```bash   
sudo apt update
sudo apt install build-essential gcc-aarch64-linux-gnu make bc flex bison libssl-dev libncurses-dev qemu-system-arm gdb-multiarch
```   

### 1.2 Download Linux kernel source
```bash   
git clone https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git
cd linux
# Checkout a stable version, e.g. 6.5, as it is known for stability and compatibility.
# Other stable versions can also be used depending on your requirements.
git checkout v6.5
```

### 1.3 Configure the kernel for virt platform with debug info
```bash   
# Clean old config
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- mrproper

# Configure for virt platform with defconfig
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- defconfig

# Enable debug information for the kernel
scripts/config --file .config \
    --enable DEBUG_INFO \
    --disable DEBUG_INFO_REDUCED

# Disable reduced debug information for better debugging
# Optional: enable KGDB for kernel debugging
# KGDB (Kernel GNU Debugger) allows debugging the Linux kernel using gdb.
# It is useful for kernel development and debugging but may not be necessary for production builds.
scripts/config --file .config \
    --enable KGDB \
    --enable KGDB_SERIAL_CONSOLE \
    --enable KGDB_KDB
```   

### 1.4 Build the kernel and dtb
```bash   
    make -j$(nproc) ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- Image dtbs
```   
- `arch/arm64/boot/Image` — _The kernel image_
    - The kernel image is the compiled Linux kernel binary that is loaded into memory during the boot process.
- `arch/arm64/boot/dts/virt.dtb` — _Device tree blob for virt platform_
    - The device tree blob (DTB) provides hardware description information to the kernel, enabling it to initialize and interact with the hardware correctly.

## 2. Build initramfs for ARM64 virt platform using busybox
### 2.1 Get BusyBox source
```bash   
git clone https://busybox.net/git/busybox.git
cd busybox
git checkout 1_36_1  # or latest stable
```   

### 2.2 Configure for static build (for initramfs)
```bash   
make distclean
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- defconfig
make menuconfig
```   
- Select Build Options 
    - Set Build BusyBox as a static binary (no shared libs) = [*]

### 2.3 Disable tc
- make menuconfig
- Navigate to: Networking Utilities
- Uncheck: tc
- Exit and save

### 2.4 Create initramfs directory structure
```bash   
mkdir -p initramfs/{bin,sbin,etc,proc,sys,usr/bin,usr/sbin,dev}
```

### 2.5 Install busybox
```bash   
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- CONFIG_PREFIX=$PWD/initramfs/ install
```

### 2.6 Create a basic init script
#### 2.6.1 Create `initramfs/init` file:                
```bash   
#!/bin/sh
mount -t proc none /proc
mount -t sysfs none /sys
echo -e "\nBooted into BusyBox minimal rootfs!"
exec /bin/sh
```

#### 2.6.2 Make it executable:
```bash   
chmod +x initramfs/init
```
### 2.7. Create the initramfs archive
```bash   
find . | cpio -o -H newc | gzip > ../initramfs.cpio.gz
```   

## 3. Run using qemu
### 3.1 Without debugging: 
```bash   
qemu-system-aarch64   -machine virt   -cpu cortex-a57   -m 1G   -kernel arch/arm64/boot/Image   -initrd initramfs.cpio.gz   -append "console=ttyAMA0"   -nographic
```   

### 3.2 With debugging as gdb server: add `-s -S` flags.
