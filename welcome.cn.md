<a id="WelcomeTop"></a>

# Welcome to StableCanvas

##  What is StableCanvas?

StableCanvas 可以视为另一种对于 stable diffusion 系列模型的 gui 程序。但是和传统的 a111/webui 或者 comfyui 不同的是，本程序更加专注于对图片的编辑以及对画布的操作，同时并不包含任何后端程序，我们的所有程序代码都运行在你的浏览器中。

简单来说你可以将这个程序比作 PhotoShop 的替代品（在AI方面），简化操作，并将 AI 创作的过程更加合理和流畅，而不需要在不同的程序之间切换。

没有后端意味着，你需要自己部署你的后端服务，当然也意味着，你可以使用任何（如果api不是标准的a111后端api你可以自定义js脚本调用它）后端进行图像生成。

[[back to top](#WelcomeTop)]

##   Showcase from YouTube

[![Showcase Youtube Playlist](http://img.youtube.com/vi/lnhcBCJ_NhM/0.jpg)](https://www.youtube.com/watch?v=lnhcBCJ_NhM&list=PLNaPKZgVE2TxkO2rc7mvXCNuTLS-uxQ6j "Showcase Youtube Playlist")

[[back to top](#WelcomeTop)]

## Shortcuts

<table border="1">
  <tr>
    <th>序号</th>
    <th>类别</th>
    <th>快捷键</th>
    <th>功能</th>
  </tr>
  <tr>
    <td>1</td>
    <td rowspan="8">全局快捷键</td>
    <td>Ctrl + S</td>
    <td>保存当前编辑器状态到项目文档文件</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Ctrl + Z</td>
    <td>撤销</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Ctrl + Y</td>
    <td>重做</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Ctrl + K</td>
    <td>打开命令面板</td>
  </tr>
  <tr>
    <td>5</td>
    <td>按下鼠标中键</td>
    <td>拖拽 Viewport</td>
  </tr>
  <tr>
    <td>6</td>
    <td>Ctrl + 鼠标滚轮</td>
    <td>在 Y 轴方向移动 Viewport</td>
  </tr>
  <tr>
    <td>7</td>
    <td>Shift + 鼠标滚轮</td>
    <td>在 X 轴方向移动 Viewport</td>
  </tr>
  <tr>
    <td>8</td>
    <td>Alt + 鼠标滚轮</td>
    <td>缩放 Viewport</td>
  </tr>
  <tr>
    <td>9</td>
    <td rowspan="3">Move tool</td>
    <td>W/A/S/D</td>
    <td>移动 Viewport</td>
  </tr>
  <tr>
    <td>10</td>
    <td>Q/E</td>
    <td>缩放 Viewport</td>
  </tr>
  <tr>
    <td>11</td>
    <td>Shift + W/A/S/D</td>
    <td>快速移动 Viewport</td>
  </tr>
  <tr>
    <td>12</td>
    <td rowspan="1">lasso tool</td>
    <td>Shift</td>
    <td>反转蒙版画笔</td>
  </tr>
</table>

##  Setting Up

###  Base on A1111/WebUI

本程序推荐你使用 a1111 webui 作为后端，因为这几乎是 stable diffusion 开源社区中的事实标准。

要使用 a1111 webui 作为后端是非常简单的，大体上你可以依据下面几个步骤开始你的后端配置：

1. 拉取 a1111 webui 代码仓库 
   - （建议使用 [1.8.0版本](https://github.com/AUTOMATIC1111/stable-diffusion-webui/releases/tag/v1.8.0) ，1.9.x版本未经过本程序完全测试不保证能正常使用）
2. 下载并配置你的模型、lora、embedding
3. **KEY STEP**: 启动时，请在启动参数中添加 ```--cors-allow-origins``` 和 ```--api```
   1. 你可以配置为 ```--cors-allow-origins=*```
   2. 或者仅仅支持本程序调用 ```--cors-allow-origins=studio.stablecanvas.com```
4. 打开本程序，在 ```Service``` 菜单中 ```Clients``` 内，配置并测试你的后端是否联通

[[back to top](#WelcomeTop)]

###  Base on ComfyUI

由于 ComfyUI 的开发便利性，现在已经很多人从 webui 迁移到 ComfyUI 中进行使用。当然本程序调用 ComfyUI 也是非常简单的。

配置启动项：
- 添加 `--enable-cors-header=*` 到你的 ComfyUI 启动项中，以允许 StableCanvas 通过网页调用 api

配置工作流：
有以下3种方式可以配置 ComfyUI Workflow：

1. 使用内置 ComfyUI 预设
本程序自带一个 ComfyUI 预设，你仅仅需要配置 后端地址 即可。

2. 自定义 ComfyUI 预设
由于 ComfyUI 的便利性，你完全可以自定义一个新的工作流用于本程序的调用。

你可以参考下面的这个 Workflow 进行进一步开发：

![workflow](./workflows/workflow.png)

需要注意的是，在编辑时请保留其中的 `<%- slot_name %>` 字符串插槽，这会在 api 调用时替换为请求内容。

> 当你决定自己配置 ComfyUI 的时候，由于 ComfyUI 不支持一些常见的编程节点所以...你可能需要配置三套 workflow 才可以无缝的完整的使用本程序，具体可以检查 ComfyUI Demo 代码。

3. 编写自定义的调用脚本
如果你需要完全定制的后端调用（比如使用自定义的python后端时），你完全可以从零开始编写 预设脚本 。

在下面的 preset 栏目中会详细介绍。

[[back to top](#WelcomeTop)]

##  Features

###  Elements

有多个视图可以对元素进行管理，不同视图的的功能如下

- Elements: 元素视图，将显示当前项目中画布上的所有元素，你可以对画布中的元素进行
- Layers: 在这个视图内，你可以创建和管理画布图层，并对一个图层中的所有元素进行统一的显示或者pin/unpin
- Latents: 在这个视图中将会列出所有latent元素，对于部分latent类型，将提供编辑操作，比如 pose
- Draws: 在这里将会展示出所有的 `draw` 元素，你可以在这里配置这个区域的 prompt 选择 备选的生成结果，或者配置其他参数

[[back to top](#WelcomeTop)]

###  Presets

你可能会疑惑，我们生成图片的的时候为什么不需要设置参数？
原因就在于，每次请求时，我们会选择预设配置，根据配置对请求生成图片。

####  A1111 preset
对于 a1111 预设，几乎和在 webui 中的逻辑一样配置即可，唯一不同的是，你可以使用 `{{__PROMPT__}}` 和 `{{__NEGATIVE_PROMPT__}}` 来配置在 `Draw Area` 中输入的 prompt 应该插入到哪里

####  ComfyUI preset
本程序没有直接提供 ComfyUI 的预设配置，但是提供了 自定义脚本预设 的能力，并提供了一个demo预设，可以用于调用 ComfyUI。

####  Script preset
脚本预设是极其灵活的功能，可以帮助你对接到任何后端。

点击 `[New Preset (script)]` 之后即可添加 script preset。

一个基本的 脚本预设 如下：

```js
/**
 * Your Custom Script
 */

export const schema = {
  type: "object",
  properties: {
    base_url: {
      type: "string",
      title: "Base URL",
      default: "http://localhost:7860",
    },
  },
};

export const request = ({ payload, progress, config }) => {
  const {
    width,
    height,
    prompt,
    negative_prompt,
    input_image_b64,
    mask_image_b64,
  } = payload;
  const { base_url } = config;

  throw new Error("Not implemented");
};
```

其中，你需要导出 `request` 函数，同时可以提供 `schema` 对象。

如果你导出了 `schema` 在 预设编辑器 中将显示对此脚本的配置，配置结果将在 `config` 参数中得到。


[[back to top](#WelcomeTop)]

###  Feeds
在 feed 视图中，你可以查看所有历史生成结果。

[[back to top](#WelcomeTop)]

###  External Tool
拓张工具，这是一个极为灵活的功能，将提供隔离于当前编辑器的 `iframe` 环境，对于开发者，可以使用 `@stable-canvas/sdk` 包进行 拓展工具 开发。

当然不使用sdk的页面也完全可以作为 拓展工具 添加到配置中。

####  Builtin

内部提供的 拓展工具 可以在这里找到 https://github.com/StableCanvas/sc-tools

其中默认添加有如下工具:
| Tools         | Description                          | 
|---------------|--------------------------------------|
| [prompt-editor](https://stablecanvas.github.io/sc-tools/prompt-editor/) | Edit A1111 style prompt          | 
| [sd-playground](https://stablecanvas.github.io/sc-tools/sd-playground/) | simple playground for sd (eg. streaming for lcm)         | 
| [pnginfo](https://stablecanvas.github.io/sc-tools/pnginfo/) | pnginfo explorer         |

####  Develop

关于如何开发 拓展工具，请关注这个 repo：
https://github.com/StableCanvas/sc-sdk

[[back to top](#WelcomeTop)]

###  Preference
在这个视窗中，你将可以对各种内部状态进行配置。

类似编辑器的设置，你可以对 global 或者 workspace 配置，两个配置将会同时启用，而 workspace 配置优先级比 global 高。

当你打开一个项目文件，或者导入 preference 配置时，将会填充到 workspace 配置中。

[[back to top](#WelcomeTop)]

###  Draw Pipeline
> 🚧 这是一个 WIP 的特性，暂时无法使用

此特性将支持对请求进行流水线定制，比如 可以在每次调用之后调用 rembg 进行抠图，比如 进行可编程的 hires fix，等等

[[back to top](#WelcomeTop)]

###  Tools

- Mouse: 默认tool，没有任何特别功能 
- Move: 移动工具，你可以移动画布上的元素，resize和拖拽元素
- Marquee: 选区工具，可以创建生成选区，在选区中你可以绘制蒙版或者直接生成图片
- Lasso: 套索工具，使用这个工具在选区中创建蒙版

[[back to top](#WelcomeTop)]

###  Project (export/import)

在某些情况下，你可能需要复用本程序中的某些配置，下面你可以从各个方面导入导出当前程序中的状态。

- Preference: 仅仅导入导出 preference 。注意这里导入时将仅仅覆盖 preference.workspace
- Presets: 你可以将预设导出，其中会包含你当前添加的预设，包括脚本预设
- Feeds: 如果你需要使用本程序进行一些批量生成工作，那么导出feeds是有必要的，你可以将所有feeds中的图片导出保存为一个 .zip 文件
- Workspace: 你可以选择两种方式导出
  - Current: 获得当前的 workspace 配置 json 数据，这里得到的数据和已有配置无关，仅仅为本程序的当前状态
  - All: 你可以导出所有的 workspace 配置

####  Template

在 startup 中，你会看到 [start from template] 按钮，你可以选择从一个模板开始你的项目。

一个模板将包含以下数据:
- Workspace: 包含布局信息。
- Editor(partial): 包含一部分的编辑器信息。其中所有的非 图片 数据都可以被包含到 template 中，比如 draw-area 中的prompt，比如 note-area

[[back to top](#WelcomeTop)]

###  Project Document File

项目文件，对于你持续进行创作很有必要，特别是你想保留一些创意记录的时候。

StableCanvas 的项目文件以 *.scd 作为拓展名（StableCanvas Document）。

在一个 scd 文件中，可以包含以下信息：
Feeds, Presets, Scripts, Workspace, Editor

在 preference 中，你可以配置 scd 导出需要包含哪些内容，除了 Editor 信息以外，其他的数据 (Feeds, Presets, Scripts, Workspace) 均可以不包括在 scd 中。

[[back to top](#WelcomeTop)]

##  FAQs

> no QA here, waiting your review

[[back to top](#WelcomeTop)]


##  Community

下面是本程序的社区链接，在这里面你可以分享你对本程序的看法或者建议，或者交流分享你的项目文件和配置~

- **Github**: https://github.com/StableCanvas/StableCanvas
- **Discord**: https://discord.gg/DneNQsYccf
- **Youtube**: https://www.youtube.com/@stablecanvasdev

[[back to top](#WelcomeTop)]
