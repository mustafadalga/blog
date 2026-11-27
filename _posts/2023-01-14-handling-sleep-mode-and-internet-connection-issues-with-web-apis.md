---
layout: post
title: "Handling sleep mode and internet connection issues with web APIs"
date: 2023-01-14 22:30:10 +0000
slug: handling-sleep-mode-and-internet-connection-issues-with-web-apis
tags: [error-handling, web-api, error-tracking, errors-in-javascript, internet-connection]
lang: en
---



![Cover Image]({{ '/assets/images/6fb8a27b-3b64-4fc6-9f64-f2700b8a73de.png' | relative_url }})

We have a rest api that we use to update data with certain time intervals. We update our data by regularly calling a function that fetches the data using the setInterval method.

However, we had some problems as a result of internet disconnection / network related problems and setInterval continuing to work even though the computer was in sleep mode.

After each api request, we were doing some operations by comparing the old data with the new data. However, due to the above situations, our rest-api could not successfully fetch updated data. Therefore, it was causing some problems in the project as it could not bring up to date data.

To solve the problem, I handled the situations where there is no internet connection and the computer is in sleep mode with Web APIs:

*   I used Web API features [navigator.onLine](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/onLine) and [offline](https://developer.mozilla.org/en-US/docs/Web/API/Window/offline_event)/[online](https://developer.mozilla.org/en-US/docs/Web/API/Window/online_event) event listeners to check if the computer has an internet connection.
*   I used the [Page Visibility API](https://developer.mozilla.org/en-US/docs/Web/API/Page_Visibility_API) to check if the computer is in sleep mode or the browser is switching to another tab.

I used a [fake rest api](https://jsonplaceholder.typicode.com/) to explain how I solved this problem.

The function we created to send the API request is as follows:

I created two variables to update data at the certain time and keep the setInterval id.

I have created a function called handleInterval to delete/create setInterval and check the status of internet connection

I called the handleInterval function to start setInterval when the user enters the page

I have created online/offline event listeners to listen if internet connection is available or not. When internet connection was lost, I immediately stopped the setInterval to avoid any problems and as soon as internet connection was re-established, I restarted the setInterval to resume the updates.

I created an event list using the Page Visibility API to check if the computer is in sleep mode or the browser is switching to another tab.

I explained how it solved a problem we encountered in the Heybooster front end project.

You can also review the sample javascript file with all the codes below:
