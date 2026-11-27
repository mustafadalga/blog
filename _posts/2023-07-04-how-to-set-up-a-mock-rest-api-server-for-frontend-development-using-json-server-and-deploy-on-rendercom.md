---
layout: post
title: "How to Set Up a Mock REST API Server for Frontend Development Using json-server and Deploy on Render.com"
date: 2023-07-04 17:14:07 +0000
slug: how-to-set-up-a-mock-rest-api-server-for-frontend-development-using-json-server-and-deploy-on-rendercom
tags: [frontend-development, rest-api, mocking, backend-developments, mock-rest-api]
lang: en
---



![Cover Image]({{ '/assets/images/aa3b4cc1-98c2-4287-bece-65278c455f52.png' | relative_url }})

Frontend developers often face a common challenge: waiting for the backend to be ready. This can sometimes hinder the pace of front-end development, especially when specific data structures or endpoints are needed for functionality. One efficient solution to this is to mock the REST API. This approach not only allows front-end development to proceed independently but also acts as a guideline for back-end developers. In this guide, we will walk you through how to set up a mock REST API server using `json-server` and deploy it on [Render.com](http://Render.com).

### **Why Mock the REST API?**

By mocking the REST API:

1. Frontend development can continue without relying on the backend.
    
2. It provides a clear specification for the backend, reducing chances of misunderstanding.
    

The mock server can easily be updated as frontend requirements evolve.

### **Steps to Set Up the Mock Server:**

#### 1\. Initializing the Project:

Start with a `package.json` that includes the necessary scripts and dependencies:

```typescript
{
  "name": "mock-api-server",
  "version": "1.0.0",
  "scripts": {
    "start": "json-server db.json --routes routes.json -p 1000 -H 0.0.0.0"
  },
  "dependencies": {
    "json-server": "^0.17.3"
  }
}
```

#### 2\. Structure Mock Data:

Create a `db.json` file. This will contain all the mock data for your endpoints. Here's a sample:

```typescript
{
  "posts": [
    { "id": 1, "title": "Sample Post 1", "content": "Lorem ipsum dolor sit amet." },
    ...
  ],
  "users": [
    { "id": 1, "name": "John Doe", "email": "john@example.com" },
    ...
  ],
  ...
}
```

#### 3\. Define Custom Routes:

For more structured or nested routes, `json-server` provides the ability to rewrite routes. Create a `routes.json`:

```typescript
{
  "/v2/posts": "/posts",
  "/v2/users": "/posts",
  "/v2/comments": "/posts"
}
```

#### 4\. Deploy on [Render.com](http://Render.com):

* Set up a Node.js environment.
    
* Configure your `package.json` to initiate `json-server` on the correct port and host.
    
* Push the repository to a GitHub platform.
    
* Deploy on [Render.com](http://Render.com) using the linked repository.
    
* Once deployed, you'll get a URL from Render, e.g., [`https://mock-api-server.onrender.com`](https://mock-api-server.onrender.com).
    

Use this URL to access your mock API endpoints:

* [`https://mock-api-server.onrender.com/v2/users`](https://mock-api-server.onrender.com/v2/users)
    
* [`https://mock-api-server.onrender.com/v2/comments`](https://mock-api-server.onrender.com/v2/comments)
    
* [`https://mock-api-server.onrender.com/v2/posts`](https://mock-api-server.onrender.com/v2/posts)
    

#### 5\. Easy Updates:

Whenever you need to modify the API, make changes to the `db.json` and `routes.json` files, and push the updates. [Render.com](http://Render.com) will automatically reflect the changes in the mock REST API.

### **Conclusion:**

Establishing a mock REST API server is invaluable for simplifying the development process. It not only ensures the frontend can be developed and tested independently but also provides a clear blueprint for the backend. Tools like `json-server` combined with platforms like [Render.com](http://Render.com) make this process straightforward, even for those with minimal backend experience. If you aim to optimize the frontend-backend collaboration, considering a mock server is a step in the right direction!
