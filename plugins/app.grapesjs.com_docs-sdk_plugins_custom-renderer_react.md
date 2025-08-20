---
url: "https://app.grapesjs.com/docs-sdk/plugins/custom-renderer/react"
title: "React | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/custom-renderer/react#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `react` |
| Plan | Startup plan |

This plugin enables visual editing with your own [React](https://react.dev/) components. It includes a custom canvas renderer for the editor and a component for rendering saved projects with React, making it ideal for integrating visual editing into component-based applications.

![React Renderer plugin](https://app.grapesjs.com/docs-sdk/assets/images/renderer-react-plugin-screen-3fc7f6524f3da524a3a6d33cba18e1e2.webp)

Install the Studio SDK plugins package:

- npm
- pnpm
- yarn

```codeBlockLines_AdAo
npm i @grapesjs/studio-sdk-plugins

```

Below is a quick demo showing how content built in the editor maps to your actual React components in real time.

- Build pages and blocks directly with your custom React components.
- Automatically parse HTML into matching React elements.
- Add Pages and Blocks with React components.
- Includes the React canvas renderer for the editor and a lightweight component to render saved projects anywhere (ideal for SSR).

Check the code from the Demo tab with a complete example of how to use the plugin.

- React
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// Import React renderer plugin for the editor
import rendererReact from '@grapesjs/studio-sdk-plugins/dist/rendererReact';

// React renderer for the project JSON
// import { RenderProject } from '@grapesjs/studio-sdk-plugins/dist/rendererReact/rendererProject';
// <RenderProject projectData={projectDataJSON} config={reactRendererConfig}/>

// Full code in the Demo tab
// import Hero from './your/components/Hero';
const reactRendererConfig = {
  components: {
    Hero: {
      component: Hero, // Hero is a React component
      // props are in the shape of GrapesJS Traits
      props: () => [\
        {\
          type: 'text',\
          name: 'title',\
          label: 'Title'\
        },\
        {\
          type: 'text',\
          name: 'subtitle',\
          label: 'Subtitle',\
          value: 'Default Subtitle'\
        }\
      ],
    },
  }
}

// ...
<StudioEditor
  options={{
    // ...
    plugins: [\
      rendererReact.init(reactRendererConfig),\
      (editor) => {\
        // Create blocks with React components\
        editor.Blocks.add('feature', {\
          label: 'Feature component',\
          category: 'React',\
          full: true,\
          content: <Feature title="Feature title" description="Feature description" />\
        }, { at: 0 });\
      }\
    ],
    project: {
      type: 'react', // Provide react type
      default: {
        pages: [\
          {\
            name: 'Page from React',\
            // Create pages with React components\
            component: (\
              <>\
                <Hero title="Build Visually with React" subtitle="A seamless editing experience using your components"/>\
                <Section>\
                  <h2 data-gjs-type="heading" style={{ textAlign: 'center', fontSize: '2rem' }}>Features</h2>\
                  <Feature title="Modular Components" description="Build and edit with reusable UI blocks." />\
                  <Feature title="HTML to React" description="Convert legacy HTML into structured React." />\
                  <Button label="Get Started" href="#start" />\
                </Section>\
              </>\
            )\
          },\
          {\
            name: 'Page from HTML',\
            component: `\
              <h1>React Components from HTML</h1>\
              <div data-gjs-type="Hero" title="React from HTML"></div>\
              <style>\
                body { font-family: system-ui; }\
              </style>\
            `\
          }\
        ]
      }
    },
  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/custom-renderer/react\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| components | object | Map of custom components.<br>**Example** <br>```codeBlockLines_AdAo<br>components: {<br> MyComponent: {<br>  allowPropId: true,<br>  allowPropClassName: true,<br>  allowChildren: true,<br>  component: ({ id, className, children, title, ...rest }) => {<br>    return (<br>       <div id={id} className={className} {...rest}><br>         <h1>{title}</h1><br>         {children}<br>       </div><br>     )<br>  },<br> }<br>}<br>``` |
| errorComponent | ReactComponent | Custom error component. This is used when, for example, you're trying to render a page with an invalid id.<br>**Example** <br>```codeBlockLines_AdAo<br>errorComponent: ({ errorType }) => <div>Error: {errorType}</div><br>``` |
| rootComponent | ReactComponent | Custom root component that wraps the entire rendered project. Usually used to provide a layout or context for the entire application.<br>**Example** <br>```codeBlockLines_AdAo<br>rootComponent: ({ children }) => <div>{children}</div><br>``` |
| headAfter | ReactComponent | Custom react component to append at the end of the \`<head>\` element.<br>**Example** <br>```codeBlockLines_AdAo<br>headAfter: () => (<><br> <meta/><br> <script src="path/to/analytics.js"></script><br></>)<br>``` |
| bodyAfter | ReactComponent | Custom react component to append at the end of the \`<body>\` element.<br>**Example** <br>```codeBlockLines_AdAo<br>bodyAfter: () => <div>Powered by MyApp</div><br>``` |

#### Component config [​](https://app.grapesjs.com/docs-sdk/plugins/custom-renderer/react\#component-config "Direct link to Component config")

| Property | Type | Description |
| --- | --- | --- |
| component\* | ReactComponent | React Component<br>**Example** <br>```codeBlockLines_AdAo<br>component: ({ id, className, children, title, ...rest }) => {<br>  return (<br>   <div id={id} className={className} {...rest}><br>     <h1>{title}</h1><br>     {children}<br>   </div><br>  )<br>}<br>``` |
| allowPropId | boolean | Indicate if the component can accept the \`id\` property. The id property could be used to apply component related styles from the StyleManager. |
| allowPropClassName | boolean | Indicate if the component can accept the \`className\` property. The className property could be used to apply class related styles from the StyleManager. |
| allowChildren | boolean | Indicate if the component can accept children. |
| props | function | Return an array of properties in a shape of GrapesJS traits.<br>**Example** <br>```codeBlockLines_AdAo<br>props: () => [<br> {<br>   type: 'text',<br>   name: 'title',<br>   label: 'Title',<br> }<br>]<br>``` |
| model | object | GrapesJS component model definition.<br>**Example** <br>```codeBlockLines_AdAo<br>model: {<br> defaults: {<br>   draggable: false,<br> }<br>}<br>``` |
| wrapperStyle | object | Allows you to customize the style of the wrapper element around your React component.<br>In the editor, all React components are automatically wrapped in a container element. This wrapper enables editing features like drag & drop, selection, and more.<br>For most cases, styling the wrapper is sufficient. If you need deeper customization (e.g., changing the wrapper element type or structure), use the \`editorRender\` option instead.<br>**Example** <br>```codeBlockLines_AdAo<br>wrapperStyle: { display: 'block' }<br>``` |
| editorRender | ReactComponent | Custom render function for displaying the component inside the editor canvas.<br>Use this when you want full control over how the component appears in the editor, including custom wrappers, helper elements, or injected behavior.<br>The \`connectDom\` function must be passed to the element that should be treated as the component root (for selection, dragging, etc.).<br>**Example** <br>```codeBlockLines_AdAo<br>component: MyComponent,<br>editorRender: ({ connectDom, editor, component, children, props }) => {<br> return (<br>   <div ref={connectDom}><br>     <div>Custom render of the component in the editor canvas</div><br>     <MyComponent {...props}>{children}</MyComponent><br>   </div><br> )<br>}<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/custom-renderer/react#plugin-options)