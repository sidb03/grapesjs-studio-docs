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

## Basic setup [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers\#basic-setup "Direct link to Basic setup")

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

## Endless scrolling pagination [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers\#endless-scrolling-pagination "Direct link to Endless scrolling pagination")

To enable endless scrolling in an Asset Provider, instead of an array, return an object with an items prop from `AssetProvider.onLoad()`.
We support both offset-based and token-based pagination.

### Offset-based pagination [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers\#offset-based-pagination "Direct link to Offset-based pagination")

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

### Token-based pagination [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers\#token-based-pagination "Direct link to Token-based pagination")

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

## Custom item layout [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers\#custom-item-layout "Direct link to Custom item layout")

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

## Commands [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers\#commands "Direct link to Commands")

Here's a list of commands to update Asset Providers dynamically.

### Get Asset Providers [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers\#get-asset-providers "Direct link to Get Asset Providers")

Get a list of registered Asset Providers.

```codeBlockLines_AdAo
const providers = editor.runCommand(StudioCommands.assetProviderGet);

```

### Add Asset Provider [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers\#add-asset-provider "Direct link to Add Asset Provider")

Register a new Asset Provider. If an Asset Provider with the same `id` already exists, it will be removed.
You can use the `index` prop to specify its position in the list of providers.

```codeBlockLines_AdAo
editor.runCommand(StudioCommands.assetProviderAdd, { provider: { id: 'new-provider-id' }, index: 0 });

```

### Remove Asset Provider [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers\#remove-asset-provider "Direct link to Remove Asset Provider")

Remove a registered Asset Provider.

```codeBlockLines_AdAo
editor.runCommand(StudioCommands.assetProviderRemove, { id: 'new-provider-id' });

```

### Properties [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers\#properties "Direct link to Properties")

#### AssetProvider properties [​](https://app.grapesjs.com/docs-sdk/configuration/assets/asset-providers\#assetprovider-properties "Direct link to AssetProvider properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| id\* | string | Asset Provider ID.<br>**Example**<br>```codeBlockLines_AdAo<br>"my-provider"<br>``` |
| types\* | string | Asset types supported by this provider. Only providers that support the current asset type show up in the asset provider filter.<br>**Example** <br>```codeBlockLines_AdAo<br>types: 'image',<br>// Or an array of types<br>types: ['image', 'video']<br>``` |
| label\* | string | Label to display in the asset provider filter. You may use a function instead to translate this string.<br>**Example** <br>```codeBlockLines_AdAo<br>label: 'My asset provider'<br>// As a function, for dynamic labels<br>label: ({ editor }) => editor.I18n.t('myProviderLabel')<br>``` |
| search | object | Search configuration.<br>**Example** <br>```codeBlockLines_AdAo<br>search: {<br> // Set this to true if you want AssetProvider.onLoad to retrigger when the user types in the search field. When false, loaded assets are filtered locally by name.<br> reloadOnInput: true,<br> // Search value  debounce time in milliseconds<br> debounceMs: 1000<br>}<br>``` |
| onLoad\* | function | Define how to fetch these assets. Return an array of assets. You may return an array of Page objects to enable endless scrolling, you can rely on the pageIndex argument for this.<br>**Example** <br>```codeBlockLines_AdAo<br>async () => {<br>  // Simple asset array<br>  return [<br>    { src: 'https://www.example.com/items/1' },<br>    { src: 'https://www.example.com/items/2' },<br>    { src: 'https://www.example.com/items/3' }<br>  ]<br>}<br>async ({ pageIndex }) => {<br>  // Offset based pagination<br>  const pageSize = 20;<br>  const params = new URLSearchParams({ page: pageIndex, pageSize })<br>  const response = await fetch(`https://www.example.com/items?${params}`)<br>  const page = await response.json()<br>  const itemCount = pageSize * pageIndex + page.items.length<br>  return {<br>    items: page.items,<br>    isLastPage: itemCount >= page.total<br>  }<br>}<br>async ({ pageCustomData }) => {<br>  // Token based pagination.<br>  const params = new URLSearchParams({ pageToken: pageCustomData?.token })<br>  const response = await fetch(`https://www.example.com/items?${params}`)<br>  const page = await response.json()<br>  return {<br>    items: page.items,<br>    nextPageCustomData: { token: page.nextPageToken },<br>    isLastPage: !page.nextPageToken<br>  }<br>}<br>``` |
| itemLayout | function | Custom layout for rendering an asset item in the AssetManager.<br>**Example** <br>```codeBlockLines_AdAo<br>itemLayout: ({ assetProps, onSelect }) => ({<br> type: 'column',<br> style: { height: 150 },<br> onClick: () => onSelect(assetProps),<br> children: [<br>   { type: 'custom', render: () => `<img src="${assetProps.src}" style="width: 100%; height: 100%; object-fit: cover">` },<br>   { type: 'text', style: { width: '100%' }, content: assetProps.name ?? '' }<br> ]<br>})<br>``` |

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