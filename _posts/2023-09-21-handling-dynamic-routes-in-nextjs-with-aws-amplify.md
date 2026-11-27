---
layout: post
title: "Handling Dynamic Routes in Next.js with AWS Amplify"
date: 2023-09-21 15:22:15 +0000
slug: handling-dynamic-routes-in-nextjs-with-aws-amplify
tags: [reactjs, nextjs, dynamic-routing, vercel, awsamplify]
lang: en
---


![Cover Image]({{ '/assets/images/d4049a71-ef94-4b56-ac4e-a79e2f625755.png' | relative_url }})

I recently encountered an interesting challenge while working on a Next.js project. I had developed a feature using dynamic routes, and while it functioned flawlessly in my local environment and on Vercel, deploying the same on AWS Amplify resulted in a persistent "Internal Server Error". This got me wondering if Amplify inherently had issues with Next.js dynamic routes.

### **The Problem**

When attempting to access the dynamic route [`https://dynamic-routes-on-amplify.d5xhiu5ptui07.amplifyapp.com/post/edit/1`](https://dynamic-routes-on-amplify.d5xhiu5ptui07.amplifyapp.com/post/edit/1), AWS Amplify would return an "Internal Server Error".

### **Exploring Solutions**

To resolve this issue, I tried different strategies:

**Server-Side Rendering (SSR) with Dynamic Routes**: In this approach, I fetched the post data on the server side using the dynamic route's parameter.

```typescript
// SSR URL Schema:/post/edit/${id}
import axios from "axios";

const fetchPost = async (id: string) => { ... };

export default async function Page({ params: { id } }) { ... }
```

**Client-Side Rendering (CSR) with Dynamic Routes**: I then tried fetching the post data on the client side after the page had loaded.

```typescript
// CSR URL Schema:/post/edit/${id}

"use client";
import axios from "axios";
import { useEffect, useState } from "react";

export default function Page({ params: { id } }) { ... }
```

1. **Dynamic Route Using Search Params**: As a variation, I also tried fetching the post data on the server side using a query string.
    

```typescript
// SSR URL Schema:/post/edit?id=${id}
import axios from "axios";
import { redirect } from "next/navigation";

export default async function Page({ searchParams }) { ... }
```

Unfortunately, none of the above solutions resolved the issue on Amplify.

### **The Workaround**

After multiple attempts, I finally landed on a workaround that used static routes with query search parameters on the client side.

```typescript
// CSR URL Schema:/post/edit?id=id
"use client";
import axios from "axios";
import { useEffect, useState } from "react";
import { redirect, useSearchParams } from "next/navigation";

export default function Page() { ... }
```

Instead of relying on dynamic routes, this approach uses a static route and fetches the post data based on the `id` provided as a query parameter. This solution worked seamlessly on Amplify.

### **Visualizing the Issue and Solution**

For clarity, here's a visualization of the routes I tried and their outcomes:

```typescript
{posts.map((post: Post) => (
    <>
        <Link key={post.id} href={`/post/edit/${post.id}`}>
            V1 SSR URL Schema:/post/edit/id - <span className="text-red-600">Doesn't work!</span>
        </Link>
        <Link key={post.id} href={`/post-v2/edit/${post.id}`}>
            V2 CSR URL Schema:/post/edit/id - <span className="text-red-600">Doesn't work!</span>
        </Link>
        <Link key={post.id} href={`/post-v3/edit?id=${post.id}`}>
            V3 CSR URL Schema:/post/edit?id=id - <span className="text-lime-600">Working!</span>
        </Link>
        <Link key={post.id} href={`/post-v4/edit?id=${post.id}`}>
            V4 SSR URL Schema:/post/edit?id=id - <span className="text-red-600">Doesn't work!</span>
        </Link>
    </>
))}
```

### **Conclusion**

While AWS Amplify is a powerful platform for hosting and deploying web applications, it's crucial to be aware of its nuances, especially when working with frameworks like Next.js. If you ever face similar challenges with dynamic routes or any other feature, remember to experiment with different rendering methods and always keep an eye out for workarounds. After all, problem-solving is at the heart of engineering!

Demo : [https://github.com/mustafadalga/nextjs-playground/tree/dynamic-routes-on-amplify](https://github.com/mustafadalga/nextjs-playground/tree/dynamic-routes-on-amplify)

Repository : [https://github.com/mustafadalga/nextjs-playground/tree/dynamic-routes-on-amplify](https://github.com/mustafadalga/nextjs-playground/tree/dynamic-routes-on-amplify)
