---
layout: post
title: "Handling Of Dynamically Imported module Errors"
date: 2023-02-04 12:25:13 +0000
slug: handling-of-dynamically-imported-module-errors
tags: [webpack, error-handling, error-boundaries, vite, error-boundary]
lang: en
---



![Cover Image]({{ '/assets/images/5d7384a3-0deb-463f-a2f5-15fd20d11d91.png' | relative_url }})

[Code splitting](https://vuejs.org/guide/best-practices/performance.html#code-splitting) is a technique used to improve the performance of large web applications in modern front-end frameworks such as Vue.js and React.js. This technique divides the application’s code into small chunks, allowing only the necessary code to be loaded. This reduces the application’s loading time, minimizes memory usage, and makes the application more modular.

To improve the web performance of the [Heybooster](https://app.heybooster.ai/), I used code splitting to load the components of routes only when they are visited. Similarly, I dynamically loaded components such as popups that are not directly needed.

We use the chunkFileNames property of [Vite.js/rollupjs](https://vitejs.dev/) to generate file names based on the content of the files for cache busting. Vite.js tracks changes made to the content of files and creates a new hash whenever any changes are made. This is used to detect changes made to the file content and prevent the use of cached files. For example, if a JavaScript file is modified, the browser may have cached it. However, since the hash value of the new file has changed, the browser will have to download the new file again, ensuring that the latest file is used. Therefore, the hash value is used to maintain the current version of the file and prevent the use of outdated files in the browser cache. This ensures that the application runs in a more up-to-date manner.

However, cache busting and code splitting caused a problem to arise in the project. Since component names were generated dynamically, after each deployment, the components were renamed based on the file changes made and old assets were deleted from the server. After a new version was deployed, if the user did not refresh the page or upgrade to the current version, they would encounter a “**TypeError: Failed to fetch dynamically imported module:**” when switching between pages or trying to open a dynamically loaded component. In order to continue from where they left off, the user had to refresh the page and cache busting. This resulted in a poor user experience, and we faced the same issue with every deployment.

We use AWS Amplify for our front-end project. While investigating whether there was a feature related to Amplify not deleting previous version files, I couldn’t find any results. When I looked at the solutions of those who encountered the same problem, there were only solutions that worked in certain cases.

I tried to solve the problem by caching the assets using a service worker with Vite PWA Plugin, but I couldn’t completely solve the issue with [Vite PWA Plugin](https://vite-pwa-org.netlify.app/). I opened a g[ithub issue](https://github.com/vite-pwa/vite-plugin-pwa/issues/450) related to this, but it still wasn’t resolved.

To solve this problem, I combined multiple methods.

#### **1\. Creating an Error Boundary to handle “Failed to fetch dynamically imported module” errors**

I created an error boundary component and wrapped the project with this component, taking advantage of the [onError](https://router.vuejs.org/api/interfaces/Router.html#Methods-onError) hooks in vue-router and [onErrorCaptured](https://vuejs.org/api/composition-api-lifecycle.html#onerrorcaptured) hooks in Vue.js to catch and handle errors. When these errors occurred, we could have shown a warning message to the user asking them to refresh the page. However, we ensured that the user continued to use the new version by refreshing the page with JavaScript on our end. The potential problem with page refreshing was entering an infinite loop. Therefore, we stored the last refresh information as a 5-minute cookie, and if there was a last refresh, we did not refresh the page to prevent it from entering an infinite loop.

In the ErrorBoundary.vue component, I used **onError** and **onErrorCaptured** hooks to handle errors that could occur in vue-router and Vue.js.

I refreshed the page by handling the “**Failed to fetch dynamically imported module**” with the **handleImportModuleError** function.

I imported the ErrorBoundary component into App.vue and wrapped the project with it.

#### **2\. Wrapping import keyword and defineAsyncComponent function to handle “Failed to fetch dynamically imported module” errors**

As seen below, we use the import keyword to load a route dynamically. We also use the defineAsyncComponent function to load components dynamically.

I created two utility functions and wrapped import and defineAsyncComponent function and handled “**Failed to fetch dynamically imported module**” errors. I refreshed the page like the error boundary above. Since there are dozens of dynamic imports in the project, I didn’t refactor by replacing these dynamic imports with the utility functions I created. However, we can use the importWrapper and defineAsyncComponentWrapper functions in future developments. In fact, ErrorBoundary.vue solved our problem, but as an alternative solution, I also wrapped the import and defineAsyncComponent functions.

In conclusion, I was able to solve the problem that I had been struggling with for more than a month. 💪

You can review the demo project: [https://github.com/mustafadalga/vue-playground/tree/15.handling-of-dynamically-imported-module-errors](https://github.com/mustafadalga/vue-playground/tree/15.handling-of-dynamically-imported-module-errors)
