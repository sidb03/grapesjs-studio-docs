---
url: "https://app.grapesjs.com/docs-sdk/plugins/layout/sidebar-buttons"
title: "Sidebar Buttons | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/layout/sidebar-buttons#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` `email` |
| Plan | Free plan |

Add a customizable sidebar to centralize key editor actions. Easily toggle panels like Layers, Blocks, and Assets for a more efficient workflow. Comes with predefined responsive layouts for tablet and mobile screens.

![Sidebar Buttons plugin](https://app.grapesjs.com/docs-sdk/assets/images/sidebar-buttons-plugin-5e91d7871678b69427f3bb05fd60fdfc.webp)

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
import { layoutSidebarButtons } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        layoutSidebarButtons\
      ],
  }}
/>

```

### Customization [​](https://app.grapesjs.com/docs-sdk/plugins/layout/sidebar-buttons\#customization "Direct link to Customization")

You can customize the sidebar by adding your own buttons or extending the existing ones. Each responsive breakpoint can have its own unique set of controls to optimize the editing experience across different screen sizes.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { layoutSidebarButtons } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        layoutSidebarButtons.init({\
          sidebarButton({ id, buttonProps, breakpoint, createSidebarButton }) {\
            // Skip updates to the sidebar buttons for layout with a breakpoint (tablet/mobile)\
            if (breakpoint) return buttonProps;\
\
            // Customize the button for 'panelBlocks'\
            if (id === 'panelBlocks') {\
              return createSidebarButton({\
                layoutComponent: {\
                  type: 'panelBlocks',\
                  symbols: false,\
                  blocks: ({ blocks }) => blocks.filter(block => block.category?.getLabel() === 'Basic')\
                }\
              });\
              // Customize the button for 'panelPagesLayers'\
            } else if (id === 'panelPagesLayers') {\
              return createSidebarButton({\
                label: 'Layers',\
                layoutComponent: { type: 'panelLayers' }\
              });\
              // Show 'panelAssets' with another placer\
            } else if (id === 'panelAssets') {\
              return createSidebarButton({\
                layoutCommand: { placer: { type: 'absolute', position: 'right' } }\
              });\
              // Hide 'panelGlobalStyles' button\
            } else if (id === 'panelGlobalStyles') {\
              return null;\
            }\
\
            // Return default button props\
            return buttonProps;\
          },\
          sidebarButtons({ breakpoint, sidebarButtons, createSidebarButton }) {\
            // Add a custom button for the default layout\
            return !breakpoint\
              ? [\
                  ...sidebarButtons,\
                  createSidebarButton({\
                    id: 'customButton',\
                    icon: 'flare',\
                    layoutCommand: {\
                      header: false,\
                      placer: { type: 'dialog', title: 'Custom Dialog' }\
                    },\
                    layoutComponent: {\
                      type: 'custom',\
                      render: () => 'Custom layout'\
                    }\
                  })\
                ]\
              : sidebarButtons;\
          },\
          rootLayout({ breakpoint, rootLayout, sidebarButtons, createSidebarButton }) {\
            if (breakpoint === 768) {\
              return {\
                ...rootLayout,\
                children: [\
                  { type: 'canvas' },\
                  {\
                    type: 'sidebarBottom',\
                    style: { padding: '0 5px', alignItems: 'center', gap: 10, minHeight: 39 },\
                    children: [\
                      ...sidebarButtons,\
                      createSidebarButton({\
                        id: 'customButton',\
                        icon: 'flare',\
                        label: 'Custom',\
                        layoutCommand: { placer: { type: 'absolute', position: 'right' } },\
                        layoutComponent: {\
                          type: 'custom',\
                          render: () => 'Custom layout'\
                        }\
                      }),\
                      { type: 'devices', style: { width: 100, marginLeft: 'auto' } }\
                    ]\
                  }\
                ]\
              };\
            }\
            return rootLayout;\
          }\
        }),\
      ],
  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/layout/sidebar-buttons\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| sidebarButton | function | Extend each sidebar button per breakpoint.<br>**Example** <br>```codeBlockLines_AdAo<br>sidebarButton: ({ id, buttonProps, breakpoint, createSidebarButton }) => ({<br>  ...buttonProps,<br>  // custom icon for panelBlocks button<br>  icon: buttonProps.id === 'panelBlocks' ? '<svg ...>' : buttonProps.icon,<br>})<br>``` |
| sidebarButtons | function | Add or filter the resultant buttons per breakpoint.<br>**Example** <br>```codeBlockLines_AdAo<br>sidebarButtons: ({ breakpoint, sidebarButtons, createSidebarButton }) => {<br>  // Add a new button for the default layout<br>  return !breakpoint ? [...sidebarButtons, createSidebarButton({...})] sidebarButtons;<br>}<br>``` |
| rootLayout | function | Customize the resultant root layout per breakpoint.<br>**Example** <br>```codeBlockLines_AdAo<br>rootLayout({ breakpoint, rootLayout, sidebarButtons, createSidebarButton }) {<br> if (breakpoint === 768) {<br>   return {<br>     ...rootLayout,<br>     children: [<br>       { type: 'canvas' },<br>       { type: 'sidebarBottom', children: [ ...sidebarButtons, createSidebarButton({...}) ] }<br>     ]<br>   };<br> }<br> return rootLayout;<br>}<br>``` |
| breakpointTablet | number | Custom tablet breakpoint.<br>**Default** <br>```codeBlockLines_AdAo<br>1024<br>``` |
| breakpointMobile | number | Custom mobile breakpoint.<br>**Default** <br>```codeBlockLines_AdAo<br>768<br>``` |

- [Customization](https://app.grapesjs.com/docs-sdk/plugins/layout/sidebar-buttons#customization)
- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/layout/sidebar-buttons#plugin-options)