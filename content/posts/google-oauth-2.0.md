---
title: "Google Oauth 2.0"
date: 2020-09-15T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["google", "oauth", "pkce"]
categories:
  - Auth
  - Google
author: ["biswash"]
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "There is a flow to how we can authenticate a user from existing providers; there is slight variation in all platforms but following is of Google"
canonicalURL: "https://canonical.url/to/page"
disableHLJS: false # to disable highlightjs
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
  # image: "/blogs/google-oauth.png" # image path/url
  alt: "<alt text>" # alt text
  caption: "<text>" # display caption under cover
  relative: false # when using page bundles set this to true
  hidden: false # only hide on current single page
editPost:
  URL: "https://github.com/bisw4sh/hugo-blogs/blob/main/content/google-oauth-2.0.md"
  Text: "Suggest Changes" # edit text
  appendFilePath: true # to append file path to Edit link
---

![google oauth flow](/blogs/google-oauth.png)

## OAuth Flow

### 1. Get Authorization Code

Redirect the user to the Google authorization endpoint to obtain an authorization code:

```javascript
const returnUri = window.location.origin + "/signin";
const GOOGLE_CLIENTID = import.meta.env.VITE_CLIENT_ID;

const searchParams = new URLSearchParams({
  redirect_uri: returnUri,
  response_type: "code",
  client_id: GOOGLE_CLIENTID,
  access_type: "offline",
  prompt: "consent",
  scope: "openid email profile",
});

window.location.href = `https://accounts.google.com/o/oauth2/v2/auth?${searchParams.toString()}`;
```

### 2. Exchange Authorization Code for Tokens

Exchange the authorization code for access & refresh tokens:

```javascript
const exchangeCodeForTokens = async (code) => {
  const tokenEndpoint = "https://oauth2.googleapis.com/token";
  const GOOGLE_CLIENTID = import.meta.env.VITE_CLIENT_ID;
  const GOOGLE_CLIENT_SECRET = import.meta.env.VITE_CLIENT_SECRET;
  const returnUri = window.location.origin + "/signin";

  const params = new URLSearchParams({
    code: code,
    client_id: GOOGLE_CLIENTID,
    client_secret: GOOGLE_CLIENT_SECRET,
    redirect_uri: returnUri,
    grant_type: "authorization_code",
  });

  const response = await fetch(tokenEndpoint, {
    method: "POST",
    headers: {
      "Content-Type": "application/x-www-form-urlencoded",
    },
    body: params.toString(),
  });

  const data = await response.json();
  localStorage.setItem("access_token", data.access_token);
  localStorage.setItem("refresh_token", data.refresh_token);
  localStorage.setItem("id_token", data.id_token);
};
```

### 3. Refresh the Access Token

Refresh the access token using the refresh token:

```javascript
const refreshAccessToken = async () => {
  const GOOGLE_CLIENTID = import.meta.env.VITE_CLIENT_ID;
  const GOOGLE_CLIENT_SECRET = import.meta.env.VITE_CLIENT_SECRET;

  const params = new URLSearchParams({
    client_id: GOOGLE_CLIENTID,
    client_secret: GOOGLE_CLIENT_SECRET,
    refresh_token: localStorage.getItem("refresh_token"),
    grant_type: "refresh_token",
  });

  const response = await fetch("https://oauth2.googleapis.com/token", {
    method: "POST",
    headers: {
      "Content-Type": "application/x-www-form-urlencoded",
    },
    body: params.toString(),
  });

  const data = await response.json();
  localStorage.setItem("access_token", data.access_token);
};
```

### Notes:

- **Access Token**: Used to access resources (expires in 1 hour).
- **Refresh Token**: Used to refresh the access token.
- **ID Token**: JWT format, can be verified.

If the refresh token is expired, redirect to the sign-in page.

Access token verification endpoint:

```javascript
const endpoint = `https://www.googleapis.com/oauth2/v2/tokeninfo?access_token=${accessToken}`;
```

## Google OAuth 2.0(Usage Explained)

1. Get a response of Oauth with params

```
https://accounts.google.com/o/oauth2/v2/auth
```

searchParams:

```
- client_id
- redirect_uri
- response_type
//response type **token** for just ccess_token & code to set **access_type** to offline.
- scope
- prompt
```

Get request with the searchparams

```
https://accounts.google.com/o/oauth2/v2/auth?
 scope=https%3A//www.googleapis.com/auth/drive.metadata.readonly&
 access_type=offline&
 include_granted_scopes=true&
 response_type=code&
 state=state_parameter_passthrough_value&
 redirect_uri=https%3A//oauth2.example.com/code&
 client_id=client_id
```

[reference](https://developers.google.com/identity/protocols/oauth2/web-server#httprest)

2. Get access token and refresh token from code [not for response_type=token as access_token is received in above step]

```http
POST /token HTTP/1.1
Host: oauth2.googleapis.com
Content-Type: application/x-www-form-urlencoded

code=4/P7q7W91a-oMsCeLvIaQm6bTrgtp7&
client_id=your_client_id&
client_secret=your_client_secret&
redirect_uri=https%3A//oauth2.example.com/code&
grant_type=authorization_code
```

- POST request on `https://oauth2.googleapis.com/token` with above params as formdata.

**_response.json_**

```json
{
  "access_token": "1/fFAGRNJru1FTz70BzhT3Zg",
  "expires_in": 3920,
  "token_type": "Bearer",
  "scope": "https://www.googleapis.com/auth/drive.metadata.readonly",
  "refresh_token": "1//xEoDL4iW3cxlI7yDbSRFYNG01kVKM2C-259HOF2aQbI"
}
```

[reference](https://developers.google.com/identity/protocols/oauth2/web-server#httprest)

3. Refresh the access token

```http
POST /token HTTP/1.1
Host: oauth2.googleapis.com
Content-Type: application/x-www-form-urlencoded

client_id=your_client_id&
client_secret=your_client_secret&
refresh_token=refresh_token&
grant_type=refresh_token
```

POST request on https://oauth2.googleapis.com/token with above params as formdata.

**_response.json_**

```json
{
  "access_token": "1/fFAGRNJru1FTz70BzhT3Zg",
  "expires_in": 3920,
  "scope": "https://www.googleapis.com/auth/drive.metadata.readonly",
  "token_type": "Bearer"
}
```

[reference](https://developers.google.com/identity/protocols/oauth2/web-server#httprest)

### Discussion

- [Google OAuth 2.0 Access Token and Refresh Token Explained](https://medium.com/starthinker/google-oauth-2-0-access-token-and-refresh-token-explained-cccf2fc0a6d9)
- [How can I verify a Google authentication API access token?](https://stackoverflow.com/questions/359472/how-can-i-verify-a-google-authentication-api-access-token)
- Check out [react-oauth](https://github.com/MomenSherif/react-oauth)
