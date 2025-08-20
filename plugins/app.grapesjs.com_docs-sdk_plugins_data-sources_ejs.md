---
url: "https://app.grapesjs.com/docs-sdk/plugins/data-sources/ejs"
title: "EJS | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/data-sources/ejs#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` `email` |
| Plan | Startup plan |

This plugin enables importing and exporting your [Data Sources](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview) records using the [EJS](https://ejs.co/) template engine.

![EJS plugin](https://app.grapesjs.com/docs-sdk/assets/images/ejs-plugin-7a6c7d8ce79ff38bf50fa81292ec3772.webp)

Install the Studio SDK plugins package:

- npm
- pnpm
- yarn
- CDN

```codeBlockLines_AdAo
npm i @grapesjs/studio-sdk-plugins

```

The example below demonstrates how the editor can detect and extract EJS expressions from the content, and how it preserves them when exporting.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { dataSourceEjs } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
    // ...
    plugins: [\
      dataSourceEjs\
    ],
    dataSources: {
      globalData: {
        user: { firstName: 'Alice', isCustomer: true },
        products: [\
          { name: 'Laptop Pro X15', price: 1200.0 },\
          { name: 'Wireless Mouse M2', price: 25.99 }\
        ]
      },
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          { type: 'sidebarLeft' },\
          {\
            type: 'canvasSidebarTop',\
            sidebarTop: {\
              rightContainer: {\
                buttons: ({ items }) => [{\
                  ...items.find(item => item.id === 'showCode'),\
                  variant: 'outline',\
                  label: 'Show code'\
                }],\
              },\
            },\
          },\
          { type: 'sidebarRight' },\
        ],
      },
    },
    project: {
      default: {
        pages: [\
          {\
            name: 'Demo',\
            component: `\
              <h1>Variable</h1>\
              <div>Hello <%= globalData.user.data.firstName %></div>\
\
              <h1>Condition</h1>\
              <div>\
                Hey, <% if (globalData.user.data.isCustomer) { %>\
                  welcome back  <%= globalData.user.data.firstName %>!\
                <% } else { %>\
                  please register to see more!\
                <% } %>\
              </div>\
\
              <h1>Collection</h1>\
\
              <% globalData.products.data.forEach(product => { %>\
                <div>\
                  <b>Product Name</b>: <%= product.name %> - <b>Price</b>: <%= product.price %>\
                </div>\
              <% }) %>\
            `,\
          }\
        ]
      },

    },

  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/data-sources/ejs\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/data-sources/ejs#plugin-options)