---
layout: post
title: "JavaScript to TypeScript Migration in a Vue.js Project"
date: 2023-05-26 08:08:53 +0000
slug: javascript-to-typescript-migration-in-a-vuejs-project
tags: [javascript, vuejs, migration, reactjs, typescript]
lang: en
---

After we upgraded our Vue.js 2 project to Vue.js 3, we decided to move the codebase from JavaScript to TypeScript. In a big project, having static types makes it easier to catch bugs before runtime, gives you much better autocomplete in the editor, and makes the code easier to follow.

The problem was that we had a lot of old `.js` files. We didn't have the time to rewrite every single file manually into TypeScript, and trying to refactor all of them at once would stop all other development work.

As a quick solution, we wrote a small bash script to change all `.js` file extensions in the project to `.ts`. The script didn't just rename the files, it also added `//@ts-nocheck` to the very first line of each file.

Because of `//@ts-nocheck`, the TypeScript compiler skipped type checking on those old files, so all our existing code kept running without breaking anything. I shared that script on [GitHub](https://github.com/mustafadalga/file-extension-converter).

Of course this was only the first step of the migration. The plan going forward is that whenever someone edits a file, they remove `//@ts-nocheck` from the top and add proper types there. Doing it step by step like this lets you migrate a big project without freezing everything and rewriting it from scratch.