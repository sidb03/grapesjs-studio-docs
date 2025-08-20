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

## Setup [​](https://app.grapesjs.com/docs-sdk/configuration/projects\#setup "Direct link to Setup")

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

## Storage [​](https://app.grapesjs.com/docs-sdk/configuration/projects\#storage "Direct link to Storage")

In Studio SDK, you can configure how to store project JSON data. This is controlled by the `storage` option of the SDK.

warning

Always rely on the JSON project data to properly load your project in the editor.

While the editor can parse and use HTML/CSS code, allowing you to include it as part of your project initialization (see [Templates](https://app.grapesjs.com/docs-sdk/configuration/templates)), it should not be used as a persistence layer for loading projects.
Important information about components are stripped away when you export the code.

### Storage Types [​](https://app.grapesjs.com/docs-sdk/configuration/projects\#storage-types "Direct link to Storage Types")

You have three options:

- We provide an option to save it in our [cloud storage](https://app.grapesjs.com/docs-sdk/configuration/projects#cloud-storage).
- You can store it in your own [self-hosted infra](https://app.grapesjs.com/docs-sdk/configuration/projects#self-hosted-storage).
- By default, your end-user's project data is saved locally in their browser.
This option is ideal for development purposes. For production, please select one of the alternatives.

### Cloud Storage [​](https://app.grapesjs.com/docs-sdk/configuration/projects\#cloud-storage "Direct link to Cloud Storage")

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

### Self Hosted Storage [​](https://app.grapesjs.com/docs-sdk/configuration/projects\#self-hosted-storage "Direct link to Self Hosted Storage")

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

### Load Project From Prop [​](https://app.grapesjs.com/docs-sdk/configuration/projects\#load-project-from-prop "Direct link to Load Project From Prop")

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

### Autosave [​](https://app.grapesjs.com/docs-sdk/configuration/projects\#autosave "Direct link to Autosave")

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

## Export [​](https://app.grapesjs.com/docs-sdk/configuration/projects\#export "Direct link to Export")

In the previous section, we covered how to store the JSON data essential for loading projects into the editor. Another key aspect of the editor is exporting projects into specific formats, such as an HTML page for websites, a newsletter's content, a PDF document, or an image file for designs.

The Studio SDK provides a default export mechanism in the form of "files." However, you can utilize the same project JSON data to implement custom export logic. With this, you can retrieve all pages, components, and their properties, allowing you to export projects in any format that suits your needs.

### Project Files [​](https://app.grapesjs.com/docs-sdk/configuration/projects\#project-files "Direct link to Project Files")

The default `studio:projectFiles` command generates a list of project files, which by default represent an HTML document for each page in the project. Since HTML is a widely used format, you can easily use it as a foundation to convert your project into other formats, such as PDF, image files, or more.

#### Project files command options [​](https://app.grapesjs.com/docs-sdk/configuration/projects\#project-files-command-options "Direct link to Project files command options")

Below the options available for the `studio:projectFiles` command.

Show options

| Property | Type | Description |
| --- | --- | --- |
| page | [Page](https://grapesjs.com/docs/api/page.html) | Export specific page. By default, all pages are exported. |
| assetsFolder | string | If the HTML containes base64 images, they will be exported as separate files and placed in the specified folder name.<br>**Default** <br>```codeBlockLines_AdAo<br>assets<br>``` |
| filenameCss | string | Filename for CSS file.<br>**Default** <br>```codeBlockLines_AdAo<br>style.css<br>``` |
| styles | string | Indicate how to export styles. By default, styles are exported in a separate CSS file. |
| skipProject | boolean | Skip project file (project JSON)<br>**Default** <br>```codeBlockLines_AdAo<br>false<br>``` |
| exportConfig |  | Configuration options for exporting data resolvers.<br>**Default** <br>```codeBlockLines_AdAo<br>undefined<br>``` |
| optionsHtml |  |  |

#### Use Case: Publish Website [​](https://app.grapesjs.com/docs-sdk/configuration/projects\#use-case-publish-website "Direct link to Use Case: Publish Website")

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

#### Use Case: Inline Data [​](https://app.grapesjs.com/docs-sdk/configuration/projects\#use-case-inline-data "Direct link to Use Case: Inline Data")

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