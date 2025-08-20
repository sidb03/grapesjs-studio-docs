---
url: "https://app.grapesjs.com/docs-sdk/configuration/pages"
title: "Pages | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/pages#__docusaurus_skipToContent_fallback)

On this page

Pages allow you to create projects with multiple pages, making it ideal for various types of `web` projects. In Studio SDK, the Pages Manager is a direct extension of the [GrapesJS Pages module](https://grapesjs.com/docs/modules/Pages.html), meaning you can always reuse the existing [Pages API](https://grapesjs.com/docs/api/pages.html) for flexibility and consistency.

![Page Manager](<Base64-Image-Removed>)

## Table of Contents

- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/pages#initialization)
- [Configuration](https://app.grapesjs.com/docs-sdk/configuration/pages#configuration)
- [I18n](https://app.grapesjs.com/docs-sdk/configuration/pages#i18n)
- [Commands](https://app.grapesjs.com/docs-sdk/configuration/pages#commands)

## Initialization [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#initialization "Direct link to Initialization")

During the `web` project type initialization, pages are enabled by default with all available features.

warning

Pages are automatically disabled in the `email` project type.

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

![Default pages](<Base64-Image-Removed>)

If you're working on a single-page project (e.g., a landing page builder), you can easily disable the Pages Manager.

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
    pages: false
  }}
/>

```

## Configuration [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#configuration "Direct link to Configuration")

By default, users can manage pages without restrictions, including creating, duplicating, or deleting them. With Studio SDK, you can easily customize each of these operations to suit your needs.

### Add Page [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#add-page "Direct link to Add Page")

![Add default pages](<Base64-Image-Removed>)

By default, new pages are created with a basic name and content. You can easily modify this behavior by implementing your own page creation logic.

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
    pages: {
      add: ({ editor, rename }) => {
        // Add new page via GrapesJS API
        const page = editor.Pages.add({
          name: 'New page',
          component: '<div>New page</div>'
        }, {
          select: true // Select added page
        });
        rename(page); // Trigger page rename action
      },
    }
  }}
/>

```

If you want to disable page creation, simply set the `add` option to `false`. This will hide the Add button.

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
    pages: {
      add: false,
    }
  }}
/>

```

### Duplicate Page [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#duplicate-page "Direct link to Duplicate Page")

![Duplicate pages](<Base64-Image-Removed>)

Users can duplicate pages through the command items (3 dots icon) available on the selected page. You can customize the duplication logic using the `duplicate` option.

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
    pages: {
      duplicate: ({ editor, page, rename }) => {
        const root = page.getMainComponent();
        const newPage = editor.Pages.add({
          name: `${page.getName()} (Copy)`,
          component: root.clone(),
        }, { select: true });

        rename(newPage);
      },

    }
  }}
/>

```

To disable page duplication, set the `duplicate` option to `false`. This will remove the duplication command from the menu.

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
    pages: {
      duplicate: false,
    }
  }}
/>

```

### Remove Page [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#remove-page "Direct link to Remove Page")

Page removal is also available in the command items and can be easily customized.

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
    pages: {
      remove: ({ editor, page }) => {
        const { Pages } = editor;
        if (confirm('Are you sure?')) {
          Pages.remove(page);
        }
        // Select the first page
        Pages.select(Pages.getAll()[0]);
      },
    }
  }}
/>

```

Like other commands, you can easily disable the page removal option if needed.

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
    pages: {
      remove: false,
    }
  }}
/>

```

### Command Items [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#command-items "Direct link to Command Items")

If you need to add more operations to your pages, you can customize the page command items using the `commandItems` option. This allows you to tailor the set of actions available on each page according to your specific requirements.

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
    pages: {
      commandItems: ({ items }) => {
        return [\
          ...items, // default items\
          {\
            id: 'custom-command',\
            label: 'Custom command',\
            cmd: ({ page }) => alert('Page: ' + page.getName())\
          }\
        ];
      }
    }
  }}
/>

```

![Custom page command](<Base64-Image-Removed>)

### Settings [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#settings "Direct link to Settings")

![Custom page command](<Base64-Image-Removed>)

Studio includes a built-in page settings panel that enables users to configure various aspects of their HTML page. This includes meta elements for SEO, social media tags, and the inclusion of custom code.

![Page setting 1](https://app.grapesjs.com/docs-sdk/assets/images/page-setting-1-bbf149a6a3e58494fa7999a25232d593.png)

![Page setting 2](https://app.grapesjs.com/docs-sdk/assets/images/page-setting-2-e5759065be89ee03172021d5f72241f0.png)

To disable the settings panel, simply pass `false` to the `settings` option.

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
    pages: {
      settings: false,
    }
  }}
/>

```

#### Load page settings [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#load-page-settings "Direct link to Load page settings")

All page settings, both global and page-specific, can be loaded from your project JSON file (e.g., retrieved from your [Storage](https://app.grapesjs.com/docs-sdk/configuration/projects#storage) or provided as a [Template](https://app.grapesjs.com/docs-sdk/configuration/templates)).

Page-specific settings can also be parsed directly from HTML strings in your project file, if the page is initialized as an HTML string. Global settings are stored and accessed via `projectFile.custom.globalPageSettings`.

Below is an example demonstrating how to load different pages along with their global and page-specific settings.

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
      default: {
        custom: {
          globalPageSettings: {
            title: 'Global title',
            description: 'Global description',
            customCodeHead: `
              <meta name="meta-global" content="Global meta"/>
              <link href="https://cdn.jsdelivr.net/npm/reset-css@5.0.2/reset.min.css" rel="stylesheet">
              <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.7.1/jquery.min.js"></script>
              <script>console.log('[GLOBAL]: Custom HTML head');</script>
              <style>.title { font-size: 5rem; }</style>
            `,
            customCodeBody: '<div>Global Custom HTML body</div>',
          }
        },
        pages: [\
          {\
            name: 'Home',\
            component: `<!DOCTYPE html>\
              <html>\
                <head>\
                  <title>Home title</title>\
                  <meta name="description" content="Home description" />\
                  <meta name="meta-home" content="Home meta">\
                  <script>console.log('[HOME]: Custom HTML head');</script>\
                </head>\
                <body>\
                  <div class="title">Home page</div>\
                  <div data-custom-html-body>\
                    <div>Home Custom HTML body</div>\
                    <script>\
                      console.log('[HOME]: Custom HTML body');\
                    </script>\
                  </div>\
                </body>\
              <html>`\
          },\
          {\
            name: 'About',\
            settings: { slug: 'about-slug' },\
            component: `<!DOCTYPE html>\
              <html>\
                <head>\
                  <title>About title</title>\
                  <meta name="description" content="About description" />\
                  <meta name="meta-about" content="About meta">\
                  <script>console.log('[ABOUT]: Custom HTML head');</script>\
                </head>\
                <body>\
                  <div class="title">About page</div>\
                </body>\
              <html>`\
          },\
          {\
            name: 'Contact',\
            settings: { slug: 'contact-slug' },\
            component: `<!DOCTYPE html>\
              <html>\
                <body>\
                  <div class="title">Contact page</div>\
                </body>\
              <html>`\
          }\
        ]
      },
    }
  }}
/>

```

## I18n [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#i18n "Direct link to I18n")

All the labels of the page manager are connected to the [i18n GrapesJS module](https://grapesjs.com/docs/modules/I18n.html). Below, you'll find the reference for each key.

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
    i18n: {
      locales: {
        en: {
          pageManager: {
            pages: 'Pages',
            page: 'Page',
            newPage: 'New Page',
            add: 'Add page',
            rename: 'Rename',
            duplicate: 'Duplicate',
            copy: 'Copy',
            delete: 'Delete',
            deletePage: 'Delete Page',
            confirmDelete: 'Are you sure you want to delete the page?',
            settings: {
              label: 'Settings',
              title: 'Page settings',
              global: 'Global settings',
              fields: {
                name: {
                  label: 'Name'
                },
                slug: {
                  label: 'Slug',
                  description: '...'
                },
                favicon: {
                  label: 'Favicon',
                  description: '...'
                },
                title: {
                  label: 'Title',
                  description: '...'
                },
                description: {
                  label: 'Description',
                  description: '...'
                },
                keywords: {
                  label: 'Keywords',
                  description: '...'
                },
                socialTitle: {
                  label: 'Social title',
                  description: '...'
                },
                socialImage: {
                  label: 'Social image',
                  description: '...'
                },
                socialDescription: {
                  label: 'Social description',
                  description: '...'
                },
                customCodeHead: {
                  label: 'Custom HTML head',
                  description: '...'
                },
                customCodeBody: {
                  label: 'Custom HTML body',
                  description: '...'
                }
              }
            }
          }
        }
      }
    }
  }}
/>

```

## Commands [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#commands "Direct link to Commands")

Here below you can find a list of commands that allow you to interact programmatically with the Page Manager.

### Get config [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#get-config "Direct link to Get config")

Get current Pages config.

```codeBlockLines_AdAo
const config = editor.runCommand(StudioCommands.getPagesConfig);

```

### Update config [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#update-config "Direct link to Update config")

Update Pages config.

```codeBlockLines_AdAo
editor.runCommand(StudioCommands.setPagesConfig, {
  config: { add: false }
});

```

### Get page settings [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#get-page-settings "Direct link to Get page settings")

Get page settings panel state.

```codeBlockLines_AdAo
const state: PanelPageSettingsState = editor.runCommand(StudioCommands.getPageSettings);

```

### Update page settings [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#update-page-settings "Direct link to Update page settings")

Update page settings panel state.

```codeBlockLines_AdAo
editor.runCommand(StudioCommands.setPageSettings, { isOpen: true });

```

### Project files [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#project-files "Direct link to Project files")

Get project files.

```codeBlockLines_AdAo
const files: ProjectFile[] = await editor.runCommand(StudioCommands.projectFiles);

```

### Example [​](https://app.grapesjs.com/docs-sdk/configuration/pages\#example "Direct link to Example")

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
const onReady = (editor) => {
  // store the editor in your state manager
};

const toggleAddPage = () => {
  const config = editor.runCommand(StudioCommands.getPagesConfig) || {};
  editor.runCommand(StudioCommands.setPagesConfig, {
    config: { add: !config.add }
  });
}
const togglePageSettings = () => {
  const state = editor.runCommand(StudioCommands.getPageSettings) || {};
  editor.runCommand(StudioCommands.setPageSettings, { isOpen: !state.isOpen });
}

// ...
<StudioEditor
  options={{
    onReady,
  }}
/>

```

- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/pages#initialization)
- [Configuration](https://app.grapesjs.com/docs-sdk/configuration/pages#configuration)
  - [Add Page](https://app.grapesjs.com/docs-sdk/configuration/pages#add-page)
  - [Duplicate Page](https://app.grapesjs.com/docs-sdk/configuration/pages#duplicate-page)
  - [Remove Page](https://app.grapesjs.com/docs-sdk/configuration/pages#remove-page)
  - [Command Items](https://app.grapesjs.com/docs-sdk/configuration/pages#command-items)
  - [Settings](https://app.grapesjs.com/docs-sdk/configuration/pages#settings)
- [I18n](https://app.grapesjs.com/docs-sdk/configuration/pages#i18n)
- [Commands](https://app.grapesjs.com/docs-sdk/configuration/pages#commands)
  - [Get config](https://app.grapesjs.com/docs-sdk/configuration/pages#get-config)
  - [Update config](https://app.grapesjs.com/docs-sdk/configuration/pages#update-config)
  - [Get page settings](https://app.grapesjs.com/docs-sdk/configuration/pages#get-page-settings)
  - [Update page settings](https://app.grapesjs.com/docs-sdk/configuration/pages#update-page-settings)
  - [Project files](https://app.grapesjs.com/docs-sdk/configuration/pages#project-files)
  - [Example](https://app.grapesjs.com/docs-sdk/configuration/pages#example)