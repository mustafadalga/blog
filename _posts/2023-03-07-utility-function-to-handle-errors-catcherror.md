---
layout: post
title: "Utility Function to Handle Errors: catchError"
date: 2023-03-07 12:53:49 +0000
slug: utility-function-to-handle-errors-catcherror
tags: [best-practices, clean-code, axios, clean-architecture, handle-errors]
lang: en
---


![Cover Image]({{ '/assets/images/609a2017-1b88-4a80-993e-454afe907b13.png' | relative_url }})

Developing a utility function to handle errors is crucial for maintaining a clean codebase and providing meaningful feedback to users. The goal of this utility, `catchError`, is to streamline error handling, particularly for Axios requests, but also for general JavaScript errors. By centralizing the error-handling logic, we can ensure consistency across the application and simplify the error handling in individual `catch` blocks.

```typescript
import { isAxiosError } from 'axios';

interface IAxiosError {
    message: string,
    status: number
}

export function catchError(error: any, defaultMessage: string = "An error occurred. Please try again later."): IAxiosError {
    if (isAxiosError(error)) {
        if (error.response) {
            const { data: { message }, status } = error.response;
            return {
                status,
                message: message || defaultMessage
            }
        }

        if (error.request) {
            return {
                status: -1,
                message: "No response from the server. Please check your internet connection."
            }
        }

        return {
            status: -2,
            message: "An unexpected error occurred while setting up the request."
        }
    }

    return {
        status: -3,
        message: error.message || defaultMessage
    }
}
```

**Usage**:

To use the `catchError` utility, simply call it within a `catch` block:

```typescript
try {
    // Some code that might throw an error, e.g., an Axios request
} catch (error) {
    const { message, status } = catchError(error);
    console.error(`Error ${status}: ${message}`);
    // Handle the error, e.g., show a toast or alert with the message
}
```

**Note**:

The utility function assigns custom status codes to certain error scenarios:

* `-1`: No server response (e.g., network issues).
    
* `-2`: Axios-specific error not tied to server response.
    
* `-3`: General JavaScript error not related to Axios.
