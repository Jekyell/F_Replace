# F_Replace
替换以下
```
技能替换
从者简介
礼装描述
指令纹章描述
礼装筛选界面
强化技能
强化宝具
加载界面提示
任务
战斗中的从者状态和技能名称
优化界面切换黑屏过渡
优化个人空间加载速度
```

### 使用方法
下载 https://github.com/Jekyell/mappings 这个仓库里的所有内容放在 `/sdcard/Android/data/com.aniplex.fategrandorder/files/Mod/`
并将Mod文件夹和他的子文件/子文件夹权限设置为777

### 模块自定义开关
游戏启动后，会自动在 Mod/configs/ 目录下生成 frep.config 配置文件。
可在文件中根据需求手动设置各功能的开启 (1) 或关闭 (0)。

### 头像替换

* **存放路径**：`Mod/Figure/Faces/`
* **格式与尺寸**：PNG，256×256 或 512×512
* **命名规则**：`从者ID_灵基突破数.png`

  * 例如：`204300_0.png` → 替换芭万希的 0 破头像


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
2. 在该目录中放置精灵图像文件及 JSON 配置文件（例如附件中的 animation.json 和 animation.png ）

补充说明：
frep.config 配置文件新增以下选项：
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
FPS=24
```

https://t.me/fgomod

