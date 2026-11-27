---
layout: post
title: "Adventures of a Front-End Developer — Our Work to Improve Web Performance"
date: 2021-12-09 10:29:09 +0000
slug: adventures-of-a-front-end-developer-our-work-to-improve-web-performance
tags: [optimization, web-performance, web-developers, web-development, web-optimization]
lang: en
---



![Cover Image]({{ '/assets/images/70f45eda-8b0f-44e7-8ee7-53b6c3d3458d.png' | relative_url }})

While I was starting out as a front-end developer at [Heybooster](https://www.heybooster.ai/), our [product owner](https://www.linkedin.com/in/neslihanbusraemikoglu/) asked me to handle the following problems:

*   Lead the development of solutions for web page speed issues, a problem for which we receive frequent feedback from users,
*   Resolve bugs in the current structure,
*   Develop new designs in the same way and responsively at a rate of 99% ,
*   Make sure we cover all userflows on the site .

I decided I would do my best to improve this project, so I began working on it.In this blog post we cover improvements made to the Front-end in general.

### Removing unused or unnecessary packages and components

*   In reviewing the npm packages in our project, I discovered some packages were installed but not used in the project.Therefore, I removed these npm packages.
*   Even for very simple operations in the project, I found that the npm package was installed and brought unnecessary loads with them to the project; in fact, I could have solved our problems without using an npm package. I removed all of these npm packages and wrote our own components and javascript methods.
*   During the development of the project, some components were no longer needed. However, they were still imported. By reviewing the code, I deleted the components that we did not need.

### Changing of large size npm packages

We were using [syncfusion](https://www.syncfusion.com/)’s packages to use line charts, treemap charts, and grid tables. These packages were large, and as a result, the chart or table took a long time to create and display, which also negatively affected the user experience. We changed these packages to resolve the problem.

We looked at the following factors , when choosing a new package:

*   Open source software
*   Features supported
*   Community support
*   Minimal package size
*   Free

Based on the above criteria, we reviewed and tested various packages. As a result, we decided to use [chart.js](https://www.npmjs.com/package/chart.js) for the line chart, [grid.js](https://gridjs.io/) for the grid table and [highcharts.js](https://www.npmjs.com/package/highcharts) for the treemap chart, which are the most suitable packages for our project.

In the graph below, you can see the size change after changing 3 npm packages:

![]({{ '/assets/images/bc0561bf-2a91-4604-9b28-59f66a99ae4f.png' | relative_url }})

We used two npm packages, [vue-markdown](https://www.npmjs.com/package/vue-markdown) and [vue-simple-markdown](https://www.npmjs.com/package/vue-simple-markdown), for markdown operations. I installed [marked.js](https://www.npmjs.com/package/markedjs) to replace both of these packages for all markdown operations.

![]({{ '/assets/images/68df5743-c24f-482a-8dbf-77520022b617.png' | relative_url }})

![]({{ '/assets/images/7de1046c-7ac7-4b5f-be50-35f163d43cd1.png' | relative_url }})

### Using mixins and helpers to avoid duplicate code

During the development of our project, I was also reviewing old codes. I found a lot of duplicate code in the project. I created mixins and helpers and removed duplicate codes for cleaner, manageable and optimized coding.

### Removing non-global packages, components, prototypes, and mixins from the main JS file

I saw that in the main javascript file main.js, non-global packages, components, prototypes, and mixins were imported. I changed these non-global packages, components, and mixins so that they could be imported and used only where needed. I also created a plugin for prototypes.

### Using vuex to manage data and states

We were using [eventBus](https://v3.vuejs.org/guide/migration/events-api.html#_2-x-syntax) for state management. But eventBus was not useful for our project and made it difficult to read, manage, and develop the code. We didn’t have a global structure to manage the data either.

Lack of a structure to manage data was leading to significant performance losses:

*   During each change of pages, API requests were being sent again, and the user had to wait until the response was received.
*   Although the same data is used in more than one component/view, the same API request was being sent in different components/views to obtain the same data.

We decided to use vuex, the official library of vue js, to improve web performance and better manage the project’s data and state. It was necessary to create the vuex modules as we needed to categorize the project’s states and data. As we need to categorize the states and data of the project, I divided them using the vuex modules into categories.

### Code Splitting and Lazy Loading

The size of the bundle files was too large because all views and components were kept in 2 bundle files. In consequence, pages and components were taking a long time to load, and it was slowing down the web page by up to thirty seconds.

![]({{ '/assets/images/ccab023d-8b3a-43eb-96e6-d544f98c632c.png' | relative_url }})

For the solution, I split the views and components into chunks instead of collecting them in a single javascript file. As a result, I reduced the size of the main bundle file by chunking it into views and components.

![]({{ '/assets/images/094574a6-f846-4eaf-85d5-b62842dc87b9.png' | relative_url }})

### Changing data without refreshing the page

Heybooster is a client-based service. The page was being refreshed when the user changes the active client. Reloading all javascript and image files every time the page was refreshed was causing performance issues. It was also unnecessary to reload some APIs since they work per-user. The feature was incorrectly developed and was not appropriate for the structure of Vue.

I followed the steps below to solve this issue:

*   I cleared all the vuex data of the active client when the client was changed.
*   I resent all client-based API requests.

### Managing cache issues

After each deployment, our biggest problem was clearing the caches. A browser was saving javascript bundle files to disk cache after loading them. In terms of performance, it’s good for the web page to load faster. However, we had difficulty deploying new developments.

![]({{ '/assets/images/9fb08c73-502f-4a4c-8941-66cef26e02e2.png' | relative_url }})

I tried the following methods to resolve the problem of clearing the cache after each deployment:

*   Using Webpack to make javascript file names dynamic after each deployment: The user had to refresh the page after each deployment to implement this solution. Because old bundle files were being deleted and new javascript files were being created using the hash of the bundle files. However, we encountered “Loading chunk errors” when the user changed pages without refreshing the page. Because the user was trying to load old javascript files, which were no longer available.
*   Displaying a warning message after each deployment with the service worker to direct the user to install the new version: As a result of this solution, we were unable to completely resolve our problem. For this reason, we have removed the service worker.

Since none of the above solutions fixed our issue, we reverted back to the default configurations.

When we decided to move the front end project to [amplify](https://aws.amazon.com/amplify/), I researched its cache management and found that it could solve our cache problem with the [performance mode](https://docs.aws.amazon.com/amplify/latest/userguide/ttl.html#Instant-cache-invalidation-with-instant-deploys) of amplify. After configuring amplify’s performance mode, amplify started to manage our javascript files and invalidate our cache after each deployment.

In general, I wrote about what I did to improve the web performance of our project. Aside from the solutions listed above, there are many possibilities. I will share my experiences as I implement and develop these.

I also share my open source developments on my github [account](https://github.com/mustafadalga).

*Originally published at* [https://insight.heybooster.ai/our-work-to-improve-web-performance/](https://insight.heybooster.ai/our-work-to-improve-web-performance/)
