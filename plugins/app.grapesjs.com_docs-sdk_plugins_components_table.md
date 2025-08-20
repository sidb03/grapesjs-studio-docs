---
url: "https://app.grapesjs.com/docs-sdk/plugins/components/table"
title: "Table | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/components/table#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` |
| Plan | Startup plan |

Add table component with additional functionalities.

![Table plugin](https://app.grapesjs.com/docs-sdk/assets/images/table-plugin-f8c7596df01f9e12fb5d94a9de4ee502.png)

Install the Studio SDK plugins package:

- npm
- pnpm
- yarn
- CDN

```codeBlockLines_AdAo
npm i @grapesjs/studio-sdk-plugins

```

Import and use the table plugin in your project:

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { tableComponent } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        tableComponent.init({\
          block: { category: 'Extra', label: 'My Table' }\
        })\
      ],
      project: {
        default: {
          pages: [\
            {\
              name: 'Home',\
              component: `\
                <h1>Table plugin</h1>\
                <table>\
                  <tbody>\
                    <tr>\
                      <td>Cell 0:0</td>\
                      <td></td>\
                    </tr>\
                    <tr>\
                      <td></td>\
                      <td>Cell 1:1</td>\
                    </tr>\
                  </tbody>\
                </table>\
            `},\
          ]
        },
      }

  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/components/table\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| block |  | Block options for the table component. See https://grapesjs.com/docs/api/block.html#properties for more information.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "My Table"<br>}<br>``` |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| openSettings | function | Customize the layout for table component setting actions, which by default are shown in a popover.<br>**Example**<br>```codeBlockLines_AdAo<br>({ editor, layoutProps }) => {<br>  // open settings in a dialog<br>  editor.runCommand('studio:layoutToggle', {<br>    ...layoutProps,<br>    header: false,<br>    style: { marginLeft: -20, marginRight: -20 },<br>    placer: { type: 'dialog', title: layoutProps.header?.label },<br>  });<br>}<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/components/table#plugin-options)