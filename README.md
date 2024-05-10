lang: [en](./README.md) | [cn](./readme.cn.md)

# StableCanvas
[StableCanvas.com](https://stablecanvas.com) is a free online tool designed to work in conjunction with the stable diffusion backend.

> As StableCanvas is not fully open source, this repository can serve as a place for bug reports, feature requests, and general discussions.

# Features

- Backend Support: Compatible with using webui or comfyui as the backend
  - Script Presets Support: Customize JavaScript scripts to interface with different backends
- User-friendly Operation: Easily perform inpainting and outpainting
- Infinite Canvas: Unlimited expansion of canvas size, enabling you to create boundless works within a single project (as long as your computer can handle it)
- Drag-and-drop Layout: Workspace layout that's adjustable by dragging
- Canvas Elements: Independent show/hide and pin/unpin functionality for elements on the canvas
- Latent Control: Control over all latents applicable to SD generation (mainly CNet modules)
- Pose Editor: Fully integrated pose editor
  - Editable animal poses
  - Complete pose data support, allowing for the editing of body, face, and hand data
- Application Replay: All configurations can be imported and exported
  - Unified export of all feeds as a `.zip` file
- Project Files: Support for saving application states as `.scd` project files, similar to `.psd`
- Plugin System: Extensible plugin system
- Development Tools: Rapid development of extension tools; you simply need to write an HTML
  - Using the `@stable-canvas/sdk` package, you can link your tools to the main program

##   Showcase from YouTube

[![Showcase Youtube Playlist](http://img.youtube.com/vi/lnhcBCJ_NhM/0.jpg)](https://www.youtube.com/watch?v=lnhcBCJ_NhM&list=PLNaPKZgVE2TxkO2rc7mvXCNuTLS-uxQ6j "Showcase Youtube Playlist")

## Shortcuts

<!--
Global Shortcuts
- Ctrl + S: Save current editor status to project document file
- Ctrl + Z: Undo
- Ctrl + Y: Redo
- Ctrl + K: Open Command Palette
- Middle Mouse Click: Drag the viewport
- Ctrl + Mouse Wheel: Move the viewport on the Y-axis
- Shift + Mouse Wheel: Move the viewport on the X-axis
- Alt + Mouse Wheel: Zoom the viewport (scale)

Move Tool Shortcuts
- WASD: Move the viewport
- QE: Zoom the viewport
- Shift + WASD: Fast move the viewport

Lasso Tool Shortcuts
- Shift: Invert mask brush
-->

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

## Configuration

### Based on AUTOMATIC1111/WebUI

This program recommends using AUTOMATIC1111's webui as the backend, as it is nearly the de facto standard within the stable diffusion open-source community.

It's quite straightforward to set up AUTOMATIC1111's webui as the backend, generally following these steps:

1. Clone the AUTOMATIC1111 webui repository
   - (Recommendation: use the [version 1.8.0](https://github.com/AUTOMATIC1111/stable-diffusion-webui/releases/tag/v1.8.0); version 1.9.x has not been fully tested with this program and compatibility is not guaranteed)
2. Download and configure your models, lora, and embeddings
3. **KEY STEP**: When starting, be sure to add `--cors-allow-origins` and `--api` to your start-up parameters
   1. You can configure it as `--cors-allow-origins=*`
   2. Or support this program only `--cors-allow-origins=studio.stablecanvas.com`
4. Open this program, in the `Service` menu inside `Clients`, configure and test whether your backend is connected

### Based on ComfyUI

Due to the convenience of ComfyUI development, many have migrated from webui to use ComfyUI. Of course, calling ComfyUI from this program is also very straightforward.

Configure startup parameters:
- Add `--enable-cors-header=*` to your ComfyUI startup options to allow StableCanvas to call the API through the web.

Configure workflow:
There are three ways to configure ComfyUI Workflow:

1. Use the built-in ComfyUI preset
   - This program comes with one ComfyUI preset, you just need to configure the backend address.
2. Customize your ComfyUI preset
   - With the convenience of ComfyUI, you can fully customize a new workflow for calling from this program.
   - You can refer to the following Workflow for further development:

![workflow](./workflows/workflow.png)

Note that during editing, preserve the `<%- slot_name %>` string slots, which will be replaced with the request content at the time of the API call.

> When you decide to configure ComfyUI on your own, keep in mind that ComfyUI does not support some common programming nodes... So you might need to configure three sets of workflows to use this program seamlessly. Details can be checked in the ComfyUI Demo code.

3. Write your custom call script
   - If you need a completely custom backend call (such as using your custom python backend), you can start from scratch and write the preset script.

More details will be introduced in the preset section of [welcome.en.md](./welcome.en.md).

# Contact Us
- :envelope: Support@StableCanvas.com