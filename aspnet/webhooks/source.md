---
uid: webhooks/source
title: ASP.NET WebHooks Quellcode und NuGet-Pakete | Microsoft Docs
author: rick-anderson
description: Links zu ASP.NET WebHooks Quellcode und NuGet-Pakete
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 733a0839c77bcfc96214bdf235ce8fe22ee2d3cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2018
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET WebHooks Quellcode und NuGet-Pakete

Microsoft ASP.NET WebHooks ist Teil der Microsoft ASP.NET-Familie von Modulen und gehostet wird, als ein [Open Source-Projekt auf GitHub](https://github.com/aspnet/WebHooks). Dies bedeutet, dass wir Beiträge akzeptieren, aber beachten Sie die [Beitrag Richtlinien](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) vor dem Senden eines Pull Request.

Folgende online-Dokumentation, die Sie lesen, ist jetzt auch als gehostete [Open Source auf GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) und akzeptiert auch Beiträge.

## <a name="nuget-packages"></a>NuGet-Pakete

Die [NuGet-Pakete](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sind in drei Bereiche unterteilt:

* [Allgemeine](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): eine allgemeine Paket, das zwischen Absendern und Empfängern gemeinsam verwendet wird.

* [Absender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): eine Reihe von Paketen, senden Ihre eigenen WebHooks an andere Benutzer unterstützen. Die Funktionalität zum Senden von WebHooks wird ausführlich in [senden WebHooks](sending/index.md).

* [Empfänger](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): eine Reihe von Paketen, die Unterstützung von WebHooks von anderen Personen erhalten. Die Funktionalität für den Empfang von WebHooks wird ausführlich in [empfangen von WebHooks](receiving/index.md).
