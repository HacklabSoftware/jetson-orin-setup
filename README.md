# jetson-orin-setup
Jetson Orin Setup r36.3 (similar steps for other version)


### Step 1
- Download Driver Package (BSP): https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v3.0/release/jetson_linux_r36.3.0_aarch64.tbz2
- Download Sample Root Filesystem: https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v3.0/release/tegra_linux_sample-root-filesystem_r36.3.0_aarch64.tbz2

### Step 2
```
tar xf jetson_linux_r36.3.0_aarch64.tbz2
sudo tar xpf tegra_linux_sample-root-filesystem_r36.3.0_aarch64.tbz2 -C Linux_for_Tegra/rootfs/
cd Linux_for_Tegra/
sudo ./apply_binaries.sh
sudo ./tools/l4t_flash_prerequisites.sh
```

### Step 3
Short hardware pins in jetson nano
![short these pins](https://forums.developer.nvidia.com/uploads/short-url/kOTtFVT26ET1CsDvcD608QXBZT.jpeg?dl=1)

Connect USB cable to PC
Restart the Jetson

### Step 4
Run the flash command to flash the image in nvme drive 
```
 sudo ./flash.sh jetson-orin-nano-devkit-nvme internal
```




__________________________________________________

### Kernel Customization 
[Source](https://docs.nvidia.com/jetson/archives/r36.3/DeveloperGuide/SD/Kernel/KernelCustomization.html)


##### Install Prerequisites
```
sudo apt install git-core
sudo apt install build-essential bc
```


##### Setup Toolchain
Download Bootlin Toolchain. For v36.3[https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v3.0/toolchain/aarch64--glibc--stable-2022.08-1.tar.bz2]


```
mkdir $HOME/l4t-gcc
cd $HOME/l4t-gcc
tar xf aarch64--glibc--stable-2022.08-1.tar.bz2
export CROSS_COMPILE=$HOME/l4t-gcc/aarch64--glibc--stable-2022.08-1/bin/aarch64-buildroot-linux-gnu-
```

##### Get kernel Sources

To get the kernel source, run the source_sync.sh script:

$ cd <install-path>/Linux_for_Tegra/source
$ ./source_sync.sh -k -t <release-tag>
The correct release-tag is specified in the release notes([source](https://docs.nvidia.com/jetson/archives/r36.3/ReleaseNotes/Jetson_Linux_Release_Notes_r36.3.pdf)) . This tag name syncs the sources to the source revision from which the release binary was built.

```
cd ~/Documents/Linux_for_Tegra/source
./source_sync.sh -k -t jetson_36.3
```

##### Building the Jetson linux Kernel
```
sudo apt-get update                                                                      
sudo apt-get install flex
sudo apt-get update                                                                            
sudo apt-get install bison
sudo apt install libncurses5-dev libncursesw5-dev
cd ~/Documents/Linux_for_Tegra/source
./generic_rt_build.sh "enable"
export CROSS_COMPILE=$HOME/l4t-gcc/aarch64--glibc--stable-2022.08-1/bin/aarch64-buildroot-linux-gnu-
make -C kernel

export INSTALL_MOD_PATH=~/Documents/Linux_for_Tegra/rootfs/
sudo -E make install -C kernel
cp kernel/kernel-jammy-src/arch/arm64/boot/Image \
  ~/Documents/Linux_for_Tegra/kernel/Image
```

##### Building the Nvidia Out-of-Tree Modules
```
cd ~/Documents/Linux_for_Tegra/source
export IGNORE_PREEMPT_RT_PRESENCE=1
export CROSS_COMPILE=$HOME/l4t-gcc/aarch64--glibc--stable-2022.08-1/bin/aarch64-buildroot-linux-gnu-
export KERNEL_HEADERS=$PWD/kernel/kernel-jammy-src
make modules

export INSTALL_MOD_PATH=~/Documents/Linux_for_Tegra/rootfs/
sudo -E make modules_install

cd ~/Documents/Linux_for_Tegra/
sudo ./tools/l4t_update_initrd.sh
```

##### Building the DTBs (Device Tree)

```
cd ~/Documents/Linux_for_Tegra/source
export CROSS_COMPILE=$HOME/l4t-gcc/aarch64--glibc--stable-2022.08-1/bin/aarch64-buildroot-linux-gnu-
export KERNEL_HEADERS=$PWD/kernel/kernel-jammy-src
make dtbs
cp nvidia-oot/device-tree/platform/generic-dts/dtbs/* \
     ~/Documents/Linux_for_Tegra/kernel/dtb/
```

Sources: 
1. Quick Start guides[https://docs.nvidia.com/jetson/archives/r35.4.1/DeveloperGuide/text/IN/QuickStart.html#in-quickstart]
2. Flashing support[https://docs.nvidia.com/jetson/archives/r35.4.1/DeveloperGuide/text/SD/FlashingSupport.html#sd-flashingsupport-flashshorinnxnano]
3. Jetson Linux Download Page[https://developer.nvidia.com/embedded/jetson-linux-r363]


