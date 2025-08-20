---
url: "https://app.grapesjs.com/docs-sdk/plugins/components/dialog"
title: "Dialog | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/components/dialog#__docusaurus_skipToContent_fallback)

On this page

warning

This component is a work in progress.

Add a dialog component with additional functionalities.

![Dialog plugin](https://app.grapesjs.com/docs-sdk/assets/images/dialog-plugin-cd8fa2215a71133288aaa786cf730544.png)

Install the Studio SDK plugins package:

- npm
- pnpm
- yarn
- CDN

```codeBlockLines_AdAo
npm i @grapesjs/studio-sdk-plugins

```

To use the Dialog plugin, you need to import it into your project:

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { dialogComponent } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        dialogComponent.init({\
          block: { category: 'Extra', label: 'My Dialog' }\
        })\
      ]

  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/components/dialog\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| block | object | Block options for the dialog component. See https://grapesjs.com/docs/api/block.html#properties for more information.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "Popup"<br>}<br>``` |

### Component properties [​](https://app.grapesjs.com/docs-sdk/plugins/components/dialog\#component-properties "Direct link to Component properties")

| Property | Type | Description |
| --- | --- | --- |
| closeWhenPressingX\* | boolean | Whether the dialog should close when pressing the X button. |
| closeWhenPressingEsc\* | boolean | Whether the dialog should close when pressing the Escape key. |
| openWhenLeavingWindow\* | boolean | Whether the dialog should open when leaving the window. |
| openWhenScrollingToLevel\* | string | Whether the dialog should open when scrolling to a specific level.<br>**Example**<br>```codeBlockLines_AdAo<br>"500"<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/components/dialog#plugin-options)
- [Component properties](https://app.grapesjs.com/docs-sdk/plugins/components/dialog#component-properties)