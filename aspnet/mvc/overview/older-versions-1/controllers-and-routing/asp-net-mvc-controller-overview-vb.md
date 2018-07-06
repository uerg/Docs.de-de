---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: ASP.NET MVC-Controller – Übersicht (VB) | Microsoft-Dokumentation
author: StephenWalther
description: In diesem Tutorial führt Sie Stephen Walther in ASP.NET MVC-Controller. Erfahren Sie, wie erstellen neue Domänencontroller und Zurückgeben von verschiedenen Arten von Aktion Res...
ms.author: aspnetcontent
ms.date: 02/16/2008
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: ac5e9242f494b8472e582bc76a6f4805db2f770f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809222"
---
<a name="aspnet-mvc-controller-overview-vb"></a>ASP.NET MVC-Controller – Übersicht (VB)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> In diesem Tutorial führt Sie Stephen Walther in ASP.NET MVC-Controller. Erfahren Sie, wie erstellen neue Domänencontroller und Zurückgeben von verschiedenen Arten von Aktionsergebnissen.


Dieses Tutorial wird beschrieben, das Thema der ASP.NET MVC-Controllern, Controlleraktionen und Aktionsergebnisse. Nachdem Sie dieses Tutorial abgeschlossen haben, wissen Sie, wie der Controller verwendet werden, um zu steuern, die ein Besucher mit einer ASP.NET MVC-Website interagiert.

## <a name="understanding-controllers"></a>Grundlegendes zu Controllern

MVC-Controller sind verantwortlich für die Reaktion auf Anforderungen für eine ASP.NET MVC-Website. Jede Browseranforderung wird ein bestimmter Domänencontroller zugeordnet. Beispiel: Angenommen Sie, dass Sie die folgende URL in die Adressleiste des Browsers eingeben:

`http://localhost/Product/Index/3`

In diesem Fall wird ein Controller, mit der Bezeichnung ProductController aufgerufen. ProductController ist verantwortlich für das Generieren der Antwort auf die Browseranforderung. Z. B. der Controller möglicherweise eine bestimmte Ansicht zurück an den Browser zurück, oder der Controller kann leiten Sie den Benutzer an einen anderen Controller.

Codebeispiel 1 enthält einen einfachen Controller, mit der Bezeichnung ProductController.

**Listing1 - Controllers\ProductController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

Wie in Codebeispiel 1 sehen ist, ist ein Controller nur eine Klasse (Visual Basic .NET oder C#-Klasse). Ein Controller ist eine Klasse, die von der Basisklasse von System.Web.Mvc.Controller abgeleitet wird. Da ein Controller von dieser Basisklasse erbt, erbt ein Controller mehrere nützliche Methoden für kostenlose (wir werden diese Methoden weiter unten erörtert).

## <a name="understanding-controller-actions"></a>Grundlegendes zu Controlleraktionen

Ein Controller verfügbar macht, Controlleraktionen. Eine Aktion ist eine Methode für einen Controller, der aufgerufen wird, wenn Sie eine bestimmte URL in die Adressleiste Ihres Browsers eingeben. Beispiel: Angenommen Sie, dass Sie eine Anforderung für die folgende URL ausführen:

`http://localhost/Product/Index/3`

In diesem Fall wird die Index()-Methode für die ProductController-Klasse aufgerufen. Die Index()-Methode ist ein Beispiel für eine Controlleraktion.

Eine Controlleraktion muss eine öffentliche Methode von einem Controller-Klasse. Visual Basic.NET Methoden in der Standardeinstellung sind die öffentlichen Methoden. Beachten Sie, dass jede öffentliche Methode, die Sie zu einer Controllerklasse hinzufügen als eine Controlleraktion automatisch bereitgestellt wird (Achten Sie Informationen dazu, da eine Controlleraktion von jedem Benutzer im Universum aufgerufen werden kann, indem Sie einfach die richtige URL in die Adressleiste des Browsers eingeben).

Es gibt einige zusätzlichen Anforderungen, die durch eine Controlleraktion erfüllt werden müssen. Eine Methode als eine Controlleraktion kann nicht überladen werden. Darüber hinaus darf keine Controlleraktion auf eine statische Methode sein. Davon abgesehen können Sie beliebige andere Methode als eine Controlleraktion.

## <a name="understanding-action-results"></a>Grundlegendes zu Aktionsergebnisse

Eine Controlleraktion gibt so genannte ein *Aktionsergebnis*. Ein Aktionsergebnis ist, was eine Controlleraktion als Reaktion auf eine Browseranforderung zurückgibt.

ASP.NET MVC-Framework unterstützt verschiedene Typen von Aktionsergebnissen, einschließlich:

1. ViewResult - stellt HTML und Markup.
2. EmptyResult - stellt kein Ergebnis dar.
3. RedirectResult - stellt eine Umleitung an eine neue URL dar.
4. JsonResult - stellt eine JavaScript Object Notation-Ergebnis, das in einer AJAX-Anwendung verwendet werden kann.
5. JavaScriptResult - stellt eine JavaScript-Skript dar.
6. ContentResult - stellt einen Textergebnis dar.
7. FileContentResult - stellt eine herunterladbare Datei (mit den binären Inhalt) dar.
8. FilePathResult - stellt eine herunterladbare Datei (mit einem Pfad).
9. FileStreamResult - stellt eine herunterladbare Datei (mit einem Dateistream) dar.

Alle diese Aktionsergebnisse erben von der ActionResult-Basisklasse.

In den meisten Fällen gibt eine Controlleraktion ein ViewResult zurück. Beispielsweise gibt die Index-Controlleraktion Programmausdruck 2 ein ViewResult zurück.

**Codebeispiel 2 - Controllers\BookController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

Wenn eine Aktion ein ViewResult zurückgibt, wird HTML im Browser zurückgegeben. Programmausdruck 2 die Index()-Methode gibt eine Ansicht namens Index, an den Browser zurück.

Beachten Sie, dass die Aktion Index() Programmausdruck 2 eine ViewResult() nicht zurückgegeben wird. Stattdessen wird die View()-Methode der Basisklasse Controller aufgerufen. Sie geben ein Aktionsergebnis normalerweise nicht direkt zurück. Stattdessen rufen Sie eine der folgenden Methoden der Basisklasse Controller:

1. Anzeigen – gibt ein ViewResult-Aktion-Ergebnis zurück.
2. Umleiten: Gibt ein RedirectResult-Aktion-Ergebnis zurück.
3. RedirectToAction - gibt ein Aktionsergebnis RedirectToRouteResult zurück.
4. RedirectToRoute - gibt ein Aktionsergebnis RedirectToRouteResult zurück.
5. JSON - gibt ein JsonResult-Aktion-Ergebnis zurück.
6. JavaScriptResult - gibt ein JavaScriptResult zurück.
7. Content - gibt ein ContentResult-Aktion-Ergebnis zurück.
8. Datei - gibt ein FileContentResult, FilePathResult oder FileStreamResult abhängig von den Parametern an die Methode übergeben.

Wenn eine Ansicht an den Browser zurückgegeben werden sollen, rufen Sie daher die View()-Methode. Wenn Sie den Benutzer eine Controlleraktion auf einen anderen umleiten möchten, rufen Sie die RedirectToAction()-Methode. Z. B. die Aktion Details() in Programmausdruck 3 zeigt eine Ansicht oder leitet den Benutzer an die Aktion Index(), je nachdem, ob der Id-Parameter einen Wert hat.

**Codebeispiel 3 - CustomerController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

Das Aktionsergebnis ContentResult ist etwas Besonderes. Sie können das Aktionsergebnis ContentResult verwenden, ein Aktionsergebnis als nur-Text zurückgeben. Die Index()-Methode in Listing 4 gibt z. B. eine Meldung zurück, als nur-Text und nicht als HTML.

**Programmausdruck 4 - Controllers\StatusController.vb**

> StatusController
> 
> 
> System.Web.Mvc.Controller


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

Wenn die Aktion StatusController.Index() aufgerufen wird, wird eine Sicht nicht zurückgegeben. Stattdessen der unformatierte Text "Hello World!" wird an den Browser zurückgegeben werden.

Wenn eine Controlleraktion ein Ergebnis, das kein Aktionsergebnis – z. B. ein Datum oder eine ganze Zahl – zurückgibt wird dann das Ergebnis in eine ContentResult automatisch eingebunden. Z. B. wenn die Aktion Index() der WorkController in Listing 5 aufgerufen wird, wird das Datum als eine ContentResult automatisch zurückgegeben.

**Programmausdruck 5 - WorkController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

Die Aktion Index() in Listing 5 gibt einen DateTime-Objekt zurück. ASP.NET MVC-Framework das DateTime-Objekt in eine Zeichenfolge konvertiert und dient als Wrapper für "DateTime"-Werts in einen ContentResult automatisch. Der Browser empfängt das Datum und die Uhrzeit, als nur-Text.

## <a name="summary"></a>Zusammenfassung

Der Zweck dieses Lernprogramms wurde, führen Sie die Konzepte von ASP.NET MVC-Controllern, Controlleraktionen und Ergebnisse von Controlleraktionen ein. Im ersten Abschnitt haben Sie gelernt, wie neue Domänencontroller zu einer ASP.NET MVC-Projekt hinzufügen. Anschließend haben Sie erfahren, wie öffentliche Methoden in einem Controller für das Universum als Controlleraktionen verfügbar gemacht. Zum Schluss erläutert die verschiedenen Typen von Aktionsergebnissen, die eine Controlleraktion zurückgegeben werden können. Insbesondere erläutert wie ein ViewResult RedirectToActionResult und ContentResult von eine Controlleraktion zurückgegeben.

> [!div class="step-by-step"]
> [Zurück](creating-a-custom-route-constraint-cs.md)
> [Weiter](creating-custom-routes-vb.md)
