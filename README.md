# F_Replace

> [!IMPORTANT]
> **前置要求**：本模块仅支持已获取 Root 权限的 Android 设备或已开启 Root 功能的 Android 模拟器。安装前请确认 Root 权限已正常工作。

## 目录

- [快速开始](#快速开始)
  - [汉化文件更新](#汉化文件更新)
  - [国服使用说明](#国服使用说明)
- [配置文件说明](#配置文件说明-frepconfig)
- [Mod 目录结构](#mod-目录结构)
- [资源替换指南](#资源替换指南)
  - [资源格式与优先级速查](#资源格式与优先级速查)
  - [各类型替换细节说明](#各类型替换细节说明)
- [工具与扩展功能](#工具与扩展功能)
  - [剧情文本打包工具 (FGSB Tool)](#剧情文本打包工具-fgsb-tool)
  - [自定义加载图像与动画](#自定义加载图像与动画)
- [常见问题 (FAQ)](#常见问题-faq)
- [相关链接](#相关链接)

## 快速开始

### 汉化文件更新

#### 方法一：自动更新（推荐）
启动 App 进入主界面，模块会自动连接Github下载并增量更新汉化文件。可在右上角文件列表中勾选需忽略更新的文件或文件夹。

#### 方法二：手动更新
1. 下载 [Jekyell/mappings](https://github.com/Jekyell/mappings) 仓库（日服汉化文件）的完整内容。
2. 解压并放入对应路径：
   `/sdcard/Android/data/com.aniplex.fategrandorder/files/Mod/`
3. 将 `Mod` 目录及其所有子文件的权限设置为 `777`。

### 国服使用说明

国服可开启 `TextReplace`、`MasterDataReplace`、`LocalizationReplace` 与 `ScriptReplace` 进行文本替换。
- **替换词下载**：下载 [替换词文件](https://t.me/fgomod/598) 后放入 `Mod/F_Replace` 目录。
- **注意事项**：`FontReplace`（字体替换）在国服不可用，请保持关闭（`0`）。

<details>
<summary>💡 自定义替换词文件 anti_harmony.json 结构说明</summary>

```json
{
  "global": {
    "全局通用替换词": "替换词"
  },
  "localization": {
    "兽": "Beast"
  },
  "masterdata": {
    "MasterData替换词": "替换词"
  },
  "script": {
    "剧情替换词": "替换词"
  },
  "textreplace": {
    "界面原文": "界面替换文本"
  }
}
```
*提示：textreplace覆盖面最广，会捕获当前界面上的所有文本，若不确定所属分类，直接写入 `global` 块即可。*
</details>

## 配置文件说明

首次启动游戏或初始化模块时，会在 `Mod/configs/` 目录下生成 `frep.config` 配置文件（`1` 为开启，`0` 为关闭）。

```ini
# 功能开关配置 (1=开启, 0=关闭)
FadeOptimize=1            # 淡入淡出动画渲染优化
TextReplace=1             # 通用文本替换
FaceReplace=1             # 从者头像替换
FontReplace=1             # 字体替换 (国服需设为 0)
MasterDataReplace=1       # MasterData 替换
ImageReplace=1            # 从者卡面/立绘/贴图图像替换
LocalizationReplace=1     # 本地化多语言文本替换 (Mod/LocalizationJpn.txt)
ScriptReplace=1           # 剧情文本替换
LoadingOverlay=1          # 自定义加载动画显示
MasterImageReplace=1      # 御主头像与立绘替换
UnlockFPS=0               # 解锁游戏帧率

# 参数配置 (单位: DP, 边距可为负数)
MARGIN_RIGHT=45           # 加载图右边距
MARGIN_BOTTOM=14          # 加载图下边距
IMAGE_SIZE=150            # 加载图显示尺寸
LoadingFPS=24             # 加载图动画播放帧率
FPS=60                    # 目标帧率 (UnlockFPS=1 时生效)
```

## Mod 目录结构

各服务器与渠道服的 `Mod` 存放根路径：
- **日服**：`/sdcard/Android/data/com.aniplex.fategrandorder/files/Mod/`
- **B 服**：`/sdcard/Android/data/com.bilibili.fatego/files/Mod/`
- **4399 渠道服**：`/sdcard/Android/data/com.bilibili.fgo.m4399/files/Mod/`

```text
Mod/
├── configs/
│   └── frep.config         # 模块核心配置文件
├── Figure/
│   ├── CharaFigure/        # 从者立绘 (Form/ 特殊版本, Lock/ 表情锁定)
│   ├── CharaGraph/         # 从者卡面
│   ├── Faces/              # 从者头像
│   ├── NarrowFigure/       # 窄版卡面
│   ├── Status/             # 状态立绘
│   ├── Load/               # 自定义加载图像与动画
│   └── Master/             # 御主头像与立绘 (Lock/ 全局锁定, equipXXXXX/ 按礼装)
├── Models/                 # 从者战斗模型与贴图
├── Np/                     # 从者宝具资源
├── Script/                 # 单 .txt 剧情文本文件
├── ScriptBundle/           # .fgsb 剧情打包文件
└── LocalizationJpn.txt     # 本地化替换文本
```

## 资源替换指南

> [!WARNING]
> 卡面与动态卡面 (MP4/WebP) 会增加显存和内存开销。建议静态图保持原图 2 倍超分，动态卡面控制在 60 帧以内，避免引发内存溢出或闪退。

### 资源格式与优先级

| 资源类型 | 对应目录 | 支持格式 | 格式读取优先级 | 备注 |
| :--- | :--- | :--- | :--- | :--- |
| **从者卡面** (`CharaGraph`) | `Figure/CharaGraph/` | `.mp4`, `.webp`, `.png` | `mp4` → `webp` → `png` | - |
| **窄卡面** (`NarrowFigure`) | `Figure/NarrowFigure/` | `.mp4`, `.webp`, `.png` | `mp4` → `webp` → `png` | 四角切角 |
| **头像** (`Face`) | `Figure/Faces/` | `.mp4`, `.webp`, `.png` | `mp4` → `webp` → `png` | 左上+右上切角 |
| **状态立绘** (`Status`) | `Figure/Status/` | `.png`, `.astc`, `.astc.zstd` | `png` → `astc` → `astc.zstd` | 不支持动图 |
| **立绘** (`CharaFigure`) | `Figure/CharaFigure/` | `.png`, `.astc`, `.astc.zstd` | `png` → `astc` → `astc.zstd` | 不支持动图 |

### 各类型替换细节说明

1. **从者卡面、头像与立绘**
   - **通用命名规范**（适用于卡面/窄卡面/头像/状态立绘）：
     - 格式：`[从者 ID]@[突破数]`（突破数范围 `1`~`4`，灵衣从 `11` 开始）。例如：`4000100@4.png`（奥尔加玛丽满破卡面）。
   - **CharaFigure**：
     - **文件名**：以实际游戏文件名为主（不适用上述通用命名规范）。
     - **特定形态**：特殊立绘存放在 `Figure/CharaFigure/Form/[FormID]/` 子目录中。
     - **表情锁定**：放入 `Figure/CharaFigure/Lock/` 的立绘将被强制锁定为默认表情，且优先读取。


2. **御主头像与立绘**
   - **全局替换**（最高优先级）：`Figure/Master/Lock/`
   - **特定礼装替换**：`Figure/Master/[礼装 ID]/`（例如 `equip00441/`）
   - **文件要求**：头像使用 `face.mp4` 或 `face.png`（`mp4` 优先）格式；主立绘为 `figure.png`；蒙版遮罩为 `figure_a.png`。

3. **模型与宝具资源**
   - **战斗模型**：放入 `Models/` 目录，加载战斗时自动替换贴图或模型。
   - **宝具动画**：放入 `Np/` 目录，自动替换对应宝具文件。

## 工具与扩展功能

### 剧情文本打包工具 (FGSB Tool)

工具下载：[FGSB Tool Releases](https://github.com/Jekyell/F_Replace/releases/tag/tools)
- **封包**（目录打包为 `.fgsb`）：`./fgsb_tool.exe pack ./剧情文件夹 20250209.fgsb`
- **解包**（`.fgsb` 解压为目录）：`./fgsb_tool.exe unpack 20250209.fgsb ./剧情文件夹`
- **读取逻辑**：打包文件存放在 `Mod/ScriptBundle/`。模块会优先读取 `Mod/Script/` 下的 `.txt` 解压文件。推荐直接使用 App 自动拉取，非必要情况下无需使用fgsb文件。

### 自定义加载图像与动画

样例参考与下载：[Load 资源样例](https://github.com/Jekyell/F_Replace/releases/tag/load)
1. 创建 `Mod/Figure/Load/` 目录。
2. 放入精灵图 `animation.png` 及配置文件 `animation.json`。
3. 在 `frep.config` 中开启 `LoadingOverlay=1` 并调整位置与帧率参数。

## 常见问题 (FAQ)

<details>
<summary><b>Q: MuMu 模拟器国际版启动游戏卡死 / 闪退怎么办？</b></summary>

<br />

- **问题原因**：MuMu 模拟器自带的系统翻译插件 (`com.mumu.acc`) 与本模块发生冲突。
- **解决方法**：
  1. 打开模拟器中的 **MT 管理器**。
  2. 打开侧边栏的 **终端模拟器**。
  3. 输入以下命令禁用该插件：
     ```bash
     su
     pm disable com.mumu.acc
     ```
  4. 彻底重启模拟器（需在 Windows 系统托盘右键退出 MuMu 模拟器主程序后重新打开）。
</details>

## 相关链接

- [Telegram 频道](https://t.me/fgomod)
- [全从者超清卡面图集](https://t.me/fgomod_1)
- [模型修改参考 (models.md)](https://github.com/Jekyell/F_Replace/blob/main/models.md)
