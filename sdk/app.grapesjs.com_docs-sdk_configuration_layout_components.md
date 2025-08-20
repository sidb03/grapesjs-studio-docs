---
url: "https://app.grapesjs.com/docs-sdk/configuration/layout/components"
title: "Layout Components | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/layout/components#__docusaurus_skipToContent_fallback)

On this page

Below is a list of layout components available to help you compose your editor interface.

## Row [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#row "Direct link to Row")

A component that arranges its child components in a horizontal row layout.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    layout: {
      default: {
        type: 'column',
        style: { height: '100%' },
        children: [\
          {\
            type: 'row',\
            className: 'custom-classname-row',\
            style: { color: 'white', padding: 3, height: 30, gap: 10 },\
            children: [\
              { type: 'text', style: { backgroundColor: 'green' }, content: 'Text 1' },\
              { type: 'text', style: { backgroundColor: 'green' }, content: 'Text 2' },\
              'Text 3'\
            ]\
          },\
          { type: 'canvas' },\
          { type: 'row', children: 'Footer text' }\
        ]
      },
    }
  }}
/>

```

You can easily customize the component, as with all layout components, by applying `className` and `style` properties.

#### Row properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#row-properties "Direct link to Row properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | row | Type of the layout component. |
| alignItems | string | The alignment of inner components along the cross axis.<br>**Example**<br>```codeBlockLines_AdAo<br>"start" | "end" | "center" | "baseline" | "stretch"<br>``` |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| children | Layout component | The children layout components.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| full | boolean | If true, the component will take up the full available space.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| gap | number | The gap between inner components.<br>**Example** <br>```codeBlockLines_AdAo<br>10<br>``` |
| grow | boolean | If true, the component will grow to fill available space.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| height | string, number | The height of the component.<br>**Example** <br>```codeBlockLines_AdAo<br>100<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| justifyContent | string | The alignment of inner components along the main axis.<br>**Example**<br>```codeBlockLines_AdAo<br>"start" | "end" | "center" | "between" | "around" | "evenly"<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |
| width | string, number | The width of the component.<br>**Example** <br>```codeBlockLines_AdAo<br>100<br>``` |

## Column [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#column "Direct link to Column")

Similar to the [`row`](https://app.grapesjs.com/docs-sdk/configuration/layout/components#row), the column component arranges its children in a vertical layout.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'column',\
            style: { width: '200px' },\
            children: ['Text 1', 'Text 2']\
          },\
          { type: 'canvas' },\
          {\
            type: 'column',\
            style: { width: '200px' },\
            children: 'Text 1'\
          }\
        ]
      },
    }
  }}
/>

```

#### Column properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#column-properties "Direct link to Column properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | column | Type of the layout component. |
| alignItems | string | The alignment of inner components along the cross axis.<br>**Example**<br>```codeBlockLines_AdAo<br>"start" | "end" | "center" | "baseline" | "stretch"<br>``` |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| children | Layout component | The children layout components.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| full | boolean | If true, the component will take up the full available space.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| gap | number | The gap between inner components.<br>**Example** <br>```codeBlockLines_AdAo<br>10<br>``` |
| grow | boolean | If true, the component will grow to fill available space.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| height | string, number | The height of the component.<br>**Example** <br>```codeBlockLines_AdAo<br>100<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| justifyContent | string | The alignment of inner components along the main axis.<br>**Example**<br>```codeBlockLines_AdAo<br>"start" | "end" | "center" | "between" | "around" | "evenly"<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |
| width | string, number | The width of the component.<br>**Example** <br>```codeBlockLines_AdAo<br>100<br>``` |

## Text [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#text "Direct link to Text")

A basic text element that can be added to components accepting children. Text can be included either directly as a string or as an object `{ type: 'text', content: '...' }`. For usage examples, see the configurations above.

#### Text properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#text-properties "Direct link to Text properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | text | Type of the layout component. |
| content\* | string | Content of the text.<br>**Example**<br>```codeBlockLines_AdAo<br>"Hello, World!"<br>``` |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| children | Layout component | The children layout components.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## Tabs [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#tabs "Direct link to Tabs")

A component that organizes content into a tabbed layout, enabling users to switch between different sections.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    project: {
      default: {
        pages: [{ name: 'Home', component: '<h1>Add dynamic tab by selecting the heading component</h1><div>Some content</div>'}]
      }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          { type: 'canvas' },\
          {\
            type: 'column',\
            style: { width: 300 },\
            children: {\
              type: 'tabs',\
              value: 'tab2', // default selected tab\
              tabs: [ // tab id and label are required\
                { id: 'tab1', label: 'Tab 1', children: ['Content Tab 1'] },\
                { id: 'tab2', label: 'Tab 2', children: 'Content Tab 2' }\
              ],\
              editorEvents: { // Update tabs state based on editor events\
                'component:selected': ({ editor, state, setState }) => {\
                  const customTabId = 'tabHeading';\
                  const initialTabs = state.tabs?.filter(tab => tab.id !== customTabId) || [];\
\
                  if (editor.getSelected()?.get('type') === 'heading') {\
                    setState({\
                      value: customTabId,\
                      tabs: [...initialTabs, { id: 'tabHeading', label: 'Heading', children: ['Selected heading'] }]\
                    });\
                  } else {\
                    setState({ tabs: initialTabs });\
                  }\
                }\
              }\
            }\
          }\
        ]
      },

    }
  }}
/>

```

Each tab within the `tabs` property must include an `id` and `label` property. Additionally, the `value` property can be set to specify the default selected tab.

#### Tabs properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#tabs-properties "Direct link to Tabs properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | tabs | Type of the layout component. |
| tabs\* | array | Tabs configuration.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  {<br>    "id": "tab1",<br>    "label": "Tab 1",<br>    "children": [<br>      "Content Tab 1"<br>    ]<br>  }<br>]<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| editorEvents | object | Update layout component state based on editor events.<br>**Example**<br>```codeBlockLines_AdAo<br>{ 'component:selected': ({ setState, editor }) => setState({ className: 'custom-cls-' + editor.getSelected().getId() }) }<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| onChange | function | The function to call when the field value changes.<br>**Example**<br>```codeBlockLines_AdAo<br>({ value, setState }) => setState({ value });<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |
| value | string | Tab id value to select on initial render.<br>**Example**<br>```codeBlockLines_AdAo<br>"tab1"<br>``` |
| variant | string | Variant of the tabs.<br>**Example**<br>```codeBlockLines_AdAo<br>"pills"<br>``` |

## Button [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#button "Direct link to Button")

A button component that can be used to trigger actions.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'column',\
            style: { width: 100, justifyContent: 'center', alignItems: 'center', gap: 5 },\
            children: [\
              {\
                type: 'button',\
                label: 'Button',\
                variant: 'outline',\
                onClick: () => alert('Button clicked')\
              },\
              {\
                type: 'button',\
                label: 'Button',\
                icon: 'close',\
                size: 's',\
                tooltip: 'Button tooltip',\
                style: { padding: 5, backgroundColor: 'red', color: 'white' },\
                onClick: ({ editor }) => alert('Num of pages: ' + editor.Pages.getAll().length)\
              },\
              {\
                type: 'button',\
                icon: '<svg viewBox="0 0 24 24"><path d="M12 2C11.5 2 11 2.19 10.59 2.59L2.59 10.59C1.8 11.37 1.8 12.63 2.59 13.41L10.59 21.41C11.37 22.2 12.63 22.2 13.41 21.41L21.41 13.41C22.2 12.63 22.2 11.37 21.41 10.59L13.41 2.59C13 2.19 12.5 2 12 2M11 7H13V13H11V7M11 15H13V17H11V15Z" style="fill: currentcolor;"></path></svg>',\
                variant: 'primary',\
                onClick: () => alert('Icon only button')\
              },\
              {\
                type: 'button',\
                icon: 'check',\
                active: true,\
                onClick: ({ state, setState }) => {\
                  const newActive = !state.active;\
                  alert('Is to activate?: ' + newActive);\
                  setState({ active: newActive });\
                }\
              }\
            ]\
          },\
          { type: 'canvas' }\
        ]
      },
    }
  }}
/>

```

The component accepts a `label` property for the button text, an `icon` that can be an SVG string or a [default icon name](https://app.grapesjs.com/docs-sdk/configuration/themes#default-icons) and additional properties to customize the button layout (eg. `variant`, `size`).

Inside the `onClick` property, you have access to the [GrapesJS `editor` instance](https://grapesjs.com/docs/api/editor.html), as well as the `state` and `setState`, which allow you to interact with the editor or modify the current button's state.

In the [Layout Commands](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#layout-commands) section, you'll find more examples of button usage for managing editor layouts.

#### Button properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#button-properties "Direct link to Button properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | button | Type of the layout component. |
| active | boolean | Indicates if the button is active.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| buttonType | string | HTML button type attribute.<br>**Example**<br>```codeBlockLines_AdAo<br>"button" | "submit" | "reset"<br>``` |
| children | Layout component | The children layout components.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| classNameBtn | string | Class name for the inner button element.<br>**Example**<br>```codeBlockLines_AdAo<br>"btn-class"<br>``` |
| disabled | boolean | Indicates whether the component is disabled.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| editorEvents | object | Events to be handled by the editor.<br>**Example**<br>```codeBlockLines_AdAo<br>{ 'component:add': ({ editor }) => { console.log('Component added'); } }<br>``` |
| icon | string | Icon to be displayed in the button.<br>**Example**<br>```codeBlockLines_AdAo<br>"close"<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| label | string | Label for the button, can be a ReactNode or a function returning a ReactNode.<br>**Example**<br>```codeBlockLines_AdAo<br>"Button"<br>``` |
| onClick | function | Click event handler for the button.<br>**Example**<br>```codeBlockLines_AdAo<br>({ event }) => { alert('Button clicked!'); }<br>``` |
| size | string | Size of the button.<br>**Example**<br>```codeBlockLines_AdAo<br>"x2s" | "xs" | "s" | "m" | "lg"<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |
| tooltip | string | Tooltip text for the button.<br>**Example**<br>```codeBlockLines_AdAo<br>"Click to submit"<br>``` |
| variant | string | Variant of the button style.<br>**Example**<br>```codeBlockLines_AdAo<br>"primary" | "outline" | "shallow"<br>``` |

## Devices [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#devices "Direct link to Devices")

Displays the available devices that users can switch between in the editor canvas for responsive style editing. The component is connected to the [GrapesJS Devices API](https://grapesjs.com/docs/api/device_manager.html).

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    layout: {
      default: {
        type: 'column',
        style: { height: '100%' },
        children: [\
          {\
            type: 'row',\
            style: { justifyContent: 'center', alignItems: 'center', gap: 5 },\
            children: [\
              {\
                type: 'button',\
                label: 'Add Random Device',\
                onClick: ({ editor }) => {\
                  const width = `${Math.floor(Math.random() * (1000 - 500 + 1) + 500)}px`;\
                  const name = `Random ${width}`;\
                  editor.Devices.add({ id: `random-${width}`, name, width });\
                  alert(`Created "${name}"`);\
                }\
              },\
              { type: 'devices', style: { width: '100px' } }\
            ]\
          },\
          { type: 'canvas' }\
        ]
      },
    }
  }}
/>

```

#### Devices properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#devices-properties "Direct link to Devices properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | devices | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## PanelPages [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelpages "Direct link to PanelPages")

Displays the pages of the project. The component is connected to the [GrapesJS Pages API](https://grapesjs.com/docs/api/pages.html) and the [Pages configuration](https://app.grapesjs.com/docs-sdk/configuration/pages).

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    project: {
      default: {
        pages: [{ name: 'Home', component: '<h1>Home page</h1>'}]
      }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'column',\
            children: {\
              type: 'panelPages',\
              style: { width: 300 },\
              header: { label: 'My pages', collapsible: false }\
            }\
          },\
          { type: 'canvas' }\
        ]
      },
    }
  }}
/>

```

#### PanelPages properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelpages-properties "Direct link to PanelPages properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | panelPages | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| header | object | Header of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| resizable | object | Resizable configuration of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## PanelPageSettings [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelpagesettings "Direct link to PanelPageSettings")

Displays the [Page Settings](https://app.grapesjs.com/docs-sdk/configuration/pages#settings) panel, allowing users to configure settings for individual pages.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    project: {
      default: {
        pages: [{ name: 'Home', component: '<h1>Home page</h1>'}]
      }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'column',\
            style: { width: 300 },\
            children: { type: 'panelPageSettings' }\
          },\
          { type: 'canvas' }\
        ]
      },
    }
  }}
/>

```

#### PanelPageSettings properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelpagesettings-properties "Direct link to PanelPageSettings properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | panelPageSettings | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| header | object | Header of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| page | [Page](https://grapesjs.com/docs/api/page.html) | The page to display settings for.<br>**Example** <br>```codeBlockLines_AdAo<br>editor.Pages.getSelected();<br>``` |
| resizable | object | Resizable configuration of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## PanelLayers [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panellayers "Direct link to PanelLayers")

Displays the layers of the currently selected page. The component is connected to the [GrapesJS Layers API](https://grapesjs.com/docs/api/layer_manager.html). enabling users to manage and organize their design layers efficiently.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    project: {
      default: {
        pages: [{ name: 'Home', component: '<h1>Home page</h1>'}]
      }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'column',\
            style: { width: 300 },\
            children: {\
              type: 'panelLayers',\
              header: {\
                label: 'Layers label',\
                icon: '<svg viewBox="0 0 24 24"><path d="M19,13H13V19H11V13H5V11H11V5H13V11H19V13Z"/></svg>'\
              }\
            }\
          },\
          { type: 'canvas' }\
        ]
      },
    }
  }}
/>

```

#### PanelLayers properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panellayers-properties "Direct link to PanelLayers properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | panelLayers | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| header | object | Header of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| resizable | object | Resizable configuration of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## PanelPagesLayers [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelpageslayers "Direct link to PanelPagesLayers")

Displays a combined panel that includes both the [PanelPages](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelpages) and [PanelLayers](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panellayers). This component provides a comprehensive view for managing project pages and their respective layers in a unified interface.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    project: {
      default: {
        pages: [{ name: 'Home', component: '<h1>Home page</h1>'}]
      }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'column',\
            children: {\
              type: 'panelPagesLayers',\
              resizable: { right: true, width: 300, height: '100%', minWidth: 200, maxWidth: 300 },\
              panelPagesProps: { header: { label: 'My Pages' } },\
              panelLayersProps: { header: { label: 'My Layers' } }\
            }\
          },\
          { type: 'canvas' }\
        ]
      },
    }
  }}
/>

```

#### PanelPagesLayers properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelpageslayers-properties "Direct link to PanelPagesLayers properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | panelPagesLayers | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| header | object | Header of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| panelLayersProps | [PanelLayersProps](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panellayers-properties) | Properties for the panel layers.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "header": {<br>    "label": "My Layers"<br>  }<br>}<br>``` |
| panelPagesProps | [PanelPagesProps](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelpages-properties) | Properties for the panel pages.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "header": {<br>    "label": "My Pages"<br>  }<br>}<br>``` |
| resizable | object | Resizable configuration of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## PanelBlocks [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelblocks "Direct link to PanelBlocks")

Displays the blocks panel, which is connected to the [GrapesJS Blocks API](https://grapesjs.com/docs/api/block_manager.html).

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'column',\
            style: { width: 300 },\
            children: [\
              {\
                type: 'button',\
                label: 'Add Random Block',\
                style: { margin: '10px auto' },\
                variant: 'outline',\
                onClick: ({ editor }) => {\
                  const id = Math.random().toString(36).substring(5);\
                  const name = `Block ${id}`;\
                  editor.Blocks.add(`block-${id}`, {\
                    label: name,\
                    category: 'Random',\
                    media: '<svg viewBox="0 0 24 24"><path d="M19,3H5C3.89,3 3,3.89 3,5V19A2,2 0 0,0 5,21H19A2,2 0 0,0 21,19V5C21,3.89 20.1,3 19,3M19,5V19H5V5H19Z" /></svg>',\
                    content: `<div>HTML from ${name}</div>`\
                  });\
                  alert(`Created "${name}"`);\
                }\
              },\
              {\
                type: 'panelBlocks',\
                header: { label: 'My blocks' }\
              }\
            ]\
          },\
          { type: 'canvas' }\
        ]
      },
    }
  }}
/>

```

#### PanelBlocks properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelblocks-properties "Direct link to PanelBlocks properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | panelBlocks | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| blocks | function | Filter blocks. |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| header | object | Header of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>``` |
| hideCategories | boolean | Whether to hide categories.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| itemLayout | function | Custom layout for rendering single blocks.<br>**Example** <br>```codeBlockLines_AdAo<br>itemLayout: ({ block, attributes }) => ({<br> type: 'column',<br> children: [<br>   { type: 'custom', render: () => block.getMedia() },<br>   { type: 'text', content: block.getLabel() }<br> ]<br>})<br>``` |
| resizable | object | Resizable configuration of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>``` |
| search | boolean | Whether to show search.<br>**Default** <br>```codeBlockLines_AdAo<br>true<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |
| symbols | boolean | Whether to show symbols.<br>**Default** <br>```codeBlockLines_AdAo<br>true<br>``` |

## PanelGlobalStyles [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelglobalstyles "Direct link to PanelGlobalStyles")

Displays Global Styles panel. The component is connected to the [Global Styles configuration](https://app.grapesjs.com/docs-sdk/configuration/global-styles).

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    project: {
      default: {
        pages: [{ name: 'Home', component: '<h1>Home page</h1>'}]
      }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'column',\
            style: { width: 300 },\
            children: [\
              {\
                type: 'panelGlobalStyles',\
                header: { label: 'Global Styles' }\
              }\
            ]\
          },\
          { type: 'canvas' }\
        ]
      },
    },
    globalStyles: {
      default: [\
        {\
          id: 'h1Color',\
          property: 'color',\
          field: 'color',\
          defaultValue: 'red',\
          selector: 'h1',\
          label: 'H1 color'\
        },\
      ]
    }
  }}
/>

```

#### PanelGlobalStyles properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelglobalstyles-properties "Direct link to PanelGlobalStyles properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | panelGlobalStyles | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| header | object | Header of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| resizable | object | Resizable configuration of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## PanelSelectors [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelselectors "Direct link to PanelSelectors")

Displays component selectors, providing users with the ability to choose the styling target. The component is connected to the [GrapesJS Selectors API](https://grapesjs.com/docs/api/selector_manager.html).

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    project: {
      default: {
        pages: [{ name: 'Home', component: '<h1>Home page</h1>'}]
      }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          { type: 'canvas' },\
          {\
            type: 'column',\
            style: { width: 300 },\
            children: {\
              type: 'panelSelectors',\
              style: { padding: 5 }\
            }\
          }\
        ]
      },
    }
  }}
/>

```

#### PanelSelectors properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelselectors-properties "Direct link to PanelSelectors properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | panelSelectors | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| header | object | Header of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| resizable | object | Resizable configuration of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>``` |
| stateSelector | boolean | Enable the state selector.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |
| styleCatalog | boolean | Enable the style catalog.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |

## PanelStyles [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelstyles "Direct link to PanelStyles")

Displays the styles of the currently selected component, allowing users to easily view and edit styles directly within the editor. The component is connected to the [GrapesJS Style Manager API](https://grapesjs.com/docs/api/style_manager.html).

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    project: {
      default: {
        pages: [{ name: 'Home', component: '<div>Select me</div>'}]
      }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          { type: 'canvas' },\
          {\
            type: 'column',\
            style: { width: 300 },\
            children: { type: 'panelStyles' }\
          }\
        ]
      },
    }
  }}
/>

```

#### PanelStyles properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelstyles-properties "Direct link to PanelStyles properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | panelStyles | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| header | object | Header of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| resizable | object | Resizable configuration of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## PanelProperties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelproperties "Direct link to PanelProperties")

Displays the properties of the currently selected component. The component is connected to the [GrapesJS Traits API](https://grapesjs.com/docs/modules/Traits.html).

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    project: {
      default: {
        pages: [{ name: 'Home', component: '<h1>Select me</h1>'}]
      }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          { type: 'canvas' },\
          {\
            type: 'column',\
            style: { width: 300 },\
            children: {\
              type: 'panelProperties',\
              style: { padding: 10 },\
            }\
          }\
        ]
      },
    }
  }}
/>

```

#### PanelProperties properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelproperties-properties "Direct link to PanelProperties properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | panelProperties | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| header | object | Header of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| resizable | object | Resizable configuration of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## PanelSidebarTabs [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelsidebartabs "Direct link to PanelSidebarTabs")

Displays combined tab views for [PanelSelectors](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelselectors), [PanelStyles](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelstyles) and [PanelProperties](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelproperties), allowing users to easily switch between different panel functionalities within the sidebar.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    project: {
      default: {
        pages: [{ name: 'Home', component: '<h1>Select me</h1>'}]
      }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          { type: 'canvas' },\
          {\
            type: 'column',\
            style: { width: 300 },\
            children: {\
              type: 'panelSidebarTabs'\
            }\
          }\
        ]
      },
    }
  }}
/>

```

#### PanelSidebarTabs properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelsidebartabs-properties "Direct link to PanelSidebarTabs properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | panelSidebarTabs | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| header | object | Header of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| resizable | object | Resizable configuration of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## PanelAssets [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelassets "Direct link to PanelAssets")

Displays project assets, providing users with access to various media and resources. The component is connected to the [GrapesJS Assets API](https://grapesjs.com/docs/api/assets.html).

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    gjsOptions: {
      storageManager: false,
      assetManager: {
        assets: Array(20).fill(0).map((_, i) => `https://picsum.photos/seed/${i}/300/300`)
      }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'column',\
            style: { width: 400 },\
            children: [\
              {\
                type: 'panelAssets',\
                header: { label: 'My Assets' },\
                content: {\
                  itemsPerRow: 2,\
                  header: { upload: false }\
                },\
                onSelect: ({ asset, editor }) => {\
                  const root = editor.getWrapper();\
                  let imgCmp = root.findFirstType('image');\
                  if (!imgCmp) imgCmp = root.append({ type: 'image' }, { at: 0 })[0];\
                  imgCmp.set({ src: asset.getSrc() });\
                }\
              }\
            ]\
          },\
          { type: 'canvas' }\
        ]
      },
    }
  }}
/>

```

#### PanelAssets properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#panelassets-properties "Direct link to PanelAssets properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | panelAssets | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| assets | array | A custom array of assets. Overrides any other configured onLoad or providers.<br>**Example** <br>```codeBlockLines_AdAo<br>assets: [<br> {<br>   type: 'image',<br>   url: 'https://example.com/image.jpg',<br> }<br>]<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| close | function | Define how to close your AssetManager. For the main AssetManager, it closes the dialog.<br>**Example**<br>```codeBlockLines_AdAo<br>() => AssetManager.close()<br>``` |
| content | object | Content configuration for the AssetManager.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br> itemsPerRow: 4,<br> header: {<br>  search: true,<br>},<br>``` |
| header | object | Header of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| onSelect | function | Callback when an asset is selected.<br>**Example** <br>```codeBlockLines_AdAo<br>onSelect: ({ asset, editor }) => {<br>   editor.AssetManager.add(asset);<br>}<br>``` |
| optionalProvider | boolean | When true, adds \`{ id: undefined, label: 'Project assets' }\` as an extra option for the asset provider filter at the beginning.<br>**Default** <br>```codeBlockLines_AdAo<br>true<br>``` |
| optionalType | boolean | When true, adds \`{ id: undefined, label: 'All' }\` as an extra option for the asset type filter at the beginning.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| providerId | string | Initial state of the asset provider filter.<br>**Example**<br>```codeBlockLines_AdAo<br>"unsplash"<br>``` |
| providers | array | The ids of providers that will be available in this assets panel layout. These providers need to exist in {@link AssetsConfig.providers } first. Defaults to the all providers specified in {@link AssetsConfig } . Set to an empty array to hide the asset provider filter.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  "unsplash"<br>]<br>``` |
| resizable | object | Resizable configuration of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |
| typeId | string | Initial state of the asset type filter.<br>**Default** <br>```codeBlockLines_AdAo<br>image<br>``` |
| types | array | Options that show up in the asset type filter. The selected type filters providers in the asset provider filter by \`assetProvider.type\`. This filter is required by default. To make it optional set \`assetManagerProps.optionalType = true\`. Set to an empty array to hide the asset type filter.<br>**Default**<br>```codeBlockLines_AdAo<br>[{ id: AssetType.image, label: 'Images'}]<br>``` |

## PanelTemplates [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#paneltemplates "Direct link to PanelTemplates")

Displays a list of templates that users can select as a starting point for their project. For more details, refer to the [Templates page](https://app.grapesjs.com/docs-sdk/configuration/templates).

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'column',\
            style: { width: 350 },\
            children: [\
              {\
                type: 'panelTemplates',\
                classNameAccordion: 'flex-grow h-full',\
                classNameAccordionContent: 'flex-grow h-full',\
                header: { label: 'Choose a template for your project' },\
                content: { itemsPerRow: 1 },\
                templates: [\
                  {\
                    id: 'template1',\
                    name: 'Template 1',\
                    data: {\
                      pages: [\
                        {\
                          name: 'Home',\
                          component: '<h1 class="title">Template 1</h1><style>.title { color: red; font-size: 10rem; text-align: center }</style>'\
                        }\
                      ]\
                    }\
                  },\
                  {\
                    id: 'template2',\
                    name: 'Template 2',\
                    data: {\
                      pages: [\
                        { component: '<h1 class="title">Template 2</h1><style>.title { color: blue; font-size: 10rem; text-align: center }</style>' }\
                      ]\
                    }\
                  },\
                  {\
                    id: 'template3',\
                    name: 'Template 3',\
                    data: {\
                      pages: [\
                        { component: '<h1 class="title">Template 3</h1><style>.title { color: green; font-size: 10rem; text-align: center }</style>' }\
                      ]\
                    }\
                  },\
                  {\
                    id: 'template4',\
                    name: 'Template 4',\
                    data: {\
                      pages: [\
                        { component: '<h1 class="title">Template 4</h1><style>.title { color: violet; font-size: 10rem; text-align: center }</style>' }\
                      ]\
                    }\
                  },\
                ]\
              }\
            ]\
          },\
          { type: 'canvas' }\
        ]
      }
    }

  }}
/>

```

#### PanelTemplates properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#paneltemplates-properties "Direct link to PanelTemplates properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | panelTemplates | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| content | object | Extra props to customize this layout panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "itemsPerRow": 3<br>}<br>``` |
| header |  | Header of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| onSelect | function | Provide a custom handler for the select button.<br>**Example**<br>```codeBlockLines_AdAo<br>({ loadTemplate, template }) => {<br>  // loads the selected template to the current project<br>  loadTemplate(template);<br>}<br>``` |
| resizable |  | Resizable configuration of the panel.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |
| templates | array | Custom array of templates to show in this panel.<br>**Example** <br>```codeBlockLines_AdAo<br>templates: [{<br>  id: 'template1',<br>  name: 'Template 1',<br>  data: {<br>    pages: [<br>      {<br>        name: 'Home',<br>        component: '<h1 class="title">Template 1</h1><style>.title { color: red; font-size: 10rem; text-align: center }</style>'<br>      }<br>    ]<br>  }<br>}]<br>``` |

## Custom [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#custom "Direct link to Custom")

Provide a custom React component via `component` or any other custom element via `render` option.

- React
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'sidebarLeft',\
            children: [\
              { // React component\
                type: 'custom',\
                component: ({ editor }) => <button onClick={() => alert(editor.getHtml())}>Get HTML!!!</button>\
              },\
              { // HTML string\
                type: 'custom',\
                render: () => 'HTML as <b>string</b>'\
              },\
              { // HTML element\
                type: 'custom',\
                render: () => {\
                  const el = document.createElement('div');\
                  el.innerHTML = 'HTML as <b>element</b>';\
                  return el;\
                }\
              },\
              // Custom element, with cleanup\
              {\
                type: 'custom',\
                render: ({ editor, addEl, removeEl }) => {\
                  const buttonEl = document.createElement('button');\
                  buttonEl.innerHTML = 'Button with <b>cleanup</b>';\
                  buttonEl.style.cssText = 'border: 1px solid; padding: 5px;';\
                  const onClick = () => alert('Button clicked: ' + editor?.getHtml());\
                  buttonEl.addEventListener('click', onClick);\
                  addEl(buttonEl);\
                  return () => {\
                    // Remember to cleanup to avoid memory leaks\
                    buttonEl.removeEventListener('click', onClick);\
                    removeEl(buttonEl);\
                  };\
                }\
              }\
            ]\
          },\
          { type: 'canvasSidebarTop' },\
          { type: 'sidebarRight' }\
        ]
      },
    }
  }}
/>

```

#### Custom properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#custom-properties "Direct link to Custom properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | custom | Type of the layout component. |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| component | React Component | Component to be rendered in the custom layout.<br>**Example**<br>```codeBlockLines_AdAo<br>({ editor }) => <button onClick={() => alert(editor.getHtml())}>Get HTML!!!</button><br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| noWrapper | boolean | Indicates if the custom layout should not be wrapped (only works for React Components as the custom render needs a wrapper)<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| render | function | Function to render the custom layout.<br>**Example**<br>```codeBlockLines_AdAo<br>({ editor, addEl, removeEl }) => '<div>Custom layout</div>'<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## VirtualList [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#virtuallist "Direct link to VirtualList")

A virtual list component that renders only the visible items, improving performance when dealing with a large number of items.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'virtualList',\
            itemsPerRow: 4,\
            style: { width: 700 },\
            items: Array(100).fill(0).map((_, i) => ({ id: i, name: 'Item ' + i })),\
            itemLayout: ({ item, editor }) => ({\
              type: 'column',\
              gap: 5,\
              children: [\
                {\
                  type: 'custom',\
                  render: () => `<img height="100" src="https://picsum.photos/seed/image-${item.id}/100/100"/>`\
                },\
                {\
                  type: 'button',\
                  label: `Add ${item.name}`,\
                  variant: 'primary',\
                  style: { width: '100%' },\
                  onClick() {\
                    editor.getWrapper().append(`<div style="padding: 10px"><img src="https://picsum.photos/seed/image-${item.id}/100/100"/><p>Item from the list: ${item.name}</p></div>`);\
                  }\
                }\
              ]\
            })\
          },\
          { type: 'canvas' }\
        ]
      }
    }
  }}
/>

```

#### VirtualList properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#virtuallist-properties "Direct link to VirtualList properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | virtualList | Type of the layout component. |
| items\* | array | The items to be displayed in the virtual list.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  {<br>    "id": "1",<br>    "name": "Item 1"<br>  },<br>  {<br>    "id": "2",<br>    "name": "Item 2"<br>  }<br>]<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| itemLayout | function | The layout component for the items in the virtual list.<br>**Example**<br>```codeBlockLines_AdAo<br>({ item, editor }) => ({ type: 'row', children: item.name })<br>``` |
| itemsPerRow | number | The number of items per row.<br>**Default** <br>```codeBlockLines_AdAo<br>12<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## Sidebar components [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#sidebar-components "Direct link to Sidebar components")

Studio includes sidebar components that can be toggled based on specific conditions, such as when the default preview command is triggered.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'sidebarLeft',\
            style: { alignItems: 'center', justifyContent: 'center' },\
            children: [\
              {\
                type: 'button',\
                label: 'Toggle Right Sidebar',\
                variant: 'outline',\
                onClick: ({ editor }) => editor.runCommand('studio:sidebarRight:toggle')\
              }\
            ]\
          },\
          {\
            type: 'column',\
            style: { flexGrow: 1 },\
            children: [\
              {\
                type: 'sidebarTop',\
                style: { alignItems: 'center', justifyContent: 'center', gap: 10 },\
                children: [\
                  {\
                    type: 'button',\
                    icon: 'eye',\
                    variant: 'outline',\
                    onClick: ({ editor }) => editor.runCommand('core:preview')\
                  },\
                  {\
                    type: 'button',\
                    label: 'Toggle Bottom Sidebar',\
                    variant: 'outline',\
                    onClick: ({ editor }) => editor.runCommand('studio:sidebarBottom:toggle')\
                  }\
                ]\
              },\
              {\
                type: 'row',\
                style: { flexGrow: 1, overflow: 'hidden' },\
                children: [\
                  { type: 'canvas', grow: true },\
                  {\
                    type: 'sidebarRight',\
                    style: { alignItems: 'center', justifyContent: 'center' },\
                    children: [\
                      {\
                        type: 'button',\
                        label: 'Toggle Left Sidebar',\
                        variant: 'outline',\
                        onClick: ({ editor }) => editor.runCommand('studio:sidebarLeft:toggle')\
                      }\
                    ]\
                  }\
                ]\
              },\
              {\
                type: 'sidebarBottom',\
                style: { alignItems: 'center', justifyContent: 'center' },\
                children: [\
                  {\
                    type: 'button',\
                    label: 'Toggle Top Sidebar',\
                    variant: 'outline',\
                    onClick: ({ editor }) => editor.runCommand('studio:sidebarTop:toggle')\
                  }\
                ]\
              }\
            ]\
          }\
        ]
      },
    }
  }}
/>

```

#### SidebarBottom properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#sidebarbottom-properties "Direct link to SidebarBottom properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | sidebarBottom | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| children | Layout component | The children layout components.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

#### SidebarLeft properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#sidebarleft-properties "Direct link to SidebarLeft properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | sidebarLeft | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| children | Layout component | The children layout components.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| resizable | boolean | Indicates whether the sidebar is resizable.<br>**Default** <br>```codeBlockLines_AdAo<br>true<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

#### SidebarRight properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#sidebarright-properties "Direct link to SidebarRight properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | sidebarRight | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| children | Layout component | The children layout components.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| resizable | boolean | Indicates whether the sidebar is resizable.<br>**Default** <br>```codeBlockLines_AdAo<br>true<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

#### SidebarTop properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#sidebartop-properties "Direct link to SidebarTop properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | sidebarTop | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| children | Layout component | The children layout components.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| devices | [DevicesProps](https://app.grapesjs.com/docs-sdk/configuration/layout/components#devices-properties) | The properties for the devices section of the top bar.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "style": {<br>    "width": 200<br>  }<br>}<br>``` |
| height | number, string | The height of the top bar.<br>**Example** <br>```codeBlockLines_AdAo<br>50<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| leftContainer | object | The properties for the left container of the top bar.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "buttons": [<br>    {<br>      "type": "button",<br>      "icon": "menu",<br>      "onClick": "toggleSidebar"<br>    }<br>  ]<br>}<br>``` |
| rightContainer | object | The properties for the right container of the top bar.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "buttons": [<br>    {<br>      "type": "button",<br>      "icon": "menu",<br>      "onClick": "toggleSidebar"<br>    }<br>  ]<br>}<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## Canvas components [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#canvas-components "Direct link to Canvas components")

The layout configuration must include one of the following canvas components:

- `canvas`: The basic canvas component
- `canvasSidebarTop` \- The canvas component combined with `sidebarTop`.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'sidebarLeft',\
            children: [\
              { type: 'panelPagesLayers' }\
            ]\
          },\
          {\
            type: 'canvasSidebarTop',\
          },\
          {\
            type: 'sidebarRight',\
          }\
        ]
      },
    }
  }}
/>

```

#### Canvas properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#canvas-properties "Direct link to Canvas properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | canvas | Type of the layout component. |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| grow | boolean | If true, the component will grow to fill available space.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

#### CanvasSidebarTop properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#canvassidebartop-properties "Direct link to CanvasSidebarTop properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | canvasSidebarTop | Type of the layout component. |
| as | string | The HTML tag to use for the layout component.<br>**Example**<br>```codeBlockLines_AdAo<br>"div"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| htmlAttrs | object | The HTML attributes for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| sidebarTop | [SidebarTopProps](https://app.grapesjs.com/docs-sdk/configuration/layout/components#sidebartop-properties) | The sidebar top.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "style": {<br>    "alignItems": "center"<br>  }<br>}<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

## Form components [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#form-components "Direct link to Form components")

The layout configuration could include one of the following form components:

- `InputField`: The basic input field component.
- `SelectField` \- The select field component.
- `ButtonGroupField` \- The button group field component.
- `CodeField` \- The code field component.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'button',\
            variant: 'primary',\
            label: 'View Form',\
            onClick: ({ editor }) => {\
              editor.runCommand('studio:layoutToggle', {\
                id: 'viewFormComponents',\
                header: false,\
                placer: { type: 'dialog', size: 'l', title: 'Form components' },\
                layout: {\
                  type: 'column',\
                  as: 'form',\
                  style: { height: 500, gap: 20 },\
                  htmlAttrs: {\
                    onSubmit: ev => {\
                      ev.preventDefault();\
                      const fd = new FormData(ev.currentTarget);\
                      editor.addComponents({\
                        type: 'text',\
                        tagName: 'p',\
                        content: JSON.stringify({\
                          name: fd.get('name'),\
                          companySize: fd.get('companySize'),\
                          account: fd.get('account'),\
                          html: fd.get('html')\
                        }, null, 2)\
                      });\
                      editor.runCommand('studio:layoutRemove', { id: 'viewFormComponents' });\
                    }\
                  },\
                  children: [\
                    {\
                      type: 'inputField',\
                      name: 'name',\
                      label: 'Name',\
                      placeholder: 'Insert your name',\
                      required: true,\
                      value: '',\
                      onChange: ({ value, setState }) => setState({ value })\
                    },\
                    {\
                      type: 'codeField',\
                      language: 'html',\
                      label: 'HTML',\
                      name: 'html',\
                      value: '<div>Hello</div>',\
                      onChange: ({ value, setState }) => setState({ value }),\
                      required: true\
                    },\
                    {\
                      type: 'selectField',\
                      name: 'companySize',\
                      label: 'Company size',\
                      emptyState: 'Select',\
                      value: '',\
                      required: true,\
                      options: [\
                        { id: '1', label: '1' },\
                        { id: '2-10', label: '2-10' },\
                        { id: '11-50', label: '11-50' },\
                        { id: '51-200', label: '51-200' },\
                        { id: '201-500', label: '201-500' },\
                        { id: '500+', label: '500+' }\
                      ],\
                      onChange: ({ value, setState }) => setState({ value })\
                    },\
                    {\
                      type: 'buttonGroupField',\
                      id: 'account',\
                      name: 'account',\
                      label: 'Account',\
                      value: '',\
                      options: [\
                        { id: 'Personal', label: 'Personal' },\
                        { id: 'Professional', label: 'Professional' }\
                      ],\
                      onChange: ({ value, setState }) => setState({ value })\
                    },\
                    {\
                      type: 'row',\
                      style: { gap: 10 },\
                      children: {\
                        type: 'button',\
                        variant: 'primary',\
                        label: 'Submit',\
                        buttonType: 'submit'\
                      }\
                    }\
                  ]\
                }\
              });\
            }\
          },\
          { type: 'canvas' }\
        ]
      },

    }
  }}
/>

```

#### InputField properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#inputfield-properties "Direct link to InputField properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | inputField | Type of the layout component. |
| value\* | string | The value of the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"username"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| disabled | boolean | Indicates whether the field is disabled.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| editorEvents | object | Update layout component state based on editor events.<br>**Example**<br>```codeBlockLines_AdAo<br>{ 'component:selected': ({ setState, editor }) => setState({ className: 'custom-cls-' + editor.getSelected().getId() }) }<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| inputType | string | The type of the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"text"<br>``` |
| label | string | The label for the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"Username"<br>``` |
| name | string | The name attribute for the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"username"<br>``` |
| onChange | function | The function to call when the field value changes.<br>**Example**<br>```codeBlockLines_AdAo<br>({ value, setState }) => setState({ value });<br>``` |
| onInput | function | The function to call when the field value changes on input.<br>**Example**<br>```codeBlockLines_AdAo<br>({ value, setState }) => setState({ value });<br>``` |
| placeholder | string | The placeholder text for the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"Enter your username"<br>``` |
| readOnly | boolean | Indicates whether the field is read-only.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| required | boolean | Indicates whether the field is required.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| row | boolean | Indicates if the field should be rendered in a row layout.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| size | string | The size of the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"m" | "s"<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

#### SelectField properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#selectfield-properties "Direct link to SelectField properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | selectField | Type of the layout component. |
| options\* | array | The options for the field.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  {<br>    "id": "1",<br>    "label": "Username 1",<br>    "content": "Username 1"<br>  },<br>  {<br>    "id": "2",<br>    "label": "Username 2",<br>    "content": "Username 2"<br>  }<br>]<br>``` |
| value\* | string | The value of the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"username"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| disabled | boolean | Indicates whether the field is disabled.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| editorEvents | object | Update layout component state based on editor events.<br>**Example**<br>```codeBlockLines_AdAo<br>{ 'component:selected': ({ setState, editor }) => setState({ className: 'custom-cls-' + editor.getSelected().getId() }) }<br>``` |
| emptyState | string | The empty state for the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"Select your username"<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| label | string | The label for the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"Username"<br>``` |
| name | string | The name attribute for the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"username"<br>``` |
| onChange | function | The function to call when the field value changes.<br>**Example**<br>```codeBlockLines_AdAo<br>({ value, setState }) => setState({ value });<br>``` |
| required | boolean | Indicates whether the field is required.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| size | string | The size of the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"l" | "m" | "s" | "xs" | "x2s"<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

#### ButtonGroupField properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#buttongroupfield-properties "Direct link to ButtonGroupField properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | buttonGroupField | Type of the layout component. |
| options\* | array | The options for the field.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  {<br>    "id": "1",<br>    "label": "Username 1",<br>    "title": "Username 1",<br>    "icon": "<svg>...</svg>"<br>  },<br>  {<br>    "id": "2",<br>    "label": "Username 2",<br>    "title": "Username 2",<br>    "icon": "<svg>...</svg>"<br>  }<br>]<br>``` |
| value\* | string | The value of the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"username"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| disabled | boolean | Indicates whether the field is disabled.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| editorEvents | object | Update layout component state based on editor events.<br>**Example**<br>```codeBlockLines_AdAo<br>{ 'component:selected': ({ setState, editor }) => setState({ className: 'custom-cls-' + editor.getSelected().getId() }) }<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| label | string | The label for the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"Username"<br>``` |
| name | string | The name attribute for the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"username"<br>``` |
| onChange | function | The function to call when the field value changes.<br>**Example**<br>```codeBlockLines_AdAo<br>({ value, setState }) => setState({ value });<br>``` |
| required | boolean | Indicates whether the field is required.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| size | string | The size of the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"m" | "s" | "xs"<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

#### CodeField properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components\#codefield-properties "Direct link to CodeField properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| type\* | codeField | Type of the layout component. |
| language\* | string | Indicates the language of the code field.<br>**Example**<br>```codeBlockLines_AdAo<br>"json"<br>``` |
| value\* | string | The value of the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"username"<br>``` |
| className | string | The CSS class name(s) for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-component-class"<br>``` |
| disabled | boolean | Indicates whether the field is disabled.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| editorEvents | object | Update layout component state based on editor events.<br>**Example**<br>```codeBlockLines_AdAo<br>{ 'component:selected': ({ setState, editor }) => setState({ className: 'custom-cls-' + editor.getSelected().getId() }) }<br>``` |
| id | string | The unique identifier for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>"component-123"<br>``` |
| label | string | The label for the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"Username"<br>``` |
| minHeight | string | Indicates the minimum height of the field.<br>**Default** <br>```codeBlockLines_AdAo<br>170px<br>``` |
| monacoOptions | object | Pass additional options to the Monaco editor. Docs: https://microsoft.github.io/monaco-editor/typedoc/interfaces/editor.IStandaloneEditorConstructionOptions.html<br>**Default**<br>```codeBlockLines_AdAo<br>{}<br>``` |
| name | string | The name attribute for the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"username"<br>``` |
| onChange | function | The function to call when the field value changes.<br>**Example**<br>```codeBlockLines_AdAo<br>({ value, setState }) => setState({ value });<br>``` |
| readOnly | boolean | Indicates whether the field is read-only.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| required | boolean | Indicates whether the field is required.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| row | boolean | Indicates if the field should be rendered in a row layout.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| size | string | The size of the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"m" | "s"<br>``` |
| style | object | The inline styles for the component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>``` |

- [Row](https://app.grapesjs.com/docs-sdk/configuration/layout/components#row)
- [Column](https://app.grapesjs.com/docs-sdk/configuration/layout/components#column)
- [Text](https://app.grapesjs.com/docs-sdk/configuration/layout/components#text)
- [Tabs](https://app.grapesjs.com/docs-sdk/configuration/layout/components#tabs)
- [Button](https://app.grapesjs.com/docs-sdk/configuration/layout/components#button)
- [Devices](https://app.grapesjs.com/docs-sdk/configuration/layout/components#devices)
- [PanelPages](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelpages)
- [PanelPageSettings](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelpagesettings)
- [PanelLayers](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panellayers)
- [PanelPagesLayers](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelpageslayers)
- [PanelBlocks](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelblocks)
- [PanelGlobalStyles](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelglobalstyles)
- [PanelSelectors](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelselectors)
- [PanelStyles](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelstyles)
- [PanelProperties](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelproperties)
- [PanelSidebarTabs](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelsidebartabs)
- [PanelAssets](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelassets)
- [PanelTemplates](https://app.grapesjs.com/docs-sdk/configuration/layout/components#paneltemplates)
- [Custom](https://app.grapesjs.com/docs-sdk/configuration/layout/components#custom)
- [VirtualList](https://app.grapesjs.com/docs-sdk/configuration/layout/components#virtuallist)
- [Sidebar components](https://app.grapesjs.com/docs-sdk/configuration/layout/components#sidebar-components)
- [Canvas components](https://app.grapesjs.com/docs-sdk/configuration/layout/components#canvas-components)
- [Form components](https://app.grapesjs.com/docs-sdk/configuration/layout/components#form-components)