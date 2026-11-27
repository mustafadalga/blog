---
layout: post
title: "Optimizing Web Performance by Reducing bundle.js File Size in the HeyBooster Project"
date: 2023-06-14 10:15:44 +0000
slug: optimizing-web-performance-by-reducing-bundlejs-file-size-in-the-heybooster-project
tags: [web-performance, web-development, clean-code, clean-architecture, web-optimization]
lang: en
---



![Cover Image]({{ '/assets/images/6b1c8b8a-6663-412e-ab43-921d65b11631.png' | relative_url }})

Hello, there!

Today I’m going to share an experience we had at HeyBooster while upgrading our Sentry version.

### The Background

HeyBooster, like many projects, heavily relies on Sentry for error tracking. We recently decided to upgrade our Sentry version. However, post-upgrade, we found an alarming issue — our `main.ts` file size had increased significantly.

As front-end developers, we’re all aware of the impact file size can have on web performance. A larger bundle means a slower load time, and slower load times can directly affect the user experience and even SEO rankings. So we knew we had to tackle this issue head-on.

### The Warning Sign

Our build logs started showing a warning that our `main.ts` bundle size was over 500kB after minification. The warning suggested several ways to handle the issue:

* Using dynamic `import()` to code-split the application
    
* Using `build.rollupOptions.output.manualChunks` to improve chunking
    
* Adjusting the chunk size limit for the warning via `build.chunkSizeWarningLimit`
    

The increased size was due to the Sentry initialization code in our `main.ts` file, which was included in the main bundle.

![]({{ '/assets/images/16f86b60-ca73-4398-89b7-6c69fa52f6ae.png' | relative_url }})

### The Solution

Our solution involved a combination of code-splitting and dynamic importing, two powerful techniques for reducing the size of JavaScript bundles.

#### Step 1: Dynamic Import

We decided to load Sentry dynamically, meaning it wouldn’t be included in the initial bundle. Instead, it would be fetched and loaded only when needed. To achieve this, we used dynamic `import()`.

```typescript
 const Sentry = await import("@sentry/vue");
```

#### Step 2: Refactoring the Initialization Code

Our Sentry initialization was initially performed directly in the `main.ts` file, causing it to be included in the main bundle. We refactored this to an async function that would be invoked after the app was created but before it was mounted.

```typescript
/**
 * Function to initialize Sentry
 */
async function initializeSentry() {
    const Sentry = await import("@sentry/vue");
}
```

This function was then called before the Vue app was mounted, ensuring Sentry was loaded and initialized before the app became interactive.

```typescript
initializeSentry().then(() => {
  app.mount("#root");
});
```

### The Result

This refactoring allowed us to significantly reduce the size of our `main.ts` bundle. As a result, our web performance improved, and we resolved the warning signs in our build logs.

### The Takeaway

Web performance is a critical aspect of user experience. As developers, it’s our responsibility to ensure our applications are as performant as possible. In our case at HeyBooster, we found that dynamic imports and clever refactoring were the keys to optimizing our Sentry usage and improving our overall web performance.
