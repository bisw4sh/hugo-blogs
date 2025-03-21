---
title: "Refreshing route pages in react-router"
date: 2020-09-15T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["react", "react-router", "client-side-routing"]
categories:
  - React
  - Routing
author: ["biswash"]
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "When we refresh the page in projects with react-router or any other client side routing,it returns 404. That is because the route doesnot exist on the backend; only exists on the front end. So, we need to configure to direct the request to the same index.html file"
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "/blogs/google oauth.png" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/hugo-papermod"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

## React Router - Content disappear on refresh (client route access)

[Issue](https://stackoverflow.com/questions/27928372/react-router-urls-dont-work-when-refreshing-or-writing-manually/54765884) - React Router is client side routing and when a url is requested to server, it has no such route.

### Discussion

- [react-router-cannot-get-url-refresh](https://ui.dev/react-router-cannot-get-url-refresh)
- [React-router URLs don't work when refreshing or writing manually](https://stackoverflow.com/questions/27928372/react-router-urls-dont-work-when-refreshing-or-writing-manually/54765884)

### Fixes

1. HashRouter - has # before all routes, clever hack, ugly and issues with google console redirect url.
2. Express server to distribute static assets.
3. [...alot more here](https://ui.dev/react-router-cannot-get-url-refresh) for webpack on dev servers, apache server.

#### Usabe Fixes

1. **\_redirects** file on public folder with following content.

```
/*  /index.html  200
```

**_worked on netlify_**

2. Vendor specific solutions

1. Netlify  
   **_netlify.toml_** file on root directory with the following content:

   ```toml
   [[redirects]]
   from = "/*"
   to = "/index.html"
   status = 200
   force = false
   ```

   [reference](https://stackoverflow.com/questions/55990467/catch-all-redirect-for-create-react-app-in-netlify)

1. Vercel  
   **_vercel.json_** file on root directory with the following content:

   ```json
   {
     "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
   }
   ```

   [reference](https://stackoverflow.com/questions/73749585/vercel-how-to-return-index-html-on-every-route-change)

1. Firebase  
   **_firebase.json_** file on root directory with the following content:

   ```json
   {
     "hosting": {
       "rewrites": [
         {
           "source": "*",
           "destination": "/index.html"
         }
       ]
     }
   }
   ```

   [reference](https://stackoverflow.com/questions/27928372/react-router-urls-dont-work-when-refreshing-or-writing-manually/54765884)
