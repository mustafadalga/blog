---
layout: post
title: "Vuex on large projects: How to structure it ?"
date: 2021-11-21 18:19:45 +0000
slug: vuex-on-large-projects-how-to-structure-it
tags: [web-design, web-developers, web-development, frontend-development, web-storage-api]
lang: en
---



![Cover Image]({{ '/assets/images/16e74d21-d99e-4f5d-9372-7546344f9494.png' | relative_url }})

If you are using vuex in a small project, you can keep state, mutation, getters, and actions in a single file.

On the other hand, when developing a large project, we can divide vuex into modules based on categories, thereby keeping file sizes down, keeping the code cleaner, and increasing the chances of more than one developer being able to work on the project at the same time.

We can also separate state, mutation, getters, and actions within these modules. By using vuex modules, we want to increase manageability and understandability.

I will explain and show how to create modules per categories in this blog post. I will divide this blog post into two parts and explain how to exchange data between these modules in the second post, so as not to prolong it.

### Modules

1. **auth:** This module stores and processes data about user accounts.
    
2. **movies:** This module stores and processes data about movies.
    
3. **notifications:** This module stores and processes data about warnings, errors, etc.
    

Vuex modules

Every module has its own state, mutations, getters, and actions files.

![]({{ '/assets/images/d642191b-bc85-4a0a-8b29-8aa944185cb0.png' | relative_url }})

Module files

In each module, we create an **index.js** file that collects state, mutations, getters, and actions.

```typescript
import state from './state';
import getters from './getters';
import mutations from './mutations';
import actions from './actions'

export default {
  namespaced: true,
  state,
  getters,
  mutations,
  actions,
};
```

We import these modules and define them in the main store file **index.js**.

```typescript
import { createStore } from 'vuex'
import auth from './modules/auth';
import movies from "./modules/movies";
import notifications from "./modules/notifications";

export default createStore({
  modules: {
    auth,
    movies,
    notifications
  }
})
```

**main.js** uses it as follows:

```typescript
 import { createApp } from 'vue'
 import App from './App.vue'
 import router from './router'
 import store from './store'
 
 createApp(App).use(store).use(router).mount('#app')
```

As a result, vuex needs to be used more accurately to provide a more manageable and clean project.

The sample project is on my [github](https://github.com/mustafadalga/vuex-on-large-projects) account and the [demo](https://vuex-on-large-projects.netlify.app/) is on netlify.

*Originally published at* [*heybooster*](https://insight.heybooster.ai/vuex-on-large-projects-how-to-structure-it/)*.*
