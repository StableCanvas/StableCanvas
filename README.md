# StableCanvas
Since StableCanvas is not fully open-source, this repository serves as a place for bug reports, feature requests, and general discussion.

[https://beta.stablecanvas.com/](https://beta.stablecanvas.com/)

# Features
- Simple, intuitive inpaint and outpint
- Infinite canvas! Use your creativity!
- Smooth and silky view window control
- Support multiple backends, multiple servers  to speed up your painting
- Edit pictures, scale pictures, crop pictures, edit layers, support various editing, and have most of the available operations close to Photoshop
- Historical operation management, easy undo, easy redo
- Extension-base system, has same extension-system as `vscode`
- Full-featured palette, color catcher
- Simple and efficient Healing algorithm to help you remove the object
- ControlNet extensions support. (https://github.com/Mikubill/sd-webui-controlnet)
- Pose Editor.
- (WIP) Complete project configuration, save your actions as ".scd" just like ".psd" files

# Usage
If it is used locally, the usage is very simple. Run the local webui program with parameters:
```
--api --cors-allow-origins="https://beta.stablecanvas.com"
```
or (When you need to test dev (non-alpha) version, please use * directly to set cors)
```
--api --cors-allow-origins=*
```

then you can start creating as I did in the demo video

tips: recommend using inpainting model

## Shortcuts

### move view window
- w/a/s/d: move canvas viewable window
- ctrl + `mouse wheel`: to move the visible window horizontally
- shift + `mouse wheel`: move the visible window vertically
- alt + `mouse wheel`: zoom visual window

- ctrl + z: undo

# Community
- youtube: https://www.youtube.com/@stablecanvasdev
- discord: https://discord.gg/DneNQsYccf

# Contact Us
- :envelope: Support@StableCanvas.com
