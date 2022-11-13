---
layout: post
title:  "Authentication with Azure Static Web Apps"
category: development
tag: Static Web Apps
---

# Authentication with Azure Static Web Apps

## Getting a User in an API Function
Microsoft provides [documentation](https://learn.microsoft.com/en-us/azure/static-web-apps/user-information?tabs=csharp) on how to get a user in a C# function. In short, the `x-ms-client-principal` header will be populated with base-64-encoded json representing a `ClaimsPrincipal`.

## Spoofing the Principal (in development)
The code in the documentation works. The `x-ms-client-principal` header is present and can be parsed as expected. 

But, where does this header get set? It is important to be able to trust it, because future authorization decisions will be made off the content of the header. 
Using the SWA emulator, it looks like the authentication information is included in a cookie, just like any other session. The follwing cookie is present only when authenticated:
```txt
StaticWebAppsAuthCookie=eyJ1c2VySWQiOiJzdGF0aWMtd2ViLWFwcC1pZC1sZWdpdCIsInVzZXJSb2xlcyI6WyJhbm9ueW1vdXMiLCJhdXRoZW50aWNhdGVkIl0sImNsYWltcyI6W10sImlkZW50aXR5UHJvdmlkZXIiOiJnaXRodWIiLCJ1c2VyRGV0YWlscyI6ImxlZ2l0aW1hdGVVc2VyMTIzIn0=
```

When calling the API from the frontend code, the `x-ms-client-principal` header is not included. Probably the information is sent in a cookie, and then when the proxy routes to the API, the header is set.

But if an attacker were to set the header in a request to an API endpoint - what would happen? It appears that the attacker's header will get sent all the way through! For example, take the following claims principal: 
```json
{"userId":"static-web-app-id-legit","userRoles":["anonymous","authenticated"],"identityProvider":"github","userDetails":"legitimateUser123"}
```
The base-64 encoding is this, which is the same as the StaticWebAppsAuthCookie value seen earlier! 
```txt
eyJ1c2VySWQiOiJzdGF0aWMtd2ViLWFwcC1pZC1sZWdpdCIsInVzZXJSb2xlcyI6WyJhbm9ueW1vdXMiLCJhdXRoZW50aWNhdGVkIl0sImlkZW50aXR5UHJvdmlkZXIiOiJnaXRodWIiLCJ1c2VyRGV0YWlscyI6ImxlZ2l0aW1hdGVVc2VyMTIzIn0=
```

Let's say an attacker wanted to impersonate `vip-user`. They could write the following JSON: 
```json
{"userId":"vip-user","userRoles":["anonymous","authenticated"],"identityProvider":"github","userDetails":"you are pwnd"}
```
And after encoding it, set the following header: 
```txt
x-ms-client-principal: eyJ1c2VySWQiOiJ2aXAtdXNlciIsInVzZXJSb2xlcyI6WyJhbm9ueW1vdXMiLCJhdXRoZW50aWNhdGVkIl0sImlkZW50aXR5UHJvdmlkZXIiOiJnaXRodWIiLCJ1c2VyRGV0YWlscyI6InlvdSBhcmUgcHduZCJ9
```
Or, try setting the cookie:
```txt
StaticWebAppsAuthCookie=eyJ1c2VySWQiOiJ2aXAtdXNlciIsInVzZXJSb2xlcyI6WyJhbm9ueW1vdXMiLCJhdXRoZW50aWNhdGVkIl0sImlkZW50aXR5UHJvdmlkZXIiOiJnaXRodWIiLCJ1c2VyRGV0YWlscyI6InlvdSBhcmUgcHduZCJ9
```

And, it would work! The spoofed header is sent all the way to the server and `vip-user` is impersonated. 

## But Can We Spoof Prod?

The previous attack was all carried out on the SWA development emulator, and would render all SWA security meaningless if replicable in production.

So, it is a good thing that in production the cookie is encrypted. Decoding from base 64 results in gibberish rather than JSON. Sending a spoofed cookie that is only base-64 encoded, and not encrypted with the SWA's certificate, causes authentication to fail and the request is treated as anonymous. Hurrah! 

From this experiment we can see that cookies are signed in production so they can't be spoofed. In the development emulator, they aren't signed at all, and are therefore trivial to spoof.



