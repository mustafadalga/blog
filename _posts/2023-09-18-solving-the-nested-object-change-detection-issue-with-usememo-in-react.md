---
layout: post
title: "Solving the Nested Object Change Detection Issue with useMemo in React"
date: 2023-09-18 12:25:16 +0000
slug: solving-the-nested-object-change-detection-issue-with-usememo-in-react
tags: [web-performance, reactjs, nextjs, usecallback, usememo]
lang: en
---


![Cover Image]({{ '/assets/images/703db673-39c9-4e67-80df-c9fb6cf2141f.png' | relative_url }})

When it comes to performance optimization in React, `useMemo` is a widely used hook. It's handy for avoiding expensive calculations on every render by memorizing the computed value. However, `useMemo` performs a shallow comparison of its dependencies, which can be an issue when you're dealing with nested objects. This post will demonstrate how to address this limitation by implementing deep comparison logic.

### **The Problem**

In React, the `useMemo` hook only performs a shallow comparison of its dependencies. This means it does not detect changes in nested structures, causing it to not recompute when an inner property of an object changes.

Here's a simplified example:

```typescript
const sorts = useMemo(() => {
  // Some expensive computation
}, [fields, customFields]);
```

In the above example, if a nested property within `fields` or `customFields` changes, `useMemo` will not recognize it, leading to potentially stale or incorrect data.

### **The Non-optimal Approach: Deep Cloning**

You might think that a simple solution would be to deep-clone the objects:

```typescript
const memoizedFields = cloneDeep(fields);
const memoizedCustomFields = cloneDeep(customFields);
```

However, this approach always creates new objects, leading `useMemo` to recompute the value every time the component renders, which defeats the purpose of memoization.

### **The Optimal Solution: Deep Comparison with Memoization**

The best way to tackle this issue is to implement deep comparison logic using a custom hook. Here's how you can do it:

```typescript
import { useRef } from 'react';
import { isEqual, cloneDeep } from 'lodash';

const useDeepCompareMemoize = (value) => {
  const ref = useRef();
  if (!isEqual(value, ref.current)) {
    ref.current = cloneDeep(value);
  }
  return ref.current;
};
```

We use the `isEqual` function from Lodash for deep equality checks and `cloneDeep` for deep cloning. Now you can use this custom hook to memoize the deeply nested objects:

```typescript
const memoizedFields = useDeepCompareMemoize(fields);
const memoizedCustomFields = useDeepCompareMemoize(customFields);

const sorts = useMemo(() => {
  // Some expensive computation
}, [memoizedFields, memoizedCustomFields]);
```

With this setup, `useMemo` will only recompute its value if there is an actual change in the deep structure of `fields` or `customFields`.

---

Deep comparison in React can be tricky but is essential when dealing with nested objects and arrays. By using a custom hook that leverages deep comparison and deep cloning, you can ensure that `useMemo` behaves as expected even with nested structures.

This approach does introduce some complexity and relies on external libraries like Lodash, but it effectively addresses the limitations of `useMemo`'s shallow comparison. Always consider the trade-offs, especially the computational cost of deep equality checks and cloning for large or deeply nested objects.
