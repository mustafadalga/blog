---
layout: post
title: "Storing and Using Last Visited Page in Vue JS"
date: 2021-11-21 19:13:26 +0000
slug: storing-and-using-last-visited-page-in-vue-js
tags: [frontend, frontend-development, web-api, store, web-storage-api]
lang: en
---


![Cover Image]({{ '/assets/images/910e1fce1329d155f6b8c8b135c1ee37.jpeg' | relative_url }})

If a user has an ongoing transaction on a website, we would like them to continue from the last transaction to provide a better user experience.

If a user registers on a website and then leaves the website after completing any step, we can ensure that the user continues from where they left off, even if the tab or browser is closed and then reopened.

This article will explain how we can improve the user experience in Vue js.

Firstly, we will store the page visited by a user in localStorage in order to be able to process this information later. The last visited page will be stored if the user refreshes the page, closes the tab, or closes the browser.

We will use vue-router’s [navigation guards](https://router.vuejs.org/guide/advanced/navigation-guards.html) to store and process a last visited page.

We store the last visited page with vue-router’s [afterEach](https://router.vuejs.org/guide/advanced/navigation-guards.html#global-after-hooks) hook.

```typescript
router.afterEach(to => localStorage.setItem('lastVisitedPage', to.name));
```

The important thing here is to do this check only when our website is first accessed. We will store but not process each page change after the first access.

We need to check the following two variables to ensure that the last visited page can be redirected. We will check this in vue-router’s [beforeEach](https://router.vuejs.org/guide/advanced/navigation-guards.html#global-before-guards) hook.

* Is the first redirect complete? : We define the initial value of this variable is true and then change its state when the redirecting process is complete.
    
* Does there exist a last visited page? : It should go to the standard redirected page if there is no last visited page.
    

We created a global variable and named it **isFirstRedirect**, which has the default value of true to keep track of whether the first redirect was made.

```typescript
let isFirstRedirect = true;

router.beforeEach((to, from, next) => {
  ....
});
```

We obtained the last visited page from localStorage and put it into the **lastVisitedPage** variable.We kept the result of redirected in **executableRedirect** variable.

If **executableRedirect**’s value is **true**, the last visited page will be redirected. Otherwise, the standard redirected page will be used.

```typescript
let isFirstRedirect = true;

router.beforeEach((to, from, next) => {

  const lastVisitedPage = localStorage.getItem('lastVisitedPage');
  const executableRedirect = (lastVisitedPage && isFirstRedirect);

  isFirstRedirect = false;

  executableRedirect ? next({ name: lastVisitedPage }) : next();

});
```

The sample project is on my [GitHub](https://github.com/mustafadalga/storing-and-using-last-visited-page) account and the [demo](https://process-last-visited-page.netlify.app/) is on netlify.

*Originally published at* [*heybooster*](https://insight.heybooster.ai/storing-and-using-last-visited-page-in-vue-js/)
