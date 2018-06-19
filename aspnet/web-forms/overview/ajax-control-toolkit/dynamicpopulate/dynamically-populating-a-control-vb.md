---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Dynamisch Auffüllen eines Steuerelements (VB) | Microsoft Docs
author: wenz
description: Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e2031a80be71a406e632955583d83920dd0f3ef7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879737"
---
<a name="dynamically-populating-a-control-vb"></a>Dynamisch Auffüllen eines Steuerelements (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung.


## <a name="overview"></a>Übersicht

Die `DynamicPopulate` -Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung. Dieses Lernprogramm zeigt, wie dies einrichten.

## <a name="steps"></a>Schritte

Erstens, Sie benötigen ein ASP.NET-Webdienst implementiert die Methode aufgerufen werden, indem Sie `DynamicPopulate`. Webdienstklasse erfordert die `ScriptService` innerhalb definierte Attribut `Microsoft.Web.Script.Services`; andernfalls ASP.NET AJAX kann nicht erstellt werden den clientseitigen JavaScript-Proxy für den Webdienst, der wiederum durch erforderlich ist `DynamicPopulate`.

Die Webmethode muss erwarten, dass ein Argument vom Typzeichenfolge sind, aufgerufen `contextKey`, da die `DynamicPopulate` Steuerelement sendet ein Teil der Kontextinformationen mit jedem Aufruf der Web-Dienst. Der folgende Webdienst gibt das aktuelle Datum in einem Format dargestellt, die von der `contextKey` Argument:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

Der Webdienst wird dann als gespeichert `DynamicPopulate.vb.asmx`. Alternativ könnten Sie implementieren die `getDate()` Methode als eine Seitenmethode in der tatsächlichen ASP.NET-Seite mit der `DynamicPopulate` Steuerelement.

Erstellen Sie im nächsten Schritt eine neue ASP.NET-Datei. Wie immer der erste Schritt besteht, enthalten die `ScriptManager` in der aktuellen Seite die ASP.NET AJAX-Bibliothek geladen und die Arbeit Control Toolkit vornehmen:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

Anschließend fügen Sie ein Bezeichnungsfeld-Steuerelement (z. B. mithilfe des HTML-Steuerelements mit dem gleichen Namen oder die &lt; `asp:Label`  / &gt; Webserver-Steuerelement) wird die höher das Ergebnis den Aufruf des Webdiensts angezeigt.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

Eine HTML-Schaltfläche (als ein HTML-Steuerelement, da es ein Postback an den Server nicht erforderlich ist) wird dann verwendet werden, die dynamische Auffüllung auslösen:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Schließlich müssen wir die `DynamicPopulateExtender` Kontrolle über das Netzwerk Dinge einrichten. Die folgenden Attribute werden festgelegt werden kann (abgesehen von offensichtlichen diejenigen `ID` und `runat` = `"server"`):

- `TargetControlID` eines Speicherorts für das Ergebnis aus den Aufruf des Webdiensts
- `ServicePath` Pfad zum Webdienst (auslassen, wenn Sie eine Seitenmethode verwenden möchten)
- `ServiceMethod` Name der Webmethode oder Seitenmethode
- `ContextKey` Informationen zum Sitzungskontext an den Webdienst gesendet werden
- `PopulateTriggerControlID` Element, das den Aufruf des Webdiensts ausgelöst
- `ClearContentsDuringUpdate` angibt, ob das Zielelement während Webdienstaufruf leer

Wie Sie sehen können, das Steuerelement erfordert einige Informationen-Zustandsmodells alles vor Ort ist jedoch ziemlich selbsterklärend. Hier wird das Markup für die `DynamicPopulateExtender` -Steuerelement in das aktuelle Szenario:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

Führen Sie die ASP.NET-Seite in den Browser, und klicken Sie auf die Schaltfläche ""; Sie erhalten das aktuelle Datum im Format Monat-Tag-Jahr.


[![Mit einem Klick auf die Schaltfläche wird das Datum vom Server abgerufen.](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Mit einem Klick auf die Schaltfläche mit den vom Server abgerufen, das Datum ([klicken Sie hier, um das Bild in voller Größe angezeigt](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Weiter](dynamically-populating-a-control-using-javascript-code-vb.md)
