# X79-E5-2689-RX580-4G-OpenCore

<div align="center">

**🌐 Language / 语言：** [English](#english) | [中文](#chinese)

</div>

---

<a id="english"></a>
## Hackintosh OpenCore EFI — X79 / E5-2689 / RX580 4G

An OpenCore EFI configuration for running macOS on an X79 motherboard with an Intel Xeon E5-2689 CPU and AMD RX580 4GB GPU.

### Hardware Compatibility

| Component | Model | Status |
|-----------|-------|--------|
| Motherboard | X79 (Generic / Huanan) | ✅ |
| CPU | Intel Xeon E5-2689 (v1, C2 stepping) | ✅ |
| GPU | AMD RX580 4GB | ✅ Native driver |
| Wi-Fi | Requires compatible card (BCM94352Z recommended) | ✅ with patch |
| Audio | Not fully DSDT-patched — USB audio adapter recommended | ⚠️ |
| Turbo Boost | Working | ✅ |

### macOS Compatibility

| macOS | Status |
|-------|--------|
| Big Sur (11) | ✅ Recommended |
| Monterey (12) | ⚠️ Not recommended for this CPU/board |
| Ventura+ | ❌ Not tested |

> **Recommendation:** Use **macOS Big Sur**. Monterey introduces changes that can cause instability on X79 platforms with this stepping.

### Deployment / Installation

#### Prerequisites

- A working Windows or macOS machine to prepare the installer
- USB drive (16 GB or larger)
- The EFI from this repository

#### Step 1 — Download macOS

On a Mac via App Store, or using [gibMacOS](https://github.com/corpnewt/gibMacOS) on Windows:

```bash
git clone https://github.com/corpnewt/gibMacOS
cd gibMacOS
python gibMacOS.command   # Select macOS Big Sur (11.x)
```

#### Step 2 — Create Bootable USB

On macOS:

```bash
sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia \
  --volume /Volumes/USB --nointeraction
```

On Windows: use [balenaEtcher](https://etcher.balena.io/) with the downloaded `.dmg`.

#### Step 3 — Copy EFI

1. Mount the USB EFI partition using [MountEFI](https://github.com/corpnewt/MountEFI) or `diskutil`
2. Delete the existing EFI folder on the USB
3. Copy the `EFI` folder from this repository to the USB EFI partition

#### Step 4 — BIOS Settings

- **Disable**: Secure Boot, CSM, Serial Port
- **Enable**: EHCI/XHCI Hand-Off, Above 4G Decoding
- **Memory**: Enable XMP if available
- **Boot**: Set USB as first boot device

#### Step 5 — Install macOS

1. Boot from USB → select **Install macOS Big Sur** in OpenCore picker
2. Format target drive as APFS (use Disk Utility)
3. Complete installation (system reboots 2–3 times; keep USB plugged in)

#### Step 6 — Post-Install EFI Migration

1. Mount the installed drive's EFI partition
2. Copy the EFI folder from USB to the internal drive's EFI partition
3. You can now boot without the USB

### Post-Install Notes

- **Audio**: DSDT is not fully customized — if built-in audio doesn't work, a USB audio adapter (~¥6 on Taobao) is a reliable workaround
- **Wi-Fi**: Patch for compatible wireless cards is included
- **iServices** (iMessage, FaceTime): Generate a new SMBIOS serial in [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) — do not use the serials in this EFI
- **Video Streaming**: Supports Youku frame interpolation (帧享) — RX580 is natively supported

### Tools

- [ProperTree](https://github.com/corpnewt/ProperTree) — edit `config.plist`
- [Hackintool](https://github.com/benbaker76/Hackintool) — USB mapping and patch verification
- [MountEFI](https://github.com/corpnewt/MountEFI) — mount EFI partitions on macOS

---

<a id="chinese"></a>
## 黑苹果 OpenCore EFI — X79 / E5-2689 / RX580 4G

适用于 X79 主板 + Intel Xeon E5-2689 + AMD RX580 4GB 的 OpenCore 引导配置。

### 硬件兼容性

| 配件 | 型号 | 状态 |
|------|------|------|
| 主板 | X79（华南/通用） | ✅ |
| CPU | Intel Xeon E5-2689（v1，C2 步进） | ✅ |
| 显卡 | AMD RX580 4GB | ✅ 免驱 |
| 无线网卡 | 需兼容网卡（推荐 BCM94352Z） | ✅ 含补丁 |
| 声卡 | DSDT 未完整定制，建议使用 USB 声卡 | ⚠️ |
| 睿频 | 正常工作 | ✅ |

### macOS 兼容性

| macOS | 状态 |
|-------|------|
| Big Sur (11) | ✅ 推荐 |
| Monterey (12) | ⚠️ 不推荐 |
| Ventura+ | ❌ 未测试 |

> **建议使用 macOS Big Sur。** Monterey 在此平台可能出现稳定性问题。

### 部署 / 安装方法

#### 前提条件

- 一台可用的 Windows 或 macOS 设备
- 16GB 或更大的 U 盘
- 本仓库的 EFI 文件

#### 第一步 — 下载 macOS

Mac 上从 App Store 下载，或使用 [gibMacOS](https://github.com/corpnewt/gibMacOS) 在 Windows 下载 Big Sur。

#### 第二步 — 制作启动 U 盘

```bash
# macOS 终端：
sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia \
  --volume /Volumes/USB --nointeraction
```

Windows 用户使用 [balenaEtcher](https://etcher.balena.io/) 写入镜像。

#### 第三步 — 复制 EFI

1. 用 [MountEFI](https://github.com/corpnewt/MountEFI) 挂载 U 盘 EFI 分区
2. 删除 U 盘上现有的 EFI 文件夹
3. 将本仓库的 `EFI` 文件夹复制到 U 盘 EFI 分区

#### 第四步 — BIOS 设置

- **关闭**：Secure Boot、CSM、串口
- **开启**：EHCI/XHCI Hand-Off、Above 4G Decoding
- **内存**：有 XMP 开启 XMP
- **启动**：设置 U 盘为第一启动项

#### 第五步 — 安装 macOS

1. 从 U 盘启动 → OpenCore 引导界面选择 **Install macOS Big Sur**
2. 用磁盘工具将目标硬盘格式化为 APFS
3. 完成安装（全程保持 U 盘插入，会重启 2-3 次）

#### 第六步 — 迁移引导

1. 挂载已安装系统硬盘的 EFI 分区
2. 将 U 盘的 EFI 文件夹复制到硬盘 EFI 分区
3. 之后可直接从硬盘启动

### 安装后注意事项

- **声卡**：DSDT 非完整定制，声卡无法驱动时购买 USB 声卡转接器（约 6 元）
- **无线网卡**：已加入无线网卡集合补丁
- **iServices**：用 [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) 生成新序列号，勿使用本 EFI 中的序列号
- **视频流媒体**：支持优酷帧享，RX580 免驱原生支持

### 常用工具

- [ProperTree](https://github.com/corpnewt/ProperTree) — 编辑 `config.plist`
- [Hackintool](https://github.com/benbaker76/Hackintool) — USB 定制和补丁校验
- [MountEFI](https://github.com/corpnewt/MountEFI) — 挂载 EFI 分区
