---
layout: post
title: "Streamlining React.js Production Builds — A Problem Solved!"
date: 2023-05-22 15:58:01 +0000
slug: streamlining-reactjs-production-builds-a-problem-solved
tags: [optimization, reactjs, clean-code, web-optimization, remove-attribute]
lang: en
---



![Cover Image]({{ '/assets/images/e35e368e-aade-4750-80e1-4cf1efba91fd.png' | relative_url }})

While developing applications, we often find ourselves juggling between two environments — testing and production. In the realm of React.js, our favourite library for testing is the Testing Library. It’s a powerful tool that allows us to easily select HTML elements using ‘data-testid’ attributes. However, while these attributes are immensely beneficial during testing, they become redundant in our production builds, resulting in cluttered and less readable HTML.

Recognizing this issue, I put my Front End Developer hat on and began working on a solution. The objective was clear — to create a system that removes these attributes for production, yet keeps their utility intact during testing. The answer? A specialized Vite plugin for React.js — the **React.js Remove Attributes Plugin**.

### Introducing: The React.js Remove Attributes Plugin

This plugin, built specifically for React.js projects, is designed to effortlessly remove specified attributes from your HTML code during production builds. Its operation results in cleaner, more optimized applications while maintaining the ease of testing.

#### Here’s Why You’ll Love It

1. **Selective Attribute Removal:** You decide which attributes you want to get rid of in the production build. It’s excellent for removing attributes like ‘data-testid’ that are only required during testing.
    
2. **File Extension Specificity:** Configure the plugin according to your project’s specific requirements. You can decide which file extensions should be considered for attribute removal. At present, the plugin supports ‘tsx’ and ‘jsx’ extensions.
    
3. **Granular Exclusions:** Don’t want to include some folders or files in the plugin’s operation? No problem. The plugin allows you to specify certain files or folders to be ignored during the attribute removal process.
    
4. **Cleaner Production Code:** By removing unnecessary attributes, this plugin ensures that your production code is neat and better performing. Say goodbye to bloated code and hello to optimized application performance.
    

#### Getting Started

You can install the plugin via npm using the following command:

```bash
npm install --save-dev react-remove-attr
```

After installation, you’ll need a Vite project to use this plugin. Import and use it in your `vite.config.js` or `vite.config.ts` file as shown below:

```typescript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import removeAttr from "react-remove-attr"

export default defineConfig({
    plugins: [
        removeAttr({
            extensions: [],
            attributes: []
        }),
        react(),
    ])
})
```

In conclusion, the React.js Remove Attributes plugin has allowed us to efficiently declutter our production builds by removing unnecessary attributes. It’s a perfect example of a simple solution to a common problem. By making our HTML code cleaner and more readable, we’ve improved both our development experience and the performance of our applications.

Happy coding!
