---
url: "https://app.grapesjs.com/docs-sdk/overview/getting-started"
title: "Get started with Studio SDK | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/overview/getting-started#__docusaurus_skipToContent_fallback)

On this page

The Studio SDK is a fully embeddable, drag-and-drop, white-label version of our standalone [Studio](https://app.grapesjs.com/studio) editor. It enables seamless integration of our ready-to-use visual builder into any external web application, giving your users a powerful editing experience. The SDK is highly customizable and extendable through the [GrapesJS](https://grapesjs.com/docs/) core API, making it adaptable to your specific needs.

To get a feel of the Studio SDK's capabilities, you can explore and experiment with our live Studio at [https://app.grapesjs.com/studio](https://app.grapesjs.com/studio).

![Studio App](https://app.grapesjs.com/docs-sdk/assets/images/studio-app-9bf0c6393e2479e2cdbffc9d3acc9430.png)

## Installation [​](https://app.grapesjs.com/docs-sdk/overview/getting-started\#installation "Direct link to Installation")

### Download [​](https://app.grapesjs.com/docs-sdk/overview/getting-started\#download "Direct link to Download")

To install the Studio SDK in your application, run the following command in your terminal.

- npm
- pnpm
- yarn
- CDN

```codeBlockLines_AdAo
npm i @grapesjs/studio-sdk

```

### Setup web project [​](https://app.grapesjs.com/docs-sdk/overview/getting-started\#setup-web-project "Direct link to Setup web project")

Now you can use the Studio SDK and embded the editor for any kind of web project. Check below the next steps.

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
      type: 'web',
      // The default project to use for new projects
      default: {
        pages: [\
          { name: 'Home', component: '<h1>Home page</h1>' },\
          { name: 'About', component: '<h1>About page</h1>' },\
          { name: 'Contact', component: '<h1>Contact page</h1>' },\
        ]
      },
    }
  }}
/>

```

### Setup email project [​](https://app.grapesjs.com/docs-sdk/overview/getting-started\#setup-email-project "Direct link to Setup email project")

The Studio SDK also enables embedding a visual editor for newsletters powered by [MJML](https://mjml.io/), ensuring responsive design and compatibility with most email clients.

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
      type: 'email',
      default: {
          pages: [\
            {\
              component: '<mjml><mj-body><mj-section><mj-column><mj-text>My email</mj-text></mj-column></mj-section></mj-body></mjml>'\
            },\
          ]
      },
    },
    i18n: {
      locales: {
        en: {
          actions: {
            importCode: {
              content: 'Paste here your MJML code and click Import'
            }
          }
        }
      }
    }
  }}
/>

```

### Next Step [​](https://app.grapesjs.com/docs-sdk/overview/getting-started\#next-step "Direct link to Next Step")

To ensure a seamless integration of the Studio SDK within your application, refer to the [Configuration Overview](https://app.grapesjs.com/docs-sdk/configuration/overview). The page summarizes the available configurations, including how to properly store projects, manage user assets, and customize various aspects of the editor.

When you're ready to publish your Studio SDK integration on a public domain, make sure to set up a license by creating an [SDK license](https://app.grapesjs.com/docs-sdk/overview/licenses).

- [Installation](https://app.grapesjs.com/docs-sdk/overview/getting-started#installation)
  - [Download](https://app.grapesjs.com/docs-sdk/overview/getting-started#download)
  - [Setup web project](https://app.grapesjs.com/docs-sdk/overview/getting-started#setup-web-project)
  - [Setup email project](https://app.grapesjs.com/docs-sdk/overview/getting-started#setup-email-project)
  - [Next Step](https://app.grapesjs.com/docs-sdk/overview/getting-started#next-step)