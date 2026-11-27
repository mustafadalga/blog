---
layout: post
title: "Going Beyond SCSS with TailwindCSS: Clean Code and CSS Optimization"
date: 2023-05-26 10:07:35 +0000
slug: going-beyond-scss-with-tailwindcss-clean-code-and-css-optimization
tags: [frontend, migration, frontend-development, tailwind, tailwind-css]
lang: en
---



![Cover Image]({{ '/assets/images/7b629886-59bf-408a-a8c3-1f61136666f0.png' | relative_url }})

In this blog post, I would like to discuss a problem I encountered some time ago and how I solved it.

Working with my project team, we were using Vue.js to develop a series of components. We managed the UI design of our project using SCSS (SASS) as a pre-processor. We created utility classes with SCSS, but we didn’t create specific utility classes for non-generic widths like 20px, 30px, 80px, unless they were needed in specific cases. This resulted in repetitive code whenever we needed such widths, leading to code duplication. You can take a look at the utility classes I created with SCSS in this [gist](https://gist.github.com/mustafadalga/3a105e7ef96da1dc85d4294902265eae).

Over time, I realized that I was writing repetitive CSS code. For example, I would use `padding: 32px` in maybe a hundred different places. This not only increased the file size unnecessarily but also violated the principles of clean code.

I needed to find a solution to this problem. As a solution, I decided to use TailwindCSS. When I started using this CSS framework, I had the ability to build only the necessary classes. For example, instead of using `padding: 32px` in 100 different places, I could create the `p-[2rem]` class just once. This eliminated repetitive code.

#### Why did I choose TailwindCSS over SCSS, and what are the differences between them?

TailwindCSS is a “utility-first” CSS framework. It allows us to create customizable, reusable, and readable CSS classes. Each class defines a specific styling property (e.g., padding, margin, width, etc.). This greatly reduces code duplication and gives us more control. Especially in large-scale projects, TailwindCSS enables scalable and manageable code.

On the other hand, SCSS serves more as a traditional CSS pre-processor. SCSS-generated style files tend to be larger and more complex. It encompasses variables, mixins, functions, and more. While these features provided by SCSS is useful, they can also lead to code complexity and expansion.

Therefore, I chose TailwindCSS to avoid repetitive CSS code and create a cleaner, more scalable codebase. The utility-first approach of TailwindCSS prevents code duplication by reusing classes created once and used in multiple places, allowing us to write code faster and more efficiently.

In conclusion, there is no definite winner between TailwindCSS and SCSS. Both are suitable for specific situations and needs. However, in my case, TailwindCSS’s scalability and flexibility reduced complexity and made the code cleaner and more organized. Additionally, by reusing utility classes in our project, we significantly reduced the size of our codebase.

Through this blog post, I hope to provide another reason for front-end engineers to consider TailwindCSS when seeking a solution to similar problems. Based on my experience, I believe that TailwindCSS’s utility-first approach can help projects progress faster and more efficiently.

I hope this blog post sufficiently explains the problems and solutions encountered by you and your team. Please let me know if any edits or additional information are required.
