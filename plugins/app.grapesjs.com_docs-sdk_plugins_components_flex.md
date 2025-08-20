---
url: "https://app.grapesjs.com/docs-sdk/plugins/components/flex"
title: "Flex Columns | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/components/flex#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` `email` |
| Plan | Startup plan |

This plugin provides a set of flexible layout blocks based on CSS Flexbox, making it easy to structure content. It allows users to create responsive row/column layouts with intuitive controls to dynamically resize columns and adjust gaps directly in the canvas.

![Flex Columns plugin](https://app.grapesjs.com/docs-sdk/assets/images/flex-plugin-0f167090e8960c40bb501981e9365e8f.webp)

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
import { flexComponent } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        flexComponent?.init({\
          // ...options\
        })\
      ],
      project: {
        default: {
          pages: [\
            {\
              name: 'Home',\
              component: `\
              <div style="padding: 2rem">\
                <h1>Horizontal</h1>\
                <div data-gjs-type="flex-row">\
                  <div data-gjs-type="flex-column" style="flex-basis: 50%"></div>\
                  <div data-gjs-type="flex-column" style="flex-basis: 50%"></div>\
                </div>\
                <div data-gjs-type="flex-row" style="gap: 2%">\
                  <div data-gjs-type="flex-column" style="flex-basis: 32%"></div>\
                  <div data-gjs-type="flex-column" style="flex-basis: 32%"></div>\
                  <div data-gjs-type="flex-column" style="flex-basis: 32%"></div>\
                </div>\
                <div data-gjs-type="flex-row">\
                  <div data-gjs-type="flex-column" style="flex-basis: 20%"></div>\
                  <div data-gjs-type="flex-column" style="flex-basis: 20%"></div>\
                  <div data-gjs-type="flex-column" style="flex-basis: 20%"></div>\
                  <div data-gjs-type="flex-column" style="flex-basis: 40%"></div>\
                </div>\
\
                <h1>Snapping</h1>\
                <div data-gjs-type="flex-row" data-gjs-snap="true">\
                  <div data-gjs-type="flex-column" style="flex-basis: 8.33%"></div>\
                  <div data-gjs-type="flex-column" style="flex-basis: 91.66%"></div>\
                </div>\
                <div data-gjs-type="flex-row" data-gjs-snap="true" data-gjs-snap-divisions="4">\
                  <div data-gjs-type="flex-column" style="flex-basis: 25%"></div>\
                  <div data-gjs-type="flex-column" style="flex-basis: 75%"></div>\
                </div>\
\
                <h1>Vertical</h1>\
                <div data-gjs-type="flex-row" style="gap: 2%; height: 300px">\
                  <div data-gjs-type="flex-column" style="flex-basis: 32%; padding: 1rem">\
                    <div data-gjs-type="flex-row" style="flex-direction: column; height: 100%">\
                      <div data-gjs-type="flex-column" style="flex-basis: 50%"></div>\
                      <div data-gjs-type="flex-column" style="flex-basis: 50%"></div>\
                    </div>\
                  </div>\
                  <div data-gjs-type="flex-column" style="flex-basis: 32%; padding: 1rem">\
                    <div data-gjs-type="flex-row" style="gap: 2%; flex-direction: column; height: 100%">\
                      <div data-gjs-type="flex-column" style="flex-basis: 32%"></div>\
                      <div data-gjs-type="flex-column" style="flex-basis: 32%"></div>\
                      <div data-gjs-type="flex-column" style="flex-basis: 32%"></div>\
                    </div>\
                  </div>\
                  <div data-gjs-type="flex-column" style="flex-basis: 32%; padding: 1rem">\
                    <div data-gjs-type="flex-row" data-gjs-snap="true" data-gjs-snap-divisions="5" style="flex-direction: column; height: 100%">\
                      <div data-gjs-type="flex-column" style="flex-basis: 20%"></div>\
                      <div data-gjs-type="flex-column" style="flex-basis: 80%"></div>\
                    </div>\
                  </div>\
                </div>\
              <div>\
\
              <style>\
                body { font-family: system-ui;}\
                h1 { text-align: center; }\
              </style>\
              `,\
            }\
          ]
        }
      }


  }}
/>

```

### Email projects [​](https://app.grapesjs.com/docs-sdk/plugins/components/flex\#email-projects "Direct link to Email projects")

The plugin also supports column snapping and resizing for the `email` project type.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { flexComponent } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        flexComponent?.init({\
          // Indicate the email project type\
          projectType: 'email'\
        })\
      ],
      project: {
        type: 'email',
        default: {
          pages: [\
            {\
              component: `\
                <mjml>\
                  <mj-body>\
                    <mj-section>\
                      <mj-column>\
                        <mj-text align="center" font-weight="700" font-size="25px">Resizable columns</mj-text>\
                      </mj-column>\
                    </mj-section>\
                    <mj-section>\
                      <mj-column width="50%"></mj-column>\
                      <mj-column width="50%"></mj-column>\
                    </mj-section>\
                    <mj-section>\
                      <mj-column width="25%"></mj-column>\
                      <mj-column width="75%"></mj-column>\
                    </mj-section>\
                    <mj-section>\
                      <mj-column>\
                        <mj-text align="center" font-weight="700" font-size="25px">Snapping</mj-text>\
                      </mj-column>\
                    </mj-section>\
                    <mj-section data-gjs-snap="true" data-gjs-snap-divisions="4">\
                      <mj-column width="75%"></mj-column>\
                      <mj-column width="25%"></mj-column>\
                    </mj-section>\
                  </mj-body>\
                </mjml>\
              `,\
            }\
          ]
        }
      }


  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/components/flex\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| blocks | [Block](https://grapesjs.com/docs/api/block.html) | Filter default layout blocks. Pass \`false\` to avoid adding layout blocks.<br>**Example**<br>```codeBlockLines_AdAo<br>({ blocks }) => {<br> // Remove the 1 Column block<br> return blocks.filter(block => block.label !== '1 Column');<br>}<br>``` |
| typeRow | string | Default component type for row.<br>**Default** <br>```codeBlockLines_AdAo<br>flex-row<br>``` |
| typeColumn | string | Default component type for column.<br>**Default** <br>```codeBlockLines_AdAo<br>flex-column<br>``` |
| extendTypeRow | boolean | Indicate if the existant \`typeRow\` component has to be extended.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| extendTypeColumn | boolean | Indicate if the existant \`typeColumn\` component has to be extended.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| minItemPercent | number | Minimum item percent size in percentage.<br>**Default** <br>```codeBlockLines_AdAo<br>5<br>``` |
| snapEnabled | boolean | Enable snapping by default.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| snapDivisions | number | Default divisions for snapping.<br>**Default** <br>```codeBlockLines_AdAo<br>12<br>``` |
| disableGapHandler | boolean | Disable gap handler.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| gapHandleSize | number | Gap size in px.<br>**Default** <br>```codeBlockLines_AdAo<br>3<br>``` |
| projectType | string | If you're using \`email\` project type in Studio SDK, you can set this option to \`email\` to active column resizing.<br>**Example**<br>```codeBlockLines_AdAo<br>"email"<br>```<br>**Default** <br>```codeBlockLines_AdAo<br>web<br>``` |
| getSize | function | Provide a custom handler for getting the column size.<br>**Example**<br>```codeBlockLines_AdAo<br>({ componentColumn }) => {<br> return parseInt(componentColumn.getStyle()['flex-basis'], 10);<br>}<br>``` |
| getGap | function | Provide a custom handler for getting the gap size.<br>**Example**<br>```codeBlockLines_AdAo<br>({ component }) => {<br> return parseInt(component.getStyle()['gap'], 10);<br>}<br>``` |
| getParentSize | function | Provide a custom handler for getting the parent size.<br>**Example**<br>```codeBlockLines_AdAo<br>({ component, isVertical }) => {<br> return component.getEl().clientWidth || 0;<br>}<br>``` |
| isParentVertical | function | Provide a custom handler for checking if the parent layout is in vertical layout.<br>**Example**<br>```codeBlockLines_AdAo<br>({ component }) => {<br> return true;<br>}<br>``` |
| setSize | function | Provide a custom handler for setting the column size.<br>**Example**<br>```codeBlockLines_AdAo<br>({ component, sizeValue, partial }) => {<br> component.addStyle({ 'flex-basis': sizeValue }, { partial });<br>}<br>``` |
| setGap | function | Provide a custom handler for setting the gap size.<br>**Example**<br>```codeBlockLines_AdAo<br>({ component, gapValue, partial }) => {<br> component.addStyle({ gap: gapValue }, { partial });<br>}<br>``` |

- [Email projects](https://app.grapesjs.com/docs-sdk/plugins/components/flex#email-projects)
- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/components/flex#plugin-options)