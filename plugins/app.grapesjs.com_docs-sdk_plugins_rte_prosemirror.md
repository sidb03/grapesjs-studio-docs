---
url: "https://app.grapesjs.com/docs-sdk/plugins/rte/prosemirror"
title: "ProseMirror | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/rte/prosemirror#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` `email` |
| Plan | Free plan |

Replace the default RTE with the [ProseMirror](https://prosemirror.net/) editor.

![RTE ProseMirror plugin](https://app.grapesjs.com/docs-sdk/assets/images/rte-pm-plugin-5ff2a9134c81a9366355ad9d8f1f42b9.png)

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
import { rteProseMirror } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        rteProseMirror?.init({\
          plugins: ({ plugins }) => [\
            // use the default plugins\
            ...plugins,\
            // Custom ProseMirror plugin\
            // new Plugin({ appendTransaction(tr) {} })\
          ],\
          toolbar({ items, component, layouts, proseMirror, commands }) {\
            const { view } = proseMirror;\
            return [\
              // Default toolbar items\
              ...items,\
              // Leverage default layouts\
              layouts.separator,\
              // Add custom Layout components\
              {\
                id: 'customButton',\
                type: 'button',\
                icon: 'flare',\
                tooltip: 'Replace selection',\
                onClick: () => {\
                  // leverage prebuilt commands\
                  const currText = commands.text.selected();\
                  const newText = `Selected: ${currText}, Component name: ${component.getName()}`;\
                  // Use the ProseMirror API\
                  const { state, dispatch } = view;\
                  dispatch(state.tr.replaceSelectionWith(state.schema.text(newText)));\
                }\
              },\
              {\
                id: 'variables',\
                type: 'selectField',\
                emptyState: 'Variables',\
                options: [\
                  { id: '{{ firstName }}', label: 'First Name' },\
                  { id: '{{ lastName }}', label: 'Last Name' }\
                ],\
                onChange: ({ value }) => commands.text.replace(value, { select: true })\
              }\
            ];\
          }\
        })\
      ],
      project: {
        default: {
          pages: [\
            {\
              name: 'Home',\
              component: `\
                <div data-gjs-type="text">\
                  <h1>RTE Plugin</h1>\
                  Text content start\
                  <div>\
                    Block content with <em>emphasis</em>\
                    , <b>bold text</b>\
                    , <a href="#some-link" target="_blank">link</a>\
                    , <img src="https://picsum.photos/seed/1/10/10"> image.\
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
            `},\
          ]
        },
      }

  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/rte/prosemirror\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| enableOnClick | boolean | Enable the RTE on click of the text component, instead of the default double-click.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| disableOnEsc | boolean | Disable the RTE on pressing the Escape key.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| schema | function | Extend the default ProseMirror schema.<br>**Example**<br>```codeBlockLines_AdAo<br>({ schema }) => {<br> // add additional nodes and return a new schema<br> return new Schema({<br>   nodes: schema.spec.nodes.append({...}),<br>   marks: schema.spec.marks<br> });<br>}<br>``` |
| plugins | function | Pass additional ProseMirror plugins.<br>**Example**<br>```codeBlockLines_AdAo<br>({ plugins }) => [<br>   // use the default plugins<br>   ...plugins,<br>   // pass your plugins<br> ]<br>``` |
| toolbar | function | Customize the toolbar items.<br>**Example** <br>```codeBlockLines_AdAo<br>toolbar({ items, component, proseMirror }) {<br>   const { view } = proseMirror;<br>   return [<br>     // use the default toolbar items<br>     ...items,<br>     {<br>       id: 'customButton',<br>       type: 'button',<br>       icon: 'flare',<br>       onClick: () => {...}<br>     }<br>   ];<br> }<br>``` |
| onEnter | function | Custom function to handle the Enter key press.<br>**Example** <br>```codeBlockLines_AdAo<br>onEnter({ commands, component }) {<br> if (component.is('link')) {<br>   // Create always a break for links<br>   commands.text.createBreak();<br>   return true;<br> }<br>}<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/rte/prosemirror#plugin-options)