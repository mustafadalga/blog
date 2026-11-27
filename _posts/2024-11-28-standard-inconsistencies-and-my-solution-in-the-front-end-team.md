---
layout: post
title: "Standard Inconsistencies and My Solution in the Front End Team"
date: 2024-11-28 19:59:22 +0000
slug: standard-inconsistencies-and-my-solution-in-the-front-end-team
tags: [front-end-development, problem-solving, team-management, coding-standards, codingstandards]
lang: en
---


As a Front End Developer, I want to share in this blog post how I established a common coding standard within the project and team, and the problems I encountered during this process.

**Code Development and Standards**

After developing our code, we always send it to code review with a pull request. However, I noticed that adhering to certain standards was a challenge in this process. I was trying to establish a standard way of writing in the project, but we encountered many issues such as the following:

• **Variable Naming:** We were using camelCase, but some developers did not follow this standard.

• **Enum Naming:** Enum names in legacy code were written in camelCase, while in the new code PascalCase was used. Finally, we decided to name enums and their properties using PascalCase.

• **Function Definition:** We previously used arrow functions, but due to code order issues, we started using declaration functions. However, developers had difficulty transitioning to this new standard. For example, functions used in the onMounted hook were defined as arrow functions, which broke code order rules.

• **Vue.js Standards:** With Vue.js 3, we started using the composition API and TypeScript. However, some developers were still working with the options API or defineComponent.

• **Vue.js Component Block Order:** The code was being developed in the order of “script,” “template,” and “style.” Despite this, some developers still placed the template block at the top.

• **Props Naming:** Props were generally written in camelCase, but in some cases, kebab-case was used.

**Setting and Applying Standards**

To address these issues, I prepared documentation outlining the standards and rules I defined, and shared it with the team. However, I realized that the documentation was not being read, and the standards were not being taken seriously. This caused me to give the same feedback repeatedly during code review processes.

**Solution: Eslint, Prettier, and Lint-Staged**

When I noticed that documentation was not effective enough, I took the following steps:

1\. **Eslint Rules:** I configured Eslint rules to align with the standards in the documentation.

2\. **Code Formatting:** I set up .editorconfig and .prettierrc files to address code formatting issues.

3\. **Using Lint-Staged:** Since it was not feasible to fix all the old code, I used the lint-staged tool to focus only on the changes being committed. This ensured that the development became more consistent.

**Build and Demo Issues**

Another frequent problem during code review and demo was that the code would fail to build. This was often due to TypeScript errors or non-functional unit tests, which were discovered only during deployment. In such cases, the code was sent back to in-progress, and we had to review it twice. To prevent this:

• I configured the lint-staged tool to run unit tests and type checking before commits. This way, issues were identified and resolved before the commit stage.

---

I have explained the issues we experienced as a Front End team and how I resolved them in general. The standards I shared here are completely tailored to our team’s needs. Different teams may need to determine their own standards.

https://gist.github.com/mustafadalga/3b026306bae517c2d72ffa884185171c
