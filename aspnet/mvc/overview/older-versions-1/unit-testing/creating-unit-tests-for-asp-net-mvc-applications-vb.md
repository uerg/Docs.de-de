---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: "Erstellen von Komponententests für ASP.NET MVC-Anwendungen (VB) | Microsoft Docs"
author: StephenWalther
description: "Informationen Sie zum Erstellen von Komponententests für Controlleraktionen. In diesem Lernprogramm veranschaulicht das Stephen Walther zu prüfen, ob eine Controlleraktion eine geben gibt..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: d92ee0c26787e5c482e8695001d8809d3ee9ee30
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Erstellen von Komponententests für ASP.NET MVC-Anwendungen (VB)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

[PDF herunterladen](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Informationen Sie zum Erstellen von Komponententests für Controlleraktionen. In diesem Lernprogramm veranschaulicht Stephen Walther testen, ob eine Controlleraktion gibt eine bestimmte Ansicht zurück, einen bestimmten Satz von Daten gibt zurück oder einen anderen Typ von Aktionsergebnis.


Das Ziel dieses Lernprogramms wird veranschaulicht, wie Sie Komponententests für die Controller in Ihre ASP.NET MVC Anwendungen schreiben können. Es wird erläutert, wie drei verschiedene Typen von Komponententests zu erstellen. Erfahren Sie, so testen Sie die Sicht eine Controlleraktion zurückgegebenes So testen Sie eine Controlleraktion zurückgegebene Daten für die Ansicht und zum Testen, und zwar unabhängig davon, ob eine Controlleraktion leitet Sie an eine zweite Controlleraktion.

## <a name="creating-the-controller-under-test"></a>Erstellen den Controller getesteten

Zunächst erstellen den Controller, den wir testen möchten. Der Controller, mit dem Namen der `ProductController`, Auflisten von 1 enthalten ist.

**Auflisten von 1 –`ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

Die `ProductController` enthält zwei Aktionsmethoden `Index()` und `Details()`. Beide Aktionsmethoden geben eine Sicht zurück. Beachten Sie, dass die `Details()` Aktion akzeptiert einen Parameter namens-ID

## <a name="testing-the-view-returned-by-a-controller"></a>Testen die Sicht zurückgegebenes einen Controller

Angenommen, wir testen möchten, und zwar unabhängig davon, ob die `ProductController` gibt die richtige Sicht zurück. Wir möchten sicherstellen, dass bei der `ProductController.Details()` Aktion aufgerufen wird, wird die Detailansicht zurückgegeben. Die Testklasse im Codebeispiel 2 enthält einen Komponententest für das Testen der Sicht zurückgegeben werden, indem die `ProductController.Details()` Aktion.

**Auflisten von 2 –`ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

Die Klasse im Codebeispiel 2 umfasst eine Testmethode, die mit dem Namen `TestDetailsView()`. Diese Methode enthält drei Codezeilen. Die erste Zeile des Codes erstellt eine neue Instanz der dem `ProductController` Klasse. Die zweite Zeile des Codes ruft des Controllers `Details()` Aktionsmethode. Zum Schluss die letzte Zeile der Code wird überprüft, und zwar unabhängig davon, ob die Sicht, durch zurückgegeben, die `Details()` Aktion ist die Detailansicht.

Die `ViewResult.ViewName` Eigenschaft darstellt, den Namen der Sicht, die von einem Controller zurückgegeben. Eine große Warnung zum Testen dieser Eigenschaft. Es gibt zwei Möglichkeiten, ein Controller eine Ansicht zurückkehren kann. Ein Controller kann eine Sicht wie folgt explizit zurückgeben:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

Alternativ kann der Name der Sicht nicht mit dem Namen der Controlleraktion wie folgt abgeleitet werden:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Diese Controlleraktion gibt auch eine Sicht mit dem Namen `Details`. Allerdings wird der Name der Sicht aus den Aktionsnamen abgeleitet. Wenn Sie den Ansichtsnamen testen möchten, müssen Sie explizit den Ansichtsnamen aus Controlleraktion zurückgeben.

Sie können den Komponententest in Auflisten von 2 ausführen, indem Sie die Tastenkombination eingeben **STRG + R, A** oder durch Klicken auf die **Ausführen aller Tests in der Projektmappe** Schaltfläche (siehe Abbildung 1). Wenn der Test erfolgreich ist, sehen Sie im Fenster Testergebnisse in Abbildung 2.


[![Führen Sie aller Tests in der Projektmappe aus](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Abbildung 01**: Führen Sie alle Tests in der Projektmappe ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))


[![War erfolgreich!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Abbildung 02**: Erfolg! ([Klicken Sie hier, um das Bild in voller Größe angezeigt](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Testen die Ansichtsdaten zurückgegebenes eines Controllers

Ein MVC-Controller übergibt Daten an eine Ansicht mit der so genannte  *`View Data`* . Angenommen, dass Sie die Details für ein bestimmtes Produkt anzuzeigen, wenn Sie aufrufen möchten die `ProductController Details()` Aktion. In diesem Fall können Sie eine Instanz des erstellen eine `Product` Klasse (definiert in Ihrem Modell), und übergeben Sie die Instanz, die die `Details` Ansicht durch nutzen `View Data`.

Das geänderte `ProductController` auflisten 3 enthält ein aktualisiertes `Details()` Aktion, die ein Produkt zurückgibt.

**Auflisten von 3:`ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

Zuerst die `Details()` Aktion erstellt eine neue Instanz der dem `Product` Klasse, die einen Laptop darstellt. Anschließend wird die Instanz von der `Product` -Klasse als zweiten Parameter an übergeben der `View()` Methode.

Sie können Komponententests schreiben zu prüfen, ob die erwarteten Daten sind Daten enthalten, die in der Sicht. Den Komponententest im Codebeispiel 4 Tests, und zwar unabhängig davon, ob ein Produkt, das einen Laptop darstellt zurückgegeben wird, beim Aufrufen der `ProductController Details()` Aktionsmethode.

**Auflisten von 4 –`ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

Auflisten von 4 der `TestDetailsView()` Methode überprüft durch den Aufruf zurückgegebenen Daten für die Ansicht der `Details()` Methode. Die `ViewData` verfügbar gemacht als Eigenschaft für die `ViewResult` durch den Aufruf zurückgegebenen die `Details()` Methode. Die `ViewData.Model` -Eigenschaft enthält das Produkt, die an die Ansicht übergeben. Der Test einfach überprüft, ob das Produkt, die in die Ansichtsdaten enthalten den Namen Laptop verfügt.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Testen das Aktionsergebnis zurückgegebenes eines Controllers

Eine komplexere Controlleraktion möglicherweise unterschiedliche Typen von Aktionsergebnisse abhängig von den Werten der an die Controlleraktion übergebenen Parameter zurück. Eine Controlleraktion kann eine Vielzahl von Aktionsergebnisse einschließlich Zurückgeben einer `ViewResult`, `RedirectToRouteResult`, oder `JsonResult`.

Z. B. das geänderte `Details()` Aktion im Codebeispiel 5 gibt den `Details` anzeigen, wenn Sie ein gültiges Produkt-Id an die Aktion übergeben. Wenn Sie eine ungültige Produkt-Id – eine Id mit einem Wert kleiner übergeben als 1 – können Sie umgeleitet werden, um die `Index()` Aktion.

**Auflisten von 5 –`ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

Sie können das Verhalten Testen der `Details()` Aktion mit dem Komponententest 6 auflisten. Der Komponententest Auflisten von 6 stellt sicher, dass Sie zur umgeleitet werden die `Index` anzeigen, wenn eine Id mit dem Wert-1, um übergeben wird die `Details()` Methode.

**Auflisten von 6 –`ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Beim Aufrufen der `RedirectToAction()` in eine Controlleraktion, Controlleraktion Methodenrückgabe eine `RedirectToRouteResult`. Der Test prüft, ob die `RedirectToRouteResult` den Benutzer weitergeleitet wird, eine Controlleraktion mit dem Namen `Index`.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gelernt, wie Komponententests für Controlleraktionen MVC erstellt wird. Zunächst haben Sie gelernt, wie Sie überprüfen, ob die richtige Ansicht durch eine Controlleraktion zurückgegeben wird. Sie haben gelernt, wie die `ViewResult.ViewName` Eigenschaft, um zu überprüfen, ob der Name einer Sicht.

Als Nächstes untersucht wie Sie testen können, den Inhalt der `View Data`. Haben Sie gelernt, überprüfen Sie, ob das richtige Produkt in zurückgegebener `View Data` nach dem Aufrufen einer Controlleraktion.

Abschließend wird erläutert, wie Sie testen können, ob eine Controlleraktion verschiedene Informationstypen Aktionsergebnisse zurückgegeben werden. Sie haben gelernt, wie zu prüfen, ob ein Controller gibt eine `ViewResult` oder ein `RedirectToRouteResult`.

>[!div class="step-by-step"]
[Zurück](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
