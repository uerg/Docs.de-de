---
title: Client-IP-Listen sicherer Adressen für ASP.NET Core
author: damienbod
description: Erfahren Sie, wie Middleware oder Aktion Schreibfilter zum remote-IP-Adressen mit einer Liste der zulässigen IP-Adressen zu überprüfen.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040109"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>Client-IP-Listen sicherer Adressen für ASP.NET Core

Durch [Damien Bowden](https://twitter.com/damien_bod) und [Tom Dykstra](https://github.com/tdykstra)
 
Dieser Artikel zeigt zwei Möglichkeiten, eine IP-Listen sicherer Adressen (auch bekannt als eine Whitelist) zu implementieren:

* Mithilfe von ASP.NET Core-Middleware die remote IP-Adresse jeder Anforderung überprüfen.
* Mithilfe von ASP.NET Core-Aktionsfilter um die Anforderungen für bestimmte Aktionsmethoden remote IP-Adresse zu überprüfen.

Die Beispiel-app zeigt beide Ansätze. In jedem Fall wird eine Zeichenfolge, die genehmigte Client-IP-Adressen in einer app-Einstellung gespeichert. Die Middleware oder Filter analysiert die Zeichenfolge in eine Liste, und überprüft, ob die remote-IP in der Liste ist. Wenn dies nicht der Fall ist, ein HTTP 403 Verboten-Statuscode zurückgegeben.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-safelist"></a>Die Listen sicherer Adressen

Die Liste ist so konfiguriert, der *"appSettings.JSON"* Datei. Dabei handelt es sich eine durch Semikolons getrennte Liste kann IPv4 und IPv6-Adressen enthalten.

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>Middleware

Die `Configure` Methode fügt die Middleware und die Zeichenfolge von Listen sicherer Adressen in einem Konstruktorparameter an diese übergibt.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

Die Middleware analysiert die Zeichenfolge in ein Array und sucht nach der remote-IP-Adresse in das Array. Wenn die remote-IP-Adresse nicht gefunden wird, gibt die Middleware zurück, die HTTP-401 nicht zulässig. Dieser Überprüfungsvorgang wird umgangen, um die HTTP Get-Anforderungen.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Action-filter

Wenn Sie eine von Listen sicherer Adressen nur für bestimmte Controller oder Aktionsmethoden anwenden möchten, verwenden Sie einen Aktionsfilter. Im Folgenden ein Beispiel: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

Der Action-Filter wird zum Dienstcontainer hinzugefügt.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Der Filter kann dann für eine Methode Controller bzw. die Aktionsmethode verwendet werden.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

In der Beispiel-app der Filter angewendet wird, auf die `Get` Methode. Wenn die app zu, indem Sie senden testen also eine `Get` API anfordern, die das Attribut ist die Client-IP-Adresse überprüfen. Wenn Sie testen, indem Sie die API mit anderen HTTP-Methode aufrufen, ist die Middleware die Client-IP-Adresse überprüfen.

## <a name="razor-pages-filter"></a>Razor-Seiten zu filtern. 

Wenn Sie eine von Listen sicherer Adressen für eine Razor-Seiten-app möchten, verwenden Sie einen Razor-Seiten-Filter. Im Folgenden ein Beispiel: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

Dieser Filter ist aktiviert, indem sie auf die Auflistung der MVC-Filter hinzufügen.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

Wenn Sie die app ausführen und eine Razor-Seite anfordern, ist der Filter für die Razor-Seiten die Client-IP-Adresse überprüfen.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über ASP.NET Core-Middleware](xref:fundamentals/middleware/index).
