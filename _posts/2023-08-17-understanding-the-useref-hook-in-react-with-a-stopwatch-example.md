---
layout: post
title: "Understanding the useRef Hook in React with a Stopwatch Example"
date: 2023-08-17 20:20:42 +0000
slug: understanding-the-useref-hook-in-react-with-a-stopwatch-example
tags: [react, vuejs, reactjs, frontend-development, nextjs]
lang: en
---


![Cover Image]({{ '/assets/images/1e4e4a5f-2cb7-4be3-8ca6-2c1150f21394.png' | relative_url }})

In React, the `useRef` hook allows us to access and interact with DOM elements directly. But more than that, `useRef` can also be used to persist values across renders without triggering a re-render. Let's delve into this concept using a simple stopwatch example.

#### **1\. Basic Setup**

We start by importing the necessary hooks and setting up our component:

```typescript
import React, { useState, useRef } from 'react';

function Stopwatch() {
  const [time, setTime] = useState(0);
  const intervalRef = useRef(null);
  // ...
}
```

#### **2\. Storing Interval ID with useRef**

We'll use an interval to update our stopwatch's time. To ensure we can clear this interval later (when stopping the stopwatch), we need to keep a reference to its ID. This is where `useRef` comes in handy:

```typescript
function handleStartClick() {
  if (intervalRef.current !== null) return;  // Prevent multiple intervals

  const intervalId = setInterval(() => {
    setTime(prevTime => prevTime + 1);
  }, 1000);

  intervalRef.current = intervalId;
}
```

Notice how we assign the interval ID to `intervalRef.current`. Unlike the state, updating a ref's value does not cause a re-render.

#### **3\. Accessing the Stored Interval ID**

Since the interval ID is stored in our ref, we can access it anytime. This is particularly useful when we want to stop the stopwatch:

```typescript
function handleStopClick() {
  clearInterval(intervalRef.current);
  intervalRef.current = null;
}
```

Here, we retrieve the interval ID with `intervalRef.current` and clear the interval. After that, we nullify the ref to indicate that the interval is no longer running.

#### **4\. Display and Interaction**

Finally, we render the elapsed time and provide buttons for user interactions:

```typescript
return (
  <div>
    <p>Time: {time} seconds</p>
    <button onClick={handleStartClick}>Start</button>
    <button onClick={handleStopClick}>Stop</button>
  </div>
);
```

#### **Conclusion**

The `useRef` hook in React offers a way to persist values across component renders without causing unnecessary re-renders. In our stopwatch example, we saw how `useRef` can be used to keep track of the interval ID, allowing for smooth interactions and precise control over the stopwatch's behavior. This pattern can be applied in various scenarios where you need to retain a piece of information across renders without affecting the component's output.

resource: [https://react.dev/reference/react/useRef#avoiding-recreating-the-ref-contents](https://react.dev/reference/react/useRef#avoiding-recreating-the-ref-contents)
