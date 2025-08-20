---
url: "https://app.grapesjs.com/docs-sdk/plugins/canvas/full-size"
title: "Full Size | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/canvas/full-size#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` `email` |
| Plan | Startup plan |

Enable full-size device in the editor canvas, independent of your screen size. Perfect for designing large templates seamlessly.

![Full size screen](https://app.grapesjs.com/docs-sdk/assets/images/full-size-plugin-04c7d39d04a596518bf6563730bc07a3.webp)

Install the Studio SDK plugins package:

- npm
- pnpm
- yarn
- CDN

```codeBlockLines_AdAo
npm i @grapesjs/studio-sdk-plugins

```

Import and use the plugin in your project:

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { canvasFullSize } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        canvasFullSize\
      ],
  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/canvas/full-size\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| deviceMaxWidth | number | Default max width for the devices.<br>**Default** <br>```codeBlockLines_AdAo<br>1200<br>``` |
| deviceMinHeigth | number | Default min height for the devices.<br>**Default** <br>```codeBlockLines_AdAo<br>500<br>``` |
| canvasOffsetY | number | Offset for the canvas on the Y axis (margin between the canvas and content frame).<br>**Default** <br>```codeBlockLines_AdAo<br>30<br>``` |
| canvasOffsetX | number | Offset for the canvas on the X axis (margin between the canvas and content frame).<br>**Default** <br>```codeBlockLines_AdAo<br>50<br>``` |
| canvasTransition | number | Transition time for the canvas when the screen size changes (in seconds).<br>**Default** <br>```codeBlockLines_AdAo<br>0.3<br>``` |
| frameBorderRadius | number | Border radius for the content frame (in px).<br>**Default** <br>```codeBlockLines_AdAo<br>5<br>``` |
| frameTransition | number | Transition time for the content frame when the device changes (in seconds).<br>**Default** <br>```codeBlockLines_AdAo<br>0.3<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/canvas/full-size#plugin-options)