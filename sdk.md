# Merged Documentation

This file contains all markdown files merged from the current directory.
Generated on: Fri Aug 8 01:59:01 IST 2025

---

# File: app.grapesjs.com_docs-sdk_configuration_assets_asset-providers.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers"
title: "Asset Providers | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#__docusaurus_skipToContent_fallback)

On this page

![Asset Manager with custom Asset Providers](https://app.grapesjs.com/docs-sdk/assets/images/asset-providers-e9da291f031980da5554eae6f3fefc34.png)

Asset Providers enables you to integrate any external source (e.g. from an API), allowing you to load custom asset types such as images, videos, and documents.

## Table of Contents

- [Basic setup](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#basic-setup)
- [Endless scrolling pagination](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#endless-scrolling-pagination)
- [Custom item layout](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#custom-item-layout)
- [Commands](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#commands)

## Basic setup [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#basic-setup "Direct link to Basic setup")

Here we configure an Asset Provider using images from Lorem Picsum:

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    project: {
      default: {
        pages: [{ name: "Home", component: '<div><p>Double click the image below to open the AssetManager</p><img id="picture"/></div>' }],
      }
    },
    assets: {
      // Select by default the Lorem Picsum provider when the asset manager opens
      providerId: "picsum-pictures",
      providers: [{\
        id: "picsum-pictures",\
        label: "Lorem Picsum pictures",\
        types: "image",\
        onLoad: async (props) => {\
          return Array(30)\
            .fill(0)\
            .map((v, i) => ({\
              src: `https://picsum.photos/seed/${i + 1}/300/300.jpg`,\
              name: `Image #${i + 1}`\
            }));\
        }\
      }]
    }
  }}
/>

```

warning

If your assets are not images, you must define a [custom item layout](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#custom-item-layout).

info

Each Asset Provider you define will show up as an option in the provider filter in the asset manager.

## Endless scrolling pagination [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#endless-scrolling-pagination "Direct link to Endless scrolling pagination")

To enable endless scrolling in an Asset Provider, instead of an array, return an object with an items prop from `AssetProvider.onLoad()`.
We support both offset-based and token-based pagination.

### Offset-based pagination [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#offset-based-pagination "Direct link to Offset-based pagination")

Here's an example for offset-based pagination, where you provide a page number and page size to an API to get a page.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

const getPageFromFakeApi = async ({ pageNumber, pageSize }) => {
  const total = 100;
  const count = (pageNumber - 1) * pageSize;
  const itemsLeft = total - count;
  const actualPageSize = Math.min(pageSize, itemsLeft);
  return {
    total,
    items: Array(actualPageSize)
      .fill(0)
      .map((v, i) => ({
        src: `https://picsum.photos/seed/${i + 1 + count}/300/300.jpg`,
        name: `Page #${pageNumber} Image #${i + 1}`
      }))
  };
};
// ...
<StudioEditor
  options={{
    project: {
      default: {
        pages: [{ name: "Home", component: '<div><p>Double click the image below to open the AssetManager</p><img id="picture"/></div>' }],
      }
    },
    assets: {
      providerId: "picsum-pictures",
      providers: [{\
        id: "picsum-pictures",\
        label: "Lorem Picsum pictures",\
        types: "image",\
        onLoad: async ({ pageIndex }) => {\
          const pageNumber = pageIndex + 1;\
          const pageSize = 30;\
          const page = await getPageFromFakeApi({ pageNumber, pageSize });\
          const count = pageIndex * pageSize + page.items.length;\
          return {\
            items: page.items,\
            isLastPage: count >= page.total\
          };\
        }\
      }]
    }
  }}
/>

```

### Token-based pagination [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#token-based-pagination "Direct link to Token-based pagination")

Here's we consume an API with token-based pagination, where each page returned by the API gives you a token you can use to request the next page.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

const getPageFromFakeApi = async ({ pageToken: count = 0, pageSize }) => {
  const total = 100;
  const itemsLeft = total - count;
  const actualPageSize = Math.min(pageSize, itemsLeft);
  return {
    nextPageToken: itemsLeft < 1 ? undefined : count + actualPageSize,
    items: Array(actualPageSize)
      .fill(0)
      .map((v, i) => ({
        src: `https://picsum.photos/seed/${i + 1 + count}/300/300.jpg`,
        name: `Page token #${count} Image #${i + 1}`,
      }))
  };
};
// ...
<StudioEditor
  options={{
    project: {
      default: {
        pages: [{ name: "Home", component: '<div><p>Double click the image below to open the AssetManager</p><img id="picture"/></div>' }],
      }
    },
    assets: {
      providerId: "picsum-pictures",
      providers: [{\
        id: "picsum-pictures",\
        label: "Lorem Picsum pictures",\
        types: "image",\
        onLoad: async ({ pageCustomData }) => {\
          const pageSize = 30;\
          const page = await getPageFromFakeApi({ pageToken: pageCustomData?.token, pageSize });\
          return {\
            items: page.items,\
            nextPageCustomData: { token: page.nextPageToken },\
            isLastPage: !page.nextPageToken\
          };\
        }\
      }]
    }
  }}
/>

```

## Custom item layout [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#custom-item-layout "Direct link to Custom item layout")

You can define a custom layout for rendering your provider's assets.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    project: {
      default: {
        pages: [{ name: "Home", component: '<div><p>Double click the image below to open the AssetManager</p><img id="picture"/></div>' }],
      }
    },
    assets: {
      providerId: "picsum-pictures",
      providers: [{\
        // ...\
        itemLayout: props => {\
          const { assetProps, onSelect } = props;\
          return {\
            id: assetProps.id,\
            type: 'column',\
            style: { border: '2px solid black', borderRadius: 8, height: 150, position: 'relative', overflow: 'hidden' },\
            onClick: () => onSelect(assetProps),\
            children: [\
              {\
                type: 'custom',\
                render: () => `<img src="${assetProps.src}" alt="${assetProps.name}" style="width: 100%; height: 100%; object-fit: cover">`\
              },\
              {\
                type: 'text',\
                style: { width: '100%', position: 'absolute', bottom: 0, zIndex: 1, background: '#fff', color: '#333', padding: 8 },\
                content: assetProps.name ?? ''\
              }\
            ]\
          };\
        }\
      }]
    }
  }}
/>

```

warning

When you define a custom `itemLayout`, make sure all items have the same fixed height.

## Commands [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#commands "Direct link to Commands")

Here's a list of commands to update Asset Providers dynamically.

### Get Asset Providers [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#get-asset-providers "Direct link to Get Asset Providers")

Get a list of registered Asset Providers.

```codeBlockLines_AdAo
const providers = editor.runCommand(StudioCommands.assetProviderGet);

```

### Add Asset Provider [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#add-asset-provider "Direct link to Add Asset Provider")

Register a new Asset Provider. If an Asset Provider with the same `id` already exists, it will be removed.
You can use the `index` prop to specify its position in the list of providers.

```codeBlockLines_AdAo
editor.runCommand(StudioCommands.assetProviderAdd, { provider: { id: 'new-provider-id' }, index: 0 });

```

### Remove Asset Provider [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#remove-asset-provider "Direct link to Remove Asset Provider")

Remove a registered Asset Provider.

```codeBlockLines_AdAo
editor.runCommand(StudioCommands.assetProviderRemove, { id: 'new-provider-id' });

```

### Properties [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#properties "Direct link to Properties")

#### AssetProvider properties [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#assetprovider-properties "Direct link to AssetProvider properties")

Show properties

| Property   | Type     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ---------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id\*       | string   | Asset Provider ID.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-provider"<br>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| types\*    | string   | Asset types supported by this provider. Only providers that support the current asset type show up in the asset provider filter.<br>**Example** <br>`codeBlockLines_AdAo<br>types: 'image',<br>// Or an array of types<br>types: ['image', 'video']<br>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| label\*    | string   | Label to display in the asset provider filter. You may use a function instead to translate this string.<br>**Example** <br>`codeBlockLines_AdAo<br>label: 'My asset provider'<br>// As a function, for dynamic labels<br>label: ({ editor }) => editor.I18n.t('myProviderLabel')<br>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| search     | object   | Search configuration.<br>**Example** <br>`codeBlockLines_AdAo<br>search: {<br> // Set this to true if you want AssetProvider.onLoad to retrigger when the user types in the search field. When false, loaded assets are filtered locally by name.<br> reloadOnInput: true,<br> // Search value  debounce time in milliseconds<br> debounceMs: 1000<br>}<br>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| onLoad\*   | function | Define how to fetch these assets. Return an array of assets. You may return an array of Page objects to enable endless scrolling, you can rely on the pageIndex argument for this.<br>**Example** <br>`` codeBlockLines_AdAo<br>async () => {<br>  // Simple asset array<br>  return [<br>    { src: 'https://www.example.com/items/1' },<br>    { src: 'https://www.example.com/items/2' },<br>    { src: 'https://www.example.com/items/3' }<br>  ]<br>}<br>async ({ pageIndex }) => {<br>  // Offset based pagination<br>  const pageSize = 20;<br>  const params = new URLSearchParams({ page: pageIndex, pageSize })<br>  const response = await fetch(`https://www.example.com/items?${params}`)<br>  const page = await response.json()<br>  const itemCount = pageSize * pageIndex + page.items.length<br>  return {<br>    items: page.items,<br>    isLastPage: itemCount >= page.total<br>  }<br>}<br>async ({ pageCustomData }) => {<br>  // Token based pagination.<br>  const params = new URLSearchParams({ pageToken: pageCustomData?.token })<br>  const response = await fetch(`https://www.example.com/items?${params}`)<br>  const page = await response.json()<br>  return {<br>    items: page.items,<br>    nextPageCustomData: { token: page.nextPageToken },<br>    isLastPage: !page.nextPageToken<br>  }<br>}<br> `` |
| itemLayout | function | Custom layout for rendering an asset item in the AssetManager.<br>**Example** <br>`` codeBlockLines_AdAo<br>itemLayout: ({ assetProps, onSelect }) => ({<br> type: 'column',<br> style: { height: 150 },<br> onClick: () => onSelect(assetProps),<br> children: [<br>   { type: 'custom', render: () => `<img src="${assetProps.src}" style="width: 100%; height: 100%; object-fit: cover">` },<br>   { type: 'text', style: { width: '100%' }, content: assetProps.name ?? '' }<br> ]<br>})<br> ``                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

- [Basic setup](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#basic-setup)
- [Endless scrolling pagination](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#endless-scrolling-pagination)
  - [Offset-based pagination](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#offset-based-pagination)
  - [Token-based pagination](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#token-based-pagination)
- [Custom item layout](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#custom-item-layout)
- [Commands](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#commands)
  - [Get Asset Providers](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#get-asset-providers)
  - [Add Asset Provider](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#add-asset-provider)
  - [Remove Asset Provider](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#remove-asset-provider)
  - [Properties](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers#properties)

---

# File: app.grapesjs.com_docs-sdk_configuration_assets_overview.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/assets/overview"
title: "Assets | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#__docusaurus_skipToContent_fallback)

On this page

![Asset Manager](https://app.grapesjs.com/docs-sdk/assets/images/asset-manager-fbf02ebd21a56c312bdeed1cc8c433f2.png)

With Studio SDK, end-users can add images, fonts, videos and pdf files to their projects. In this page we'll see how to manage media and resources in your project, including uploading files and configuring storage settings. You can also integrate external services using [Asset Providers](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers), allowing you to load custom asset types such as images, videos, and documents.

## Table of Contents

- [Asset Storage](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#asset-storage)
  - [Cloud Storage](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#cloud-storage)
  - [Custom Storage](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#custom-storage)
- [Custom Asset Manager](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#custom-asset-manager)

## Asset Storage [​](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#asset-storage "Direct link to Asset Storage")

You can configure how to store these assets, so they can be used safely in the published product.
This is controlled by the `assets` option of the SDK.

You can either use our [cloud storage](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#cloud-storage), or use a [custom storage](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#custom-storage) method of your own.

warning

Always configure one of these, otherwise assets will be added as data urls, resulting in heavy html exports and potentially slower load times.

### Cloud Storage [​](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#cloud-storage "Direct link to Cloud Storage")

We provide an easy way to upload assets for your users.
To use this, set `assets.storageType` to `cloud`, and add a unique id for each of your projects and end-users.

- React
- JS

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    licenseKey: "YOUR_LICENSE_KEY",
    assets: {
      storageType: "cloud"
    },
    project: {
      id: "UNIQUE_PROJECT_ID"
    },
    identity: {
      id: "UNIQUE_END_USER_ID"
    }
  }}
/>

```

warning

Avoid sensitive data in these identifiers, like emails, or phone numbers.

These project and identity ids are mandatory.
We use them for allowing end-users to reuse their assets across different projects.

### Custom Storage [​](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#custom-storage "Direct link to Custom Storage")

If you want to handle asset storage on your own, set `assets.storageType` to `self` and specify `assets.onUpload` and `assets.onDelete` callbacks to indicate how to upload and delete assets with your storage.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

const myUploadAssetsLogic = async (files) => {
  await waitAndFailRandomly('Testing when upload assets failed');
  const mockUploadedFiles = files.map(file => ({ file, url: URL.createObjectURL(file) }));
  console.log('Assets to upload to the self-hosted storage', mockUploadedFiles);
  return mockUploadedFiles;
}
const myDeleteAssetsLogic = async ({ assets }) => {
  await waitAndFailRandomly('Testing when delete assets failed');
  console.log('Assets to delete from the self-hosted storage', assets.map(asset => asset.getSrc()));
}
const waitAndFailRandomly = async (str) => {
  await new Promise(res => setTimeout(res, 1000)); // fake delay
  if (Math.random() >= 0.7) throw new Error(str);
}

// ...
<StudioEditor
  options={{
    // ...
    assets: {
      storageType: 'self',
      // this is an example, you can do whatever you want in these callbacks as long as you return the same arrays.
      onUpload: async ({ files, editor }) => {
        // TODO: implement myUploadAssetsLogic to upload files
        const results = await myUploadAssetsLogic(files);
        // return an array with the uploaded assets
        return results.map(({ file, url }) => ({
          id: url,
          src: url,
          name: file.name,
          mimeType: file.type,
          size: file.size
        }));
      },
      onDelete: async ({ assets, editor }) => {
        // TODO: implement myDeleteAssetsLogic to delete assets
        await myDeleteAssetsLogic({ assets });
        // No need to return anything, just let the promise resolve without errors
      }
    },
    project: {
      default: {
        // project assets
        assets: Array(5).fill(0).map((_, i) => ({
          name: 'Project image ' + i,
          src: `https://picsum.photos/seed/from-project-${i}/300/300`
        })),
        pages: [\
          { name: 'Home', component: '<h1>Double click the image to open the Asset Manager</h1><img src="https://picsum.photos/seed/x/300/300"/>'},\
        ]
      }
    },
  }}
/>

```

#### Custom load [​](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#custom-load "Direct link to Custom load")

By default, uploaded asset references (e.g. `{ src: 'https://....' }`) are stored directly inside the [Project JSON](https://app.grapesjs.com/docs-sdk/configuration/projects#storage).
If you prefer to load the list of assets directly from your storage, you can use a custom `assets.onLoad` callback.
When this callback is used, the assets defined in the JSON will be ignored.

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
    assets: {
      storageType: 'self',
      onUpload: async ({ files, editor }) => {
        const results = await myUploadAssetsLogic(files);
        return results.map(({ file, url }) => ({ id: url, src: url, name: file.name, mimeType: file.type, size: file.size }));
      },
      onDelete: async ({ assets, editor }) => {
        await myDeleteAssetsLogic({ assets });
      },
      onLoad: async () => {
        // Fetch and return assets from your storage backend
        return Array(10).fill(0).map((_, i) => ({
          name: 'Image from storage ' + i,
          type: 'image',
          src: `https://picsum.photos/seed/from-storage-${i}/300/300`
        }))
      },
    },
    project: {
      default: {
        // These assets will be ignored
        assets: Array(5).fill(0).map((_, i) => ({ name: 'Project image ' + i, src: `https://picsum.photos/seed/from-project-${i}/300/300` })),
        pages: [\
          { name: 'Home', component: '<h1>Double click the image to open the Asset Manager</h1><img src="https://picsum.photos/seed/x/300/300"/>'},\
        ],
      },
    },
  }}
/>

```

## Custom Asset Manager [​](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#custom-asset-manager "Direct link to Custom Asset Manager")

If your product requires a custom asset manager, you can opt out of the default one by leveraging GrapesJS’s native interface and providing your own callbacks.

To be compatible with the editor, your custom asset manager should handle three key actions:

- How to open the asset manager
- How to close it
- How to select and return an asset

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
      storageManager: false, // Disabled for demo purposes
      assetManager: {
        custom: {
          // Tell the editor how to open your custom Asset Manager
          open(props) {
            // Example with a custom dialog (pseudo-code)
            if (typeof myCustomAssetManagerDialog !== 'undefined') {
              myCustomAssetManagerDialog.open({
                // Indicate how to select the asset
                onSelect(imageUrl) { props.select({ src: imageUrl }) },
                // Propagate the close action back to the editor
                onClose() { props.close(); }
              });
            } else {
              // Fallback demo using native file picker
              const input = document.createElement('input');
              input.type = 'file';
              input.accept = 'image/*';
              input.onchange = (event) => {
                const file = event.target.files[0];
                if (file) {
                  const reader = new FileReader();
                  reader.onload = () => {
                    props.select({ src: reader.result });
                    props.close();
                  };
                  reader.readAsDataURL(file);
                } else {
                  props.close();
                }
              };
              input.oncancel = () => props.close();
              input.click();
            }
          },
          // Tell the editor how to close your custom Asset Manager
          close(props) {
            if (typeof myCustomAssetManagerDialog !== 'undefined') {
              myCustomAssetManagerDialog.close();
            } else {
              alert('Custom Asset Manager closed!');
            }
          }
        }
      }
    },
    project: {
      default: {
        pages: [\
          { name: 'Home', component: '<h1>Double click the image to open the Asset Manager</h1><img src="https://picsum.photos/seed/x/300/300"/>'},\
        ]
      }
    },
  }}
/>

```

- [Asset Storage](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#asset-storage)
  - [Cloud Storage](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#cloud-storage)
  - [Custom Storage](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#custom-storage)
- [Custom Asset Manager](https://app.grapesjs.com/docs-sdk/configuration/assets/overview#custom-asset-manager)

---

# File: app.grapesjs.com_docs-sdk_configuration_blocks.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/blocks"
title: "Blocks | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/blocks#__docusaurus_skipToContent_fallback)

On this page

Blocks let users easily add pre-configured content to the editor. A block can represent a single component, like an image, or a group of components.

![Blocks Panel](https://app.grapesjs.com/docs-sdk/assets/images/blocks-panel-c22874feb018b37e959b7a68f6aeb39d.webp)

This guide will walk you through setting up and customizing blocks using the built-in Block Manager. The default Block Manager UI is lightweight, with built-in drag-and-drop support. However, you can also extend it or build a custom UI to better fit your needs.

Before you start

For a better understanding of this guide, we recommend checking out the [Components](https://app.grapesjs.com/docs-sdk/configuration/components/overview) section first.

## Table of Contents

- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/blocks#initialization)
- [Configuration](https://app.grapesjs.com/docs-sdk/configuration/blocks#configuration)
- [Programmatic usage](https://app.grapesjs.com/docs-sdk/configuration/blocks#programmatic-usage)
- [I18n](https://app.grapesjs.com/docs-sdk/configuration/blocks#i18n)
- [Customization](https://app.grapesjs.com/docs-sdk/configuration/blocks#customization)

## Initialization [​](https://app.grapesjs.com/docs-sdk/configuration/blocks#initialization "Direct link to Initialization")

Studio SDK comes with a set of basic blocks tailored to the selected project type. You can easily extend this by adding custom blocks using the `blocks.default` option.

The demo below shows how to initialize the editor with your custom blocks.

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
    blocks: {
      default: [\
        {\
          id: 'my-block-image',\
          label: 'My Image',\
          media: '<svg viewBox="0 0 24 24"><path d="M20 5a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V7c0-1.1.9-2 2-2h16M5 16h14l-4.5-6-3.5 4.5-2.5-3L5 16Z"/></svg>',\
          // Component Definition as content\
          content: { type: 'image', src: 'https://picsum.photos/seed/my-image/100/100' }\
        },\
        {\
          id: 'my-block-section',\
          label: 'My Section',\
          media: '<svg viewBox="0 0 24 24"><path d="M21 3H3c-.6 0-1 .4-1 1v6c0 .6.4 1 1 1h18c.6 0 1-.4 1-1V4c0-.6-.4-1-1-1m0 10H3c-.6 0-1 .4-1 1v6c0 .6.4 1 1 1h18c.6 0 1-.4 1-1v-6c0-.6-.4-1-1-1Z"/></svg>',\
          // Component Definition with nested components\
          content: {\
            tagName: 'section',\
            style: { padding: '25px', background: '#f3f3f3' },\
            components: [\
              { tagName: 'h1', components: 'Section' },\
              { type: 'image', src: 'https://picsum.photos/seed/my-image/100/100' }\
            ]\
          }\
        },\
        {\
          id: 'my-block-section-html',\
          label: 'My HTML',\
          media: '<svg viewBox="0 0 24 24"><path d="M21 3H3c-.6 0-1 .4-1 1v6c0 .6.4 1 1 1h18c.6 0 1-.4 1-1V4c0-.6-.4-1-1-1m0 10H3c-.6 0-1 .4-1 1v6c0 .6.4 1 1 1h18c.6 0 1-.4 1-1v-6c0-.6-.4-1-1-1Z"/></svg>',\
          // HTML strings will be transformed to Component Definitions\
          content: `\
            <section style="padding: 25px; background: #f3f3f3">\
              <h1>Section (HTML)</h1>\
              <img src="https://picsum.photos/seed/my-image/100/100" />\
            </section>\
          `\
        }\
      ]
    },
    // Custom default project for demo purpose
    project: {
      default: {
        pages: [ { name: 'Home', component: '<h1>Blocks demo project</h1>' } ]
      },
    },
    // Custom editor layout for demo purpose
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'panelBlocks',\
            header: { label: 'Blocks', collapsible: false, style: { width: '300px' } },\
            symbols: false,\
          },\
          { type: 'canvas' }\
        ]
      }
    },
  }}
/>

```

![Blocks initialization](https://app.grapesjs.com/docs-sdk/assets/images/blocks-init-9364cddbb694dd7beb83b51d0be9f11e.webp)

In the demo above, you can see that the predefined blocks are added after your default ones. If you need to remove or filter them for any reason, you can do so [programmatically](https://app.grapesjs.com/docs-sdk/configuration/blocks#programmatic-usage).

## Configuration [​](https://app.grapesjs.com/docs-sdk/configuration/blocks#configuration "Direct link to Configuration")

Each block comes with [configurable properties](https://grapesjs.com/docs/api/block.html#properties). You can group blocks into categories, add custom media, and create dynamic content for the dropped component.

In the demo below, you'll see some common configuration options in action.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{// ...
    blocks: {
      default: [\
        {\
          id: 'image-1',\
          label: 'Image',\
          // Put the block in a category\
          category: 'Images',\
          media: '<svg viewBox="0 0 24 24"><path d="M20 5a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V7c0-1.1.9-2 2-2h16M5 16h14l-4.5-6-3.5 4.5-2.5-3L5 16Z"/></svg>',\
          content: { type: 'image', src: 'https://picsum.photos/seed/my-image-1/100/100' },\
          // Select the created component when dropped in the canvas\
          select: true\
        },\
        {\
          id: 'image-2',\
          label: 'Activate Image',\
          category: 'Images',\
          media: '<svg viewBox="0 0 24 24"><path d="M20 5a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V7c0-1.1.9-2 2-2h16M5 16h14l-4.5-6-3.5 4.5-2.5-3L5 16Z"/></svg>',\
          content: { type: 'image', src: 'https://picsum.photos/seed/my-image-2/100/100' },\
          select: true,\
          // The Component View of the 'image' has a custom 'onActive' method (opens Asset Manager)\
          // that will be triggered when the component is dropped in the canvas.\
          activate: true\
        },\
        {\
          id: 'section-1',\
          label: 'Full block',\
          category: 'Sections',\
          media: '<svg viewBox="0 0 24 24"><path d="M21 3H3c-.6 0-1 .4-1 1v6c0 .6.4 1 1 1h18c.6 0 1-.4 1-1V4c0-.6-.4-1-1-1m0 10H3c-.6 0-1 .4-1 1v6c0 .6.4 1 1 1h18c.6 0 1-.4 1-1v-6c0-.6-.4-1-1-1Z"/></svg>',\
          content: '<section>Simple section</section>',\
          // Full width block\
          full: true\
        },\
        {\
          id: 'section-2',\
          label: 'Your media',\
          category: 'Sections',\
          media: '<img src="https://picsum.photos/seed/my-image-10/200/200" style="width: 200px; height: 100px; display: block; object-fit: cover; border-radius: 3px;"/>',\
          content: '<section>Media as image</section>',\
          full: true,\
          // This will remove default classes of the block\
          attributes: { class: 'custom-class' }\
        },\
        {\
          id: 'section-3',\
          label: 'Disabled',\
          category: 'Sections',\
          media: '<svg viewBox="0 0 24 24"><path d="M12 2c5.5 0 10 4.5 10 10s-4.5 10-10 10S2 17.5 2 12 6.5 2 12 2m0 2c-1.9 0-3.6.6-4.9 1.7l11.2 11.2c1-1.4 1.7-3.1 1.7-4.9a8 8 0 0 0-8-8m4.9 14.3L5.7 7.1a8 8 0 0 0 11.2 11.2Z"/></svg>',\
          content: '<section>Disabled</section>',\
          // Disable the block\
          disable: true,\
          // Custom onClick callback\
          onClick(block) {\
            alert(`Block ${block.getLabel()} disabled`);\
          }\
        },\
        {\
          id: 'section-4',\
          label: 'Dynamic',\
          category: 'Sections',\
          media: '<svg viewBox="0 0 24 24"><path d="M7 11H1v2h6v-2m2.2-3.2L7 5.6 5.5 7.1l2.2 2 1.4-1.3M13 1h-2v6h2V1m5.4 6L17 5.7l-2.2 2.2 1.4 1.4L18.4 7M17 11v2h6v-2h-6m-5-2a3 3 0 0 0-3 3 3 3 0 0 0 3 3 3 3 0 0 0 3-3 3 3 0 0 0-3-3m2.8 7.2 2.1 2.2 1.5-1.4-2.2-2.2-1.4 1.4m-9.2.8 1.5 1.4 2-2.2-1.3-1.4L5.6 17m5.4 6h2v-6h-2v6Z"/></svg>',\
          // Content as a function, you can return the HTML string or a Component Defintion\
          content: () => `<section>Section created at: ${new Date().toUTCString()}</section>`\
        }\
      ]
    },
    // Custom default project for demo purpose
    project: {/**/},
    // Custom editor layout for demo purpose
    layout: {/**/}
  }}
/>

```

![Blocks initialization custom](https://app.grapesjs.com/docs-sdk/assets/images/blocks-init-props-24515aa03eef4ad919e92bf09328b298.webp)

## Programmatic usage [​](https://app.grapesjs.com/docs-sdk/configuration/blocks#programmatic-usage "Direct link to Programmatic usage")

If you need to manage blocks dynamically, you can use the [GrapesJS Blocks APIs](https://grapesjs.com/docs/api/block_manager.html).

In the demo below, you'll see how to add, update, and remove blocks using `editor.Blocks` API. Here, we update the blocks on load via a custom plugin, but these methods can be applied at any time.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{// ...
    plugins: [\
      // Example of a custom plugin to manage blocks\
      editor => {\
        const { Blocks } = editor;\
        const blockMedia = '<svg viewBox="0 0 24 24"><path d="M19,5H22V7H19V10H17V7H14V5H17V2H19V5M17,19V13H19V21H3V5H11V7H5V19H17Z" /></svg>';\
        const newCategory = {\
          id: 'new-category',\
          label: 'New Category',\
          icon: '<svg viewBox="0 0 24 24"><path d="M7 11H1v2h6v-2m2.2-3.2L7 5.6 5.5 7.1l2.2 2 1.4-1.3M13 1h-2v6h2V1m5.4 6L17 5.7l-2.2 2.2 1.4 1.4L18.4 7M17 11v2h6v-2h-6m-5-2a3 3 0 0 0-3 3 3 3 0 0 0 3 3 3 3 0 0 0 3-3 3 3 0 0 0-3-3m2.8 7.2 2.1 2.2 1.5-1.4-2.2-2.2-1.4 1.4m-9.2.8 1.5 1.4 2-2.2-1.3-1.4L5.6 17m5.4 6h2v-6h-2v6Z"/></svg>'\
        };\
\
        // Remove already defined blocks (keep only 'text' and 'image')\
        const allBlocksId = Blocks.getAll().map(block => block.id);\
        const blocksToKeep = ['text', 'image'];\
        allBlocksId.forEach(blockId => {\
          if (!blocksToKeep.includes(blockId)) {\
            Blocks.remove(blockId);\
          }\
        });\
\
        // Add new blocks\
        Blocks.add('block-id-1', {\
          label: 'Block 1',\
          media: blockMedia,\
          category: newCategory,\
          content: '<div>Block 1</div>'\
        }, {\
          at: 0 // Let's place this block at the beginning of the list\
        });\
\
        Blocks.add('block-id-2', {\
          label: 'Block 2',\
          media: blockMedia,\
          category: newCategory,\
          content: '<div>Block 2</div>'\
        });\
\
        Blocks.add('block-id-3', {\
          label: 'Block 3',\
          media: blockMedia,\
          category: 'Another Category',\
          content: '<div>Block 3</div>'\
        });\
\
        // Update defined blocks\
        const blockImage = Blocks.get('image');\
        blockImage?.set({\
          label: 'Image (updated)',\
          content: '<img src="https://placehold.co/200/777/white.png?text=Image+Updated"/>'\
        });\
\
    },\
    ],
    // Custom default project for demo purpose
    project: {/**/},
    // Custom editor layout for demo purpose
    layout: {/**/}
  }}
/>

```

### Events [​](https://app.grapesjs.com/docs-sdk/configuration/blocks#events "Direct link to Events")

You can also subscribe to block events to trigger custom actions. For a complete list of available events, check the reference [here](https://grapesjs.com/docs/api/block_manager.html#available-events).

In the demo below, we demonstrate how to use block events to update a label during drag operations and disable a block based when the component changes.

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
    plugins: [\
      editor => {\
        const { Blocks, Components } = editor;\
\
        // Remove all blocks for the demo purpose\
        Blocks.getAll().map(block => block.id).forEach(id => Blocks.remove(id));\
\
        // Create a new component\
        Components.addType('cmp-unique', {\
          model: {\
            defaults: {\
              copyable: false,\
              components: '{{ PLACEHOLDER_COMPONENT }}',\
              style: { 'text-align': 'center', color: 'red' }\
            }\
          }\
        });\
\
        // Add a new block\
        Blocks.add('block-unique', {\
          label: 'Unique Block',\
          media: '<svg viewBox="0 0 24 24"><path d="M19,5H22V7H19V10H17V7H14V5H17V2H19V5M17,19V13H19V21H3V5H11V7H5V19H17Z" /></svg>',\
          content: { type: 'cmp-unique' },\
          full: true\
        });\
\
        const cleanUpBlockDrag = block => {\
          block.set({ label: block.getLabel().replace('(Drag)', '').trim() });\
        };\
\
        // Update block during drag operations\
        editor.on(Blocks.events.dragStart, block => {\
          block.set({ label: `${block.getLabel()} (Drag)` });\
        });\
        editor.on(Blocks.events.dragEnd, (createdComponent, block) => {\
          cleanUpBlockDrag(block);\
        });\
\
        // Disable block based on the component\
        editor.on(Components.events.add, cmp => {\
          if (cmp.get('type') === 'cmp-unique') {\
            const block = Blocks.get('block-unique');\
            cleanUpBlockDrag(block);\
            block.set({ disable: true });\
          }\
        });\
        editor.on(Components.events.removed, cmp => {\
          if (cmp.get('type') === 'cmp-unique') {\
            Blocks.get('block-unique').set({ disable: false });\
          }\
        });\
\
    },\
    ],
    // Custom default project for demo purpose
    project: {/**/},
    // Custom editor layout for demo purpose
    layout: {/**/}
  }}
/>

```

## I18n [​](https://app.grapesjs.com/docs-sdk/configuration/blocks#i18n "Direct link to I18n")

All block manager labels are integrated with [I18n GrapesJS module](https://grapesjs.com/docs/modules/I18n.html). Below is a reference for each available key.

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
          blockManager: {
            notFound: "No blocks found",
            blocks: "Blocks",
            add: "Add more blocks",
            search: "Search...",
            types: {
              regular: "Regular",
              symbols: "Symbols"
            },
            symbols: {
              notFound: "No symbols found",
              instancesProject: "Instance/s in the project",
              delete: "Delete symbol",
              deleteConfirm: "Are you sure you want to delete the symbol? All instances inside the project will be detached."
            }
          },
        }
      }
    },
    // Custom default project for demo purpose
    project: {/**/},
    // Custom editor layout for demo purpose
    layout: {/**/}
  }}
/>

```

Since blocks and categories can be added dynamically, they must be referenced by their ID.

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
          blockManager: {
            labels: {
              block1: 'Block 1 (EN)',
              block2: 'Block 2 (EN)',
            },
            categories: {
              category1: 'Category 1 (EN)',
              category2: 'Category 2 (EN)',
            },
          }
        }
      }
    },
    plugins: [\
      editor => {\
        const { Blocks } = editor;\
        const blockMedia = '<svg viewBox="0 0 24 24"><path d="M19,5H22V7H19V10H17V7H14V5H17V2H19V5M17,19V13H19V21H3V5H11V7H5V19H17Z" /></svg>';\
        // Remove all blocks for the demo purpose\
        Blocks.getAll().map(block => block.id).forEach(id => Blocks.remove(id));\
\
        Blocks.add('block1', {\
          label: 'Block 1',\
          media: blockMedia,\
          category: { id: 'category1', label: 'Category 1' },\
          content: '<div>Block 1</div>'\
        });\
\
        Blocks.add('block2', {\
          label: 'Block 2',\
          media: blockMedia,\
          category: { id: 'category2', label: 'Category 2' },\
          content: '<div>Block 2</div>'\
        });\
\
    }\
    ],

    // Custom default project for demo purpose
    project: {/**/},
    // Custom editor layout for demo purpose
    layout: {/**/}
  }}
/>

```

## Customization [​](https://app.grapesjs.com/docs-sdk/configuration/blocks#customization "Direct link to Customization")

The Block Manager UI is rendered using the built-in [`panelBlocks` layout component](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelblocks). You can create multiple custom `panelBlocks`, each with its own set of [properties](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelblocks-properties).

In the demo below, we showcase different ways to customize the `panelBlocks` component.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

function openBlocks(editor, layout) {
  editor.runCommand('studio:layoutUpdate', {
    id: 'blockPanelContainer',
    layout,
    header: false,
    placer: { type: 'static', layoutId: 'hiddenColumnForBlocks' },
    style: { width: 250, height: '100%', borderRightWidth: 1 }
  });
}

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
            resizable: false,\
            style: { padding: '10px 5px', alignItems: 'center', width: 45, gap: 10 },\
            children: [\
              {\
                type: 'button',\
                icon: '<svg viewBox="0 0 24 24"><path d="M14,17H12V9H10V7H14M19,3H5A2,2 0 0,0 3,5V19A2,2 0 0,0 5,21H19A2,2 0 0,0 21,19V5A2,2 0 0,0 19,3Z" /></svg>',\
                tooltip: 'Simple panel',\
                onClick: ({ editor }) => {\
                  openBlocks(editor, {\
                    type: 'panelBlocks',\
                    symbols: false,\
                    search: false\
                  });\
                }\
              },\
              {\
                type: 'button',\
                icon: '<svg viewBox="0 0 24 24"><path d="M15,11C15,12.11 14.1,13 13,13H11V15H15V17H9V13C9,11.89 9.9,11 11,11H13V9H9V7H13A2,2 0 0,1 15,9M19,3H5A2,2 0 0,0 3,5V19A2,2 0 0,0 5,21H19A2,2 0 0,0 21,19V5A2,2 0 0,0 19,3Z" /></svg>',\
                tooltip: 'Hide categories',\
                onClick: ({ editor }) => {\
                  openBlocks(editor, {\
                    type: 'panelBlocks',\
                    symbols: false,\
                    hideCategories: true,\
                    /**\
                     * Customize with your own CSS.\
                     * Here we'll use this CSS rule:\
                     * .custom-blocks-layout .gs-block-manager__blocks {\
                     *   font-size: 0.7rem;\
                     *   grid-template-columns: repeat(auto-fill, minmax(55px, 1fr));\
                     * }\
                     */\
                    className: 'custom-blocks-layout'\
                  });\
                }\
              },\
              {\
                type: 'button',\
                icon: '<svg viewBox="0 0 24 24"><path d="M15,10.5A1.5,1.5 0 0,1 13.5,12C14.34,12 15,12.67 15,13.5V15C15,16.11 14.11,17 13,17H9V15H13V13H11V11H13V9H9V7H13C14.11,7 15,7.89 15,9M19,3H5C3.91,3 3,3.9 3,5V19A2,2 0 0,0 5,21H19C20.11,21 21,20.1 21,19V5A2,2 0 0,0 19,3Z" /></svg>',\
                tooltip: 'Filtered Blocks',\
                onClick: ({ editor }) => {\
                  openBlocks(editor, {\
                    type: 'panelBlocks',\
                    symbols: false,\
                    // Filter blocks by category\
                    blocks: ({ blocks }) => {\
                      return blocks.filter(block => block.category?.getLabel() === 'Basic');\
                    }\
                  });\
                }\
              },\
              {\
                type: 'button',\
                icon: '<svg viewBox="0 0 24 24"><path d="M15,17H13V13H9V7H11V11H13V7H15M19,3H5A2,2 0 0,0 3,5V19A2,2 0 0,0 5,21H19A2,2 0 0,0 21,19V5A2,2 0 0,0 19,3Z" /></svg>',\
                tooltip: 'Custom block layout',\
                onClick: ({ editor }) => {\
                  openBlocks(editor, {\
                    type: 'panelBlocks',\
                    symbols: false,\
                    itemLayout: ({ block, attributes }) =>\
                      block.id === 'icon' ? null // Skip custom layout for the 'icon' block\
                      : {\
                          type: 'column',\
                          className: `my-custom-block ${attributes.className}`,\
                          style: { backgroundColor: 'white', borderRadius: 1000 },\
                          htmlAttrs: attributes, // Reuse original attributes (eg. to keep the drag and drop logic)\
                          children: [\
                            {\
                              type: 'text',\
                              content: block.getLabel(),\
                              style: { textAlign: 'center' },\
                            },\
                            {\
                              type: 'custom',\
                              className: 'gs-utl-block-media',\
                              style: { width: 30, margin: '0 auto' },\
                              render: () => block.getMedia()\
                            }\
                          ]\
                        }\
                  });\
                }\
              }\
            ]\
          },\
          { id: 'hiddenColumnForBlocks', type: 'column' },\
          { type: 'canvas' }\
        ]
      }
    },

  }}
/>

```

- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/blocks#initialization)
- [Configuration](https://app.grapesjs.com/docs-sdk/configuration/blocks#configuration)
- [Programmatic usage](https://app.grapesjs.com/docs-sdk/configuration/blocks#programmatic-usage)
  - [Events](https://app.grapesjs.com/docs-sdk/configuration/blocks#events)
- [I18n](https://app.grapesjs.com/docs-sdk/configuration/blocks#i18n)
- [Customization](https://app.grapesjs.com/docs-sdk/configuration/blocks#customization)

---

# File: app.grapesjs.com_docs-sdk_configuration_components_methods.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/components/methods"
title: "Component Methods | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/components/methods#__docusaurus_skipToContent_fallback)

On this page

When you add a component to the document, you create a **Component Model** instance. The model stores the component's properties, which are [saved in the project JSON](https://app.grapesjs.com/docs-sdk/configuration/projects#storage) and used to generate the final [export code](https://app.grapesjs.com/docs-sdk/configuration/projects#export).

Alongside the model, a **Component View** instance controls how the component appears in the canvas. By default, it renders based on the model's structure (eg. tagName, attributes, nested components), but you can customize its behavior as needed.

This page covers the methods available in both instances, helping you manipulate and extend components effectively.

## Table of Contents

- [Component Model](https://app.grapesjs.com/docs-sdk/configuration/components/methods#component-model)
- [Component View](https://app.grapesjs.com/docs-sdk/configuration/components/methods#component-view)

## Component Model [​](https://app.grapesjs.com/docs-sdk/configuration/components/methods#component-model "Direct link to Component Model")

The Component Model instance stores the component's properties and persists them in the project. It provides methods to access and update properties, manage child components, and define how the component is exported.

It's also possible to extend the model, which allows you to provide custom logic and behavior.

### Creating Model [​](https://app.grapesjs.com/docs-sdk/configuration/components/methods#creating-model "Direct link to Creating Model")

You can create new Component Models via [Component Definitions](https://app.grapesjs.com/docs-sdk/configuration/components/overview#component-definition) or HTML strings.

```codeBlockLines_AdAo
const rootComponentModel = editor.getWrapper();
const addedComponentModels = rootComponentModel.append(`
  <div>Div content</div>
  <span>Span content</span>
`);
//
const divComponentModel = addedComponentModels[0];
const spanComponentModel = addedComponentModels[1];

```

Since we didn't define custom component types, these models use default configurations.

### Accessing and Updating Model [​](https://app.grapesjs.com/docs-sdk/configuration/components/methods#accessing-and-updating-model "Direct link to Accessing and Updating Model")

The Component Model provides [various methods](https://grapesjs.com/docs/api/component.html) that allows you to access and update the [component's properties](https://app.grapesjs.com/docs-sdk/configuration/components/properties) and their children programmatically.

```codeBlockLines_AdAo
// Access the properties
const allProperties = componentModel.props();
const cmpRemovable = componentModel.get('removable');

// Update the properties
componentModel.set({ removable: false });

// Get component attributes (exported as HTML attributes)
const attrs = componentModel.getAttributes();

if (!attrs.title) {
  componentModel.addAttributes({ title: 'New title' });
}

// Access child components
const children = componentModel.components();
children.forEach((childModel) => console.log(childModel.get('tagName')));

// Replace children (HTML or Component Definition)
const newComponents = componentModel.components(`<div>Content 1</div><div>Content 2</div>`);

// Remove component
componentModel.remove();

// Find inner components by type
const firstComponentFromFind = componentModel.findType('image')[0];

// Get parent component
const parentComponent = componentModel.parent();

// Closest parent component by type
const closestComponent = componentModel.closestType('some-parent-type');

// Get export code
const componentHTML = componentModel.toHTML();

// Get component definition as JSON
const componentJSON = JSON.parse(JSON.stringify(componentModel));

```

### Extending Models [​](https://app.grapesjs.com/docs-sdk/configuration/components/methods#extending-models "Direct link to Extending Models")

To extend the Component Model, you have to define custom component types using the `editor.Components.addType` method. This allows you to create new component types, update or extend existing ones.

In the demo below, we'll define two components:

- `component-a`: Implements a custom export method ( `toHTML`) to change its generated output.
- `component-b`: Extends `component-a`, adding further customizations to the export method.

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
    plugins: [\
      editor => {\
        // Create new component type\
        editor.Components.addType('component-a', {\
          isComponent: el => el.classList?.contains('cmp-a'),\
          model: {\
            defaults: {\
              attributes: { title: 'Title Component' }\
            },\
\
            init() {\
              console.log('Component A init');\
            },\
\
            // Update the export code\
            toHTML() {\
              return `<custom-component-a>\
                <div>Model tagName: ${this.get('tagName')}</div>\
                <div>Model attributes: ${JSON.stringify(this.getAttributes())}</div>\
                ${this.getInnerHTML()}\
              </custom-component-a>`;\
            }\
          },\
        });\
\
        // Update component type\
        editor.Components.addType('component-a', {\
          model: {\
            defaults: {\
              attributes: { title: 'Title Component A' }\
            }\
          }\
        });\
\
        // Extend component type\
        const ComponentA = editor.Components.getType('component-a').model;\
\
        editor.Components.addType('component-b', {\
          isComponent: el => el.classList?.contains('cmp-b'),\
          // Indicate the type to extend\
          extend: 'component-a',\
          // Array of model methods to extend from 'component-a'\
          extendFn: ['init'],\
          model: {\
            defaults: {\
              attributes: { title: 'Title Component B' }\
            },\
\
            init() {\
              // With defined 'extendFn', also the init method from 'component-a' will be called\
              console.log('Component B init');\
            },\
\
            toHTML() {\
              const resultCmpA = ComponentA.prototype.toHTML.apply(this);\
              return `<custom-component-b>${resultCmpA}</custom-component-b>`;\
            }\
          },\
        });\
      }\
    ],
    project: {
      default: {
        pages: [{\
          name: 'Home',\
          component: `\
            <section class="cmp-a">\
              <div>Content Component A</div>\
            </section>\
            <hr/>\
            <div class="cmp-b">\
              <div>Content Component B</div>\
            </div>\
          `\
        }]
      }
    },
    // Custom editor layout for demo purpose
    layout: {/**/}
  }}
/>

```

In the next example, we'll define a few component types and demonstrate how they interact using custom methods.

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
    plugins: [\
      editor => {\
        // Create a component type for sections\
        editor.Components.addType('section-component', {\
          isComponent: el => el.tagName === 'SECTION',\
          model: {\
            defaults: {\
              style: { padding: '30px', display: 'flex', gap: '25px', 'background-color': 'lightgray' },\
              traits: [{\
                type: 'button',\
                label: 'Add new card',\
                command: (editor, trait) => trait.component.addNewCard(),\
              }]\
            },\
\
            addNewCard() {\
              this.append({\
                type: 'card-component',\
                cardTitle: 'New card'\
              });\
            },\
          },\
        });\
\
        // Create a component type for cards\
        editor.Components.addType('card-component', {\
          isComponent: el => el.classList?.contains('card'),\
          model: {\
            defaults: {\
              style: { padding: '10px', 'border-radius': '10px', 'background-color': 'white', width: '25%' },\
              cardTitle: 'Card title',\
              components: (model) => `\
                <h2>${model.get('cardTitle')}</h2>\
                <img style="width: 100%; height: 100px; object-fit: cover;" src="https://picsum.photos/seed/card-image/100/100"/>\
                <p>Card content</p>\
              `,\
              traits: [{\
                type: 'text',\
                name: 'cardTitle',\
                changeProp: true, // Update properties, not attributes\
              }]\
            },\
\
            init() {\
              this.on('change:cardTitle', this.handleCardTitleChange);\
            },\
\
            handleCardTitleChange() {\
              // Update the content of the <h2> tag\
              const h2CmpModel = this.components().find(c => c.get('tagName') === 'h2');\
              h2CmpModel.components('Updated: ' + this.get('cardTitle'));\
            }\
          },\
        });\
\
        editor.onReady(() => {\
          const cmpSection = editor.getWrapper().findType('section-component')[0];\
          editor.select(cmpSection);\
        });\
      }\
    ],
    project: {
      default: {
        pages: [{\
          name: 'Home',\
          component: '<section><div class="card"></div></section>'\
        }]
      }
    },
    // Custom editor layout for demo purpose
    layout: {/**/}
  }}
/>

```

### Lifecycle Methods and Events [​](https://app.grapesjs.com/docs-sdk/configuration/components/methods#lifecycle-methods-and-events "Direct link to Lifecycle Methods and Events")

The model offers methods and events to track changes and execute custom logic throughout the component's lifecycle.

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
    plugins: [\
      editor => {\
        editor.Components.addType('my-custom-component', {\
          model: {\
            defaults: {\
              customProp: 'Initial value',\
              attributes: { title: 'HTML title' },\
              components: 'Some content',\
              traits: [\
                { name: 'customProp', changeProp: true },\
                { name: 'title' },\
              ]\
            },\
\
            init() {\
              console.log('Component model created');\
              // Listen to property changes\
              this.on('change:customProp', this.handlePropChange);\
              // Listen to attribute changes\
              this.on('change:attributes', this.handleAttrChange);\
              // Listen to the title attribute change\
              this.on('change:attributes:title', this.handleTitleChange);\
            },\
\
            removed() {\
              console.log('Component removed');\
            },\
\
            handlePropChange() {\
              console.log('New customProp value:', this.get('customProp'));\
            },\
\
            handleAttrChange() {\
              console.log('Attributes updated:', this.getAttributes());\
            },\
\
            handleTitleChange() {\
              console.log('Attribute title updated:', this.getAttributes().title);\
            },\
          },\
        });\
      }\
    ],
    project: {
      default: {
        pages: [{\
          name: 'Home',\
          component: '<div data-gjs-type="my-custom-component"></div>'\
        }]
      }
    },
    // Custom editor layout for demo purpose
    layout: {/**/}
  }}
/>

```

## Component View [​](https://app.grapesjs.com/docs-sdk/configuration/components/methods#component-view "Direct link to Component View")

The **Component View** controls how a component appears and behaves in the editor canvas. By default, it reflects the **Component Model** properties ( `tagName`, `attributes`, nested `components`, etc.), but you can extend it to improve editing, handle special DOM behaviors, or add custom interactions.

For example, the `text` component includes a Rich Text Editor for inline editing, while the `image` component opens the asset manager on double-click. Custom views let you add UI elements, event listeners, or dynamic content—all without affecting the final exported code.

### Customizing View [​](https://app.grapesjs.com/docs-sdk/configuration/components/methods#customizing-view "Direct link to Customizing View")

A Component View instance is created alongside the Component Model when the component is rendered in the editor canvas.

In the demo below, you'll find an example of a fully customized view leveraging built-in and custom methods, along with DOM event listeners and model change listeners.

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
    plugins: [\
      editor => {\
        editor.Components.addType('custom-view', {\
          model: {\
            defaults: {\
              tagName: 'section',\
              droppable: false,\
              attributes: { title: 'Section title' },\
              components: '<div>Content from model</div>',\
              traits: [{\
                name: 'title',\
                label: 'Update title'\
              }]\
            }\
          },\
          view: {\
            // Use custom tagName, which overrides the one provided by the model\
            tagName: 'div',\
\
            // Add DOM event listeners\
            events: () => ({\
              dblclick: 'onActive',\
              // Add event listeners to inner elements\
              'click img': 'changeWithRandomImage'\
            }),\
\
            // Called once the Component View instance is created\
            init() {\
              // Listen to model events\
              this.listenTo(this.model, 'change:attributes:title', this.updateImageWithTitle);\
            },\
\
            // Called on component attributes update (eg. new attribute added from the model)\
            onAttrUpdate() {\
              this.el.style.cssText = 'border: 5px solid #8b5cf6;';\
            },\
\
            // Called when the element is rendered.\
            onRender() {\
              const { el } = this;\
              const imageStyle = 'display: block; margin: 0 auto; width: 150px; height: 150px; cursor: help;';\
              el.innerHTML = `\
                <ul>\
                  <li>Double-click anywhere on the element to open the Asset Manager</li>\
                  <li>Click on the image to change it with a random one</li>\
                  <li>Update the image by changing the component title</li>\
                </ul>\
                <img style="${imageStyle}" src="${this.getImageWithTitle()}" />\
              `;\
            },\
\
            // Called when the element is removed from the canvas.\
            // Useful to cleanup global event listeners or other resources.\
            removed() {\
              console.log('Element removed');\
            },\
\
            onActive() {\
              editor.Assets.open({\
                select: asset => {\
                  const imageEl = this.el.querySelector('img');\
                  imageEl?.setAttribute('src', asset.getSrc());\
                }\
              });\
            },\
\
            changeWithRandomImage() {\
              const imageEl = this.el.querySelector('img');\
              const randomImage = `https://picsum.photos/seed/random-${Math.random()}/150`;\
              imageEl?.setAttribute('src', randomImage);\
            },\
\
            updateImageWithTitle() {\
              const imageEl = this.el.querySelector('img');\
              imageEl?.setAttribute('src', this.getImageWithTitle());\
            },\
\
            getImageWithTitle() {\
              const modelAttrs = this.model.getAttributes();\
              return `https://placehold.co/150/777/white.png?text=${modelAttrs.title}`;\
            }\
          }\
        });\
      }\
    ],
    project: {
      default: {
        assets: Array(100).fill(0).map((_, i) => `https://picsum.photos/seed/${i}/200/200`),
        pages: [{\
          name: 'Home',\
          component: '<div data-gjs-type="custom-view"></div>'\
        }]
      }
    },
    // Custom editor layout for demo purpose
    layout: {/**/}
  }}
/>

```

The key aspect of the Component View is that it controls how the component behaves inside the editor canvas but doesn't affect the final exported code. You can add UI elements, event listeners, or dynamic content without modifying the HTML output.

### Extending Views [​](https://app.grapesjs.com/docs-sdk/configuration/components/methods#extending-views "Direct link to Extending Views")

Just like the Component Model, you can extend the Component View of existing components to customize or extend their behavior.

In the demo below, we define and extend multiple components to demonstrate how to customize the rendering loigc:

- `component-a`: Defines a base component with a default structure, styling logic, and custom rendering behavior.
- `component-b`: Extends `component-a`, overriding its styling and custom content method.
- `component-c`: Extends the model of `component-a` and the view of `component-b`, inheriting and modifying its rendering behavior.

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
    plugins: [\
      editor => {\
        editor.Components.addType('component-a', {\
          model: {\
            defaults: {\
              droppable: false,\
              components: '<div>Content from model</div>',\
            }\
          },\
          view: {\
            getColor: () => '#8b5cf6',\
\
            getCustomContent: () => 'Default custom content',\
\
            onAttrUpdate() {\
              this.el.style.cssText = `border: 5px solid ${this.getColor()}`;\
            },\
\
            onRender() {\
              if (!this.el.querySelector('.cmp-a'))  {\
                this.el.insertAdjacentHTML('afterbegin', `\
                  <div class="cmp-a" style="background-color: ${this.getColor()}; margin: 10px; padding: 10px; color: white;">\
                    ${this.getCustomContent()}\
                  </div>\
                `);\
              }\
            },\
          }\
        });\
\
        // Update component view\
        editor.Components.addType('component-a', {\
          view: {\
            getCustomContent: () => 'Custom content A',\
          }\
        });\
\
        // Extend component view (without extending the model)\
        editor.Components.addType('component-b', {\
          // Indicate the view type to extend\
          extendView: 'component-a',\
          view: {\
            getColor: () => '#ef5cf6',\
            getCustomContent: () => 'Custom content B',\
          }\
        });\
\
        // Extend component view\
        const ComponentViewB = editor.Components.getType('component-b').view;\
\
        editor.Components.addType('component-c', {\
          extendView: 'component-b',\
          // Array of view methods to extend from 'component-b'\
          extendFnView: ['onRender'],\
          // Extend the model of 'component-a'\
          extend: 'component-a',\
          view: {\
            getColor: () => '#f6845c',\
\
            getCustomContent() {\
              const resultB = ComponentViewB.prototype.getCustomContent.apply(this);\
              return resultB + ' + C';\
            },\
\
            onRender() {\
              if (!this.el.querySelector('.cmp-c'))  {\
                this.el.insertAdjacentHTML('afterbegin', `\
                  <div class="cmp-c" style="background-color: ${this.getColor()}; margin: 10px; padding: 10px; color: white;">\
                    Custom content C\
                  </div>\
                `);\
              }\
            },\
          }\
        });\
      }\
    ],
    project: {
      default: {
        assets: Array(100).fill(0).map((_, i) => `https://picsum.photos/seed/${i}/200/200`),
        pages: [{\
          name: 'Home',\
          component: `<div style="display: flex; padding: 10px; gap: 10px">\
            <div data-gjs-type="component-a"></div>\
            <div data-gjs-type="component-b"></div>\
            <div data-gjs-type="component-c"></div>\
          </div>`\
        }]
      }
    },
    // Custom editor layout for demo purpose
    layout: {/**/}
  }}
/>

```

### Model and View Interaction [​](https://app.grapesjs.com/docs-sdk/configuration/components/methods#model-and-view-interaction "Direct link to Model and View Interaction")

The Component View can interact with the Component Model, enabling you to update properties, attributes, or child components directly from the view. This is useful when you need to modify the model (and, consequently, the export code) based on user interactions in the canvas.

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
    plugins: [\
      editor => {\
        editor.Components.addType('cmp-with-data', {\
          model: {\
            defaults: {\
              components: `\
                <h2 style="text-align: center">Random products</h2>\
                <div style="display: flex; gap: 10px;" data-products="true"></div>\
              `,\
            },\
\
            init() {\
              this.fetchProducts();\
            },\
\
            getProductsContainer() {\
              return this.components().find(child => child.getAttributes()['data-products']);\
            },\
\
            async fetchProducts() {\
              const result = await fetch('https://dummyjson.com/products');\
              const productsRes = await result.json();\
              const shuffled = productsRes.products.sort(() => 0.5 - Math.random());\
              const firstProducts = shuffled.slice(0, 4);\
              const container = this.getProductsContainer();\
              container.components(''); // cleanup container\
\
              firstProducts.forEach(product => {\
                container.append(`\
                  <article style="width: 25%; display: flex; flex-direction: column; justify-content: space-between;">\
                    <h1>${product.title}</h1>\
                    <img style="width: 100%; height: 100px; object-fit: cover;" src="${product.thumbnail}"/>\
                    <p>${product.description}</p>\
                    <h3>Price: $${product.price}</h3>\
                  </article>\
                `);\
              });\
            }\
          },\
          view: {\
            events: {\
              'click .load-products': 'fetchNewProducts',\
            },\
\
            fetchNewProducts() {\
              // Get the view element from an inner component\
              const container = this.model.getProductsContainer();\
              const containerEl = container.getView().el;\
              containerEl.innerHTML = '<h2>Loading...</h2>';\
              this.model.fetchProducts();\
            },\
\
            onRender() {\
              if (!this.el.querySelector('.load-products'))  {\
                this.el.insertAdjacentHTML('afterbegin', `\
                  <button class="load-products" style="background-color: #8b5cf6; color: white; margin: 10px 0; padding: 10px; border: none; cursor: pointer">\
                    Load New Products\
                  </button>\
                `);\
              }\
            },\
          }\
        });\
      }\
    ],
    project: {
      default: {
        pages: [{\
          name: 'Home',\
          component: `<div data-gjs-type="cmp-with-data"></div>`\
        }]
      }
    },
    // Custom editor layout for demo purpose
    layout: {/**/}
  }}
/>

```

- [Component Model](https://app.grapesjs.com/docs-sdk/configuration/components/methods#component-model)
  - [Creating Model](https://app.grapesjs.com/docs-sdk/configuration/components/methods#creating-model)
  - [Accessing and Updating Model](https://app.grapesjs.com/docs-sdk/configuration/components/methods#accessing-and-updating-model)
  - [Extending Models](https://app.grapesjs.com/docs-sdk/configuration/components/methods#extending-models)
  - [Lifecycle Methods and Events](https://app.grapesjs.com/docs-sdk/configuration/components/methods#lifecycle-methods-and-events)
- [Component View](https://app.grapesjs.com/docs-sdk/configuration/components/methods#component-view)
  - [Customizing View](https://app.grapesjs.com/docs-sdk/configuration/components/methods#customizing-view)
  - [Extending Views](https://app.grapesjs.com/docs-sdk/configuration/components/methods#extending-views)
  - [Model and View Interaction](https://app.grapesjs.com/docs-sdk/configuration/components/methods#model-and-view-interaction)

---

# File: app.grapesjs.com_docs-sdk_configuration_components_overview.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/components/overview"
title: "Components | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/components/overview#__docusaurus_skipToContent_fallback)

On this page

warning

This page is a work in progress.

Components are the building blocks of a Studio SDK project. A component can be a simple element like an image or text box, or a more complex structure composed of multiple nested components, such as sections or even entire pages.

## Table of Contents

- [When do you need Components?](https://app.grapesjs.com/docs-sdk/configuration/components/overview#when-do-you-need-components)
- [How do Components work?](https://app.grapesjs.com/docs-sdk/configuration/components/overview#how-do-components-work)
- [Component Properties](https://app.grapesjs.com/docs-sdk/configuration/components/overview#component-properties)
- [Component Methods](https://app.grapesjs.com/docs-sdk/configuration/components/overview#component-methods)

## When do you need Components? [​](https://app.grapesjs.com/docs-sdk/configuration/components/overview#when-do-you-need-components "Direct link to When do you need Components?")

The Components allows developers to define custom behaviors, interactions, and constraints such as specifying what can be edited, how the component is rendered, and where it can be placed on the page.

With the Components API, you can:

- **Control editing behavior**: Define which parts of a component are editable and how users interact with it (e.g., enabling inline text editing or restricting modifications).
- **Customize rendering**: Modify how the component is structured in the DOM and how it exports to the final HTML.
- **Define placement rules**: Set constraints on where components can be dropped or nested within other components.
- **Bind dynamic actions**: Attach custom logic to user interactions, such as triggering the Asset Manager when an image is double-clicked.

By leveraging these features, developers can create highly interactive and reusable components tailored to their needs.

## How do Components work? [​](https://app.grapesjs.com/docs-sdk/configuration/components/overview#how-do-components-work "Direct link to How do Components work?")

Let's start exploring how components work by adding some basic components to a new project.

tip

Check the **Demo** tab to the see the code in action.

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
      // Define a default project that acts as a fallback when no Storage is configured.
      default: {
        pages: [{ name: 'Home', component: '<h1>Home page</h1>' }]
      }
    },
    plugins: [\
      // Use a plugin as a playground for our examples\
      editor => {\
        // Wait for the project to load before interacting with its components\
        editor.onReady(() => {\
          // The first page is selected by default\
          const homePage = editor.Pages.getSelected();\
          // The main component (usually the <body>) is created with the page\
          const rootComponent = homePage.getMainComponent();\
\
          // Append a new component as an HTML string\
          const newComponent = rootComponent.append(`<div>\
            <img src="https://picsum.photos/seed/my-image/150/150"/>\
            <p>Content text here</p>\
          </div>`)[0];\
        });\
      }\
\
    ],
  }}
/>

```

### Component Definition [​](https://app.grapesjs.com/docs-sdk/configuration/components/overview#component-definition "Direct link to Component Definition")

The Component Definition is an object that details the configuration of a Component.

A complete Component Definition may contain additional information, but it can be as simple as you want:

```codeBlockLines_AdAo
{
  "tagName": "div",
  "components": [ // Nested components\
    {\
      "type": "image", // Detected as an image type\
      "attributes": { "src": "https://picsum.photos/seed/my-image/150/150" }\
    },\
    {\
      "type": "text", // Detected as an editable text\
      "tagName": "p",\
      "components": [ // Nested components\
        { "type": "textnode", "content": "Content text here" }\
      ]\
    }\
  ]
}

```

The editor automatically recognizes predefined Components like `image` or `text`, and applies the appropriate behavior in the canvas. This allows you to double-click on the image to open the [Asset Manager](https://app.grapesjs.com/docs-sdk/configuration/assets/overview) or edit a text directly in the canvas.

In the first example of this page we import elements from an HTML string, which then gets parsed and transformed into a Component Definition automatically.

You can append components by passing in a component definition object instead of using HTML:

```codeBlockLines_AdAo
const newComponent = rootComponent.append({
  tagName: 'div',
  components: [\
    {\
      type: 'image',\
      attributes: { src: 'https://picsum.photos/seed/my-image/150/150' }\
    },\
    {\
      type: 'text',\
      tagName: 'p',\
      components: [{ type: 'textnode', content: 'Content text here' }]\
    }\
  ]
})[0];

```

In the next section, we'll define a **custom component** and learn how to make the editor recognize it after parsing the HTML string.

### Define a Custom Component [​](https://app.grapesjs.com/docs-sdk/configuration/components/overview#define-a-custom-component "Direct link to Define a Custom Component")

To define a custom component, use the `editor.Components.addType` method. This allows you to create a new component type or extend an existing one.

Using the `isComponent` property, you can specify how the editor should recognize the component when parsing an HTML element.

warning

Custom components should always be defined inside a plugin to ensure proper integration.

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
    plugins: [\
      editor => {\
        // Register a new component type\
        editor.Components.addType('card', {\
          // Make the editor understand how to recognize the component from parsed HTML\
          isComponent: el => el.classList?.contains('card'),\
          // Provide the default properties of the component (more about it in the next section).\
          model: {\
            defaults: {\
              attributes: { 'data-custom-attr': 'my-value' },\
            }\
          }\
        });\
\
        editor.onReady(() => {\
          // ...\
\
          // Add the 'card' class to make our div component recognizable by the custom type\
          const component = rootComponent.append(`<div class="card">\
            <img src="https://picsum.photos/seed/my-image/150/150"/>\
            <p>Content text here</p>\
          </div>`)[0];\
\
          alert('New component added: ' + component.get('type'));\
\
        });\
      }\
\
    ],
  }}
/>

```

### Add Components to Blocks [​](https://app.grapesjs.com/docs-sdk/configuration/components/overview#add-components-to-blocks "Direct link to Add Components to Blocks")

Your new Component can now be used in the editor canvas, but you still need to bind it to a block for it to show up in the Block Manager, an UI where your users can select component types to add into the canvas.

tip

You can open the block manager by clicking the purple plus icon in the top right corner of the editor.

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
    plugins: [\
    editor => {\
        // ...\
        // Add new custom component type to block manager\
        editor.Blocks.add('card', {\
          label: 'Card',\
          category: 'My Components',\
          media: '<svg xmlns="http://www.w3.org/2000/svg" height="40px" viewBox="0 -960 960 960" width="40px" fill="#e8eaed"><path d="M560-440h200v-80H560v80Zm0-120h200v-80H560v80ZM200-320h320v-22q0-45-44-71.5T360-440q-72 0-116 26.5T200-342v22Zm160-160q33 0 56.5-23.5T440-560q0-33-23.5-56.5T360-640q-33 0-56.5 23.5T280-560q0 33 23.5 56.5T360-480ZM160-160q-33 0-56.5-23.5T80-240v-480q0-33 23.5-56.5T160-800h640q33 0 56.5 23.5T880-720v480q0 33-23.5 56.5T800-160H160Zm0-80h640v-480H160v480Zm0 0v-480 480Z"/></svg>',\
          content: '<div class="card"><img src="https://picsum.photos/seed/my-image/150/150"/><p>Content text here</p></div>'\
        });\
        // ...\
      }\
\
    ],
  }}
/>

```

## Component Properties [​](https://app.grapesjs.com/docs-sdk/configuration/components/overview#component-properties "Direct link to Component Properties")

Every custom component can have its own properties, along with a set of built-in properties that can be customized or extended.

To explore all available properties and learn how to use them to customize component behavior in the editor, refer to the [Component Properties page](https://app.grapesjs.com/docs-sdk/configuration/components/properties).

## Component Methods [​](https://app.grapesjs.com/docs-sdk/configuration/components/overview#component-methods "Direct link to Component Methods")

Every component created inside the editor has a component instance that provide a set of built-in methods that you can use to manipulate components, retrieve their properties, or listen to events.

To explore the available methods, how to extend and use them to customize component behavior in the editor, refer to the [Component Methods page](https://app.grapesjs.com/docs-sdk/configuration/components/methods).

warning

This page is a work in progress.

- [When do you need Components?](https://app.grapesjs.com/docs-sdk/configuration/components/overview#when-do-you-need-components)
- [How do Components work?](https://app.grapesjs.com/docs-sdk/configuration/components/overview#how-do-components-work)
  - [Component Definition](https://app.grapesjs.com/docs-sdk/configuration/components/overview#component-definition)
  - [Define a Custom Component](https://app.grapesjs.com/docs-sdk/configuration/components/overview#define-a-custom-component)
  - [Add Components to Blocks](https://app.grapesjs.com/docs-sdk/configuration/components/overview#add-components-to-blocks)
- [Component Properties](https://app.grapesjs.com/docs-sdk/configuration/components/overview#component-properties)
- [Component Methods](https://app.grapesjs.com/docs-sdk/configuration/components/overview#component-methods)

---

# File: app.grapesjs.com_docs-sdk_configuration_components_properties.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/components/properties"
title: "Component Properties | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/components/properties#__docusaurus_skipToContent_fallback)

On this page

In this section, we'll explore the various properties that can be defined for a component. These properties control how the editor renders the component in the canvas, define its functionalities and behaviors, and determine how it is exported in the final HTML.

## Table of Contents

- [Structural Properties](https://app.grapesjs.com/docs-sdk/configuration/components/properties#structural-properties)
- [Behavior Properties](https://app.grapesjs.com/docs-sdk/configuration/components/properties#behavior-properties)
- [Visual Properties](https://app.grapesjs.com/docs-sdk/configuration/components/properties#visual-properties)
- [Styling Properties](https://app.grapesjs.com/docs-sdk/configuration/components/properties#styling-properties)
- [Script Properties](https://app.grapesjs.com/docs-sdk/configuration/components/properties#script-properties)
- [Traits](https://app.grapesjs.com/docs-sdk/configuration/components/properties#traits)
- [Toolbar](https://app.grapesjs.com/docs-sdk/configuration/components/properties#toolbar)
- [Context Menu](https://app.grapesjs.com/docs-sdk/configuration/components/properties#context-menu)
- [Properties from HTML](https://app.grapesjs.com/docs-sdk/configuration/components/properties#properties-from-html)

## Structural Properties [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#structural-properties "Direct link to Structural Properties")

The core properties of a component define how it is structured, rendered, and exported. Most components, except for certain types like `textnode` or `comment`, are built using fundamental properties such as `tagName`, `attributes`, and optionally, nested `components`.

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
    plugins: [\
      editor => {\
        editor.Components.addType('customSpan', {\
          isComponent: el => el.classList?.contains('cls-custom-span'),\
          model: {\
            defaults: {\
              tagName: 'span',\
              attributes: { class: 'cls-custom-span', style: 'color: red' },\
              // If not provided, use these default nested components\
              components: 'Custom <b>SPAN</b> with default content',\
            }\
          }\
        });\
\
       editor.Components.addType('customDiv', {\
         isComponent: el => el.classList?.contains('cls-custom-div'),\
         model: {\
             defaults: {\
               // tagName: 'div', // default is already 'div'\
               attributes: { class: 'cls-custom-div', style: 'color: blue; padding: 10px 0', 'data-custom-prop': 'value' },\
               // You can mix HTML strings with component defintions\
               components: [\
                 'Custom <b>DIV</b>, <span class="cls-custom-span">Custom <b>SPAN</b> with custom content</span> and ',\
                 { type: 'customSpan' }\
               ],\
             }\
         }\
        });\
\
        editor.onReady(() => {\
          // ...\
          // 'editor.getWrapper()' is the same as 'editor.Pages.getSelected().getMainComponent()'\
          const rootComponent = editor.getWrapper();\
\
          // Add new custom components\
          rootComponent.append([\
            { type: 'customDiv' },\
            '<div class="cls-custom-div">Custom <b>DIV</b> with custom content</div>'\
          ]);\
        });\
      }\
    ],
    project: {
      // Empty project for our demo purpose
      default: { pages: [{ name: 'Home' }] }
    },
    // Custom editor layout for our demo purpose
    layout: {
      default: {
        type: 'column',
        style: { height: '100%' },
        children: [\
          {\
            type: 'row',\
            style: { padding: 10, gap: 10 },\
            children: [\
                {\
                    type: 'button',\
                    variant: 'primary',\
                    label: 'Show component HTML',\
                    // Print the HTML of the root component\
                    onClick: ({ editor }) => alert(editor.getWrapper().toHTML({ keepInlineStyle: true }))\
                },\
                {\
                    type: 'button',\
                    variant: 'outline',\
                    label: 'Show component JSON',\
                    // Print the JSON of the root component\
                    onClick: ({ editor }) => alert(JSON.stringify(editor.getWrapper(), null, 2))\
                }\
            ]\
          },\
          { type: 'canvas' }\
        ]
      }
    },
  }}
/>

```

#### Structural properties list [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#structural-properties-list "Direct link to Structural properties list")

Show properties

| Property   | Type                  | Description                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type       | string                | Indicates the component type.<br>**Default**<br>`codeBlockLines_AdAo<br>'default'<br>`                                                                                                                                                                                                                   |
| tagName    | string                | Specifies the HTML tag for the component.<br>**Default**<br>`codeBlockLines_AdAo<br>'div'<br>`                                                                                                                                                                                                           |
| attributes | object                | A key-value object defining the component's attributes.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "title": "My title",<br>  "data-some-prop": "some-value"<br>}<br>`<br>**Default**<br>`codeBlockLines_AdAo<br>{}<br>`                                                                            |
| components | array, string, object | Defines child components. Can be an HTML string, a component definition object, or an array containing both.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  "<div>Component as HTML</div>",<br>  {<br>    "type": "myComponentType"<br>  }<br>]<br>`<br>**Default**<br>`codeBlockLines_AdAo<br>[]<br>` |
| void       | boolean               | Indicates whether the component is a self-closing HTML element (e.g., <br/>, <hr/>).<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                                                                               |

## Behavior Properties [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#behavior-properties "Direct link to Behavior Properties")

By default, most components do not have specific restrictions on how they behave inside the editor, aside from standard HTML rules (e.g., an image cannot contain child components). However, the editor allows you to customize component behavior using built-in properties.

The demo below showcases common customizations and how they impact component behavior within the editor.

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
    plugins: [\
      editor => {\
        editor.Components.addType('componentA', {\
          isComponent: el => el.classList?.contains('component-a'),\
          model: {\
            defaults: {\
              // Restrict this component to accept only the "componentA-item" inside\
              droppable: '.component-a-item',\
              // Disable dragging this component\
              draggable: false,\
              attributes: { class: 'component-a', style: 'width: 100%; background: darkred; padding: 20px; margin: 20px' },\
            }\
          }\
        });\
\
        editor.Components.addType('componentA-item', {\
          isComponent: el => el.classList?.contains('component-a-item'),\
          model: {\
            defaults: {\
              // Restrict this component to be dragged only inside the "componentA"\
              draggable: '.component-a',\
              // Avoid dropping anything inside this component\
              droppable: false,\
              attributes: { class: 'component-a-item', style: 'width: 100%; background: red; color: white; padding: 10px; margin: 10px 0' },\
            }\
          }\
        });\
\
        editor.Components.addType('componentB', {\
          isComponent: el => el.classList?.contains('component-b'),\
          model: {\
            // ComponentB doesn't have any restrictions\
            defaults: {\
              attributes: { class: 'component-b', style: 'width: 100%; background: darkblue; padding: 20px; margin: 20px' },\
            }\
          }\
        });\
\
        editor.Components.addType('componentB-item', {\
          isComponent: el => el.classList?.contains('component-b-item'),\
          model: {\
            defaults: {\
              draggable: '.component-b',\
              droppable: false,\
              attributes: { class: 'component-b-item', style: 'width: 100%; background: blue; color: white; padding: 10px; margin: 10px 0' },\
            }\
          }\
        });\
\
        editor.Components.addType('componentC', {\
          isComponent: el => el.classList?.contains('component-c'),\
          model: {\
            defaults: {\
              removable: false, // Disable the remove\
              copyable: false, // Disable the copy\
              // For more advanced use cases, you can rely on functions\
              draggable: (source, target, index) => {\
                return target.is('wrapper') || (target.is('componentB') && index === 0);\
              },\
              attributes: { class: 'component-c', style: 'width: 100%; background: purple; color: white; padding: 10px; margin: 10px 0' },\
              components: "Component C - you can't remove or copy me. You can only drag me around the main component or as the first one inside 'ComponentB'.",\
            }\
          }\
        });\
\
        editor.Components.addType('component-no-select', {\
          isComponent: el => el.classList?.contains('component-no-select'),\
          model: {\
            defaults: {\
              selectable: false,\
              attributes: { class: 'component-no-select', style: 'width: 100%; background: gray; color: white; padding: 10px; margin: 10px 0' },\
              components: "You can't select me",\
            }\
          }\
        });\
\
        editor.Components.addType('component-no-select-hover', {\
          isComponent: el => el.classList?.contains('component-no-select-hover'),\
          model: {\
            defaults: {\
              selectable: false,\
              hoverable: false,\
              attributes: { class: 'component-no-select-hover', style: 'width: 100%; background: gray; color: white; padding: 10px; margin: 10px 0' },\
              components: "You can't select or hover me, <u>but you can with inner components</u>",\
            }\
          }\
        });\
\
        editor.Components.addType('component-locked', {\
          isComponent: el => el.classList?.contains('component-locked'),\
          model: {\
            defaults: {\
              locked: true,\
              attributes: { class: 'component-locked', style: 'width: 100%; background: gray; color: white; padding: 10px; margin: 10px 0' },\
              components: "I'm locked, you can't interact with me and this also affects <u>inner components</u>",\
            }\
          }\
        });\
\
\
        editor.onReady(() => {\
          // ...\
          rootComponent.append(`\
            <div style="display: flex; gap: 20px">\
                <div class="component-a">\
                    <div class="component-a-item">Component A item 1</div>\
                    <div class="component-a-item">Component A item 2</div>\
                    <div class="component-a-item">Component A item 3</div>\
                </div>\
                <div class="component-b">\
                    <div class="component-b-item">Component B item 1</div>\
                    <div class="component-b-item">Component B item 2</div>\
                    <div class="component-b-item">Component B item 3</div>\
                </div>\
            </div>\
            <div class="component-c"></div>\
            <div class="component-no-select"></div>\
            <div class="component-no-select-hover"></div>\
            <div class="component-locked"></div>\
            <style>\
              html {\
                font-family: system-ui, sans-serif;\
                font-size: 1.5rem;\
              }\
            </style>\
          `);\
\
        });\
      }\
    ],
  }}
/>

```

#### Behavior properties list [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#behavior-properties-list "Direct link to Behavior properties list")

Show properties

| Property   | Type                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ---------- | ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| copyable   | boolean                   | Allow component to be clonable.<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| draggable  | string, boolean, function | Indicates whether the component can be dragged into other components. Accepts a boolean (false prevents dragging), a query selector string to specify allowed drop targets, or a function for advanced logic (returns true to allow dragging, false to prevent it).<br>**Example**<br>`codeBlockLines_AdAo<br>// Use false to indicate the component is not draggable into other components  <br>draggable: false, <br>// Allows dragging only into elements matching the selector <br>draggable: '.some-class, [data-gjs-type=row]',<br>// Use a function for more advanced logic (expects a Boolean as result) <br>// Example: Allows dragging only into the wrapper component <br>draggable: (source, target, index) => target.is('wrapper')<br>`<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>` |
| droppable  | string, boolean, function | Indicates whether other components can be dropped inside this component. Accepts a boolean (false prevents dropping), a query selector string to define allowed child components, or a function for advanced logic (returns true to allow dropping, false to prevent it).<br>**Example**<br>`codeBlockLines_AdAo<br>// Use false to prevent other components from being dropped inside  <br>droppable: false, <br>// Use a query selector string to specify allowed child components <br>droppable: '.some-class, [data-gjs-type=column]',<br>// Use a function for more advanced logic (expects a Boolean as result) <br>// Example: Allows dropping only images inside <br>droppable: (source, target, index) => source.is('image')<br>`<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>`           |
| hoverable  | boolean                   | Shows an outline border when hovering the element<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| locked     | boolean                   | Disable any interaction of the component and its children in the canvas. You can unlock inner children by setting its locked property to false.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| removable  | boolean                   | Allow component to be removable.<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| selectable | boolean                   | Allow to select the component.<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

## Visual Properties [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#visual-properties "Direct link to Visual Properties")

Visual Properties define how components are presented within the editor's interface, ensuring a more intuitive and consistent editing experience.

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
    plugins: [\
      editor => {\
        editor.Components.addType('custom-name-icon', {\
          model: {\
            defaults: {\
              name: 'Custom Name & Icon',\
              icon: '<svg viewBox="0 0 24 24"><path d="M16 9h3l-5 7m-4-7h4l-2 8M5 9h3l2 7m5-12h2l2 3h-3m-5-3h2l1 3h-4M7 4h2L8 7H5m1-5L2 8l10 14L22 8l-4-6H6Z"/></svg>',\
              attributes: { style: 'padding: 20px; color: red' },\
              components: 'Component with custom name and icon',\
            }\
          }\
        });\
\
        editor.Components.addType('no-badge', {\
          model: {\
            defaults: {\
              name: 'Without badge',\
              badgable: false,\
              attributes: { style: 'padding: 20px; color: blue' },\
              components: "If you hover me, you won't see any badge",\
            }\
          }\
        });\
\
        editor.Components.addType('no-layer', {\
          model: {\
            defaults: {\
              name: 'Without layer',\
              layerable: false,\
              attributes: { style: 'padding: 20px; color: green' },\
              components: "You won't see me in Layers",\
            }\
          }\
        });\
\
        editor.Components.addType('no-highlightable', {\
          model: {\
            defaults: {\
              name: 'Without outline',\
              highlightable: false,\
              attributes: { style: 'padding: 20px; color: green' },\
              components: "Component without outline, when enabled",\
            }\
          }\
        });\
\
        editor.onReady(() => {\
          editor.getWrapper().append([\
            { type: 'custom-name-icon' },\
            { type: 'no-badge' },\
            { type: 'no-layer' },\
            { type: 'no-highlightable' },\
          ]);\
        });\
      }\
    ],
    project: {
      // Empty project for our demo purpose
      default: { pages: [{ name: 'Home' }] }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          { type: 'panelLayers', style: { width: 300 } },\
          {\
            type: 'canvasSidebarTop',\
            sidebarTop: {\
                leftContainer: { buttons: [] },\
                rightContainer: {\
                  buttons: ({ items }) => [{\
                    ...items.find(item => item.id === 'componentOutline'),\
                    label: 'Toggle outline'\
                  }],\
                },\
            }\
          },\
        ]
      }
    }
  }}
/>

```

#### Visual properties list [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#visual-properties-list "Direct link to Visual properties list")

Show properties

| Property      | Type    | Description                                                                                                                              |
| ------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| name          | string  | Name of the component that will be displayed in the editor interface.<br>**Example**<br>`codeBlockLines_AdAo<br>'My component name'<br>` |
| icon          | string  | Icon of the component that will be displayed in the editor interface.<br>**Example**<br>`codeBlockLines_AdAo<br>'<svg>...</svg>'<br>`    |
| badgable      | boolean | Show the badge of the component name in the editor canvas.<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>`                          |
| highlightable | boolean | Show component outline in the canvas when the outline command is enabled.<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>`           |
| layerable     | boolean | Allow to show the component in the Layers panel.<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>`                                    |

## Styling Properties [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#styling-properties "Direct link to Styling Properties")

Components support configurable styles and controlled styling options. You can define component-specific styles, CSS rules applied per component type, and specify which properties are editable in the Style Manager.

The demo below demonstrates how to apply and customize these style-related properties.

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
    plugins: [\
      editor => {\
        editor.Components.addType('component-style', {\
          model: {\
            defaults: {\
              style: { color: 'red', padding: '20px' },\
              components: 'Styles per component',\
            }\
          }\
        });\
\
        editor.Components.addType('component-style-class', {\
          model: {\
            defaults: {\
              attributes: { class: 'component-style' },\
              styles: '.component-style { color: green; padding: 20px }',\
              components: 'Styles with class selector',\
            }\
          }\
        });\
\
        editor.Components.addType('component-only-color', {\
          model: {\
            defaults: {\
              stylable: ['color'],\
              style: { color: 'blue', padding: '20px' },\
              components: 'Allow updating only the color change',\
            }\
          }\
        });\
\
        editor.Components.addType('component-skip-color', {\
          model: {\
            defaults: {\
              unstylable: ['color'],\
              style: { color: 'darkblue', padding: '20px' },\
              components: 'Do not allow updating the color',\
            }\
          }\
        });\
\
        editor.onReady(() => {\
          editor.getWrapper().append([\
            { type: 'component-style' },\
            { type: 'component-style-class' },\
            { type: 'component-only-color' },\
            { type: 'component-skip-color' },\
          ]);\
        });\
      }\
    ],
    project: {
      // Empty project for our demo purpose
      default: { pages: [{ name: 'Home' }] }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'canvasSidebarTop',\
            sidebarTop: {\
                leftContainer: { buttons: [] },\
                rightContainer: {\
                  buttons: ({ items }) => [{\
                    ...items.find(item => item.id === 'showCode'),\
                    variant: 'outline',\
                    label: 'Show code'\
                  }],\
                },\
            }\
          },\
          { type: 'panelSidebarTabs', style: { width: 300 } }\
        ]
      }
    }
  }}
/>

```

#### Styling properties list [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#styling-properties-list "Direct link to Styling properties list")

Show properties

| Property   | Type           | Description                                                                                                                                                                                                                                                                                                                                             |
| ---------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| style      | object         | Styles to apply on a single component. Useful for styles that may vary per instance of a component. Does not support media queries or pseudo-selectors.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "width": "100px",<br>  "background-color": "red"<br>}<br>`                                                                                     |
| styles     | string         | Styles applied at component type level. Can be useful when all the components of the type share the same styles. Supports all the CSS features like media queries or pseudo-selectors.<br>**Example**<br>`codeBlockLines_AdAo<br>'.my-cmp { color: red; } @media (max-width: 600px) { .my-cmp { color: blue; } }'<br>`                                  |
| stylable   | boolean, array | Indicate if the component could be styled via the Style Manager. Can be a boolean or an array of specific CSS properties that are allowed to be modified, other properties will be hidden from the Style Manager.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  "color",<br>  "width"<br>]<br>`<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>` |
| unstylable | array          | Opposite of 'stylable', Specifies CSS properties that should be hidden from the Style Manager.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  "height",<br>  "font-size"<br>]<br>`<br>**Default**<br>`codeBlockLines_AdAo<br>[]<br>`                                                                                                                  |

## Script Properties [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#script-properties "Direct link to Script Properties")

Script properties allow you to attach custom JavaScript behavior to your components. These properties are useful for adding interactivity, dynamic functionality, or any custom logic. This enable you to export the scripts along with the HTML output of the component, ensuring only the script of components dropped in the page are included with the export.

This section explores how to define and use script-related properties for more advanced customization of your components.

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
    plugins: [\
      editor => {\
        editor.Components.addType('component-with-script', {\
          model: {\
            defaults: {\
              style: { color: 'red', padding: '20px' },\
              components: 'Component with script',\
              customProp: 'defaultValue',\
              'script-props': ['customProp'],\
              script: function (props) {\
                const el = this;\
                setInterval(() => {\
                  const time = new Date().toLocaleTimeString();\
                  el.innerHTML = 'Prop value: "' + props.customProp + '" - time: ' + time;\
                }, 1000)\
              },\
            }\
          }\
        });\
\
        editor.Components.addType('component-custom-export', {\
          // Extend the previous component but provide a custom script for the export\
          extend: 'component-with-script',\
          model: {\
            defaults: {\
              style: { color: 'blue', padding: '20px' },\
              'script-export': function (props) {\
                console.log('Custom export', props.customProp);\
              },\
            }\
          }\
        });\
\
        editor.onReady(() => {\
          editor.getWrapper().append([\
            { type: 'component-with-script', customProp: 'customValue1' },\
            { type: 'component-custom-export', customProp: 'customValue2' },\
          ]);\
        });\
      }\
    ],
    project: {
      // Empty project for our demo purpose
      default: { pages: [{ name: 'Home' }] }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'canvasSidebarTop',\
            sidebarTop: {\
                leftContainer: { buttons: [] },\
                rightContainer: {\
                  buttons: ({ items }) => [{\
                    ...items.find(item => item.id === 'showCode'),\
                    variant: 'outline',\
                    label: 'Show code'\
                  }],\
                },\
            }\
          },\
        ]
      }
    }
  }}
/>

```

#### Script properties list [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#script-properties-list "Direct link to Script properties list")

Show properties

| Property      | Type     | Description                                                                                                                              |
| ------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| script        | function | Script to attach to the component<br>**Example** <br>`codeBlockLines_AdAo<br>function () { console.log('Component script', this) }<br>`  |
| script-props  | array    | Component properties to pass to the script<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  "name",<br>  "customProp"<br>]<br>`          |
| script-export | function | Custom script to use in the export<br>**Example** <br>`codeBlockLines_AdAo<br>function () { console.log('Component script', this) }<br>` |

## Traits [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#traits "Direct link to Traits")

![Component Traits](https://app.grapesjs.com/docs-sdk/assets/images/component-traits-9e21de20b4621beaa5948294838c420c.png)

Traits are used to allow end-users to interact with and modify the state of components, such as their HTML attributes or any other component properties. Traits can be thought of as form fields or controls that provide a user-friendly way to adjust component configurations directly within the editor.

You can create custom traits or use built-in ones to make your components more configurable. Traits can be linked to specific component properties, and their values can be easily adjusted from the Traits Manager, which is part of the editor UI.

In Studio, Traits are referred to as _Properties_ by default, following a more commonly used naming convention. They are available in the built-in right sidebar tabs, but you can customize the label for your end-users (e.g., renaming it to _Settings_).

![Traits default position](https://app.grapesjs.com/docs-sdk/assets/images/component-traits-default-position-ead5888b72c1a6d270d1b94221423368.png)

tip

If you need to modify the layout, including where Traits appear, refer to the [Layout Configuration page](https://app.grapesjs.com/docs-sdk/configuration/layout/overview).

In the demo below, we showcase some of the built-in traits and their uses.

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
    plugins: [\
      editor => {\
        editor.Components.addType('cmp-with-traits', {\
          model: {\
            defaults: {\
              components: 'Component with Traits',\
              attributes: { 'data-simple-text': 'Default attribute value' },\
              traitCustomProp: 'Default property value',\
              traits: [\
                {\
                  name: 'data-simple-text',\
                  label: 'Trait on attribute',\
                },\
                {\
                  name: 'traitCustomProp',\
                  label: 'Trait on property',\
                  changeProp: true, // Read/write to component properties instead of attributes\
                },\
                {\
                  type: 'number',\
                  name: 'number-type',\
                  min: 0,\
                  value: 50,\
                  max: 1000,\
                  step: 100,\
                },\
                {\
                  type: 'select',\
                  name: 'select-type',\
                  value: 'opt2',\
                  options: [\
                    { id: 'opt1', label: 'Option 1' },\
                    { id: 'opt2', label: 'Option 2' },\
                  ],\
                },\
                {\
                  type: 'checkbox',\
                  name: 'checkbox-type',\
                  label: 'Boolean value',\
                  value: true,\
                },\
                {\
                  type: 'color',\
                  name: 'color-type',\
                  value: 'red',\
                },\
                {\
                  type: 'button',\
                  label: 'Click me',\
                  command: (editor) => alert(editor.getSelected()?.getName()),\
                },\
                {\
                  type: 'radio',\
                  name: 'radio-type',\
                  value: 'opt2',\
                  options: [\
                    { id: 'opt1', label: 'Option 1' },\
                    { id: 'opt2', label: 'Option 2' },\
                    {\
                      id: 'opt3',\
                      icon: '<svg viewBox="0 0 24 24"><path d="M19 13h-6v6h-2v-6H5v-2h6V5h2v6h6v2Z"/></svg>',\
                      title: 'Custom icon',\
                    },\
                  ],\
                },\
                // Studio built-in traits\
                {\
                  type: 'image',\
                  name: 'image-type',\
                  value: 'https://picsum.photos/seed/image/300/300',\
                },\
                {\
                  type: 'code',\
                  name: 'code-type',\
                  value: '<div>Code type</div>',\
                  typeProps: { language: 'html', clean: true, padding: 5 },\
                },\
                {\
                  type: 'stack',\
                  name: 'customStackValue',\
                  label: 'Stack type',\
                  addItem: () => ({\
                    id: Math.random().toString(36).substring(2, 7),\
                    itemValue: 'New value',\
                  }),\
                  labelItem: (item) => `${item.id}: ${item.itemValue}`,\
                  getValue({ component, trait }) {\
                    return component.get(trait.getName()) || [];\
                  },\
                  setValue: ({ component, emitUpdate, trait, value }) => {\
                    component.set({ [trait.getName()]: value });\
                    emitUpdate();\
                  },\
                  properties: [\
                    { name: 'id', label: 'Key' },\
                    { name: 'itemValue', label: 'Value' },\
                  ],\
                },\
                {\
                  type: 'custom',\
                  name: 'from-custom',\
                  label: 'Custom React Component',\
                  value: 'Default value',\
                  // Use categories to group traits\
                  category: {\
                    id: 'my-category',\
                    label: 'Custom Traits',\
                    icon: '<svg viewBox="0 0 24 24"><path d="M12 2A10 10 0 0 0 2 12a10 10 0 0 0 10 10 10 10 0 0 0 10-10A10 10 0 0 0 12 2m3.5 6A1.5 1.5 0 0 1 17 9.5a1.5 1.5 0 0 1-1.5 1.5A1.5 1.5 0 0 1 14 9.5 1.5 1.5 0 0 1 15.5 8m-7 0A1.5 1.5 0 0 1 10 9.5 1.5 1.5 0 0 1 8.5 11 1.5 1.5 0 0 1 7 9.5 1.5 1.5 0 0 1 8.5 8m3.5 9.5A5.5 5.5 0 0 1 6.9 14H17c-.8 2-2.8 3.5-5.1 3.5Z"/></svg>',\
                    open: false,\
                  },\
                  component: ({ trait }) => <div>\
                      <label>{trait.getLabel()}</label>\
                      <input type="text" value={trait.getValue()} onChange={(ev) => trait.setValue(ev.target.value)} />\
                  </div>,\
                },\
                {\
                  type: 'custom',\
                  name: 'from-custom-js',\
                  label: 'Custom JS Component',\
                  value: 'Default JS value',\
                  // Use a category declared before\
                  category: { id: 'my-category' },\
                  render({ trait, addEl, removeEl, onUpdate }) {\
                    const rootEl = document.createElement('div');\
                    rootEl.innerHTML = `<label>${trait.getLabel()}</label><input type="text"/>`;\
                    const input = rootEl.querySelector('input');\
                    input.value = trait.getValue();\
                    input.addEventListener('change', (ev: any) => trait.setValue(ev.target.value));\
                    addEl(rootEl);\
                    onUpdate(() => { input.value = trait.getValue() });\
                    return () => removeEl(rootEl);\
                  }\
              }\
              ],\
            },\
          },\
        });\
\
        editor.onReady(() => {\
          const cmp = editor.getWrapper().append({ type: 'cmp-with-traits' })[0];\
          editor.select(cmp);\
        });\
      }\
    ],
    project: {
      // Empty project for our demo purpose
      default: { pages: [{ name: 'Home' }] }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'canvasSidebarTop',\
            sidebarTop: {\
              rightContainer: {\
                buttons: [\
                  {\
                    type: 'button',\
                    variant: 'primary',\
                    size: 'm',\
                    icon: 'codeBraces',\
                    label: 'Show component JSON',\
                    onClick: ({ editor }) => alert(JSON.stringify(editor.getSelected() || {}, null, 2))\
                  }\
                ],\
              },\
            }\
          },\
          { type: 'panelProperties', style: { padding: '10px 10px 100px', width: 300 } },\
        ],
      },
    },
  }}
/>

```

## Toolbar [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#toolbar "Direct link to Toolbar")

![Component Toolbar](https://app.grapesjs.com/docs-sdk/assets/images/component-toolbar-0e776190c285c4081def4496d7671842.png)

The Toolbar is a component-related set of actions/commands that appear in the canvas near the selected component.

You can customize the toolbar content globally through the `components.toolbar` option or on a per-component basis with the `toolbarItems` property.

Below is an example showing how to configure the toolbar both globally and for individual components.

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
        pages: [\
          {\
            name: 'Home',\
            component: `\
              <div style="padding: 20px; max-width: 400px; margin: 0 auto; display: flex; flex-direction: column;">\
                <h1 style="font-size: 3rem">Heading component</h1>\
                <div style="margin: 20px 0; font-size: 2rem">Text component</div>\
                <img src="https://picsum.photos/seed/image1/300/300"/>\
              <div>\
            `\
          }\
        ]
      }
    },
    plugins: [\
      editor => {\
        // Update toolbar for the Heading components\
        editor.Components.addType('heading', {\
          model: {\
            defaults: {\
              toolbarItems: ({ items, component }) => [\
                ...items, // default component items\
                {\
                  id: 'component-update',\
                  label:\
                    '<svg viewBox="0 0 24 24"><path d="M21,10.12H14.22L16.96,7.3C14.23,4.6 9.81,4.5 7.08,7.2C4.35,9.91 4.35,14.28 7.08,17C9.81,19.7 14.23,19.7 16.96,17C18.32,15.65 19,14.08 19,12.1H21C21,14.08 20.12,16.65 18.36,18.39C14.85,21.87 9.15,21.87 5.64,18.39C2.14,14.92 2.11,9.28 5.62,5.81C9.13,2.34 14.76,2.34 18.27,5.81L21,3V10.12M12.5,8V12.25L16,14.33L15.28,15.54L11,13V8H12.5Z" /></svg>',\
                  command: () => {\
                    component.components('Updated heading: ' + new Date().toLocaleTimeString());\
                  }\
                }\
              ]\
            }\
          }\
        });\
        // Update toolbar for the Image components\
        editor.Components.addType('image', {\
          model: {\
            defaults: {\
              toolbarItems: ({ component }) => [\
                // skipping default items\
                {\
                  id: 'component-copy-html',\
                  label:\
                    '<svg viewBox="0 0 24 24"><path d="M12.89,3L14.85,3.4L11.11,21L9.15,20.6L12.89,3M19.59,12L16,8.41V5.58L22.42,12L16,18.41V15.58L19.59,12M1.58,12L8,5.58V8.41L4.41,12L8,15.58V18.41L1.58,12Z" /></svg>',\
                  command: () => {\
                    navigator.clipboard.writeText(component.toHTML());\
                    alert('Image HTML copied to clipboard');\
                  }\
                },\
                {\
                  id: 'component-fetch-image',\
                  label:\
                    '<svg viewBox="0 0 24 24"><path d="M8.5 13.5L5 18H13.03C13.11 19.1 13.47 20.12 14.03 21H5C3.9 21 3 20.11 3 19V5C3 3.9 3.9 3 5 3H19C20.1 3 21 3.89 21 5V11.18C20.5 11.07 20 11 19.5 11C17.78 11 16.23 11.67 15.07 12.76L14.5 12L11 16.5L8.5 13.5M19 20C17.62 20 16.5 18.88 16.5 17.5C16.5 17.1 16.59 16.72 16.76 16.38L15.67 15.29C15.25 15.92 15 16.68 15 17.5C15 19.71 16.79 21.5 19 21.5V23L21.25 20.75L19 18.5V20M19 13.5V12L16.75 14.25L19 16.5V15C20.38 15 21.5 16.12 21.5 17.5C21.5 17.9 21.41 18.28 21.24 18.62L22.33 19.71C22.75 19.08 23 18.32 23 17.5C23 15.29 21.21 13.5 19 13.5Z" /></svg>',\
                  command: async () => {\
                    await fetch('https://picsum.photos/200').then(async ({ url }) => {\
                      component.setAttributes({ src: url });\
                    });\
                  }\
                }\
              ]\
            }\
          }\
        });\
      }\
    ],
    components: {
      toolbar: ({ items, component }) => {
        return [\
          {\
            id: 'global-preview',\
            label:\
              '<svg viewBox="0 0 24 24"><path d="M12,9A3,3 0 0,0 9,12A3,3 0 0,0 12,15A3,3 0 0,0 15,12A3,3 0 0,0 12,9M12,17A5,5 0 0,1 7,12A5,5 0 0,1 12,7A5,5 0 0,1 17,12A5,5 0 0,1 12,17M12,4.5C7,4.5 2.73,7.61 1,12C2.73,16.39 7,19.5 12,19.5C17,19.5 21.27,16.39 23,12C21.27,7.61 17,4.5 12,4.5Z" /></svg>',\
            command: 'core:preview'\
          },\
          // default items that you can 'map' and filter'\
          ...items,\
          {\
            id: 'global-info',\
            label:\
              '<svg viewBox="0 0 24 24"><path d="M13,9H11V7H13M13,17H11V11H13M12,2A10,10 0 0,0 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12A10,10 0 0,0 12,2Z" /></svg>',\
            command: () => {\
              alert('Component name: ' + component.getName());\
            }\
          }\
        ];
      }
    },

  }}
/>

```

The `components.toolbar` function (global) and the `toolbarItems` property (component-specific) both receive the same arguments:

- `items`: An array of default items that you can modify or filter (e.g., via `items.map` or `items.filter`).
- `component`: The component instance the menu is being called for.
- `editor`: The studio editor instance.

The `toolbar` function should return an array of toolbar items. The default items array is first passed to any component-specific configuration, then to the global one. If the final result is an empty array, the toolbar won't be displayed.

You can execute an action by providing a string value to the `command` property. The command should be registered in the editor instance (see [default commands](https://grapesjs.com/docs/modules/Commands.html#default-commands)). You can also provide a function to the `command` property to execute custom logic, including asynchronous operations.

## Context Menu [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#context-menu "Direct link to Context Menu")

![Component Context Menu](https://app.grapesjs.com/docs-sdk/assets/images/component-context-menu-9302b58ac7e6df204b2f7158734943e7.png)

The Context Menu is a dropdown that appears when you right-click on a component in either the canvas or the layers panel. It's also accessible via the component toolbar, which is especially useful on devices without right-click capability.

You can customize the context menu content globally through the `components.contextMenu` option or on a per-component basis with the `contextMenu` property.

Below is an example showing how to configure the context menu both globally and for individual components.

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
        pages: [\
          {\
            name: 'Home',\
            component: `\
              <div style="padding: 20px; max-width: 400px; margin: 0 auto; display: flex; flex-direction: column;">\
                <h1 style="font-size: 3rem">Heading component</h1>\
                <div style="margin: 20px 0; font-size: 2rem">Text component</div>\
                <img src="https://picsum.photos/seed/image1/300/300"/>\
              <div>\
            `\
          }\
        ]
      }
    },
    plugins: [\
      editor => {\
        // Update context menu for the Heading components\
        editor.Components.addType('heading', {\
          model: {\
            defaults: {\
              contextMenu: ({ items, component }) => [\
                ...items, // default component items\
                {\
                  id: 'headingHTML',\
                  label: 'Heading HTML',\
                  icon: '<svg viewBox="0 0 24 24"><path d="m14.6 16.6 4.6-4.6-4.6-4.6L16 6l6 6-6 6-1.4-1.4m-5.2 0L4.8 12l4.6-4.6L8 6l-6 6 6 6 1.4-1.4Z"/></svg>',\
                  onClick: () => alert('Heading HTML: ' + component.toHTML())\
                }\
              ]\
            }\
          }\
        });\
        // Update context menu for the Image components\
        editor.Components.addType('image', {\
          model: {\
            defaults: {\
              contextMenu: ({ component }) => [\
                // skipping default items\
                {\
                  id: 'imageItems',\
                  label: 'Image items',\
                  icon: 'image',\
                  items: [\
                    { id: 'i1', label: 'Item 1', icon: 'check' },\
                    { id: 'i2', label: 'Item 2', onClick: () => alert('Item 2') },\
                    {\
                      id: 'i3',\
                      label: 'Item 3',\
                      items: [\
                        {\
                          id: 'i3-1',\
                          label: 'Replace image',\
                          icon: 'refresh',\
                          onClick: () =>\
                            component.set({ src: `https://picsum.photos/seed/${Math.random()}/300/300` })\
                        }\
                      ]\
                    }\
                  ]\
                }\
              ]\
            }\
          }\
        });\
      }\
    ],
    components: {
      contextMenu: ({ items, component, type, source }) => {
        return [\
          {\
            id: 'globalBefore',\
            label: 'Global: Disabled',\
            disabled: true\
          },\
          // default items that you can 'map' and filter'\
          ...items,\
          {\
            id: 'globalAfter',\
            label: 'Global: Info',\
            onClick: () => alert(`Type: ${type} \nSource: ${source} \nHTML: ${component.toHTML()}`)\
          }\
        ];
      }
    },
  }}
/>

```

The `components.contextMenu` function (global) and the `contextMenu` property (component-specific) both receive the same arguments:

- `items`: An array of default items that you can modify or filter (e.g., via `items.map` or `items.filter`).
- `component`: The component instance the menu is being called for.
- `type`: The type of the component.
- `source`: Indicates where the context menu was called from ( `canvas` or `layers`).

The `contextMenu` function should return an array of context menu items. The default items array is first passed to any component-specific configuration, then to the global one. If the final result is an empty array, the context menu won't be displayed.

## Properties from HTML [​](https://app.grapesjs.com/docs-sdk/configuration/components/properties#properties-from-html "Direct link to Properties from HTML")

GrapesJS allows you to define component properties directly through HTML using the `data-gjs-PROPERTY_NAME="VALUE"` attributes. This enables you to pass properties when importing components via HTML strings.

The editor also automatically converts certain string values into appropriate JavaScript data types:

- **Booleans**: Strings like `"true"` and `"false"` will be automatically converted to their respective boolean `true`/ `false` values.
- **Arrays**: Strings formatted like `"[1,2,3]"` will be parsed to arrays `[1, 2, 3]`.
- **Objects**: Strings that resemble object notation, like `{"key": "value"}`, will be transformed into JavaScript objects `{key: "value"}`.

In the demo below, you'll see how properties are passed through HTML attributes and how they are processed by the editor.

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
    plugins: [\
      editor => {\
        editor.onReady(() => {\
          editor.getWrapper().append(`\
            <div\
              style="color: red; padding: 20px"\
              data-gjs-value="Simple value"\
              data-gjs-array='["value1",2]'\
              data-gjs-object='{"key1": "value", "key2": 2}'\
            >Component imported from HTML</div>\
          `);\
        });\
      }\
    ],
    project: {
      // Empty project for our demo purpose
      default: { pages: [{ name: 'Home' }] }
    },
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: {
          type: 'canvasSidebarTop',
          sidebarTop: {
            rightContainer: {
              buttons: [\
                {\
                  type: 'button',\
                  variant: 'outline',\
                  label: 'Show component JSON',\
                  onClick: ({ editor }) => alert(JSON.stringify(editor.getWrapper(), null, 2))\
                }\
              ],
            },
          }
        },
      }
    }
  }}
/>

```

- [Structural Properties](https://app.grapesjs.com/docs-sdk/configuration/components/properties#structural-properties)
- [Behavior Properties](https://app.grapesjs.com/docs-sdk/configuration/components/properties#behavior-properties)
- [Visual Properties](https://app.grapesjs.com/docs-sdk/configuration/components/properties#visual-properties)
- [Styling Properties](https://app.grapesjs.com/docs-sdk/configuration/components/properties#styling-properties)
- [Script Properties](https://app.grapesjs.com/docs-sdk/configuration/components/properties#script-properties)
- [Traits](https://app.grapesjs.com/docs-sdk/configuration/components/properties#traits)
- [Toolbar](https://app.grapesjs.com/docs-sdk/configuration/components/properties#toolbar)
- [Context Menu](https://app.grapesjs.com/docs-sdk/configuration/components/properties#context-menu)
- [Properties from HTML](https://app.grapesjs.com/docs-sdk/configuration/components/properties#properties-from-html)

---

# File: app.grapesjs.com_docs-sdk_configuration_datasources_overview.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/datasources/overview"
title: "Data Sources | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#__docusaurus_skipToContent_fallback)

On this page

Data Sources in Studio SDK unlock dynamic content creation by letting users bind components to structured data, no custom code required. This feature enables variable rendering, conditional logic, and repeatable collections directly within the visual editor. From personalized emails to adaptive web pages and smart document layouts, users can build rich, data-driven templates with ease. A built-in UI makes connecting data intuitive and accessible for everyone.

![Data Sources](https://app.grapesjs.com/docs-sdk/assets/images/data-sources-9868deda649304e1241431fd8c5052a0.webp)

## Table of Contents

- [When do you need Data Sources?](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#when-do-you-need-data-sources)
- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#initialization)
- [Data Components](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#data-components)
- [Programmatic Usage](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#programmatic-usage)
- [I18n](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#i18n)
- [Template Engines](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#template-engines)
- [Commands](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#commands)

## When do you need Data Sources? [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#when-do-you-need-data-sources "Direct link to When do you need Data Sources?")

Use Data Sources when you want your content to adapt based on dynamic data. Whether you're building a personalized email, a product listing, or a user-specific dashboard, Data Sources make it easy to connect your content to real data. Here's what you can do:

- **Bind data to components:** Display values like a user's name, product titles, or pricing directly inside the content.
- **Centralize your data:** Keep your data in one place and reuse it across pages, templates, and components.
- **Show or hide content with conditions:** Render content only when specific conditions are met.
- **Flexible export options:** Choose how your content gets exported: either with data fully resolved (ready-to-publish) or as templates using placeholders/merge tags (for use with external template engines).

## Initialization [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#initialization "Direct link to Initialization")

Data Source components are built into the Studio SDK, and you can use them in your projects without any additional setup. To make the most of this feature, you'll typically want to expose the built-in Data Source blocks and provide some initial data for users to work with.

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
    dataSources: {
      blocks: true, // This enables the Data Source specific blocks
      globalData: { // Provide default globalData for the project
        user: { firstName: 'Alice', isCustomer: true },
        products: [\
          { name: 'Laptop Pro X15', price: 1200.0 },\
          { name: 'Wireless Mouse M2', price: 25.99 }\
        ]
      },
    },
    project: {
      default: {
        pages: [\
          {\
            name: 'Data Sources demo',\
            component: `\
              <h1>Variable</h1>\
              <data-variable data-gjs-data-resolver='{"path": "globalData.user.data.firstName", "defaultValue": "Guest"}'></data-variable>\
\
              <h1>Condition</h1>\
              <div> Hey,\
                <data-condition data-gjs-data-resolver='{"condition": {"logicalOperator": "and", "statements": [{"left": { "type":"data-variable", "path":"globalData.user.data.isCustomer" }, "operator": "equals", "right": true}] }}'>\
                  <data-condition-true-content>welcome back, valued customer!</data-condition-true-content>\
                  <data-condition-false-content>please register to see more!</data-condition-false-content>\
                </data-condition>\
              </div>\
\
              <h1>Collection</h1>\
              <ul data-gjs-type="data-collection"\
                data-gjs-data-resolver='{"collectionId": "myCollectionId", "dataSource": {"type":"data-variable", "path":"globalData.products.data" } }'\
              >\
                <li data-gjs-type="data-collection-item">\
                  <b>Product Name</b>: <data-variable data-gjs-data-resolver='{"collectionId": "myCollectionId", "variableType": "currentItem", "path": "name" }'></data-variable>\
                  - <b>Price</b>: <data-variable data-gjs-data-resolver='{"collectionId": "myCollectionId", "variableType": "currentItem", "path": "price" }'></data-variable>\
                </li>\
              </ul>\
            `\
          },\
        ]

    },
    },
    // Custom editor layout for demo purpose
    layout: {
      default: {
        type: 'row',
        style: { height: '100%' },
        children: [\
          {\
            type: 'panelBlocks',\
            header: { label: 'Blocks', collapsible: false, style: { width: '300px' } },\
            symbols: false,\
          },\
          { type: 'canvas' }\
        ]
      }
    },
  }}
/>

```

![Data Sources Initialization](https://app.grapesjs.com/docs-sdk/assets/images/data-sources-init-16ce042cae7482ea20620788571a07ca.webp)

## Data Components [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#data-components "Direct link to Data Components")

warning

For a better understanding of this section, we recommend checking out the [Components](https://app.grapesjs.com/docs-sdk/configuration/components/overview) section first.

Studio SDK includes a set of built-in data components, each supporting its own `dataResolver` options. These components are the foundation for building data-driven UIs within the editor. They can also be easily imported from existing HTML, making it simple to turn static designs into dynamic templates.

- `data-variable`: Renders a single data point.

```codeBlockLines_AdAo
{
  type: "data-variable",
  dataResolver: {
    path: "DATA_SOURCE_ID.RECORD_ID.PROPERTY_NAME", // Path to the data
    defaultValue: "Default value" // Fallback value
  }
}

```

The `path` points to the data field. If the path is invalid or the value is undefined, `defaultValue` is used.

- `data-condition`: For conditional rendering.

```codeBlockLines_AdAo
{
  type: "data-condition",
  dataResolver: {
    condition: {
     logicalOperator: "and",
      statements: [{\
        left: { type: "data-variable", path: "VARIABLE_PATH" },\
        operator: "equals",\
        right: true\
      }]
    }
  },
  components: [{\
      type: "data-condition-true-content",\
      components: "welcome back, valued customer!",\
    },{\
      type: "data-condition-false-content",\
      components: "please register to see more!"\
  }],
}

```

The `logicalOperator` combines multiple `statements`. Each statement evaluates a condition using `left` (data path or direct value), `operator`, and `right` (data path or direct value).

- `data-collection`: For rendering arrays of data

```codeBlockLines_AdAo
{
  type: 'data-collection',
  dataResolver: {
    collectionId: 'myCollectionId',
    dataSource: { type: 'data-variable', path: 'VARIABLE_PATH' }
  },
  components: {
    type: 'data-collection-item',
    components: [\
      {\
        type: 'data-variable',\
        dataResolver: {\
          collectionId: 'myCollectionId',\
          variableType: 'currentItem',\
          path: 'name',\
        }\
      },\
    ]
  }
}

```

The `dataSource.path` must point to an array. The `data-collection-item` component (defined separately or inline) serves as the template for each item, within which you can access the current item's properties.

## Programmatic Usage [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#programmatic-usage "Direct link to Programmatic Usage")

In the initial example, we saw how `dataSources.globalData` can be used to provide default data to the editor. This is a great way to kickstart your project, but you can also manage data sources programmatically via [GrapesJS DataSource APIs](https://grapesjs.com/docs/api/datasources.html), for a more advanced use cases.

Data Sources are stored in the project JSON. If you try adding a new data source while the editor is still loading the project, it will be overwritten by the JSON data. Make sure the editor is fully initialized before managing data sources programmatically.

```codeBlockLines_AdAo
// Example of a plugin
editor => {
  editor.onReady(() => {
    // The editor has finished loading the project data
    editor.DataSources.add({
      /* your data source configuration */
    });
  });
};

```

After initializing the editor, you can use the following API to create, update, or remove data sources:

```codeBlockLines_AdAo
// Access the DataSources module
const dsm = editor.DataSources;

// Add a new data source.
const ds = dsm.add({
  id: 'my_data_source_id',
  skipFromStorage: true, // Skip storing the data source in the JSON (optional).
  records: [\
    { id: 'id1', name: 'value1' },\
    { id: 'id2', name: 'value2' }\
  ]
});

// Get the data source by ID.
const ds = dsm.get('my_data_source_id');

// Get the data source records.
const dsRecords = ds.getRecords();

// Update records.
ds.setRecords([\
  { id: 'id1', name: 'value1 UP' },\
  { id: 'id2', name: 'value2 UP' }\
]);

// Update single record.
const dsRecord = ds.getRecord('id1');
dsRecord.set({ name: 'value1-2' });

// Get value by path.
const result = dsm.getValue('my_data_source_id.id1.name');

// Remove data source by ID.
dsm.remove('my_data_source_id');

```

## I18n [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#i18n "Direct link to I18n")

All user-facing labels and default text within the `dataSources` UI and components are fully internationalizable using the standard GrapesJS I18n module.
You can provide translations by defining the appropriate keys in your editor's I18n configuration, as listed below.

- React
- JS

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
          dataSources: {
            confirm: "Confirm",
            clearDataValue: "Clear data value",
            connectDataValue: "Connect data value",
            variable: "Variable",
            defaultValue: "Default value",
            variablePath: "Variable path",
            selectVariablePath: "Select variable path",
            openPathExplorer: "Open path explorer",
            closePathExplorer: "Close path explorer",
            toggleResolvedPath: "Toggle resolved path",
            items: "{length} item/s",
            properties: "{length} propertie/s",
            editVariable: "Edit variable",
            editCondition: "Edit condition",
            editCollection: "Edit collection",
            condition: "Condition",
            conditionTrue: "True condition",
            conditionFalse: "False condition",
            conditionAnd: "AND",
            conditionOr: "OR",
            conditionElse: "Else",
            addCondition: "Add condition",
            deleteCondition: "Delete condition",
            operator: "Operator",
            leftValue: "Left value",
            rightValue: "Right value",
            ifTrue: "Value if true",
            ifFalse: "Value if false",
            valueTrue: "True",
            valueFalse: "False",
            collection: "Collection",
            collectionItem: "Collection item",
            collectionId: "Collection ID",
            collectionStartIndex: "Start index",
            collectionEndIndex: "End index",
            collectionUpToEndIndex: "All",
            operators: {
              "=": "= (Equals)",
              "!=": "!= (Not equals)",
              ">": "> (Greater than)",
              ">=": ">= (Greater than or equals)",
              "<": "< (Less than)",
              "<=": "<= (Less than or equals)",
              contains: "Contains",
              startsWith: "Starts with",
              endsWith: "Ends with",
              matchesRegex: "Matches regex",
              equalsIgnoreCase: "Equals (ignore case)",
              trimEquals: "Trimmed equals",
              and: "AND",
              or: "OR",
              xor: "XOR",
              equals: "Equals",
              isDefined: "Is defined",
              isNull: "Is null",
              isUndefined: "Is undefined",
              isTruthy: "Is truthy",
              isFalsy: "Is falsy",
              isDefaultValue: "Is default value",
              isArray: "Is array",
              isObject: "Is object",
              isString: "Is string",
              isNumber: "Is number",
              isBoolean: "Is boolean"
            }
          },
        }
      }
    },

  }}
/>

```

## Template Engines [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#template-engines "Direct link to Template Engines")

Studio allows integration with custom template engines by providing an importer and exporter. This makes it possible to map your template engine's syntax (e.g. Handlebars, EJS) to Studio's data components and vice versa.

See [Template Engines page](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines) for details on how to set up the importer and exporter for your preferred syntax.

## Commands [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#commands "Direct link to Commands")

Studio SDK provides also commands to interact with `dataSources` programmatically.

### Toggle Data Sources Preview [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#toggle-data-sources-preview "Direct link to Toggle Data Sources Preview")

Toggles the editor's canvas view mode. This command switches between displaying resolved data values (preview mode) and showing placeholders or data binding indicators (edit mode). This helps visualize live data or identify data-bound elements for editing.

```codeBlockLines_AdAo
const newState = editor.runCommand(StudioCommands.toggleStateDataSource);
if (newState.showPlaceholder) {
  console.log('Now showing placeholders for data sources.');
} else {
  console.log('Now showing resolved data values.');
}

```

### Get Data Sources State [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#get-data-sources-state "Direct link to Get Data Sources State")

Retrieves the current data source state in the Studio editor (i.e., whether placeholders or resolved values are being displayed).

```codeBlockLines_AdAo
const currentState = editor.runCommand(StudioCommands.getStateDataSource);
console.log('Current state', currentState);

```

### Example [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#example "Direct link to Example")

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
          { type: 'sidebarLeft' },\
          {\
            type: 'canvasSidebarTop',\
            sidebarTop: {\
              leftContainer: {\
                buttons: ({ items }) => [\
                  ...items,\
                  {\
                    id: 'toggle-datasources-preview',\
                    title: 'Toggle Data Sources',\
                    icon: 'databaseOutlineOn',\
                    onClick: ({ editor }) => {\
                      editor.runCommand('studio:toggleStateDataSource');\
                    },\
                    editorEvents: {\
                      ['studio:toggleDataSourcesPreview']: ({ fromEvent, setState }) => {\
                        setState({ active: fromEvent.showPlaceholder });\
                      }\
                    }\
                  }\
                ]\
              }\
            }\
          },\
          { type: 'sidebarRight' }\
        ]
      }
    },
    dataSources: {
      blocks: true, // This enables the Data Source specific blocks
      globalData: { // Provide default globalData for the project
        user: { firstName: 'Alice', isCustomer: true },
        products: [\
          { name: 'Laptop Pro X15', price: 1200.0 },\
          { name: 'Wireless Mouse M2', price: 25.99 }\
        ]
      },
    },
    project: {
      default: {
        pages: [\
          {\
            name: 'Data Sources demo',\
            component: `\
              <h1>Variable</h1>\
              <data-variable data-gjs-data-resolver='{"path": "globalData.user.data.firstName", "defaultValue": "Guest"}'></data-variable>\
\
              <h1>Condition</h1>\
              <div> Hey,\
                <data-condition data-gjs-data-resolver='{"condition": {"logicalOperator": "and", "statements": [{"left": { "type":"data-variable", "path":"globalData.user.data.isCustomer" }, "operator": "equals", "right": true}] }}'>\
                  <data-condition-true-content>welcome back, valued customer!</data-condition-true-content>\
                  <data-condition-false-content>please register to see more!</data-condition-false-content>\
                </data-condition>\
              </div>\
\
              <h1>Collection</h1>\
              <ul data-gjs-type="data-collection"\
                data-gjs-data-resolver='{"collectionId": "myCollectionId", "dataSource": {"type":"data-variable", "path":"globalData.products.data" } }'\
              >\
                <li data-gjs-type="data-collection-item">\
                  <b>Product Name</b>: <data-variable data-gjs-data-resolver='{"collectionId": "myCollectionId", "variableType": "currentItem", "path": "name" }'></data-variable>\
                  - <b>Price</b>: <data-variable data-gjs-data-resolver='{"collectionId": "myCollectionId", "variableType": "currentItem", "path": "price" }'></data-variable>\
                </li>\
              </ul>\
            `\
          },\
        ]

    },
    },
  }}
/>

```

- [When do you need Data Sources?](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#when-do-you-need-data-sources)
- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#initialization)
- [Data Components](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#data-components)
- [Programmatic Usage](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#programmatic-usage)
- [I18n](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#i18n)
- [Template Engines](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#template-engines)
- [Commands](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#commands)
  - [Toggle Data Sources Preview](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#toggle-data-sources-preview)
  - [Get Data Sources State](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#get-data-sources-state)
  - [Example](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview#example)

---

# File: app.grapesjs.com_docs-sdk_configuration_datasources_template-engines.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines"
title: "Template Engines | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#__docusaurus_skipToContent_fallback)

On this page

Studio SKD supports custom template engine integration for managing dynamic content within your projects. This means you can use your preferred templating syntax for data binding, conditionals, and loops, while Studio takes care of the visual editing and component structure.

## Table of Contents

- [How It Works](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#how-it-works)
- [Importer](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#importer)
- [Exporter](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#exporter)

## How It Works [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#how-it-works "Direct link to How It Works")

To integrate a template engine, you need to implement two components:

- **Importer** \- Parses the input string (e.g., HTML with embedded template syntax) and converts recognized expressions into Studio's data-aware components—such as Variables, Conditions, and Collections.
- **Exporter** \- Defines how data components are translated back into your template engine's syntax for output (e.g., when generating code or exporting files).

Studio comes with built-in support for common engines like [Handlebars](https://app.grapesjs.com/docs-sdk/plugins/data-sources/handlebars) and [EJS](https://app.grapesjs.com/docs-sdk/plugins/data-sources/ejs), but you can register your own to support virtually any templating system.

## Importer [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#importer "Direct link to Importer")

The Importer is responsible for parsing an input string (such as HTML with embedded template syntax) and transforming it into a format Studio can understand. Specifically, it converts template expressions into Data Components.

You register an importer using the `studio:dataSourceSetImporter` command:

```codeBlockLines_AdAo
editor.runCommand('studio:dataSourceSetImporter', {
  /**
   * Parses the input string and replaces template syntax with data components in HTML.
   * @param input The raw HTML/template string.
   * @returns A string with template syntax replaced by data component structures.
   */
  import(input: string) {
    return fromTemplateEngineToGrapesJS(input);
  }
});

```

### Native Parsers [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#native-parsers "Direct link to Native Parsers")

For accurate and reliable results, it is **strongly recommended** to use the official parser or compiler for your chosen template engine.

Template engines often have complex syntax, including nesting, escaping, comments, and custom directives, which are best handled by their own parsing tools.

Here's a list of popular template engines along with their dedicated JavaScript parsers/compilers:

| Template Engine    | NPM Package                                              |
| ------------------ | -------------------------------------------------------- |
| **Handlebars**     | [`handlebars`](https://www.npmjs.com/package/handlebars) |
| **EJS**            | [`ejs`](https://www.npmjs.com/package/ejs)               |
| **Liquid**         | [`liquidjs`](https://www.npmjs.com/package/liquidjs)     |
| **Nunjucks**       | [`nunjucks`](https://www.npmjs.com/package/nunjucks)     |
| **Mustache**       | [`mustache`](https://www.npmjs.com/package/mustache)     |
| **Pug**            | [`pug`](https://www.npmjs.com/package/pug)               |
| **Eta**            | [`eta`](https://www.npmjs.com/package/eta)               |
| **Twig (JS port)** | [`twig`](https://www.npmjs.com/package/twig)             |
| **Dot.js**         | [`dot`](https://www.npmjs.com/package/dot)               |

Most of these libraries expose an AST or compile method. When writing an Importer, you can:

- Use the parser to tokenize the template.
- Walk through the AST or output.
- Replace expressions ( `{{ variable }}`, `{% if %}`) with Studio data components.

### Importer Demo [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#importer-demo "Direct link to Importer Demo")

While using a proper parser is the preferred method, the following example demonstrates a basic importer using simple, hardcoded string replacements. This example is only for illustrative purposes.

The goal is to replace known snippets of template syntax with the full HTML GrapesJS needs to recognize them as Data Components.

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
    dataSources: {
      globalData: {
        user: { firstName: 'Alice', isCustomer: true },
        products: [\
          { name: 'Laptop Pro X15', price: 1200.0 },\
          { name: 'Wireless Mouse M2', price: 25.99 }\
        ]
      },
    },
    project: {
      default: {
        pages: [\
          {\
            component: `\
              <h1>Variable</h1>\
              <div>Hello {{ globalData.user.data.firstName }}</div>\
\
              <h1>Condition</h1>\
              <div>{{#if globalData.user.data.isCustomer }}<p>You are a customer</p>{{/if}}</div>\
\
              <h1>Collection</h1>\
              {{#each globalData.products.data }}<div><h4>{{this.name}}</h4> <p>Price: {{this.price}}</p></div>{{/each}}\
            `\
          },\
        ]

    },
    },
    plugins: [\
      editor => {\
        editor.runCommand('studio:dataSourceSetImporter', {\
          import: (input: string) => {\
            let output = input;\
\
            // replacing a simple data bind with DataVariable component\
            output = output.replace(\
              '{{ globalData.user.data.firstName }}',\
              `<data-variable data-gjs-data-resolver='{"path":"globalData.user.data.firstName"}'></data-variable>`\
            );\
\
            // replacing a simple condition with DataCondition component\
            const conditionBlockContent = '<p>You are a customer</p>';\
            output = output.replace(\
              `{{#if globalData.user.data.isCustomer }}${conditionBlockContent}{{/if}}`,\
              `<data-condition\
                data-gjs-data-resolver='{"condition": { "logicalOperator": "and", "statements": [ { "left": { "type": "data-variable", "path": "globalData.user.data.isCustomer" }, "operator": "isTruthy" } ] }}'>\
                  ${conditionBlockContent}\
              </data-condition>`\
            );\
\
            // replacing a simple collection with DataCollection component\
            const collectionItemTemplate = `<div><h4>{{this.name}}</h4> <p>Price: {{this.price}}</p></div>`;\
            output = output.replace(\
              `{{#each globalData.products.data }}${collectionItemTemplate}{{/each}}`,\
              `<div\
                data-gjs-type="data-collection"\
                data-gjs-data-resolver='{ "collectionId": "products", "dataSource":{ "type":"data-variable", "path":"globalData.products.data" }}'>\
                  <div data-gjs-type="data-collection-item">\
                    <h4><data-variable data-gjs-data-resolver='{"collectionId": "products", "variableType": "currentItem", "path": "name" }'></data-variable></h4>\
                    <p>Price: <data-variable data-gjs-data-resolver='{"collectionId": "products", "variableType": "currentItem", "path": "price" }'></data-variable></p>\
                  </div>\
              </div>`\
            );\
\
            return output;\
          }\
        });\
      },\
\
    ]
  }}
/>

```

### Import in Email Projects [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#import-in-email-projects "Direct link to Import in Email Projects")

The example below shows how to use the importer in an email project. It includes MJML syntax and Handlebars expressions, which are converted into Studio's data components.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { dataSourceHandlebars } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{// ...
    plugins: [dataSourceHandlebars],
    dataSources: {
    globalData: {
      currentUser: { firstName: 'John', isCustomer: true },
      products: Array.from({ length: 4 }, (_, i) => ({
        id: `pid_${i + 1}`,
        name: `Product ${i + 1}`,
        price: Math.floor(Math.random() * 900) + 100
      }))
    },
    },
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
          <mj-text>Hello {{ globalData.currentUser.data.firstName }},</mj-text>\
          {{#if globalData.currentUser.data.isCustomer }}\
            <mj-text>Thanks for being a loyal customer! We have exclusive deals for you:</mj-text>\
          {{else}}\
            <mj-text>Sign up today to receive special offers and discounts!</mj-text>\
          {{/if}}\
          {{#each globalData.products.data }}\
            <mj-text><b>{{ this.name }}</b></mj-text>\
            <mj-text>Price: \${{ this.price }}</mj-text>\
          {{/each}}\
          <mj-text>Thank you for choosing us!</mj-text>\
        </mj-column>\
      </mj-section>\
    </mj-body>\
    </mjml>`\
        }\
      ]
    }
    }
  }}
/>

```

## Exporter [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#exporter "Direct link to Exporter")

The exporter tells Studio how to represent its Data Components using your template engine's syntax. You register an exporter using the `studio:dataSourceSetExporter` command.

The command expects an object where each function is responsible for generating the syntax for a specific part of a Data Component.

### Exporter Demo [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#exporter-demo "Direct link to Exporter Demo")

Here's a full example of how a simple set of Data Components might be exported using the syntax of a templating engine like Handlebars:

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
    dataSources: {
      globalData: {
        user: { firstName: 'Alice', isCustomer: true },
        products: [\
          { name: 'Laptop Pro X15', price: 1200.0 },\
          { name: 'Wireless Mouse M2', price: 25.99 }\
        ]
      },
    },
    project: {
      default: {
        pages: [\
          {\
            component: `\
              <h1>Variable</h1>\
              <data-variable data-gjs-data-resolver='{"path": "globalData.user.data.firstName" }'></data-variable>\
\
              <h1>Condition</h1>\
              <div> Hey,\
                <data-condition data-gjs-data-resolver='{"condition": {"logicalOperator": "and", "statements": [{"left": { "type":"data-variable", "path":"globalData.user.data.isCustomer" }, "operator": "equals", "right": true}] }}'>\
                  <data-condition-true-content>welcome back, valued customer!</data-condition-true-content>\
                  <data-condition-false-content>please register to see more!</data-condition-false-content>\
                </data-condition>\
              </div>\
\
              <h1>Collection</h1>\
              <ul data-gjs-type="data-collection"\
                data-gjs-data-resolver='{"collectionId": "myCollectionId", "dataSource": {"type":"data-variable", "path":"globalData.products.data" } }'\
              >\
                <li data-gjs-type="data-collection-item">\
                  <b>Product Name</b>: <data-variable data-gjs-data-resolver='{"collectionId": "myCollectionId", "variableType": "currentItem", "path": "name" }'></data-variable>\
                  - <b>Price</b>: <data-variable data-gjs-data-resolver='{"collectionId": "myCollectionId", "variableType": "currentItem", "path": "price" }'></data-variable>\
                </li>\
              </ul>\
            `\
          },\
        ]

    },
    },
    plugins: [\
      editor => {\
        editor.runCommand('studio:dataSourceSetExporter', {\
          // Generates the syntax for a DataVariable.\
          getVariableSyntax(props) {\
            const cmpDataVariable = props.component;\
            const dataResolver = cmpDataVariable.getDataResolver();\
            const path = dataResolver?.path;\
            return path ? `{{ ${path} }}` : '';\
          },\
\
          // Generates the starting syntax for a DataCondition.\
          getConditionalStartSyntax(props) {\
            const cmpDataCondition = props.component;\
            const dataResolver = cmpDataCondition.getDataResolver();\
            const condition = dataResolver?.condition;\
            return condition ? `{{#if ${condition.statements?.[0]?.left?.path} }}` : '';\
          },\
\
          // Generates the 'else' part of a DataCondition's syntax.\
          getConditionElseSyntax() {\
            return '{{else}}';\
          },\
\
          // Generates the ending syntax for a Data Condition.\
          getConditionalEndSyntax() {\
            return '{{/if}}';\
          },\
\
          // Generates the ending syntax for a Data Condition.\
          getCollectionStartSyntax(props) {\
            const cmpDataCollection = props.component;\
            const dataResolver = cmpDataCollection.getDataResolver();\
            const path = dataResolver?.dataSource?.path;\
            return path ? `{{#each ${path} }}` : '';\
          },\
\
          // Generates the ending syntax for a Data Condition.\
          getCollectionEndSyntax() {\
            return '{{/each}}';\
          }\
        }\
         // satisfies IDataSourceExporter // For TS\
        );\
      },\
\
    ]
  }}
/>

```

### Export in Email Projects [​](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#export-in-email-projects "Direct link to Export in Email Projects")

The example below demonstrates how to export an email project using MJML and Handlebars syntax. It includes Data Variables, Conditions, and Collections, which are converted back into the appropriate Handlebars syntax when you export the project.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { dataSourceHandlebars } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{// ...
    plugins: [dataSourceHandlebars],
    dataSources: {
    blocks: true,
    globalData: {
      currentUser: { firstName: 'John', isCustomer: true },
      products: Array.from({ length: 4 }, (_, i) => ({
        id: `pid_${i + 1}`,
        name: `Product ${i + 1}`,
        price: Math.floor(Math.random() * 900) + 100
      }))
    },
    },
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
          <mj-text>Hello <data-variable data-gjs-data-resolver='{"path": "globalData.currentUser.data.firstName"}'></data-variable>, </mj-text>\
        </mj-column>\
      </mj-section>\
\
      <mj-section>\
        <data-condition data-gjs-data-resolver='{\
            "condition": {\
              "logicalOperator": "and",\
              "statements": [{ "left": { "type": "data-variable", "path": "globalData.currentUser.data.isCustomer" }, "operator": "isTruthy" }]\
            }\
          }'>\
          <data-condition-true-content>\
            <mj-column>\
              <mj-text>Thanks for being a loyal customer! We have exclusive deals for you:</mj-text>\
            </mj-column>\
          </data-condition-true-content>\
          <data-condition-false-content>\
            <mj-column>\
              <mj-text>Sign up today to receive special offers and discounts!</mj-text>\
            </mj-column>\
          </data-condition-false-content>\
        </data-condition>\
      </mj-section>\
\
      <mj-section>\
        <data-collection data-gjs-data-resolver='{\
          "collectionId": "promoProducts",\
          "dataSource": { "type": "data-variable", "path": "globalData.products.data" }\
        }'>\
          <data-collection-item>\
            <mj-column>\
              <mj-text>\
                <b><data-variable data-gjs-data-resolver='{"collectionId": "promoProducts", "variableType": "currentItem", "path": "name"}'></data-variable></b>\
              </mj-text>\
              <mj-text>\
                Price: $<data-variable data-gjs-data-resolver='{"collectionId": "promoProducts", "variableType": "currentItem", "path": "price"}'></data-variable>\
              </mj-text>\
            </mj-column>\
          </data-collection-item>\
        </data-collection>\
      </mj-section>\
\
      <mj-section>\
        <mj-column>\
          <mj-text>Thank you for choosing us!</mj-text>\
        </mj-column>\
      </mj-section>\
\
    </mj-body>\
    </mjml>`\
        }\
      ],
    },
    }

  }}
/>

```

- [How It Works](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#how-it-works)
- [Importer](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#importer)
  - [Native Parsers](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#native-parsers)
  - [Importer Demo](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#importer-demo)
  - [Import in Email Projects](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#import-in-email-projects)
- [Exporter](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#exporter)
  - [Exporter Demo](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#exporter-demo)
  - [Export in Email Projects](https://app.grapesjs.com/docs-sdk/configuration/datasources/template-engines#export-in-email-projects)

---

# File: app.grapesjs.com_docs-sdk_configuration_fonts.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/fonts"
title: "Fonts | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/fonts#__docusaurus_skipToContent_fallback)

On this page

![Font Select](Base64-Image-Removed)

By default, the editor shows a limited set of common system fonts (like Arial, Times New Roman, etc.). To let users access a broader range of fonts, such as web fonts or brand-specific typefaces, you can enable the Font Manager.

The Font Manager requires a font provider to supply available fonts. In this guide, we'll show you how to configure a custom provider, or use the built-in plugins for a quick and easy setup.

## Table of Contents

- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/fonts#initialization)
- [Project Fonts](https://app.grapesjs.com/docs-sdk/configuration/fonts#project-fonts)
- [Default Fonts](https://app.grapesjs.com/docs-sdk/configuration/fonts#default-fonts)
- [Font Providers](https://app.grapesjs.com/docs-sdk/configuration/fonts#font-providers)
- [Variable Fonts](https://app.grapesjs.com/docs-sdk/configuration/fonts#variable-fonts)
- [I18n](https://app.grapesjs.com/docs-sdk/configuration/fonts#i18n)
- [Commands](https://app.grapesjs.com/docs-sdk/configuration/fonts#commands)

## Initialization [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#initialization "Direct link to Initialization")

Below is an example of how to initialize the Font Manager using the [Google Fonts Provider](https://app.grapesjs.com/docs-sdk/plugins/asset-providers/google-fonts) plugin. This setup allows users to browse and apply a wide range of fonts directly within the editor.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { googleFontsAssetProvider } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
    fonts: {
      enableFontManager: true,
    },
    project: {
      default: {
        pages: [{\
          name: 'Home',\
          component: '<p>Open the Typography section on the right panel. Use the "+" button in the "Font" property to add new fonts.</p>'\
        }]
      }
    },
    plugins: [\
      googleFontsAssetProvider.init({ apiKey: 'GOOGLE_FONTS_API_KEY' }),\
      editor => {\
        editor.onReady(() => {\
          const textCmp = editor.getWrapper().find('p')[0];\
          editor.select(textCmp);\
        });\
      }\
    ],
  }}
/>

```

## Project Fonts [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#project-fonts "Direct link to Project Fonts")

![Project Fonts](Base64-Image-Removed)

Custom fonts are stored in the project JSON under `custom.globalPageSettings.fonts`. When the Font Manager is enabled, these fonts will appear in the [Global Page Settings](https://app.grapesjs.com/docs-sdk/configuration/pages#settings) panel within the editor.

You can also preconfigure your project (e.g. in [Templates](https://app.grapesjs.com/docs-sdk/configuration/templates)) by including default custom fonts in the initial JSON.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    fonts: {
      enableFontManager: true,
      // You can also disable showing project fonts in the editor.
      // showProjectFonts: false,
    },
    project: {
      default: {
        pages: [\
          {\
            name: 'Home',\
            component: `\
              <p>Manage your project's fonts:</p>\
              <ol>\
                <li>Click the gear icon next to the page name "Home" on the top left panel.</li>\
                <li>Switch from 'Page: "Home"' to 'Page: "Global Settings"' at the top left and scroll to the bottom.</li>\
              </ol>\
              <p style="font-family: Aboreto">This text uses the Aboreto font</p>`\
          }\
        ],
        custom: {
          globalPageSettings: {
            fonts: {
              Aboreto: {
                variants: {
                  regular: {
                    source: 'https://fonts.gstatic.com/s/aboreto/v2/5DCXAKLhwDDQ4N8blKHeA2yuxSY.woff2'
                  }
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

## Default Fonts [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#default-fonts "Direct link to Default Fonts")

Use the `fonts.default` option to customize the default fonts shown in the font family fields. You can provide a static array or a function to dynamically extend or modify the list.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    fonts: {
      enableFontManager: true,
      default: ({ baseDefault }) => [\
        // Add font objects with font files. This will be added to the project JSON automatically.\
        {\
          family: 'Aboreto',\
          variants: {\
            regular: { source: 'https://fonts.gstatic.com/s/aboreto/v2/5DCXAKLhwDDQ4N8blKHeA2yuxSY.woff2' }\
          }\
        },\
        // Add fonts without font files that rely on system fonts with fallbacks.\
        {\
          id: '"Times New Roman", serif',\
          label: 'Times New Roman'\
        },\
        // Use parts of the base default fonts.\
        baseDefault[0]\
      ]
    },
    project: {
      default: {
        pages: [\
          {\
            name: 'Home',\
            component: '<p style="font-family: Aboreto;">Open the Typography section on the right panel. Use the "+" button in the "Font" property to add new fonts.</p>'\
          }\
        ]
      }
    },
    plugins: [\
      editor => {\
        editor.onReady(() => {\
          const textCmp = editor.getWrapper().find('p')[0];\
          editor.select(textCmp);\
        });\
      }\
    ]

  }}
/>

```

## Font Providers [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#font-providers "Direct link to Font Providers")

![Font Providers](https://app.grapesjs.com/docs-sdk/assets/images/font-asset-manager-f88ba87b42ef88e6de775f6843988a6b.webp)

Font providers are responsible for supplying custom fonts to the editor. You can use the [Asset Providers](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers) interface to add additional fonts, either through a built-in plugin like Google Fonts or by defining your own provider to load fonts from any external source or internal service.

Below is an example of how to create a custom font provider.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { StudioCommands } from '@grapesjs/studio-sdk';
// ...
<StudioEditor
  options={{
    fonts: {
      enableFontManager: true,
    },
    project: {
      default: {
        pages: [\
          {\
            name: 'Home',\
            component: '<p>Open the Typography section on the right panel. Use the "+" button in the "Font" property to add new fonts.</p>'\
          }\
        ]
      }
    },
    assets: {
      providers: [\
        {\
          id: "custom-fonts",\
          label: "Custom Font Provider",\
          types: "font",\
          onLoad: async (props) => {\
            return [\
              {\
                id: 'aboreto',\
                type: 'font',\
                src: 'https://fonts.gstatic.com/s/aboreto/v2/5DCXAKLhwDDQ4N8blKHeA2yuxSY.woff2',\
                name: 'Aboreto',\
                customData: {\
                  font: {\
                    family: 'Aboreto',\
                    variants: {\
                      regular: {\
                        source: 'https://fonts.gstatic.com/s/aboreto/v2/5DCXAKLhwDDQ4N8blKHeA2yuxSY.woff2',\
                      }\
                    }\
                  }\
                }\
              }\
            ];\
          },\
          itemLayout: ({ assetProps, editor, onSelect }) => {\
            const { font } = assetProps.customData;\
            const fontFamily = editor.runCommand(StudioCommands.menuFontLoad, { font });\
            return {\
              type: 'column',\
              children: [\
                {\
                  onClick: () => onSelect(assetProps),\
                  type: 'button',\
                  children: [assetProps.name],\
                  style: {\
                    fontFamily,\
                    fontSize: '24px',\
                    padding: '8px'\
                  }\
                }\
              ]\
            };\
          }\
        }\
      ]
    },
  }}
/>

```

Font objects define font variants. Each variant has an URL to a font file, and an optional [descriptors](https://developer.mozilla.org/en-US/docs/Web/API/FontFace/FontFace#descriptors) object.

#### Font properties [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#font-properties "Direct link to Font properties")

Show properties

| Property   | Type   | Description                                                                                                                                                                                                                                                       |
| ---------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| variants\* | object | The font variants that will be available in the editor when you add this font.<br>**Example** <br>`codeBlockLines_AdAo<br>variants: {<br>  regular: {<br>    source: 'https://fonts.gstatic.com/s/aboreto/v2/5DCXAKLhwDDQ4N8blKHeA2yuxSY.woff2',<br>  }<br>}<br>` |

#### Variant properties [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#variant-properties "Direct link to Variant properties")

Show properties

| Property    | Type   | Description                                                                                                                                                                                                                                                                                                                          |
| ----------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| source\*    | string | The url with the font file for this variant.                                                                                                                                                                                                                                                                                         |
| descriptors | object | Extra arguments for FontFace init.<br>https://developer.mozilla.org/en-US/docs/Web/API/FontFace/FontFace#descriptors<br>**Example**<br>`codeBlockLines_AdAo<br>// variable fonts<br>descriptors: {<br>  // here we describe the range of values supported by the weight axis of this variable font<br>  weight: "300 800",<br>}<br>` |

## Variable Fonts [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#variable-fonts "Direct link to Variable Fonts")

The Studio editor supports [variable fonts](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_fonts/Variable_fonts_guide).
Describe variable font axes supported by your font file in the [variant.descriptors](https://developer.mozilla.org/en-US/docs/Web/API/FontFace/FontFace#descriptors) object.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    fonts: {
      enableFontManager: true,
    },
    project: {
      default: {
        pages: [\
          {\
            name: 'Home',\
            component: `\
              <p style="font-family: '42dot Sans';">This text uses the variable font 42dot Sans. Try changing the weight in Styles > Typography > Weight.</p>`\
          }\
        ],
        custom: {
          globalPageSettings: {
            fonts: {
              "42dot Sans": {
                variants: {
                  regular: {
                    source:
                      "https://fonts.gstatic.com/s/42dotsans/v2/BXRuvFK-2v7FnQ1xTYUJ0WrfXcjWzZw.woff2",
                    descriptors: {
                      // here we describe the range of values supported by the weight axis of this variable font
                      weight: "300 800",
                    },
                  },
                },
              }
            }
          }
        }
      }
    }

  }}
/>

```

## I18n [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#i18n "Direct link to I18n")

These are the i18n keys you can use to customize labels in the Font Manager.

- React
- JS

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
          fontManager: {
            addFontToProject: "Add font to project",
            projectFonts: "Project fonts",
            emptyProjectFonts: "There are no fonts in this project.",
            selectFont: "Select a font"
          },
        }
      }
    },
  }}
/>

```

## Commands [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#commands "Direct link to Commands")

Here's a list of commands to manage fonts dynamically.

### Get Fonts [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#get-fonts "Direct link to Get Fonts")

Get a list of registered fonts.

```codeBlockLines_AdAo
import { StudioCommands } from '@grapesjs/studio-sdk';
const fonts = editor.runCommand(StudioCommands.fontGet);

```

### Add Font [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#add-font "Direct link to Add Font")

Register a new font. If a font with the same key already exists, it will be replaced.

```codeBlockLines_AdAo
import { StudioCommands } from '@grapesjs/studio-sdk';
const font = {
  family: 'Aboreto',
  variants: {
    regular: {
      source: 'https://fonts.gstatic.com/s/aboreto/v2/5DCXAKLhwDDQ4N8blKHeA2yuxSY.woff2'
    }
  }
};
editor.runCommand(StudioCommands.fontAdd, {
  font
});

```

### Remove Font [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#remove-font "Direct link to Remove Font")

Remove a registered font.

```codeBlockLines_AdAo
import { StudioCommands } from '@grapesjs/studio-sdk';
editor.runCommand(StudioCommands.fontRemove, { family: 'Aboreto' });

```

### Open Font Manager [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#open-font-manager "Direct link to Open Font Manager")

Open the Font Manager.

```codeBlockLines_AdAo
import { StudioCommands } from '@grapesjs/studio-sdk';
editor.runCommand(StudioCommands.fontManagerOpen, { modalTitle: 'Pick a font' });

```

### Load Menu Font [​](https://app.grapesjs.com/docs-sdk/configuration/fonts#load-menu-font "Direct link to Load Menu Font")

Loads a custom font for displaying in menu options with a live preview. Some font services offer lightweight font files specifically for preview purposes, which can be used here to improve performance.

```codeBlockLines_AdAo
import { StudioCommands } from '@grapesjs/studio-sdk';
const font = {
  family: 'Aboreto',
  variants: {
    regular: {
      source: 'https://fonts.gstatic.com/s/aboreto/v2/5DCXAKLhwDDQ4N8blKHeA2yuxSY.woff2'
    }
  }
};
const menuFontFamily = editor.runCommand(StudioCommands.menuFontLoad, { font });

```

- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/fonts#initialization)
- [Project Fonts](https://app.grapesjs.com/docs-sdk/configuration/fonts#project-fonts)
- [Default Fonts](https://app.grapesjs.com/docs-sdk/configuration/fonts#default-fonts)
- [Font Providers](https://app.grapesjs.com/docs-sdk/configuration/fonts#font-providers)
- [Variable Fonts](https://app.grapesjs.com/docs-sdk/configuration/fonts#variable-fonts)
- [I18n](https://app.grapesjs.com/docs-sdk/configuration/fonts#i18n)
- [Commands](https://app.grapesjs.com/docs-sdk/configuration/fonts#commands)
  - [Get Fonts](https://app.grapesjs.com/docs-sdk/configuration/fonts#get-fonts)
  - [Add Font](https://app.grapesjs.com/docs-sdk/configuration/fonts#add-font)
  - [Remove Font](https://app.grapesjs.com/docs-sdk/configuration/fonts#remove-font)
  - [Open Font Manager](https://app.grapesjs.com/docs-sdk/configuration/fonts#open-font-manager)
  - [Load Menu Font](https://app.grapesjs.com/docs-sdk/configuration/fonts#load-menu-font)

---

# File: app.grapesjs.com_docs-sdk_configuration_global-styles.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/global-styles"
title: "Global Styles | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/global-styles#__docusaurus_skipToContent_fallback)

On this page

Studio SDK offers a Global Styles panel, enabling you to define common styles that apply across all elements in your project. This panel simplifies the process of setting up and maintaining consistent styling, making it easy for end users to edit and manage global styles.

![Global Styles](https://app.grapesjs.com/docs-sdk/assets/images/global-styles-508f8a5bbae3a4210aa0244a586309ba.png)

## Table of Contents

- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/global-styles#initialization)
- [Properties](https://app.grapesjs.com/docs-sdk/configuration/global-styles#properties)
- [Updating Global Styles](https://app.grapesjs.com/docs-sdk/configuration/global-styles#updating-global-styles)
- [I18n](https://app.grapesjs.com/docs-sdk/configuration/global-styles#i18n)
- [Advanced Usage](https://app.grapesjs.com/docs-sdk/configuration/global-styles#advanced-usage)
- [Usage in Email Projects](https://app.grapesjs.com/docs-sdk/configuration/global-styles#usage-in-email-projects)

## Initialization [​](https://app.grapesjs.com/docs-sdk/configuration/global-styles#initialization "Direct link to Initialization")

To enable global styles, configure the `globalStyles.default` option. Each item in this array represents a style rule that applies across the project.

Here's a simple example of setting up global styles:

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
    plugins: [\
      editor =>\
        editor.onReady(() => {\
          // let's show the global style panel on start\
          editor.runCommand('studio:layoutToggle', {\
            id: 'gs',\
            layout: 'panelGlobalStyles',\
            header: { label: 'Global Styles' },\
            placer: { type: 'absolute', position: 'right' }\
          });\
        })\
    ],
    project: {
      default: {
        pages: [\
          { name: 'Home', component: '<h1>Home page</h1>'},\
          { name: 'About', component: '<h1>About page</h1>'},\
        ]
      }
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
        {\
          id: 'h1Size',\
          property: 'font-size',\
          field: { type: 'number', min: 0.1, max: 10, step: 0.1, units: ['rem'] },\
          defaultValue: '2rem',\
          selector: 'h1',\
          label: 'H1 size'\
        }\
      ]
    }
  }}
/>

```

In this example, we've defined two global styles: the color and font size for `h1` elements. Updating these styles in the panel will automatically apply the changes to all `h1` elements across your project. You can see the effect by navigating through a different page.

## Properties [​](https://app.grapesjs.com/docs-sdk/configuration/global-styles#properties "Direct link to Properties")

### Global Style properties [​](https://app.grapesjs.com/docs-sdk/configuration/global-styles#global-style-properties "Direct link to Global Style properties")

Below are the available properties for the global style object.

Show properties

| Property     | Type                                                                                                    | Description                                                                                                                              |
| ------------ | ------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| id\*         | string                                                                                                  | A unique identifier for the style rule.<br>**Example**<br>`codeBlockLines_AdAo<br>"myRuleId"<br>`                                        |
| property\*   | string                                                                                                  | The CSS property name (e.g., 'color', 'font-size', 'margin', etc.).<br>**Example**<br>`codeBlockLines_AdAo<br>"font-size"<br>`           |
| selector\*   | string                                                                                                  | The CSS selector to which this style will be applied.<br>**Example**<br>`codeBlockLines_AdAo<br>"h1"<br>`                                |
| label        | string                                                                                                  | The label for the style rule (used in the editor UI).<br>**Example**<br>`codeBlockLines_AdAo<br>"H1 size"<br>`                           |
| field        | [GlobalStyleFieldProps](https://app.grapesjs.com/docs-sdk/configuration/global-styles#field-properties) | The field to render in the editor UI.                                                                                                    |
| defaultValue | string, number                                                                                          | Default value to use on the CSS property.                                                                                                |
| value        | string, number                                                                                          | The value to be applied to the CSS property.                                                                                             |
| category     | object                                                                                                  | Put the style rule in a category.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "id": "h1Styles",<br>  "label": "H1 Styles"<br>}<br>` |

### Field properties [​](https://app.grapesjs.com/docs-sdk/configuration/global-styles#field-properties "Direct link to Field properties")

Below are the properties available for individual global style fields.

Show properties

#### Text

| Property     | Type   | Description                                                                                                                                                                  |
| ------------ | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type\*       | text   | Simple text field type.                                                                                                                                                      |
| defaultValue | string | Default value to use on the field. Unlike the \`defaultValue\` of the global style property, this won't affect the global style itself and will only serve as a placeholder. |

#### Color

| Property     | Type   | Description                                                                                                                                                                  |
| ------------ | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| defaultValue | string | Default value to use on the field. Unlike the \`defaultValue\` of the global style property, this won't affect the global style itself and will only serve as a placeholder. |
| type\*       | color  | Field type for colors properties.                                                                                                                                            |

#### Number

| Property     | Type   | Description                                                                                                                                                                  |
| ------------ | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| defaultValue | string | Default value to use on the field. Unlike the \`defaultValue\` of the global style property, this won't affect the global style itself and will only serve as a placeholder. |
| type\*       | number | Field type for numeric properties.                                                                                                                                           |
| min          | number | Minimum value allowed.                                                                                                                                                       |
| max          | number | Maximum value allowed.                                                                                                                                                       |
| step         | number | Step value.                                                                                                                                                                  |
| units        | array  | Units to display.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  "px",<br>  "rem"<br>]<br>`                                                                                |

#### Select

| Property     | Type   | Description                                                                                                                                                                  |
| ------------ | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| defaultValue | string | Default value to use on the field. Unlike the \`defaultValue\` of the global style property, this won't affect the global style itself and will only serve as a placeholder. |
| type\*       | select | Select field type.                                                                                                                                                           |
| options      | array  | Options to display in the field.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "id": "option1",<br>    "label": "Option 2"<br>  }<br>]<br>`                      |

#### SelectFont

| Property     | Type       | Description                                                                                                                                                                  |
| ------------ | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| defaultValue | string     | Default value to use on the field. Unlike the \`defaultValue\` of the global style property, this won't affect the global style itself and will only serve as a placeholder. |
| type\*       | selectFont | Select field type.                                                                                                                                                           |
| options      | array      | Prepend custom options to the font select.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "id": "option0",<br>    "label": "Option 0"<br>  }<br>]<br>`            |

#### Radio

| Property     | Type   | Description                                                                                                                                                                  |
| ------------ | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| defaultValue | string | Default value to use on the field. Unlike the \`defaultValue\` of the global style property, this won't affect the global style itself and will only serve as a placeholder. |
| type\*       | radio  | Similar to the select field type but render options as buttons.                                                                                                              |
| options      | array  | Options to display in the field.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "id": "option1",<br>    "label": "Option 2"<br>  }<br>]<br>`                      |

## Updating Global Styles [​](https://app.grapesjs.com/docs-sdk/configuration/global-styles#updating-global-styles "Direct link to Updating Global Styles")

Global styles are powerful and highly flexible, but be mindful when adjusting the configuration once it’s set, as updates made by end-users in the Global Styles panel will impact element styles across the project. Think of it as creating an internal design system—a well-defined configuration supports consistency, enables reusable templates, and makes future maintenance easier.

Below, you'll find examples demonstrating the effects of adding, updating, and deleting global styles in the configuration. These will illustrate how each action impacts the project's element styles.

### Adding a new style [​](https://app.grapesjs.com/docs-sdk/configuration/global-styles#adding-a-new-style "Direct link to Adding a new style")

When adding a new rule with a new `id` to the global styles, it integrates seamlessly without impacting existing styles. However, if you set a `defaultValue`, be aware that it may override the default value of the specified CSS property, potentially affecting elements styled by that rule.

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
    plugins: [\
      editor =>\
        editor.onReady(() => {\
          editor.runCommand('studio:layoutToggle', {\
            id: 'gs',\
            layout: 'panelGlobalStyles',\
            header: { label: 'Global Styles' },\
            placer: { type: 'absolute', position: 'right' }\
          });\
        })\
    ],
    project: {
      // loading by default a project containing already h1Color (blue) and h1Size (3rem)
      default: {
        styles:[{selectors:[],selectorsAdd:"h1",style:{color:{type:"data-variable",defaultValue:"red",path:"globalStyles.h1Color.value"},"font-size":{type:"data-variable",defaultValue:"2rem",path:"globalStyles.h1Size.value"}},groups:["globalStyles:h1Color","globalStyles:h1Size"]}],
        pages:[{name:"Home",component:"<h1>Home page Update</h1><p>Home page text</p>"}],
        dataSources:[{id:"globalStyles",records:[{value:"blue",id:"h1Color",property:"color",field:"color",defaultValue:"red",selector:"h1",label:"H1 color",_internal:!0},{value:"3rem",id:"h1Size",property:"font-size",field:{type:"number",min:.1,max:10,step:.1,units:["rem"]},defaultValue:"2rem",selector:"h1",label:"H1 size",_internal:!0}]}]
      }
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
        // Adding a new rule\
        {\
          id: 'h1Weight',\
          property: 'font-weight',\
          field: {\
            type: 'select',\
            options: [{ id: '500', label: 'Normal' }, { id: '600', label: 'Bold' }]\
          },\
          defaultValue: '', // skip default value to avoid any kind of override\
          selector: 'h1',\
          label: 'H1 weight'\
        },\
        {\
          id: 'h1Size',\
          property: 'font-size',\
          field: { type: 'number', min: 0.1, max: 10, step: 0.1, units: ['rem'] },\
          defaultValue: '2rem',\
          selector: 'h1',\
          label: 'H1 size'\
        }\
      ]
    }
  }}
/>

```

### Updating existing style [​](https://app.grapesjs.com/docs-sdk/configuration/global-styles#updating-existing-style "Direct link to Updating existing style")

When updating an existing global style, only specific properties can be modified to minimize disruptions to current styles. For example, properties like `label` and `field` can be updated, but changes to `property`, `defaultValue`, or `selector` are skipped to avoid breaking existing style rules.

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
    plugins: [\
      editor =>\
        editor.onReady(() => {\
          editor.runCommand('studio:layoutToggle', {\
            id: 'gs',\
            layout: 'panelGlobalStyles',\
            header: { label: 'Global Styles' },\
            placer: { type: 'absolute', position: 'right' }\
          });\
        })\
    ],
    project: {
      // loading by default a project containing h1Color (blue), h1Size (3rem) and h1Weight (500)
      default: {
        styles:[{selectors:[],selectorsAdd:"h1",style:{color:{type:"data-variable",defaultValue:"red",path:"globalStyles.h1Color.value"},"font-size":{type:"data-variable",defaultValue:"2rem",path:"globalStyles.h1Size.value"},"font-weight":{type:"data-variable",defaultValue:"",path:"globalStyles.h1Weight.value"}},groups:["globalStyles:h1Color","globalStyles:h1Size","globalStyles:h1Weight"]}],
        pages:[{name:"Home",component:"<h1>Home page Update</h1><p>Home page text</p>"}],
        dataSources:[{id:"globalStyles",records:[{value:"blue",id:"h1Color",property:"color",field:"color",defaultValue:"red",selector:"h1",label:"H1 color",_internal:!0},{value:"3rem",id:"h1Size",property:"font-size",field:{type:"number",min:.1,max:10,step:.1,units:["rem"]},defaultValue:"2rem",selector:"h1",label:"H1 size",_internal:!0},{value:"500",id:"h1Weight",property:"font-weight",field:{type:"select",options:[{id:"500",label:"Normal"},{id:"600",label:"Bold"}]},defaultValue:"",selector:"h1",label:"H1 weight",_internal:!0}]}]
      }
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
        {\
          // Same id, as a different one would create a new global style\
          id: 'h1Weight',\
\
          // Skipped from updates\
          property: 'font-family',\
          defaultValue: 'another value',\
          selector: 'h5',\
\
          // Valid for updates\
          label: 'H1 weight (Up)',\
          field: { type: 'number', min: 500, max: 600, step: 100 },\
        },\
        {\
          id: 'h1Size',\
          property: 'font-size',\
          field: { type: 'number', min: 0.1, max: 10, step: 0.1, units: ['rem'] },\
          defaultValue: '2rem',\
          selector: 'h1',\
          label: 'H1 size'\
        }\
      ]

    }
  }}
/>

```

### Removing existing style [​](https://app.grapesjs.com/docs-sdk/configuration/global-styles#removing-existing-style "Direct link to Removing existing style")

Removing a rule from the global styles configuration will prevent it from appearing in the Global Styles panel, but it won't delete the style from the project. This ensures that any styles previously applied remain intact, avoiding unexpected changes to the project's appearance.

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
    plugins: [\
      editor =>\
        editor.onReady(() => {\
          editor.runCommand('studio:layoutToggle', {\
            id: 'gs',\
            layout: 'panelGlobalStyles',\
            header: { label: 'Global Styles' },\
            placer: { type: 'absolute', position: 'right' }\
          });\
        })\
    ],
    project: {
      // loading by default a project containing h1Color (blue), h1Size (3rem) and h1Weight (500)
      default: {
        styles:[{selectors:[],selectorsAdd:"h1",style:{color:{type:"data-variable",defaultValue:"red",path:"globalStyles.h1Color.value"},"font-size":{type:"data-variable",defaultValue:"2rem",path:"globalStyles.h1Size.value"},"font-weight":{type:"data-variable",defaultValue:"",path:"globalStyles.h1Weight.value"}},groups:["globalStyles:h1Color","globalStyles:h1Size","globalStyles:h1Weight"]}],
        pages:[{name:"Home",component:"<h1>Home page Update</h1><p>Home page text</p>"}],
        dataSources:[{id:"globalStyles",records:[{value:"blue",id:"h1Color",property:"color",field:"color",defaultValue:"red",selector:"h1",label:"H1 color",_internal:!0},{value:"3rem",id:"h1Size",property:"font-size",field:{type:"number",min:.1,max:10,step:.1,units:["rem"]},defaultValue:"2rem",selector:"h1",label:"H1 size",_internal:!0},{value:"500",id:"h1Weight",property:"font-weight",field:{type:"select",options:[{id:"500",label:"Normal"},{id:"600",label:"Bold"}]},defaultValue:"",selector:"h1",label:"H1 weight",_internal:!0}]}]
      }
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
        {\
          id: 'h1Size',\
          property: 'font-size',\
          field: { type: 'number', min: 0.1, max: 10, step: 0.1, units: ['rem'] },\
          defaultValue: '2rem',\
          selector: 'h1',\
          label: 'H1 size'\
        }\
        // Removed 'h1Weight'\
      ]
    }
  }}
/>

```

## I18n [​](https://app.grapesjs.com/docs-sdk/configuration/global-styles#i18n "Direct link to I18n")

The labels of the Global Styles panel can be translated into different languages based on the `id` assigned to each global style rule. The configuration in `i18n` takes precedence over labels defined directly in the global styles configuration.

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
    plugins: [\
      editor =>\
        editor.onReady(() => {\
          editor.runCommand('studio:layoutToggle', {\
            id: 'gs',\
            layout: 'panelGlobalStyles',\
            header: { label: 'Global Styles' },\
            placer: { type: 'absolute', position: 'right' }\
          });\
        })\
    ],
    project: {
      default: {pages: [{ name: 'Home', component: '<h1>Home page</h1>'}]}
    },
    globalStyles: {
      default: [\
        {\
          id: 'h1Color',\
          property: 'color',\
          category: { id: 'h1Styles', label: 'H1 styles', open: true },\
          field: 'color',\
          defaultValue: 'red',\
          selector: 'h1',\
          label: 'H1 color (Up)'\
        },\
        {\
          id: 'h1Size',\
          property: 'font-size',\
          field: {\
            type: 'select',\
            options: [\
              { id: '2rem', label: 'Normal' },\
              { id: '3rem', label: 'Big' }\
            ]\
          },\
          category: { id: 'h1Styles' },\
          defaultValue: '2rem',\
          selector: 'h1',\
          label: 'H1 size'\
        }\
      ]
    },
    i18n: {
      locales: {
        en: {
          globalStyleManager: {
            notFound: 'No global styles found',
            fields: {
              // The key is the global style id
              h1Color: {
                label: 'H1 color (EN)'
              },
              h1Size: {
                label: 'H1 size (EN)',
                options: {
                  '2rem': 'Normal (EN)',
                  '3rem': 'Big (EN)'
                }
              }
            },
            categories: {
              // category id
              h1Styles: {
                label: 'H1 styles (EN)'
              }
            }
          }
        }
      }
    }
  }}
/>

```

## Advanced Usage [​](https://app.grapesjs.com/docs-sdk/configuration/global-styles#advanced-usage "Direct link to Advanced Usage")

In this section, we'll explore a more comprehensive example of global styles configuration, including grouping styles with categories, using CSS variables for dynamic theming, and integrating styles with an external CSS library. These features offer enhanced flexibility and control over your design system.

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
    plugins: [\
      editor =>\
        editor.onReady(() => {\
          editor.runCommand('studio:layoutToggle', {\
            id: 'gs',\
            layout: 'panelGlobalStyles',\
            header: { label: 'Global Styles' },\
            placer: { type: 'absolute', position: 'right' }\
          });\
        })\
    ],
    project: {
      default: {
        pages: [\
          {\
            name: 'Home',\
            component: `<!doctype html>\
              <html>\
                <head>\
                  <meta name="viewport" content="width=device-width, initial-scale=1">\
                  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">\
                </head>\
                <body>\
                  <style>\
                    .btn {\
                      border: none;\
                    }\
                    .text-primary {\
                      color: var(--bs-primary) !important;\
                    }\
                    .bg-secondary {\
                      background-color: var(--bs-secondary) !important;\
                    }\
                    .btn-primary {\
                      --bs-btn-bg: var(--bs-primary);\
                    }\
                    .btn-primary-soft {\
                      --bs-btn-bg: color-mix(in srgb, var(--bs-primary), transparent 90%);\
                      --bs-btn-color: var(--bs-primary);\
                    }\
                  </style>\
                  <nav class="navbar navbar-expand-lg py-3">\
                    <div class="container px-5">\
                        <a class="navbar-brand text-primary" href="##">Demo section</a>\
                        <div class="collapse navbar-collapse">\
                            <ul class="navbar-nav ms-auto me-lg-5">\
                                <li class="nav-item"><a class="nav-link" href="##">Home</a></li>\
                                <li class="nav-item"><a class="nav-link" href="##">About</a></li>\
                                <li class="nav-item"><a class="nav-link" href="##">Contact</a></li>\
                            </ul>\
                            <a class="my-btn-cls btn ms-lg-4 btn-primary" href="##">\
                                Button 1\
                            </a>\
                        </div>\
                    </div>\
                  </nav>\
                  <header>\
                    <div class="pt-5">\
                        <div class="container px-5">\
                            <div class="row gx-5 align-items-center">\
                                <div class="col-lg-6">\
                                    <h1 class="mb-3">Build your project faster in Studio</h1>\
                                    <p class="mb-5">Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aenean commodo ligula eget dolor.</p>\
                                    <div class="d-flex flex-column flex-sm-row">\
                                        <a class="my-btn-cls btn btn-lg btn-primary me-sm-3 mb-3 mb-sm-0" href="#btn2">\
                                            Button 2\
                                        </a>\
                                        <a class="my-btn-cls btn btn-lg btn-primary-soft" href="##" target="_blank">\
                                          Button 3\
                                        </a>\
                                    </div>\
                                </div>\
                                <div class="col-lg-6">\
                                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 678.6 510.8">\
                                    <path d="M353 90H187c-7 0-12-6-12-12V12c0-7 5-12 12-12h166c7 0 12 5 12 12v66c0 6-5 12-12 12Z" fill="var(--bs-primary)"/>\
                                    <path d="M353 293H187c-7 0-12-6-12-12v-66c0-7 5-12 12-12h166c7 0 12 5 12 12v66c0 6-5 12-12 12Zm-1-103h-65c-7 0-12-6-12-12v-64c0-7 5-12 12-12h65c7 0 13 5 13 12v64c0 6-6 12-13 12Zm-100 0h-65c-7 0-12-6-12-12v-64c0-7 5-12 12-12h65c7 0 13 5 13 12v64c0 6-6 12-13 12Z" fill="var(--bs-secondary)"/>\
                                    <path d="m679 509-2 2H2a1 1 0 1 1 0-3h675l2 1Z" fill="#2f2e43"/>\
                                    <path fill="#ec9c9f" d="m522 454-14 15-15-14 14-14 15 13z"/>\
                                    <path d="m533 469-30 31c-2 3-5 5-9 6l-8 3c-2 1-4 0-5-1s-2-4-1-6l8-14 8-30h1l6 3c4 1 7-1 9-3 3-4 1-10 1-10 1-1 2-2 3-1 2 0 3 2 4 3 2 0 8 4 8 5 3 0 5 0 6 2 2 1 2 3 2 5s-1 5-3 7Z" fill="#090814"/>\
                                    <path fill="#ec9c9f" d="M486 471h20v20h-20z"/>\
                                    <path d="M502 509h-43c-3 0-7-1-10-3l-8-3-3-4c0-3 2-4 4-5l16-4 28-15 1 7c3 3 6 5 9 4 5 0 9-6 9-6l2 1 1 5 2 10 3 6-3 4c-2 2-5 3-8 3Zm6-289h-71l-24 149 71 85 24-21-56-64 56-149z" fill="#090814"/>\
                                    <path fill="#090814" d="m466 242 42-22v255l-24 1-18-234z"/>\
                                    <path d="m423 280 1-23-14-1v24c-3 2-4 6-4 10-1 7 4 13 10 13s11-5 11-13c0-4-1-8-4-10Z" fill="#ec9c9f"/>\
                                    <path d="M488 53s19 0 12-19c-7-18-21-10-21-10s-6 3-4 10" fill="#090814"/>\
                                    <path d="M490 62a26 26 0 1 0-34 25l5 33 26-21s-6-7-9-15c7-5 12-13 12-22Z" fill="#ec9c9f"/>\
                                    <path d="M476 84s4-13-2-19-6 3-6 3l-4-1s0-9-8-10l5-11s-19 3-20 1c-4-15 37-27 48-4 17 35-13 41-13 41Z" fill="#090814"/>\
                                    <path d="M453 107c-19 4-33 20-35 39l-11 131h20l26-170Z" fill="var(--bs-primary)"/>\
                                    <path fill="var(--bs-primary)" d="m434 212-4 37 88-26-11-27 20-45-41-58-32 2-12 16"/>\
                                    <path d="m505 247 12-20-12-7-12 20c-3 1-6 3-9 7-3 6-2 13 3 16 5 4 12 1 16-5 2-4 3-8 2-11Z" fill="#ec9c9f"/>\
                                    <path d="m480 94 7-1 12 10c14 2 18 3 23 12 2 4 3 8 3 13l11 64-26 62-17-19 18-49-3-11" fill="var(--bs-primary)"/>\
                                  </svg>\
                                </div>\
                            </div>\
                        </div>\
                    </div>\
                    <div class="mt-5" style="margin-bottom: -1px;">\
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 144.54 17.34" preserveAspectRatio="none" fill="var(--bs-secondary)"><path d="M144.54,17.34H0V0H144.54ZM0,0S32.36,17.34,72.27,17.34,144.54,0,144.54,0"></path></svg>\
                    </div>\
                  </header>\
                  <section class="bg-secondary py-5">\
                    <div class="container px-5">\
                        <div class="row gx-5 justify-content-center">\
                            <div class="col-lg-8">\
                                <div class="text-center mb-10">\
                                    <h1 class="mb-2">Build your project faster in Studio</h1>\
                                    <p class="lead">Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aenean commodo ligula eget dolor.</p>\
                                </div>\
                            </div>\
                        </div>\
                    </div>\
                  </section>\
                </body>\
              </html>\
            `\
          }\
        ]
      }
    },
    globalStyles: {
      default: [\
        // Variables\
        {\
          id: 'varPrimary',\
          property: '--bs-primary', // connect to a CSS variable\
          field: 'color',\
          selector: ':root',\
          label: 'Primary color',\
          defaultValue: '#0d6efd',\
          category: { id: 'vars', label: 'Variables', open: true }\
        },\
        {\
          id: 'varSecondary',\
          property: '--bs-secondary',\
          field: 'color',\
          selector: ':root',\
          label: 'Secondary color',\
          defaultValue: '#eff0f1',\
          category: { id: 'vars' }\
        },\
        // Body\
        {\
          id: 'bodyBg',\
          property: 'background-color',\
          field: 'color',\
          selector: 'body',\
          label: 'Body background',\
          defaultValue: 'white',\
          category: { id: 'body', label: 'Body styles' }\
        },\
        {\
          id: 'bodyColor',\
          property: 'color',\
          field: 'color',\
          selector: 'body',\
          label: 'Body color',\
          defaultValue: '#484c51',\
          category: { id: 'body' }\
        },\
        // H1\
        {\
          id: 'h1Color',\
          property: 'color',\
          field: 'color',\
          selector: 'h1',\
          label: 'H1 color',\
          defaultValue: 'inherit',\
          category: { id: 'h1', label: 'H1' }\
        },\
        {\
          id: 'h1Size',\
          property: 'font-size',\
          field: { type: 'number', min: 0.1, max: 10, step: 0.1, units: ['rem'] },\
          defaultValue: '2rem',\
          selector: 'h1',\
          label: 'H1 size',\
          category: { id: 'h1' }\
        },\
        // Button\
        {\
          id: 'btnColor',\
          property: 'background-color',\
          field: 'color',\
          selector: '.btn-primary',\
          label: 'Primary button color',\
          category: { id: 'buttons', label: 'Buttons' }\
        },\
        {\
          id: 'btnRadius',\
          property: 'border-radius',\
          field: {\
            type: 'select',\
            options: [\
              { id: '0', label: 'None' },\
              { id: '', label: 'Default' },\
              { id: '1rem', label: 'Large' },\
              { id: '10rem', label: 'Full' }\
            ]\
          },\
          selector: '.btn',\
          label: 'Button radius',\
          category: { id: 'buttons' }\
        }\
      ]
    }
  }}
/>

```

## Usage in Email Projects [​](https://app.grapesjs.com/docs-sdk/configuration/global-styles#usage-in-email-projects "Direct link to Usage in Email Projects")

Global Styles can also be used in email projects with the same configuration approach as web, allowing you to style MJML components or specific selectors.

The main difference is that, in addition to using classic selectors like `.some-link-wrapper > a`, you can also apply global styles directly to MJML tags, such as `mj-section`, `mj-button`, or `mj-text`.

warning

You can also continue using HTML selectors in email templates, but note that MJML components are often compiled into inline styles, so targeting the MJML tags directly gives more predictable results.

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
    plugins: [\
      editor =>\
        editor.onReady(() => {\
          editor.runCommand('studio:layoutToggle', {\
            id: 'gs',\
            layout: 'panelGlobalStyles',\
            header: { label: 'Global Styles' },\
            placer: { type: 'absolute', position: 'left' }\
          });\
        })\
    ],
    project: {
      type: 'email',
      default: {
        pages: [\
          {\
            component: `<mjml>\
              <mj-head>\
                <mj-attributes>\
                  <mj-text />\
                  <mj-section background-color="#eee" />\
                  <mj-button background-color="#cf549e" />\
                </mj-attributes>\
              </mj-head>\
              <mj-body>\
                <mj-section>\
                  <mj-column background-color="#ddd">\
                    <mj-text>Some text and a <a href="##">link</a></mj-text>\
                    <mj-button>Button</mj-button>\
                  </mj-column>\
                </mj-section>\
              </mj-body>\
            </mjml>`\
          }\
        ]
      }
    },
    globalStyles: {
      default: [\
        // ALL\
        {\
          id: 'all-color',\
          field: { type: 'color', defaultValue: '#000000' },\
          label: 'Color',\
          property: 'color',\
          selector: 'mj-all',\
          category: { id: 'all', label: 'All', open: true }\
        },\
        {\
          id: 'all-font-size',\
          field: { type: 'number', min: 0, step: 1, units: ['px'], defaultValue: '13px' },\
          label: 'Font size',\
          property: 'font-size',\
          selector: 'mj-all',\
          category: { id: 'all' }\
        },\
        // Sections\
        {\
          id: 'section-bg',\
          field: { type: 'color', defaultValue: 'initial' },\
          label: 'Background',\
          property: 'background-color',\
          selector: 'mj-section',\
          category: { id: 'sections', label: 'Sections', open: true }\
        },\
        // Text\
        {\
          id: 'text-color',\
          field: { type: 'color', defaultValue: '#000000' },\
          label: 'Color',\
          property: 'color',\
          selector: 'mj-text',\
          category: { id: 'text', label: 'Text', open: true }\
        },\
        // Links\
        {\
          id: 'link-color',\
          field: { type: 'color', defaultValue: '#000000' },\
          label: 'Color',\
          property: 'color',\
          selector: 'a',\
          category: { id: 'links', label: 'Links', open: true }\
        },\
        // Buttons\
        {\
          id: 'button-bg',\
          field: { row: true, type: 'color', defaultValue: '#414141' },\
          label: 'Background',\
          property: 'background-color',\
          selector: 'mj-button',\
          category: { id: 'buttons', label: 'Buttons', open: true }\
        },\
        {\
          id: 'button-border-radius',\
          field: { type: 'number', min: 0, units: ['px'], defaultValue: '3px' },\
          label: 'Border radius',\
          property: 'border-radius',\
          selector: 'mj-button',\
          category: { id: 'buttons' }\
        }\
      ]
    },
  }}
/>

```

- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/global-styles#initialization)
- [Properties](https://app.grapesjs.com/docs-sdk/configuration/global-styles#properties)
  - [Global Style properties](https://app.grapesjs.com/docs-sdk/configuration/global-styles#global-style-properties)
  - [Field properties](https://app.grapesjs.com/docs-sdk/configuration/global-styles#field-properties)
- [Updating Global Styles](https://app.grapesjs.com/docs-sdk/configuration/global-styles#updating-global-styles)
  - [Adding a new style](https://app.grapesjs.com/docs-sdk/configuration/global-styles#adding-a-new-style)
  - [Updating existing style](https://app.grapesjs.com/docs-sdk/configuration/global-styles#updating-existing-style)
  - [Removing existing style](https://app.grapesjs.com/docs-sdk/configuration/global-styles#removing-existing-style)
- [I18n](https://app.grapesjs.com/docs-sdk/configuration/global-styles#i18n)
- [Advanced Usage](https://app.grapesjs.com/docs-sdk/configuration/global-styles#advanced-usage)
- [Usage in Email Projects](https://app.grapesjs.com/docs-sdk/configuration/global-styles#usage-in-email-projects)

---

# File: app.grapesjs.com_docs-sdk_configuration_layout_components.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/layout/components"
title: "Layout Components | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/layout/components#__docusaurus_skipToContent_fallback)

On this page

Below is a list of layout components available to help you compose your editor interface.

## Row [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#row "Direct link to Row")

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

#### Row properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#row-properties "Direct link to Row properties")

Show properties

| Property       | Type             | Description                                                                                                                                                  |
| -------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----- | -------- | ---------- | ---------------- | --------------- |
| type\*         | row              | Type of the layout component.                                                                                                                                |
| alignItems     | string           | The alignment of inner components along the cross axis.<br>**Example**<br>```codeBlockLines_AdAo<br>"start"                                                  | "end" | "center" | "baseline" | "stretch"<br>``` |
| as             | string           | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                                           |
| children       | Layout component | The children layout components.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>` |
| className      | string           | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                 |
| full           | boolean          | If true, the component will take up the full available space.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                          |
| gap            | number           | The gap between inner components.<br>**Example** <br>`codeBlockLines_AdAo<br>10<br>`                                                                         |
| grow           | boolean          | If true, the component will grow to fill available space.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                              |
| height         | string, number   | The height of the component.<br>**Example** <br>`codeBlockLines_AdAo<br>100<br>`                                                                             |
| htmlAttrs      | object           | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`                            |
| id             | string           | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                      |
| justifyContent | string           | The alignment of inner components along the main axis.<br>**Example**<br>```codeBlockLines_AdAo<br>"start"                                                   | "end" | "center" | "between"  | "around"         | "evenly"<br>``` |
| style          | object           | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                      |
| width          | string, number   | The width of the component.<br>**Example** <br>`codeBlockLines_AdAo<br>100<br>`                                                                              |

## Column [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#column "Direct link to Column")

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

#### Column properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#column-properties "Direct link to Column properties")

Show properties

| Property       | Type             | Description                                                                                                                                                  |
| -------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----- | -------- | ---------- | ---------------- | --------------- |
| type\*         | column           | Type of the layout component.                                                                                                                                |
| alignItems     | string           | The alignment of inner components along the cross axis.<br>**Example**<br>```codeBlockLines_AdAo<br>"start"                                                  | "end" | "center" | "baseline" | "stretch"<br>``` |
| as             | string           | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                                           |
| children       | Layout component | The children layout components.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>` |
| className      | string           | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                 |
| full           | boolean          | If true, the component will take up the full available space.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                          |
| gap            | number           | The gap between inner components.<br>**Example** <br>`codeBlockLines_AdAo<br>10<br>`                                                                         |
| grow           | boolean          | If true, the component will grow to fill available space.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                              |
| height         | string, number   | The height of the component.<br>**Example** <br>`codeBlockLines_AdAo<br>100<br>`                                                                             |
| htmlAttrs      | object           | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`                            |
| id             | string           | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                      |
| justifyContent | string           | The alignment of inner components along the main axis.<br>**Example**<br>```codeBlockLines_AdAo<br>"start"                                                   | "end" | "center" | "between"  | "around"         | "evenly"<br>``` |
| style          | object           | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                      |
| width          | string, number   | The width of the component.<br>**Example** <br>`codeBlockLines_AdAo<br>100<br>`                                                                              |

## Text [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#text "Direct link to Text")

A basic text element that can be added to components accepting children. Text can be included either directly as a string or as an object `{ type: 'text', content: '...' }`. For usage examples, see the configurations above.

#### Text properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#text-properties "Direct link to Text properties")

Show properties

| Property  | Type             | Description                                                                                                                                                  |
| --------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| type\*    | text             | Type of the layout component.                                                                                                                                |
| content\* | string           | Content of the text.<br>**Example**<br>`codeBlockLines_AdAo<br>"Hello, World!"<br>`                                                                          |
| as        | string           | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                                           |
| children  | Layout component | The children layout components.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>` |
| className | string           | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                 |
| htmlAttrs | object           | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`                            |
| id        | string           | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                      |
| style     | object           | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                      |

## Tabs [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#tabs "Direct link to Tabs")

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

#### Tabs properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#tabs-properties "Direct link to Tabs properties")

Show properties

| Property     | Type     | Description                                                                                                                                                                                                                    |
| ------------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| type\*       | tabs     | Type of the layout component.                                                                                                                                                                                                  |
| tabs\*       | array    | Tabs configuration.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "id": "tab1",<br>    "label": "Tab 1",<br>    "children": [<br>      "Content Tab 1"<br>    ]<br>  }<br>]<br>`                                   |
| className    | string   | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                                                                                   |
| editorEvents | object   | Update layout component state based on editor events.<br>**Example**<br>`codeBlockLines_AdAo<br>{ 'component:selected': ({ setState, editor }) => setState({ className: 'custom-cls-' + editor.getSelected().getId() }) }<br>` |
| id           | string   | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                                                                                        |
| onChange     | function | The function to call when the field value changes.<br>**Example**<br>`codeBlockLines_AdAo<br>({ value, setState }) => setState({ value });<br>`                                                                                |
| style        | object   | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                                                                                        |
| value        | string   | Tab id value to select on initial render.<br>**Example**<br>`codeBlockLines_AdAo<br>"tab1"<br>`                                                                                                                                |
| variant      | string   | Variant of the tabs.<br>**Example**<br>`codeBlockLines_AdAo<br>"pills"<br>`                                                                                                                                                    |

## Button [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#button "Direct link to Button")

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

#### Button properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#button-properties "Direct link to Button properties")

Show properties

| Property     | Type             | Description                                                                                                                                                  |
| ------------ | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------- | ---------------- | --- | ----------- |
| type\*       | button           | Type of the layout component.                                                                                                                                |
| active       | boolean          | Indicates if the button is active.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                     |
| as           | string           | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                                           |
| buttonType   | string           | HTML button type attribute.<br>**Example**<br>```codeBlockLines_AdAo<br>"button"                                                                             | "submit"  | "reset"<br>```   |
| children     | Layout component | The children layout components.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>` |
| className    | string           | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                 |
| classNameBtn | string           | Class name for the inner button element.<br>**Example**<br>`codeBlockLines_AdAo<br>"btn-class"<br>`                                                          |
| disabled     | boolean          | Indicates whether the component is disabled.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                           |
| editorEvents | object           | Events to be handled by the editor.<br>**Example**<br>`codeBlockLines_AdAo<br>{ 'component:add': ({ editor }) => { console.log('Component added'); } }<br>`  |
| icon         | string           | Icon to be displayed in the button.<br>**Example**<br>`codeBlockLines_AdAo<br>"close"<br>`                                                                   |
| id           | string           | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                      |
| label        | string           | Label for the button, can be a ReactNode or a function returning a ReactNode.<br>**Example**<br>`codeBlockLines_AdAo<br>"Button"<br>`                        |
| onClick      | function         | Click event handler for the button.<br>**Example**<br>`codeBlockLines_AdAo<br>({ event }) => { alert('Button clicked!'); }<br>`                              |
| size         | string           | Size of the button.<br>**Example**<br>```codeBlockLines_AdAo<br>"x2s"                                                                                        | "xs"      | "s"              | "m" | "lg"<br>``` |
| style        | object           | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                      |
| tooltip      | string           | Tooltip text for the button.<br>**Example**<br>`codeBlockLines_AdAo<br>"Click to submit"<br>`                                                                |
| variant      | string           | Variant of the button style.<br>**Example**<br>```codeBlockLines_AdAo<br>"primary"                                                                           | "outline" | "shallow"<br>``` |

## Devices [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#devices "Direct link to Devices")

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

#### Devices properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#devices-properties "Direct link to Devices properties")

Show properties

| Property  | Type    | Description                                                                                                                             |
| --------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| type\*    | devices | Type of the layout component.                                                                                                           |
| as        | string  | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                      |
| className | string  | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                            |
| id        | string  | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                 |
| style     | object  | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>` |

## PanelPages [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelpages "Direct link to PanelPages")

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

#### PanelPages properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelpages-properties "Direct link to PanelPages properties")

Show properties

| Property  | Type       | Description                                                                                                                             |
| --------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| type\*    | panelPages | Type of the layout component.                                                                                                           |
| as        | string     | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                      |
| className | string     | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                            |
| header    | object     | Header of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>`          |
| htmlAttrs | object     | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`       |
| id        | string     | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                 |
| resizable | object     | Resizable configuration of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>`       |
| style     | object     | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>` |

## PanelPageSettings [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelpagesettings "Direct link to PanelPageSettings")

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

#### PanelPageSettings properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelpagesettings-properties "Direct link to PanelPageSettings properties")

Show properties

| Property  | Type                                            | Description                                                                                                                             |
| --------- | ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| type\*    | panelPageSettings                               | Type of the layout component.                                                                                                           |
| as        | string                                          | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                      |
| className | string                                          | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                            |
| header    | object                                          | Header of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>`          |
| htmlAttrs | object                                          | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`       |
| id        | string                                          | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                 |
| page      | [Page](https://grapesjs.com/docs/api/page.html) | The page to display settings for.<br>**Example** <br>`codeBlockLines_AdAo<br>editor.Pages.getSelected();<br>`                           |
| resizable | object                                          | Resizable configuration of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>`       |
| style     | object                                          | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>` |

## PanelLayers [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panellayers "Direct link to PanelLayers")

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

#### PanelLayers properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panellayers-properties "Direct link to PanelLayers properties")

Show properties

| Property  | Type        | Description                                                                                                                             |
| --------- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| type\*    | panelLayers | Type of the layout component.                                                                                                           |
| as        | string      | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                      |
| className | string      | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                            |
| header    | object      | Header of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>`          |
| htmlAttrs | object      | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`       |
| id        | string      | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                 |
| resizable | object      | Resizable configuration of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>`       |
| style     | object      | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>` |

## PanelPagesLayers [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelpageslayers "Direct link to PanelPagesLayers")

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

#### PanelPagesLayers properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelpageslayers-properties "Direct link to PanelPagesLayers properties")

Show properties

| Property         | Type                                                                                                         | Description                                                                                                                                |
| ---------------- | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| type\*           | panelPagesLayers                                                                                             | Type of the layout component.                                                                                                              |
| as               | string                                                                                                       | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                         |
| className        | string                                                                                                       | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                               |
| header           | object                                                                                                       | Header of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>`             |
| htmlAttrs        | object                                                                                                       | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`          |
| id               | string                                                                                                       | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                    |
| panelLayersProps | [PanelLayersProps](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panellayers-properties) | Properties for the panel layers.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "header": {<br>    "label": "My Layers"<br>  }<br>}<br>` |
| panelPagesProps  | [PanelPagesProps](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelpages-properties)   | Properties for the panel pages.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "header": {<br>    "label": "My Pages"<br>  }<br>}<br>`   |
| resizable        | object                                                                                                       | Resizable configuration of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>`          |
| style            | object                                                                                                       | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`    |

## PanelBlocks [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelblocks "Direct link to PanelBlocks")

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

#### PanelBlocks properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelblocks-properties "Direct link to PanelBlocks properties")

Show properties

| Property       | Type        | Description                                                                                                                                                                                                                                                                                      |
| -------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| type\*         | panelBlocks | Type of the layout component.                                                                                                                                                                                                                                                                    |
| as             | string      | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                                                                                                                                                                               |
| blocks         | function    | Filter blocks.                                                                                                                                                                                                                                                                                   |
| className      | string      | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                                                                                                                                                     |
| header         | object      | Header of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>`                                                                                                                                                                   |
| hideCategories | boolean     | Whether to hide categories.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                                                                                                                                |
| htmlAttrs      | object      | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`                                                                                                                                                                |
| id             | string      | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                                                                                                                                                          |
| itemLayout     | function    | Custom layout for rendering single blocks.<br>**Example** <br>`codeBlockLines_AdAo<br>itemLayout: ({ block, attributes }) => ({<br> type: 'column',<br> children: [<br>   { type: 'custom', render: () => block.getMedia() },<br>   { type: 'text', content: block.getLabel() }<br> ]<br>})<br>` |
| resizable      | object      | Resizable configuration of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>`                                                                                                                                                                |
| search         | boolean     | Whether to show search.<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>`                                                                                                                                                                                                                     |
| style          | object      | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                                                                                                                                                          |
| symbols        | boolean     | Whether to show symbols.<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>`                                                                                                                                                                                                                    |

## PanelGlobalStyles [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelglobalstyles "Direct link to PanelGlobalStyles")

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

#### PanelGlobalStyles properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelglobalstyles-properties "Direct link to PanelGlobalStyles properties")

Show properties

| Property  | Type              | Description                                                                                                                             |
| --------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| type\*    | panelGlobalStyles | Type of the layout component.                                                                                                           |
| as        | string            | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                      |
| className | string            | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                            |
| header    | object            | Header of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>`          |
| htmlAttrs | object            | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`       |
| id        | string            | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                 |
| resizable | object            | Resizable configuration of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>`       |
| style     | object            | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>` |

## PanelSelectors [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelselectors "Direct link to PanelSelectors")

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

#### PanelSelectors properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelselectors-properties "Direct link to PanelSelectors properties")

Show properties

| Property      | Type           | Description                                                                                                                             |
| ------------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| type\*        | panelSelectors | Type of the layout component.                                                                                                           |
| as            | string         | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                      |
| className     | string         | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                            |
| header        | object         | Header of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>`          |
| htmlAttrs     | object         | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`       |
| id            | string         | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                 |
| resizable     | object         | Resizable configuration of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>`       |
| stateSelector | boolean        | Enable the state selector.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                        |
| style         | object         | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>` |
| styleCatalog  | boolean        | Enable the style catalog.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                         |

## PanelStyles [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelstyles "Direct link to PanelStyles")

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

#### PanelStyles properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelstyles-properties "Direct link to PanelStyles properties")

Show properties

| Property  | Type        | Description                                                                                                                             |
| --------- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| type\*    | panelStyles | Type of the layout component.                                                                                                           |
| as        | string      | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                      |
| className | string      | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                            |
| header    | object      | Header of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>`          |
| htmlAttrs | object      | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`       |
| id        | string      | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                 |
| resizable | object      | Resizable configuration of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>`       |
| style     | object      | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>` |

## PanelProperties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelproperties "Direct link to PanelProperties")

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

#### PanelProperties properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelproperties-properties "Direct link to PanelProperties properties")

Show properties

| Property  | Type            | Description                                                                                                                             |
| --------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| type\*    | panelProperties | Type of the layout component.                                                                                                           |
| as        | string          | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                      |
| className | string          | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                            |
| header    | object          | Header of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>`          |
| htmlAttrs | object          | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`       |
| id        | string          | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                 |
| resizable | object          | Resizable configuration of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>`       |
| style     | object          | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>` |

## PanelSidebarTabs [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelsidebartabs "Direct link to PanelSidebarTabs")

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

#### PanelSidebarTabs properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelsidebartabs-properties "Direct link to PanelSidebarTabs properties")

Show properties

| Property  | Type             | Description                                                                                                                             |
| --------- | ---------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| type\*    | panelSidebarTabs | Type of the layout component.                                                                                                           |
| as        | string           | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                      |
| className | string           | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                            |
| header    | object           | Header of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>`          |
| htmlAttrs | object           | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`       |
| id        | string           | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                 |
| resizable | object           | Resizable configuration of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>`       |
| style     | object           | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>` |

## PanelAssets [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelassets "Direct link to PanelAssets")

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

#### PanelAssets properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#panelassets-properties "Direct link to PanelAssets properties")

Show properties

| Property         | Type        | Description                                                                                                                                                                                                                                                                                                                                                                                     |
| ---------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type\*           | panelAssets | Type of the layout component.                                                                                                                                                                                                                                                                                                                                                                   |
| as               | string      | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                                                                                                                                                                                                                                                                              |
| assets           | array       | A custom array of assets. Overrides any other configured onLoad or providers.<br>**Example** <br>`codeBlockLines_AdAo<br>assets: [<br> {<br>   type: 'image',<br>   url: 'https://example.com/image.jpg',<br> }<br>]<br>`                                                                                                                                                                       |
| className        | string      | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                                                                                                                                                                                                                                                    |
| close            | function    | Define how to close your AssetManager. For the main AssetManager, it closes the dialog.<br>**Example**<br>`codeBlockLines_AdAo<br>() => AssetManager.close()<br>`                                                                                                                                                                                                                               |
| content          | object      | Content configuration for the AssetManager.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br> itemsPerRow: 4,<br> header: {<br>  search: true,<br>},<br>`                                                                                                                                                                                                                                         |
| header           | object      | Header of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>`                                                                                                                                                                                                                                                                  |
| htmlAttrs        | object      | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`                                                                                                                                                                                                                                                               |
| id               | string      | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                                                                                                                                                                                                                                                         |
| onSelect         | function    | Callback when an asset is selected.<br>**Example** <br>`codeBlockLines_AdAo<br>onSelect: ({ asset, editor }) => {<br>   editor.AssetManager.add(asset);<br>}<br>`                                                                                                                                                                                                                               |
| optionalProvider | boolean     | When true, adds \`{ id: undefined, label: 'Project assets' }\` as an extra option for the asset provider filter at the beginning.<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>`                                                                                                                                                                                                          |
| optionalType     | boolean     | When true, adds \`{ id: undefined, label: 'All' }\` as an extra option for the asset type filter at the beginning.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                                                                                                                                        |
| providerId       | string      | Initial state of the asset provider filter.<br>**Example**<br>`codeBlockLines_AdAo<br>"unsplash"<br>`                                                                                                                                                                                                                                                                                           |
| providers        | array       | The ids of providers that will be available in this assets panel layout. These providers need to exist in {@link AssetsConfig.providers } first. Defaults to the all providers specified in {@link AssetsConfig } . Set to an empty array to hide the asset provider filter.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  "unsplash"<br>]<br>`                                              |
| resizable        | object      | Resizable configuration of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>`                                                                                                                                                                                                                                                               |
| style            | object      | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                                                                                                                                                                                                                                                         |
| typeId           | string      | Initial state of the asset type filter.<br>**Default** <br>`codeBlockLines_AdAo<br>image<br>`                                                                                                                                                                                                                                                                                                   |
| types            | array       | Options that show up in the asset type filter. The selected type filters providers in the asset provider filter by \`assetProvider.type\`. This filter is required by default. To make it optional set \`assetManagerProps.optionalType = true\`. Set to an empty array to hide the asset type filter.<br>**Default**<br>`codeBlockLines_AdAo<br>[{ id: AssetType.image, label: 'Images'}]<br>` |

## PanelTemplates [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#paneltemplates "Direct link to PanelTemplates")

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

#### PanelTemplates properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#paneltemplates-properties "Direct link to PanelTemplates properties")

Show properties

| Property  | Type           | Description                                                                                                                                                                                                                                                                                                                                                                                         |
| --------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type\*    | panelTemplates | Type of the layout component.                                                                                                                                                                                                                                                                                                                                                                       |
| as        | string         | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                                                                                                                                                                                                                                                                                  |
| className | string         | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                                                                                                                                                                                                                                                        |
| content   | object         | Extra props to customize this layout panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "itemsPerRow": 3<br>}<br>`                                                                                                                                                                                                                                                                             |
| header    |                | Header of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "label": "My label",<br>  "collapsible": false<br>}<br>`                                                                                                                                                                                                                                                                      |
| htmlAttrs | object         | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`                                                                                                                                                                                                                                                                   |
| id        | string         | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                                                                                                                                                                                                                                                             |
| onSelect  | function       | Provide a custom handler for the select button.<br>**Example**<br>`codeBlockLines_AdAo<br>({ loadTemplate, template }) => {<br>  // loads the selected template to the current project<br>  loadTemplate(template);<br>}<br>`                                                                                                                                                                       |
| resizable |                | Resizable configuration of the panel.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "width": 100,<br>  "height": 300<br>}<br>`                                                                                                                                                                                                                                                                   |
| style     | object         | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                                                                                                                                                                                                                                                             |
| templates | array          | Custom array of templates to show in this panel.<br>**Example** <br>`codeBlockLines_AdAo<br>templates: [{<br>  id: 'template1',<br>  name: 'Template 1',<br>  data: {<br>    pages: [<br>      {<br>        name: 'Home',<br>        component: '<h1 class="title">Template 1</h1><style>.title { color: red; font-size: 10rem; text-align: center }</style>'<br>      }<br>    ]<br>  }<br>}]<br>` |

## Custom [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#custom "Direct link to Custom")

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

#### Custom properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#custom-properties "Direct link to Custom properties")

Show properties

| Property  | Type            | Description                                                                                                                                                                        |
| --------- | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type\*    | custom          | Type of the layout component.                                                                                                                                                      |
| className | string          | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                                       |
| component | React Component | Component to be rendered in the custom layout.<br>**Example**<br>`codeBlockLines_AdAo<br>({ editor }) => <button onClick={() => alert(editor.getHtml())}>Get HTML!!!</button><br>` |
| id        | string          | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                                            |
| noWrapper | boolean         | Indicates if the custom layout should not be wrapped (only works for React Components as the custom render needs a wrapper)<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`  |
| render    | function        | Function to render the custom layout.<br>**Example**<br>`codeBlockLines_AdAo<br>({ editor, addEl, removeEl }) => '<div>Custom layout</div>'<br>`                                   |
| style     | object          | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                                            |

## VirtualList [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#virtuallist "Direct link to VirtualList")

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

#### VirtualList properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#virtuallist-properties "Direct link to VirtualList properties")

Show properties

| Property    | Type        | Description                                                                                                                                                                                                           |
| ----------- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type\*      | virtualList | Type of the layout component.                                                                                                                                                                                         |
| items\*     | array       | The items to be displayed in the virtual list.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "id": "1",<br>    "name": "Item 1"<br>  },<br>  {<br>    "id": "2",<br>    "name": "Item 2"<br>  }<br>]<br>` |
| className   | string      | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                                                                          |
| id          | string      | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                                                                               |
| itemLayout  | function    | The layout component for the items in the virtual list.<br>**Example**<br>`codeBlockLines_AdAo<br>({ item, editor }) => ({ type: 'row', children: item.name })<br>`                                                   |
| itemsPerRow | number      | The number of items per row.<br>**Default** <br>`codeBlockLines_AdAo<br>12<br>`                                                                                                                                       |
| style       | object      | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                                                                               |

## Sidebar components [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#sidebar-components "Direct link to Sidebar components")

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

#### SidebarBottom properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#sidebarbottom-properties "Direct link to SidebarBottom properties")

Show properties

| Property  | Type             | Description                                                                                                                                                  |
| --------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| type\*    | sidebarBottom    | Type of the layout component.                                                                                                                                |
| as        | string           | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                                           |
| children  | Layout component | The children layout components.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>` |
| className | string           | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                 |
| htmlAttrs | object           | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`                            |
| id        | string           | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                      |
| style     | object           | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                      |

#### SidebarLeft properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#sidebarleft-properties "Direct link to SidebarLeft properties")

Show properties

| Property  | Type             | Description                                                                                                                                                  |
| --------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| type\*    | sidebarLeft      | Type of the layout component.                                                                                                                                |
| as        | string           | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                                           |
| children  | Layout component | The children layout components.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>` |
| className | string           | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                 |
| id        | string           | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                      |
| resizable | boolean          | Indicates whether the sidebar is resizable.<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>`                                                             |
| style     | object           | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                      |

#### SidebarRight properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#sidebarright-properties "Direct link to SidebarRight properties")

Show properties

| Property  | Type             | Description                                                                                                                                                  |
| --------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| type\*    | sidebarRight     | Type of the layout component.                                                                                                                                |
| as        | string           | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                                           |
| children  | Layout component | The children layout components.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>` |
| className | string           | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                 |
| id        | string           | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                      |
| resizable | boolean          | Indicates whether the sidebar is resizable.<br>**Default** <br>`codeBlockLines_AdAo<br>true<br>`                                                             |
| style     | object           | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                      |

#### SidebarTop properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#sidebartop-properties "Direct link to SidebarTop properties")

Show properties

| Property       | Type                                                                                                 | Description                                                                                                                                                                                                                                     |
| -------------- | ---------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type\*         | sidebarTop                                                                                           | Type of the layout component.                                                                                                                                                                                                                   |
| as             | string                                                                                               | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                                                                                                                              |
| children       | Layout component                                                                                     | The children layout components.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "type": "text",<br>    "content": "Hello, World!"<br>  }<br>]<br>`                                                                                    |
| className      | string                                                                                               | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                                                                                                    |
| devices        | [DevicesProps](https://app.grapesjs.com/docs-sdk/configuration/layout/components#devices-properties) | The properties for the devices section of the top bar.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "style": {<br>    "width": 200<br>  }<br>}<br>`                                                                                         |
| height         | number, string                                                                                       | The height of the top bar.<br>**Example** <br>`codeBlockLines_AdAo<br>50<br>`                                                                                                                                                                   |
| id             | string                                                                                               | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                                                                                                         |
| leftContainer  | object                                                                                               | The properties for the left container of the top bar.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "buttons": [<br>    {<br>      "type": "button",<br>      "icon": "menu",<br>      "onClick": "toggleSidebar"<br>    }<br>  ]<br>}<br>`  |
| rightContainer | object                                                                                               | The properties for the right container of the top bar.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "buttons": [<br>    {<br>      "type": "button",<br>      "icon": "menu",<br>      "onClick": "toggleSidebar"<br>    }<br>  ]<br>}<br>` |
| style          | object                                                                                               | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                                                                                                         |

## Canvas components [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#canvas-components "Direct link to Canvas components")

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

#### Canvas properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#canvas-properties "Direct link to Canvas properties")

Show properties

| Property  | Type    | Description                                                                                                                             |
| --------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| type\*    | canvas  | Type of the layout component.                                                                                                           |
| className | string  | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                            |
| grow      | boolean | If true, the component will grow to fill available space.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                         |
| id        | string  | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                 |
| style     | object  | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>` |

#### CanvasSidebarTop properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#canvassidebartop-properties "Direct link to CanvasSidebarTop properties")

Show properties

| Property   | Type                                                                                                       | Description                                                                                                                             |
| ---------- | ---------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| type\*     | canvasSidebarTop                                                                                           | Type of the layout component.                                                                                                           |
| as         | string                                                                                                     | The HTML tag to use for the layout component.<br>**Example**<br>`codeBlockLines_AdAo<br>"div"<br>`                                      |
| className  | string                                                                                                     | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                            |
| htmlAttrs  | object                                                                                                     | The HTML attributes for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "data-test-id": "component-123"<br>}<br>`       |
| id         | string                                                                                                     | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                 |
| sidebarTop | [SidebarTopProps](https://app.grapesjs.com/docs-sdk/configuration/layout/components#sidebartop-properties) | The sidebar top.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "style": {<br>    "alignItems": "center"<br>  }<br>}<br>`             |
| style      | object                                                                                                     | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>` |

## Form components [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#form-components "Direct link to Form components")

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

#### InputField properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#inputfield-properties "Direct link to InputField properties")

Show properties

| Property     | Type       | Description                                                                                                                                                                                                                    |
| ------------ | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- |
| type\*       | inputField | Type of the layout component.                                                                                                                                                                                                  |
| value\*      | string     | The value of the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"username"<br>`                                                                                                                                              |
| className    | string     | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                                                                                   |
| disabled     | boolean    | Indicates whether the field is disabled.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                                                 |
| editorEvents | object     | Update layout component state based on editor events.<br>**Example**<br>`codeBlockLines_AdAo<br>{ 'component:selected': ({ setState, editor }) => setState({ className: 'custom-cls-' + editor.getSelected().getId() }) }<br>` |
| id           | string     | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                                                                                        |
| inputType    | string     | The type of the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"text"<br>`                                                                                                                                                   |
| label        | string     | The label for the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"Username"<br>`                                                                                                                                             |
| name         | string     | The name attribute for the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"username"<br>`                                                                                                                                    |
| onChange     | function   | The function to call when the field value changes.<br>**Example**<br>`codeBlockLines_AdAo<br>({ value, setState }) => setState({ value });<br>`                                                                                |
| onInput      | function   | The function to call when the field value changes on input.<br>**Example**<br>`codeBlockLines_AdAo<br>({ value, setState }) => setState({ value });<br>`                                                                       |
| placeholder  | string     | The placeholder text for the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"Enter your username"<br>`                                                                                                                       |
| readOnly     | boolean    | Indicates whether the field is read-only.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                                                |
| required     | boolean    | Indicates whether the field is required.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                                                 |
| row          | boolean    | Indicates if the field should be rendered in a row layout.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                               |
| size         | string     | The size of the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"m"                                                                                                                                                         | "s"<br>``` |
| style        | object     | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                                                                                        |

#### SelectField properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#selectfield-properties "Direct link to SelectField properties")

Show properties

| Property     | Type        | Description                                                                                                                                                                                                                                                                 |
| ------------ | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- | --- | ---- | ------------ |
| type\*       | selectField | Type of the layout component.                                                                                                                                                                                                                                               |
| options\*    | array       | The options for the field.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "id": "1",<br>    "label": "Username 1",<br>    "content": "Username 1"<br>  },<br>  {<br>    "id": "2",<br>    "label": "Username 2",<br>    "content": "Username 2"<br>  }<br>]<br>` |
| value\*      | string      | The value of the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"username"<br>`                                                                                                                                                                                           |
| className    | string      | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                                                                                                                                |
| disabled     | boolean     | Indicates whether the field is disabled.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                                                                                              |
| editorEvents | object      | Update layout component state based on editor events.<br>**Example**<br>`codeBlockLines_AdAo<br>{ 'component:selected': ({ setState, editor }) => setState({ className: 'custom-cls-' + editor.getSelected().getId() }) }<br>`                                              |
| emptyState   | string      | The empty state for the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"Select your username"<br>`                                                                                                                                                                        |
| id           | string      | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                                                                                                                                     |
| label        | string      | The label for the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"Username"<br>`                                                                                                                                                                                          |
| name         | string      | The name attribute for the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"username"<br>`                                                                                                                                                                                 |
| onChange     | function    | The function to call when the field value changes.<br>**Example**<br>`codeBlockLines_AdAo<br>({ value, setState }) => setState({ value });<br>`                                                                                                                             |
| required     | boolean     | Indicates whether the field is required.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                                                                                              |
| size         | string      | The size of the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"l"                                                                                                                                                                                                      | "m" | "s" | "xs" | "x2s"<br>``` |
| style        | object      | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                                                                                                                                     |

#### ButtonGroupField properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#buttongroupfield-properties "Direct link to ButtonGroupField properties")

Show properties

| Property     | Type             | Description                                                                                                                                                                                                                                                                                                                               |
| ------------ | ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- | ----------- |
| type\*       | buttonGroupField | Type of the layout component.                                                                                                                                                                                                                                                                                                             |
| options\*    | array            | The options for the field.<br>**Example**<br>`codeBlockLines_AdAo<br>[<br>  {<br>    "id": "1",<br>    "label": "Username 1",<br>    "title": "Username 1",<br>    "icon": "<svg>...</svg>"<br>  },<br>  {<br>    "id": "2",<br>    "label": "Username 2",<br>    "title": "Username 2",<br>    "icon": "<svg>...</svg>"<br>  }<br>]<br>` |
| value\*      | string           | The value of the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"username"<br>`                                                                                                                                                                                                                                                         |
| className    | string           | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                                                                                                                                                                                              |
| disabled     | boolean          | Indicates whether the field is disabled.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                                                                                                                                                            |
| editorEvents | object           | Update layout component state based on editor events.<br>**Example**<br>`codeBlockLines_AdAo<br>{ 'component:selected': ({ setState, editor }) => setState({ className: 'custom-cls-' + editor.getSelected().getId() }) }<br>`                                                                                                            |
| id           | string           | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                                                                                                                                                                                                   |
| label        | string           | The label for the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"Username"<br>`                                                                                                                                                                                                                                                        |
| name         | string           | The name attribute for the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"username"<br>`                                                                                                                                                                                                                                               |
| onChange     | function         | The function to call when the field value changes.<br>**Example**<br>`codeBlockLines_AdAo<br>({ value, setState }) => setState({ value });<br>`                                                                                                                                                                                           |
| required     | boolean          | Indicates whether the field is required.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                                                                                                                                                            |
| size         | string           | The size of the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"m"                                                                                                                                                                                                                                                                    | "s" | "xs"<br>``` |
| style        | object           | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                                                                                                                                                                                                   |

#### CodeField properties [​](https://app.grapesjs.com/docs-sdk/configuration/layout/components#codefield-properties "Direct link to CodeField properties")

Show properties

| Property      | Type      | Description                                                                                                                                                                                                                    |
| ------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- |
| type\*        | codeField | Type of the layout component.                                                                                                                                                                                                  |
| language\*    | string    | Indicates the language of the code field.<br>**Example**<br>`codeBlockLines_AdAo<br>"json"<br>`                                                                                                                                |
| value\*       | string    | The value of the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"username"<br>`                                                                                                                                              |
| className     | string    | The CSS class name(s) for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"my-component-class"<br>`                                                                                                                   |
| disabled      | boolean   | Indicates whether the field is disabled.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                                                 |
| editorEvents  | object    | Update layout component state based on editor events.<br>**Example**<br>`codeBlockLines_AdAo<br>{ 'component:selected': ({ setState, editor }) => setState({ className: 'custom-cls-' + editor.getSelected().getId() }) }<br>` |
| id            | string    | The unique identifier for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>"component-123"<br>`                                                                                                                        |
| label         | string    | The label for the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"Username"<br>`                                                                                                                                             |
| minHeight     | string    | Indicates the minimum height of the field.<br>**Default** <br>`codeBlockLines_AdAo<br>170px<br>`                                                                                                                               |
| monacoOptions | object    | Pass additional options to the Monaco editor. Docs: https://microsoft.github.io/monaco-editor/typedoc/interfaces/editor.IStandaloneEditorConstructionOptions.html<br>**Default**<br>`codeBlockLines_AdAo<br>{}<br>`            |
| name          | string    | The name attribute for the field.<br>**Example**<br>`codeBlockLines_AdAo<br>"username"<br>`                                                                                                                                    |
| onChange      | function  | The function to call when the field value changes.<br>**Example**<br>`codeBlockLines_AdAo<br>({ value, setState }) => setState({ value });<br>`                                                                                |
| readOnly      | boolean   | Indicates whether the field is read-only.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                                                |
| required      | boolean   | Indicates whether the field is required.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                                                 |
| row           | boolean   | Indicates if the field should be rendered in a row layout.<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                                               |
| size          | string    | The size of the field.<br>**Example**<br>```codeBlockLines_AdAo<br>"m"                                                                                                                                                         | "s"<br>``` |
| style         | object    | The inline styles for the component.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "color": "red",<br>  "fontSize": "14px"<br>}<br>`                                                                                        |

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

---

# File: app.grapesjs.com_docs-sdk_configuration_layout_overview.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/layout/overview"
title: "Layout | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#__docusaurus_skipToContent_fallback)

On this page

Studio SDK provides a powerful and flexible layout system specifically for the editor UI, enabling full customization of editor components and panel arrangements to suit your specific needs.

![Layout example 3 columns](https://app.grapesjs.com/docs-sdk/assets/images/layout-example-3-columns-c1ddab2710d988c96a9c8a39b7034663.png)

![Layout example sidebar](https://app.grapesjs.com/docs-sdk/assets/images/layout-example-sidebar-4491f363311daf2e56a882929d378f0f.png)

![Layout example sidebar inversed](https://app.grapesjs.com/docs-sdk/assets/images/layout-example-sidebar-inv-799ec59a79fef3a7bc03ccf18786140e.png)

![Layout example rows](https://app.grapesjs.com/docs-sdk/assets/images/layout-example-rows-f0894e895d5ae7001646e065d7b291e1.png)

## Table of Contents

- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#initialization)
- [Layout Components](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#layout-components)
- [Responsive Layout](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#responsive-layout)
- [Layout Commands](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#layout-commands)

## Initialization [​](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#initialization "Direct link to Initialization")

Studio initializes with a preconfigured layout, allowing you to start using the editor right out of the box.

Below is the configuration used by the default layout via `layout.default` option.

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
          { type: 'sidebarLeft' },\
          { type: 'canvasSidebarTop' },\
          { type: 'sidebarRight' }\
        ]
      },
    }
  }}
/>

```

The layout is built using Studio component configurations, each defined by a `type` along with other properties.

Studio includes a set of predefined [Layout Components](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#layout-components), which you can use to compose a customized editor interface.

### Required conditions [​](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#required-conditions "Direct link to Required conditions")

When configuring the layout, keep these two requirements in mind:

1. The root component of `layout.default` must be a [`row`](https://app.grapesjs.com/docs-sdk/configuration/layout/components#row) or [`column`](https://app.grapesjs.com/docs-sdk/configuration/layout/components#column) type.

2. The `layout.default` must include inside one of the [Canvas components](https://app.grapesjs.com/docs-sdk/configuration/layout/components#canvas-components).

## Layout Components [​](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#layout-components "Direct link to Layout Components")

Check the [Layout Components](https://app.grapesjs.com/docs-sdk/configuration/layout/components) page to explore all the components available for building your editor interface.

## Responsive Layout [​](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#responsive-layout "Direct link to Responsive Layout")

Studio supports also responsive configuration, enabling the layout to adjust automatically based on the width of the editor container.

Just like with `layout.default`, each `responsive` layout configuration must adhere to the same [required conditions](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#required-conditions).

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
          { type: 'sidebarLeft' },\
          { type: 'canvasSidebarTop' },\
          { type: 'sidebarRight' }\
        ]
      },
      responsive: {
        // Studio will switch the layout when the editor container width is below 1000px.
        1000: {
          type: 'row',
          style: { height: '100%' },
          children: [{ type: 'sidebarLeft' }, { type: 'canvas' }]
        },
        600: {
          type: 'column',
          style: { height: '100%' },
          children: [{ type: 'canvas' }, { type: 'row', children: 'Text' }]
        }
      }
    }
  }}
/>

```

## Layout Commands [​](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#layout-commands "Direct link to Layout Commands")

Studio provides a set of commands for managing the layout of the editor interface.

### Dynamic layouts [​](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#dynamic-layouts "Direct link to Dynamic layouts")

You can manage dynamic layouts with `studio:layoutAdd`, `studio:layoutRemove`, and `studio:layoutToggle` commands. Various `placer` types are available to position your components precisely.

#### Absolute placer [​](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#absolute-placer "Direct link to Absolute placer")

Use `absolute` placer allows you to position the layout at a specific absolute location within the editor interface.

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
            style: { padding: 5, gap: 5, borderRightWidth: 1, zIndex: 20, alignItems: 'center' },\
            children: [\
              {\
                type: 'button',\
                icon: 'layers',\
                editorEvents: {\
                  'studio:layoutToggle:layoutId1': ({ fromEvent, setState }) => setState({ active: fromEvent.isOpen })\
                },\
                onClick: ({ editor }) => {\
                  editor.runCommand('studio:layoutRemove', { id: 'layoutId2' });\
                  editor.runCommand('studio:layoutToggle', {\
                    id: 'layoutId1',\
                    layout: { type: 'panelPagesLayers' },\
                    header: { label: 'Layers' },\
                    placer: { type: 'absolute', position: 'left' },\
                    style: { marginLeft: 42 }\
                  });\
                }\
              },\
              {\
                type: 'button',\
                icon: 'viewGridPlus',\
                editorEvents: {\
                  'studio:layoutToggle:layoutId2': ({ fromEvent, setState }) => setState({ active: fromEvent.isOpen })\
                },\
                onClick: ({ editor }) => {\
                  editor.runCommand('studio:layoutRemove', { id: 'layoutId1' });\
                  editor.runCommand('studio:layoutToggle', {\
                    id: 'layoutId2',\
                    layout: { type: 'panelBlocks' },\
                    header: { label: 'Blocks' },\
                    placer: { type: 'absolute', position: 'right' },\
                  });\
                }\
              }\
            ]\
          },\
          { type: 'canvas', grow: true }\
        ]
      },
    }
  }}
/>

```

#### Static placer [​](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#static-placer "Direct link to Static placer")

Use `static` placer to position your components according to a specifically defined layout in your configuration.

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
            style: { padding: 5, gap: 5, borderRightWidth: 1, alignItems: 'center' },\
            children: [\
              {\
                type: 'button',\
                tooltip: 'Layers',\
                icon: 'layers',\
                editorEvents: {\
                  'studio:layoutToggle:layoutId1': ({ fromEvent, setState }) => setState({ active: fromEvent.isOpen })\
                },\
                onClick: ({ editor }) => {\
                  editor.runCommand('studio:layoutRemove', { id: 'layoutId2' });\
                  editor.runCommand('studio:layoutToggle', {\
                    id: 'layoutId1',\
                    layout: { type: 'panelPagesLayers' },\
                    header: { label: 'Layers' },\
                    placer: { type: 'static', layoutId: 'hiddenLeftContainer' },\
                    style: { width: 300, overflow: 'hidden' }\
                  });\
                }\
              },\
              {\
                type: 'button',\
                tooltip: 'Blocks',\
                icon: 'viewGridPlus',\
                editorEvents: {\
                  'studio:layoutToggle:layoutId2': ({ fromEvent, setState }) => setState({ active: fromEvent.isOpen })\
                },\
                onClick: ({ editor }) => {\
                  editor.runCommand('studio:layoutRemove', { id: 'layoutId1' });\
                  editor.runCommand('studio:layoutToggle', {\
                    id: 'layoutId2',\
                    layout: { type: 'panelBlocks' },\
                    header: { label: 'Blocks' },\
                    placer: { type: 'static', layoutId: 'hiddenRightContainer' },\
                    style: { width: 300, overflow: 'hidden' }\
                  });\
                }\
              }\
            ]\
          },\
          { id: 'hiddenLeftContainer', type: 'column' },\
          { type: 'canvas', grow: true },\
          { id: 'hiddenRightContainer', type: 'column' }\
        ]
      }
    }
  }}
/>

```

#### Popover placer [​](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#popover-placer "Direct link to Popover placer")

The `popover` placer allows you to position your components inside a dynamically positioned popover.

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
            style: { padding: 5, borderRightWidth: 1, alignItems: 'center', justifyContent: 'space-between' },\
            children: [\
              {\
                type: 'button',\
                tooltip: 'Layers',\
                icon: 'layers',\
                editorEvents: {\
                  'studio:layoutToggle:layoutId1': ({ fromEvent, setState }) => setState({ active: fromEvent.isOpen })\
                },\
                onClick: ({ editor, event }) => {\
                  const rect = event.currentTarget.getBoundingClientRect();\
                  editor.runCommand('studio:layoutRemove', { id: 'layoutId2' });\
                  editor.runCommand('studio:layoutToggle', {\
                    id: 'layoutId1',\
                    layout: { type: 'panelLayers' },\
                    header: { label: 'Layers' },\
                    placer: { type: 'popover', x: rect.x + rect.width, y: rect.y },\
                    style: { height: 300, width: 230 }\
                  });\
                }\
              },\
              {\
                type: 'button',\
                tooltip: 'Blocks',\
                icon: 'viewGridPlus',\
                editorEvents: {\
                  'studio:layoutToggle:layoutId2': ({ fromEvent, setState }) => setState({ active: fromEvent.isOpen }),\
                  'block:drag:stop': ({ fromEvent, editor }) => {\
                    fromEvent && editor.runCommand('studio:layoutRemove', { id: 'layoutId2' });\
                  }\
                },\
                onClick: ({ editor, event }) => {\
                  const rect = event.currentTarget.getBoundingClientRect();\
                  editor.runCommand('studio:layoutRemove', { id: 'layoutId1' });\
                  editor.runCommand('studio:layoutToggle', {\
                    id: 'layoutId2',\
                    layout: { type: 'panelBlocks', symbols: false },\
                    header: { label: 'Blocks' },\
                    placer: { type: 'popover', closeOnClickAway: true, x: rect.x + rect.width, y: rect.y },\
                    style: { height: 300, width: 230 }\
                  });\
                }\
              }\
            ]\
          },\
          { type: 'canvas', grow: true }\
        ]
      },
    }
  }}
/>

```

#### Dialog placer [​](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#dialog-placer "Direct link to Dialog placer")

The `dialog` placer enables you to position your layout within a dialog component.

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
            style: { padding: 5, borderRightWidth: 1, alignItems: 'center' },\
            children: [\
              {\
                type: 'button',\
                tooltip: 'Layers',\
                icon: 'layers',\
                onClick: ({ editor }) => {\
                  editor.runCommand('studio:layoutToggle', {\
                    id: 'layoutId1',\
                    header: false,\
                    layout: { type: 'panelLayers' },\
                    placer: { type: 'dialog', title: 'Layers' },\
                    style: { height: 300 }\
                  });\
                }\
              },\
              {\
                type: 'button',\
                tooltip: 'Blocks',\
                icon: 'viewGridPlus',\
                onClick: ({ editor }) => {\
                  editor.runCommand('studio:layoutToggle', {\
                    id: 'layoutId2',\
                    header: false,\
                    layout: { type: 'panelBlocks', symbols: false },\
                    placer: { type: 'dialog', title: 'Blocks', size: 'l' },\
                    style: { height: 300 }\
                  });\
                }\
              }\
            ]\
          },\
          { type: 'canvas', grow: true }\
        ]
      },
    }
  }}
/>

```

- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#initialization)
  - [Required conditions](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#required-conditions)
- [Layout Components](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#layout-components)
- [Responsive Layout](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#responsive-layout)
- [Layout Commands](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#layout-commands)
  - [Dynamic layouts](https://app.grapesjs.com/docs-sdk/configuration/layout/overview#dynamic-layouts)

---

# File: app.grapesjs.com_docs-sdk_configuration_overview.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/overview"
title: "Configuration Overview | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/overview#__docusaurus_skipToContent_fallback)

On this page

### Projects [​](https://app.grapesjs.com/docs-sdk/configuration/overview#projects "Direct link to Projects")

Learn how to configure the Studio SDK and integrate it into your application. This includes detailed instructions on setting up various types of projects, how to load and store them, ensuring that you can leverage the full capabilities of the Studio SDK. For more information on setting up different types of projects, please refer to the relevant [page](https://app.grapesjs.com/docs-sdk/configuration/projects).

![Studio App HTML Website Builder](https://app.grapesjs.com/docs-sdk/assets/images/studio-app-9bf0c6393e2479e2cdbffc9d3acc9430.png)

![Studio App HTML Email Newsletter Builder](https://app.grapesjs.com/docs-sdk/assets/images/project-email-a881e73ba2c6c07b42c1826fe984c273.webp)

### Blocks [​](https://app.grapesjs.com/docs-sdk/configuration/overview#blocks "Direct link to Blocks")

The editor includes a Block module that allows you to define reusable content elements for your users in the project. You can customize blocks to include individual components or complex layouts. For detailed instructions on setting up and managing blocks, please refer to the relevant [page](https://app.grapesjs.com/docs-sdk/configuration/blocks).

![Blocks Panel](https://app.grapesjs.com/docs-sdk/assets/images/blocks-panel-c22874feb018b37e959b7a68f6aeb39d.webp)

### Pages [​](https://app.grapesjs.com/docs-sdk/configuration/overview#pages "Direct link to Pages")

The Studio SDK supports multipage functionality, allowing for customization in managing pages within your project. For detailed instructions on setting up and managing pages, please refer to the relevant [page](https://app.grapesjs.com/docs-sdk/configuration/pages).

![Default pages](Base64-Image-Removed)

### Assets [​](https://app.grapesjs.com/docs-sdk/configuration/overview#assets "Direct link to Assets")

Learn how to manage media and resources in your project, including uploading files and configuring storage settings. You can also integrate external services using Asset Providers, allowing you to load custom asset types such as images, videos, and documents. For more details on asset management, see the [Assets Configuration page](https://app.grapesjs.com/docs-sdk/configuration/assets/overview).

![Asset Manager](https://app.grapesjs.com/docs-sdk/assets/images/asset-manager-fbf02ebd21a56c312bdeed1cc8c433f2.png)

![Asset Providers](https://app.grapesjs.com/docs-sdk/assets/images/asset-providers-e9da291f031980da5554eae6f3fefc34.png)

### Themes [​](https://app.grapesjs.com/docs-sdk/configuration/overview#themes "Direct link to Themes")

Learn how to personalize the visual appearance of your editor, including customizing icons, choosing themes, and modifying styles to align with your brand's identity. For more details on theme customization, please refer to the relevant [page](https://app.grapesjs.com/docs-sdk/configuration/themes).

![Default light theme](https://app.grapesjs.com/docs-sdk/assets/images/example-light-default-15f56c00115e4ea887a64b5ad6440b8d.jpeg)

![Default dark theme](https://app.grapesjs.com/docs-sdk/assets/images/example-dark-default-eb15c7af877fdc85baff8c02e999cbad.jpeg)

![Custom dark theme](https://app.grapesjs.com/docs-sdk/assets/images/example-dark-exaggerated-0eafda5210e222b9a3cddfa9342497ef.jpeg)

![Custom light theme](https://app.grapesjs.com/docs-sdk/assets/images/example-light-exaggerated-ced25e08a8ed8333517dd0e16b134622.jpeg)

### Layout [​](https://app.grapesjs.com/docs-sdk/configuration/overview#layout "Direct link to Layout")

Discover how to adjust the layout interface of your editor, customizing panel positioning, dynamic layouts, responsive configurations and other structural elements to create an editor layout that best fits your users' workflow. For detailed instructions, see the [Layout Configuration page](https://app.grapesjs.com/docs-sdk/configuration/layout/overview).

![Layout example 3 columns](https://app.grapesjs.com/docs-sdk/assets/images/layout-example-3-columns-c1ddab2710d988c96a9c8a39b7034663.png)

![Layout example sidebar](https://app.grapesjs.com/docs-sdk/assets/images/layout-example-sidebar-4491f363311daf2e56a882929d378f0f.png)

![Layout example sidebar inversed](https://app.grapesjs.com/docs-sdk/assets/images/layout-example-sidebar-inv-799ec59a79fef3a7bc03ccf18786140e.png)

![Layout example rows](https://app.grapesjs.com/docs-sdk/assets/images/layout-example-rows-f0894e895d5ae7001646e065d7b291e1.png)

### Global Styles [​](https://app.grapesjs.com/docs-sdk/configuration/overview#global-styles "Direct link to Global Styles")

Define and manage styles that apply across your entire project, creating a cohesive and easily maintainable design system. Configure common styles, leverage CSS variables, and integrate external libraries to ensure consistent theming and styling for all components. For detailed instructions, see the [Global Styles Configuration page](https://app.grapesjs.com/docs-sdk/configuration/global-styles).

![Global Styles](https://app.grapesjs.com/docs-sdk/assets/images/global-styles-508f8a5bbae3a4210aa0244a586309ba.png)

### Templates [​](https://app.grapesjs.com/docs-sdk/configuration/overview#templates "Direct link to Templates")

Enable your users to select and start with pre-configured designs using our Template manager. For detailed instructions, see the [Template Configuration page](https://app.grapesjs.com/docs-sdk/configuration/templates).

![Templates](https://app.grapesjs.com/docs-sdk/assets/images/templates-c15932661a1b99ef524c8b6cedb492c4.png)

### Data Sources [​](https://app.grapesjs.com/docs-sdk/configuration/overview#data-sources "Direct link to Data Sources")

Connect your content to structured data and build dynamic, data-driven content with ease. Use variables, conditions, and repeatable collections to personalize content and automate rendering logic. Export fully resolved content or template-ready structures using merge tags. For implementation details, see the [Data Sources Configuration page](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview).

![Data Sources](https://app.grapesjs.com/docs-sdk/assets/images/data-sources-9868deda649304e1241431fd8c5052a0.webp)

### Fonts [​](https://app.grapesjs.com/docs-sdk/configuration/overview#fonts "Direct link to Fonts")

Customize the typography of your projects by managing fonts directly within the editor. Add custom fonts through project configuration, modify the default font list, and integrate external providers like Google Fonts. Enable a flexible and extensible font manager to give users full control over font selection. For implementation details, see the [Fonts Configuration page](https://app.grapesjs.com/docs-sdk/configuration/fonts).

![Font Select](Base64-Image-Removed)

![Font Manager](https://app.grapesjs.com/docs-sdk/assets/images/font-asset-manager-f88ba87b42ef88e6de775f6843988a6b.webp)

- [Projects](https://app.grapesjs.com/docs-sdk/configuration/overview#projects)
- [Blocks](https://app.grapesjs.com/docs-sdk/configuration/overview#blocks)
- [Pages](https://app.grapesjs.com/docs-sdk/configuration/overview#pages)
- [Assets](https://app.grapesjs.com/docs-sdk/configuration/overview#assets)
- [Themes](https://app.grapesjs.com/docs-sdk/configuration/overview#themes)
- [Layout](https://app.grapesjs.com/docs-sdk/configuration/overview#layout)
- [Global Styles](https://app.grapesjs.com/docs-sdk/configuration/overview#global-styles)
- [Templates](https://app.grapesjs.com/docs-sdk/configuration/overview#templates)
- [Data Sources](https://app.grapesjs.com/docs-sdk/configuration/overview#data-sources)
- [Fonts](https://app.grapesjs.com/docs-sdk/configuration/overview#fonts)

---

# File: app.grapesjs.com_docs-sdk_configuration_pages.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/pages"
title: "Pages | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/pages#__docusaurus_skipToContent_fallback)

On this page

Pages allow you to create projects with multiple pages, making it ideal for various types of `web` projects. In Studio SDK, the Pages Manager is a direct extension of the [GrapesJS Pages module](https://grapesjs.com/docs/modules/Pages.html), meaning you can always reuse the existing [Pages API](https://grapesjs.com/docs/api/pages.html) for flexibility and consistency.

![Page Manager](Base64-Image-Removed)

## Table of Contents

- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/pages#initialization)
- [Configuration](https://app.grapesjs.com/docs-sdk/configuration/pages#configuration)
- [I18n](https://app.grapesjs.com/docs-sdk/configuration/pages#i18n)
- [Commands](https://app.grapesjs.com/docs-sdk/configuration/pages#commands)

## Initialization [​](https://app.grapesjs.com/docs-sdk/configuration/pages#initialization "Direct link to Initialization")

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

![Default pages](Base64-Image-Removed)

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

## Configuration [​](https://app.grapesjs.com/docs-sdk/configuration/pages#configuration "Direct link to Configuration")

By default, users can manage pages without restrictions, including creating, duplicating, or deleting them. With Studio SDK, you can easily customize each of these operations to suit your needs.

### Add Page [​](https://app.grapesjs.com/docs-sdk/configuration/pages#add-page "Direct link to Add Page")

![Add default pages](Base64-Image-Removed)

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

### Duplicate Page [​](https://app.grapesjs.com/docs-sdk/configuration/pages#duplicate-page "Direct link to Duplicate Page")

![Duplicate pages](Base64-Image-Removed)

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

### Remove Page [​](https://app.grapesjs.com/docs-sdk/configuration/pages#remove-page "Direct link to Remove Page")

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

### Command Items [​](https://app.grapesjs.com/docs-sdk/configuration/pages#command-items "Direct link to Command Items")

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

![Custom page command](Base64-Image-Removed)

### Settings [​](https://app.grapesjs.com/docs-sdk/configuration/pages#settings "Direct link to Settings")

![Custom page command](Base64-Image-Removed)

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

#### Load page settings [​](https://app.grapesjs.com/docs-sdk/configuration/pages#load-page-settings "Direct link to Load page settings")

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

## I18n [​](https://app.grapesjs.com/docs-sdk/configuration/pages#i18n "Direct link to I18n")

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

## Commands [​](https://app.grapesjs.com/docs-sdk/configuration/pages#commands "Direct link to Commands")

Here below you can find a list of commands that allow you to interact programmatically with the Page Manager.

### Get config [​](https://app.grapesjs.com/docs-sdk/configuration/pages#get-config "Direct link to Get config")

Get current Pages config.

```codeBlockLines_AdAo
const config = editor.runCommand(StudioCommands.getPagesConfig);

```

### Update config [​](https://app.grapesjs.com/docs-sdk/configuration/pages#update-config "Direct link to Update config")

Update Pages config.

```codeBlockLines_AdAo
editor.runCommand(StudioCommands.setPagesConfig, {
  config: { add: false }
});

```

### Get page settings [​](https://app.grapesjs.com/docs-sdk/configuration/pages#get-page-settings "Direct link to Get page settings")

Get page settings panel state.

```codeBlockLines_AdAo
const state: PanelPageSettingsState = editor.runCommand(StudioCommands.getPageSettings);

```

### Update page settings [​](https://app.grapesjs.com/docs-sdk/configuration/pages#update-page-settings "Direct link to Update page settings")

Update page settings panel state.

```codeBlockLines_AdAo
editor.runCommand(StudioCommands.setPageSettings, { isOpen: true });

```

### Project files [​](https://app.grapesjs.com/docs-sdk/configuration/pages#project-files "Direct link to Project files")

Get project files.

```codeBlockLines_AdAo
const files: ProjectFile[] = await editor.runCommand(StudioCommands.projectFiles);

```

### Example [​](https://app.grapesjs.com/docs-sdk/configuration/pages#example "Direct link to Example")

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

---

# File: app.grapesjs.com_docs-sdk_configuration_projects.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/projects"
title: "Projects | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/projects#__docusaurus_skipToContent_fallback)

On this page

This section provides a comprehensive guide to setting up and managing your projects. This includes detailed instructions on creating new projects, configuring project storage, and utilizing various tools and features to enhance your workflow.

![Studio App HTML Website Builder](https://app.grapesjs.com/docs-sdk/assets/images/studio-app-9bf0c6393e2479e2cdbffc9d3acc9430.png)

![Studio App HTML Email Newsletter Builder](https://app.grapesjs.com/docs-sdk/assets/images/project-email-a881e73ba2c6c07b42c1826fe984c273.webp)

## Table of Contents

- [Setup](https://app.grapesjs.com/docs-sdk/configuration/projects#setup)
- [Storage](https://app.grapesjs.com/docs-sdk/configuration/projects#storage)
- [Export](https://app.grapesjs.com/docs-sdk/configuration/projects#export)

## Setup [​](https://app.grapesjs.com/docs-sdk/configuration/projects#setup "Direct link to Setup")

Configure the Studio SDK and use it in your project. For more information on setting up different types of projects, please visit this [page](https://app.grapesjs.com/docs-sdk/project-types/web).

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

## Storage [​](https://app.grapesjs.com/docs-sdk/configuration/projects#storage "Direct link to Storage")

In Studio SDK, you can configure how to store project JSON data. This is controlled by the `storage` option of the SDK.

warning

Always rely on the JSON project data to properly load your project in the editor.

While the editor can parse and use HTML/CSS code, allowing you to include it as part of your project initialization (see [Templates](https://app.grapesjs.com/docs-sdk/configuration/templates)), it should not be used as a persistence layer for loading projects.
Important information about components are stripped away when you export the code.

### Storage Types [​](https://app.grapesjs.com/docs-sdk/configuration/projects#storage-types "Direct link to Storage Types")

You have three options:

- We provide an option to save it in our [cloud storage](https://app.grapesjs.com/docs-sdk/configuration/projects#cloud-storage).
- You can store it in your own [self-hosted infra](https://app.grapesjs.com/docs-sdk/configuration/projects#self-hosted-storage).
- By default, your end-user's project data is saved locally in their browser.
  This option is ideal for development purposes. For production, please select one of the alternatives.

### Cloud Storage [​](https://app.grapesjs.com/docs-sdk/configuration/projects#cloud-storage "Direct link to Cloud Storage")

We provide an easy way to store project data for your users.
To use this, set `storage.type` to `cloud`, and add a unique id for each of your projects and end-users.

- React
- JS

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    licenseKey: "YOUR_LICENSE_KEY",
    storage: {
      type: "cloud"
    },
    project: {
      id: "UNIQUE_PROJECT_ID"
    },
    identity: {
      id: "UNIQUE_END_USER_ID"
    }
  }}
/>

```

These project and identity IDs are mandatory; however, avoid including sensitive data such as emails or phone numbers in these identifiers.

### Self Hosted Storage [​](https://app.grapesjs.com/docs-sdk/configuration/projects#self-hosted-storage "Direct link to Self Hosted Storage")

If you want to handle project data storage on your own, set `storage.type` to `self`.
Then, define the `storage.onSave` and `storage.onLoad` callbacks.

warning

The `storage.onSave` and `storage.onLoad` callbacks are mandatory when using `storage.type = 'self'`.
Specifying only `storage.onSave` will not work.
Alternatively, if the project JSON is already available at editor load (e.g., server-side rendering), you can provide the [project as a prop](https://app.grapesjs.com/docs-sdk/configuration/projects#load-project-from-prop).

In the example below, we're emulating self-hosted storage using the browser's session storage. Typically, you would send the data to your backend server and determine where and how to store the project JSON.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

const saveToSessionStorage = async (projectId, project) => {
  await waitAndFailRandomly('Testing when project save failed');
  sessionStorage.setItem(projectId, JSON.stringify(project));
}

const loadFromSessionStorage = async (projectId) => {
  await waitAndFailRandomly('Testing when project load failed');
  const projectString = sessionStorage.getItem(projectId);
  return projectString ? JSON.parse(projectString) : null;
}

const waitAndFailRandomly = async (str) => {
  await new Promise(res => setTimeout(res, 1000)); // fake delay
  if (Math.random() >= 0.8) throw new Error(str);
}

// ...
<StudioEditor
  options={{
    // ...
    storage: {
      type: 'self',
      autosaveChanges: 5, // save after every 5 changes

      onSave: async ({ project }) => {
        await saveToSessionStorage('DEMO_PROJECT_ID', project);
        console.log('Project saved', { project });
      },

      onLoad: async () => {
        const project = await loadFromSessionStorage('DEMO_PROJECT_ID');
        console.log('Project loaded', { project });

        // If the project doesn't exist (eg. first load), let's return a new one.
        return {
          project: project || {
            pages: [\
              { name: 'Home', component: '<h1>New project</h1>' },\
            ]
          }
        };
      },
    },
    project: {
      // Default acts here as a fallback project, in case the load fails.
      default: {
        pages: [\
          { name: 'Home', component: '<h1>Fallback Project, reload to retry</h1>' },\
        ]
      },
    }
  }}
/>

```

### Load Project From Prop [​](https://app.grapesjs.com/docs-sdk/configuration/projects#load-project-from-prop "Direct link to Load Project From Prop")

Instead of defining the `storage.onLoad` callback, you can set the `storage.project` option with the project JSON object.
This disables the `storage.onLoad` callback, and uses the assigned project data directly.

This approach is ideal for server-side rendered pages, where the project JSON is made available at load time. It eliminates the need for asynchronous loading, enabling the project to load instantly.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

const projectJSONFromServer = {
  pages: [\
    { name: 'Home', component: '<h1>Loaded Project</h1>' },\
  ]
};

// ...
<StudioEditor
  options={{
    // ...
    storage: {
      type: 'self',
      autosaveChanges: 5,
      project: projectJSONFromServer,
      onSave: async ({ project }) => console.log('Save project', { project }),
    }
  }}
/>

```

### Autosave [​](https://app.grapesjs.com/docs-sdk/configuration/projects#autosave "Direct link to Autosave")

To configure how saves are triggered, use `autosaveChanges` and `autosaveIntervalMs`.

- React
- JS

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// ...
<StudioEditor
  options={{
    // ...
    storage: {
      // save after every 5 changes
      autosaveChanges: 5,
      // save after every 10 seconds
      autosaveIntervalMs: 10000
    }
  }}
/>

```

## Export [​](https://app.grapesjs.com/docs-sdk/configuration/projects#export "Direct link to Export")

In the previous section, we covered how to store the JSON data essential for loading projects into the editor. Another key aspect of the editor is exporting projects into specific formats, such as an HTML page for websites, a newsletter's content, a PDF document, or an image file for designs.

The Studio SDK provides a default export mechanism in the form of "files." However, you can utilize the same project JSON data to implement custom export logic. With this, you can retrieve all pages, components, and their properties, allowing you to export projects in any format that suits your needs.

### Project Files [​](https://app.grapesjs.com/docs-sdk/configuration/projects#project-files "Direct link to Project Files")

The default `studio:projectFiles` command generates a list of project files, which by default represent an HTML document for each page in the project. Since HTML is a widely used format, you can easily use it as a foundation to convert your project into other formats, such as PDF, image files, or more.

#### Project files command options [​](https://app.grapesjs.com/docs-sdk/configuration/projects#project-files-command-options "Direct link to Project files command options")

Below the options available for the `studio:projectFiles` command.

Show options

| Property     | Type                                            | Description                                                                                                                                                                  |
| ------------ | ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| page         | [Page](https://grapesjs.com/docs/api/page.html) | Export specific page. By default, all pages are exported.                                                                                                                    |
| assetsFolder | string                                          | If the HTML containes base64 images, they will be exported as separate files and placed in the specified folder name.<br>**Default** <br>`codeBlockLines_AdAo<br>assets<br>` |
| filenameCss  | string                                          | Filename for CSS file.<br>**Default** <br>`codeBlockLines_AdAo<br>style.css<br>`                                                                                             |
| styles       | string                                          | Indicate how to export styles. By default, styles are exported in a separate CSS file.                                                                                       |
| skipProject  | boolean                                         | Skip project file (project JSON)<br>**Default** <br>`codeBlockLines_AdAo<br>false<br>`                                                                                       |
| exportConfig |                                                 | Configuration options for exporting data resolvers.<br>**Default** <br>`codeBlockLines_AdAo<br>undefined<br>`                                                                |
| optionsHtml  |                                                 |                                                                                                                                                                              |

#### Use Case: Publish Website [​](https://app.grapesjs.com/docs-sdk/configuration/projects#use-case-publish-website "Direct link to Use Case: Publish Website")

In the example below, we'll demonstrate a common use case: exporting and publishing a project as a website. The demo will present two simulated environments: one for staging, which updates on every save, and one for production, where manual publishing is required by clicking the Rocket icon.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

const PROJECT_ID = 'DEMO_PROJECT_ID_EXPORT';

const getWebsiteKey = (env = 'STAGE') => PROJECT_ID + '_WEBSITE_' + env;

const saveToSessionStorage = async (projectId, project) => {
  sessionStorage.setItem(projectId, JSON.stringify(project));
}

const loadFromSessionStorage = async (projectId) => {
  const projectString = sessionStorage.getItem(projectId);
  return projectString ? JSON.parse(projectString) : null;
}

const publishWebsite = async (editor, env) => {
  const files = await editor.runCommand('studio:projectFiles', { styles: 'inline' })
  // For simplicity, we'll "publish" only the first page.
  const firstPage = files.find(file => file.mimeType === 'text/html');
  const websiteData = {
    lastPublished: new Date().toLocaleString(),
    html: firstPage.content,
  };
  sessionStorage.setItem(getWebsiteKey(env), JSON.stringify(websiteData));
}

const viewPublishedWebsite = (editor, env) => {
  const websiteDataString = sessionStorage.getItem(getWebsiteKey(env));
  const websiteData = websiteDataString ? JSON.parse(websiteDataString) : null;
  const emptyStateText = 'Website not yet published! ' + (env === 'PROD' ? 'Click on the "rocket" icon to publish on PROD!' : 'Save a project to see it in STAGE');

  editor?.runCommand('studio:layoutToggle', {
    id: 'viewPublishedWebsite',
    header: false,
    placer: { type: 'dialog', size: 'l', title: 'Published website on ' + env },
    layout: {
      type: 'column',
      style: { minHeight: 600 },
      children: websiteData ? [\
        'Last time published: ' + websiteData.lastPublished,\
        {\
          type: 'row',\
          as: 'iframe',\
          srcDoc: websiteData.html,\
          style: { backgroundColor: 'white', height: 600 },\
        }\
      ] : emptyStateText,
    },
  });
}

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
            style: { padding: 5, gap: 10, borderBottomWidth: '1px', justifyContent: 'center' },\
            children: [\
              {\
                type: 'button',\
                variant: 'outline',\
                label: 'View Website Stage',\
                onClick: ({ editor }) => viewPublishedWebsite(editor, 'STAGE'),\
              },\
              {\
                type: 'button',\
                variant: 'primary',\
                label: 'View Website Prod',\
                onClick: ({ editor }) => viewPublishedWebsite(editor, 'PROD'),\
              },\
            ]\
          },\
          {\
            type: 'row',\
            style: { flexGrow: 1 },\
            children: [\
              {\
                type: 'canvasSidebarTop',\
                sidebarTop: {\
                  leftContainer: {\
                    buttons: ({ items }) => [\
                      ...items,\
                      {\
                        type: 'button',\
                        icon: '<svg viewBox="0 0 24 24"><path d="m13.13 22.19-1.63-3.83a21.05 21.05 0 0 0 4.4-2.27l-2.77 6.1M5.64 12.5l-3.83-1.63 6.1-2.77a21.05 21.05 0 0 0-2.27 4.4M21.61 2.39S16.66.27 11 5.93a19.82 19.82 0 0 0-4.35 6.71c-.28.75-.09 1.57.46 2.13l2.13 2.12c.55.56 1.37.74 2.12.46A19.1 19.1 0 0 0 18.07 13c5.66-5.66 3.54-10.61 3.54-10.61m-7.07 7.07a2 2 0 0 1 2.83-2.83 2 2 0 0 1-2.83 2.83m-5.66 7.07-1.41-1.41 1.41 1.41M6.24 22l3.64-3.64a3.06 3.06 0 0 1-.97-.45L4.83 22h1.41M2 22h1.41l4.77-4.76-1.42-1.41L2 20.59V22m0-2.83 4.09-4.08c-.21-.3-.36-.62-.45-.97L2 17.76v1.41Z"/></svg>',\
                        tooltip: 'Publish website ',\
                        onClick: ({ editor, event }) => {\
                          const layoutId =  'publishWebsiteProd';\
                          const rect = event.currentTarget.getBoundingClientRect();\
                          editor.runCommand('studio:layoutToggle', {\
                            id: layoutId,\
                            header: false,\
                            placer: {\
                              type: 'popover',\
                              closeOnClickAway: true,\
                              x: rect.x, y: rect.y, w: rect.width, h: rect.height,\
                              options: { placement: 'bottom-start' }\
                            },\
                            style: { width: 200 },\
                            layout: {\
                              type: 'column',\
                              style: { padding: 10, gap: 10 },\
                              children: [\
                                'Click to publish on PROD',\
                                {\
                                  type: 'button',\
                                  variant: 'primary',\
                                  label: 'Publish',\
                                  full: true,\
                                  onClick: async ({ editor }) => {\
                                    await publishWebsite(editor, 'PROD');\
                                    editor.runCommand('studio:layoutRemove', { id: layoutId });\
                                  },\
                                },\
                              ]\
                            },\
                          });\
                        },\
                      },\
                    ]\
                  }\
                }\
              },\
              { type: 'sidebarRight' },\
            ]\
          }\
        ],
      },
    },
    storage: {
      type: 'self',
      autosaveChanges: 5, // save after every 5 changes

      onSave: async ({ project, editor }) => {
        await saveToSessionStorage(PROJECT_ID, project);
        // With every save, we'll publish the website to STAGE
        await publishWebsite(editor, 'STAGE');
        console.log('Project saved and publised to STAGE', { project });
      },

      onLoad: async () => {
        const project = await loadFromSessionStorage(PROJECT_ID);
        console.log('Project loaded', { project });
        return {
          project: project || {
            pages: [\
              { name: 'Home', component: '<h1>New project</h1>' },\
            ]
          }
        };
      },
    }
  }}
/>

```

#### Use Case: Inline Data [​](https://app.grapesjs.com/docs-sdk/configuration/projects#use-case-inline-data "Direct link to Use Case: Inline Data")

Another common scenario is including the project JSON and HTML data directly in a form submission.
In this example, we'll use the `email` project type, but the usage of the `studio:projectFiles` command remains exactly the same.
Instead of calling an external API, we'll update the hidden form fields with the project JSON and HTML data on every change.

warning

Check the **Demo** tab for the complete code example.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';

// Project JSON, available on load
const inlineProject = {
  pages: [\
    { component: `<mjml>\
<mj-body background-color="#E7E7E7">\
  <mj-section full-width="full-width" background-color="#040B4F" padding-bottom="0">\
    <mj-column width="100%">\
      <mj-text color="#ffffff" font-weight="bold" align="center" text-transform="uppercase" font-size="30px" letter-spacing="1px" padding="30px 25px 28px 25px">New Email Template</mj-text>\
      <mj-text color="#17CBC4" align="center" padding-top="0" font-weight="bold" text-transform="uppercase" letter-spacing="1px" line-height="20px">Start new template</mj-text>\
      <mj-image src="https://res.cloudinary.com/dheck1ubc/image/upload/v1544156968/Email/Images/AnnouncementOffset/header-top.png" width="600px" padding="0" href="https://google.com" />\
    </mj-column>\
  </mj-section>\
  <mj-wrapper padding="0px 0px 100px 0px">>\
    <mj-section background-color="#ffffff" padding-left="15px" padding-right="15px">\
      <mj-column width="100%">\
        <mj-text color="#212b35" font-weight="bold" font-size="20px">Start typing here...</mj-text>\
        <mj-divider align="center" border-color="#DFE3E8" border-width="1px" />\
      </mj-column>\
    </mj-section>\
    <mj-section background-color="#ffffff" padding-left="15px" padding-right="15px" padding-top="0">\
      <mj-column width="50%">\
        <mj-image src="https://res.cloudinary.com/dheck1ubc/image/upload/v1544153577/Email/Images/AnnouncementOffset/Image_1.png" />\
      </mj-column>\
      <mj-column width="50%">\
        <mj-image src="https://res.cloudinary.com/dheck1ubc/image/upload/v1544153578/Email/Images/AnnouncementOffset/Image_2.png" />\
      </mj-column>\
    </mj-section>\
  </mj-wrapper>\
</mj-body>\
</mjml>` },\
  ]
};

const styleRow = { display: 'flex', gap: 10 };

const styleInput = { width: '100%', border: '1px solid', borderRadius: 5, padding: 5 };

const onDataSubmit = (ev) => {
  ev.preventDefault();
  const fd = new FormData(ev.currentTarget);
  const subject = fd.get('subject');
  const to = fd.get('to');
  const projectJSON = fd.get('projectJSON');
  const projectHTML = fd.get('projectHTML');

  console.log({ subject, to, projectJSON, projectHTML });
  alert(`DATA TO SAVE
    Subject: ${subject}
    To: ${to}
    Project JSON: ${projectJSON}
    HTML: ${projectHTML}
  `);
};

// ...
<StudioEditor
  options={{
    // ...
    theme: 'light',
    settingsMenu: false,
    layout: {
      default: {
        type: 'column',
        style: { height: '100%' },
        children: [\
          {\
            type: 'row',\
            style: { flexGrow: 1 },\
            children: {\
              type: 'canvasSidebarTop',\
              sidebarTop: {\
                  rightContainer: {\
                    buttons: ({ items }) => items.filter(item => ['undo', 'redo'].includes(item.id)),\
                  }\
                }\
            }\
          }\
        ],
      },
    },
    project: { type: 'email' },
    storage: {
      type: 'self',
      project: inlineProject,
      onSave: async ({ project, editor }) => {
        const files = await editor.runCommand('studio:projectFiles')
        const html = files.find(file => file.mimeType === 'text/html').content;

        const elJSON = document.querySelector('input[name="projectJSON"]');
        elJSON && (elJSON.value = JSON.stringify(project));

        const elHTML = document.querySelector('input[name="projectHTML"]');
        elHTML && (elHTML.value = html);

        console.log('Project updated', { project });
      },
    }
  }}
/>

```

- [Setup](https://app.grapesjs.com/docs-sdk/configuration/projects#setup)
- [Storage](https://app.grapesjs.com/docs-sdk/configuration/projects#storage)
  - [Storage Types](https://app.grapesjs.com/docs-sdk/configuration/projects#storage-types)
  - [Cloud Storage](https://app.grapesjs.com/docs-sdk/configuration/projects#cloud-storage)
  - [Self Hosted Storage](https://app.grapesjs.com/docs-sdk/configuration/projects#self-hosted-storage)
  - [Load Project From Prop](https://app.grapesjs.com/docs-sdk/configuration/projects#load-project-from-prop)
  - [Autosave](https://app.grapesjs.com/docs-sdk/configuration/projects#autosave)
- [Export](https://app.grapesjs.com/docs-sdk/configuration/projects#export)
  - [Project Files](https://app.grapesjs.com/docs-sdk/configuration/projects#project-files)

---

# File: app.grapesjs.com_docs-sdk_configuration_templates.md

---

url: "https://app.grapesjs.com/docs-sdk/configuration/templates"
title: "Templates | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/configuration/templates#__docusaurus_skipToContent_fallback)

On this page

The Studio SDK includes a powerful Template Manager, enabling you to display available templates that users can select as starting points for their projects.
The Template Manager utilizes the [PanelTemplates layout component](https://app.grapesjs.com/docs-sdk/configuration/layout/components#paneltemplates), giving you full flexibility to decide how and where the templates are rendered within your application.

![Templates](https://app.grapesjs.com/docs-sdk/assets/images/templates-c15932661a1b99ef524c8b6cedb492c4.png)

## Table of Contents

- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/templates#initialization)
- [Properties](https://app.grapesjs.com/docs-sdk/configuration/templates#properties)
  - [TemplatesConfig properties](https://app.grapesjs.com/docs-sdk/configuration/templates#templatesconfig-properties)
  - [TemplateItem properties](https://app.grapesjs.com/docs-sdk/configuration/templates#templateitem-properties)
- [I18n](https://app.grapesjs.com/docs-sdk/configuration/templates#i18n)

## Initialization [​](https://app.grapesjs.com/docs-sdk/configuration/templates#initialization "Direct link to Initialization")

Customize the template-fetching logic by defining a handler using the `templates.onLoad` option. This handler should return an array of `TemplateItem` objects, representing the available templates.

The example below demonstrates how to add a button in the top-left corner of the editor to open the PanelTemplates within a dialog:

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
        height: '100%',
        children: [\
          {\
            type: 'canvasSidebarTop',\
            sidebarTop: {\
              leftContainer: {\
                buttons: ({ items }) => [\
                  ...items,\
                  {\
                    id: 'openTemplatesButtonId',\
                    size: 's',\
                    icon: '<svg viewBox="0 0 24 24"><path d="M20 14H6C3.8 14 2 15.8 2 18S3.8 22 6 22H20C21.1 22 22 21.1 22 20V16C22 14.9 21.1 14 20 14M6 20C4.9 20 4 19.1 4 18S4.9 16 6 16 8 16.9 8 18 7.1 20 6 20M6.3 12L13 5.3C13.8 4.5 15 4.5 15.8 5.3L18.6 8.1C19.4 8.9 19.4 10.1 18.6 10.9L17.7 12H6.3M2 13.5V4C2 2.9 2.9 2 4 2H8C9.1 2 10 2.9 10 4V5.5L2 13.5Z" /></svg>',\
                    onClick: ({ editor }) => {\
                      editor.runCommand('studio:layoutToggle', {\
                        id: 'my-templates-panel',\
                        header: false,\
                        placer: { type: 'dialog', title: 'Choose a template for your project', size: 'l' },\
                        layout: {\
                          type: 'panelTemplates',\
                          content: { itemsPerRow: 3 },\
                          onSelect: ({ loadTemplate, template }) => {\
                            // Load the selected template to the current project\
                            loadTemplate(template);\
                            // Close the dialog layout\
                            editor.runCommand('studio:layoutRemove', { id: 'my-templates-panel' })\
                          }\
                        }\
                      });\
                    }\
                  }\
                ]\
              }\
            },\
            grow: true\
          },\
          { type: 'sidebarRight' }\
        ]
      }
    },
    templates: {
      // The onLoad can be an asyncronous function, so you can fetch templates from your API
      onLoad: async () => [\
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
      ]
    }

  }}
/>

```

tip

`template.data` is the GrapesJS project data JSON. You can always get the current data from an existing project in Studio via `editor.getProjectData()`.

warning

Templates returned by the custom onLoad handler are shared across all panelTemplates instances. To have different templates in each panel, use the [templates prop](https://app.grapesjs.com/docs-sdk/configuration/layout/components#paneltemplates) instead.

In this example, instead of using a button, we open the dialog when the editor loads:

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
    plugins: [\
      editor =>\
        editor.onReady(() => {\
          editor.runCommand('studio:layoutToggle', {\
            id: 'my-templates-panel',\
            header: false,\
            placer: { type: 'dialog', title: 'Choose a template for your project', size: 'l' },\
            layout: {\
              type: 'panelTemplates',\
              content: { itemsPerRow: 3 },\
              onSelect: ({ loadTemplate, template }) => {\
                loadTemplate(template);\
                editor.runCommand('studio:layoutRemove', { id: 'my-templates-panel' })\
              }\
            }\
          });\
        })\
    ],
    templates: {
      onLoad: async () => [\
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
      ]
    }

  }}
/>

```

## Properties [​](https://app.grapesjs.com/docs-sdk/configuration/templates#properties "Direct link to Properties")

### TemplatesConfig properties [​](https://app.grapesjs.com/docs-sdk/configuration/templates#templatesconfig-properties "Direct link to TemplatesConfig properties")

Show properties

| Property | Type     | Description                                                                                                                                                                                                                                                                                                                                                                                 |
| -------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| onLoad   | function | Provide a custom handler for loading list of available templates to display in the templates layout panel. It should return an array of TemplateItems.<br>**Example** <br>`codeBlockLines_AdAo<br>onLoad: async ({editor, fetchCommunityTemplates}) => {<br>  const response = await fetch('TEMPLATES_URL');<br>  const templates = await response.json();<br>  return templates;<br>}<br>` |

### TemplateItem properties [​](https://app.grapesjs.com/docs-sdk/configuration/templates#templateitem-properties "Direct link to TemplateItem properties")

Show properties

| Property | Type   | Description                                                                                                                                                                                                                                                                                                                                               |
| -------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id\*     | string | Unique id for this template item.<br>**Example**<br>`codeBlockLines_AdAo<br>"template1"<br>`                                                                                                                                                                                                                                                              |
| name\*   | string | Name displayed for this template item.<br>**Example**<br>`codeBlockLines_AdAo<br>"Template 1"<br>`                                                                                                                                                                                                                                                        |
| media    | string | A thumbnail URL for this template.<br>**Example**<br>`codeBlockLines_AdAo<br>"https://example.com/template1.jpg"<br>`                                                                                                                                                                                                                                     |
| author   | object | An object containing the name of the author and optionally a link to his socials/website.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "name": "GrapesJS",<br>  "link": "grapesjs.com"<br>}<br>`                                                                                                                                                      |
| data\*   | object | GrapesJS project data that will be loaded when the user selects this template.<br>**Example**<br>`codeBlockLines_AdAo<br>{<br>  "pages": [<br>    {<br>      "name": "Home",<br>      "component": "<h1 class=\"red-bg\">Red background template</h1><style>.red-bg { color: white; background: red; height: 100dvh; }</style>"<br>    }<br>  ]<br>}<br>` |

## I18n [​](https://app.grapesjs.com/docs-sdk/configuration/templates#i18n "Direct link to I18n")

The labels of the templates panel can be translated into different languages:

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
    plugins: [\
      editor =>\
        editor.onReady(() => {\
          editor.runCommand('studio:layoutToggle', {\
            id: 'my-templates-panel',\
            header: false,\
            placer: { type: 'dialog', title: 'Choose a template for your project', size: 'l' },\
            layout: {\
              type: 'panelTemplates',\
              content: { itemsPerRow: 3 },\
            }\
          });\
        })\
    ],
    templates: {
      // return empty array
      onLoad: async () => []
    },
    i18n: {
      locales: {
        en: {
          templates: {
            notFound: 'No templates found'
          }
        }
      }
    }
  }}
/>

```

- [Initialization](https://app.grapesjs.com/docs-sdk/configuration/templates#initialization)
- [Properties](https://app.grapesjs.com/docs-sdk/configuration/templates#properties)
  - [TemplatesConfig properties](https://app.grapesjs.com/docs-sdk/configuration/templates#templatesconfig-properties)
  - [TemplateItem properties](https://app.grapesjs.com/docs-sdk/configuration/templates#templateitem-properties)
- [I18n](https://app.grapesjs.com/docs-sdk/configuration/templates#i18n)

---

# File: app.grapesjs.com_docs-sdk_examples_overview.md

---

url: "https://app.grapesjs.com/docs-sdk/examples/overview"
title: "Examples | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/examples/overview#__docusaurus_skipToContent_fallback)

This section showcases practical implementations of the Studio SDK. Explore a variety of use cases, from basic integrations to advanced customizations, to better understand how to leverage the SDK's features in your projects.

---

# File: app.grapesjs.com_docs-sdk_overview_getting-started.md

---

url: "https://app.grapesjs.com/docs-sdk/overview/getting-started"
title: "Get started with Studio SDK | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/overview/getting-started#__docusaurus_skipToContent_fallback)

On this page

The Studio SDK is a fully embeddable, drag-and-drop, white-label version of our standalone [Studio](https://app.grapesjs.com/studio) editor. It enables seamless integration of our ready-to-use visual builder into any external web application, giving your users a powerful editing experience. The SDK is highly customizable and extendable through the [GrapesJS](https://grapesjs.com/docs/) core API, making it adaptable to your specific needs.

To get a feel of the Studio SDK's capabilities, you can explore and experiment with our live Studio at [https://app.grapesjs.com/studio](https://app.grapesjs.com/studio).

![Studio App](https://app.grapesjs.com/docs-sdk/assets/images/studio-app-9bf0c6393e2479e2cdbffc9d3acc9430.png)

## Installation [​](https://app.grapesjs.com/docs-sdk/overview/getting-started#installation "Direct link to Installation")

### Download [​](https://app.grapesjs.com/docs-sdk/overview/getting-started#download "Direct link to Download")

To install the Studio SDK in your application, run the following command in your terminal.

- npm
- pnpm
- yarn
- CDN

```codeBlockLines_AdAo
npm i @grapesjs/studio-sdk

```

### Setup web project [​](https://app.grapesjs.com/docs-sdk/overview/getting-started#setup-web-project "Direct link to Setup web project")

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

### Setup email project [​](https://app.grapesjs.com/docs-sdk/overview/getting-started#setup-email-project "Direct link to Setup email project")

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

### Next Step [​](https://app.grapesjs.com/docs-sdk/overview/getting-started#next-step "Direct link to Next Step")

To ensure a seamless integration of the Studio SDK within your application, refer to the [Configuration Overview](https://app.grapesjs.com/docs-sdk/configuration/overview). The page summarizes the available configurations, including how to properly store projects, manage user assets, and customize various aspects of the editor.

When you're ready to publish your Studio SDK integration on a public domain, make sure to set up a license by creating an [SDK license](https://app.grapesjs.com/docs-sdk/overview/licenses).

- [Installation](https://app.grapesjs.com/docs-sdk/overview/getting-started#installation)
  - [Download](https://app.grapesjs.com/docs-sdk/overview/getting-started#download)
  - [Setup web project](https://app.grapesjs.com/docs-sdk/overview/getting-started#setup-web-project)
  - [Setup email project](https://app.grapesjs.com/docs-sdk/overview/getting-started#setup-email-project)
  - [Next Step](https://app.grapesjs.com/docs-sdk/overview/getting-started#next-step)

---

# File: app.grapesjs.com_docs-sdk_overview_licenses.md

---

url: "https://app.grapesjs.com/docs-sdk/overview/licenses"
title: "Licenses | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/overview/licenses#__docusaurus_skipToContent_fallback)

On this page

Learn how to create a license for your Studio SDK. A license is essential for running the Studio on a public domain (you can still use all the features locally without it). Follow the steps below to sign up, create, and configure your license.

### Sign up [​](https://app.grapesjs.com/docs-sdk/overview/licenses#sign-up "Direct link to Sign up")

To get started, sign-in from [https://app.grapesjs.com](https://app.grapesjs.com/). Signing up is the first step to access the licensing features of the Studio SDK.

![Sign In](https://app.grapesjs.com/docs-sdk/assets/images/sign-in-d05017d8d9ff13117fe7cf64f2a356c1.png)

The image above shows the sign-in page where you can log-in and access your account.

### Create license [​](https://app.grapesjs.com/docs-sdk/overview/licenses#create-license "Direct link to Create license")

Once signed in, you can create a new license for your application. This license will enable you to use the Studio SDK in your applications.

![Create License](https://app.grapesjs.com/docs-sdk/assets/images/create-license-a12e32e94ffb7bce06cea6a123f436a4.png)

The image above illustrates the interface for creating a new license.

### Configure license [​](https://app.grapesjs.com/docs-sdk/overview/licenses#configure-license "Direct link to Configure license")

After creating the license, you need to configure it to match your project's requirements. This step ensures that the license is correctly set up and ready for use.

To ensure proper functionality, it is essential to configure your license by adding the domain where the Studio SDK will be deployed. Additionally, you must enable the license to run the Studio on a public domain (you can still use all the Studio features in localhost without a license). This configuration step is crucial for the seamless integration and operation of the Studio SDK.

![Configure License](https://app.grapesjs.com/docs-sdk/assets/images/configure-license-950315475904289a0ffcd79c5ed32075.png)

The image above shows the configuration options available for your license. Adjust these settings to configure the domain and enable the license.

- [Sign up](https://app.grapesjs.com/docs-sdk/overview/licenses#sign-up)
- [Create license](https://app.grapesjs.com/docs-sdk/overview/licenses#create-license)
- [Configure license](https://app.grapesjs.com/docs-sdk/overview/licenses#configure-license)

---

# File: app.grapesjs.com_docs-sdk_plugins_overview.md

---

url: "https://app.grapesjs.com/docs-sdk/plugins/overview"
title: "Plugins | GrapesJS Studio SDK"

---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/overview#__docusaurus_skipToContent_fallback)

On this page

In this section, you will find a variety of example plugins that illustrate how to effectively use them within the SDK. These examples are designed to help you get started quickly and easily.

## What is a Plugin? [​](https://app.grapesjs.com/docs-sdk/plugins/overview#what-is-a-plugin "Direct link to What is a Plugin?")

A plugin is an extension that enhances the core functionality of the editor through functions that are run when the editor is initialized. Plugins allow you to introduce new features, modify existing behaviors, or integrate external libraries without altering the core logic.

By using plugins, developers can customize the canvas to fit specific use cases, whether it's adding new components, or extending the editor's capabilities.

## Why use Plugins? [​](https://app.grapesjs.com/docs-sdk/plugins/overview#why-use-plugins "Direct link to Why use Plugins?")

Plugins are essential for keeping the editor modular, scalable, and adaptable to different needs. Here's why you should use plugins:

- **Customization**: Extend the platform with tailored functionality for your project.
- **Reusability**: Create reusable features without modifying the core editor.
- **Integration**: Easily connect external libraries or APIs.
- **Maintainability**: Keep core updates separate from custom modifications.

- [What is a Plugin?](https://app.grapesjs.com/docs-sdk/plugins/overview#what-is-a-plugin)
- [Why use Plugins?](https://app.grapesjs.com/docs-sdk/plugins/overview#why-use-plugins)

---
