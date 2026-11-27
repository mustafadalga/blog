---
layout: post
title: "Driving Documentation Culture: A Developer’s Journey in the Heybooster Project"
date: 2023-06-13 08:22:15 +0000
slug: driving-documentation-culture-a-developers-journey-in-the-heybooster-project
tags: [software-development, documentation, clean-code, documentation-tool, team-culture]
lang: en
---



![Cover Image]({{ '/assets/images/4135a14f-a751-4d8a-9660-df449a139d16.png' | relative_url }})

As any seasoned software developer will tell you, writing code is just one part of the job. One aspect often overlooked is documentation. As an integral member of the Heybooster team, I was tasked with the mission to improve our team’s culture of documentation. The journey was filled with learning, challenges, and a fair bit of innovation. In this blog post, I’ll recount the steps I took to research, establish, and implement effective documentation practices in our project.

### Diving into TSDoc and JSDoc Comments

The first step in our journey was to standardize how we wrote comments in our Vue.js TypeScript codebase. After some research, we landed on TSDoc and JSDoc comments. TSDoc and JSDoc are documentation generators for TypeScript and JavaScript respectively. They use special syntax within code comments to generate documentation.

I spent time learning the ins and outs of TSDoc and JSDoc comments, experimenting with them in our codebase, and understanding how to use them effectively. To help my team consistently write TSDoc comments, I created and published a [cheatsheet](https://github.com/mustafadalga/tsdoc-cheatsheet) on GitHub. This cheatsheet became a vital tool for our team, ensuring that we all followed the same standards and made our code more understandable for anyone who might work on it in the future.

### On the Hunt for Tools and the Challenge of

The next stage in our journey was finding tools to auto-generate documentation from our TSDoc comments. After evaluating several options, we initially decided to use documentation.js. However, we quickly ran into a major roadblock. The tool struggled to support Vue.js’s `<script setup>` syntax, which we extensively used in our project.

We didn’t let this setback dampen our spirits, though. Seeing this as an opportunity, I went back to the drawing board and created an npm package called [vue-setup-doc](https://www.npmjs.com/package/vue-setup-doc). This utility helped to bridge the gap left by documentation.js. It successfully parsed our code that used `<script setup>` syntax, ensuring that no part of our codebase was left undocumented.

### From TSDoc Comments to Markdown Files

With our TSDoc comments in place and vue-setup-doc ready to go, the next step was to generate Markdown documentation files. We configured [vue-setup-doc](https://www.npmjs.com/package/vue-setup-doc) to scan our TypeScript files, parse the TSDoc comments, and generate corresponding Markdown files. This process made it incredibly easy for us to turn our code comments into readable and accessible documentation.

### Publishing with Vitepress

Once we had our Markdown files, we needed a way to publish our documentation. For this, we turned to Vitepress, a Vue-powered static site generator. We established a separate documentation project using Vitepress, where we configured it to render our Markdown files into a well-structured and user-friendly documentation website.

### A Three-Step Process for Effective Documentation

This journey led us to establish a three-step process for effective documentation:

1. Writing TSDoc comments in our code
    
2. Generating Markdown documentation files using vue-setup-doc
    
3. Publishing our documentation using Vitepress
    

Each step of this process is crucial in maintaining a high standard of documentation in our project.

### Reflections and Lessons Learned

Looking back, the motivation behind undertaking this project was simple: to make our team’s work more understandable, accessible, and maintainable. I am proud to say that we achieved these goals. The process of improving our documentation culture was a challenging one, but it was also incredibly rewarding. It has contributed to a better working environment, where knowledge is shared more effectively, and code maintainability is greatly enhanced.

The lessons I’ve learned throughout this journey have been invaluable. The importance of good documentation cannot be overstated. It is not just about explaining what a piece of code does, but why it does it that way. This clarity leads to better collaboration and a more robust codebase.

Another key takeaway is the power of building custom solutions when existing tools fall short. The creation of vue-setup-doc was a response to a real challenge we faced, and it taught me that sometimes, the best way forward is to pave your path.

### Looking Forward

This journey of improving our team’s culture of documentation is far from over. As we continue to grow and evolve, so will our processes and tools. I am excited to keep sharing our experiences and improvements in future blog posts.

I hope that sharing our journey helps others facing similar challenges in their projects. Remember, good documentation is a journey, not a destination. It requires a commitment to continuous learning and improvement. But the rewards, as we’ve found, are well worth the effort.
