# F_Replace（FGO Replace）


## 目录

- [快速开始](#快速开始)
  - [App 内配置与管理](#app-内配置与管理)
  - [汉化文件更新（日服）](#汉化文件更新日服)
  - [国服使用说明](#国服使用说明)
- [配置文件说明 (`frep.config`)](#配置文件说明-frepconfig)
- [Mod 目录结构](#mod-目录结构)
- [资源替换指南](#资源替换指南)
  - [资源格式与优先级](#资源格式与优先级)
  - [各类型替换细节说明](#各类型替换细节说明)
- [特色功能指南](#特色功能指南)
  - [自定义加载图像与交互编辑](#自定义加载图像与交互编辑)
  - [超分图集按需自动下载](#超分图集按需自动下载)
- [工具与扩展接口](#工具与扩展接口)
  - [剧情文本打包工具 (FGSB Tool)](#剧情文本打包工具-fgsb-tool)
  - [剧情文本语音接口 (ScriptVoice)](#剧情文本语音接口-scriptvoice)
- [常见问题 (FAQ)](#常见问题-faq)
- [相关链接](#相关链接)

---

## 快速开始

### App 内配置与管理

安装模块并在 LSPosed 中勾选对应区服的 FGO 后，打开模块 App 即可通过底部导航栏完成全部日常操作：

- **更新页**：一键检查并增量拉取汉化仓库，支持开启「自动更新」「完成后自动启动游戏」，点击编辑按钮可可视化勾选同步忽略列表（对应 `Mod/.gitignore`）。
- **配置页**：可视化管理全部功能开关与数值参数，自动读写 `Mod/configs/frep.config`。
  - 标注 **「即时」** 的开关支持游戏运行中热切换生效，无需重启游戏。
  - 标注 **「需重启」** 的开关修改后需重启游戏生效。

> [!IMPORTANT]
> **无 Root（LSPatch）用户必看**：由于没有 Root 权限，模块 App 无法直接访问游戏私有目录 `/sdcard/Android/data/<包名>/files/Mod/`，因此**无法通过 App 界面修改配置或更新汉化**。
> - **修改配置**：请手动编辑对应区服目录下的 `Mod/configs/frep.config` 文件（下文的「即时」项在手动保存文件后同样会自动热生效）。
> - **日服更新汉化**：推荐在 `frep.config` 中手动设置 `AutoMappingDownload=1`，由游戏进程自身在启动时自动更新。

### 汉化文件更新（日服）

#### 方法一：游戏启动自动更新（推荐）
直接打开游戏即可自动检查并更新 mapping 汉化仓库。

- **开启方法**：在 App 配置页开启「游戏内自动更新汉化仓库」，或在 `Mod/configs/frep.config` 中设置 `AutoMappingDownload=1`（默认 `0` 关闭）。
- **忽略文件配置**：如需忽略某些文件或文件夹，可在 App 更新页勾选，或手动创建 `Mod/.gitignore`（语法与 Git `.gitignore` 完全一致）。
- **首次启用说明**：首次启用会下载覆盖一次全量包，如有自定义保留文件请在启动游戏前提前配置好 `.gitignore`。

#### 方法二：App 内自动更新
启动 App 进入主界面，模块会自动连接 GitHub 下载并增量更新汉化文件，可在同步过滤编辑页勾选需忽略更新的文件或文件夹。

#### 方法三：手动更新
1. 下载 [Jekyell/mappings](https://github.com/Jekyell/mappings) 仓库（日服汉化文件）的完整内容。
2. 解压并放入对应路径：
   `/sdcard/Android/data/com.aniplex.fategrandorder/files/Mod/`
3. 将 `Mod` 目录及其所有子文件的权限设置为 `777`。

### 国服使用说明

国服版本（`FRep_CN`）无需下载日服 mapping 汉化仓库。开启 `TextReplace`、`MasterDataReplace`、`LocalizationReplace` 与 `ScriptReplace` 即可进行反和谐或自定义文本替换；其余图像替换、3D 全息闪卡、动画跳过、帧率解锁、底层性能优化与超分自动下载等功能与日服一致。

- **替换词下载**：下载 [替换词文件](https://t.me/fgomod/924) 后放入 `Mod/F_Replace/` 目录。
- **区服差异**：`FontReplace`（字体替换）、`UIReplace`（UI 图集单精灵替换）、`AutoMappingDownload` 与 `AutoCommandCardNpCardTextDownload` 为日服专属功能，国服无需配置。

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
*提示：`textreplace` 覆盖面最广，会捕获当前界面上的所有文本；若不确定所属分类，直接写入 `global` 块即可。*
</details>

---

## 配置文件说明 (`frep.config`)

首次启动游戏或打开 App 配置页时，会在 `Mod/configs/` 目录下自动生成 `frep.config` 配置文件（`1` 为开启，`0` 为关闭）。Root（LSPosed）用户推荐直接在 App 配置页可视化修改；**无 Root（LSPatch）用户请手动编辑此文件**：

```ini
# ==================== 1. 文本与资源替换 ====================
TextReplace=1                         # 通用文本替换 [需重启]
LocalizationReplace=1                 # Localization 文本替换 (Mod/LocalizationJpn.txt) [需重启]
ScriptReplace=1                       # 剧情文本替换 [需重启]
ScriptVoice=1                         # 剧情文本语音播放 [即时]
MasterDataReplace=1                   # MasterData 数据替换 [需重启]
FontReplace=1                         # 自定义字体替换 (仅日服) [需重启]
UIReplace=1                           # UI 界面单精灵替换 (仅日服) [需重启]

# ==================== 2. 图像与立绘替换 ====================
ImageReplace=1                        # 从者卡面/立绘/贴图图像替换总开关 [需重启]
FaceReplace=1                         # 从者头像替换 [需重启]
MasterImageReplace=1                  # 御主头像与立绘替换 [需重启]
HoloCharaGraph=0                      # 3D 全息闪卡 - 标准卡面 (CharaGraph) [即时]
HoloFace=0                            # 3D 全息闪卡 - 从者头像 (Face) [即时]
HoloNarrowFigure=0                    # 3D 全息闪卡 - 编队窄条 (NarrowFigure) [即时]

# ==================== 3. 动画跳过与性能 ====================
FadeOptimize=1                        # 淡入淡出转场优化 [即时]
AssetLoadOptimize=1                   # 资源并发加载优化 [即时]
RuntimePerfOptimize=1                 # 底层运行性能优化 [即时]
SkipServantCombine=0                  # 跳过从者强化动画 [即时]
SkipLimitUpCombine=0                  # 跳过灵基再临动画 [即时]
SkipSkillCombine=0                    # 跳过技能强化动画 [即时]
SkipTdCombine=0                       # 跳过宝具强化动画 [即时]
SkipSvtEqCombine=0                    # 跳过概念礼装强化动画 [即时]
SkipLvExceedCombine=0                 # 跳过圣杯转临动画 [即时]
SkipFriendshipExceedCombine=0         # 跳过牵绊上限提升动画 [即时]
SkipCostumeCombine=0                  # 跳过灵衣开放动画 [即时]
SkipItemGetEffect=0                   # 跳过稀有道具获取动画 [即时]
SkipNoblePhantasm=0                   # 跳过宝具动画 [即时]
UnlockFPS=0                           # 解锁游戏帧率上限 [需重启]
FPS=60                                # 目标帧率 (UnlockFPS=1 时生效，范围 15~240) [需重启]

# ==================== 4. 按需自动下载 ====================
AutoMappingDownload=0                 # 游戏启动时自动更新 mapping 汉化仓库 (仅日服) [需重启]
AutoCharaFigureDownload=0             # 从者立绘超分自动下载 (CharaFigure) [即时]
AutoCharaGraphDownload=0              # 从者卡面超分自动下载 (CharaGraph) [即时]
AutoNarrowFigureDownload=0            # 窄条头像超分自动下载 (NarrowFigure) [即时]
AutoFaceDownload=0                    # 从者头像超分自动下载 (Face) [即时]
AutoStatusDownload=0                  # 状态图超分自动下载 (Status) [即时]
AutoCommandCardNpCardTextDownload=0   # 宝具卡中文文本图自动下载 (仅日服) [即时]
AutoCommandCardSvtImgDownload=0       # 指令卡从者贴图自动下载 (仅调试) [即时]

# ==================== 5. 加载图与数值参数 ====================
LoadingOverlay=1                      # 自定义加载动画显示 [即时]
LoadingOverlayEdit=0                  # 加载图交互编辑模式 (长按加载图拖拽缩放) [即时]
MARGIN_RIGHT=45                       # 加载图右边距 (单位: DP，可为负数) [即时]
MARGIN_BOTTOM=14                      # 加载图下边距 (单位: DP，可为负数) [即时]
IMAGE_SIZE=150                        # 加载图显示尺寸 (单位: DP，范围 32~1000) [即时]
LoadingFPS=24                         # 加载图动画播放帧率 (单位: FPS，范围 1~120) [需重启]
```

---

## Mod 目录结构

各服务器与渠道服的 `Mod` 存放根路径：

- **日服**：`/sdcard/Android/data/com.aniplex.fategrandorder/files/Mod/`
- **B 服**：`/sdcard/Android/data/com.bilibili.fatego/files/Mod/`
- **4399 渠道服**：`/sdcard/Android/data/com.bilibili.fgo.m4399/files/Mod/`
- **小米**：`/sdcard/Android/data/com.bilibili.fgo.mi/files/Mod/`
- **九游**：`/sdcard/Android/data/com.bilibili.fgo.uc/files/Mod/`
- **华为**：`/sdcard/Android/data/com.bilibili.fgo.huawei/files/Mod/`

```text
Mod/
├── .gitignore                  # 自定义忽略更新文件规则 (语法与 Git 一致，可选)
├── configs/
│   └── frep.config             # 模块核心配置文件
├── F_Replace/                  # 文本/数据替换词库与国服 anti_harmony.json
├── Font                        # 自定义字体 AssetBundle 文件 (仅日服)
├── UI/                         # UI 图集单精灵替换 PNG 目录 (仅日服)
│   ├── <AtlasName>/            # 指定图集名匹配 (优先)
│   └── <SpriteName>.png        # 全局精灵名兜底匹配
├── Figure/
│   ├── CharaFigure/            # 从者立绘 (Form/ 特殊形态, Lock/ 表情锁定)
│   │   └── AutoDownload/       # 自动下载超分图集缓存
│   ├── CharaGraph/             # 从者卡面
│   │   └── AutoDownload/       # 自动下载超分图集缓存
│   ├── Faces/                  # 从者头像
│   │   └── AutoDownload/       # 自动下载超分图集缓存
│   ├── NarrowFigure/           # 窄版卡面
│   │   └── AutoDownload/       # 自动下载超分图集缓存
│   ├── Status/                 # 状态详情立绘
│   │   └── AutoDownload/       # 自动下载超分图集缓存
│   ├── CommandCard/            # 指令卡通用色卡文字图片
│   │   ├── NpText/             # 宝具卡文本图片替换
│   │   │   └── AutoDownload/   # 自动下载中文宝具卡文本缓存
│   │   └── SvtImg/             # 指令卡人物贴图替换
│   │       └── AutoDownload/   # 自动下载指令卡从者贴图缓存 (仅调试)
│   ├── Load/                   # 自定义加载图像与动画 (animation.png + animation.json)
│   └── Master/                 # 御主头像与立绘 (Lock/ 全局锁定, equipXXXXX/ 按礼装)
├── Models/                     # 从者战斗模型与贴图
│   ├── Motion/                 # 动作包替换
│   ├── Camera/                 # 镜头包替换
│   ├── Effect/Weapon/          # 武器特效替换
│   └── Audio/                  # 战斗语音/音频替换
├── Np/                         # 从者宝具模型与贴图资源
│   └── Audio/                  # 宝具音频替换
├── Script/                     # 单 .txt 剧情文本文件
├── ScriptBundle/               # .fgsb 剧情打包文件
├── ScriptVoice/                # 剧情文本自定义语音
└── LocalizationJpn.txt         # 本地化替换文本
```

---

## 资源替换指南

> [!WARNING]
> 高清超分与动态卡面（MP4 / WebP）会增加内存与显存开销。建议静态图保持原图 2 倍超分，动态卡面控制在 60 帧以内，避免低内存设备出现 OOM 或闪退。
>
> 动图首次加载会在后台生成缓存，期间偶有短暂卡顿属于正常现象，缓存生成完成后即可流畅复用。

### 资源格式与优先级

| 资源类型 | 对应目录 | 支持格式 | 格式读取优先级 | 备注 |
| :--- | :--- | :--- | :--- | :--- |
| **从者卡面** (`CharaGraph`) | `Figure/CharaGraph/` | `.mp4`, `.webp`, `.png` | `mp4` → `webp` → `png` | 支持动图与 3D 闪卡 |
| **窄卡面** (`NarrowFigure`) | `Figure/NarrowFigure/` | `.mp4`, `.webp`, `.png` | `mp4` → `webp` → `png` | 四角切角，支持 3D 闪卡 |
| **头像** (`Face`) | `Figure/Faces/` | `.mp4`, `.webp`, `.png` | `mp4` → `webp` → `png` | 左上+右上切角，支持 3D 闪卡 |
| **状态立绘** (`Status`) | `Figure/Status/` | `.png`, `.astc`, `.astc.zstd` | `png` → `astc` → `astc.zstd` | 不支持动图 |
| **剧情立绘** (`CharaFigure`) | `Figure/CharaFigure/` | `.png`, `.astc`, `.astc.zstd` | `png` → `astc` → `astc.zstd` | 不支持动图 |
| **UI 单精灵** (`UIReplace`) | `UI/` | `.png` | `<图集名>/<精灵名>.png` → `<精灵名>.png` | 仅日服，自动动态合页 |

### 各类型替换细节说明

1. **从者卡面、窄卡面、头像与状态立绘**
   - **通用命名规范**：`[从者 ID]@[突破数]`（突破数通常为 `1`~`4`，部分灵衣从 `11` 开始，具体以实际 ID 为准；部分灵衣的从者 ID 可能会变化）。例如：`4000100@4.png`（奥尔加玛丽满破卡面）。
   - **不确定命名时**：可临时开启对应的 `Auto*Download` 开关，待自动下载后查看 `AutoDownload/` 文件夹里的实际文件名。
   - **剧情立绘 (`CharaFigure`)**：
     - **文件名**：以游戏内实际资源名为准（不适用上述 `ID@突破数` 命名规范）。
     - **特定形态**：存放在 `Figure/CharaFigure/Form/[FormID]/` 子目录中。
     - **表情锁定**：放入 `Figure/CharaFigure/Lock/` 的立绘将强制锁定为默认表情，且优先级最高。

2. **御主头像与立绘**
   - **全局替换**（最高优先级）：`Figure/Master/Lock/`
   - **特定礼装替换**：`Figure/Master/[礼装 ID]/`（例如 `equip00441/`）
   - **文件要求**：头像使用 `face.mp4` 或 `face.png`（`mp4` 优先）；主立绘为 `figure.png`；蒙版遮罩为 `figure_a.png`。

3. **UI 界面单精灵替换（仅日服）**
   - 开启 `UIReplace=1` 后，将替换图片放入 `Mod/UI/` 即可在不重打整张图集的情况下单独替换任意 NGUI 按钮或图标精灵。
   - 放在 `Mod/UI/<原图集名>/<精灵名>.png` 仅对该图集生效（推荐，避免同名误替换）；直接放在 `Mod/UI/<精灵名>.png` 则对所有图集兜底生效。

4. **战斗模型、特效与宝具资源**
   - **战斗模型 / 贴图 / 动作 / 武器特效 / 音频**：放入 `Models/` 及对应子目录（`Motion/`、`Camera/`、`Effect/Weapon/`、`Audio/`），进入战斗加载时自动替换。详见 [模型修改参考 (models.md)](https://github.com/Jekyell/F_Replace/blob/main/models.md)。
   - **宝具资源与音频**：放入 `Np/` 及 `Np/Audio/` 目录，自动替换对应宝具文件。

5. **指令卡色卡文字图片替换**
   - 将色卡文字图片放入 `Mod/Figure/CommandCard/`。
   - 示例文件：[Telegram 示例](https://t.me/fgomod/830)

6. **指令卡宝具文本与人物贴图替换**
   - **放置路径**：`Mod/Figure/CommandCard/NpText/`（宝具文本）或 `Mod/Figure/CommandCard/SvtImg/`（人物贴图）。
   - **自动下载开关**：
     - `AutoCommandCardNpCardTextDownload=1`：自动下载中文宝具卡文本图至 `CommandCard/NpText/AutoDownload/` 并自动替换（国服无需开启）。
     - `AutoCommandCardSvtImgDownload=1`：仅用于拉取原图至 `CommandCard/SvtImg/AutoDownload/` 方便核对从者 ID 变体命名，不参与替换。
   - **宝具文本命名规范**：
     - **A · 标准**：运行时贴图名 `card_servant_np` → `NpText/<svtId>.png`
     - **B · 灵基覆写**：运行时贴图名纯数字（如 `405910`）→ `NpText/<svtId>@405910.png`
     - **C · 变身**：运行时贴图名 `card_servant_np_304802` → `NpText/<svtId>@304802.png`
     - **D · 序号**：运行时贴图名 `card_servant_np4` → `NpText/<svtId>@4.png`
   - **示例**：
     - 标准：AtlasAcademy 链接 `.../Servants/Commands/4000100/card_servant_np.png` → 命名为 `4000100.png`。
     - 灵基覆写：水摩根第二宝具 `.../Servants/Commands/505300/505310.png` → 命名为 `505300@505310.png`。
   - **指令卡人物贴图命名规范**：`<id>@<灵基阶段>.png`（如圣玛修等在详情页与战斗中 ID 不同的变体，可开启 `AutoCommandCardSvtImgDownload` 查看实际文件名）。

---

## 特色功能指南


### 自定义加载图像与交互编辑

样例参考与下载：[Load 资源样例](https://github.com/Jekyell/F_Replace/releases/tag/load)

1. 创建 `Mod/Figure/Load/` 目录，放入精灵图 `animation.png` 及帧配置 `animation.json`。
2. 保持 `LoadingOverlay=1`，即可替换游戏右下角默认芙芙加载动画。
3. **可视化调整位置与大小**：
   - 在 App 配置页开启「加载图交互编辑模式」（`LoadingOverlayEdit=1`）。
   - 进入游戏出现加载图时，**长按加载图**即可进入交互编辑状态：单指拖动调整位置、双指捏合缩放尺寸，点击保存后自动将 `MARGIN_RIGHT`、`MARGIN_BOTTOM`、`IMAGE_SIZE` 写回 `frep.config`。
   - 调整完毕建议关闭 `LoadingOverlayEdit`，防止游玩时误触。

### 超分图集按需自动下载

开启 `AutoCharaFigureDownload` / `AutoCharaGraphDownload` / `AutoNarrowFigureDownload` / `AutoFaceDownload` / `AutoStatusDownload` 等开关后，游戏在加载对应从者资源时会自动按需从云端拉取超分图集并实时刷新显示。建议国内网络环境挂梯子使用以保证下载稳定。

> [!TIP]
> - **本地 MOD 始终优先**：若本地目录已手动放入自定义文件（例如 `Figure/CharaGraph/4000100@4.png`），模块会始终优先加载本地文件，不会触发该资源的自动下载，也不会覆盖你的文件。
> - **独立缓存与增量更新**：自动下载的资源统一存放在各子目录的 `AutoDownload/` 文件夹中（例如 `Figure/CharaGraph/AutoDownload/`），支持云端清单增量校验与异常自动重试。

---

## 工具与扩展接口

### 剧情文本打包工具 (FGSB Tool)

工具下载：[FGSB Tool Releases](https://github.com/Jekyell/F_Replace/releases/tag/tools)

- **封包**（目录打包为 `.fgsb`）：`./fgsb_tool.exe pack ./剧情文件夹 20250209.fgsb`
- **解包**（`.fgsb` 解压为目录）：`./fgsb_tool.exe unpack 20250209.fgsb ./剧情文件夹`
- **读取逻辑**：打包文件存放在 `Mod/ScriptBundle/`。模块会优先读取 `Mod/Script/` 下的 `.txt` 解压文件。推荐直接使用 App 自动拉取，非必要情况下无需手动打包 `.fgsb`。

### 剧情文本语音接口 (ScriptVoice)

#### 一、文件放置
创建 `Mod/ScriptVoice/` 文件夹，按剧情 ID 建立子目录（支持 `mp3` / `wav` 格式）：

```text
Mod/ScriptVoice/
└─ 9415901010                ← 剧情ID
   ├─ 1.mp3                  ← 语音文件，文件名对应编号
   ├─ 2.mp3
   ├─ 3.mp3
   └─ Mapping/mapping.json   ← 映射表（也可直接放 9415901010/mapping.json，二选一）
```

#### 二、mapping.json 写法
```json
{
  "id": "9415901010",
  "1": [ { "jp": "日文原文", "cn": "中文翻译" } ],
  "2": [ { "jp": "……", "cn": "……" } ]
}
```

**规则说明**：
1. 最外层的 `"1"`、`"2"`、`"3"` 是语音编号，必须与语音文件名一致（`1.mp3`、`2.mp3`…）。
2. 数组条数不限，键名任意（`jp`、`cn`、`en` 均可，只匹配值不校验键名），不同语言文本可共用同一条音频。
3. 文本需与游戏台词逐字一致（含标点）；剧情中的 `[r]`、`[charaFace]` 等控制符会自动忽略，JSON 中填写纯文本即可。

- **示例配置下载**：[Telegram 示例配置](https://t.me/fgomod/818)

---

## 常见问题 (FAQ)

<details>
<summary><b>Q: MuMu 模拟器国际版启动游戏卡死 / 闪退怎么办？</b></summary>

<br />

* **问题原因**：MuMu 模拟器自带的系统翻译插件 (`com.mumu.acc`) 与本模块发生冲突。
* **解决方法**：
  1. 打开模拟器中的 **MT 管理器**，进入侧边栏的 **终端模拟器**。
  2. 输入以下命令禁用该插件：
     ```bash
     su
     pm disable com.mumu.acc
     ```
  3. 彻底重启模拟器（需在 Windows 系统托盘右键退出 MuMu 模拟器主程序后重新打开）。

</details>

<details>
<summary><b>Q: AutoDownload 功能下载很慢或者下载失败怎么办？</b></summary>

<br />

* **解决方法**：开启网络代理（梯子）后再进入对应界面触发下载即可，模块支持自动重试。

</details>

<details>
<summary><b>Q: 在 App 里改了开关但游戏里没立刻变化怎么办？</b></summary>

<br />

* 带有 **「即时」** 标签的开关（如 3D 全息闪卡、动画跳过、底层性能优化、超分自动下载等）会在游戏内自动热生效。
* 带有 **「需重启」** 标签的开关（如 `UnlockFPS`、`UIReplace`、`FontReplace`、各项核心替换总开关等）需要在彻底关闭并重启游戏后生效。

</details>



---

## 相关链接

- [Telegram 频道](https://t.me/fgomod)
- [日服汉化仓库 (Jekyell/mappings)](https://github.com/Jekyell/mappings)
- [模型修改参考 (models.md)](https://github.com/Jekyell/F_Replace/blob/main/models.md)
