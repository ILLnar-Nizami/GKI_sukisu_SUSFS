# GKI Kernel Build Automation

> Non-GKI devices can try the resources on the [SukiSU cloud drive](https://alist.shirkneko.top). OnePlus ColorOS 14/15 is not supported.
>
> If this is your first time here, please **read everything carefully** so you don’t waste other people’s time.
>
> Latest updates: 1) OnePlus 8 ELITE SoC can use the 6.6 kernel (untested). 2) Fixed build errors for these GKI versions — [5.10.(66, 81, 101), 5.15.(74, 94, 104)].

### Downloads

You can [download everything here](https://github.com/zzh20188/GKI_KernelSU_SUSFS/releases).

1. **AnyKernel3.zip**
   - Download and flash directly.
   - Use a flasher such as [HorizonKernelFlasher](https://github.com/libxzr/HorizonKernelFlasher/releases).
2. **boot.img**
   - Download the archive that matches your kernel format (no compression, gz, lz4) and review the [boot.img guide](https://kernelsu.org/zh_CN/guide/installation.html#install-by-kernelsu-boot-image).
   - Flash with [FASTBOOT](https://magiskcn.com/) or another flasher targeting the boot partition in the ROOT slot (e.g. AiWanJi, KernelFlasher).

### Supported components

| Feature | Description |
| --- | --- |
| [KernelSU](https://kernelsu.org/zh_CN/) | Supports **stock, MKSU, SUKISU, NEXT** |
| [SUSFS4](https://gitlab.com/simonpunk/susfs4ksu) | Kernel-level helper patches for hiding KSU |
| [BBR](https://blog.thinkin.top/archives/ke-pu-bbrdao-di-shi-shi-me) | TCP congestion control to speed up the network |
| [Wireguard](https://zh.wikipedia.org/wiki/WireGuard) | See the linked wiki |
| [LZ4KD](https://github.com/ShirkNeko/SukiSU_patch/tree/main/other) | A ZRAM algorithm said to come from HUAWEI sources, ported by [Yuncai Zhifeng](http://www.coolapk.com/u/24963680) |

<details>
<summary>Additional ZRAM algorithms</summary>

- LZ4K
- LZ4HC
- deflate
- 842
- ~~zstdn~~
- lz4k_oplus

</details>

### KSU manager

After the build finishes you will see files like `Next-Manager(12600)`. That is simply the ***latest manager*** uploaded together with the kernel.
![Example](./assets/get_manager.gif)
The [Release](https://github.com/zzh20188/GKI_KernelSU_SUSFS/releases) assets also contain the ***latest manager***.
![release](./assets/release_manager.gif)

### Emergency recovery guide

> [!IMPORTANT]
> **When to trigger**  
> Run recovery if the device cannot boot because:  
> - You flashed an incorrect or incompatible kernel
> - Kernel version mismatch (e.g. 5.10.66 flashing a 233 build)

1. **Enter FASTBOOT mode**
   - Use Power + Volume- hardware keys or run `adb reboot bootloader`.
2. **Flash boot.img**
```bash
$ fastboot flash boot <full boot.img filename>
```
### Ways to obtain stock boot images

1. **Extract from existing firmware**
   - OTA package: unpack it and run the [payload-dumper tool](https://magiskcn.com/payload-dumper-go-boot.html).
   - Full flashing package: unpack and grab `boot.img` directly.
2. **Use external sources**
   - Search community platforms with “device model + stock boot” (e.g. XDA, Coolapk).
   - Use [mobile-friendly remote extraction](https://magiskcn.com/payload-dumper-compose.html).

> [!TIP]
> ### Kernel version compatibility guide
> 
> **1. Cross minor-version flashing**  
> If the device GKI major version is 5.10.x (e.g. 5.10.168) you can flash kernels with the same major but higher minors (e.g. 5.10.198).  
> For **X-lts** releases, using `android12-5.10.X-lts-AnyKernel3.zip` as the example:
> - **X-lts** means a long-term support build (highest minor, 5.10.236 here)
> - The LTS build number keeps increasing as the GKI source is updated (others like 198 stay permanent)
> - ⚠️ Note: Latest ≠ most stable (e.g. 6.6.x can reboot randomly)
> 
> **2. Kernel version spoofing**  
> Run this in the MT Manager terminal:
> ```bash
> uname -r | sed 's/^[^-]*//'
> ```
> Copy the result and paste the version into the Action build panel to spoof it.
> 
> **3. Build optimization tips**  
> Adjust the [workflow config](.github/workflows/kernel-a12-5.10.yml) (e.g. kernel-a12-5.10.yml):
> - ▶️ Remove/comment unused GKI options (**faster builds**)
> - ➕ Add the exact GKI versions you need (see the [customization guide](https://www.coolapk.com/feed/62820671?shareKey=OGMxYmZmNTk0YzIxNjgxNzM1MzI~&shareUid=11253396&shareFrom=com.coolapk.market_15.2.2))
> - 📅 Modify the kernel build timestamp per the note around line 490 in [gki-kernel.yml](.github/workflows/gki-kernel.yml)

### More

Feel free to share ideas or requests — I’ll do my best!
