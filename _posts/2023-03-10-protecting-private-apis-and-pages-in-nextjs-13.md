---
layout: post
title: "Protecting Private APIs and Pages in Next.js 13"
date: 2023-03-10 20:19:59 +0000
slug: protecting-private-apis-and-pages-in-nextjs-13
tags: [jwt, nextjs, nextjs-13, nextjs-middleware, httponly]
lang: en
---


![Cover Image]({{ '/assets/images/5c751430-c3a2-424a-a12e-6411b4e5689c.png' | relative_url }})

Next.js has always been a favorite for its simplicity and powerful features. With the release of Next.js 13, the framework introduced a new way to handle middleware, making it even easier to add protection layers to your application. In this post, I'll walk you through how I manage and protect private APIs and pages in my Next.js 13 project using global middleware.

#### **1\. The Importance of JWT and HttpOnly Cookies**

Before diving into the middleware, it's essential to understand the authentication mechanism I use. JSON Web Tokens (JWT) are a compact, URL-safe means of representing claims to be transferred between two parties. In my setup, I store the JWT in a secure HttpOnly cookie. This approach ensures that the token cannot be accessed via JavaScript on the client side, adding an extra layer of security against potential XSS attacks.

#### **2\. Setting Up Global Middleware**

Next.js 13 allows you to define global middleware that runs before every request. This is the perfect place to add our authentication checks.

Here's the middleware in its entirety:

```typescript
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
import { CookieKeys } from "@/_enums";

const authenticatedRoutes = [
    "/dashboard",
    "/profile",
    "/settings",
    "/orders",
    "/inbox",
    "/favorites",
];
const authenticatedApis = [
    "/api/user",
    "/api/orders",
    "/api/profile",
    "/api/settings",
    "/api/messages",
    "/api/favorites",
];

export function middleware(request: NextRequest) {
    const isAuthenticated: boolean = !!(request.cookies.get(CookieKeys.Jwt)?.value);
    const pathName = request.nextUrl.pathname;

    if (!isAuthenticated) {
        if (authenticatedRoutes.includes(pathName)) {
            const url = new URL('/', request.url);
            return NextResponse.redirect(url.toString());
        }
        if (authenticatedApis.includes(pathName)) {
            return NextResponse.json({ message: "You are not authorized to access this resource." }, { status: 403 });
        }
    }

    return NextResponse.next();
}
```

#### **3\. How the Middleware Works**

* **Authenticated Routes & APIs**: I maintain two arrays, one for private pages and another for private API endpoints. Any request to these routes or endpoints will need to pass the authentication check.
    
* **Middleware Function**: The middleware function checks if the JWT exists in the cookies. If it doesn't, it determines if the request is for a private page or a private API and takes the appropriate action.
    

#### **4\. Handling Unauthenticated Requests**

For unauthenticated requests:

* **Private Pages**: Users are redirected to the homepage (or a login page if you have one). This ensures that only authenticated users can access private content.
    
* **Private APIs**: Instead of a redirect, the server responds with a 403 (Forbidden) status and a user-friendly message. This approach provides clear feedback to the client or developer about the authentication requirement.
    

#### **5\. Advantages of This Approach**

* **Simplicity**: The middleware is concise and easy to understand. It provides a centralized place for authentication checks, ensuring consistency across the application.
    
* **Flexibility**: By maintaining arrays of private routes and APIs, it's straightforward to add or remove protected endpoints. This modular approach means less room for error and easier maintenance.
    
* **Security**: Using JWTs with HttpOnly cookies ensures that even if there's an XSS vulnerability, attackers cannot access the JWT via JavaScript.
    

#### **Conclusion**

Protecting private content is crucial for any web application. With Next.js 13's new middleware capabilities, it's never been easier to add robust protection layers to your project. By combining JWTs, HttpOnly cookies, and global middleware, you can ensure that your private pages and APIs remain secure and accessible only to authenticated users.
