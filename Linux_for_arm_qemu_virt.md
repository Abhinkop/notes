Linux for arm qemu virt

1. Build the kernel for ARM64 virt platform with debug info
    1. Prepare the environment
        sudo apt update
        sudo apt install build-essential gcc-aarch64-linux-gnu make bc flex bison libssl-dev libncurses-dev qemu-system-arm gdb-multiarch
    2. Download Linux kernel source
        git clone https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git
        cd linux
        # Checkout a stable version, e.g. 6.5, as it is known for stability and compatibility.
        # Other stable versions can also be used depending on your requirements.
        git checkout v6.5
    3. Configure the kernel for virt platform with debug info
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

    4. Build the kernel and dtb
        # Adjust the -j value based on your system's capacity to avoid instability.
        # For example, use -j4 for 4 cores or -j$(nproc) for all available cores.
        make -j$(nproc) ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- Image dtbs
        
        - arch/arm64/boot/Image — the kernel image
            # The kernel image is the compiled Linux kernel binary that is loaded into memory during the boot process.
        - arch/arm64/boot/dts/virt.dtb — device tree blob for virt platform
            # The device tree blob (DTB) provides hardware description information to the kernel, enabling it to initialize and interact with the hardware correctly.

2. Build initramfs for ARM64 virt platform using busybox
    1. Get BusyBox source
        git clone https://busybox.net/git/busybox.git
        cd busybox
        git checkout 1_36_1  # or latest stable

    2. Configure for static build (for initramfs)
        make distclean
        make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- defconfig
        make menuconfig
            Set Build Options → Build BusyBox as a static binary (no shared libs) = [*]

    3. Disable tc
        make menuconfig
        Navigate to: Networking Utilities
        Uncheck: tc
        Exit and save

    4. Create initramfs directory structure
        mkdir -p initramfs/{bin,sbin,etc,proc,sys,usr/bin,usr/sbin,dev}

    5. Install busybox
        make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- CONFIG_PREFIX=$PWD/initramfs/ install

    6. Create a basic init script
        Create initramfs/init file:                #!/bin/sh
            mount -t proc none /proc
            mount -t sysfs none /sys
            echo -e "\nBooted into BusyBox minimal rootfs!"
            exec /bin/sh
        Make it executable:
            chmod +x initramfs/init

    7. Create the initramfs archive
        find . | cpio -o -H newc | gzip > ../initramfs.cpio.gz

3. Run using qemu
     - Without debugging: qemu-system-aarch64   -machine virt   -cpu cortex-a57   -m 1G   -kernel arch/arm64/boot/Image   -initrd initramfs.cpio.gz   -append "console=ttyAMA0"   -nographic
     - With debugging as gdb server: add "-s -S"
