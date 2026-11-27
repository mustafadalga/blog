---
layout: post
title: "How to create multiple mixins in one file and using specific mixins in vue js"
date: 2021-11-22 06:55:26 +0000
slug: how-to-create-multiple-mixins-in-one-file-and-using-specific-mixins-in-vue-js
tags: [mixin, hooks, mixins, vue-mixin, composables]
lang: en
---



![Cover Image]({{ '/assets/images/635cbf9b-fb06-43e3-b3c9-4b6350fcccac.png' | relative_url }})

When we have features such as method, computed or watch, that we want to use in more than one component or view, we can create and use mixins to avoid repetitive codes.

In small projects, we can create and import a mixin file for each repetitive feature (method, computed, watch,etc…) and use it. However, as the project grows, creating a mixin file for each feature will cause the mixins folder to swell. Instead of creating many mixin files, we can create a main mixin file per category. It would be a better method to create mixin files according to the scope, collect the mixins in the relevant files, and import only the necessary mixin.

mixins

We gathered computed and methods that we use in more than one component/view related to insights under mixins/insights/index.js.

We have included only the mixins we want to use as follows.

![]({{ '/assets/images/02f71da8-34af-4bec-9491-59f260730453.png' | relative_url }})

import of specific mix

**Example usage in template:**

**Example usage in script:**

At the time of writing this article, we used mixins as the best practice. However, we can improve or change solutions over time.

*Originally published at* [*https://insight.heybooster.ai*](https://insight.heybooster.ai/how-to-create-multiple-mixins-in-one-file-and-using-specific-mixin-in-vue-js/) *on September 30, 2021.*
