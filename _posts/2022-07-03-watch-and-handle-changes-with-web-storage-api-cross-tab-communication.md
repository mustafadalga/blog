---
layout: post
title: "Watch and Handle Changes with Web Storage Api — Cross Tab Communication"
date: 2022-07-03 20:52:42 +0000
slug: watch-and-handle-changes-with-web-storage-api-cross-tab-communication
tags: [web-development, webdev, web-api, web-1, web-storage-api]
lang: en
---



Heybooster is a client-based project. The data in the Heybooster changes depending on the client.

The user can see different client information on different browser tabs under normal circumstances. However, we don’t want to show different client information in different browser tabs. Therefore, our goal is to synchronize the active client across all browser tabs.

To solve the problem , we listened to the active client with the web storage api and handled the client change status.

I created a demo project to explain how to solve our problem. You can check it at the end of this blog post.

In the code below, we have defined the first client as activeClient by default when there is no activeClientID. We created an action to change activeClientID. This action will change the status of the activeClientID store and localstorage.

```typescript
import { defineStore } from 'pinia'
import { clients } from "@/data";

const store = defineStore({
    id: 'global',
    state: () => ({
        activeClientID: JSON.parse(localStorage.getItem('activeClientID')) || clients[0].id,
    }),
    getters: {
        getActiveClientID: (state) => state.activeClientID,
    },
    actions: {
        changeActiveClientID(activeClientID) {
            this.activeClientID = Number(activeClientID);
            localStorage.setItem('activeClientID',JSON.stringify(this.activeClientID))
        }
    }
})


export default store;
```

By using the function below, when the activeClientID is changed, we automatically refresh the page in all other tabs so that they receive the same activeClientID.

```typescript
export function activeClientIDListener() {
    window.addEventListener("storage", ({ key }) => {
        if (key == 'activeClientID') {
            window.location.reload();
        }
    });
}
```

To use the above codes, we import them into the App.vue file.

* We obtained the client and activeClientID information from the store and displayed them in the dropdown.
    
* We called the changeActiveClientID action when the client is changed.
    
* With the changeActiveClientID action, activeClientID information will be changed in the store and localStorage, and the page will be refreshed in other tabs.
    
* We called activeClientIDListener to listen to client change status in other tabs.
    

```typescript
<script setup>
import { ref } from "vue";
import CustomSelect from "@/components/CustomSelect.vue";
import store from "@/stores";
import { activeClientIDListener } from "./utils";
const useStore = store();
const activeClientID = ref(useStore.activeClientID);
const clients = useStore.getClients;
activeClientIDListener();
</script>

<template>
    <CustomSelect
       :options="clients"
       :initialValue="activeClientID"
       @change="useStore.changeActiveClientID"/>
</template>
```

Demo project: [https://cross-tab-communication.netlify.app/](https://cross-tab-communication.netlify.app/)

Demo project code: [https://github.com/mustafadalga/cross-tab-communication](https://github.com/mustafadalga/cross-tab-communication)

*Originally published at* [*heybooster.*](https://insight.heybooster.ai/cross-tab-communication/)
