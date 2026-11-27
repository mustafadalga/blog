---
layout: post
title: "Solving Next.js Caching Issues on AWS Amplify"
date: 2023-10-24 10:15:31 +0000
slug: solving-nextjs-caching-issues-on-aws-amplify
tags: [web-performance, frontend-development, nextjs, amplify, nextjs-13]
lang: en
---


Developing with Next.js and deploying on AWS Amplify offers many advantages, but we encountered a significant hurdle: caching issues. Despite updates, users kept seeing outdated content, thanks to stubborn caching. Our journey to resolve this led us down several paths before finding success.

**Initial Attempt to Break the Cache**

We tried leveraging Next.js features, setting 'force-dynamic' and 'revalidate' options, hoping to invalidate old caches:

```typescript
const dynamic = 'force-dynamic';
const revalidate = 0;
```

This didn’t yield the results we expected; users still encountered old content post-deployment.

**The Winning Move: Custom Headers**

Our breakthrough came when we decided to take control of caching behavior with AWS Amplify's custom headers. We added a 'no-store' directive, ensuring browsers wouldn’t cache our pages:

```typescript
customHeaders:
  - pattern: '**/*'
    headers:
      - key: Cache-Control
        value: no-store
```

Success! This adjustment finally allowed users to see the most recent content after each deployment, resolving our stale content dilemma.

---

### **In Conclusion**

This experience was a reminder: that understanding the intricacies of our tools is crucial. While we sacrificed some performance benefits by disabling caching, the trade-off ensured our content stayed fresh. Sometimes, solutions require thinking outside of the conventional scope and tweaking configurations creatively.
