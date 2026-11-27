---
layout: post
title: "Global React-Query Setup with RootProvider in Next.js"
date: 2023-08-13 21:31:53 +0000
slug: global-react-query-setup-with-rootprovider-in-nextjs
tags: [vuejs, reactjs, nextjs, state-management, react-query]
lang: en
---


![Cover Image]({{ '/assets/images/634deb2a-1406-4b7b-8805-47f81edaa822.png' | relative_url }})

In the vast ocean of front-end development, I often find myself navigating through the waves of data fetching, caching, and state management. In my journey with Heybooster, while transitioning from Vue.js to Next.js, I discovered the magic of `react-query` and decided to incorporate it. Here's my take on the integration.

### **Why did I choose react-query for Heybooster?**

1. **Simplifying Caching:** With React-Query, I no longer needed to wrangle with caching; it was like having an assistant that took care of it all.
    
2. **Always Updated:** The data in the application gets a fresh update in the background, ensuring our users always get the latest.
    
3. **Developer-friendly:** React-Query’s built-in dev tools are a lifesaver for debugging and data inspection.
    
4. **No More Boilerplate:** Writing caching mechanisms manually was a thing of the past. With hooks like `useQuery`, data fetching became a walk in the park.
    

In our previous Vue.js version, I found myself writing custom code for caching. But React-Query swooped in and saved the day, streamlining our codebase.

### **The Need for a Global Client Provider**

React-Query's heart is the `QueryClient`. This is where all the magic happens - cache management, query configurations, and more. To ensure every component, regardless of where it's nested, can access this magic, I wrapped our entire app with the `QueryClientProvider`.

This design ensured that every corner of our app had consistent access to the features and configurations provided by `react-query`.

### **My Approach: Making React-Query Client Global with RootProvider**

I took a strategic decision to embed the `QueryClientProvider` within a `RootProvider`. Here's my rationale:

1. **Single Source of Truth:** By placing the `QueryClient` within the `RootProvider`, I ensured that there was a centralized configuration. This means easy management and updates.
    
2. **Scalability:** A centralized provider like this makes future expansions, such as adding more providers or configurations, a breeze.
    
3. **Uniformity:** A global provider ensures uniform behavior across the app, since every component taps into the same client.
    

**RootProvider.tsx**

This is where the magic begins. Heybooster initializes the `QueryClient` and wraps the application with `QueryClientProvider`, making the client available to all child components.

```typescript
import { QueryClient, QueryClientProvider } from "react-query";
...

export default function Providers({ children }: { children: ReactNode }) {
    const [ client ] = useState(new QueryClient());

    return (
        <QueryClientProvider client={client}>
            ...
            {children}
        </QueryClientProvider>
    );
}
```

This global provider ensures that any component, no matter how deeply nested, can utilize the power of React-Query without additional setup.

**layout.tsx**

Here, the entire application layout is defined. Notably, the application is wrapped with the `RootProvider` to ensure that React-Query's functionalities are available throughout.

```typescript
...

export default function RootLayout({ children }: Props) {
    return (
        ...
        <RootProvider>
            ...
            {children}
        </RootProvider>
        ...
    )
}
```

**useFetchData.ts**

This is a custom hook that demonstrates how to use `react-query` for data fetching. It fetches data from a Next.js API route and provides the fetched data and a loading state.

```typescript
import { useQuery } from "react-query";
...

export default function useFetchCustomVariables(): { variables: string[], isLoading: boolean } {
    const { data: variables = [], isLoading } = useQuery<string[]>("data",fetchData);

    return {
        variables,
        isLoading
    }
}
```

---

React-Query is a robust tool for managing server state in React applications. By globally structuring the QueryClient in RootProvider, I have ensured a streamlined and consistent approach to data fetching and state management. It's a testament to how modern libraries can significantly simplify and optimize traditionally complex tasks in front-end development.
