---
published: true
title: 'Error 405 : ASP.NET Core Web API PUT and DELETE Methods not allowed'
cover_image: 'https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/master/assets/blog-cover/net-core.png'
description: 'PUT and DELETE Methods not allowed in ASP.NET Core Web API (405 Error)'
tags: csharp, dotnet, api, programming
series:
canonical_url:
---

### Reason:
The `WebDAVModule` set PUT and DELETE request methods disabled by default and due to that PUT and DELETE throw 405 errors.

### Solution:
To make the PUT and DELETE requests work, we need to override the `WebDAVModule` setting in `web.config` file by adding the below settings under the `system.webServer`.

```
<system.webServer>
  <modules runAllManagedModulesForAllRequests="false">
    <remove name="WebDAVModule" />
  </modules>
</system.webServer>
```

> There may be 2 web.config files in your project, if that is the case then update the one that is inside the wwwroot folder.

> Also with the latest version of .Net CORE (2.0 and above), there might be a case of no web.config file available at all, if that is your case then add a web.config file on your own.

### NOTE:
If you are facing the same issue with multiple APIs hosted on the same server, then either you can add the above entries under `web.config` file of all the affected API’s or you can remove the below entry of `WebDAVModule` from `ApplicationHost.config` file under the module section:

```
<add name="WebDAVModule" />
```

`ApplicationHost.config` can be found at

> C:\Windows\System32\inetsrv\config

Hope that helps !!!

---
_Originally published at [http://diary-techies.blogspot.com](https://diary-techies.blogspot.com/2019/01/error-405-aspnet-core-web-api-put-and.html) on 
January 07, 2019._
