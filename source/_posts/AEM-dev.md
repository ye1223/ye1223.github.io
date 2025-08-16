---
title: How to Develop AEM Components
categories:
  - []
wordCount: 710
charCount: 10068
imgCount: 2
vidCount: 0
wsCount: 0
cbCount: 2
readTime: About 4 minutes
date: 2024-02-10 17:33:55
tags:
featured_image:
---

## Prerequisites

- Understanding how to configure components are on AEM pages
- Basic front-end development skills, not limited to HTML, JavaScript, Vue.js

> [!TIP]
>
> **Using the CLP project as an example**

The top level directory structure of an AEM project is roughly like this. **Frontend component development** mainly focuses on the `ui.apps` and `ui.frontend` directories:

```cmd
├── all
├── core
├── it.tests
├── ui.apps
├── ui.apps.structure
├── ui.config
├── ui.content
├── ui.frontend
├── ui.tests
├── archetype.properties
└── pom.xml
```

<br/>

## `ui.apps`

> Component dialogs and metadata are defined in `ui.apps`

In `ui.apps/src/main/content/jcr_root/apps/clphk-postlogin/components`, you can find declared AEM components. Their directory structure is:

```cmd
├── _cq_dialog
│   └── .content.xml ①
├── .content.xml ②
└── XXXXX.html
```

### 🔦 Define Component Dialog

- `_cq_dialog` folder defines the component dialog:

  - <img src="https://s2.loli.net/2024/04/12/d2v5UMseGuFEKkl.png" alt="image-20240301134106075" style="zoom:30%;float:left" />

  - The dialog pops up when clicking a component in the page editor for configuring component options.

  - ①xml defines dialog structure including fields, labels, defaults etc. It follows JCR (Java Content Repository) to describe layout and functions.

  - ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
              xmlns:cq="http://www.day.com/jcr/cq/1.0"
              xmlns:jcr="http://www.jcp.org/jcr/1.0"
              xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
              jcr:primaryType="nt:unstructured"
              sling:resourceType="cq/gui/components/authoring/dialog"
              jcr:title="...DIALOG TITLE">
        <items jcr:primaryType="nt:unstructured">
            <tab1 jcr:primaryType="nt:unstructured"
                  sling:resourceType="cq/gui/components/authoring/dialog/tabpanel"
                  jcr:title="...TAB 1">
                <items jcr:primaryType="nt:unstructured">
                    <field1 jcr:primaryType="nt:unstructured"
                            sling:resourceType="granite/ui/components/foundation/form/textfield"
                            fieldLabel="...FIELD 1"
                            name="./field1"/>
                    <!-- ...MORE FIELDS HERE -->
                </items>
            </tab1>
            <!-- ...MORE TABS HERE -->
        </items>
    </jcr:root>
    ```

### 🔦 Define Component

- ② xml file:

  - Defines AEM component metadata.

  - ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:Component"
        jcr:title="CLP Eco Feed Vertical" // Component title
        componentGroup="CLP HK PostLogin - Develop"/> // Component group
    ```

  - <img src="https://s2.loli.net/2024/04/12/RoS9EbPM3ujOIlt.png" alt="image-20240301140937620" style="zoom:40%;float:left" />

### 🔦 Define Component Template

> [Reference](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=en)

The HTML file uses **HTL** (HTML Template Language) to render custom components in AEM pages (In this project, use Vue.js for joint development).

```xml
<sly data-sly-use.templates="core/wcm/components/commons/v1/templates.html" data-sly-set.hasContent="${false}" />

<!-- aquire modal data -->
<sly data-sly-use.clpecofeed="com.clp.clphkpostlogin.core.models.CLPEcoFeed"></sly>

<div data-dom>
  <!-- properties is defined by dialog data -->
  <VUE-COMPONENT props1="${properties.modeltype}" props2="${clpecofeed}"></VUE-COMPONENT>
  <!-- in Vue SFC, can get the properties from the `prop` -->
</div>

<sly data-sly-call="${templates.placeholder @ isEmpty=!hasContent}"></sly>
```

<br/>

## `ui.frontend`

> `ui.frontend` is built with **webpack** + **Vue** in this project

### 🔦 Write SFC (Single File Component)

- Write Vue components in <font color=red><u>**`ui.frontend/src/main/webpack/vue/components`**</u></font> (Please ensure to define <font color=blue>`name` </font>attribute for reference in AEM).

> `componnents directory` structure
>
> ```
> - _register
>   - common # common components
>   - components aem components
>   - elementui elementui components
> - common
> - components match aem naming
> - utils common js libs
> - main.js entry
> ```
>
> Notes:
>
> 1. Register needed Element UI components in `_register/elementui`.
> 2. Register new AEM components in `_register/components`.
> 3. AEM components can use registered Element UI and common components.

### 🔦 Register Components

- Export Vue components in <font color=red><u>**`ui.frontend/src/main/webpack/vue/_register/component`**</u></font> for `main.js` to import and mount with `createApp.component()`.

<br/>

## 💡 Preview Components

> Please configre the component on AEM page before Preview Components

### For independent components

> [!TIP]
>
> **independent components** refers to to a component that is developed and previewed <u>standalone</u>, without associations with other components. For example, no need to navigate to other components or pages.
>
> No need to bundle the entire `ui.frontend`.

- Mount defined component in <font color=red><u>**`ui.frontend/src/main/webpack/templates/local.html`**</u></font>:

  - ```vue
    <div id="localApp" class="dtt-uf dtt-uf-vc dtt-uf-dc">
      <div class="dtt-uf">
        <div data-dom>
          <CustomComponent />
        </div>
      </div>
    </div>
    ```

- Run `npm run local` and open browser to preview this component.

  > "local": "cross-env VUE_APP_MY_ENV=local webpack-dev-server --open --config ./webpack.local.js"

### For associated components

> [!TIP]
>
> Need to bundle the **_entire_** `ui.frontend`.

#### Option 1: Maven build

<u>**Universal but Time-Consuming Solutions**</u>

- In project directoty, run **`mvn clean install -PautoInstallSinglePackage`**. Then preview in **AEM system**.

  > **Production environment**
  >
  > For debug logs during development:
  >
  > ​ Comment out Terser plugin in `webpack` prod config that removes `console.log`.
  >
  > ​ Or use `window.console.log`.

#### Option 2: AEM Sync plugin in VsCode

- Install `AEM Sync` plugin in VsCode.

- In `ui.frontend` directoty, run `npm run dev`. Plugin will sync build to AEM. Preview in launched server.

  > "dev": "webpack -d --env dev --config ./webpack.dev.js && clientlib --verbose"

  > **Development environment**
