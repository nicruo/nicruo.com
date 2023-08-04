+++ 
date = 2019-04-10T17:30:00+00:00
title = "Check Exchange credentials with Basic Authentication"
aliases = ['/2019/04/10/check-exchange-credentials-with-basic-authentication', '/2019/04/10/check-exchange-credentials-with-basic-authentication.html']
+++

With an http request, you can test if a user's Exchange credentials are valid.

Here is an example in c#

```csharp
string username = "...@...";
string password = "...";
string ewsEndPoint = "https://.../EWS/Exchange.asmx";

var byteArray = Encoding.ASCII.GetBytes(username + ":" + password);

var authenticationHeaderValue = new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));

HttpClient client = new HttpClient();
client.DefaultRequestHeaders.Authorization = authenticationHeaderValue;

var httpResponseMessage = await client.GetAsync(ewsEndPoint);

```

It's a simple way to use your Exchange as an authentication provider.