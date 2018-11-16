---
title: Externe OAuth-Authentifizierungsanbieter
author: rick-anderson
description: Entdecken Sie externe OAuth-Authentifizierungsanbieter, die mit ASP.NET Core-apps funktionieren.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/otherlogins
ms.openlocfilehash: b69c366ec1bf12ccf434991fc8a79eaf8c09da3d
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708464"
---
# <a name="external-oauth-authentication-providers"></a><span data-ttu-id="8d2ac-103">Externe OAuth-Authentifizierungsanbieter</span><span class="sxs-lookup"><span data-stu-id="8d2ac-103">External OAuth authentication providers</span></span>

<span data-ttu-id="8d2ac-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), und [Valeriy Novytskyy](https://github.com/01binary)</span><span class="sxs-lookup"><span data-stu-id="8d2ac-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), and [Valeriy Novytskyy](https://github.com/01binary)</span></span>

<span data-ttu-id="8d2ac-105">Die folgende Liste enthält allgemeine externe OAuth-Authentifizierungsanbieter, die mit ASP.NET Core-apps funktionieren.</span><span class="sxs-lookup"><span data-stu-id="8d2ac-105">The following list includes common external OAuth authentication providers that work with ASP.NET Core apps.</span></span> <span data-ttu-id="8d2ac-106">Drittanbieter-NuGet-Pakete, z. B. diejenigen, die von verwaltet [Aspnet-Contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth), können verwendet werden, um die durch das ASP.NET Core-Team implementiert Authentifizierungsanbieter zu ergänzen.</span><span class="sxs-lookup"><span data-stu-id="8d2ac-106">Third-party NuGet packages, such as the ones maintained by [aspnet-contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth), can be used to complement the authentication providers implemented by the ASP.NET Core team.</span></span>

* <span data-ttu-id="8d2ac-107">[LinkedIn](https://www.linkedin.com/developer/apps) ([Anweisungen](https://developer.linkedin.com/docs/oauth2))</span><span class="sxs-lookup"><span data-stu-id="8d2ac-107">[LinkedIn](https://www.linkedin.com/developer/apps) ([Instructions](https://developer.linkedin.com/docs/oauth2))</span></span>

* <span data-ttu-id="8d2ac-108">[Instagram](https://www.instagram.com/developer/register/) ([Anweisungen](https://www.instagram.com/developer/authentication/))</span><span class="sxs-lookup"><span data-stu-id="8d2ac-108">[Instagram](https://www.instagram.com/developer/register/) ([Instructions](https://www.instagram.com/developer/authentication/))</span></span>

* <span data-ttu-id="8d2ac-109">[Reddit](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps) ([Anweisungen](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example))</span><span class="sxs-lookup"><span data-stu-id="8d2ac-109">[Reddit](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps) ([Instructions](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example))</span></span>

* <span data-ttu-id="8d2ac-110">[Github](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew) ([Anweisungen](https://developer.github.com/v3/oauth/))</span><span class="sxs-lookup"><span data-stu-id="8d2ac-110">[Github](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew) ([Instructions](https://developer.github.com/v3/oauth/))</span></span>

* <span data-ttu-id="8d2ac-111">[Yahoo](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F) ([Anweisungen](https://developer.yahoo.com/bbauth/user.html))</span><span class="sxs-lookup"><span data-stu-id="8d2ac-111">[Yahoo](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F) ([Instructions](https://developer.yahoo.com/bbauth/user.html))</span></span>

* <span data-ttu-id="8d2ac-112">[Tumblr](https://www.tumblr.com/oauth/apps) ([Anweisungen](https://www.tumblr.com/docs/api/v2#auth))</span><span class="sxs-lookup"><span data-stu-id="8d2ac-112">[Tumblr](https://www.tumblr.com/oauth/apps) ([Instructions](https://www.tumblr.com/docs/api/v2#auth))</span></span>

* <span data-ttu-id="8d2ac-113">[Pinterest](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F) ([Anweisungen](https://developers.pinterest.com/docs/api/overview/?))</span><span class="sxs-lookup"><span data-stu-id="8d2ac-113">[Pinterest](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F) ([Instructions](https://developers.pinterest.com/docs/api/overview/?))</span></span>

* <span data-ttu-id="8d2ac-114">[Pocket](https://getpocket.com/developer/apps/new) ([Anweisungen](https://getpocket.com/developer/docs/authentication))</span><span class="sxs-lookup"><span data-stu-id="8d2ac-114">[Pocket](https://getpocket.com/developer/apps/new) ([Instructions](https://getpocket.com/developer/docs/authentication))</span></span>

* <span data-ttu-id="8d2ac-115">[Flickr](https://www.flickr.com/services/apps/create) ([Anweisungen](https://www.flickr.com/services/api/auth.oauth.html))</span><span class="sxs-lookup"><span data-stu-id="8d2ac-115">[Flickr](https://www.flickr.com/services/apps/create) ([Instructions](https://www.flickr.com/services/api/auth.oauth.html))</span></span>

* <span data-ttu-id="8d2ac-116">[Dribble](https://dribbble.com/signup) ([Anweisungen](http://developer.dribbble.com/v1/oauth/))</span><span class="sxs-lookup"><span data-stu-id="8d2ac-116">[Dribble](https://dribbble.com/signup) ([Instructions](http://developer.dribbble.com/v1/oauth/))</span></span>

* <span data-ttu-id="8d2ac-117">[Vimeo](https://vimeo.com/join) ([Anweisungen](https://developer.vimeo.com/api/authentication))</span><span class="sxs-lookup"><span data-stu-id="8d2ac-117">[Vimeo](https://vimeo.com/join) ([Instructions](https://developer.vimeo.com/api/authentication))</span></span>

* <span data-ttu-id="8d2ac-118">[SoundCloud](https://soundcloud.com/you/apps/new) ([Anweisungen](https://developers.soundcloud.com/blog/we-love-oauth-2))</span><span class="sxs-lookup"><span data-stu-id="8d2ac-118">[SoundCloud](https://soundcloud.com/you/apps/new) ([Instructions](https://developers.soundcloud.com/blog/we-love-oauth-2))</span></span>

* <span data-ttu-id="8d2ac-119">[VK](https://vk.com/apps?act=manage) ([Anweisungen](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites))</span><span class="sxs-lookup"><span data-stu-id="8d2ac-119">[VK](https://vk.com/apps?act=manage) ([Instructions](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites))</span></span>

[!INCLUDE[Multiple authentication providers](includes/chain-auth-providers.md)]

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]
