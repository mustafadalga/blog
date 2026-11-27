---
layout: post
title: "Securing Next.js Apps: A Journey with JWT and Server-Side Cookies"
date: 2023-05-08 17:21:14 +0000
slug: securing-nextjs-apps-a-journey-with-jwt-and-server-side-cookies
tags: [security, jwt, nextjs, clean-architecture, json-web-tokens-jwt]
lang: en
---


![Cover Image]({{ '/assets/images/e342888b-900f-474c-8fb9-6db84459edfd.png' | relative_url }})

In this blog post, I will dive deep into my authentication solution implemented in our [heybooster](http://heybooster.ai) project. We'll explore how it utilizes JSON Web Tokens (JWT) stored securely in server-side cookies, and how state management is handled using Zustand.

### **1\. Login API**

Let's start at the beginning - the login process. Once a user logs in successfully, the server sends back a JWT. I've ensured that this token is securely stored in a server-side cookie.

```typescript
// /api/auth/login/route.ts
import { NextResponse } from "next/server";
import { cookies } from "next/headers";
import api from "@/_services/loginApiService";
function setJWTCookie(jwt: string, date: Date) {
    cookies().set({
        name: 'jwt',
        value: jwt,
        httpOnly: true,
        secure: true,
        path: '/',
        expires: date
    });
}

export async function POST(request: Request) {
// ... Rest of the login logic ...
    setJWTCookie(jwt,date);
// ... Rest of the login logic ...
}
```

The `setJWTCookie` function I developed ensures that the JWT is securely stored with properties like `httpOnly` and `secure`.

### **2\. Checking Authentication Status**

To determine if a user is authenticated, I inspect the JWT in the server-side cookie.

```typescript
// /api/auth/check/route.ts
export async function GET(request: NextRequest) {
    const jwt: string | undefined = cookies().get(CookieKeys.Jwt)?.value;
    const isAuthenticated: boolean = !!jwt;
    // ... Rest of the check logic ...
}
```

### **3\. Logging Out**

The logout process I've implemented is clear-cut: remove the JWT from the server-side cookie.

```typescript
// /api/auth/logout/route.ts
export async function POST(request: NextRequest) {
    cookies().delete(CookieKeys.Jwt);
    // ... Rest of the logout logic ...
}
```

### **4\. Client-Side Logic**

I've also established a custom hook named `useAuthStatusFetcher`. This is pivotal in fetching the authentication status from the server and then updating the Zustand store.

```typescript
import axios from "axios";
import { useCallback } from "react";
import { useQuery } from "react-query";
import { useAuthStore } from "@/_store/useAuthStore";

export interface IResponse {
    isAuthenticated: boolean
}

const fetchStatus = async () => {
    let status: boolean = false;
    try {
        const { data } = await axios.get<IResponse>('/api/auth/check');
        status = data.isAuthenticated;
    } catch (error) {
        // ....
    }

    return status;
}

export default function useAuthStatusFetcher() {
    const setIsAuthenticated = useAuthStore(state => state.setIsAuthenticated);
    const { refetch, } = useQuery([ "isAuthenticated" ], fetchStatus, {
        enabled: false,
        onSuccess: (isAuthenticated) => {
            setIsAuthenticated(isAuthenticated)
        }
    });

    const fetchAuthStatus = useCallback(() => {
        refetch();
    }, [ refetch ]);

    return { fetchAuthStatus };
}
```

### **5\. Zustand Store**

For state management in the project, I turned to Zustand, a minimal yet powerful solution.

```typescript
import { create, StoreApi } from 'zustand';

export interface State {
    isAuthenticated: boolean,
}

export interface Actions {
    setIsAuthenticated: (isAuthenticated: boolean) => void;
    reset: () => void
}

const initialState: State = {
    isAuthenticated: false
}

const createState = (set: StoreApi<State & Actions>["setState"]): State & Actions => ({
    ...initialState,
    setIsAuthenticated: (isAuthenticated) => set(({ isAuthenticated })),
    reset: () => set(initialState)
});

export const useAuthStore = create(createState);
```

### **6\. Form Handling**

After successful login, I've made sure that the form fetches the authentication status, updating the Zustand store accordingly.

```typescript
"use client";
import { useForm } from 'react-hook-form';
import axios from "axios";
import useAuthStatusFetcher from "@/_hooks/auth/useAuthStatusFetcher";

interface IFormInput {
    username: string;
    password: string;
}

export default function PopupLogin() {
    const { fetchAuthStatus } = useAuthStatusFetcher();

    const {
        register,
        handleSubmit,
    } = useForm<IFormInput>();
    
    const onSubmit = async (data: IFormInput) => {
        const { username, password } = data;

        try {
            await axios.post('api/auth/login', { username, password });
            fetchAuthStatus();
            // rest of the code

        } catch (error) {
            //rest of code
        }
    };

    return (
        <div
            className="grid place-items-center overflow-x-hidden overflow-y-auto fixed inset-0 z-50 bg-neutral-800/70">
            <div className="relative w-full max-w-md px-4">
                <div className="bg-white rounded-lg shadow relative">
                    <form onSubmit={handleSubmit(onSubmit)}
                          className="space-y-6 px-6 lg:px-8 pb-4 sm:pb-6 xl:pb-8">
                        <h3 className="text-xl font-medium text-gray-900">Sign in</h3>
                        <div>
                            <label htmlFor="username"
                                   className="text-sm font-medium text-gray-900 block mb-2 ">Your
                                username</label>
                            <input type="text"
                                   id="username"
                                   {...register('username', { required: true })} />
                        </div>
                        <div>
                            <label htmlFor="password"
                                   className="text-sm font-medium text-gray-900 block mb-2 ">Your
                                password</label>
                            <input type="password"
                                   id="password"
                                   {...register('password', { required: true })}/>
                        </div>

                        <button type="submit">
                            Login
                        </button>
                    </form>
                </div>
            </div>
        </div>
    );
};
```

### **7\. App State Initialization**

An essential part of the experience is that the app begins with the correct authentication status. My `AppStateInitializer` component ensures this by fetching the status during the initial page load.

```typescript
"use client";
import { ReactNode } from "react";
import AppStateInitializer from "@/_components/AppStateInitializer";

export default function Providers({ children }: { children: ReactNode }) {
    return (
            <AppStateInitializer/>
            {children}
    );
}
```

### **8\. Handling Authentication in Components**

To enhance the user experience, I've ensured components like the navbar dynamically change based on the authentication status.

```typescript
"use client";
import { useAuthStore } from "@/_store/useAuthStore";

const Header = () => {
    const isAuthenticated = useAuthStore(state => state.isAuthenticated);

    return (
        <nav
            className="sticky z-10 top-0 h-16 w-full px-6 py-3 flex items-center justify-between border-b border-gray-100 bg-white">
            <div className="flex items-center gap-3">
                {isAuthenticated && (
                    <div>
                        ...rest of code
                    </div>
                )}
            </div>

            <div>
                {isAuthenticated ? (
                    <div>
                        ...rest of code
                    </div>
                ) : (
                    <div>
                        ...rest of code
                    </div>
                )}
            </div>
        </nav>
    );
};

export default Header;
```

### **9\. Protecting Routes with High-Order Components (HOC)**

Security is paramount. To ensure unauthenticated users cannot access specific pages, I crafted a High-Order Component (HOC) named `withAuth`.

```typescript
import { ComponentType } from 'react';
import { useAuthStore } from "@/_store/useAuthStore";
import { useCommonStore } from "@/_store/useCommonStore";
import { redirect } from "next/navigation";

export default function withAuth(WrappedComponent: ComponentType<any>) {
    const AuthCheckComponent = (props: any) => {
        const isAuthenticated = useAuthStore(state => state.isAuthenticated);
        const apiStatuses = useCommonStore(state => state.apiStatuses);

        if (!apiStatuses?.isAuthenticated?.isLoaded) {
            return <Skeleton/>
        }

        if (!isAuthenticated) {
            redirect("/");
            return null;
        }

        return <WrappedComponent {...props} />;
    };

    AuthCheckComponent.displayName = `WithAuthCheck(${getDisplayName(WrappedComponent)})`;

    return AuthCheckComponent;
};

function getDisplayName(WrappedComponent: ComponentType<any>): string {
    return WrappedComponent.displayName || WrappedComponent.name || 'Component';
}
```

---

### **Conclusion**

Crafting an authentication system in Next.js that seamlessly integrates JWTs, server-side cookies, and Zustand was both a challenge and a triumph. As a single front-end developer, I delved deep into the intricacies of security and user experience, ensuring that each piece of the puzzle fit just right. This journey has been a testament to what's achievable with dedication, innovation, and a bit of code magic. If you're embarking on a Next.js project, I hope my solution inspires and guides your authentication endeavors. Here's to building secure and user-friendly web applications! 🌟
