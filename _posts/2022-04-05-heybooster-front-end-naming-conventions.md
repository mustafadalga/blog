---
layout: post
title: "Heybooster Front End Naming Conventions"
date: 2022-04-05 19:39:13 +0000
slug: heybooster-front-end-naming-conventions
tags: [best-practices, clean-code, project-structure, react-project-structure, naming-conventions]
lang: en
---



![Cover Image]({{ '/assets/images/337529b9bb13588a2067128323257f86.jpeg' | relative_url }})

In this article, you will find the naming conventions we used in our [Heybooster](https://heybooster.ai/) front-end project.

### Variable Naming

* The camelCase naming convention is used to define a variable.
    

**Examples**

```typescript
const dataSourceName = '';
const stepsConnectStatus = {
  1: false,
  2: false,
  3: false,
  4: false,
};
const apiBaseUrl = {};
const bookmarkID = [];
const loaderStatus = [];
const axiosCancelStatusCodes = [];
const notifications = [];
```

### Props Naming

* The camelCase naming convention is used to define a prop.
    

**Example**

Define a prop:

```typescript
props: {
  showAll: {
    type: Boolean,
    default: true
  },
},
```

Pass a props:

```typescript
<DataSource showAll="true"/>
```

### Computed Property Naming

* The camelCase naming convention is used to define a computed property.
    

**Example**

```typescript
computed:{
  background() {
    return `bg-${this.priority.type}`;
  },
  insightCreatedDate(){
    return  new Date(this.insight.ts * 1000).toLocaleString();
  },
},
```

### Method / Function Naming

* The camelCase naming convention is used to define a method/function.
    
* The camelCase naming convention also used to setters, getters, mutations method in Vuex.
    
* The camelCase naming convention also used to SCSS mixins.
    

**Examples**

*Example function:*

```typescript
function getSourceTypeIcon(sourceType) {

  const dataSourceTypesIcons = {
    [dataSourceTypes.googleAnalytics]: "google-analytics.svg",
    [dataSourceTypes.googleAdwords]: "google-adwords.svg",
    [dataSourceTypes.googleSearchConsole]: "google-search.svg",
    [dataSourceTypes.facebook]: "facebook.svg",
    [dataSourceTypes.shopify]: "shopify.svg",
  };

  return dataSourceTypesIcons[sourceType];
}
```

*Example method:*

```typescript
methods: {
  resetChartVariables() {
    this.activeMetric = null;
    this.requestStatus = false;
    this.chartTitle = "";
  }
}
```

*Example SCSS Mixin:*

```typescript
@mixin setBoxShadow($value){
  -webkit-box-shadow: $value;
  -moz-box-shadow:    $value;
  box-shadow:         $value;
}
```

### Folder Naming

* The kebab-case naming convention is used to define a folder.
    

**Examples**

```typescript
cross-tab-communication
crypto-js
```

### Component/View Naming

* The PascalCase naming convention is used to define a component or view.
    

**Examples**

```typescript
DataSourceChangePopup.vue
MyInsights.vue
```

### JavaScript/TypeScript File Naming

* The kebab-case naming convention is used to define a javascript/typescript file.
    

**Examples**

```typescript
onboarding-pages.js
priority-list.ts
```

*Originally published at* [*heybooster*](https://insight.heybooster.ai/heybooster-front-end-naming-conventions/)*.*
