---
url: "https://app.grapesjs.com/docs-sdk/plugins/rte/tinymce"
title: "TinyMCE | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/rte/tinymce#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` `email` |
| Plan | Startup plan |

Replace the default RTE with [TinyMCE 6](https://www.tiny.cloud/docs/tinymce/6/) (latest MIT).

![RTE TinyMCE plugin](https://app.grapesjs.com/docs-sdk/assets/images/rte-tinymce-plugin-7f28515bad46ee0680da049c2677838c.png)

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
import { rteTinyMce } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        rteTinyMce.init({\
          enableOnClick: true,\
          // Custom TinyMCE configuration\
          loadConfig: ({ component, config }) => {\
            const demoRte = component.get('demorte');\
            if (demoRte === 'fixed') {\
              return {\
                toolbar:\
                  'bold italic underline strikethrough | alignleft aligncenter alignright alignjustify | link image media',\
                fixed_toolbar_container_target: document.querySelector('.rteContainer')\
              };\
            } else if (demoRte === 'quickbar') {\
              return {\
                plugins: `${config.plugins} quickbars`,\
                toolbar: false,\
                quickbars_selection_toolbar: 'bold italic underline strikethrough | quicklink image'\
              };\
            }\
            return {};\
          }\
        })\
      ],
      layout: {
        default: {
          type: 'row',
          style: { height: '100%' },
          children: [\
            { type: 'sidebarLeft' },\
            {\
              type: 'column',\
              style: { flexGrow: 1 },\
              children: [\
                { type: 'sidebarTop' },\
                { type: 'canvas' },\
                // Empty container for the fixed RTE toolbar\
                { type: 'row', className: 'rteContainer', style: { justifyContent: 'center' } }\
              ]\
            },\
            { type: 'sidebarRight' }\
          ]
        },
      },
      project: {
        default: {
          pages: [\
            {\
              name: 'Home',\
              component: `\
              <div data-gjs-type="text">\
                <h1>Default configuration</h1>\
                Text content start\
                <div>\
                  Block content with <em>emphasis</em>, <b>bold text</b>, <a href="#some-link" target="_blank">link</a>.\
                </div>\
                <ul>\
                  <li>List item 1</li>\
                  <li>List item 2</li>\
                </ul>\
                <ol>\
                  <li>Ordered list item 1</li>\
                  <li>Ordered list item 1</li>\
                </ol>\
                Text content end\
              </div>\
\
              <div data-gjs-type="text" data-gjs-demorte="fixed">\
                <h1>Fixed position</h1>\
                Text content start\
                <div>\
                  Block content with <em>emphasis</em>, <b>bold text</b>, <a href="#some-link" target="_blank">link</a>.\
                </div>\
                <ul>\
                  <li>List item 1</li>\
                  <li>List item 2</li>\
                </ul>\
                <ol>\
                  <li>Ordered list item 1</li>\
                  <li>Ordered list item 1</li>\
                </ol>\
                Text content end\
              </div>\
\
              <div data-gjs-type="text" data-gjs-demorte="quickbar">\
                <h1>Quickbar</h1>\
                Text content start\
                <div>\
                  Block content with <em>emphasis</em>, <b>bold text</b>, <a href="#some-link" target="_blank">link</a>.\
                </div>\
                <ul>\
                  <li>List item 1</li>\
                  <li>List item 2</li>\
                </ul>\
                <ol>\
                  <li>Ordered list item 1</li>\
                  <li>Ordered list item 1</li>\
                </ol>\
                Text content end\
              </div>\
              `\
            },\
          ]
        },
      }


  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/rte/tinymce\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| cdnScript | string | CDN scripts to load dynamically in case the library is not available.<br>**Default** <br>```codeBlockLines_AdAo<br>https://cdn.jsdelivr.net/npm/tinymce@6.8.5/tinymce.min.js<br>``` |
| enableOnClick | boolean | Enable RTE on click of the text component, instead of the default double-click.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| skipCustomTheme | boolean | Skip adding custom CSS styles for the toolbar based on the Studio theme.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| loadConfig | function | Pass your custom configuration to the TinyMCE editor.<br>**Example**<br>```codeBlockLines_AdAo<br>({ config, editor, component }) => {<br> return {<br>  toolbar: 'bold italic underline strikethrough',<br>  // ...<br> }<br>}<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/rte/tinymce#plugin-options)