---
layout: post
title: "Creating a Reusable Axios Wrapper:  A Clean and Efficient Approach"
date: 2024-05-18 08:25:15 +0000
slug: creating-a-reusable-axios-wrapper-a-clean-and-efficient-approach
tags: [vuejs, frontend-development, axios-interceptor, axios, axios-https-request]
lang: en
---


In one of our projects, we use the axios library to make API requests to our backend REST endpoints. However, we need to add authorization headers to these API requests. After the user logs in, we store the JWT token received from the backend and send this token in the authorization header for each API request to ensure proper authentication.

Initially, we were inefficiently adding the JWT token to the axios headers for every API request. Here's a simple example of how it was done:

```typescript
export function useGetPosts() {
  const store = useStore();
  const apiURL = import.meta.env.API_BASE_URL;
  const jwt = computed(() => store.getters.getJWT);

  async function getPosts() {
    const response = await axios.get(`${apiURL}/posts`, {
      headers: {
        Authorization: jwt.value,
      },
    });
    return response;
  }

  return {
    getPosts,
  };
}
```

Additionally, since our project operates on a client basis, it is essential to synchronize the client's information whenever it changes. Therefore, all pending API requests must be canceled when the client changes, as these requests are intended to load information for the previous client and are no longer needed. To cancel unnecessary network requests, we needed to add cancel tokens to each API request.

When your MacBook goes into sleep mode, it cuts off the internet connection to save energy, causing network errors for background web applications. In our project, we have background processes that send periodic REST API requests to update data, but these API requests result in network errors when the MacBook enters sleep mode. To handle this issue, we needed to use axios response error interceptors to ignore the errors.

Handling the above three cases for each API request would result in unnecessarily messy code. Therefore, I created an axios wrapper composable to provide a clean and reusable solution.

```typescript
import { useStore } from "vuex";
import { computed } from "vue";
import axios from "axios";
import type { InternalAxiosRequestConfig, CancelTokenSource, AxiosInstance, AxiosError } from "axios";

interface IExtendedAxiosError extends AxiosError {
    toJSON: () => Record<string, unknown>;
}

export default function useAxios() {
    const store = useStore();
    const jwt = computed<string>(() => store.getters.getJWT);
    const axiosCancelToken = computed<CancelTokenSource>(() => store.getters.getAxiosCancelToken);
    const axiosInstance: AxiosInstance = axios.create({
        baseURL: store.getters.getApiBaseUrl
    });

    setup();

    function setup() {
        axiosInstance.interceptors.request.use(onRequest, (error) => Promise.reject(error));
        axiosInstance.interceptors.response.use(response => response, onResponseError);
    }

    function onRequest(config: InternalAxiosRequestConfig): InternalAxiosRequestConfig {
        if (config.headers) {
            config.headers.Authorization = jwt.value;
        }
        config.cancelToken = axiosCancelToken.value.token;
        return config;
    }

    function onResponseError(error: IExtendedAxiosError): Promise<AxiosError> | undefined {
        const isNetworkError = Object.hasOwn(error, "toJSON") && error.toJSON().message == "Network Error";
        return isNetworkError ? undefined : Promise.reject(error);
    }

    return {
        axios: axiosInstance
    }
}
```

Here's the store where I keep and update the JWT, cancel token, and API URL:

```typescript
import { createStore } from 'vuex'
import axios, { type CancelTokenSource } from "axios"


export interface State {
    axiosCancelToken: CancelTokenSource,
    jwt: string | null,
    apiURL: string,
    activeClient: string | null
}

const store = createStore<State>({
    state: {
        axiosCancelToken: axios.CancelToken.source(),
        jwt: null,
        apiURL: import.meta.env.API_BASE_URL,
        activeClient: null
    },
    getters: {
        getJWT(state) {
            return state.jwt
        },
        getAxiosCancelToken(state) {
            return state.axiosCancelToken
        },
        getApiURL(state) {
            return state.apiURL
        },
        getActiveclient(state) {
            return state.activeClient
        }
    },
    mutations: {
        setJWT(state, jwt) {
            state.jwt = jwt
        },
        setAxiosCancelToken(state) {
            state.axiosCancelToken = axios.CancelToken.source()
        },
        cancelAxiosCancelToken(state) {
            state.axiosCancelToken.cancel();
        },
        setActiveClient(state, activeClient) {
            state.activeClient = activeClient
        }
    }
})

export default store;
```

Here’s a simple usage example:

```typescript
<script setup lang="ts">
import useAxios from "@/composables/useAxios";
import { useStore } from "vuex";

const { axios } = useAxios();
getUsers();

async function getUsers() {
  const response = await axios.get("https://reqres.in/api/users?delay=3");
  //...
}
</script>
```

You can check out the full example in this repo: [GitHub Repo Link](https://github.com/mustafadalga/vue-playground/tree/26.use-axios) and the demo here: [Demo Link](https://guileless-dieffenbachia-3e85e5.netlify.app/).
