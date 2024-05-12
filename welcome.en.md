<a id="WelcomeTop"></a>

# Welcome to StableCanvas

## What is StableCanvas?

StableCanvas can be viewed as another GUI program for the stable diffusion series of models. However, unlike traditional a111/webui or comfyui, this program focuses more on editing images and operating on the canvas, and does not include any backend programs - all our program code runs in your browser. 

Simply put, you can consider this program as an alternative to Photoshop (in the AI aspect), simplifying operations and making the AI creation process more reasonable and smooth, without having to switch between different programs.

Having no backend means that you need to deploy your own backend service, but of course it also means that you can use any backend (if the API is not the standard a111 backend API, you can customize a JavaScript script to call it) for image generation.

[[back to top](#WelcomeTop)]

## Showcase from YouTube

[![Showcase Youtube Playlist](http://img.youtube.com/vi/lnhcBCJ_NhM/0.jpg)](https://www.youtube.com/watch?v=lnhcBCJ_NhM&list=PLNaPKZgVE2TxkO2rc7mvXCNuTLS-uxQ6j "Showcase Youtube Playlist")

[[back to top](#WelcomeTop)]

## Shortcuts

<table border="1">
  <tr>
    <th>No.</th>
    <th>Category</th>
    <th>Shortcut</th>
    <th>Function</th>
  </tr>
  <tr>
    <td>1</td>
    <td rowspan="8">Global Shortcuts</td>
    <td>Ctrl + S</td>
    <td>Save the current editor status to the project document file</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Ctrl + Z</td>
    <td>Undo</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Ctrl + Y</td>
    <td>Redo</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Ctrl + K</td>
    <td>Open Command Palette</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Middle Mouse Click</td>
    <td>Drag the viewport</td>
  </tr>
  <tr>
    <td>6</td>
    <td>Ctrl + Mouse Wheel</td>
    <td>Move the viewport on the Y-axis</td>
  </tr>
  <tr>
    <td>7</td>
    <td>Shift + Mouse Wheel</td>
    <td>Move the viewport on the X-axis</td>
  </tr>
  <tr>
    <td>8</td>
    <td>Alt + Mouse Wheel</td>
    <td>Zoom the viewport</td>
  </tr>
  <tr>
    <td>9</td>
    <td rowspan="3">Move Tool</td>
    <td>W/A/S/D</td>
    <td>Move the viewport</td>
  </tr>
  <tr>
    <td>10</td>
    <td>Q/E</td>
    <td>Zoom the viewport</td>
  </tr>
  <tr>
    <td>11</td>
    <td>Shift + W/A/S/D</td>
    <td>Fast move the viewport</td>
  </tr>
  <tr>
    <td>12</td>
    <td rowspan="1">Lasso Tool</td>
    <td>Shift</td>
    <td>Invert mask brush</td>
  </tr>
</table>

## Setting Up

### Base on A1111/WebUI

This program recommends using a1111 webui as the backend because it is almost the de facto standard in the stable diffusion open-source community.

Using a1111 webui as the backend is very simple. Generally, you can start your backend configuration according to the following steps:

1. Pull the a1111 webui code repository
   - (It is recommended to use [version 1.8.0](https://github.com/AUTOMATIC1111/stable-diffusion-webui/releases/tag/v1.8.0), the 1.9.x version has not been fully tested by this program and it is not guaranteed to work properly)
2. Download and configure your model, lora, and embedding
3. **KEY STEP**: When starting up, please add ```--cors-allow-origins``` and ```--api``` to the startup parameters
   1. You can configure it as ```--cors-allow-origins=*```
   2. Or only support this program call ```--cors-allow-origins=studio.stablecanvas.com```
4. Open this program, go to ```Service``` menu, and configure and test your backend connection in ```Clients```

[[back to top](#WelcomeTop)]

### Base on ComfyUI

Due to the development convenience of ComfyUI, many people have now migrated from webui to ComfyUI for use. Of course, it is also very simple to call ComfyUI with this program.

Configure startup items:
- Add `--enable-cors-header=*` to your ComfyUI startup item to allow StableCanvas to call the API via the web page

Configure workflows:
There are three ways to configure ComfyUI workflows:

1. Use the built-in ComfyUI preset
   - This program comes with a ComfyUI preset, you just need to configure the backend address.

2. Custom ComfyUI preset
   - Due to the convenience of ComfyUI, you can completely customize a new workflow for this program to call.

You can refer to the following Workflow for further development:

![workflow](./workflows/workflow.png)

Note that when editing, please keep the `<%- slot_name %>` string slot, which will be replaced with the request content when the API is called.

> When you decide to configure ComfyUI by yourself, due to the fact that ComfyUI does not support some common programming nodes, you may need to configure three sets of workflows to use this program seamlessly and completely. Please check the ComfyUI Demo code for details.

3. Write a custom call script
   - If you need a completely customized backend call (e.g. when using a custom Python backend), you can start from scratch and write a preset script.

The following section on presets will provide more details.

[[back to top](#WelcomeTop)]

## Features

### Elements

There are multiple views to manage elements, and the functions of different views are as follows:

- Elements: Element view, which will display all elements on the current project canvas, and you can perform operations on the elements in the canvas
- Layers: In this view, you can create and manage canvas layers, and perform unified display or pin/unpin operations on all elements in a layer
- Latents: This view will list all latent elements, and for some latent types, editing operations such as pose will be provided
- Draws: This will show all `draw` elements, where you can configure the prompt selection and alternative generation results for this area, or configure other parameters

[[back to top](#WelcomeTop)]

### Presets

You may wonder why we don't need to set parameters when generating images.
The reason is that we will choose a preset configuration for each request and generate images according to the configuration.

#### A1111 preset
For the a1111 preset, just configure it in the same logic as in webui, the only difference is that you can use `{{__PROMPT__}}` and `{{__NEGATIVE_PROMPT__}}` to configure where the prompt entered in the `Draw Area` should be inserted

#### ComfyUI preset
This program does not provide a preset configuration for ComfyUI directly, but provides the ability to customize script presets, and provides a demo preset that can be used to call ComfyUI.

#### Script preset
Script presets are extremely flexible and can help you connect to any backend.

Click `[New Preset (script)]` to add a script preset.

A basic script preset is as follows:

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

You need to export the `request` function and can provide the `schema` object.

If you export `schema`, the configuration for this script will be displayed in the preset editor, and the configuration result will be obtained in the `config` parameter.


[[back to top](#WelcomeTop)]

### Feeds
In the feed view, you can view all the historical generation results.

[[back to top](#WelcomeTop)]

### External Tool
Expansion tool, this is an extremely flexible function, which will provide an isolated `iframe` environment from the current editor. For developers, you can use the `@stable-canvas/sdk` package to develop expansion tools.

Of course, pages that do not use the SDK can also be added to the configuration as expansion tools.

#### Builtin

The built-in expansion tools can be found here: https://github.com/StableCanvas/sc-tools

The following tools are included by default:
| Tools         | Description                          | 
|---------------|--------------------------------------|
| [prompt-editor](https://stablecanvas.github.io/sc-tools/prompt-editor/) | Edit A1111 style prompt          | 
| [sd-playground](https://stablecanvas.github.io/sc-tools/sd-playground/) | simple playground for sd (eg. streaming for lcm)         | 
| [pnginfo](https://stablecanvas.github.io/sc-tools/pnginfo/) | pnginfo explorer         |

#### Develop

For information on how to develop expansion tools, please refer to this repo:
https://github.com/StableCanvas/sc-sdk

[[back to top](#WelcomeTop)]

### Preference
In this window, you can configure various internal states.

Similar to editor settings, you can configure global or workspace settings, and the two configurations will be enabled at the same time, with the workspace settings taking precedence over the global settings.

When you open a project file or import preference settings, it will be filled into the workspace settings.

[[back to top](#WelcomeTop)]

### Draw Pipeline
> 🚧 This is a WIP feature and is currently unavailable

This feature will support customization of the request pipeline, such as calling rembg for matting after each call, or performing programmable hires fixes, etc.

[[back to top](#WelcomeTop)]

### Tools

- Mouse: Default tool, with no special function 
- Move: Move tool, you can move, resize, and drag elements on the canvas
- Marquee: Marquee tool, you can create a marquee selection, and you can draw a mask or generate an image directly in the selection
- Lasso: Lasso tool, use this tool to create a mask in the selection

[[back to top](#WelcomeTop)]

### Project (export/import)

In some cases, you may need to reuse some configurations in this program. Here you can import and export the current state of the program from various aspects.

- Preference: Only import and export preference. Note that when importing, only preference.workspace will be overwritten
- Presets: You can export presets, which will include the presets you have added, including script presets
- Feeds: If you need to use this program to do some batch generation work, it is necessary to export feeds, you can export all the images in feeds as a .zip file
- Workspace: You can choose two ways to export
  - Current: Get the current workspace configuration json data. The data obtained here is independent of the existing configuration and is only the current state of this program
  - All: You can export all workspace configurations

#### Template

At startup, you will see the [start from template] button, and you can choose to start your project from a template.

A template will contain the following data:
- Workspace: Contains layout information.
- Editor (partial): Contains some of the editor information. All non-image data can be included in the template, such as prompts in the draw-area, or note-area

[[back to top](#WelcomeTop)]

### Project Document File

Project files are necessary for your continuous creation, especially when you want to keep some creative records.

The project file of StableCanvas uses *.scd as the file extension (StableCanvas Document).

An scd file can contain the following information: Feeds, Presets, Scripts, Workspace, Editor

In the preference, you can configure which content to include in the scd export, except for the Editor information, other data (Feeds, Presets, Scripts, Workspace) can be excluded from the scd.

[[back to top](#WelcomeTop)]

## FAQs

### 1. detect by: `No service info, click to refresh.`

- Firstly, you need to set up a functioning backend for a1111/webui.
- Secondly, the detect image recognition API relies on the [sd-webui-controlnet](https://github.com/Mikubill/sd-webui-controlnet) plugin, which can be used after proper installation.

[[back to top](#WelcomeTop)]


## Community

The following are community links for this program, where you can share your thoughts or suggestions on this program, or exchange and share your project files and configurations~

- **Github**: https://github.com/StableCanvas/StableCanvas
- **Discord**: https://discord.gg/DneNQsYccf
- **Youtube**: https://www.youtube.com/@stablecanvasdev

[[back to top](#WelcomeTop)]
