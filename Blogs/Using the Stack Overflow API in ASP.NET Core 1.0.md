---
title: Using the Stack Overflow API in ASP.NET Core 1.0
author: Martin Kearn
description: I recently had the need to use the Stack Overflow API in a web project and it took me a while so I thought I'd capture what I learnt.
image: http://martink.me/images/MartinKearnProfile1.jpg
thumbnail: http://martink.me/images/MartinKearnProfile1.jpg
type: article
status: published
published: 2016/05/07 09:30:00
categories: 
  - ASP.net
  - Stack Overflow
---

# Using the Stack Overflow API in ASP.NET Core 1.0
I recently had the need to use the [Stack Overflow API](http://api.stackexchange.com/) in a web project and it took me a while so I thought I'd capture what I learnt.

## The Stack Overflow API
Stack Overflow have a really nice REST API. It lets you get to pretty much any information in Stack Overflow and is [really well documented](http://api.stackexchange.com/docs).

The API follows the usual pattern of OAuth authentication. You register your application, get a key and then follow the usual [OAuth Authentication procedure](http://api.stackexchange.com/docs/authentication).

The nice thing about the API is that a lot of the methods do not require authentication and you can access them with a simple, unauthenticated GET request. You can see all the methods and whether authentication is required here: http://api.stackexchange.com/docs. 

There are request limits and other restrictions for unauthenticated apps, but you can do things like search for unanswered questions without authentication.

## GZIP and DEFLATE
The one thing about the API which took me a while to figure out is that the API uses [GZIP or DEFLATE compression](http://api.stackexchange.com/docs/compression) by default.

This means that if you call the API as usual, you will get what looks like a corrupted response. In debug, it will look something like this because the response is not being uncompressed in your code:

```
"\u001f�\b\0\0\0\0\0\u0004\0���r�F��_eJ��/�0��O�R.�����.��\\�Օj0\u0018��@�\u0006@QL*\u000fu�\u000f��\u0003@\u0004I�\u0006���lB\u0001��0�\u000f{�{\u001a��\"����x�\u007f^�r�\u001f.d���u}���>2\u001f���E��uy��ϋR�7���\"�x��_l*]ޤ��\v�q/da{�ޭ��\v�x�V�.u\f��e����IWr�'�u��^X�v��\\��^ֲ�T��j>ZZxv��\u001e\u0013*�Q\u001c\u0005<d~ \u0018�\u0013�G����������4�y��\"\u007fV����g��\u001c~[�V�L�nr��_�e�V�4�\v������\u0001��U-�]q��$+��\u0006p\b�ՎȪAz�H�z~�V72��fL/\u0012�U\u001a\u001eS��7�����\v[<�h.��S�TQ�]P��ɪ���N��zw\u0013�\u001a�s��\u001d;�m8�Jm�n�\u0014����\u001b�\v<��[�~\u0010���=�NVY�Ƃ\u0019�0�t���t�i�#�Jfj���NuY\u0016�������L�NAPAz\n�
```

## How to Call the API and decompress the response
The solution to this is actually really simple and I discovered it in this [Stack Overflow thread](http://stackoverflow.com/questions/839888/httpwebrequest-native-gzip-compression/7775204?noredirect=1#comment59368488_7775204) (in the comments of the main answer).

You have to create a `HttpClientHandler`, set the `AutomaticDecompression` property. You then use that to create your `HttpClient` as usual.

This is my complete c# code that uses the [Search function](http://api.stackexchange.com/docs/search) to get unanswered questions on ASP.NET

```
HttpClientHandler handler = new HttpClientHandler();
handler.AutomaticDecompression = DecompressionMethods.GZip | DecompressionMethods.Deflate;
using (var httpClient = new HttpClient(handler))
{
    var apiUrl = ("http://api.stackexchange.com/2.2/search/advanced?order=desc&sort=activity&accepted=False&tagged=asp.net&site=stackoverflow");

    //setup HttpClient
    httpClient.BaseAddress = new Uri(apiUrl);
    httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

    //make request
    var response = await httpClient.GetStringAsync(apiUrl);
}
```

## In Summary
Today I learnt how to decompress compressed API responses. Stack Overflow is the only API I've come across that compresses its responses, but I'm sure there are others so this will be useful in the future.

### Resources
* [Stack Overflow API](http://api.stackexchange.com/)
* [Stack Overflow API Compression Docs](http://api.stackexchange.com/docs/compression)
* [HttpClientHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler.aspx)
