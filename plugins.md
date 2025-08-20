# Merged Documentation

This file contains all markdown files merged from the current directory.
Generated on: Fri Aug  8 02:44:51 IST 2025

---

# File: app.grapesjs.com_docs-sdk_plugins_asset-providers_google-fonts.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/asset-providers/google-fonts"
title: "Google Fonts | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/asset-providers/google-fonts#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` |
| Plan | Startup plan |

This plugin adds an asset provider for Google Fonts, allowing users to add Google Fonts to projects.

![Google Fonts Asset Provider plugin](https://app.grapesjs.com/docs-sdk/assets/images/google-fonts-asset-provider-plugin-f88ba87b42ef88e6de775f6843988a6b.webp)

Install the Studio SDK plugins package:

- npm
- pnpm
- yarn
- CDN

```codeBlockLines_AdAo
npm i @grapesjs/studio-sdk-plugins

```

Use the plugin in the project with your Google Fonts `apiKey`:

warning

You must provide your own `apiKey` in the plugin options when using this in your project.
Get a **Web Fonts Developer API** key from the [Google Cloud Console](https://console.cloud.google.com/marketplace/product/google/webfonts.googleapis.com).

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
          component: `\
            <p>Open the Typography section on the right panel. Use the "+" button in "Font" to select a google font.</p>\
          `\
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

info

The demo uses a shared `apiKey`, which may occasionally exceed Google's rate limits.

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/asset-providers/google-fonts\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| apiKey | string | Google font API key. Get yours at https://console.cloud.google.com/ |
| searchParams | [SearchParamsProps](https://app.grapesjs.com/docs-sdk/plugins/asset-providers/google-fonts#searchparams-properties) | Override search params used for the listing. |
| i18n | object |  |

#### SearchParams properties [​](https://app.grapesjs.com/docs-sdk/plugins/asset-providers/google-fonts\#searchparams-properties "Direct link to SearchParams properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| auth |  | Auth client or API Key for the request |
| $.xgafv | string | V1 error format. |
| access\_token | string | OAuth access token. |
| alt | string | Data format for response. |
| callback | string | JSONP |
| fields | string | Selector specifying which fields to include in a partial response. |
| key | string | API key. Your API key identifies your project and provides you with API access, quota, and reports. Required unless you provide an OAuth 2.0 token. |
| oauth\_token | string | OAuth 2.0 token for the current user. |
| prettyPrint | boolean | Returns response with indentations and line breaks. |
| quotaUser | string | Available to use for quota purposes for server-side applications. Can be any arbitrary string assigned to a user, but should not exceed 40 characters. |
| uploadType | string | Legacy upload protocol for media (e.g. "media", "multipart"). |
| upload\_protocol | string | Upload protocol for media (e.g. "raw", "multipart"). |
| capability | array | Controls the font urls in \`Webfont.files\`, by default, static ttf fonts are sent. |
| category | string | Filters by Webfont.category, if category is found in Webfont.categories. If not set, returns all families. |
| family | array | Filters by Webfont.family, using literal match. If not set, returns all families |
| sort | string | Enables sorting of the list. |
| subset | string | Filters by Webfont.subset, if subset is found in Webfont.subsets. If not set, returns all families. |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/asset-providers/google-fonts#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_asset-providers_youtube-asset-provider.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/asset-providers/youtube-asset-provider"
title: "YouTube Asset Provider | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/asset-providers/youtube-asset-provider#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` |
| Plan | Startup plan |

This plugin adds an asset provider for YouTube videos and integrates a button into the default video component traits, allowing users to easily browse and select YouTube videos.

![YouTube Asset Provider plugin](https://app.grapesjs.com/docs-sdk/assets/images/youtube-asset-provider-plugin-fd4d55ac13caa583a2b38720c57034fa.png)

Install the Studio SDK plugins package:

- npm
- pnpm
- yarn
- CDN

```codeBlockLines_AdAo
npm i @grapesjs/studio-sdk-plugins

```

Use the plugin in the project with your YouTube `apiKey`:

warning

You must provide your own `apiKey` in the plugin options when using this in your project.
Get a **YouTube Data API v3** key from the [Google Cloud Console](https://console.cloud.google.com/apis/library/browse?q=youtube).

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { youtubeAssetProvider } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
    project: {
      default: {
        pages: [{\
          name: 'Home',\
          component: `\
            <h2>Double click the video below to open the YouTube Provider</h2>\
            <iframe data-gjs-type="video" data-gjs-provider="yt" style="width: 300px"/>\
          `\
        }]
      }
    },
    plugins: [\
      youtubeAssetProvider.init({\
        apiKey: 'YOUTUBE_API_KEY',\
        searchParams: ({ searchValue }) => ({\
          q: searchValue || 'Default search query',\
        })\
      }),\
      editor => {\
        editor.onReady(() => {\
          const videoCmp = editor.getWrapper().findType('video')[0];\
          editor.select(videoCmp);\
        });\
      }\
    ],

  }}
/>

```

info

The demo uses a shared `apiKey`, which may occasionally exceed YouTube's rate limits.

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/asset-providers/youtube-asset-provider\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| apiKey | string | A YouTube Data API v3 key. Get yours at https://console.cloud.google.com/ |
| searchParams | [SearchParamsProps](https://app.grapesjs.com/docs-sdk/plugins/asset-providers/youtube-asset-provider#searchparams-properties) | Override search params used for listing videos. |
| thumbnailQuality | string | Preferred thumbnail quality<br>**Default** <br>```codeBlockLines_AdAo<br>high<br>``` |
| skipVideoComponent | boolean | Don't add the 'Search on YouTube' button to properties panel of the video component.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| i18n | object |  |

#### SearchParams properties [​](https://app.grapesjs.com/docs-sdk/plugins/asset-providers/youtube-asset-provider\#searchparams-properties "Direct link to SearchParams properties")

Show properties

| Property | Type | Description |
| --- | --- | --- |
| auth |  | Auth client or API Key for the request |
| $.xgafv | string | V1 error format. |
| access\_token | string | OAuth access token. |
| alt | string | Data format for response. |
| callback | string | JSONP |
| fields | string | Selector specifying which fields to include in a partial response. |
| key | string | API key. Your API key identifies your project and provides you with API access, quota, and reports. Required unless you provide an OAuth 2.0 token. |
| oauth\_token | string | OAuth 2.0 token for the current user. |
| prettyPrint | boolean | Returns response with indentations and line breaks. |
| quotaUser | string | Available to use for quota purposes for server-side applications. Can be any arbitrary string assigned to a user, but should not exceed 40 characters. |
| uploadType | string | Legacy upload protocol for media (e.g. "media", "multipart"). |
| upload\_protocol | string | Upload protocol for media (e.g. "raw", "multipart"). |
| channelId | string | Filter on resources belonging to this channelId. |
| channelType | string | Add a filter on the channel search. |
| eventType | string | Filter on the livestream status of the videos. |
| forContentOwner | boolean | Search owned by a content owner. |
| forDeveloper | boolean | Restrict the search to only retrieve videos uploaded using the project id of the authenticated user. |
| forMine | boolean | Search for the private videos of the authenticated user. |
| location | string | Filter on location of the video |
| locationRadius | string | Filter on distance from the location (specified above). |
| maxResults | number | The \*maxResults\* parameter specifies the maximum number of items that should be returned in the result set. |
| onBehalfOfContentOwner | string | \*Note:\* This parameter is intended exclusively for YouTube content partners. The \*onBehalfOfContentOwner\* parameter indicates that the request's authorization credentials identify a YouTube CMS user who is acting on behalf of the content owner specified in the parameter value. This parameter is intended for YouTube content partners that own and manage many different YouTube channels. It allows content owners to authenticate once and get access to all their video and channel data, without having to provide authentication credentials for each individual channel. The CMS account that the user authenticates with must be linked to the specified YouTube content owner. |
| order | string | Sort order of the results. |
| pageToken | string | The \*pageToken\* parameter identifies a specific page in the result set that should be returned. In an API response, the nextPageToken and prevPageToken properties identify other pages that could be retrieved. |
| part | array | The \*part\* parameter specifies a list of one or more search resource properties that the API response will include. Set the parameter value to snippet. |
| publishedAfter | string | Filter on resources published after this date. |
| publishedBefore | string | Filter on resources published before this date. |
| q | string | Textual search terms to match. |
| regionCode | string | Display the content as seen by viewers in this country. |
| relevanceLanguage | string | Return results relevant to this language. |
| safeSearch | string | Indicates whether the search results should include restricted content as well as standard content. |
| topicId | string | Restrict results to a particular topic. |
| type | array | Restrict results to a particular set of resource types from One Platform. |
| videoCaption | string | Filter on the presence of captions on the videos. |
| videoCategoryId | string | Filter on videos in a specific category. |
| videoDefinition | string | Filter on the definition of the videos. |
| videoDimension | string | Filter on 3d videos. |
| videoDuration | string | Filter on the duration of the videos. |
| videoEmbeddable | string | Filter on embeddable videos. |
| videoLicense | string | Filter on the license of the videos. |
| videoPaidProductPlacement | string |  |
| videoSyndicated | string | Filter on syndicated videos. |
| videoType | string | Filter on videos of a specific type. |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/asset-providers/youtube-asset-provider#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_canvas_absolute-mode.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/canvas/absolute-mode"
title: "Absolute Mode | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/canvas/absolute-mode#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` |
| Plan | Startup plan |

Enables free drag-and-drop positioning using absolute coordinates. This mode is ideal for fixed-layout designs like documents for print, business cards, certificates, or static prototypes where responsiveness isn't required.

While absolute positioning isn't suited for responsive web design, you can enable it conditionally (e.g. only when the element already uses position: absolute) to apply it where it makes sense.

![Absolute Mode plugin](https://app.grapesjs.com/docs-sdk/assets/images/absolute-mode-plugin-b201805b6abcb4f8b3e46795187668cc.webp)

Install the Studio SDK plugins package:

- npm
- pnpm
- yarn
- CDN

```codeBlockLines_AdAo
npm i @grapesjs/studio-sdk-plugins

```

The example below demonstrates the default global absolute mode, where all existing and newly added components are positioned using absolute coordinates.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { canvasAbsoluteMode } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        canvasAbsoluteMode\
      ],
      devices: {
        default: [\
          { id: 'desktop', name: 'Desktop', width: '' }\
        ]
      },
      project: {
        default: {
          pages: [\
            {\
              name: 'Presentation',\
              component: `\
                <div style="position: relative; width: 800px; height: 500px; margin: 70px auto 0; background: linear-gradient(135deg, #f5f7fa, #c3cfe2); color: #1a1a1a; border-radius: 12px; box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1); overflow: hidden;">\
                  <div style="position: absolute; top: 0; left: 550px; width: 300px; height: 100%; background-color: #baccec; transform: skewX(-12deg)"></div>\
\
                  <h1 style="position: absolute; top: 40px; left: 40px; font-size: 50px; margin: 0; font-weight: 700;">\
                    Absolute Mode\
                  </h1>\
\
                  <p style="position: absolute; top: 135px; left: 40px; font-size: 22px; max-width: 450px; line-height: 1.5; color: #333;">\
                    Enable free positioning for your elements — perfect for fixed layouts like presentations, business cards, or print-ready designs.\
                  </p>\
\
                  <ul data-gjs-type="text" style="position: absolute; top: 290px; left: 40px; font-size: 18px; line-height: 2; list-style: none; padding: 0;">\
                    <li>🎯 Drag & place elements anywhere</li>\
                    <li>🧲 Smart snapping & axis locking</li>\
                    <li>⚙️ You custom logic</li>\
                  </ul>\
\
                  <div style="position: absolute; left: 540px; top: 100px; width: 200px; height: 200px; background: rgba(255, 255, 255, 0.3); border-radius: 20px; backdrop-filter: blur(10px); box-shadow: 0 8px 24px rgba(0,0,0,0.1); display: flex; align-items: center; justify-content: center; font-size: 80px;">\
                    📐\
                  </div>\
\
                  <div style="position: absolute; top: 405px; left: 590px; font-size: 14px; color: #555;">\
                    Studio SDK · GrapesJS\
                  </div>\
              </div>\
\
              <style>\
                body {\
                  position: relative;\
                  background: linear-gradient(135deg, #f5f7fa, #c3cfe2);\
                  font-family: system-ui;\
                  overflow: hidden;\
                }\
              </style>\
              `,\
            }\
          ]
        }
      },


  }}
/>

```

#### Conditional Usage [​](https://app.grapesjs.com/docs-sdk/plugins/canvas/absolute-mode\#conditional-usage "Direct link to Conditional Usage")

You can also enable absolute mode conditionally, for example, only for elements that already have `position: absolute` set in their styles.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { canvasAbsoluteMode } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        canvasAbsoluteMode.init({\
          globalAbsolute: false,\
\
          // This is the default behavior when globalAbsolute is false\
          enableAbsolute: ({ component }) => {\
            const cmpEl = component.getEl();\
            if (cmpEl && getComputedStyle(cmpEl).position === 'absolute') {\
              return true;\
            }\
          }\
        })\
      ],
      project: {
        default: {
          pages: [\
            {\
              name: 'Demo',\
              component: `\
                <div style="padding: 20px; display: flex; flex-direction: column; gap: 10px; margin: 20px 0">\
                  <div class="static-box">Static Box 1</div>\
                  <div class="static-box">Static Box 2</div>\
                  <div class="static-box">Static Box 3</div>\
                </div>\
\
                <div class="absolute-box">Absolute box</div>\
\
                <style>\
                  body {\
                    font-family: system-ui;\
                  }\
                  .static-box, .absolute-box {\
                    padding: 1rem;\
                    border-radius: 8px;\
                    background-color: rgba(75, 227, 165, 1);\
                  }\
                  .absolute-box {\
                    position: absolute;\
                    background-color: rgba(227, 75, 75, 1);\
                    top: 250px;\
                    left: 20px;\
                  }\
                </style>\
              `,\
            }\
          ]
        }
      },


  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/canvas/absolute-mode\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| globalAbsolute | boolean | Enable absolute mode globally. This will activate absolute drag mode for all components.<br>**Default** <br>```codeBlockLines_AdAo<br>true<br>``` |
| enableAbsolute | function | Custom function to determine whether an element should use absolute positioning mode. This allows to define your own logic for enabling absolute mode, such as checking if the element has \`position: absolute\` or matches a specific condition.<br>This check is skipped if the \`globalAbsolute\` option is set to \`true\`.<br>**Example** <br>```codeBlockLines_AdAo<br>enableAbsolute: ({ component }) => getComputedStyle(component.getEl()).position === 'absolute'<br>``` |
| snapping | object | Enables grid-based snapping for absolute positioning. Defines the snapping intervals in pixels along the x and y axes. When set, dragged elements will align to the closest grid point.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "x": 100,<br>  "y": 50<br>}<br>``` |
| locking | object | Enables axis locking during element dragging. When enabled, holding the Shift key while snapping to a nearby component locks movement to the matched axis (horizontal or vertical), allowing for precise alignment based on distance indicators.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "x": true,<br>  "y": false<br>}<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/canvas/absolute-mode#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_canvas_emptyState.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/canvas/emptyState"
title: "Empty State | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/canvas/emptyState#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` `email` |
| Plan | Startup plan |

Update empty components with custom content inside.

![Custom empty state](https://app.grapesjs.com/docs-sdk/assets/images/empty-state-plugin-02cc8c6bca3ffee3674949805d0c3633.webp)

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
import { canvasEmptyState } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      project: {
        // For demo purposes, define a default empty project to display the body's empty state.
        default: { pages: [{ name: 'Home' }] }
      },
      plugins: [\
        canvasEmptyState?.init({\
          emptyStates: [\
            {\
              isValid: ['wrapper'],\
              render: ({ editor, mount, unmount }) => {\
                const container = document.createElement('div');\
                container.className = 'empty-state-wrapper';\
                container.innerHTML = `\
                  <div class="empty-state-wrapper__card">\
                    <div class="empty-state-wrapper__content">\
                      <h1>This is the empty state element of the body!</h1>\
                      <p>Drop the column block to see another component with a custom empty state!</p>\
                    </div>\
                    <button class="empty-state-wrapper__btn">Add Block</button>\
                  </div>\
\
                  <style>\
                  .empty-state-wrapper {\
                    font-family: system-ui, sans-serif;\
                    background-color: #f5f5f5;\
                    display: flex;\
                    justify-content: center;\
                    align-items: center;\
                    height: 100dvh;\
                  }\
                  .empty-state-wrapper__card {\
                    text-align: center;\
                    display: flex;\
                    flex-direction: column;\
                    align-items: center;\
                    justify-content: center;\
                    padding: 24px;\
                    border-radius: 4px;\
                    background-color: white;\
                    color: #333;\
                    gap: 16px;\
                  }\
                  .empty-state-wrapper__btn {\
                    background-color: #8b5cf6;\
                    padding: 8px 16px;\
                    margin: 0;\
                    color: white;\
                    border: none;\
                    cursor: pointer;\
                    border-radius: 4px;\
                  }\
                  </style>\
                `;\
\
                const btn = container.querySelector('button');\
                btn.addEventListener('click', () => {\
                  editor.runCommand('studio:layoutToggle', {\
                    id: 'emptyStateWrapperBlocks',\
                    layout: { type: 'panelBlocks' },\
                    header: { label: 'Blocks' },\
                    placer: { type: 'absolute', position: 'left' }\
                  });\
                });\
\
                // Mount the empty state container\
                mount(container);\
                return () => unmount(container);\
              }\
            },\
            {\
              isValid: ({ component }) => component.is('gridColumn'),\
              render: ({ editor, component, mount }) => {\
                const container = document.createElement('div');\
                container.className = 'empty-state-wrapper';\
                container.innerHTML = `\
                  <button class="empty-state-column__btn">Add text to column</button>\
                  <style>\
                    .empty-state-wrapper {\
                      display: flex;\
                      justify-content: center;\
                      align-items: center;\
                    }\
                    .empty-state-column__btn {\
                      background-color: #8b5cf6;\
                      padding: 8px 16px;\
                      margin: 0;\
                      color: white;\
                      border: none;\
                      cursor: pointer;\
                      border-radius: 4px;\
                    }\
                  </style>\
                `;\
                const btn = container.querySelector('button');\
                btn.addEventListener('click', () => {\
                  const textCmp = component.append('<div>New Text</div>')[0];\
                  editor.select(textCmp);\
                  textCmp.trigger('active');\
                });\
                mount(container);\
              }\
            }\
          ]\
        })\
      ],


  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/canvas/emptyState\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| emptyStates | array | Empty state types to render.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br> {<br>   isValid: 'componentA',// check for valid componet type, as a string...<br>   isValid: ['componentA', 'componentB'], // ...as an array of component types...<br>   isValid: ({ component }) => component.is('componentA'), // ...or as a function<br>   // Render function to run when the component is empty<br>   render: ({ editor, component, mount, unmount }) => {<br>     const container = document.createElement('div');<br>     // ...<br>     window.addEventListener('someGlobalEvent', onSomeEvent);<br>     mount(container);<br>     // Clean up function<br>     return () => {<br>       unmount(container);<br>       window.removeEventListener('someGlobalEvent', onSomeEvent);<br>     }<br>   },<br> }<br>]<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/canvas/emptyState#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_canvas_full-size.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/canvas/full-size"
title: "Full Size | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/canvas/full-size#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` `email` |
| Plan | Startup plan |

Enable full-size device in the editor canvas, independent of your screen size. Perfect for designing large templates seamlessly.

![Full size screen](https://app.grapesjs.com/docs-sdk/assets/images/full-size-plugin-04c7d39d04a596518bf6563730bc07a3.webp)

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
import { canvasFullSize } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        canvasFullSize\
      ],
  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/canvas/full-size\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| deviceMaxWidth | number | Default max width for the devices.<br>**Default** <br>```codeBlockLines_AdAo<br>1200<br>``` |
| deviceMinHeigth | number | Default min height for the devices.<br>**Default** <br>```codeBlockLines_AdAo<br>500<br>``` |
| canvasOffsetY | number | Offset for the canvas on the Y axis (margin between the canvas and content frame).<br>**Default** <br>```codeBlockLines_AdAo<br>30<br>``` |
| canvasOffsetX | number | Offset for the canvas on the X axis (margin between the canvas and content frame).<br>**Default** <br>```codeBlockLines_AdAo<br>50<br>``` |
| canvasTransition | number | Transition time for the canvas when the screen size changes (in seconds).<br>**Default** <br>```codeBlockLines_AdAo<br>0.3<br>``` |
| frameBorderRadius | number | Border radius for the content frame (in px).<br>**Default** <br>```codeBlockLines_AdAo<br>5<br>``` |
| frameTransition | number | Transition time for the content frame when the device changes (in seconds).<br>**Default** <br>```codeBlockLines_AdAo<br>0.3<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/canvas/full-size#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_canvas_grid-mode.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/canvas/grid-mode"
title: "Grid Mode | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/canvas/grid-mode#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` |
| Plan | Startup plan |

Visually manage CSS Grid layouts right inside the editor. This plugin lets users drag grid elements on the canvas and update grid-related CSS properties from the Style Manager.

![Grid Mode plugin](https://app.grapesjs.com/docs-sdk/assets/images/grid-mode-plugin-253bb04bd7670e02ddff6ab01f87ffb3.webp)

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
import { canvasGridMode } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        canvasGridMode?.init({\
          styleableGrid: true,\
        })\
      ],
      project: {
        default: {
          pages: [\
            {\
              name: 'Home',\
              component: `\
              <h1>Grid Row</h1>\
              <div class="grid-row">\
                <div class="grid-item" style="grid-column: 1 / 5;">\
                  First container\
                </div>\
                <div class="grid-item" style="grid-column: 9 / 13;">\
                  Second container\
                </div>\
              </div>\
\
              <h1>Grid section</h1>\
              <section class="grid-section">\
                <div class="grid-item" style="grid-area: 1 / 1 / 2 / 13; background-color: #e3f2fd">\
                  Header\
                </div>\
                <div class="grid-item" style="grid-area: 2 / 1 / 5 / 3; background-color: #fce4ec">\
                  Sidebar\
                </div>\
                <div class="grid-item" style="grid-area: 2 / 4 / 5 / 10; background-color: #e8f5e9">\
                  Main Content\
                </div>\
                <div class="grid-item" style="grid-area: 2 / 11 / 5 / 13; background-color: #fff3e0">\
                  Extras\
                </div>\
                <div class="grid-item" style="grid-area: 5 / 1 / 6 / 13; background: #ede7f6;">Footer</div>\
              </section>\
\
              <style>\
                body { font-family: system-ui; }\
                h1 { text-align: center; }\
\
                .grid-row {\
                  display: grid;\
                  grid-template-columns: repeat(12, 1fr);\
                  gap: 10px;\
                  padding: 2rem;\
                }\
\
                .grid-item {\
                  border: 1px solid #ddd;\
                  padding: 1.5rem;\
                  border-radius: 5px;\
                }\
\
                .grid-section {\
                  display: grid;\
                  grid-template-columns: repeat(12, 1fr);\
                  grid-template-rows: repeat(7, minmax(50px, 1fr));\
                  gap: 1rem;\
                  padding: 2rem;\
                  background-color: #f9f9f9;\
                }\
\
                @media (max-width: 992px) {\
                  .grid-row {\
                    grid-template-rows: repeat(2, 1fr);\
                  }\
                }\
              </style>\
              `,\
            }\
          ]
        }
      }


  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/canvas/grid-mode\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| enableGrid | function | Provide a custom logic to enable the grid mode.<br>**Example** <br>```codeBlockLines_AdAo<br>enableGrid: ({ component, isParentGrid }) => {<br> if (isParentGrid && component.is('image')) {<br>  return true; // enable grid mode only for images<br> }<br>}<br>``` |
| itemResizable | object | Make grid items resizable when selected. Could also be a function for custom logic.<br>**Example** <br>```codeBlockLines_AdAo<br>itemResizable: ({ component }) => {<br>   if (component.is('text')) {<br>    // Make the text component resizable only on the right and left sides.<br>    return { cr: true, cl: true };<br>   } else if (component.is('image')) {<br>    // Disable resizing for images<br>    return false;<br>   }<br>   // Enable resizing for all other components<br>   return true;<br>}<br>```<br>**Default** <br>```codeBlockLines_AdAo<br>true<br>``` |
| styleableGrid | boolean | Allow to edit grid properties in Style Manager.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/canvas/grid-mode#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_components_accordion.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/components/accordion"
title: "Accordion | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/components/accordion#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` |
| Plan | Startup plan |

Accessible and customizable accordion component, allowing users to create collapsible content sections with smooth animations and keyboard navigation support.

![Accordion plugin](https://app.grapesjs.com/docs-sdk/assets/images/accordion-plugin-09ee15747de2cbc119e7e9ca654abec6.webp)

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
import { accordionComponent } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        accordionComponent.init({\
          block: { category: 'My Accordions' },\
          blockGroup: { category: 'My Accordions' },\
        })\
      ],
      project: {
        default: {
          pages: [\
            {\
              name: 'Home',\
              component: `\
              <div class="demo-container">\
                <h1>Single accordion</h1>\
\
                <details data-gjs-type="accordion"></details>\
\
                <h1>Single accordion open</h1>\
\
                <details data-gjs-type="accordion" open>\
                  <summary data-gjs-type="accordion-header">\
                    <div>Accordion Header</div>\
                    <div data-gjs-type="accordion-marker"></div>\
                  </summary>\
                  <div data-gjs-type="accordion-content">\
                    <div>Accordion Content</div>\
                  </div>\
                </details>\
\
                <h1>Accordion group</h1>\
\
                <div class="cls-accordion-group" data-gjs-type="accordion-group"></div>\
\
                <h1>Custom styles</h1>\
\
                <div data-gjs-type="accordion-group" class="cls-accordion-group">\
                  <details data-gjs-type="accordion">\
                    <summary data-gjs-type="accordion-header" class="cls-summary">\
                      <div>Accordion 1</div>\
                      <div data-gjs-type="accordion-marker"></div>\
                    </summary>\
                    <div data-gjs-type="accordion-content" class="cls-content">\
                      <div>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</div>\
                    </div>\
                  </details>\
                  <details data-gjs-type="accordion">\
                    <summary data-gjs-type="accordion-header" class="cls-summary">\
                      <div>Accordion 2</div>\
                      <div data-gjs-type="accordion-marker">\
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M11,4H13V16L18.5,10.5L19.92,11.92L12,19.84L4.08,11.92L5.5,10.5L11,16V4Z" /></svg>\
                      </div>\
                    </summary>\
                    <div data-gjs-type="accordion-content" class="cls-content">\
                      <div>\
                        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.\
                      </div>\
                    </div>\
                  </details>\
                  <details data-gjs-type="accordion">\
                    <summary data-gjs-type="accordion-header" class="cls-summary">\
                      <div>Accordion 2</div>\
                      <div data-gjs-type="accordion-marker">\
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><title>arrow-down-drop-circle</title><path d="M12,2A10,10 0 0,1 22,12A10,10 0 0,1 12,22A10,10 0 0,1 2,12A10,10 0 0,1 12,2M7,10L12,15L17,10H7Z" /></svg>\
                      </div>\
                    </summary>\
                    <div data-gjs-type="accordion-content" class="cls-content">\
                      <div>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</div>\
                    </div>\
                  </details>\
                </div>\
              </div>\
\
\
              <style>\
                body { font-family: system-ui; }\
                h1 { text-align: center }\
                .demo-container { padding: 2rem; }\
\
                .cls-accordion-group {\
                  display: flex;\
                  flex-direction: column;\
                  gap: 10px;\
                }\
                .cls-summary {\
                  cursor: pointer;\
                  display: flex;\
                  align-items: center;\
                  justify-content: space-between;\
                  background-color: #f9f9f9;\
                  padding: 10px;\
                  border: 1px solid #ddd;\
                  border-radius: 5px;\
                }\
                .cls-content {\
                  padding: 10px;\
                }\
              </style>\
              `,\
            }\
          ]
        }
      }


  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/components/accordion\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| block | [Block](https://grapesjs.com/docs/api/block.html) | Additional Block properties of the accordion component. Pass \`false\` to avoid adding the block.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "My label"<br>}<br>``` |
| blockGroup | [Block](https://grapesjs.com/docs/api/block.html) | Additional Block properties of the group accordion component. Pass \`false\` to avoid adding the block.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "My label"<br>}<br>``` |
| toolbarIconOpen | string | SVG toolbar button icon to toggle the accordion open state. You can pass an empty string to avoid adding the toolbar button.<br>**Example**<br>```codeBlockLines_AdAo<br>"<svg viewBox='0 0 24 24'><path d='M12 9a3 3 0..."<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/components/accordion#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_components_animation.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/components/animation"
title: "Animation | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/components/animation#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` |
| Plan | Startup plan |

This plugin adds scroll-triggered animation components using built-in or custom CSS animations. It supports grouping for staggered effects and provides editor controls to fine-tune playback timing.

![Animation plugin](https://app.grapesjs.com/docs-sdk/assets/images/animation-plugin-f099c7cb5c88ffd5ca7794cbfcd0d7af.webp)

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
import { animationComponent } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        animationComponent.init({\
          animations({ items }) {\
            return [\
              ...items, // Keep existing animations\
              // Add your custom animations here\
              {\
                id: 'customWiggle',\
                name: 'Custom Wiggle',\
                css: `\
                @keyframes customWiggle {\
                  0% { transform: rotate(0deg); }\
                  15% { transform: rotate(-5deg); }\
                  30% { transform: rotate(5deg); }\
                  45% { transform: rotate(-5deg); }\
                  60% { transform: rotate(3deg); }\
                  75% { transform: rotate(-2deg); }\
                  100% { transform: rotate(0deg); }\
                }`\
              }\
            ];\
          }\
        })\
      ],
      project: {
        default: {
          pages: [\
            {\
              name: 'Animation Demo',\
              component: `\
                <style>\
                  body {\
                    font-family: sans-serif;\
                    padding: 0;\
                    margin: 0;\
                  }\
                  section {\
                    padding: 50px 20px;\
                    text-align: center;\
                    border-bottom: 1px solid #eee;\
                  }\
                  h1 {\
                    font-size: 2rem;\
                    margin-bottom: 20px;\
                  }\
                  .animated-box {\
                    background: #f3f4f6;\
                    padding: 20px;\
                    margin: 10px auto;\
                    max-width: 400px;\
                    border-radius: 8px;\
                    box-shadow: 0 1px 4px rgba(0,0,0,0.05);\
                  }\
                  .animation-grid {\
                    display: grid;\
                    grid-template-columns: repeat(5, 1fr);\
                  }\
                </style>\
\
                <section style="min-height: 100vh">\
                  <h1>Animations</h1>\
                  <div class="animation-grid">\
                    <div data-gjs-type="animation" style="animation-name: flash" class="animated-box">flash</div>\
                    <div data-gjs-type="animation" style="animation-name: pulse" class="animated-box">pulse</div>\
                    <div data-gjs-type="animation" style="animation-name: shake" class="animated-box">shake</div>\
                    <div data-gjs-type="animation" style="animation-name: tada" class="animated-box">tada</div>\
                    <div data-gjs-type="animation" style="animation-name: heartBeat" class="animated-box">heartBeat</div>\
                    <div data-gjs-type="animation" style="animation-name: bounce" class="animated-box">bounce</div>\
                    <div data-gjs-type="animation" style="animation-name: bounceIn" class="animated-box">bounceIn</div>\
                    <div data-gjs-type="animation" style="animation-name: bounceInDown" class="animated-box">bounceInDown</div>\
                    <div data-gjs-type="animation" style="animation-name: bounceInLeft" class="animated-box">bounceInLeft</div>\
                    <div data-gjs-type="animation" style="animation-name: bounceInRight" class="animated-box">bounceInRight</div>\
                    <div data-gjs-type="animation" style="animation-name: bounceInUp" class="animated-box">bounceInUp</div>\
                    <div data-gjs-type="animation" style="animation-name: fadeIn" class="animated-box">fadeIn</div>\
                    <div data-gjs-type="animation" style="animation-name: fadeInDown" class="animated-box">fadeInDown</div>\
                    <div data-gjs-type="animation" style="animation-name: fadeInLeft" class="animated-box">fadeInLeft</div>\
                    <div data-gjs-type="animation" style="animation-name: fadeInRight" class="animated-box">fadeInRight</div>\
                    <div data-gjs-type="animation" style="animation-name: fadeInUp" class="animated-box">fadeInUp</div>\
                    <div data-gjs-type="animation" style="animation-name: flipInX" class="animated-box">flipInX</div>\
                    <div data-gjs-type="animation" style="animation-name: flipInY" class="animated-box">flipInY</div>\
                    <div data-gjs-type="animation" style="animation-name: rotateIn" class="animated-box">rotateIn</div>\
                    <div data-gjs-type="animation" style="animation-name: rotateInDownLeft" class="animated-box">rotateInDownLeft</div>\
                    <div data-gjs-type="animation" style="animation-name: rotateInDownRight" class="animated-box">rotateInDownRight</div>\
                    <div data-gjs-type="animation" style="animation-name: rotateInUpLeft" class="animated-box">rotateInUpLeft</div>\
                    <div data-gjs-type="animation" style="animation-name: rotateInUpRight" class="animated-box">rotateInUpRight</div>\
                    <div data-gjs-type="animation" style="animation-name: slideInDown" class="animated-box">slideInDown</div>\
                    <div data-gjs-type="animation" style="animation-name: slideInLeft" class="animated-box">slideInLeft</div>\
                    <div data-gjs-type="animation" style="animation-name: slideInRight" class="animated-box">slideInRight</div>\
                    <div data-gjs-type="animation" style="animation-name: slideInUp" class="animated-box">slideInUp</div>\
                    <div data-gjs-type="animation" style="animation-name: zoomIn" class="animated-box">zoomIn</div>\
                    <div data-gjs-type="animation" style="animation-name: zoomInDown" class="animated-box">zoomInDown</div>\
                    <div data-gjs-type="animation" style="animation-name: zoomInLeft" class="animated-box">zoomInLeft</div>\
                    <div data-gjs-type="animation" style="animation-name: zoomInRight" class="animated-box">zoomInRight</div>\
                    <div data-gjs-type="animation" style="animation-name: zoomInUp" class="animated-box">zoomInUp</div>\
                  </div>\
                </section>\
\
                <section>\
                  <h1>Scroll-triggered</h1>\
                  <div data-gjs-type="animation" style="animation-name: slideInLeft" class="animated-box">SlideInLeft</div>\
                  <div data-gjs-type="animation" style="animation-name: zoomIn; animation-delay: 0.5s" class="animated-box">ZoomIn with delay</div>\
                </section>\
\
                <section>\
                  <h1>Repeat on scroll</h1>\
                  <div data-gjs-type="animation" style="animation-name: slideInLeft; --animation-repeat: true" class="animated-box">SlideInLeft</div>\
                  <div data-gjs-type="animation" style="animation-name: zoomIn; --animation-repeat: true" class="animated-box">ZoomIn</div>\
                </section>\
\
                <section>\
                  <h1>Grouped Animations (Staggered)</h1>\
                  <div data-gjs-type="animation-group">\
                    <div data-gjs-type="animation" style="animation-name: fadeInDown" class="animated-box">Item 1</div>\
                    <div data-gjs-type="animation" style="animation-name: fadeInLeft" class="animated-box">Item 2</div>\
                    <div data-gjs-type="animation" style="animation-name: fadeInUp" class="animated-box">Item 3</div>\
                  </div>\
                </section>\
\
                <section>\
                  <h1>Grouped Animations (Repeat on scroll)</h1>\
                  <div data-gjs-type="animation-group" style="--animation-repeat: true">\
                    <div data-gjs-type="animation" style="animation-name: fadeIn" class="animated-box">Item 1</div>\
                    <div data-gjs-type="animation" style="animation-name: fadeIn" class="animated-box">Item 2</div>\
                    <div data-gjs-type="animation" style="animation-name: fadeIn" class="animated-box">Item 3</div>\
                  </div>\
                </section>\
\
                <section>\
                  <h1>Custom Animations</h1>\
                  <div data-gjs-type="animation" style="animation-name: customWiggle; animation-duration: 1.5s" class="animated-box">\
                    Custom "wiggle" animation\
                  </div>\
                </section>\
                `\
            }\
          ]
        }
      }


  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/components/animation\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| block | [Block](https://grapesjs.com/docs/api/block.html) | Additional Block properties of the animation component. Pass \`false\` to avoid adding the block.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "My label"<br>}<br>``` |
| blockGroup | [Block](https://grapesjs.com/docs/api/block.html) | Additional Block properties of the animation group component. Pass \`false\` to avoid adding the block.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "My label"<br>}<br>``` |
| animations | function | Extend or update animation list.<br>**Example** <br>```codeBlockLines_AdAo<br>animations: ({ items }) => {<br> return [<br>   // Remove bounce animations<br>   ...items.filter(item => !item.id.startsWith('bounce')),<br>   // Add custom animation<br>   {<br>     id: 'animationId',<br>     name: 'Custom animation',<br>     css: '@keyframes animationId { 0% { opacity: 0; } 100% { opacity: 1; } }',<br>   }<br> ]<br>}<br>``` |
| animationStyle | object | Initial style object of the animation component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "animation-duration": "0.5s"<br>}<br>``` |
| animationGroupStyle | object | Initial style object of the animation group component.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "--stagger-delay": "0.5s"<br>}<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/components/animation#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_components_dialog.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/components/dialog"
title: "Dialog | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/components/dialog#__docusaurus_skipToContent_fallback)

On this page

warning

This component is a work in progress.

Add a dialog component with additional functionalities.

![Dialog plugin](https://app.grapesjs.com/docs-sdk/assets/images/dialog-plugin-cd8fa2215a71133288aaa786cf730544.png)

Install the Studio SDK plugins package:

- npm
- pnpm
- yarn
- CDN

```codeBlockLines_AdAo
npm i @grapesjs/studio-sdk-plugins

```

To use the Dialog plugin, you need to import it into your project:

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { dialogComponent } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        dialogComponent.init({\
          block: { category: 'Extra', label: 'My Dialog' }\
        })\
      ]

  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/components/dialog\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| block | object | Block options for the dialog component. See https://grapesjs.com/docs/api/block.html#properties for more information.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "Popup"<br>}<br>``` |

### Component properties [​](https://app.grapesjs.com/docs-sdk/plugins/components/dialog\#component-properties "Direct link to Component properties")

| Property | Type | Description |
| --- | --- | --- |
| closeWhenPressingX\* | boolean | Whether the dialog should close when pressing the X button. |
| closeWhenPressingEsc\* | boolean | Whether the dialog should close when pressing the Escape key. |
| openWhenLeavingWindow\* | boolean | Whether the dialog should open when leaving the window. |
| openWhenScrollingToLevel\* | string | Whether the dialog should open when scrolling to a specific level.<br>**Example**<br>```codeBlockLines_AdAo<br>"500"<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/components/dialog#plugin-options)
- [Component properties](https://app.grapesjs.com/docs-sdk/plugins/components/dialog#component-properties)

---

# File: app.grapesjs.com_docs-sdk_plugins_components_flex.md

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

---

# File: app.grapesjs.com_docs-sdk_plugins_components_fslightbox.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/components/fslightbox"
title: "FsLightbox | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/components/fslightbox#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` |
| Plan | Startup plan |

Image viewer plugin based on [FsLightbox](https://fslightbox.com/).

![FsLightbox plugin](https://app.grapesjs.com/docs-sdk/assets/images/fslightbox-plugin-5d9d81980202b475e60ccaa248036915.png)

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
import { fsLightboxComponent } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        fsLightboxComponent?.init({\
          block: false // Skip default block\
        }),\
        // Add custom blocks for the lightbox\
        editor => {\
          editor.Blocks.add('lightbox-image', {\
            label: 'Image',\
            category: 'Lightbox examples',\
            media: '<svg viewBox="0 0 24 24"><path d="M5,14L8.5,9.5L11,12.5L14.5,8L19,14M20,2H4A2,2 0 0,0 2,4V22L6,18H20A2,2 0 0,0 22,16V4C22,2.89 21.1,2 20,2Z" /></svg>',\
            content: {\
              type: 'fslightbox',\
              attributes: { href: 'https://placehold.co/300/777/white.png?text=Image+LB+(Open)' },\
              components: `<h2>Image</h2><img src="https://placehold.co/300/777/white.png?text=Image+LB">`\
            }\
          });\
          editor.Blocks.add('lightbox-video', {\
            label: 'Video',\
            category: 'Lightbox examples',\
            media: '<svg viewBox="0 0 24 24"><path d="M18,14L14,10.8V14H6V6H14V9.2L18,6M20,2H4A2,2 0 0,0 2,4V22L6,18H20A2,2 0 0,0 22,16V4C22,2.89 21.1,2 20,2Z" /></svg>',\
            content: {\
              type: 'fslightbox',\
              'source-type': 'video',\
              attributes: { href: 'https://videos.pexels.com/video-files/15462514/15462514-uhd_2560_1440_30fps.mp4' },\
              components: `<h2>Video</h2><img src="https://images.pexels.com/videos/15462514/pexels-photo-15462514.jpeg?auto=compress&cs=tinysrgb&dpr=1&h=300">`\
            }\
          });\
          editor.Blocks.add('lightbox-el', {\
            label: 'Element',\
            category: 'Lightbox examples',\
            media: '<svg viewBox="0 0 24 24"><path d="M20,2H4A2,2 0 0,0 2,4V22L6,18H20A2,2 0 0,0 22,16V4A2,2 0 0,0 20,2M8,14H6V12H8V14M8,11H6V9H8V11M8,8H6V6H8V8M15,14H10V12H15V14M18,11H10V9H18V11M18,8H10V6H18V8Z" /></svg>',\
            content: () => { // Generate block with dynamic IDs\
              const randomId = Math.random().toString(36).substring(9);\
              const id = `el-id-${randomId}`;\
              return [\
                {\
                  type: 'fslightbox',\
                  'source-type': 'el',\
                  attributes: { href: `#${id}`, 'data-fslightbox': 'custom-elements' },\
                  components: `<h2>Custom element #${randomId}</h2>`\
                },\
                `<div style="display: none">\
                  <div id="${id}" class="custom-element">Custom content #${randomId}</div>\
                </div>`\
              ];\
            }\
          });\
        }\
      ],
      project: {
        default: {
          pages: [\
            {\
              name: 'Home',\
              component: `\
                <h1>Single images</h1>\
                <a data-fslightbox="image1" href="https://placehold.co/1500/777/white.png?text=Image+1+(Open)">\
                  <img src="https://placehold.co/200/777/white.png?text=Image+1">\
                </a>\
                <a data-fslightbox="image2" href="https://placehold.co/500/777/white.png?text=Image+2+(Open)">\
                  <img src="https://placehold.co/200/777/white.png?text=Image+2">\
                </a>\
                <a data-fslightbox="image3" href="https://placehold.co/500/777/white.png?text=Image+3+(Open)">\
                  <img src="https://placehold.co/200/777/white.png?text=Image+3">\
                </a>\
                <a data-fslightbox="image4" href="https://place-hold.it/500">\
                  <img src="https://place-hold.it/200">\
                </a>\
\
                <h1>Grouped images</h1>\
                <a data-fslightbox="image-group-1" href="https://placehold.co/500/777/white.png?text=Image+1+(Open)">\
                  <img src="https://placehold.co/200/777/white.png?text=Image+1">\
                </a>\
                <a data-fslightbox="image-group-1" href="https://placehold.co/500/777/white.png?text=Image+2+(Open)">\
                  <img src="https://placehold.co/200/777/white.png?text=Image+2">\
                </a>\
                <a data-fslightbox="image-group-1" href="https://placehold.co/500/777/white.png?text=Image+3+(Open)">\
                  <img src="https://placehold.co/200/777/white.png?text=Image+3">\
                </a>\
\
                <h1>Videos</h1>\
                <a data-fslightbox="videos" data-gjs-source-type="video" href="https://www.youtube.com/watch?v=kJyRVdnRwZA">\
                  <h2>YouTube video</h2>\
                  <img src="https://i.ytimg.com/vi/kJyRVdnRwZA/maxresdefault.jpg" style="height: 300px">\
                </a>\
                <a data-fslightbox="videos" data-gjs-source-type="video" href="https://videos.pexels.com/video-files/15462514/15462514-uhd_2560_1440_30fps.mp4">\
                  <h2>HTML video</h2>\
                  <img src="https://images.pexels.com/videos/15462514/pexels-photo-15462514.jpeg?auto=compress&cs=tinysrgb&dpr=1&h=300">\
                </a>\
\
                <h1>Custom types</h1>\
                <a data-fslightbox="custom-types" href="#vimeo-video">Vimeo Video (iframe)</a>\
                <a data-fslightbox="custom-types" href="#google-maps">Google Maps</a>\
                <a data-fslightbox="custom-types" href="#custom-element">Custom element</a>\
                <div style="display: none">\
                    INTENTIONALLY HIDDEN CONTAINER\
                    <iframe id="vimeo-video" data-gjs-type="default" src="https://player.vimeo.com/video/22439234" width="1200" height="600" frameBorder="0" allow="autoplay; fullscreen" allowFullScreen></iframe>\
                    <iframe id="google-maps" data-gjs-type="default" src="https://maps.google.com/maps?&amp;z=1&amp;t=q&amp;output=embed" frameborder="0" width="1200" height="600"></iframe>\
                    <div id="custom-element">\
                      <div class="custom-element">Custom content</div>\
                    </div>\
                </div>\
\
                <style>\
                  body {\
                    font-family: system-ui;\
                    text-align: center;\
                  }\
                  .custom-element {\
                    background: #8b5cf6;\
                    color: white;\
                    padding: 20px;\
                    text-align: center;\
                    width: 500px;\
                    height: 300px;\
                    border-radius: 10px;\
                    font-size: 2rem;\
                  }\
                </style>\
              `\
            },\
          ]
        }
      }


  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/components/fslightbox\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| block |  | Block options for the component. See https://grapesjs.com/docs/api/block.html#properties for more information.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "My label"<br>}<br>``` |
| toolbarIconOpen | string | SVG toolbar button icon for opening the lightbox. You can pass an empty string to avoid adding the toolbar button.<br>**Example**<br>```codeBlockLines_AdAo<br>"<svg viewBox=\"0 0 24 24\"><path d=\"M12 9a3 3 0..."<br>``` |
| defaultSrc | string | Default source for the lightbox component.<br>**Example**<br>```codeBlockLines_AdAo<br>"https://placehold.co/300/777/white.png?text=Image"<br>``` |
| cdnScript | string | CDN script to load dynamically in case the library is not available.<br>**Example**<br>```codeBlockLines_AdAo<br>"https://cdn.jsdelivr.net/npm/fslightbox@3.4.2/index.js"<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/components/fslightbox#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_components_iconify.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/components/iconify"
title: "Iconify | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/components/iconify#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` |
| Plan | Startup plan |

Add an icon component with the icon picker by levering the [Iconify collections](https://iconify.design/).

![Iconify plugin](https://app.grapesjs.com/docs-sdk/assets/images/iconify-plugin-265b0e0e9218b0d33744366e5a069bef.png)

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
import { iconifyComponent } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        iconifyComponent.init({\
          block: { category: 'Extra', label: 'Iconify' }\
        })\
      ],
      project: {
        default: {
          pages: [\
            {\
              name: 'Home',\
              component: `\
                <h2>Double click the icon below to show the icon picker</h2>\
                <div data-type-iconify>\
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path fill="currentColor" d="M12 2.5L8.42 8.06L2 9.74l4.2 5.14l-.38 6.62L12 19.09l6.18 2.41l-.38-6.62L22 9.74l-6.42-1.68zm-2.62 8c.62 0 1.12.5 1.12 1.13a1.12 1.12 0 0 1-1.12 1.12c-.63 0-1.13-.5-1.13-1.12c0-.63.5-1.13 1.13-1.13m5.25 0c.62 0 1.12.5 1.12 1.13a1.12 1.12 0 0 1-1.12 1.12c-.63 0-1.13-.5-1.13-1.12c0-.63.5-1.13 1.13-1.13M9 15h6a3.249 3.249 0 0 1-6 0"></path></svg\
                </div>\
              `,\
            }\
          ]
        }
      }

  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/components/iconify\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| block | [Block](https://grapesjs.com/docs/api/block.html) | Additional Block properties of the component. Pass \`false\` to avoid adding the block.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "My label"<br>}<br>``` |
| collections | array | List of icon collections to load.<br>**Example**<br>```codeBlockLines_AdAo<br>[<br>  "mdi",<br>  "fa-solid"<br>]<br>``` |
| extendIconComponent | boolean | Extend the icon picker trait to the default icon component.<br>**Default** <br>```codeBlockLines_AdAo<br>true<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/components/iconify#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_components_lightGallery.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/components/lightGallery"
title: "LightGallery | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/components/lightGallery#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` |
| Plan | Startup plan |

Gallery component based on [lightGallery](https://www.lightgalleryjs.com/).

![LightGallery plugin](https://app.grapesjs.com/docs-sdk/assets/images/lightgallery-plugin-0ef8d47f0aaa2bbdd0c83cffb9087fa4.png)

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
import { lightGalleryComponent } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        lightGalleryComponent?.init({\
          block: false // Skip default block\
        }),\
        // Add custom gallery blocks\
        editor => {\
          editor.Blocks.add('gallery-images', {\
            label: 'Gallery Images',\
            category: 'LightGallery examples',\
            media: '<svg viewBox="0 0 24 24"><path d="M5,14L8.5,9.5L11,12.5L14.5,8L19,14M20,2H4A2,2 0 0,0 2,4V22L6,18H20A2,2 0 0,0 22,16V4C22,2.89 21.1,2 20,2Z" /></svg>',\
            content: {\
              type: 'lightGallery',\
              components: [\
                {\
                  type: 'lightGallery-item',\
                  attributes: { href: 'https://placehold.co/1500/777/white.png?text=Image+1+(Open)' },\
                  components: { type: 'image', src: 'https://placehold.co/100/777/white.png?text=Image+1' }\
                },\
                {\
                  type: 'lightGallery-item',\
                  attributes: { href: 'https://placehold.co/1500/777/white.png?text=Image+2+(Open)' },\
                  components: { type: 'image', src: 'https://placehold.co/100/777/white.png?text=Image+2' }\
                }\
              ]\
            }\
          });\
          editor.Blocks.add('gallery-videos', {\
            label: 'Gallery Video',\
            category: 'LightGallery examples',\
            media: '<svg viewBox="0 0 24 24"><path d="M18,14L14,10.8V14H6V6H14V9.2L18,6M20,2H4A2,2 0 0,0 2,4V22L6,18H20A2,2 0 0,0 22,16V4C22,2.89 21.1,2 20,2Z" /></svg>',\
            content: {\
              type: 'lightGallery',\
              components: [\
                {\
                  type: 'lightGallery-item',\
                  attributes: { 'data-src': 'https://www.youtube.com/watch?v=EIUJfXk3_3w', 'data-sub-html': 'Video Caption 1' },\
                  components: { type: 'image', src: 'https://img.youtube.com/vi/EIUJfXk3_3w/maxresdefault.jpg', style: { width: '200px' } }\
                },\
                {\
                  type: 'lightGallery-item',\
                  attributes: { 'data-src': 'https://vimeo.com/112836958', 'data-sub-html': 'Video Caption 2' },\
                  components: { type: 'image', src: 'https://i.vimeocdn.com/video/506427660-2e93a42675715090a52de8e6645d592c5f58c1a7e388231d801072c9b2d9843d-d?mw=300', style: { width: '200px' } }\
                }\
              ]\
            }\
          });\
        },\
      ],
      project: {
        default: {
          pages: [\
            {\
              name: 'Home',\
              component: `\
                <h1>Single images</h1>\
                <div class="columns">\
                  <div data-lightgallery>\
                    <a href="https://placehold.co/1500/777/white.png?text=Image+1+(Open)">\
                        <img alt="Image 1" src="https://placehold.co/100/777/white.png?text=Image+1" />\
                    </a>\
                  </div>\
                  <div data-lightgallery>\
                    <a href="https://placehold.co/1500/777/white.png?text=Image+2+(Open)">\
                        <img src="https://placehold.co/100/777/white.png?text=Image+2" />\
                    </a>\
                  </div>\
                </div>\
\
                <h1>Multiple images</h1>\
                <div data-lightgallery class="columns">\
                  <a href="https://placehold.co/1500/777/white.png?text=Image+1+(Open)">\
                      <img alt="Image 1" src="https://placehold.co/100/777/white.png?text=Image+1" />\
                  </a>\
                  <a href="https://placehold.co/1500/777/white.png?text=Image+2+(Open)">\
                      <img alt="Image 2" src="https://placehold.co/100/777/white.png?text=Image+2" />\
                  </a>\
                  <a href="https://placehold.co/1500/777/white.png?text=Image+3+(Open)">\
                      <img alt="Image 3" src="https://placehold.co/100/777/white.png?text=Image+3" />\
                  </a>\
                </div>\
\
                <h1>Video gallery</h1>\
                <div data-lightgallery class="columns">\
                  <a data-src="https://www.youtube.com/watch?v=EIUJfXk3_3w" data-poster="https://img.youtube.com/vi/EIUJfXk3_3w/maxresdefault.jpg" data-sub-html="Puffin Hunts Fish To Feed Puffling">\
                      <h2>YouTube video</h2>\
                      <img width="200" src="https://img.youtube.com/vi/EIUJfXk3_3w/maxresdefault.jpg"/>\
                  </a>\
                  <a data-src="https://vimeo.com/112836958" data-poster="https://i.vimeocdn.com/video/506427660-2e93a42675715090a52de8e6645d592c5f58c1a7e388231d801072c9b2d9843d-d?mw=2880&q=70" data-sub-html="Nature">\
                      <h2>Vimeo video</h2>\
                      <img width="200" src="https://i.vimeocdn.com/video/506427660-2e93a42675715090a52de8e6645d592c5f58c1a7e388231d801072c9b2d9843d-d?mw=300"/>\
                  </a>\
                  <a data-src="https://private-sharing.wistia.com/medias/mwhrulrucj" data-poster="https://embed-ssl.wistia.com/deliveries/dfcb95bfd3e8cb39022e8d195e24931c2117ef05.webp?image_crop_resized=300x200" data-sub-html="Sample Wistia video">\
                      <h2>Wistia video</h2>\
                      <img width="200" src="https://embed-ssl.wistia.com/deliveries/dfcb95bfd3e8cb39022e8d195e24931c2117ef05.webp?image_crop_resized=224x126"/>\
                  </a>\
                </div>\
\
                <h1>Iframe gallery</h1>\
                <div data-lightgallery class="columns">\
                  <a data-iframe="true" data-src="https://example.com/" data-sub-html="Website Caption">\
                      <img src="https://placehold.co/100/777/white.png?text=Website" />\
                  </a>\
                  <a data-iframe="true" data-src="https://www.google.com/maps/embed" data-sub-html="Google Maps Caption">\
                      <img src="https://placehold.co/100/777/white.png?text=Google+Maps" />\
                  </a>\
                  <a data-iframe="true" data-src="https://pdfobject.com/pdf/sample.pdf" data-sub-html="PDF Caption">\
                      <img src="https://placehold.co/100/777/white.png?text=PDF" />\
                  </a>\
                </div>\
\
                <h1>Inline Gallery</h1>\
                <div data-lightgallery class="inline-gallery" data-gjs-inline="true" data-gjs-download="false">\
                  <a href="https://placehold.co/1500/777/white.png?text=Image+1+(Open)">\
                      <img alt="Image 1" src="https://placehold.co/100/777/white.png?text=Image+1" />\
                  </a>\
                  <a href="https://placehold.co/1500/777/white.png?text=Image+2+(Open)">\
                      <img alt="Image 2" src="https://placehold.co/100/777/white.png?text=Image+2" />\
                  </a>\
                  <a href="https://placehold.co/1500/777/white.png?text=Image+3+(Open)">\
                      <img alt="Image 3" src="https://placehold.co/100/777/white.png?text=Image+3" />\
                  </a>\
                </div>\
\
                <h1>Custom item selector</h1>\
                <div data-lightgallery class="columns" data-gjs-selector="[data-lightgallery-item]">\
                  <div class="custom-wrapper">\
                    <a data-lightgallery-item href="https://placehold.co/1500/777/white.png?text=Image+1+(Open)">\
                        <img alt="Image 1" src="https://placehold.co/100/777/white.png?text=Image+1" />\
                    </a>\
                  </div>\
                  <div class="custom-wrapper">\
                    <a data-lightgallery-item href="https://placehold.co/1500/777/white.png?text=Image+2+(Open)">\
                        <img alt="Image 2" src="https://placehold.co/100/777/white.png?text=Image+2" />\
                    </a>\
                  </div>\
                </div>\
\
                <style>\
                  body {\
                    font-family: system-ui;\
                    text-align: center;\
                  }\
                  .columns {\
                    display: flex;\
                    justify-content: center;\
                    gap: 10px;\
                  }\
                  .inline-gallery {\
                    display: flex;\
                    justify-content: center;\
                    gap: 10px;\
                    height: 500px;\
                  }\
                  .custom-wrapper {\
                    border: 2px solid;\
                    padding: 10px;\
                    border-radius: 10px;\
                  }\
                  [data-lightgallery] {\
                    border: 2px solid #333;\
                    position: relative;\
                    padding: 10px;\
                  }\
                  [data-lightgallery]:after {\
                    content: "lightGallery Example";\
                    position: absolute;\
                    top: 0;\
                    background-color: #333;\
                    color: white;\
                    left: -2px;\
                    font-size: 0.5rem;\
                    padding: 3px;\
                    transform: translateY(-100%);\
                  }\
                </style>\
              `\
            },\
          ]
        }
      }


  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/components/lightGallery\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| block |  | Block options for the component. See https://grapesjs.com/docs/api/block.html#properties for more information.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "My label"<br>}<br>``` |
| lgLicenseKey | string | This plugin includes a commercial license when used within the Studio SDK. If you're using it outside the SDK (core GrapesJS) in a commercial project, you must obtain a valid license separately. Learn more about the lightGallery license key here: https://www.lightgalleryjs.com/license/ |
| toolbarIconOpen | string | SVG toolbar button icon for opening the gallery. You can pass an empty string to avoid adding the toolbar button.<br>**Example**<br>```codeBlockLines_AdAo<br>"<svg viewBox=\"0 0 24 24\"><path d=\"M12 9a3 3 0..."<br>``` |
| defaultSrc | string | Default source for the inner image component.<br>**Default** <br>```codeBlockLines_AdAo<br>https://placehold.co/200/777/white.png?text=Image<br>``` |
| cdnScript |  | CDN scripts to load dynamically in case the library is not available.<br>**Default**<br>```codeBlockLines_AdAo<br>[<br>  "https://cdn.jsdelivr.net/npm/lightgallery@2.8.2/lightgallery.min.js",<br>  "https://cdn.jsdelivr.net/npm/lightgallery@2.8.2/plugins/thumbnail/lg-thumbnail.min.js",<br>  "https://cdn.jsdelivr.net/npm/lightgallery@2.8.2/plugins/video/lg-video.min.js",<br>  "https://cdn.jsdelivr.net/npm/lightgallery@2.8.2/plugins/autoplay/lg-autoplay.min.js",<br>  "https://cdn.jsdelivr.net/npm/lightgallery@2.8.2/plugins/zoom/lg-zoom.min.j"<br>]<br>``` |
| cdnStyle |  | CDN styles to load dynamically.<br>**Default** <br>```codeBlockLines_AdAo<br>https://cdn.jsdelivr.net/npm/lightgallery@2.8.2/css/lightgallery-bundle.min.css<br>``` |
| plugins | array | Plugins to include. You have to ensure to load the required plugins in the \`cdnScript\` option. Check the list of available plugins here: https://www.lightgalleryjs.com/docs/getting-started/#plugins<br>**Default**<br>```codeBlockLines_AdAo<br>[<br>  "lgThumbnail",<br>  "lgVideo",<br>  "lgAutoplay",<br>  "lgZoom"<br>]<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/components/lightGallery#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_components_listPages.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/components/listPages"
title: "List Pages | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/components/listPages#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` |
| Plan | Startup plan |

Add a list component that dynamically generates navigation links based on the pages in your project.

![List Pages plugin](https://app.grapesjs.com/docs-sdk/assets/images/list-pages-plugin-a315be106d0897f4167a6c93c71e7878.png)

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
import { listPagesComponent } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        listPagesComponent?.init({\
          block: { category: 'Extra', label: 'My List Pages' }\
        })\
      ],
      project: {
        default: {
          pages: [\
            {\
              id: 'id-home-page',\
              name: 'Home',\
              component: `\
                <h1>Auto-generated</h1>\
                <ul data-gjs-type="list-pages"></ul>\
\
                <h1>Statically defined, with custom styles</h1>\
                <ul class="list-pages" data-gjs-type="list-pages">\
                  <li data-gjs-type="list-pages-item">\
                    <a data-gjs-type="list-pages-link" href="page://id-home-page">\
                      Home\
                    </a>\
                  </li>\
                  <li data-gjs-type="list-pages-item">\
                    <a data-gjs-type="list-pages-link" href="page://id-about-page">\
                      About\
                    </a>\
                  </li>\
                  <li data-gjs-type="list-pages-item">\
                    <a data-gjs-type="list-pages-link" href="page://id-contact-page">\
                      Contact FIX ME\
                    </a>\
                  </li>\
                </ul>\
\
                <style>\
                  body {\
                    font-family: system-ui;\
                  }\
                  .list-pages {\
                    list-style: none;\
                    margin: 0;\
                    padding: 0;\
                    display: flex;\
                    gap: 1rem;\
                  }\
                  .list-pages a {\
                    display: block;\
                    color: red;\
                    text-decoration: none;\
                    border: 1px solid;\
                    padding: 0.5rem 1rem;\
                    border-radius: 1rem;\
                  }\
                </style>\
              `\
            },\
            { id: 'id-about-page', name: 'About', component: '<h1>About page</h1>' },\
            { id: 'id-contact-page', name: 'Contact', component: '<h1>Contact page</h1>' }\
          ]
        }
      }


  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/components/listPages\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| block | object | Block options for the component. See https://grapesjs.com/docs/api/block.html#properties for more information.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "My label"<br>}<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/components/listPages#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_components_swiper.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/components/swiper"
title: "Swiper | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/components/swiper#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` |
| Plan | Startup plan |

Slider component plugin based on [Swiper](https://swiperjs.com/).

![Swiper plugin](https://app.grapesjs.com/docs-sdk/assets/images/swiper-plugin-ded7740b473376f249829eced9003fee.png)

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
import { swiperComponent } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        swiperComponent?.init({\
          block: false // Skip default block\
        }),\
        // Add custom blocks for the swiper\
        editor => {\
          editor.Blocks.add('swiper', {\
            label: 'Swiper Slider',\
            category: 'Swiper example',\
            media: '<svg viewBox="0 0 24 24"><path d="M22 7.6c0-1-.5-1.6-1.3-1.6H3.4C2.5 6 2 6.7 2 7.6v9.8c0 1 .5 1.6 1.3 1.6h17.4c.8 0 1.3-.6 1.3-1.6V7.6zM21 18H3V7h18v11z" fill-rule="nonzero"/><path d="M4 12.5L6 14v-3zM20 12.5L18 14v-3z"/></svg>',\
            content: `<div class="swiper" style="height: 200px">\
              <div class="swiper-wrapper">\
                <div class="swiper-slide"><div>Slide 1</div></div>\
                <div class="swiper-slide"><div>Slide 2</div></div>\
                <div class="swiper-slide"><div>Slide 3</div></div>\
              </div>\
              <div class="swiper-button-next"></div>\
              <div class="swiper-button-prev"></div>\
            </div>`\
          });\
        }\
      ],
      project: {
        default: {
          pages: [\
            {\
              name: 'Home',\
              component: `\
                <h1>Default</h1>\
                <div class="swiper" style="height: 200px">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide">Slide 1</div>\
                    <div class="swiper-slide">Slide 2</div>\
                    <div class="swiper-slide">Slide 3</div>\
                  </div>\
                </div>\
\
                <h1>Navigation</h1>\
                <div class="swiper" style="height: 200px">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide">Slide 1</div>\
                    <div class="swiper-slide">Slide 2</div>\
                    <div class="swiper-slide">Slide 3</div>\
                  </div>\
                  <div class="swiper-button-next"></div>\
                  <div class="swiper-button-prev"></div>\
                </div>\
\
                <h1>Pagination</h1>\
                <div class="swiper" style="height: 200px">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide">Slide 1</div>\
                    <div class="swiper-slide">Slide 2</div>\
                    <div class="swiper-slide">Slide 3</div>\
                  </div>\
                  <div class="swiper-pagination"></div>\
                </div>\
\
                <h1>Scrollbar</h1>\
                <div class="swiper" style="height: 200px">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide">Slide 1</div>\
                    <div class="swiper-slide">Slide 2</div>\
                    <div class="swiper-slide">Slide 3</div>\
                  </div>\
                  <div class="swiper-scrollbar"></div>\
                </div>\
\
                <h1>Vertical</h1>\
                <div class="swiper" style="height: 200px" data-gjs-vertical="true">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide">Slide 1</div>\
                    <div class="swiper-slide">Slide 2</div>\
                    <div class="swiper-slide">Slide 3</div>\
                  </div>\
                  <div class="swiper-pagination"></div>\
                </div>\
\
                <h1>Autoplay</h1>\
                <div class="swiper" style="height: 200px" data-gjs-autoplay="true">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide">Slide 1</div>\
                    <div class="swiper-slide">Slide 2</div>\
                    <div class="swiper-slide">Slide 3</div>\
                  </div>\
                  <div class="swiper-pagination"></div>\
                </div>\
\
                <h1>Space between</h1>\
                <div class="swiper" style="height: 200px" data-gjs-space-between="100">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide swiper-slide-bg">Slide 1</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 2</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 3</div>\
                  </div>\
                  <div class="swiper-pagination"></div>\
                </div>\
\
                <h1>Slides per view</h1>\
                <div class="swiper" style="height: 200px" data-gjs-space-between="30" data-gjs-slides-per-view="2">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide swiper-slide-bg">Slide 1</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 2</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 3</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 4</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 5</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 6</div>\
                  </div>\
                  <div class="swiper-pagination"></div>\
                </div>\
\
                <h1>Slides per group</h1>\
                <div class="swiper" style="height: 200px" data-gjs-space-between="30" data-gjs-slides-per-view="2" data-gjs-slides-per-group="2">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide swiper-slide-bg">Slide 1</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 2</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 3</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 4</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 5</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 6</div>\
                  </div>\
                  <div class="swiper-pagination"></div>\
                </div>\
\
                <h1>Freemode</h1>\
                <div class="swiper" style="height: 200px" data-gjs-space-between="30" data-gjs-slides-per-view="3" data-gjs-free-mode="true">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide swiper-slide-bg">Slide 1</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 2</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 3</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 4</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 5</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 6</div>\
                  </div>\
                </div>\
\
                <h1>Smooth Autoplay</h1>\
                <div class="swiper" style="height: 200px"\
                  data-gjs-loop="true"\
                  data-gjs-speed="5000"\
                  data-gjs-slides-per-view="3.5"\
                  data-gjs-space-between="50"\
                  data-gjs-autoplay="true"\
                  data-gjs-autoplay-delay="0"\
                  data-gjs-autoplay-disable-on-interaction="false"\
                  data-gjs-allow-touch-move="false"\
                >\
                  <div class="swiper-wrapper" style="transition-timing-function: linear">\
                    <div class="swiper-slide swiper-slide-bg">Slide 1</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 2</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 3</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 4</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 5</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 6</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 7</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 8</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 9</div>\
                  </div>\
                </div>\
\
                <h1>Parallax</h1>\
                <div class="swiper" style="height: 300px; text-align: left; padding: 50px;" data-gjs-parallax="true" data-gjs-speed="1000">\
                  <div class="parallax-bg" style="background-image: url(https://placehold.co/800x400/eee/777.png?text=Image+Parallax)" data-swiper-parallax="-20%"></div>\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide">\
                      <h1 data-swiper-parallax="-300">Slide 1</h1>\
                      <h2 data-swiper-parallax="-200">Subtitle</h2>\
                      <div data-swiper-parallax="-100">Some text content</div>\
                    </div>\
                    <div class="swiper-slide">\
                      <h1 data-swiper-parallax="-300">Slide 2</h1>\
                      <h2 data-swiper-parallax="-200">Subtitle</h2>\
                      <div data-swiper-parallax="-100">Some text content</div>\
                    </div>\
                    <div class="swiper-slide">\
                      <h1 data-swiper-parallax="-300">Slide 3</h1>\
                      <h2 data-swiper-parallax="-200">Subtitle</h2>\
                      <div data-swiper-parallax="-100">Some text content</div>\
                    </div>\
                  </div>\
                  <div class="swiper-button-next"></div>\
                  <div class="swiper-button-prev"></div>\
                </div>\
\
                <h1>Autoheight</h1>\
                <div class="swiper" data-gjs-space-between="30" data-gjs-auto-height="true">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide swiper-slide-bg" style="height: 200px">Slide 1</div>\
                    <div class="swiper-slide swiper-slide-bg" style="height: 300px">Slide 2</div>\
                    <div class="swiper-slide swiper-slide-bg" style="height: 400px">Slide 3</div>\
                  </div>\
                  <div class="swiper-button-next"></div>\
                  <div class="swiper-button-prev"></div>\
                </div>\
\
                <h1>Effect Fade</h1>\
                <div class="swiper" style="height: 400px" data-gjs-effect="fade">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide">\
                      <img class="slide-img" src="https://placehold.co/800x400/777/white.png?text=Image+1" />\
                    </div>\
                    <div class="swiper-slide">\
                      <img class="slide-img" src="https://placehold.co/800x400/777/white.png?text=Image+2" />\
                    </div>\
                    <div class="swiper-slide">\
                      <img class="slide-img" src="https://placehold.co/800x400/777/white.png?text=Image+3" />\
                    </div>\
                  </div>\
                  <div class="swiper-button-next"></div>\
                  <div class="swiper-button-prev"></div>\
                  <div class="swiper-pagination"></div>\
                </div>\
\
                <h1>Effect Cube</h1>\
                <div class="swiper" style="height: 400px; width: 400px; margin: 50px auto;" data-gjs-effect="cube">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide">\
                      <img src="https://placehold.co/400x400/777/white.png?text=Image+1" />\
                    </div>\
                    <div class="swiper-slide">\
                      <img src="https://placehold.co/400x400/777/white.png?text=Image+2" />\
                    </div>\
                    <div class="swiper-slide">\
                      <img src="https://placehold.co/400x400/777/white.png?text=Image+3" />\
                    </div>\
                  </div>\
                  <div class="swiper-button-next"></div>\
                  <div class="swiper-button-prev"></div>\
                  <div class="swiper-pagination"></div>\
                </div>\
\
                <h1>Effect Coverflow</h1>\
                <div\
                  class="swiper swiper-coverflow" style="height: 400px"\
                  data-gjs-effect="coverflow"\
                  data-gjs-slides-per-view="auto"\
                  data-gjs-centered-slides="true"\
                >\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide">\
                      <img class="slide-img" src="https://placehold.co/400x400/777/white.png?text=Image+1" />\
                    </div>\
                    <div class="swiper-slide">\
                      <img class="slide-img" src="https://placehold.co/400x400/777/white.png?text=Image+2" />\
                    </div>\
                    <div class="swiper-slide">\
                      <img class="slide-img" src="https://placehold.co/400x400/777/white.png?text=Image+3" />\
                    </div>\
                  </div>\
                  <div class="swiper-pagination"></div>\
                </div>\
\
                <h1>Effect Flip</h1>\
                <div class="swiper swiper-flip" style="height: 300px; width: 300px; padding: 50px; margin: 25px auto;" data-gjs-effect="flip">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide">\
                      <img class="slide-img" src="https://placehold.co/400x400/777/white.png?text=Image+1" />\
                    </div>\
                    <div class="swiper-slide">\
                      <img class="slide-img" src="https://placehold.co/400x400/777/white.png?text=Image+2" />\
                    </div>\
                    <div class="swiper-slide">\
                      <img class="slide-img" src="https://placehold.co/400x400/777/white.png?text=Image+3" />\
                    </div>\
                  </div>\
                  <div class="swiper-button-next"></div>\
                  <div class="swiper-button-prev"></div>\
                  <div class="swiper-pagination"></div>\
                </div>\
\
                <h1>Effect Cards</h1>\
                <div class="swiper" style="height: 300px; width: 300px; margin: 0 auto;" data-gjs-effect="cards">\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide">\
                      <img class="slide-img" src="https://placehold.co/400x400/777/white.png?text=Image+1" />\
                    </div>\
                    <div class="swiper-slide">\
                      <img class="slide-img" src="https://placehold.co/400x400/777/white.png?text=Image+2" />\
                    </div>\
                    <div class="swiper-slide">\
                      <img class="slide-img" src="https://placehold.co/400x400/777/white.png?text=Image+3" />\
                    </div>\
                  </div>\
                </div>\
\
                <h1>Responsive</h1>\
                <div class="swiper" style="height: 200px; padding: 50px"\
                  data-gjs-space-between="10"\
                  data-gjs-slides-per-view="1"\
                  data-gjs-mobile="true"\
                  data-gjs-mobile-slides-per-view="2"\
                  data-gjs-mobile-space-between="20"\
                  data-gjs-tablet="true"\
                  data-gjs-tablet-slides-per-view="3"\
                  data-gjs-tablet-space-between="30"\
                >\
                  <div class="swiper-wrapper">\
                    <div class="swiper-slide swiper-slide-bg">Slide 1</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 2</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 3</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 4</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 5</div>\
                    <div class="swiper-slide swiper-slide-bg">Slide 6</div>\
                  </div>\
                  <div class="swiper-button-next"></div>\
                  <div class="swiper-button-prev"></div>\
                  <div class="swiper-pagination"></div>\
                </div>\
\
                <style>\
                  body {\
                    font-family: system-ui;\
                    text-align: center;\
                  }\
                  .columns {\
                    display: flex;\
                    justify-content: center;\
                    gap: 10px;\
                  }\
                  .slide-img {\
                    width: 100%;\
                    object-fit: cover;\
                    height: 100%;\
                  }\
                  .parallax-bg {\
                    position: absolute;\
                    left: 0;\
                    top: 0;\
                    width: 130%;\
                    height: 100%;\
                    background-size: cover;\
                    background-position: center;\
                  }\
                  .swiper-slide-bg {\
                    background: #f1f1f1\
                  }\
                  .swiper-coverflow .swiper-slide,\
                  .swiper-flip .swiper-slide {\
                    width: 50%;\
                  }\
                  .swiper {\
                    border: 2px solid #333;\
                    margin: 50px;\
                  }\
                  .swiper:after {\
                    content: "Swiper Example";\
                    position: absolute;\
                    top: 0;\
                    background-color: #333;\
                    color: white;\
                    left: 0;\
                    font-size: 0.5rem;\
                    padding: 3px;\
                  }\
                </style>\
                `\
            },\
          ]
        }
      }


  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/components/swiper\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| block | [Block](https://grapesjs.com/docs/api/block.html) | Additional Block properties of the component. Pass \`false\` to avoid adding the block.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "My label"<br>}<br>``` |
| cdnScript | string | CDN scripts to load dynamically in case the library is not available.<br>**Default** <br>```codeBlockLines_AdAo<br>https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.js<br>``` |
| cdnStyle | string | CDN styles to load dynamically.<br>**Default** <br>```codeBlockLines_AdAo<br>https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.css<br>``` |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/components/swiper#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_components_table_sitemap.xml.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/components/table/sitemap.xml"
title: "GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/components/table/sitemap.xml#__docusaurus_skipToContent_fallback)

# Page Not Found

We could not find what you were looking for.

Please contact the owner of the site that linked you to the original URL and let them know their link is broken.

---

# File: app.grapesjs.com_docs-sdk_plugins_components_table.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/components/table"
title: "Table | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/components/table#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` |
| Plan | Startup plan |

Add table component with additional functionalities.

![Table plugin](https://app.grapesjs.com/docs-sdk/assets/images/table-plugin-f8c7596df01f9e12fb5d94a9de4ee502.png)

Install the Studio SDK plugins package:

- npm
- pnpm
- yarn
- CDN

```codeBlockLines_AdAo
npm i @grapesjs/studio-sdk-plugins

```

Import and use the table plugin in your project:

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { tableComponent } from "@grapesjs/studio-sdk-plugins";
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        tableComponent.init({\
          block: { category: 'Extra', label: 'My Table' }\
        })\
      ],
      project: {
        default: {
          pages: [\
            {\
              name: 'Home',\
              component: `\
                <h1>Table plugin</h1>\
                <table>\
                  <tbody>\
                    <tr>\
                      <td>Cell 0:0</td>\
                      <td></td>\
                    </tr>\
                    <tr>\
                      <td></td>\
                      <td>Cell 1:1</td>\
                    </tr>\
                  </tbody>\
                </table>\
            `},\
          ]
        },
      }

  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/components/table\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| block |  | Block options for the table component. See https://grapesjs.com/docs/api/block.html#properties for more information.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "My Table"<br>}<br>``` |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| openSettings | function | Customize the layout for table component setting actions, which by default are shown in a popover.<br>**Example**<br>```codeBlockLines_AdAo<br>({ editor, layoutProps }) => {<br>  // open settings in a dialog<br>  editor.runCommand('studio:layoutToggle', {<br>    ...layoutProps,<br>    header: false,<br>    style: { marginLeft: -20, marginRight: -20 },<br>    placer: { type: 'dialog', title: layoutProps.header?.label },<br>  });<br>}<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/components/table#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_custom-renderer_react.md

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

---

# File: app.grapesjs.com_docs-sdk_plugins_data-sources_ejs.md

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

---

# File: app.grapesjs.com_docs-sdk_plugins_data-sources_handlebars.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/data-sources/handlebars"
title: "Handlebars | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/data-sources/handlebars#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` `email` |
| Plan | Startup plan |

This plugin enables importing and exporting your [Data Sources](https://app.grapesjs.com/docs-sdk/configuration/datasources/overview) records using the [Handlebars](https://handlebarsjs.com/) template engine.

![Handlebars plugin](https://app.grapesjs.com/docs-sdk/assets/images/handlebars-plugin-49143c2d1dd498e102104131bc3de23e.webp)

Install the Studio SDK plugins package:

- npm
- pnpm
- yarn
- CDN

```codeBlockLines_AdAo
npm i @grapesjs/studio-sdk-plugins

```

The example below demonstrates how the editor can detect and extract Handlebars expressions from the content, and how it preserves them when exporting.

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { dataSourceHandlebars } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
    // ...
    plugins: [\
      dataSourceHandlebars\
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
              <div>Hello {{ globalData.user.data.firstName }}</div>\
\
              <h1>Condition</h1>\
              <div>\
                Hey, {{#if globalData.user.data.isCustomer }}\
                  welcome back {{ globalData.user.data.firstName }}!\
                {{else}}\
                  please register to see more!\
                {{/if}}\
              </div>\
\
              <h1>Collection</h1>\
\
              {{#each globalData.products.data }}\
                <div>\
                  <b>Product Name</b>: {{ this.name }} - <b>Price</b>: {{ this.price }}\
                </div>\
              {{/each}}\
            `,\
          }\
        ]
      },

    },

  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/data-sources/handlebars\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/data-sources/handlebars#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_layout_sidebar-buttons.md

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

---

# File: app.grapesjs.com_docs-sdk_plugins_overview.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/overview"
title: "Plugins | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/overview#__docusaurus_skipToContent_fallback)

On this page

In this section, you will find a variety of example plugins that illustrate how to effectively use them within the SDK. These examples are designed to help you get started quickly and easily.

## What is a Plugin? [​](https://app.grapesjs.com/docs-sdk/plugins/overview\#what-is-a-plugin "Direct link to What is a Plugin?")

A plugin is an extension that enhances the core functionality of the editor through functions that are run when the editor is initialized. Plugins allow you to introduce new features, modify existing behaviors, or integrate external libraries without altering the core logic.

By using plugins, developers can customize the canvas to fit specific use cases, whether it's adding new components, or extending the editor's capabilities.

## Why use Plugins? [​](https://app.grapesjs.com/docs-sdk/plugins/overview\#why-use-plugins "Direct link to Why use Plugins?")

Plugins are essential for keeping the editor modular, scalable, and adaptable to different needs. Here's why you should use plugins:

- **Customization**: Extend the platform with tailored functionality for your project.
- **Reusability**: Create reusable features without modifying the core editor.
- **Integration**: Easily connect external libraries or APIs.
- **Maintainability**: Keep core updates separate from custom modifications.

- [What is a Plugin?](https://app.grapesjs.com/docs-sdk/plugins/overview#what-is-a-plugin)
- [Why use Plugins?](https://app.grapesjs.com/docs-sdk/plugins/overview#why-use-plugins)

---

# File: app.grapesjs.com_docs-sdk_plugins_preset_printable.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/preset/printable"
title: "Printable | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/preset/printable#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `document` |
| Plan | Free plan |

This preset plugin is designed to help you build HTML content that's consistent and optimized for printing. It includes default printable device sizes (like A4, A5, etc.), additional components (eg. pageBreak component) and provides utility commands such as triggering the browser's print preview across all pages.

![Printable plugin](https://app.grapesjs.com/docs-sdk/assets/images/printable-plugin-1053616058e037d32b8ddf9aa8935ae6.webp)

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
import { presetPrintable, canvasFullSize } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        presetPrintable,\
        canvasFullSize, // Optional\
      ],
      project: {
        type: 'document', // Ensure the project type is set to 'document'
        default: {
          pages: [\
            {\
              name: 'Invoice',\
              component: `<!DOCTYPE html>\
                <html>\
                  <body style="padding: 40px; font-family: Arial, Helvetica, sans-serif">\
                    <header class="invoice-header flex-between mb-40">\
                      <div>\
                        <h1>INVOICE</h1>\
                        <p class="mb-4">#INV-00123</p>\
                        <p class="mb-4">Date: 2025-05-28</p>\
                      </div>\
                      <div style="line-height: 1.5rem">\
                        <strong>Your Company</strong><br />\
                        123 Business Street<br />\
                        City, Country\
                      </div>\
                    </header>\
\
                    <section class="mb-40">\
                      <h3 class="mb-10">Bill To:</h3>\
                      <div style="line-height: 1.5rem">\
                        Client Name<br/>\
                        Client Company<br/>\
                        456 Client Road<br/>\
                        City, Country<br/>\
                      </div>\
                    </section>\
\
                    <table class="invoice-table">\
                      <thead>\
                        <tr>\
                          <th>Description</th>\
                          <th>Qty</th>\
                          <th>Unit Price</th>\
                          <th>Total</th>\
                        </tr>\
                      </thead>\
                      <tbody>\
                        <tr>\
                          <td>Design Services</td>\
                          <td>10</td>\
                          <td>$50</td>\
                          <td>$500</td>\
                        </tr>\
                        <tr>\
                          <td>Hosting (3 months)</td>\
                          <td>1</td>\
                          <td>$150</td>\
                          <td>$150</td>\
                        </tr>\
                      </tbody>\
                    </table>\
\
                    <section class="total-section mb-40" style="text-align: right;">\
                      <p class="mb-4"><strong>Subtotal:</strong> $650</p>\
                      <p class="mb-4"><strong>Tax (10%):</strong> $65</p>\
                      <p class="mb-4" style="font-size: 18px;"><strong>Total:</strong> $715</p>\
                    </section>\
\
                    <footer class="invoice-footer" style="text-align: center;">\
                      Thank you for your business!<br />\
                      Payment is due within 15 days.\
                    </footer>\
                  </body>\
                  <style>\
                    .flex-between {\
                      display: flex;\
                      justify-content: space-between;\
                      align-items: center;\
                    }\
\
                    .mb-40 { margin-bottom: 40px; }\
                    .mb-10 { margin-bottom: 10px; }\
                    .mb-4 { margin-bottom: 4px; }\
\
                    .invoice-header h1 {\
                      margin: 0;\
                    }\
\
                    .invoice-table {\
                      width: 100%;\
                      border-collapse: collapse;\
                      margin-bottom: 40px;\
                    }\
\
                    .invoice-table th,\
                    .invoice-table td {\
                      padding: 12px;\
                      border: 1px solid #ddd;\
                      text-align: right;\
                    }\
\
                    .invoice-table th:first-child,\
                    .invoice-table td:first-child {\
                      text-align: left;\
                    }\
\
                    .invoice-table thead {\
                      background-color: #f5f5f5;\
                    }\
\
                    .invoice-footer {\
                      font-size: 12px;\
                      color: #888;\
                    }\
                  </style>\
                <html>\
              `,\
            }\
          ]
        }
      },
      // Custom layout for demo purposes
      layout: {
        default: {
          type: 'row',
          height: '100%',
          children: [\
            {\
              type: 'sidebarLeft',\
              children: { type: 'panelLayers', header: { label: 'Layers', collapsible: false, icon: 'layers' } }\
            },\
            {\
              type: 'canvasSidebarTop',\
              sidebarTop: {\
                rightContainer: {\
                  buttons: ({ items }) => [\
                    {\
                      id: 'print',\
                      icon: '<svg viewBox="0 0 24 24"><path d="M18 3H6v4h12m1 5a1 1 0 0 1-1-1 1 1 0 0 1 1-1 1 1 0 0 1 1 1 1 1 0 0 1-1 1m-3 7H8v-5h8m3-6H5a3 3 0 0 0-3 3v6h4v4h12v-4h4v-6a3 3 0 0 0-3-3Z"/></svg>',\
                      onClick: ({ editor }) => editor.runCommand('presetPrintable:print')\
                    },\
                    ...items.filter(item => !['showImportCode', 'fullscreen'].includes(item.id))\
                  ]\
                }\
              }\
            },\
            { type: 'sidebarRight' }\
          ]
        }
      },


  }}
/>

```

### Repeatable Elements [​](https://app.grapesjs.com/docs-sdk/plugins/preset/printable\#repeatable-elements "Direct link to Repeatable Elements")

The example below shows how to render collections of repeatable elements using dynamic data, ideal for print-ready layouts like badges, labels, or employee cards.

![Printable plugin - Repeatable Elements](https://app.grapesjs.com/docs-sdk/assets/images/printable-repeat-el-78f2c8a15aa9a526422aadbacc43b695.webp)

- React
- JS
- 🍇  Demo

```codeBlockLines_AdAo
import StudioEditor from '@grapesjs/studio-sdk/react';
import '@grapesjs/studio-sdk/style';
import { presetPrintable, canvasFullSize } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        presetPrintable.init({\
          blockPageBreak: false\
        }),\
        canvasFullSize,\
      ],
      project: {
        type: 'document',
        default: {
          pages: [\
            {\
              name: 'Invoice',\
              component: `<!DOCTYPE html>\
                <html>\
                  <head>\
                    <style>\
                      .cards {\
                        display: grid;\
                        grid-template-columns: repeat(2, 1fr);\
                        gap: 1em;\
                      }\
                      .card {\
                        background-image:linear-gradient(120deg,rgba(217,56,143,1) 8%,rgba(227,118,128,1) 55%,rgba(247,234,244,1) 55%);\
                        border-radius: 12px;\
                        padding: 20px;\
                        color: white;\
                        height: 171px;\
                        border: 1px solid #ddd;\
                        display: flex;\
                        flex-direction: column;\
                        justify-content: center;\
                      }\
\
                      .card-left {\
                        align-self: flex-start;\
                      }\
\
                      .card-right {\
                        align-self: flex-end;\
                        text-align: right;\
                        color: #555;\
                      }\
\
                      .name {\
                        font-size: 1.3em;\
                        font-weight: 700;\
                      }\
\
                      .title {\
                        font-size: 0.8em;\
                        opacity: 0.85;\
                        margin: 4px 0;\
                      }\
\
                      .company {\
                        font-size: 0.9em;\
                        font-weight: 600;\
                        margin-bottom: 8px;\
                        opacity: 0.9;\
                      }\
\
                      .contact {\
                        font-size: 0.7em;\
                        line-height: 1.5;\
                        opacity: 0.9;\
                      }\
                    </style>\
                  </head>\
                  <body style="padding: 10px 40px; font-family: Arial, Helvetica, sans-serif">\
                    <data-collection\
                      class="cards"\
                      data-gjs-data-resolver='{"collectionId": "ppl", "dataSource": {"type":"data-variable", "path":"globalData.people.data" } }'\
                    >\
                      <data-collection-item class="card">\
                        <div class="card-left">\
                          <div class="name" data-gjs-type="data-variable" data-gjs-data-resolver='{"collectionId": "ppl", "variableType": "currentItem", "path": "name" }'></div>\
                          <div class="title" data-gjs-type="data-variable" data-gjs-data-resolver='{"collectionId": "ppl", "variableType": "currentItem", "path": "title" }'></div>\
                          <div class="company" data-gjs-type="data-variable" data-gjs-data-resolver='{"collectionId": "ppl", "variableType": "currentItem", "path": "company" }'></div>\
                        </div>\
                        <div class="card-right">\
                          <div class="contact">\
                            <data-variable data-gjs-data-resolver='{"collectionId": "ppl", "variableType": "currentItem", "path": "email" }'></data-variable><br/>\
                            <data-variable data-gjs-data-resolver='{"collectionId": "ppl", "variableType": "currentItem", "path": "phone" }'></data-variable><br/>\
                            <b data-gjs-type="data-variable" data-gjs-data-resolver='{"collectionId": "ppl", "variableType": "currentItem", "path": "website" }'></b>\
                          </div>\
                        </div>\
                      </data-collection-item>\
                    </ul>\
                  </body>\
                <html>\
              `,\
            }\
          ]
        }
      },
      dataSources: {
        globalData: {
          people: [\
            {\
              name: 'Alice Morgan',\
              title: 'Creative Director',\
              company: 'Morgan Studio',\
              email: 'alice@morganstudio.com',\
              phone: '+1 234 567 8901',\
              website: 'morganstudio.com'\
            },\
            {\
              name: 'Ben Carter',\
              title: 'Product Manager',\
              company: 'Carter Labs',\
              email: 'ben@carterlabs.io',\
              phone: '+1 555 987 1234',\
              website: 'carterlabs.io'\
            },\
            {\
              name: 'Carla Rossi',\
              title: 'Marketing Lead',\
              company: 'Rossi & Co',\
              email: 'carla@rossico.com',\
              phone: '+39 012 3456 789',\
              website: 'rossico.com'\
            },\
            {\
              name: 'Daniel Kim',\
              title: 'UX Designer',\
              company: 'Designory',\
              email: 'daniel@designory.com',\
              phone: '+82 10 1234 5678',\
              website: 'designory.com'\
            },\
            {\
              name: 'Eva Liu',\
              title: 'Software Engineer',\
              company: 'Codeverse',\
              email: 'eva@codeverse.dev',\
              phone: '+44 20 7946 0958',\
              website: 'codeverse.dev'\
            },\
            {\
              name: 'Frank Zane',\
              title: 'Operations Lead',\
              company: 'Logify',\
              email: 'frank@logify.org',\
              phone: '+1 312 321 4321',\
              website: 'logify.org'\
            },\
            {\
              name: 'Grace Tang',\
              title: 'Data Analyst',\
              company: 'QuantIQ',\
              email: 'grace@quantiq.com',\
              phone: '+61 3 9123 4567',\
              website: 'quantiq.com'\
            },\
            {\
              name: 'Hugo Martins',\
              title: 'Art Director',\
              company: 'Studio Verde',\
              email: 'hugo@verde.studio',\
              phone: '+55 11 98765 4321',\
              website: 'verde.studio'\
            },\
            {\
              name: 'Isabel Romero',\
              title: 'HR Manager',\
              company: 'PeopleFlow',\
              email: 'isabel@peopleflow.com',\
              phone: '+34 91 234 5678',\
              website: 'peopleflow.com'\
            },\
            {\
              name: 'Jason Lee',\
              title: 'CTO',\
              company: 'Devstream',\
              email: 'jason@devstream.io',\
              phone: '+1 800 123 4567',\
              website: 'devstream.io'\
            },\
            {\
              name: 'Klara Schmidt',\
              title: 'Finance Consultant',\
              company: 'LedgerWise',\
              email: 'klara@ledgerwise.de',\
              phone: '+49 30 1234 5678',\
              website: 'ledgerwise.de'\
            },\
            {\
              name: 'Luis Hernandez',\
              title: 'Full Stack Developer',\
              company: 'BitForge',\
              email: 'luis@bitforge.tech',\
              phone: '+52 55 1234 5678',\
              website: 'bitforge.tech'\
            },\
            {\
              name: 'Nina Patel',\
              title: 'Content Strategist',\
              company: 'StoryGrid',\
              email: 'nina@storygrid.com',\
              phone: '+91 22 4000 1234',\
              website: 'storygrid.com'\
            },\
            {\
              name: 'Omar Khalid',\
              title: 'Growth Hacker',\
              company: 'ScaleUp',\
              email: 'omar@scaleup.app',\
              phone: '+971 4 567 1234',\
              website: 'scaleup.app'\
            },\
            {\
              name: 'Yuki Tanaka',\
              title: 'Innovation Lead',\
              company: 'Neogenics',\
              email: 'yuki@neogenics.jp',\
              phone: '+81 3 1234 5678',\
              website: 'neogenics.jp'\
            }\
          ]
        }
      },
      // Custom layout for demo purposes
      layout: {
        default: {
          type: 'row',
          height: '100%',
          children: [\
            {\
              type: 'sidebarLeft',\
              children: { type: 'panelLayers', header: { label: 'Layers', collapsible: false, icon: 'layers' } }\
            },\
            {\
              type: 'canvasSidebarTop',\
              sidebarTop: {\
                rightContainer: {\
                  buttons: ({ items }) => [\
                    {\
                      id: 'print',\
                      icon: '<svg viewBox="0 0 24 24"><path d="M18 3H6v4h12m1 5a1 1 0 0 1-1-1 1 1 0 0 1 1-1 1 1 0 0 1 1 1 1 1 0 0 1-1 1m-3 7H8v-5h8m3-6H5a3 3 0 0 0-3 3v6h4v4h12v-4h4v-6a3 3 0 0 0-3-3Z"/></svg>',\
                      onClick: ({ editor }) => editor.runCommand('presetPrintable:print')\
                    },\
                    ...items.filter(item => !['showImportCode', 'fullscreen'].includes(item.id))\
                  ]\
                }\
              }\
            },\
            { type: 'sidebarRight' }\
          ]
        }
      },


  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/preset/printable\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| blockPageBreak | [Block](https://grapesjs.com/docs/api/block.html) | Additional Block properties of the page break component. Pass \`false\` to avoid adding the block.<br>**Example**<br>```codeBlockLines_AdAo<br>{<br>  "category": "Extra",<br>  "label": "My label"<br>}<br>``` |
| fixedHeight | boolean | With this option enabled the document won't grow in height and it won't be possible to scroll.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| devices | function | Customize the printable devices.<br>**Example** <br>```codeBlockLines_AdAo<br>devices: ({ items }) => {<br> return [<br>   ...items,<br>   {<br>     id: 'custom',<br>     name: 'Custom',<br>     width: 100,<br>     height: 200,<br>     unit: 'mm'<br>  }<br> ]<br>}<br>``` |
| selectedDevice | string | Selected device id. Available printable sizes: 'a5', 'a5-portrait', 'a4', 'a3', 'b5', 'b4', 'letter', 'legal', 'ledger'.<br>**Default** <br>```codeBlockLines_AdAo<br>a4<br>``` |
| enablePageBreaksSpot | boolean | Enables the page break indicators in the canvas.<br>**Default** <br>```codeBlockLines_AdAo<br>true<br>``` |

- [Repeatable Elements](https://app.grapesjs.com/docs-sdk/plugins/preset/printable#repeatable-elements)
- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/preset/printable#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_rte_prosemirror.md

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

---

# File: app.grapesjs.com_docs-sdk_plugins_rte_tinymce.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/rte/tinymce"
title: "TinyMCE | GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/rte/tinymce#__docusaurus_skipToContent_fallback)

On this page

|     |     |
| --- | --- |
| Project types | `web` `email` |
| Plan | Startup plan |

Replace the default RTE with [TinyMCE 6](https://www.tiny.cloud/docs/tinymce/6/) (latest MIT).

![RTE TinyMCE plugin](https://app.grapesjs.com/docs-sdk/assets/images/rte-tinymce-plugin-7f28515bad46ee0680da049c2677838c.png)

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
import { rteTinyMce } from '@grapesjs/studio-sdk-plugins';
// ...
<StudioEditor
  options={{
      // ...
      plugins: [\
        rteTinyMce.init({\
          enableOnClick: true,\
          // Custom TinyMCE configuration\
          loadConfig: ({ component, config }) => {\
            const demoRte = component.get('demorte');\
            if (demoRte === 'fixed') {\
              return {\
                toolbar:\
                  'bold italic underline strikethrough | alignleft aligncenter alignright alignjustify | link image media',\
                fixed_toolbar_container_target: document.querySelector('.rteContainer')\
              };\
            } else if (demoRte === 'quickbar') {\
              return {\
                plugins: `${config.plugins} quickbars`,\
                toolbar: false,\
                quickbars_selection_toolbar: 'bold italic underline strikethrough | quicklink image'\
              };\
            }\
            return {};\
          }\
        })\
      ],
      layout: {
        default: {
          type: 'row',
          style: { height: '100%' },
          children: [\
            { type: 'sidebarLeft' },\
            {\
              type: 'column',\
              style: { flexGrow: 1 },\
              children: [\
                { type: 'sidebarTop' },\
                { type: 'canvas' },\
                // Empty container for the fixed RTE toolbar\
                { type: 'row', className: 'rteContainer', style: { justifyContent: 'center' } }\
              ]\
            },\
            { type: 'sidebarRight' }\
          ]
        },
      },
      project: {
        default: {
          pages: [\
            {\
              name: 'Home',\
              component: `\
              <div data-gjs-type="text">\
                <h1>Default configuration</h1>\
                Text content start\
                <div>\
                  Block content with <em>emphasis</em>, <b>bold text</b>, <a href="#some-link" target="_blank">link</a>.\
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
\
              <div data-gjs-type="text" data-gjs-demorte="fixed">\
                <h1>Fixed position</h1>\
                Text content start\
                <div>\
                  Block content with <em>emphasis</em>, <b>bold text</b>, <a href="#some-link" target="_blank">link</a>.\
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
\
              <div data-gjs-type="text" data-gjs-demorte="quickbar">\
                <h1>Quickbar</h1>\
                Text content start\
                <div>\
                  Block content with <em>emphasis</em>, <b>bold text</b>, <a href="#some-link" target="_blank">link</a>.\
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
              `\
            },\
          ]
        },
      }


  }}
/>

```

### Plugin options [​](https://app.grapesjs.com/docs-sdk/plugins/rte/tinymce\#plugin-options "Direct link to Plugin options")

| Property | Type | Description |
| --- | --- | --- |
| licenseKey | string | The license key for the plugin. This is optional, only required if the plugin is used outside of Studio SDK.<br>**Example**<br>```codeBlockLines_AdAo<br>"your-license-key"<br>``` |
| cdnScript | string | CDN scripts to load dynamically in case the library is not available.<br>**Default** <br>```codeBlockLines_AdAo<br>https://cdn.jsdelivr.net/npm/tinymce@6.8.5/tinymce.min.js<br>``` |
| enableOnClick | boolean | Enable RTE on click of the text component, instead of the default double-click.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| skipCustomTheme | boolean | Skip adding custom CSS styles for the toolbar based on the Studio theme.<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| loadConfig | function | Pass your custom configuration to the TinyMCE editor.<br>**Example**<br>```codeBlockLines_AdAo<br>({ config, editor, component }) => {<br> return {<br>  toolbar: 'bold italic underline strikethrough',<br>  // ...<br> }<br>}<br>``` |

- [Plugin options](https://app.grapesjs.com/docs-sdk/plugins/rte/tinymce#plugin-options)

---

# File: app.grapesjs.com_docs-sdk_plugins_sitemap.xml.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins/sitemap.xml"
title: "GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins/sitemap.xml#__docusaurus_skipToContent_fallback)

# Page Not Found

We could not find what you were looking for.

Please contact the owner of the site that linked you to the original URL and let them know their link is broken.

---

# File: app.grapesjs.com_docs-sdk_plugins.md

---
url: "https://app.grapesjs.com/docs-sdk/plugins"
title: "GrapesJS Studio SDK"
---

[Skip to main content](https://app.grapesjs.com/docs-sdk/plugins#__docusaurus_skipToContent_fallback)

# Page Not Found

We could not find what you were looking for.

Please contact the owner of the site that linked you to the original URL and let them know their link is broken.

---

