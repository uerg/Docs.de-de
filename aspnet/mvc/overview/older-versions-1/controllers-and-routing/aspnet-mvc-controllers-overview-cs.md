---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Übersicht über ASP.NET MVC-Controller (c#) | Microsoft Docs
author: StephenWalther
description: In diesem Lernprogramm führt Sie Stephen Walther in ASP.NET MVC-Controller. Erfahren Sie, wie zum Erstellen von neuen Controller und Aktion Res verschiedene Datentypen zurückgeben...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 95e7c555a52c8c3b765a6fffab15276491cf5714
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869087"
---
<a name="aspnet-mvc-controller-overview-c"></a>Übersicht über ASP.NET MVC-Controller (c#)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> In diesem Lernprogramm führt Sie Stephen Walther in ASP.NET MVC-Controller. Erfahren Sie, wie neue Domänencontroller zu erstellen und unterschiedliche Rückgabetypen von Aktionsergebnisse.


In diesem Lernprogramm wird das Thema der ASP.NET MVC-Controller, Controlleraktionen und Aktionsergebnisse erklärt. Nachdem Sie dieses Lernprogramm abgeschlossen haben, werden Sie verstehen, wie Domänencontroller verwendet werden, um zu steuern, die ein Besucher mit einer ASP.NET MVC-Website interagiert.

## <a name="understanding-controllers"></a>Grundlegendes zu Controllern

MVC-Controller sind verantwortlich für die Reaktion auf Anforderungen für eine ASP.NET MVC-Website. Jede Browseranforderung wird ein bestimmter Domänencontroller zugeordnet. Angenommen Sie, dass Sie die folgende URL in die Adressleiste des Browsers eingeben:

`http://localhost/Product/Index/3`

In diesem Fall wird ein Controller namens ProductController aufgerufen. Die ProductController ist verantwortlich für die Antwort auf die Browseranforderung zu generieren. Z. B. der Controller möglicherweise eine bestimmte Ansicht zurück an den Browser zurück, oder der Controller kann den Benutzer weiterleiten, auf einen anderen Controller.

Auflisten von 1 enthält einen einfachen Controller mit dem Namen ProductController.

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Wie Sie im Codebeispiel 1 sehen können, ist ein Controller nur einer Klasse (Visual Basic .NET oder C#-Klasse). Ein Domänencontroller ist eine Klasse, die von der Basisklasse System.Web.Mvc.Controller abgeleitet wird. Da es sich bei ein Domänencontroller aus dieser Basisklasse erbt, erbt ein Controller mehrere nützliche Methoden kostenlos (erörtert, diese Methoden in wenigen Augenblicken).

## <a name="understanding-controller-actions"></a>Grundlegendes zu Controlleraktionen

Ein Controller macht Controlleraktionen verfügbar. Eine Aktion ist eine Methode auf einem Domänencontroller, der aufgerufen wird, wenn Sie eine bestimmte URL in der Adressleiste des Browsers eingeben. Angenommen Sie, dass Sie eine Anforderung für die folgende URL verwenden:

`http://localhost/Product/Index/3`

In diesem Fall wird die Index()-Methode für die Klasse ProductController aufgerufen. Die Index()-Methode ist ein Beispiel für eine Controlleraktion.

Eine Controlleraktion muss eine öffentliche Methode einer Controllerklasse sein. C#-Methoden, in der Standardeinstellung werden private. Beachten Sie, dass keine öffentliche Methode, die Sie zu einer Controllerklasse hinzufügen automatisch als eine Controlleraktion bereitgestellt wird (Achten Sie Informationen hierzu, da eine Controlleraktion von einem Benutzer im Universum aufgerufen werden kann, indem Sie einfach die richtige URL in die Adressleiste des Webbrowsers eingeben).

Es gibt einige zusätzlichen Anforderungen, die durch eine Controlleraktion erfüllt werden müssen. Eine Methode als eine Controlleraktion verwendet, kann nicht überladen werden. Darüber hinaus darf keine Controlleraktion eine statische Methode sein. Als dem können Sie beliebige andere Methode als eine Controlleraktion.

## <a name="understanding-action-results"></a>Grundlegendes zu den Ergebnissen der Aktion

Gibt eine Controlleraktion zurück, so genannte ein *Aktionsergebnis*. Ein Aktionsergebnis ist, was eine Controlleraktion als Antwort auf eine Browseranforderung zurückgibt.

Das ASP.NET-MVC-Framework unterstützt mehrere Typen von Aktionsergebnisse, einschließlich:

1. ViewResult - stellt HTML und Markup.
2. EmptyResult - stellt kein Ergebnis dar.
3. RedirectResult - stellt eine Umleitung an eine neue URL um.
4. JsonResult - stellt einen JavaScript Object Notation-Ergebnis, das in einer AJAX-Anwendung verwendet werden kann.
5. JavaScriptResult - stellt eine JavaScript-Skript dar.
6. ContentResult - stellt einen Textergebnis dar.
7. FileContentResult - stellt eine herunterladbare Datei (mit dem binären Inhalt) dar.
8. FilePathResult - stellt eine herunterladbare Datei (mit einem Pfad).
9. FileStreamResult - stellt eine herunterladbare Datei (mit einem Dateistream) dar.

Alle diese Aktionsergebnisse erben von der ActionResult-Basisklasse.

In den meisten Fällen gibt eine Controlleraktion ein ViewResult zurück. Index-Controlleraktion Auflisten von 2 gibt beispielsweise ein ViewResult zurück.

**Auflisten von 2 – Controllers\BookController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

Wenn eine Aktion ein ViewResult zurückgibt, wird HTML an den Browser zurückgegeben. Die Index()-Methode im Codebeispiel 2 Gibt eine Ansicht mit dem Namen Index, an den Browser zurück.

Beachten Sie, dass die Aktion Index() Auflisten von 2 eine ViewResult() nicht zurückgegeben wird. Stattdessen wird die View()-Methode der Basisklasse Controller aufgerufen. Normalerweise wird ein Aktionsergebnis nicht direkt zurückgeben. Stattdessen rufen Sie eine der folgenden Methoden der Basisklasse Controller:

1. Anzeigen – gibt ein ViewResult-Aktion-Ergebnis zurück.
2. Umgeleitet – gibt ein RedirectResult-Aktion-Ergebnis zurück.
3. RedirectToAction - gibt ein Aktionsergebnis RedirectToRouteResult zurück.
4. RedirectToRoute - gibt ein Aktionsergebnis RedirectToRouteResult zurück.
5. JSON - gibt ein JsonResult-Aktion-Ergebnis zurück.
6. JavaScriptResult - gibt ein JavaScriptResult zurück.
7. Content - Returns a ContentResult action result.
8. Datei - gibt ein FileContentResult, FilePathResult oder FileStreamResult abhängig von den Parametern an die Methode übergeben.

Wenn Sie eine Sicht an den Browser zurückgeben möchten, rufen Sie deshalb die View()-Methode. Wenn Sie den Benutzer eine Controlleraktion in eine andere umleiten möchten, rufen Sie die RedirectToAction()-Methode. Z. B. die Aktion Details() auflisten 3 zeigt eine Ansicht oder leitet den Benutzer an die Aktion Index(), je nachdem, ob die Id-Parameter einen Wert aufweist.

**3 – CustomerController.cs auflisten**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

Das Aktionsergebnis ContentResult ist ein Sonderfall. Das Aktionsergebnis ContentResult können Sie ein Aktionsergebnis als nur-Text zurückgeben. Beispielsweise erfolgt die Methodenrückgabe Index() 4 Auflisten einer Nachricht als nur-Text und nicht als HTML.

**4 – Controllers\StatusController.cs auflisten**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

Wenn die StatusController.Index() Aktion aufgerufen wird, wird eine Sicht nicht zurückgegeben. Stattdessen den unformatierten Text "Hello World!" wird an den Browser zurückgegeben.

Wenn eine Controlleraktion als Ergebnis kein Aktionsergebnis - beispielsweise ein Datum oder eine ganze Zahl - zurückgegeben wird das Ergebnis automatisch in eine ContentResult eingebunden. Z. B. wenn die Aktion Index() der WorkController auflisten 5 aufgerufen wird, wird das Datum als eine ContentResult automatisch zurückgegeben.

**5 – WorkController.cs auflisten**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

Die Aktion Index() auflisten 5 zurückgegeben ein DateTime-Objekt. ASP.NET MVC-Framework die DateTime-Objekt in eine Zeichenfolge konvertiert und dient als Wrapper für den DateTime-Wert in einer ContentResult automatisch. Der Browser empfängt das Datum und die Uhrzeit als nur-Text.

## <a name="summary"></a>Zusammenfassung

Der Zweck dieses Lernprogramms war, Einführung in die Konzepte der ASP.NET MVC-Controller, Controlleraktionen und Controller Aktionsergebnisse. Im ersten Abschnitt haben Sie gelernt, wie neue Controller einer ASP.NET MVC-Projekt hinzugefügt wird. Als Nächstes haben Sie gelernt, wie öffentliche Methoden eines Controllers für die Universe als Controlleraktionen verfügbar gemacht. Schließlich erläutert die verschiedenen Arten von Aktionsergebnisse, die eine Controlleraktion zurückgegeben werden können. Insbesondere besprochen haben wir wie ein ViewResult, RedirectToActionResult und ContentResult in eine Controlleraktion zurückgegeben.

> [!div class="step-by-step"]
> [Zurück](creating-an-action-vb.md)
> [Weiter](creating-custom-routes-cs.md)
