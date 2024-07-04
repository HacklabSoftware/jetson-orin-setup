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





Sources: 
1. Quick Start guides[https://docs.nvidia.com/jetson/archives/r35.4.1/DeveloperGuide/text/IN/QuickStart.html#in-quickstart]
2. Flashing support[https://docs.nvidia.com/jetson/archives/r35.4.1/DeveloperGuide/text/SD/FlashingSupport.html#sd-flashingsupport-flashshorinnxnano]
3. Jetson Linux Download Page[https://developer.nvidia.com/embedded/jetson-linux-r363]


