---
layout: post
title: "Heybooster Front End Project: Optimization with Clean Code Principles"
date: 2023-05-26 11:00:07 +0000
slug: heybooster-front-end-project-optimization-with-clean-code-principles
tags: [software-architecture, software-engineering, clean-code, software-engineer, software-engineers]
lang: en
---


![Cover Image]({{ '/assets/images/bf6a04b9-bf43-40c2-837b-2d7a73213ee8.png' | relative_url }})

In the process of software development, it’s paramount for the code to be sustainable, readable, and manageable. By embracing and implementing these principles, I have made significant improvements in our [Heybooster](https://app.heybooster.ai/) Front End project. In this blog post, I will explain how code optimization and clean code principles were applied and how these improvements affected the project.

#### Reorganization of Constant Enum Variables

In our Vue.js project, there were numerous components, layouts, and constant variables used within components. To facilitate the management of these variables and eliminate their repetition, I gathered all constant enum variables under an ‘enums’ directory. By doing this, I deleted all repeating enum variables and began to import and use them from the ‘enums’ directory whenever needed. This change enabled the code to be more optimized and eliminated repeating parts.

#### Creation of a Constants Directory

I applied a similar approach to constants as well. I gathered all repeating constants under a ‘constants’ directory. From now on, constants are imported and used from this directory only. This change allowed constants to be defined only once and helped make the code more concise and manageable.

#### Optimizing Repeating Functions in API Requests with Vuex Actions

There were repeating functions in the project for multiple components and API requests. I recreated these repeating functions with Vuex actions and deleted the repeating methods. Thus, I wrote the code performing the same function only once and began to use these actions wherever needed.

#### Correction of Repeating Codes with Composable Functions

Various code structures such as methods, functions, computed properties, and others built utilizing Vue.js were frequently duplicated throughout our project. To address this redundancy and streamline our codebase, I created composable functions as replacements. After eliminating the redundant instances of methods, functions, computed properties, and similar codes, I started using these composable functions, thereby simplifying our code structure.

#### **Gathering Utility Functions**

In the project, there were utility functions that were recreated and used in multiple places for various operations. To facilitate the management of these codes and eliminate the repeating parts, I gathered these codes under a directory named ‘utility’. Hence, I began to manage and use these utility functions from one place, prevented code repetition, and optimized the code.

#### **Creating an Axios Wrapper Plugin**

It can be a time-consuming process to import axios and configure it for every API request. To speed up this process and make it more efficient, I created an axios wrapper plugin. I defined this plugin globally and started using it everywhere in the project. This way, I eliminated the problem of wasting time importing axios and configuring it every time.

#### Conclusion

These changes and improvements have made the code of our [Heybooster](https://app.heybooster.ai) Front End project more concise, manageable, and optimized. The application of clean code principles facilitated the maintenance of our project. As a software developer, the sustainability and quality of the code have always been my priorities, and these principles allow us to work more effectively in our projects. These types of improvements and optimizations not only enhance the quality of the code but also facilitate teamwork and contribute to the overall success of the project.
