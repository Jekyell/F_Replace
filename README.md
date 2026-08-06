# F_Replace

> [!IMPORTANT]
> **需要 Root 权限**
> 
> 本模块仅支持已获取 Root 权限的 Android 设备或已开启 Root 功能的 Android 模拟器。
> 未 Root 的设备无法使用本模块。安装前请确认 Root 权限已正常工作。

---

## 📖 目录 (Table of Contents)

- [前置要求](#需要-root-权限)
- [快速上手](#-快速上手)
  - [汉化文件更新](#汉化文件更新)
  - [国服使用说明](#国服使用说明)
- [配置文件说明 (frep.config)](#-配置文件说明-frepconfig)
- [Mod 目录结构](#-mod-目录结构)
- [资源替换指南](#-资源替换指南)
  - [从者卡面与头像](#1-从者卡面与头像)
  - [格式与优先级支持表](#2-格式与优先级支持表)
  - [御主头像与立绘 (Master)](#3-御主头像与立绘-master)
  - [表情锁定、模型与宝具替换](#4-表情锁定模型与宝具替换)
  - [剧情文本配套工具 (FGSB Tool)](#5-剧情文本配套工具-fgsb-tool)
  - [自定义加载图像与动画 (Load)](#6-自定义加载图像与动画-load)
- [常见问题与故障排查](#-常见问题与故障排查-faq)
- [相关链接与社区](#-相关链接与社区)

---

## 🚀 快速上手

### 汉化文件更新

#### 方法一：自动更新（推荐）
点击模块图标进入软件界面，会自动连接网络下载并增量更新汉化文件。可在右上角文件列表中勾选忽略更新哪些文件/文件夹。

#### 方法二：手动更新
1. 下载 [Jekyell/mappings](https://github.com/Jekyell/mappings) 仓库（日服汉化文件）中的所有内容。
2. 将解压后的内容放入以下路径：
   `/sdcard/Android/data/com.aniplex.fategrandorder/files/Mod/`
3. 将 `Mod` 文件夹及其所有子文件夹/子文件的权限设置为 `777` (全读写执行权限)。

### 国服使用说明

在国服环境使用时，可以在配置文件中针对性开关各功能：
- **支持项**：可开启 `TextReplace`（文本替换）、`MasterDataReplace`（数据库替换）、`LocalizationReplace`（本地化替换）、`ScriptReplace`（剧情脚本替换）。
  - 替换词文件获取：[Telegram 频道消息](https://t.me/fgomod/598)
- **不可用项**：`FontReplace`（字体替换）在国服下不可用，请保持关闭（`0`）。

---

## ⚙️ 配置文件说明 (`frep.config`)

游戏首次启动或模块初始化时，会在 `Mod/configs/` 目录下自动生成配置文件 `frep.config`。可在该文件中将各功能设置为开启 (`1`) 或关闭 (`0`)，或调整数值参数。

### 完整配置选项清单

```ini
# F_Replace 模块开关配置 (1=开启, 0=关闭)
FadeOptimize=1            # 淡入淡出动画渲染优化开关
TextReplace=1             # 界面及战斗通用文本替换总开关
FaceReplace=1             # 从者头像替换总开关
FontReplace=1             # 字体替换开关 (国服请设为0)
MasterDataReplace=1       # MasterData 数据替换总开关
ImageReplace=1            # 从者卡面/立绘/贴图等图像替换总开关
LocalizationReplace=1     # 本地化多语言文本替换开关 (读取 Mod/LocalizationJpn.txt)
ScriptReplace=1           # 剧情脚本文本替换总开关
LoadingOverlay=1          # 自定义加载动画显示开关
MasterImageReplace=1      # 御主头像与立绘替换总开关
UnlockFPS=0               # 解锁游戏帧率开关 (0=关闭, 1=开启)

# 自定义加载图/动画位置与参数配置 (单位: DP, 边距可为负数)
MARGIN_RIGHT=45           # 图像右边距
MARGIN_BOTTOM=14          # 图像下边距
IMAGE_SIZE=150            # 图像显示尺寸
LoadingFPS=24             # 加载图动画播放帧率 (FPS)
FPS=60                    # 游戏帧率解锁目标值 (UnlockFPS=1 时生效)
```

---

## 📂 Mod 目录结构

> [!NOTE]
> **不同服务器 / 渠道服的 Mod 存放根路径**：
> - **日服**：`/sdcard/Android/data/com.aniplex.fategrandorder/files/Mod/`
> - **B 服**：`/sdcard/Android/data/com.bilibili.fatego/files/Mod/`
> - **4399 渠道服**：`/sdcard/Android/data/com.bilibili.fgo.m4399/files/Mod/`

完整的 `Mod/` 目录层级架构如下：

```text
Mod/
├── configs/
│   └── frep.config         # 模块核心配置文件
├── Figure/
│   ├── CharaFigure/        # 从者战斗立绘
│   │   ├── Form/           # 特定形态立绘目录 (例如 Form/2/)
│   │   └── Lock/           # 表情锁定立绘目录
│   ├── CharaGraph/         # 从者大卡面
│   ├── Faces/              # 从者头像 (256x256 / 512x512)
│   ├── NarrowFigure/       # 窄版/半身卡面
│   ├── Status/             # 状态栏/队伍界面立绘
│   ├── Load/               # 自定义加载图像/动画 (animation.png + animation.json)
│   └── Master/             # 御主头像与立绘
│       ├── Lock/           # 全局御主替换 (最高优先级)
│       └── equipXXXXX/     # 按特定魔术礼装 ID 替换 (例如 equip00441/)
├── Models/                 # 从者战斗模型与贴图
├── Np/                     # 从者宝具资源
├── Script/                 # 单 .txt 剧情文本文件
├── ScriptBundle/           # 打包好的 .fgsb 剧情打包文件
└── LocalizationJpn.txt     # 本地化替换文本文件
```

---

## 🖼️ 资源替换指南

> [!WARNING]
> **实验性功能提示**
> 卡面与动态卡面 (MP4/WebP) 替换功能会增加显存与内存开销。
> - 建议静态图像分辨率采用原图的 2 倍左右超分。
> - 动态卡面建议帧率保持在 30 帧以内，分辨率不宜过高，避免造成内存溢出或游戏闪退。

### 1. 从者卡面与头像

- **支持的类型**：`Face`、`NarrowFigure`、`CharaGraph`、`CharaFigure`（不支持动图）、`Status`（不支持动图）。
- **命名规范**：`[从者ID]@[突破数]`
  - **突破数范围**：`1` ~ `4`（灵衣从 `11` 开始）。
  - **卡面示例**：`4000100@4` 表示奥尔加玛丽满破卡面。
  - **头像单图示例**：在 `Mod/Figure/Faces/` 下放置 `204300@1.png` 替换芭万希第一个头像。
- **Form 目录**：部分特定形态立绘存放在 `Form` 子目录下（如 `Mod/Figure/CharaFigure/Form/2/`）。

### 2. 格式与优先级支持表

不同类型的资源所支持的文件格式及读取优先级如下：

| 资源类型 | 目录名称 | 支持的文件格式 | 格式读取优先级 | 备注 |
| :--- | :--- | :--- | :--- | :--- |
| `CharaGraph` | `Figure/CharaGraph/` | `.mp4`, `.webp`, `.png` | `mp4` → `webp` → `png` | 支持动态卡面 |
| `NarrowFigure` | `Figure/NarrowFigure/` | `.mp4`, `.webp`, `.png` | `mp4` → `webp` → `png` | 切角为四个角 |
| `Face` | `Figure/Faces/` | `.mp4`, `.webp`, `.png` | `mp4` → `webp` → `png` | 切角为左上 + 右上 |
| `Status` | `Figure/Status/` | `.webp`, `.png`, `.astc`, `.astc.zstd` |  `png` → `astc` → `astc.zstd` | - |
| `CharaFigure` | `Figure/CharaFigure/` | `.png`, `.astc`, `.astc.zstd` | `png` → `astc` → `astc.zstd` | - |

### 3. 御主头像与立绘

- **存放路径**：
  - **全局替换（最高优先级）**：`Mod/Figure/Master/Lock/`
  - **特定礼装替换**：`Mod/Figure/Master/[礼装ID]/`（例如 `Mod/Figure/Master/equip00441/`）
- **文件命名与格式**：
  - **头像**：`face.mp4` 或 `face.png`（`face.mp4` 优先级高于 `face.png`）。
  - **立绘**：`figure.png`（主立绘）。
  - **立绘蒙版**：`figure_a.png`（Alpha 遮罩图）。

### 4. 表情锁定、模型与宝具替换

- **从者立绘表情锁定**：
  - **路径**：`Mod/Figure/CharaFigure/Lock/`
  - **说明**：放入此目录的立绘，其表情会被强制锁定为默认表情，且该目录下的立绘会被优先读取替换。
- **从者模型/贴图替换**：
  - **路径**：`Mod/Models/`
  - **说明**：放入修改后的贴图文件或完整 Model 资源，游戏加载战斗模型时将自动替换。
- **从者宝具替换**：
  - **路径**：`Mod/Np/`
  - **说明**：放入修改后的宝具文件，游戏加载宝具资源时将自动替换。

### 5. 剧情文本配套工具 (FGSB Tool)

工具下载地址：[FGSB Tool Release](https://github.com/Jekyell/F_Replace/releases/tag/tools)

- **封包（文件夹打包为 `.fgsb`）**：
  ```bash
  ./fgsb_tool.exe pack ./剧情文件夹 20250209.fgsb
  ```
- **解包（`.fgsb` 解压为文件夹）**：
  ```bash
  ./fgsb_tool.exe unpack 20250209.fgsb ./剧情文件夹
  ```
- **读取逻辑**：打包后的 `.fgsb` 放入 `Mod/ScriptBundle/` 目录。模块会**优先**读取 `Mod/Script/` 目录下的 `.txt` 文件。推荐使用 App 自动拉取即可，非特殊情况没有使用fgsb文件的必要。

### 6. 自定义加载图像与动画
- **制作参考与样例下载**：[自定义加载图制作参考](https://github.com/Jekyell/F_Replace/releases/tag/load)
1. **配置步骤**：
   - 创建 `Mod/Figure/Load/` 目录。
   - 放入精灵图 `animation.png` 及配置文件 `animation.json`（可参考 [Load Release](https://github.com/Jekyell/F_Replace/releases/tag/load) 样例）。
2. **位置与帧率调整**：在 `frep.config` 中调整 `LoadingOverlay=1`、`MARGIN_RIGHT`、`MARGIN_BOTTOM`、`IMAGE_SIZE` 和 `LoadingFPS`。位置调整支持负数。

---

## ❓ 常见问题与故障排查 (FAQ)

### Q: MuMu 模拟器国际版启动游戏卡死/闪退怎么办？

- **问题原因**：MuMu 模拟器国际版自带的系统翻译插件 (`com.mumu.acc`) 与本模块发生冲突。
- **解决方法**：
  1. 打开模拟器中的 **MT 管理器**。
  2. 打开侧边栏的 **终端模拟器**。
  3. 依次输入以下命令禁用该翻译插件：
     ```bash
     su
     pm disable com.mumu.acc
     ```
  4. 彻底重启模拟器（**注意**：需在电脑右下角的 Windows 系统托盘中右键退出 MuMu 模拟器主程序后重新打开）。

---

## 🔗 相关链接

- 📢 **tg 频道**：https://t.me/fgomod
- 🖼️ **全从者超清卡面图集**：https://t.me/fgomod_1
- 🛠️ **模型修改教程**：[models.md](https://github.com/Jekyell/F_Replace/blob/main/models.md)
