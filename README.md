# Introduction

This repo is a static blog site(SSG) made using [hugo](https://gohugo.io/) & with the theme [PaperMod](https://themes.gohugo.io/themes/hugo-papermod/)

1. [Papermod's doc](https://github.com/adityatelange/hugo-PaperMod/wiki) use this to setup the papermod theme

2. [Example of usecase](https://github.com/adityatelange/hugo-PaperMod/blob/exampleSite/config.yml)

3. [Demo](https://github.com/adityatelange/hugo-PaperMod/blob/exampleSite/config.yml)

4. [Guide](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-installation/)

---

## Key things to consider

1. **./hugo.yml**
```
# baseURL: "http://bisw4sh.github.io/hugo"
baseURL: "/"
title: Biswash
copyright: "© [bisw4sh](https://biswashdhungana.com.np)"
pagination:
  pagerSize: 5
theme: PaperMod

enableRobotsTXT: true
buildDrafts: true
buildFuture: true
buildExpired: true

minify:
  disableXML: true
  minifyOutput: true

params:
  env: production # to enable google analytics, opengraph, twitter-cards and schema.
  title: biswash
  description: "Static blog and portfolio site using hugo static site generator."
  keywords: [Blog, Portfolio, PaperMod]
  author: ["Me", "biswash"]
  # author: ["Me", "You"] # multiple authors
  images: ["<link or path of image for opengraph, twitter-cards>"]
  DateFormat: "January 2, 2006"
  defaultTheme: auto # dark, light
  disableThemeToggle: false

  ShowReadingTime: true
  ShowShareButtons: true
  ShowPostNavLinks: true
  ShowBreadCrumbs: true
  ShowCodeCopyButtons: true
  ShowWordCount: true
  ShowRssButtonInSectionTermList: true
  UseHugoToc: true
  disableSpecial1stPost: true
  disableScrollToTop: false
  comments: false
  hidemeta: false
  hideSummary: false
  showtoc: false
  tocopen: false

  assets:
    # disableHLJS: true # to disable highlight.js
    # disableFingerprinting: true
    favicon: "<link / abs url>"
    favicon16x16: "<link / abs url>"
    favicon32x32: "<link / abs url>"
    apple_touch_icon: "<link / abs url>"
    safari_pinned_tab: "<link / abs url>"

  label:
    text: "Home"
    icon: /apple-touch-icon.png
    iconHeight: 35

  # profile-mode
  profileMode:
    enabled: false # needs to be explicitly set
    title: biswash's blogs
    subtitle: "I'm a software engineer from Pokhara, Nepal, specializing in full-stack web development. I mainly do backend work with TypeScript and Go. I'm interested in databases, cloud, and web3, focusing on performance, scalability, and decentralization."
    imageUrl: "conv.jpg"
    imageWidth: 250
    imageHeight: 250
    imageTitle: my mugshot
    buttons:
      - name: Posts
        url: posts
      - name: Tags
        url: tags

  # home-info mode
  homeInfoParams:
    Title: "Hi there \U0001F44B"
    Content: "I'm a software engineer from Pokhara, Nepal, specializing in full-stack web development. I mainly do backend work with TypeScript and Go. I'm interested in databases, cloud, and web3, focusing on performance, scalability, and decentralization."

  socialIcons:
    - name: github
      url: "https://github.com/bisw4sh"
    - name: gitlab
      url: "https://gitlab.com/bisw4sh"
    - name: leetcode
      url: "https://leetcode.com/u/bisw4sh/"
    - name: linkedin
      url: "https://www.linkedin.com/in/biswashdhungana/"
    - name: x
      url: "https://x.com/bisw4sh"
    - name: bluesky
      url: "https://bsky.app/profile/bisw4sh.bsky.social"
    - name: facebook
      url: "https://facebook.com/bisw4sh"
    - name: instagram
      url: "https://instagram.com/bisw4sh"

  analytics:
    google:
      SiteVerificationTag: "XYZabc"
    bing:
      SiteVerificationTag: "XYZabc"
    yandex:
      SiteVerificationTag: "XYZabc"

  cover:
    hidden: false # hide everywhere but not in structured data
    hiddenInList: true # hide on list pages and home
    hiddenInSingle: false # hide on single page

  editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link

  # for search
  # https://fusejs.io/api/options.html
  fuseOpts:
    isCaseSensitive: false
    shouldSort: true
    location: 0
    distance: 1000
    threshold: 0.4
    minMatchCharLength: 0
    limit: 10 # refer: https://www.fusejs.io/api/methods.html#search
    keys: ["title", "permalink", "summary", "content"]
menu:
  main:
    - identifier: categories
      name: categories
      url: /categories/
      weight: 10
    - identifier: tags
      name: tags
      url: /tags/
      weight: 20
    - identifier: example
      name: portfolio
      url: https://biswashdhungana.com.np
      weight: 30
# Read: https://github.com/adityatelange/hugo-PaperMod/wiki/FAQs#using-hugos-syntax-highlighter-chroma
pygmentsUseClasses: true
markup:
  highlight:
    noClasses: false
    anchorLineNos: true
    codeFences: true
    guessSyntax: true
    lineNos: true
    style: monokai

outputs:
  home:
    - HTML
    - RSS
    - JSON # necessary for search
```

2. **./archetypes/default.md**
```
---
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
date: '{{ .Date }}'
# weight: 1
# aliases: ["/first"]
tags: ["tag1", "tag2", "tag3"]
categories:
  - DefaultCategory
author: ["biswash"]
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Description of the post"
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
    image: "/" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/bisw4sh/hugo-blogs"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---
```

3. **./content/posts**
- This is necessary because, if this isn't the case, it won't be displayed in the main page. Only one folder's contents are displayed.

4. **./<static_files>**
- This is the cause for images and cover images

```
//hugo.yml
cover:
    image: "/blogs/react-router.jpg" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
```    

```
//<./posts/<post_name.md>>
![google oauth flow](/blogs/google-oauth.png)
```

---

### Setting up the hugo projects

#### 1. scaffold the hugo projects with :
` hugo new site <site_name> --format -yml`

--format : {yml (recommended), toml, json}

#### 2. use themes to get started or you could create your own.
` git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke `

#### 3. Signal what theme is being used
`echo "theme = 'ananke'" >> hugo.toml `

#### 4. start the server
`hugo server` or `rm rf public server` to remove the build and start again

#### 5. add contents
`hugo new content content/posts/my-first-post.md`
 - totally depends on the theme used or configuration.

---

 Get the themes from:
 1. [Jamstack Themes](https://jamstackthemes.dev/)
 2. [Hugo Themes](https://themes.gohugo.io/)