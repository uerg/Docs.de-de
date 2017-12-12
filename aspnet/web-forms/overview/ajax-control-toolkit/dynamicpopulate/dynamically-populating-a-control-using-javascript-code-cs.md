---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: "Dynamisch Auffüllen eines Steuerelements mithilfe von JavaScript-Code (c#) | Microsoft Docs"
author: wenz
description: "Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b3b23e16f2e4dd26f50eb3e07f35d078dd19a19
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a>Dynamisch Auffüllen eines Steuerelements mithilfe von JavaScript-Code (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung. Es ist auch möglich, die mit JavaScript-Code für benutzerdefinierte clientseitige Auffüllung auslösen.


## <a name="overview"></a>Übersicht

Die `DynamicPopulate` -Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung. Es ist auch möglich, die mit JavaScript-Code für benutzerdefinierte clientseitige Auffüllung auslösen.

## <a name="steps"></a>Schritte

Erstens, Sie benötigen ein ASP.NET-Webdienst implementiert die Methode aufgerufen werden, indem Sie die `DynamicPopulateExtender` Steuerelement. Der Webdienst implementiert die Methode `getDate()` erwartet ein Argument vom Typzeichenfolge aufgerufen `contextKey`, da die `DynamicPopulate` Steuerelement sendet ein Teil der Kontextinformationen mit jedem Aufruf der Web-Dienst. Hier ist der Code (Datei `DynamicPopulate.cs.asmx`) die ruft des aktuellen Datums in einem der drei Formate:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

Erstellen Sie im nächsten Schritt eine neue ASP.NET-Website und beginnen Sie mit ASP.NET AJAX ScriptManager-Steuerelement:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Anschließend fügen Sie ein Bezeichnungsfeld-Steuerelement (z. B. mithilfe des HTML-Steuerelements mit dem gleichen Namen oder die `<asp:Label />` Webserver-Steuerelement) wird die höher das Ergebnis den Aufruf des Webdiensts angezeigt.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Als Nächstes enthalten eine `DynamicPopulateExtender` steuern und Bereitstellen von Webdienstinformationen, Zielsteuerelement, aber nicht den Namen des Steuerelements, der die Auffüllung später Dies wird mit benutzerdefinierten JavaScript löst!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Jetzt auf der JavaScript-Teil. Die `$find()` Funktion, die von der ASP.NET AJAX-Bibliothek definiert gibt einen Verweis auf serverseitige Objekte des ASP.NET AJAX-Steuerelement-Toolkits, z. B. `DynamicPopulateExtender`. In der aktuellen Datei `$find("dpe")` gibt einen Verweis auf die `DynamicPopulateExtender` Steuerelement auf der Seite. Macht eine Methode namens `populate()` die löst der dynamischen Auffüllung. Die `populate()` Methode erfordert ein Argument: die Kontext-Taste das dient als Argument für die `getDate()` -Webmethode. Also z. B. `$find("dpe").populate("format1")` füllen die Bezeichnung mit dem aktuellen Datum im Format Monat-Tag-Jahr.

Um das Beispiel etwas flexibler zu machen, kann der Benutzer jetzt zwischen mehreren Datumsformate auswählen. Für jeden von ihnen wird ein Optionsfeld angezeigt. Einmal füllt der Benutzer auf ein Optionsfeld klickt, JavaScript-Code dynamisch die Bezeichnung mit dem ausgewählten Datum-Format. Hier sind die Optionsfelder aus:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Beachten Sie, dass innerhalb des Kontexts eines Optionsfelds, der JavaScript-Ausdruck `this.value` verweist auf den Wert der aktuellen Schaltfläche, die ausgeführt wird, um genau die gleichen Informationen werden die `getDate()` Methode arbeiten kann.


[![Mit einem Klick auf die Schaltfläche ruft das Datum vom Server in das angegebene Format ab.](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Mit einem Klick auf die Schaltfläche ruft das Datum ab, aus dem Server in das angegebene Format ([klicken Sie hier, um das Bild in voller Größe angezeigt](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

>[!div class="step-by-step"]
[Zurück](dynamically-populating-a-control-cs.md)
[Weiter](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
