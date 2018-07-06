---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: Erstellen von Komponententests für ASP.NET MVC-Anwendungen (c#) | Microsoft-Dokumentation
author: StephenWalther
description: Erfahren Sie, wie Komponententests für Controlleraktionen zu erstellen. In diesem Tutorial veranschaulicht das Stephen Walther zu prüfen, ob eine Controlleraktion eine geben zurückgegeben...
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: f9e6945a379d37f1539c7135041f50dcc7041750
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826678"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a>Erstellen von Komponententests für ASP.NET MVC-Anwendungen (c#)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

[PDF herunterladen](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> Erfahren Sie, wie Komponententests für Controlleraktionen zu erstellen. In diesem Tutorial veranschaulicht das Stephen Walther zu prüfen, ob eine Controlleraktion eine bestimmte Ansicht zurückgibt, einen bestimmten Satz von Daten gibt zurück oder einen anderen Typ von Aktionsergebnis.


Das Ziel in diesem Tutorial wird veranschaulicht, wie Sie Komponententests für die Controller in Ihre ASP.NET MVC Anwendungen schreiben können. Gewusst wie: Erstellen Sie drei verschiedene Typen von Komponententests erörtert. Erfahren Sie, so testen Sie die Sicht zurückgegeben werden, indem eine Controlleraktion, so testen Sie die Daten anzeigen, die eine Controlleraktion vom und zum Testen, und zwar unabhängig davon, ob Sie eine Controlleraktion auf eine zweite Controlleraktion umleitet.

## <a name="creating-the-controller-under-test"></a>Erstellen des Controllers im Test

Wir erstellen zunächst den Controller, den wir testen möchten. Der Controller, mit dem Namen der `ProductController`, in der Liste 1 enthalten ist.

**Codebeispiel 1: `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

Die `ProductController` enthält zwei Aktionsmethoden `Index()` und `Details()`. Beide Aktionsmethoden geben eine Ansicht zurück. Beachten Sie, dass die `Details()` Aktion akzeptiert einen Parameter, die mit dem Namen Id.

## <a name="testing-the-view-returned-by-a-controller"></a>Testen die Ansicht zurückgegeben durch einen Controller

Angenommen, wir testen möchten, ob die `ProductController` gibt die richtige Ansicht zurück. Wir möchten sicherstellen, die bei der `ProductController.Details()` Aktion wird aufgerufen, die Detailansicht wird zurückgegeben. Die Testklasse im Codebeispiel 2 enthält einen Komponententest für das Testen der Sicht zurückgegeben werden, indem die `ProductController.Details()` Aktion.

**Codebeispiel 2: `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

Die Klasse im Codebeispiel 2 enthält eine Testmethode, die mit dem Namen `TestDetailsView()`. Diese Methode enthält drei Zeilen Code. Die erste Zeile des Codes erstellt eine neue Instanz der dem `ProductController` Klasse. Die zweite Codezeile ruft des Controllers `Details()` Aktionsmethode. Zum Schluss die letzte Zeile der Code wird überprüft, ob die Ansicht, durch zurückgegeben, die `Details()` Aktion wird die Detailansicht.

Die `ViewResult.ViewName` Eigenschaft darstellt, den Namen der Ansicht, die von einem Controller zurückgegeben. Eine eindringliche Warnung zum Testen dieser Eigenschaft. Es gibt zwei Möglichkeiten, ein Controller eine Ansicht zurückkehren kann. Ein Controller kann explizit eine Ansicht wie folgt zurückgegeben:

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

Alternativ kann der Name der Ansicht aus dem Namen der Controlleraktion wie folgt abgeleitet werden:

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

Diese Controlleraktion gibt auch eine Ansicht namens `Details`. Allerdings wird der Name der Sicht aus den Aktionsnamen abgeleitet. Wenn Sie den Ansichtsnamen testen möchten, müssen Sie explizit den Ansichtsnamen aus die Controlleraktion zurückgeben.

Sie können den Komponententest in Liste 2 ausführen, indem Sie die Tastenkombination eingeben **STRG + R, A** oder durch Klicken auf die **Ausführen aller Tests in der Projektmappe** Schaltfläche (siehe Abbildung 1). Wenn der Test erfolgreich ist, sehen Sie im Fenster Testergebnisse in Abbildung 2.


[![Führen Sie aller Tests in der Lösung aus](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)

**Abbildung 01**: Führen Sie alle Tests in der Projektmappe ([klicken Sie, um das Bild in voller Größe anzeigen](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))


[![Success!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)

**Abbildung 02**: Erfolg! ([Klicken Sie, um das Bild in voller Größe anzeigen](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Testen das Anzeigen von Daten zurückgegeben von einem Controller

Ein MVC-Controller übergibt Daten an eine Ansicht mit der so genannte *`View Data`*. Beispiel: Angenommen, dass Sie die Details für ein bestimmtes Produkt angezeigt wird, wenn Sie aufrufen möchten die `ProductController Details()` Aktion. In diesem Fall können Sie eine Instanz von erstellen eine `Product` Klasse (definiert in Ihrem Modell), und übergeben Sie die Instanz, die die `Details` Ansicht durch die Nutzung von `View Data`.

Die geänderte `ProductController` in Programmausdruck 3 enthält eine aktualisierte `Details()` Aktion, die ein Produkt zurück.

**Codebeispiel 3: `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

Zunächst wird die `Details()` Aktion erstellt eine neue Instanz der der `Product` Klasse, die einen Laptop darstellt. Anschließend wird die Instanz von der `Product` Klasse übergeben wird, als der zweite Parameter für die `View()` Methode.

Sie können Komponententests schreiben, testen, ob die erwarteten Daten sind Daten enthalten, die in der Ansicht. Im Komponententest in Listing 4 Tests, und zwar unabhängig davon, ob ein Produkt, das einen Laptop darstellt zurückgegeben wird, beim Aufrufen der `ProductController Details()` Aktionsmethode.

**Codebeispiel 4: `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

In Listing 4 die `TestDetailsView()` Methode testet den Sichtdaten zurückgegeben werden, durch den Aufruf der `Details()` Methode. Die `ViewData` wird als Eigenschaft verfügbar, auf die `ViewResult` durch den Aufruf zurückgegebenen der `Details()` Methode. Die `ViewData.Model` Eigenschaft enthält das Produkt an die Ansicht übergeben. Der Test einfach überprüft, ob das Produkt in die Ansichtsdaten enthalten den Namen Laptop verfügt.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Testen das Aktionsergebnis zurückgegeben durch einen Controller

Eine komplexere Controlleraktion möglicherweise verschiedene Arten von Aktionsergebnissen abhängig von den Werten der Parameter der Controlleraktion, die an zurück. Eine Controlleraktion kann eine Vielzahl von Typen von Aktionsergebnissen einschließlich Zurückgeben einer `ViewResult`, `RedirectToRouteResult`, oder `JsonResult`.

Z. B. die geänderte `Details()` Aktion in Listing 5 gibt den `Details` anzeigen, wenn Sie ein gültige Produkt-Id an die Aktion übergeben. Wenn Sie eine ungültige Produkt-Id – eine Id mit einem Wert, kleiner übergeben als 1 – dann umgeleitet werden, um die `Index()` Aktion.

**Codebeispiel 5: `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

Sie können das Verhalten der Testen der `Details()` Aktion mit den Komponententest in Codebeispiel 6. Der Komponententest in Codebeispiel 6 stellt sicher, dass Sie zur umgeleitet werden die `Index` anzeigen, wenn eine Id mit dem Wert-1 zu übergeben, wird die `Details()` Methode.

**Codebeispiel 6: `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

Beim Aufrufen der `RedirectToAction()` -Methode in eine Controlleraktion, die Controlleraktion gibt eine `RedirectToRouteResult`. Der Test überprüft, ob die `RedirectToRouteResult` leitet den Benutzer an eine Controlleraktion, die mit dem Namen `Index`.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie Komponententests für MVC-Controlleraktionen zu erstellen. Zuerst, wie Sie überprüfen, ob die richtige Ansicht von eine Controlleraktion zurückgegeben wird. Sie haben gelernt, wie mit der `ViewResult.ViewName` Eigenschaft, um zu überprüfen, ob der Name einer Sicht.

Als Nächstes, wir untersucht, wie Sie den Inhalt der testen können `View Data`. Haben Sie gelernt, überprüfen Sie, ob das richtige Produkt liegendes `View Data` nach dem Aufruf von eine Controlleraktion.

Abschließend wird erläutert, wie Sie testen können, ob verschiedene Arten von Aktionsergebnissen aus eine Controlleraktion zurückgegeben werden. Sie erfahren, wie zu prüfen, ob ein Controller gibt eine `ViewResult` oder `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Nächste](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
