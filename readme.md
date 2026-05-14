##  H2

使用的版本为 [hooke007/MPV_lazy](https://github.com/hooke007/MPV_lazy) 2023v5 懒人包

## 控制台常用命令

### 调整字幕位置：

```sh
set sub-pos 120
```

`--sub-pos=<0-150>`

指定字幕在屏幕上的位置。该值是字幕的垂直位置，单位是屏幕高度的%。100是原始位置，通常不是屏幕的绝对底部，而是在底部和字幕之间有一些留空。高于100的数值会使字幕进一步向下移动。



## 主要配置文件：

### mpv.conf：

播放器核心设置

### input_uosc.conf：

快捷键设置

### profiles.conf

预设组，可以设置播放时置顶

### script-opts/uosc.conf

uosc 的配置文件

## 主要脚本：

### uosc

简洁好用的 GUI 界面，支持自定义组件

内置组件的预设在 `uosc/elements/Controls.lua` 内22行部分

### speed-control

实现速度快捷切换的脚本

### thumbfast

进度条显示缩略图脚本

### stats

文件信息的汉化版

### autoload

自动加载下一个视频



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

#### 配置方案

高端GPU用这些（如GTX 1080, RTX 2070, RTX 3060, Vega 56, 5700XT, 6600XT）

```
CTRL+1 no-osd change-list glsl-shaders set "~~/shaders/Anime4K_Clamp_Highlights.glsl;~~/shaders/Anime4K_Restore_CNN_VL.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_VL.glsl;~~/shaders/Anime4K_AutoDownscalePre_x2.glsl;~~/shaders/Anime4K_AutoDownscalePre_x4.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_M.glsl"; show-text "Anime4K: Mode A (HQ)"
CTRL+2 no-osd change-list glsl-shaders set "~~/shaders/Anime4K_Clamp_Highlights.glsl;~~/shaders/Anime4K_Restore_CNN_Soft_VL.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_VL.glsl;~~/shaders/Anime4K_AutoDownscalePre_x2.glsl;~~/shaders/Anime4K_AutoDownscalePre_x4.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_M.glsl"; show-text "Anime4K: Mode B (HQ)"
CTRL+3 no-osd change-list glsl-shaders set "~~/shaders/Anime4K_Clamp_Highlights.glsl;~~/shaders/Anime4K_Upscale_Denoise_CNN_x2_VL.glsl;~~/shaders/Anime4K_AutoDownscalePre_x2.glsl;~~/shaders/Anime4K_AutoDownscalePre_x4.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_M.glsl"; show-text "Anime4K: Mode C (HQ)"
CTRL+4 no-osd change-list glsl-shaders set "~~/shaders/Anime4K_Clamp_Highlights.glsl;~~/shaders/Anime4K_Restore_CNN_VL.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_VL.glsl;~~/shaders/Anime4K_Restore_CNN_M.glsl;~~/shaders/Anime4K_AutoDownscalePre_x2.glsl;~~/shaders/Anime4K_AutoDownscalePre_x4.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_M.glsl"; show-text "Anime4K: Mode A+A (HQ)"
CTRL+5 no-osd change-list glsl-shaders set "~~/shaders/Anime4K_Clamp_Highlights.glsl;~~/shaders/Anime4K_Restore_CNN_Soft_VL.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_VL.glsl;~~/shaders/Anime4K_AutoDownscalePre_x2.glsl;~~/shaders/Anime4K_AutoDownscalePre_x4.glsl;~~/shaders/Anime4K_Restore_CNN_Soft_M.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_M.glsl"; show-text "Anime4K: Mode B+B (HQ)"
CTRL+6 no-osd change-list glsl-shaders set "~~/shaders/Anime4K_Clamp_Highlights.glsl;~~/shaders/Anime4K_Upscale_Denoise_CNN_x2_VL.glsl;~~/shaders/Anime4K_AutoDownscalePre_x2.glsl;~~/shaders/Anime4K_AutoDownscalePre_x4.glsl;~~/shaders/Anime4K_Restore_CNN_M.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_M.glsl"; show-text "Anime4K: Mode C+A (HQ)"
CTRL+0 no-osd change-list glsl-shaders clr ""; show-text "GLSL shaders cleared"
```

中端GPU（如GTX 970, GTX 1060, RX 570,GTX1650)

```
CTRL+1 no-osd change-list glsl-shaders set "~~/shaders/Anime4K_Clamp_Highlights.glsl;~~/shaders/Anime4K_Restore_CNN_M.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_M.glsl;~~/shaders/Anime4K_AutoDownscalePre_x2.glsl;~~/shaders/Anime4K_AutoDownscalePre_x4.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_S.glsl"; show-text "Anime4K: Mode A (Fast)"
CTRL+2 no-osd change-list glsl-shaders set "~~/shaders/Anime4K_Clamp_Highlights.glsl;~~/shaders/Anime4K_Restore_CNN_Soft_M.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_M.glsl;~~/shaders/Anime4K_AutoDownscalePre_x2.glsl;~~/shaders/Anime4K_AutoDownscalePre_x4.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_S.glsl"; show-text "Anime4K: Mode B (Fast)"
CTRL+3 no-osd change-list glsl-shaders set "~~/shaders/Anime4K_Clamp_Highlights.glsl;~~/shaders/Anime4K_Upscale_Denoise_CNN_x2_M.glsl;~~/shaders/Anime4K_AutoDownscalePre_x2.glsl;~~/shaders/Anime4K_AutoDownscalePre_x4.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_S.glsl"; show-text "Anime4K: Mode C (Fast)"
CTRL+4 no-osd change-list glsl-shaders set "~~/shaders/Anime4K_Clamp_Highlights.glsl;~~/shaders/Anime4K_Restore_CNN_M.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_M.glsl;~~/shaders/Anime4K_Restore_CNN_S.glsl;~~/shaders/Anime4K_AutoDownscalePre_x2.glsl;~~/shaders/Anime4K_AutoDownscalePre_x4.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_S.glsl"; show-text "Anime4K: Mode A+A (Fast)"
CTRL+5 no-osd change-list glsl-shaders set "~~/shaders/Anime4K_Clamp_Highlights.glsl;~~/shaders/Anime4K_Restore_CNN_Soft_M.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_M.glsl;~~/shaders/Anime4K_AutoDownscalePre_x2.glsl;~~/shaders/Anime4K_AutoDownscalePre_x4.glsl;~~/shaders/Anime4K_Restore_CNN_Soft_S.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_S.glsl"; show-text "Anime4K: Mode B+B (Fast)"
CTRL+6 no-osd change-list glsl-shaders set "~~/shaders/Anime4K_Clamp_Highlights.glsl;~~/shaders/Anime4K_Upscale_Denoise_CNN_x2_M.glsl;~~/shaders/Anime4K_AutoDownscalePre_x2.glsl;~~/shaders/Anime4K_AutoDownscalePre_x4.glsl;~~/shaders/Anime4K_Restore_CNN_S.glsl;~~/shaders/Anime4K_Upscale_CNN_x2_S.glsl"; show-text "Anime4K: Mode C+A (Fast)"
CTRL+0 no-osd change-list glsl-shaders clr ""; show-text "GLSL shaders cleared"
```

以上是官方给出的示例，如果是核显或者入门级独显，如Vega8,UHD 630,Geforce 840M这种，最好使用更低一级的glsl文件

选择合适的配置文件并设置好快捷键后，就可以在MPV中使用了，打开视频文件后，按CTRL+1,2,...启用自己想要的方案，CTRL+0为清除全部着色器恢复原样。

文件名已注明，质量等级由UL>VL>L>M>S，自己可以尝试更改上述input.conf中的引用名调用不同等级的文件，最终目标是将平均帧时间控制在一定的范围内。

24fps视频~41000μs

30fps视频~33000μs

60fps视频~16000μs


MPV中按TAB(依我之前视频给出的MPV配置方案，如果用官方默认方案为i)显示视频信息，平均帧时间看Frame Timings这一项的average。

#### 官方说明文档，包含上面的内容：

- 高级说明：[Anime4K/md/GLSL_Instructions_Advanced.md](https://github.com/bloc97/Anime4K/blob/8e39551ce96ed172605c89b7dd8be855b5502cc9/md/GLSL_Instructions_Advanced.md#advanced-usage-instructions-glsl--mpv-v4x)
- Win说明：[Anime4K/md/GLSL_Instructions_Windows_MPV.md](https://github.com/bloc97/Anime4K/blob/8e39551ce96ed172605c89b7dd8be855b5502cc9/md/GLSL_Instructions_Windows_MPV.md)

