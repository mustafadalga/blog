---
layout: post
title: "Debugging TypeScript & Jest-DOM Matchers in Vue.js"
date: 2023-06-13 17:09:33 +0000
slug: debugging-typescript-jest-dom-matchers-in-vuejs
tags: [unit-testing, jest, testing-library, vitest, jest-dom]
lang: en
---



![Cover Image]({{ '/assets/images/b7fc26de-9066-4ac9-8cbc-3572dda3c421.png' | relative_url }})

One of the joys of modern web development is the seamless integration of various libraries and frameworks. However, every so often, you can run into hitches that send you down the rabbit hole of debugging. Today, I want to share a recent experience addressing a TypeScript configuration problem in a Vue.js project, which I hope will save someone else a headache or two.

### The Problem

Our project uses a robust tech stack including Vue.js, TypeScript, Vite, Vitest, and the Vue Testing Library. We also installed the `@testing-library/jest-dom` package to use jest-dom matchers with Vite and the Vue Testing Library.

To configure jest-dom, we created a `jest-dom.setup.ts` file in the root directory:

```typescript
import matchers from "@testing-library/jest-dom/matchers";
import { expect } from "vitest";
expect.extend(matchers);
```

Despite the configuration, TypeScript kept throwing errors because it couldn’t find the jest-dom TypeScript matchers. Here are a few of the errors we received:

```typescript
- TS2339: Property 'toHaveTextContent' does not exist on type 'Assertion '.
- TS2339: Property 'toHaveAttribute' does not exist on type 'Assertion '.
- TS2339: Property 'toBeInTheDocument' does not exist on type 'Assertion '
- TS2339: Property 'toHaveClass' does not exist on type 'Assertion '.
```

### The Journey

We tried numerous solutions and explored many Stack Overflow threads. Finally, we found a solution that involved updating our TypeScript configuration files: `tsconfig.app.json` and `tsconfig.vitest.json`.

Our `tsconfig.app.json` file was updated to include the types from jest-dom as shown below:

```typescript
{
  "extends": "@vue/tsconfig/tsconfig.web.json",
  "include": ["env.d.ts", "src/**/*", "src/**/*.vue"],
  "exclude": ["src/**/__tests__/*"],
  "compilerOptions": {
    "composite": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    },
    "types": ["node", "jsdom", "@testing-library/jest-dom"]
  }
}
```

In the `compilerOptions` of `tsconfig.app.json`, we added "@testing-library/jest-dom" to the `types` array.

Then, we also updated our `tsconfig.vitest.json` file to extend the `tsconfig.app.json` as follows:

```typescript
{
  "extends": "./tsconfig.app.json",
  "compilerOptions": {
    "composite": true,
  }
}
```

### The Solution

The changes in the `tsconfig.app.json` and `tsconfig.vitest.json` files were the key to solving the problem. By including the jest-dom types in `tsconfig.app.json`, we were able to extend them in `tsconfig.vitest.json`. This allowed TypeScript to recognize the jest-dom matchers in our testing environment.

### Wrapping Up

While this solution might seem simple in hindsight, it took some time to put all the pieces together. Debugging is, after all, an art and an essential part of every developer’s journey.

As we continue to build more complex projects using a variety of tools and libraries, understanding the nuts and bolts of these tools becomes ever more crucial. I hope this blog post sheds some light on the intriguing world of TypeScript configurations and assists someone out there facing a similar issue.
