---
layout: post
title: "How to exchange data in different vuex modules"
date: 2021-11-22 12:07:36 +0000
slug: how-to-exchange-data-in-different-vuex-modules
tags: [vuejs, vuex, state-management, vue3, vuejs-state-management]
lang: en
---



![Cover Image]({{ '/assets/images/2b8e3df5-9654-4183-b4af-b96ee20e9120.png' | relative_url }})

Our [previous](https://medium.com/heybooster/vuex-on-large-projects-how-to-structure-it-78c45c2b3f16) article discussed the importance of using vuex modules in large projects.

I will explain and show how we can exchange data between vuex modules in this blog post.

In the course of using vuex modules, we may encounter situations such as the following:

* Using a different module state in an action.
    
* Using a different module mutation to mutate data for a different module within an action.
    
* Using a different module state in a mutation.
    
* Using a different module mutation in a mutation.
    
* Using a different module state in a getters.
    
* Using a different module getter in a getters.
    

I will provide examples for each of the scenarios above.

### Using different module state in an action

We use the **addMovieBookmark** action in the movies module when we want to add a movie to bookmark. We need the movieID and user email to add a movie to the bookmark. However, the user’s email address is storing in the **auth** module.

For this reason, we need to access the auth module’s states. I will use rootState to access the email address in the auth module.

We can access states in a different module with **rootState**.

```typescript
    async addMovieBookmark({ rootState, commit }, movieID) {
            const data = {
            email: rootState.auth.user.email,
            movieID: movieID
        }

        axios.post('add-movie-bookmark',data);
    }
```

### Using a different module mutation to mutate data for a different module within an action

We will show a warning message when the user encounters an error while adding a movie bookmark via the **addMovieBookmark** action in the movie module. We use the notifications module for information about errors and warnings.

Therefore, we will use the **setNotification** mutation of the notification module to create a new notification.

In the commit we added ***{ root: true }*** as a third argument to access the **setNotification** mutation in the notification module.

```typescript
    async addMovieBookmark({ rootState, commit }, movieID) {
        try {
            const data = {
                email: rootState.auth.user.email,
                movieID: movieID
            }

           const response=await axios.post('add-movie-bookmark',data);

        } catch (error) {

            const notification = "An error occurred during adding movie to bookmark.Please try again.";
            commit("notifications/setNotification", notification, { root: true })
        }
    },
```

### Using different module state in a mutation

When we use the **addMovieBookmark** action to add a movie to bookmark, we will also need to change the bookmarks status in vuex.

We use the **changeMovieBookmarkStatus** mutation to change the bookmark status of the movie. We need to make sure that the email of the movie added to the bookmark is the same as the user email in order to change the bookmark status. Therefore, we must send the email address of the user to the changeMovieBookmarkStatus mutation.

We sent the user’s email to the **changeMovieBookmarkStatus** mutation from auth module. We also sent the bookmark’s email address.

```typescript
    async addMovieBookmark({ rootState, commit }, movieID) {
            const data = {
                email: rootState.auth.user.email,
                movieID: movieID
            }

           const response=await axios.post('add-movie-bookmark',data);
           const data=response.data.data
 
            commit('changeMovieBookmarkStatus', {
                id: movieID,
                status: data.status,
                bookmarkEmail: data.email,
                userEmail: rootState.auth.user.email, 
            });

    },
```

As long as the bookmark email and the user email are equal, the **changeMovieBookmarkStatus** mutation will be performed.

```typescript
changeMovieBookmarkStatus: (state, data) => {

    if (data.bookmarkEmail != data.userEmail) return;

    const movieIndex = state.movies.findIndex(movie => movie.id == data.id);
    state.movies[movieIndex].bookmarkStatus = data.status;
}
```

### Using different module mutation in a mutation.

In the case of changing the bookmark status of a movie, if the movie’s email address and the user’s email address differ, we should not change the bookmark status and at the same time, we should show a warning message to the user.

Therefore, we need to use the **setNotification** mutation of the notification module from within the changeMovieBookmarkStatus mutation.

We will send the commit to **changeMovieBookmarkStatus** as an argument to access the setNotification mutation.

```typescript
async addMovieBookmark({ rootState, commit }, movieID) {
            const data = {
                email: rootState.auth.user.email,
                movieID: movieID
            }

           const response=await axios.post('add-movie-bookmark',data);
           const data=response.data.data
 
            commit('changeMovieBookmarkStatus', {
                id: movieID,
                status: data.status,
                bookmarkEmail: data.email,
                userEmail: rootState.auth.user.email, 
                commit:commit
            });
    },
```

We accessed the setNotification mutation using data.commit in the changeMovieBookmarkStatus mutation.

```typescript
    changeMovieBookmarkStatus: (state, data) => {
     
        if (data.bookmarkEmail == data.userEmail){

            const movieIndex = state.movies.findIndex(movie => movie.id == data.id);
            state.movies[movieIndex].bookmarkStatus = data.status;

        }else{

            const notification = "An error occurred during adding movie to bookmark.Please try again.";
            data.commit("notifications/setNotification", notification, { root: true })

        }
    }
```

### Using different module state in a getters.

We will need to add two more arguments to our getters method to use another module’s state: getters and rootState.

As a result, we added the getters and rootState arguments, and we then received the user’s email address from the auth module using the rootState.

```typescript
    getBookmarkedMovies: (state,getters,rootState) => {
        const email = rootState.auth.user.email

        return  state.movies.map(movie => ({ ...movie, userEmail: email}))
    }
```

### Using different module getter in a getters

We want to show user information and favorite movies in user’s profile. For this, we need to use the information from the movies and auth modules.

We will need to add three more arguments to our getters method to use another module’s getter: getters, rootState and rootGetters

As a result, we added the getters, rootState and rootGetters arguments, and we then received the bookmarked movies from the movies module using the rootGetters.

```typescript

    getUserProfile: (state, getters, rootState, rootGetters) => {

        const favouriteMovies = rootGetters['movies/getBookmarkedMovies'].map(movie => ({
            id: movie.id,
            poster_path: movie.poster_path
        }))

        return {
            ...state.user,
            favouriteMovies: favouriteMovies
        }
    }
```

The sample project is on my [github](https://github.com/mustafadalga/vuex-on-large-projects) account and the [demo](https://vuex-on-large-projects.netlify.app/) is on netlify.

*Originally published at heybooster*
