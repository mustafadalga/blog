---
layout: post
title: "Adventures of a Front-End Developer — Migration of Front-End Project"
date: 2022-10-31 08:17:40 +0000
slug: adventures-of-a-front-end-developer-migration-of-front-end-project
tags: [software-development, frontend, software-architecture, migration, frontend-development]
lang: en
---


![Cover Image]({{ '/assets/images/6857db1a-6511-4e81-b19c-70c69d11e852.png' | relative_url }})

In this blog post I will explain how I upgraded our [Heybooster](https://www.heybooster.ai/) front-end project which is built on the vue.js framework and what motivated me to do so.

In our previous project we used the vue version of 2.5.11 and the webpack version of 3.6.0 and we had some problems with it.

* Developments weren’t easy,
    
* Some of npm packages, which we wanted to use, weren’t supported,
    
* Vue v2 is going to reach End of Life by the end of 2023,
    
* Disadvantages of mixins, [https://vuejs.org/guide/reusability/composables.html#vs-mixins](https://vuejs.org/guide/reusability/composables.html#vs-mixins)
    
* Inability to use the new features of Vue.
    

Besides the above issues, I had the following goals:

* To improve development environment
    
* To improve web performance
    
* Desire to use Typescript
    
* Desire to write tests with Vitest and cypress
    

I attempted to upgrade webpack and Vue almost ten times after I joined Heybooster, but many problems prevented me from upgrading it. A year and a half later I decided to upgrade the project using a different approach: By creating a new vue project and moving the files from the old to the new.

Now I\`m going to explain how I did the migration process.

First, I created a new vue.js project with TypeScript, vite and Cypress options.I deleted all unnecessary settings and files after installing the project. I rewrote the index.html file of the old project into the index.html file of the new project.

In our new project, I rewrote webpack.config.js configurations in vite.config.js.

**vite.config.ts**

```typescript
import { fileURLToPath, URL } from "node:url";

import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";

interface IAssetFileNames {
  [key: string]: any;
}

interface IDefineConfig {
  command: string,
  mode: string
}

// https://vitejs.dev/config/
export default defineConfig(({ command, mode }: IDefineConfig) => {

  const configs = {
    plugins: [vue()],
    resolve: {
      alias: {
        "@": fileURLToPath(new URL("./src", import.meta.url)),
      },
    },
    server: {
      strictPort: true,
      port: 8080,
      host: "::", //'0.0.0.0',
    },
    build: {
      rollupOptions: {
        output: {
          assetFileNames: (assetInfo: IAssetFileNames) => {
            const extType: string = assetInfo.name.split(".")[1];
            const exceptionWords = [ "google", "facebook", "shopify", "amazon", "adword", "adwords", "pinterest", "slack" ];

            if (exceptionWords.some(word => assetInfo.name.includes(word))) {
              return `assets/${extType}/[hash][extname]`;
            }

            return `assets/${extType}/[name][extname]`;
          },
          chunkFileNames: "assets/js/[name]-[hash].js"

        }
      }
    },
    esbuild: {}
  }

  if (mode == "production") {
    configs.esbuild = {
      drop: [ "console", "debugger" ]
    };
  }

  return configs;
});
```

**webpack.config.js**

```typescript
var path = require('path');
var webpack = require('webpack');
const dotenv = require('dotenv');

function envVariables() {
  const env = dotenv.config().parsed;

  return Object.keys(env).reduce((newObject, key) => {

    newObject[key] = JSON.stringify(env[key]);

    return newObject;

  }, {});

}

module.exports = {
  entry: [ "core-js/modules/es.promise", "core-js/modules/es.array.iterator", './src/main.js' ],
  output: {
    path: path.resolve(__dirname, './dist'),
    publicPath: '/dist/',
    filename: 'build.js',
    chunkFilename: './js/[name].bundle.js',
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [ 'style-loader', 'css-loader' ]
      },
      {
        test: /\.scss$/,
        use: [ 'style-loader', 'css-loader', 'sass-loader' ]
      },
      {
        test: /\.sass$/,
        use: [ 'style-loader', 'css-loader', 'sass-loader?indentedSyntax' ]
      },
      {
        test: /\.vue$/,
        loader: 'vue-loader',
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/
      },
      {
        test: /\.(png|jpg|gif|svg)$/,
        loader: 'file-loader',
        options: {
          name(resourcePath,resourceQuery) {
            const exceptionWords = [ "google", "facebook", "shopify", "amazon", "adword", "adwords", "pinterest", "slack" ];
            const filename = path.parse(resourcePath).name.toLowerCase();

            if (exceptionWords.some(word => filename.includes(word))) {

              return 'images/[contenthash].[ext]';

            }

            return 'images/[name].[ext]';
          },
        }
      },
      {
        test: /\.(woff(2)?|ttf|eot)$/,
        loader: 'file-loader',
        options: {
          name: 'fonts/[name].[ext]?[hash]'
        }
      }
    ]
  },
  externals: {
    moment: 'moment'
  },
  resolve: {
    alias: {
      vue$: 'vue/dist/vue.esm.js',
      '@': path.resolve('src')
    },
    extensions: [ '*', '.js', '.vue', '.json' ]
  },
  devServer: {
    historyApiFallback: true,
    noInfo: true,
    overlay: true
  },
  performance: {
    hints: 'warning',
    maxEntrypointSize: 2500000,
    maxAssetSize: 2500000,
  },
  devtool: '#source-map',
  plugins: [
    new webpack.optimize.CommonsChunkPlugin({
      children: true,
      // (use all children of the chunk)

      async: 'common',
      // (create an async commons chunk)

      minChunks: 3,
      // (3 children must share the module before it's separated)
    }),
    new webpack.DefinePlugin({
      'process.env': envVariables()
    }),
  ]
};

if (JSON.parse(envVariables().MODE) == "PRODUCTION") {

  // http://vue-loader.vuejs.org/en/workflow/production.html
  module.exports.plugins = (module.exports.plugins || []).concat([
    //https://github.com/webpack/webpack/issues/3216#issuecomment-257347714

    new webpack.optimize.UglifyJsPlugin({
      sourceMap: true,
      compress: {
        warnings: false,
        drop_console: true
      }
    }),
    new webpack.LoaderOptionsPlugin({
      minimize: true,
    }),
  ]);
}
```

I have installed the new versions of npm packages, which we are using in the project . I tested the newer versions to see if they worked. While testing these packages, I took note of minor changes/breakings for refactoring our codes used within the project.

I moved the components, views, layouts, assets, plugins, store etc files from the old project to the new one.

#### Importing extension-less Vue components

In the previous project, we used the webpack , so we didn’t have to use the .vue extension when importing vue components. However, by starting to use vite.js , we have to use the .vue extension when importing vue components. As a result, I have added the .vue extension to all the components that were imported without the .vue extension

![]({{ '/assets/images/3b6f4376-8729-4e54-bcba-98d21259e436.png' | relative_url }})

Importing extension-less Vue components

#### Env Variables

In the previous project, we could access env variables with **process.env\[VARIABLE\_NAME\]**. But in the new project, by starting to use vite.js, we access the env variables with **import.meta.env\[VITE\_VARIABLE\_NAME\]**.

So, I have added the **VITE\_** prefix to the beginning of all variables in the .env file and have changed all the codes in the project starting with the process.env prefix to **import.meta.env.VITE\_**

![]({{ '/assets/images/b54b92dd-a884-4cbf-a73a-d4a6600eb50d.png' | relative_url }})

#### Importing SCSS Files

I was adding ~ alias to the path of SCSS files when I imported them. As new project does not require this, I have deleted all the aliases beginning with ~ in the project.

![]({{ '/assets/images/4fa0d84f-2bd4-4bbe-be8a-83e1c7e1a4e0.png' | relative_url }})

#### Fixing Deprecated Codes

* **Deprecated Hooks**
    

In Vue 3, the **destroyed** hook has been changed to **unmounted**, and the **beforeDestroy** hook has been changed to **beforeUnmount**. I changed the deprecated hooks used in the project as follows.

![]({{ '/assets/images/daf61c62-2402-429b-8299-8052fd291f7c.png' | relative_url }})

beforeUnmount

![]({{ '/assets/images/4619a9b3-b7de-4bab-b1be-e705857ba83c.png' | relative_url }})

unmounted

* **Deprecated Slash(/) as Division in SASS**
    

With the upgrade of the SASS version, the [slash as a division](https://sass-lang.com/documentation/breaking-changes/slash-div) has been deprecated. As a solution, I changed the codes where slashes (/) are used for division.

![]({{ '/assets/images/68d65c6c-40df-4b55-8934-0529999bc553.png' | relative_url }})

* **Deprecated Deep Selectors**
    

In Vue 2 , we use the **/deep/** selector to apply scoped styles to child components. However, with Vue 3, the **/deep/** selector has been deprecated and **:deep()** has replaced it.

![]({{ '/assets/images/1bd3becd-7320-4901-b0a0-127ac9a8e5b8.png' | relative_url }})

* **Passing params that are not defined as part of the path in vue-router**
    

We were sending some data as params on the router, but since params that are not defined in the path are not supported in the updated [vue-router version](https://github.com/vuejs/router/blob/main/packages/router/CHANGELOG.md#414-2022-08-22), we refactored these codes and kept the data in vuex.

![]({{ '/assets/images/f3056e49-6efd-443e-be85-2181aafb41d8.png' | relative_url }})

* **Dynamic Assets URL**
    

In Vue 2, we used the Require module loader to load dynamic assets. With the introduction of vite.js in vue 3, this method has been deprecated.

I created a helper function to load dynamic assets with vite.js. I have replaced the require module loader with this helper function.

```typescript
function getImageUrl(type, file) {

  let folder = "";
  if (type == "image") {
    folder = "images";
  } else if (type == "icon") {
    folder = "icons";
  }

  return new URL(`../../assets/${folder}/${file}`, import.meta.url).href;
}
```

![]({{ '/assets/images/ef4ec4a8-91f3-436c-9512-6e5b6b7f5231.png' | relative_url }})

* **Removed APIs — $children**
    

Since the $children API was deprecated in vue 3, I refactored the code.

![]({{ '/assets/images/318e05ba-b11f-4692-b85c-3ed59a6d289d.png' | relative_url }})

* **Removed APIs — $set**
    

We were using the Vue $set api for dynamically created object keys to be reactive. [But with Vue 3, They are no longer required with proxy-based change detection](https://v3-migration.vuejs.org/breaking-changes/#removed-apis). So , I refactored code as follow.

![]({{ '/assets/images/1047b2cd-a33a-4982-ac4b-57b4d0d71054.png' | relative_url }})

#### Lazy loading components

Since lazy loading components have been changed in Vue 3, I used the [defineAsyncComponent](https://vuejs.org/guide/components/async.html) function to load component as a lazy loading component.

![]({{ '/assets/images/f7485a94-160a-45d6-bce9-1909fa420b94.png' | relative_url }})

#### Fixing the codes of npm packages that are not working

In the new project, some codes were deprecated as a result of the upgrade of npm packages. I resolved the issue by replacing all code that used deprecated third party packages.

![]({{ '/assets/images/2fbc8839-3fc2-4a39-b899-0cfb0f555ab3.png' | relative_url }})

![]({{ '/assets/images/4ca70c99-5a7e-4ce2-8249-264dd905e678.png' | relative_url }})

![]({{ '/assets/images/b66af385-5262-4d96-9877-1a95eb2ea217.png' | relative_url }})

![]({{ '/assets/images/1f3af73c-31eb-4164-95bb-2ecd94790fc2.png' | relative_url }})

#### Rewrite of plugins

Our plugin we created with Vue 2 didn’t work with Vue 3, so I have re-written it with Vue 3.

**file-constants-vue-2-js**

```typescript
import axios from "axios";
import store from "@/store";

const Constants = {
  install(Vue) {
    Vue.prototype.$constants = (key) => {

      const constantList = {
        $http: axios.create({
          baseURL: store.getters.getApiBaseUrl,
        }),
        axios: axios.create({
          baseURL: store.getters.getServerlessApiBaseUrl,
        }),
      }

      return constantList[key];
    }
  }
}

export default Constants;
```

**constants-vue-3.ts**

```typescript
import axios from "axios";
import { store } from "@/store";


export default {
  install: (app) => {

    // constants list
    const constants = {
      $http: axios.create({
        baseURL: store.getters.getApiBaseUrl
      }),
      axios: axios.create({
        baseURL: store.getters.getServerlessApiBaseUrl
      })
    };


    app.config.globalProperties.$constants = (key) => {
      return constants[key];
    };

    app.provide("$constants", constants);
  }
};
```

#### Configure of Store — Vuex

We use Vuex as the state management library. There have been some minor changes in the new version of Vuex.

**To apply the changes:**

* I changed our store creation code with createStore.
    

![]({{ '/assets/images/d621a8a4-c0b3-48d8-97bd-1fbe79b57e14.png' | relative_url }})

* I imported and installed the store.
    

![]({{ '/assets/images/940ce13e-7dd5-4096-9ba5-c7d878b2fef2.png' | relative_url }})

* In Vue 2, we needed to access the Vue instance in order to use the [plugins](https://vuejs.org/guide/reusability/plugins.html#plugins) in Vuex. In Vue 2, we were using the constants plugin in Vuex as follows.
    

![]({{ '/assets/images/b12f783e-6aae-465c-ac36-1dd7391d99f8.png' | relative_url }})

* This method has been changed with Vue 3. Therefore, I configured the plugin instance in the store and I changed the plugin codes we used in the store.
    

![]({{ '/assets/images/6ffecc68-71b2-450f-9bc7-edb62c0fe459.png' | relative_url }})

main.ts

![]({{ '/assets/images/b4e52c77-c833-433d-b202-663f1e8919d2.png' | relative_url }})

**Project size(dist) — New project vs Previous Project**

As a result of the migration process, the project size has been reduced by almost 54.88%.

![]({{ '/assets/images/40acf10b-e1ef-44d9-8023-c18c44cc3150.png' | relative_url }})

![]({{ '/assets/images/7200a0f6-23d4-46db-9ee8-bcd4ec1da326.png' | relative_url }})
