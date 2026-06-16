# 🖥️ DELL OptiPlex 7070 Hackintosh OpenCore EFI

![macOS](https://img.shields.io/badge/macOS-Sequoia-orange) ![OpenCore](https://img.shields.io/badge/OpenCore-1.0.4-blue) ![Status](https://img.shields.io/badge/Status-Stable-brightgreen)

戴尔 OptiPlex 7070 黑苹果 OpenCore EFI 配置，基于 Intel Core i7-9700T + Q370 芯片组。

## 硬件配置

| 组件 | 型号 | 驱动情况 |
|------|------|:--------:|
| 主板 | DELL OPTIPLEX 7070 (Q370 芯片组) | ✅ |
| CPU | Intel Core i7-9700T (Coffee Lake-U, 8C/8T) | ✅ |
| 核显 | Intel UHD Graphics 630 (Device ID: 8086-3E98) | ✅ 硬件加速正常 |
| 声卡 | Realtek ALC255 (ALC255, layout-id: 66) | ✅ 输入输出正常 |
| 有线网卡 | Intel I219-LM | ✅ |
| 无线网卡 | ~~Qualcomm QCA61x4A~~ | ❌ SSDT 屏蔽 |
| 蓝牙 | ~~Qualcomm QCA61x4A Bluetooth (0CF3-E007)~~ | ❌ |
| 硬盘 | ST1000LM049 (SATA) + Intel Optane H10 NVMe | ✅ |
| 内存 | 16GB DDR4 | ✅ |
| 显示器 | 24P1W1 (DP, 1920×1080) | ✅ |

## BIOS 设置

| 选项 | 设置 |
|------|:----:|
| BIOS 版本 | 1.35.0 (2025-09-04) |
| SATA Operation | AHCI |
| Secure Boot | Disabled |
| UEFI Boot | Enabled |
| VT-d | Disabled (或开启并勾选 Kernel → DisableIoMapper) |
| CFG Lock | Unlocked |
| DVMT Pre-Allocated | 64MB+ |

## 配置详情

### SMBIOS

| 项目 | 值 |
|------|:---:|
| 机型 | iMac19,1 |
| AAPL,ig-platform-id | 00009B3E |
| framebuffer-stolenmem | 00003001 (48MB) |
| boot-args | `-v debug=0x100 keepsyms=1 igfxonln=1` |

> 请自行生成三码（SMBIOS），不要直接使用本仓库的序列号。

### Kexts

| Kext | 版本 | 说明 |
|------|:---:|:----:|
| [Lilu.kext](https://github.com/acidanthera/Lilu) | v1.7.3 | 内核扩展注入框架 |
| [VirtualSMC.kext](https://github.com/acidanthera/VirtualSMC) | v1.3.8 | SMC 模拟 |
| [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen) | v1.7.1 | 显卡补丁 |
| [AppleALC.kext](https://github.com/acidanthera/AppleALC) | v1.9.8 | 声卡驱动 (layout 66) |
| [IntelMausiEthernet.kext](https://github.com/acidanthera/IntelMausi) | v3.0.3 | 有线网卡驱动 |
| [SMCProcessor.kext](https://github.com/acidanthera/VirtualSMC) | v1.3.8 | CPU 传感器 |
| [SMCSuperIO.kext](https://github.com/acidanthera/VirtualSMC) | v1.3.8 | 风扇/温度传感器 |
| [USBToolBox.kext](https://github.com/USBToolBox/tool) | v1.2.0 | USB 工具盒 |
| [UTBMap.kext](https://github.com/USBToolBox/tool) | v1.1 | USB 定制映射 (仅后置USB口可用) |
| [XHCI-unsupported.kext](https://github.com/RehabMan/OS-X-USB-Inject-All) | v0.9.2 | XHCI 控制器兼容性 |

### ACPI 热补丁 (SSDT)

| SSDT | 功能 |
|:----|:----:|
| SSDT-EC.aml | 仿冒嵌入式控制器 |
| SSDT-PLUG.aml | XCPM 电源管理插件 |
| SSDT-PMC.aml | NVRAM PMC 补丁 |
| SSDT-MCHC.aml | MCHC 设备仿冒 |
| SSDT-SBUS.aml | SMBus 支持 |
| SSDT-HPET.aml | HPET IRQ 修复 (配合 _STA→XSTA, _CRS→XCRS 重命名) |
| SSDT-RTCAWAC.aml | RTC AWAC 兼容性修复 |
| SSDT-Disable_Network_RP08.aml | 屏蔽板载 Qualcomm 无线网卡 |
| SSDT-Disable_NVMe_RP17.aml | 屏蔽 Optane NVMe 占用 RP17 |

### Drivers

| Driver | 说明 |
|:-------|:----:|
| HfsPlus.efi | HFS+ 文件系统支持 |
| OpenRuntime.efi | OpenCore 运行时环境 |
| OpenCanopy.efi | 图形引导界面 |
| ResetNvramEntry.efi | NVRAM 重置入口 |

## macOS 兼容性

| macOS 版本 | 状态 |
|:-----------|:----:|
| macOS Sequoia 15.x | ✅ 已测试 |
| macOS Sonoma 14.x | ✅ |
| macOS Ventura 13.x | ✅ |
| macOS Monterey 12.x | ✅ |

## 正常工作

- [x] Intel UHD Graphics 630 硬件加速 / 视频编码解码
- [x] DP 视频输出 (1920×1080@60Hz)
- [x] 声卡输入输出 (3.5mm 接口)
- [x] 有线网卡 (Intel I219-LM)
- [x] 睡眠 / 唤醒
- [x] CPU 睿频 / XCPM 电源管理
- [x] USB 2.0/3.0 (后置定制端口)
- [x] NVRAM 读写
- [x] iMessage / FaceTime / iCloud (需三码)
- [x] App Store
- [x] 开机音效 (OpenCanopy GUI)

## 不工作

- [ ] 板载 Qualcomm 无线网卡 (硬件不支持 macOS)
- [ ] 板载 Qualcomm 蓝牙 (硬件不支持 macOS)
- [ ] 英特尔 Optane NVMe (SSDT 屏蔽)

## USB 端口映射

USB 已通过 USBToolBox + UTBMap 定制映射，仅启用后置的可用 USB 端口。

## 致谢

- [Acidanthera](https://github.com/acidanthera) 开发维护 OpenCore 及核心驱动
- [Dortania](https://dortania.github.io/OpenCore-Install-Guide/) 提供详细的 OpenCore 安装指南
- [USBToolBox](https://github.com/USBToolBox/tool) USB 定制工具

## DVMT (动态显存) 修改

Dell OptiPlex 7070 的 BIOS 默认将 DVMT Pre-Allocated 锁定为 32MB，这会导致 UHD 630 在 macOS 下出现开机黑屏或显存异常问题。

本 EFI 已通过 WhateverGreen 的 `framebuffer-stolenmem` 补丁（`00003001`）实现了软件层面的 DVMT 修正。但更彻底的方案是通过 **modGRUBShell** 直接修改 BIOS NVRAM 中的 DVMT 变量，将其永久改为 64MB。

### 修改步骤

1. 重启电脑，在 OpenCore 引导菜单中选中 **modGRUBShell**（辅助工具，默认隐藏，按空格键显示）
2. 输入以下命令检查当前 DVMT 值：

   ```
   setup_var 0x8DC
   ```

   > 若返回 `0x01` 或其他值均可继续；若提示错误，说明该地址不适用于你的 BIOS 版本，请勿继续。

3. 将 DVMT Pre-Allocated 设为 64MB：

   ```
   setup_var 0x8DC 0x02
   ```

4. 重启电脑，检查图形性能是否正常

### 值对照表

| 值 | 显存大小 |
|:--:|:--------:|
| 0x00 | 0MB |
| 0x01 | 32MB (默认) |
| **0x02** | **64MB (推荐)** |
| 0x03 | 96MB |
| 0x04 | 128MB |

### 注意事项

- 如不确定 `0x8DC` 是否适用于你的 BIOS 版本，请先用 `setup_var 0x8DC` 测试，若地址有效则会返回当前值
- 设置后建议重新启动进入 BIOS 确认 "Primary Display" 设为 "Intel HD Graphics"
- 完成 DVMT 修改后，如果出现图形异常，可在 config.plist 中调整 `framebuffer-stolenmem` 的值或删除该补丁
