---
layout: post
title: "Setting Up a Mock REST API Server for Frontend Development"
date: 2023-07-03 16:54:41 +0000
slug: setting-up-a-mock-rest-api-server-for-frontend-development
tags: [postman, frontend-development, rest-api, mock-server, mock-api-server]
lang: en
---


![Cover Image]({{ '/assets/images/41baa9a1-a433-426a-b7a4-b79d6a74a3f8.png' | relative_url }})

When developing the front end of an application, waiting for the back end to be ready can sometimes slow down the development process. This is especially true when the front end requires specific data structures or endpoints to function correctly. A solution? Mock the REST API! This not only allows frontend development to proceed independently but also serves as a specification for backend developers. In this post, we'll explore how we set up a mock REST API server using `json-server` and deployed it on [Render.com](http://Render.com).

**The Challenge**:

The main goal was to create a mock REST API server that could mimic the expected responses from a real backend. The server needed to be flexible, allowing updates to the mock data as frontend requirements evolved. Additionally, it was crucial to deploy the mock server online so that it could be shared with both frontend and backend teams.

**The Solution:** `json-server` **and** [**Render.com**](http://Render.com):

1. **Choosing** `json-server`:
    
    We opted for `json-server`, a zero-coding tool that can generate a full mock REST API based on a simple JSON file. It supports various HTTP methods like `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`, making it versatile for mimicking a real RESTful API.
    
2. **Structuring the Mock Data**:
    
    The mock data was structured in a `db.json` file. Initially, there were challenges in structuring nested routes, but with `json-server`'s route rewrites, we could mimic almost any API structure.
    
3. **Deploying on** [**Render.com**](http://Render.com):
    
    [Render.com](http://Render.com) was chosen for deployment because of its simplicity and integration with Git repositories. The steps included:
    
    * Setting up a Node.js environment.
        
    * Configuring the `package.json` to start `json-server` on the correct port and host.
        
    * Pushing the repository to a Git platform.
        
    * Deploying on [Render.com](http://Render.com) using the repository.
        

**Benefits Achieved**:

1. **Independent Frontend Development**:
    
    With the mock server in place, front-end development could proceed without waiting for the backend. This led to faster development cycles and early identification of potential UI/UX issues.
    
2. **Clear Backend Specifications**:
    
    The mock server served as a clear specification for the backend team. By sharing the mock API's endpoints and expected responses, there was less room for misunderstandings or mismatches in the data format.
    
3. **Easy Updates**:
    
    As frontend requirements evolved, updates to the mock data were easy. This ensured that the mock server always reflected the most recent data needs.
    

---

**Conclusion**:

Setting up a mock REST API server proved invaluable in streamlining the development process. It allowed the frontend to be developed and tested independently, served as a clear specification for the backend, and provided a platform for easy updates. Tools like `json-server` and platforms like [Render.com](http://Render.com) make the process straightforward, even for developers with minimal backend experience. For teams looking to optimize their frontend-backend workflow, a mock server is definitely worth considering!
