---
layout: post
title: "Using Production Environment Variables in Next.js Development Mode"
date: 2023-01-21 09:00:27 +0000
slug: using-production-environment-variables-in-nextjs-development-mode
tags: [reactjs, nextjs, next, environment-variables, nextjs-132]
lang: en
---


![Cover Image]({{ '/assets/images/21e7eaa3-0bb8-4414-835d-44b9e325bae3.png' | relative_url }})

**Problem:**

When developing applications with Next.js, developers often need to test their local changes with different sets of environment variables. By default, Next.js provides a way to use environment variables through `.env` files. Typically, you'd have a `.env.development` for local development and a `.env.production` for your production setup. However, there arises a challenge when you want to run your local development server with production environment variables. Simply changing the `NODE_ENV` to `production` doesn't do the trick, as `next dev` always assumes a development environment.

**Solution:**

Next.js has a built-in feature where the `.env.local` file overrides any other environment configuration. Leveraging this, we can copy our desired environment's variables into `.env.local` before starting our development server.

Here's how you can set this up:

1. Create your environment files:
    
    * `.env.development`: Contains environment variables for the development environment.
        
    * `.env.production`: Contains environment variables for the production environment.
        

Update your `package.json` scripts:

```json
{
  "scripts": {
    "dev": "cp .env.development .env.local && next dev",
    "dev:prod": "cp .env.production .env.local && next dev"
  }
}
```

With these scripts in place:

* Running `npm run dev` will use the development environment variables.
    
* Running `npm run dev:prod` will use the production environment variables in development mode.
    

**Benefits:**

1. **Flexibility**: This approach allows for easy switching between different environment variables without manually altering files or configurations.
    
2. **Consistency**: By using a consistent method, there's less room for error, ensuring that the right environment variables are always loaded.
    
3. **Leverage Built-in Features**: This solution utilizes Next.js's existing behavior of giving precedence to `.env.local`, making it a straightforward approach without the need for additional packages or complex setups.
    

---

**Conclusion**:

Managing environmental variables can sometimes be a challenge in development. However, with this simple trick, you can easily switch between different sets of environment variables in Next.js, making your development process smoother and more efficient.
