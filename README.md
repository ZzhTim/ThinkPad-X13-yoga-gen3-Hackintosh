# ThinkPad X13 Yoga Gen 3 (21AW) — macOS 26.7.1 Hackintosh EFI

![macOS](https://img.shields.io/badge/macOS-26.7.1%20Tahoe-blue)

Lenovo ThinkPad X13 Yoga Gen 3（Alder Lake-P）在 macOS Tahoe 26.7.1 下的 OpenCore EFI。

> 仓库中的 SMBIOS 序列号 / MLB / UUID / ROM 均为**占位符**，请先用 GenSMBIOS 生成

---

## 硬件配置

| 部件 | 型号 |
|---|---|
| 机型 | ThinkPad X13 Yoga Gen 3 (21AW) |
| CPU | Intel Core i5-1245U（2P + 8E） |
| 核显 | Intel Iris Xe Graphics 80 EU（仿冒Tiger Lake 0x9A49） |
| 内存 | 16 GB LPDDR4X 4267Mhz |
| 硬盘 | NVMe SSD |
| 声卡 | Realtek ALC287（HDA `8086:51c8`）Layout-id:13 |
| 无线 | Intel AX211（CNVi `8086:51f0`） |
| 蓝牙 | Intel AX211 Bluetooth（USB `8087:0033`） |
| 触摸板 | Synaptics SYNA800B（I2C HID，`PNP0C50`） |
| 触摸屏 | Wacom WACF2200（I2C HID） |
| 屏幕 | 1920x1200 60hz |

## 系统环境

- macOS Tahoe **26.7.1**（25G241）
- OpenCore 1.0.8
- SMBIOS：`MacBookPro16,2`
- 引导参数：

```
amfi_get_out_of_my_way=1 -ngreentglwithgfx -allow3d -disablegfxfirmware
ngreenSched=4 ngreenV142=3 ngreen-dmc=adlp -ngreenv189 -ngreenkeepefi
-ngreenxefix -ngreenfixbootmode ngreen-eu=80 ngreen-l3=6 -ngreengva keepsyms=1
```

> Lilu 对 boot-args 有约 228 字符的截断限制，修改时注意长度。

---

## 工作状态

| 功能 | 状态 | 说明 |
|---|:---:|---|
| 核显 / Metal | ✅ | NootedGreen |
| 硬件视频解码 | ✅ | NootedGreen |
| 亮度调节 | ✅ | AppleBacklight |
| 扬声器 / 耳机 | ✅ | AppleALC + SimpleHDA |
| Wi-Fi | ✅ | AirportItlwm |
| 蓝牙 | ✅ | IntelBluetoothFirmware + IntelBTPatcher + BlueToolFixup |
| 触摸板 | ✅ | VoodooI2C + VoodooI2CHID，GPIO中断模式 |
| 触摸屏 | ✅ | VoodooI2C + VoodooI2CHID，轮询模式 |
| USB | ✅ | USBMap |
| CPU 名称显示 | ✅ | RestrictEvents |
| 睡眠 | ❌ | 暂未实现，需要禁用睡眠 |
| FairPlay | ⚠️ | 开启 `-ngreengva` 后 Apple Music / Apple TV 失效 |
| 红点中键滚动 | ⚠️ | 无原生实现，依赖用户态工具 |
| 指纹 | ❌ | 无法模拟Touch ID |
| 麦克风 | ⚠️ | Intel智音麦克风不可用 |

### 已知限制

- **系统睡眠不可用**：TGL framebuffer 的 `hwSaveNVRAM` 在 macOS 26 上调用保留 vtable slot，睡眠入口 panic；需要 NootedGreen 侧补丁。
- **FairPlay**：`-ngreengva` 让 VA 插件走硬件密钥交换路径，但本机没有 ME，Apple Music / Apple TV 会失败；需要时去掉该参数。
- **Intel 蓝牙**：Continuity / AirDrop / Handoff 支持有限
- **小红点中键**：VoodooPS2Mouse 没有该机型的中键滚动支持，需要用户态软件（如Scrollapp）实现
- **指纹**：Goodix 传感器在 macOS 下无解，不支持 Touch ID / 指纹解锁。
- **雷电4**：可用，不支持热插拔

---


### 核显驱动

- 参考[NootedGreen](https://github.com/sgiammori/NootedGreen) 及 [Hackintosh-XPS13Plus-IrisXe-Tahoe](https://github.com/JulioNectar/Hackintosh-XPS13Plus-IrisXe-Tahoe)

---

## BIOS 设置

- Secure Boot：Disabled
- Boot Mode：UEFI Only
- Sleep State：Linux S3
- VT-d：Disabled

---

## 致谢

- [OpenCore](https://github.com/acidanthera/OpenCorePkg)、[Lilu](https://github.com/acidanthera/Lilu)、[VirtualSMC](https://github.com/acidanthera/VirtualSMC)、[WhateverGreen](https://github.com/acidanthera/WhateverGreen)
- [AppleALC](https://github.com/acidanthera/AppleALC)、[RestrictEvents](https://github.com/acidanthera/RestrictEvents)、[BrcmPatchRAM](https://github.com/acidanthera/BrcmPatchRAM)
- [VoodooI2C](https://github.com/VoodooI2C/VoodooI2C)、[VoodooPS2](https://github.com/acidanthera/VoodooPS2)
- [OpenIntelWireless](https://github.com/OpenIntelWireless)
- [NootedGreen](https://github.com/sgiammori/NootedGreen) 及 [Hackintosh-XPS13Plus-IrisXe-Tahoe](https://github.com/JulioNectar/Hackintosh-XPS13Plus-IrisXe-Tahoe)
- [SimpleHDA](https://github.com/laobamac/SimpleHDA)

第三方 kext 遵循各自许可证；Apple 二进制文件未包含在本仓库中。
