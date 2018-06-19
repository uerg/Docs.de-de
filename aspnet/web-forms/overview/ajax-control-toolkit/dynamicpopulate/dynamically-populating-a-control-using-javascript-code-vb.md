---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Dynamisch Auffüllen eines Steuerelements mithilfe von JavaScript-Code (VB) | Microsoft Docs
author: wenz
description: Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 04bbc6fca839c2b1ed5cafd3a4411604b98e187d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869984"
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a>Dynamisch Auffüllen eines Steuerelements mithilfe von JavaScript-Code (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)

> Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung. Es ist auch möglich, die mit JavaScript-Code für benutzerdefinierte clientseitige Auffüllung auslösen.


## <a name="overview"></a>Übersicht

Die `DynamicPopulate` -Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung. Es ist auch möglich, die mit JavaScript-Code für benutzerdefinierte clientseitige Auffüllung auslösen.

## <a name="steps"></a>Schritte

Erstens, Sie benötigen ein ASP.NET-Webdienst implementiert die Methode aufgerufen werden, indem Sie die `DynamicPopulateExtender` Steuerelement. Der Webdienst implementiert die Methode `getDate()` erwartet ein Argument vom Typzeichenfolge aufgerufen `contextKey`, da die `DynamicPopulate` Steuerelement sendet ein Teil der Kontextinformationen mit jedem Aufruf der Web-Dienst. Hier ist der Code (Datei `DynamicPopulate.vb.asmx`) die ruft des aktuellen Datums in einem der drei Formate:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

Erstellen Sie im nächsten Schritt eine neue ASP.NET-Website und beginnen Sie mit ASP.NET AJAX ScriptManager-Steuerelement:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

Anschließend fügen Sie ein Bezeichnungsfeld-Steuerelement (z. B. mithilfe des HTML-Steuerelements mit dem gleichen Namen oder die `<asp:Label />` Webserver-Steuerelement) wird die höher das Ergebnis den Aufruf des Webdiensts angezeigt.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

Als Nächstes enthalten eine `DynamicPopulateExtender` steuern und Bereitstellen von Webdienstinformationen, Zielsteuerelement, aber nicht den Namen des Steuerelements, der die Auffüllung später Dies wird mit benutzerdefinierten JavaScript löst!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

Jetzt auf der JavaScript-Teil. Die `$find()` Funktion, die von der ASP.NET AJAX-Bibliothek definiert gibt einen Verweis auf serverseitige Objekte des ASP.NET AJAX-Steuerelement-Toolkits, z. B. `DynamicPopulateExtender`. In der aktuellen Datei `$find("dpe")` gibt einen Verweis auf die `DynamicPopulateExtender` Steuerelement auf der Seite. Macht eine Methode namens `populate()` die löst der dynamischen Auffüllung. Die `populate()` Methode erfordert ein Argument: die Kontext-Taste das dient als Argument für die `getDate()` -Webmethode. Also z. B. `$find("dpe").populate("format1")` füllen die Bezeichnung mit dem aktuellen Datum im Format Monat-Tag-Jahr.

Um das Beispiel etwas flexibler zu machen, kann der Benutzer jetzt zwischen mehreren Datumsformate auswählen. Für jeden von ihnen wird ein Optionsfeld angezeigt. Einmal füllt der Benutzer auf ein Optionsfeld klickt, JavaScript-Code dynamisch die Bezeichnung mit dem ausgewählten Datum-Format. Hier sind die Optionsfelder aus:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

Beachten Sie, dass innerhalb des Kontexts eines Optionsfelds, der JavaScript-Ausdruck `this.value` verweist auf den Wert der aktuellen Schaltfläche, die ausgeführt wird, um genau die gleichen Informationen werden die `getDate()` Methode arbeiten kann.


[![Mit einem Klick auf die Schaltfläche ruft das Datum vom Server in das angegebene Format ab.](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)

Mit einem Klick auf die Schaltfläche ruft das Datum ab, aus dem Server in das angegebene Format ([klicken Sie hier, um das Bild in voller Größe angezeigt](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](dynamically-populating-a-control-vb.md)
> [Weiter](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)
