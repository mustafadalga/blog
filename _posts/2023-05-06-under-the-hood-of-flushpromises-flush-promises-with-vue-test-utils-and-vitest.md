---
layout: post
title: "Under the Hood of flushPromises: Flush Promises with Vue Test Utils and Vitest"
date: 2023-05-06 18:53:30 +0000
slug: under-the-hood-of-flushpromises-flush-promises-with-vue-test-utils-and-vitest
tags: [unit-testing, vuejs, reactjs, jest, vitest]
lang: en
---


In this blog post, we’ll help you understand how `flushPromises` works under the hood in the context of `vue-test-utils`. We'll explain how `flushPromises` works as a microtask queue and provides a step-by-step explanation of its workings. We'll also demonstrate a real-world example using `vue-test-utils`, Vitest, Vue.js, and TypeScript.

### Understanding Flush Promises

`flushPromises` is a utility function that helps you wait for all pending promises to resolve in your Vue.js unit tests. It's particularly useful when testing asynchronous operations such as API calls, where you need to ensure that all pending promises have been resolved before making assertions about the component's state or the DOM.

The `flushPromises` function works by emptying the microtask queue, which is a queue of tasks that are scheduled to run in the order they were added. When a promise is created or a block is executed, the associated task is added to the microtask queue. The microtask queue is processed after the current task is completed and before the browser updates the rendering. By emptying the microtask queue, `flushPromises` ensures that all pending promises are executed and resolved before your test continues.

### Real-world Example

Before creating the `DataFetcher.vue` component, we need to understand the requirements and the behavior we expect from it.

The `DataFetcher.vue` component should:

1. When the button is clicked, it should make an asynchronous API call using axios to fetch the data.
    
2. Once the data is fetched, it should update the DOM to display the fetched data.
    

With these requirements in mind, we can create the `DataFetcher.vue` component as shown below:

```typescript
<script setup lang="ts">
import axios from 'axios';
import { ref } from "vue";
import type { User } from "@/types";

const user = ref<User>();

async function fetchData(): Promise<void> {
    const response = await axios.get('https://jsonplaceholder.typicode.com/users/1');
    user.value = response.data;
}
</script>


<template>
    <div>
        <button @click="fetchData">Fetch Data</button>
        <div v-if="user" class="user">
            {{ user.name }}
        </div>
    </div>
</template>
```

Now, let’s write a unit test for `DataFetcher.vue` using Vitest and vue-test-utils. The test file `DataFetcher.test.ts` is as follows:

```typescript
import { mount, flushPromises, type VueWrapper } from "@vue/test-utils";
import axios from "axios";
import DataFetcher from "@/components/DataFetcher.vue";
import { vi, it, expect, type SpyInstance } from "vitest";
import type { User } from "@/types";

it("fetches data and updates the DOM", async (): Promise<void> => {
    const user: User = {
        id: 1,
        name: "Leanne Graham",
        username: "Bret",
        email: "Sincere@april.biz",
        phone: "1-770-736-8031 x56442",
        website: "hildegard.org",
    };

    // Spy on the axios.get method
    const mockedAxiosGet:SpyInstance = vi.spyOn(axios, "get");
    mockedAxiosGet.mockReturnValueOnce(Promise.resolve({ data: user }));

    // Mount the component
    const wrapper: VueWrapper = mount(DataFetcher);

    // Trigger the button click
    await wrapper.find("button").trigger("click");

    // Wait for all pending promises to resolve
    await flushPromises();

    // Now you can make assertions about the component's state or DOM after the promises have resolved
    expect(wrapper.find(".user").html()).toContain(user.name);

    // Clean up the axios.get mock
    mockedAxiosGet.mockRestore();
});
```

In the test above, we’re using `flushPromises` to ensure that the promises are resolved before we make assertions about the component's state or DOM. By doing so, we can accurately test how the component behaves when the `fetchData` method is called and the data is fetched asynchronously.

Here’s a summary of the process:

1. We create a `User` object with dummy data for the purpose of this test.
    
2. We spy on the `axios.get` method using Vitest's `vi.spyOn` method and mock its return value to resolve with our dummy data.
    
3. We mount the `DataFetcher` component using `vue-test-utils`' `mount` function.
    
4. We trigger the button click event using the `trigger` method, which in turn calls the `fetchData` method of the component.
    
5. We call `flushPromises` to wait for all pending promises to be resolved.
    
6. After the promises have been resolved, we make assertions about the component’s state and DOM to ensure that the expected behavior is taking place.
    
7. Finally, we cleaned up the `axios.get` mocked by calling the `mockRestore` method on the mocked instance.
    

---

In this blog post, we’ve discussed how `flushPromises` works under the hood and it helps us wait for all pending promises to resolve in our Vue.js unit tests. We've also demonstrated a real-world example using `vue-test-utils`, Vitest, Vue.js, and TypeScript. By understanding the concept of the microtask queue and utilizing `flushPromises`, you can write more accurate and reliable tests for your Vue.js applications.

[https://github.com/mustafadalga/vue-playground/tree/19.flush-promises-under-the-hood](https://github.com/mustafadalga/vue-playground/tree/19.flush-promises-under-the-hood)
