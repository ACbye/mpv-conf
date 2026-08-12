# 自用 MPV 配置说明

自用 mpv 配置（基于 [mpv-winbuild](https://github.com/shinchiro/mpv-winbuild-cmake) 构建 `mpv-x86_64-v3-20260810-git-f4d13e1c2c`，内核版本 mpv v0.41.0-922-gf4d13e1c2），配置由旧版 [hooke007/MPV_lazy](https://github.com/hooke007/MPV_lazy) 2023v5 懒人包迁移并重写

## 控制台常用命令

### 调整字幕位置：

```sh
set sub-pos 120
```

`--sub-pos=<0-150>`

指定字幕在屏幕上的位置。该值是字幕的垂直位置，单位是屏幕高度的%。100是原始位置，通常不是屏幕的绝对底部，而是在底部和字幕之间有一些留空。高于100的数值会使字幕进一步向下移动。



## 主要配置文件：

### mpv.conf：

播放器核心设置（渲染器 gpu-next + d3d11，硬解 auto,nvdec-copy）

### script-opts.conf

脚本选项集中管理（由 mpv.conf 通过 `include` 引入，比分散写在各脚本 conf 中更直观）

### input_uosc.conf：

快捷键设置（uosc 增强键位，含 Anime4K 着色器绑定，见下文）

### profiles.conf

预设组（HDR/SDR 等按片源自动切换）

### script-opts/uosc.conf

uosc 的配置文件

## 主要脚本：

### uosc

简洁好用的 GUI 界面，支持自定义组件（当前版本 5.13）

内置组件的预设在 `uosc/elements/Controls.lua` 内22行部分

### thumbfast

进度条显示缩略图脚本（当前为上游 po5/thumbfast master 版，非旧汉化版）

### speed-control

实现速度快捷切换的脚本

### stats

文件信息的汉化版（TAB 键常驻显示统计信息，按 t 切换）

### autoload

自动加载下一个视频

### save_global_props

跨会话保存全局属性（如音量），重启后恢复



## Anime4K指南

模式 A（针对 1080p 动漫进行了优化）。

模式 B（针对 720p 动漫进行了优化）。

模式 C（针对 480p 动漫进行了优化）。

如果要提高感知质量，请使用相应的辅助模式。

| 主模式 | 对应的辅助模式 |
| ------ | -------------- |
| A      | A+A            |
| B      | B+B            |
| C      | C+A            |

这些模式只能用于 x2 或更高的升频比。如果您有 1080p 屏幕，在 1080p 动漫上使用模式 A 将提高图像质量，但模式 A+A 很可能会过度锐化并降低图像质量。

### 当前配置内置的键位（input_uosc.conf）

中端GPU（如GTX 970, GTX 1060, RX 570, GTX1650）—— Fast 组：

```
Ctrl+1    Anime4K: Mode A+A (Fast)
Ctrl+2    Anime4K: Mode B+B (Fast)
Ctrl+3    Anime4K: Mode C+A (Fast)
```

高端GPU用这些（如GTX 1080, RTX 2070, RTX 3060, Vega 56, 5700XT, 6600XT）—— HQ 组：

```
Ctrl+4    Anime4K: Mode A+A (HQ)
Ctrl+5    Anime4K: Mode B+B (HQ)
Ctrl+6    Anime4K: Mode C+A (HQ)
Ctrl+7    Anime4K: Mode A (HQ)
Ctrl+8    Anime4K: Mode B (HQ)
Ctrl+9    Anime4K: Mode C (HQ)
Ctrl+0    清除全部着色器
```

如果是核显或者入门级独显，如Vega8, UHD 630, Geforce 840M这种，最好使用更低一级的glsl文件（可自行修改上述键位引用的文件名，质量等级由 UL>VL>L>M>S）。

打开视频文件后，按 Ctrl+1..9 启用对应方案，Ctrl+0 清除全部着色器恢复原样。最终目标是将平均帧时间控制在一定的范围内：

| 视频帧率 | 最大时间 (ms) |
| -------- | ------------- |
| 24       | 41            |
| 30       | 33            |
| 60       | 16            |

按 TAB 显示视频信息，平均帧时间看 Frame Timings 这一项的 average。

#### 官方说明文档：

- 高级说明：[Anime4K/md/GLSL_Instructions_Advanced.md](https://github.com/bloc97/Anime4K/blob/8e39551ce96ed172605c89b7dd8be855b5502cc9/md/GLSL_Instructions_Advanced.md#advanced-usage-instructions-glsl--mpv-v4x)
- Win说明：[Anime4K/md/GLSL_Instructions_Windows_MPV.md](https://github.com/bloc97/Anime4K/blob/8e39551ce96ed172605c89b7dd8be855b5502cc9/md/GLSL_Instructions_Windows_MPV.md)



## 排障记录

### uosc 进度条缩略图不显示（mpv 0.41 + thumbfast）

- **现象**：升级 mpv 到 0.41 后，uosc 悬停进度条无缩略图；同一份配置在旧版 mpv 0.35 上正常
- **根因**：旧版 thumbfast 子进程参数硬编码 `--vo=null`，与 `--o=`（编码模式）冲突。mpv 0.41 起不再像 0.35 那样用内置 encoding profile 覆盖命令行 `--vo` 值，导致 vo=null 在编码上下文下初始化失败（`Error opening/initializing the selected video_out`），子进程秒退、永远写不出缩略图文件。与 uosc 协议、PATH、IPC 管道均无关
- **修复**：将 `scripts/thumbfast.lua` 升级为上游 po5/thumbfast master 版（已移除 `--vo=null`），实测 uosc 悬停 → 子进程出图 → overlay 渲染全链路正常
- **备份**：旧汉化版原文件见 `_cache/backup/thumbfast_20260811/`，如需回滚直接复制回 `scripts/thumbfast.lua`
- **注意**：新版 thumbfast 选项名有变化（`binpath`→`mpv_path`、`tnpath`→`thumbnail`），已移除 `min_duration`/`sw_threads` 等旧选项，`script-opts.conf` 中对应项已注释；默认缩略图尺寸由 300 改为 200（可用 `thumbfast-max_width`/`thumbfast-max_height` 调回）
