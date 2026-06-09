# F_Replace


### 使用方法
下载 https://github.com/Jekyell/mappings 这个仓库里的所有内容放在 `/sdcard/Android/data/com.aniplex.fategrandorder/files/Mod/`
并将Mod文件夹和他的子文件/子文件夹权限设置为777

### 模块自定义开关
游戏启动后，会自动在 Mod/configs/ 目录下生成 frep.config 配置文件。
可在文件中根据需求手动设置各功能的开启 (1) 或关闭 (0)。

### 头像替换

* **存放路径**：`Mod/Figure/Faces/`
* **格式与尺寸**：PNG，256×256 或 512×512
* **命名规则**：`从者ID@图片序号.png`

  * 例如：`204300@1.png` → 替换芭万希的第一个头像


### 剧情文本配套工具 (FGSB Tool)

工具下载： [FGSB Tool](https://github.com/Jekyell/F_Replace/releases/tag/tools)

**封包（将文件夹打包为 fgsb）：**
```bash
./fgsb_tool.exe pack ./剧情文件夹 20250209.fgsb

```
**解包（将 fgsb 解压为文件夹）：**

```bash
./fgsb_tool.exe unpack 20250209.fgsb ./剧情文件夹

```

将打包好的 .fgsb 文件放入 Mod/ScriptBundle/ 目录下即可实现单文件剧情文本替换。与原有的单txt剧情文本替换功能兼容，模块会优先读取 Mod/Script/ 目录下的 txt 文件

### 自定义加载图像功能

操作步骤：
1. 创建 /Mod/Figure/Load 目录
2. 在该目录中放置精灵图像文件及 JSON 配置文件（例如附件中的 animation.json 和 animation.png  https://github.com/Jekyell/F_Replace/releases/tag/load ）

frep.config 配置项：
```
# 自定义加载动画图像位置配置 (单位: DP)
# 功能开关
LoadingOverlay=1
# 右侧边距
MARGIN_RIGHT=45
# 底部边距
MARGIN_BOTTOM=14
# 图像尺寸
IMAGE_SIZE=150
# 每秒播放的动画帧数
LoadingFPS=24
```

### 动图/单图卡面替换功能
本次更新功能属于实验性功能，可能会造成内存/显存溢出，或其他未知错误。

适用范围：Face，NarrowFigure，CharaGraph，Charafigure(不支持动图)，Status(不支持动图)

使用说明： 统一命名规范为 [从者id]@突破数，如奥尔加玛丽满破：4000100@4（突破数范围为 1~4）。

对于单图替换和动图替换，最好使用原卡面尺寸的同比例倍数，支持高分辨率图像。但动图要注意帧率和分辨率，规格过高会导致设备运行卡顿甚至内存溢出导致游戏闪退，建议以原图尺寸的2倍左右超分，帧率30帧。

全从者卡面超清图集：https://t.me/fgomod_1  
下载后请在 Mod/Figure 文件夹里面创建对应的压缩包文件夹，把图片解压进去  
若在模拟器使用发现卡面不够清晰，可以模拟器设置里调高分辨率  

| 类型             | 支持格式                                   | 优先级                                   | 备注                              |
| -------------- | -------------------------------------- | ------------------------------------- | ------------------------------- |
| `CharaGraph`   | `.mp4`, `.webp`, `.png`                | `mp4` → `webp` → `png`                | -                   |
| `CharaFigure`  | `.png`, `.astc`, `.astc.zstd`          | `png` → `astc` → `astc.zstd`          | -         |
| `NarrowFigure` | `.mp4`, `.webp`, `.png`                | `mp4` → `webp` → `png`                | 切角为四个角  |
| `Face`         | `.mp4`, `.webp`, `.png`                | `mp4` → `webp` → `png`                | 切角为左上+右上 |
| `Status`       | `.webp`, `.png`, `.astc`, `.astc.zstd` | `webp` → `png` → `astc` → `astc.zstd` | -              |


### 帧率解锁功能
```
#是否开启解锁FPS功能项（0关闭，1开启）
UnlockFPS=0
#帧率
FPS=60
```

频道：https://t.me/fgomod

