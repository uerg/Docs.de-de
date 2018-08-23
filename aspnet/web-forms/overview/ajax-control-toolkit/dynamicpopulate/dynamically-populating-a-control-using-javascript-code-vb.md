---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Dynamisches Auffüllen eines Steuerelements, die mit JavaScript-Code (VB) | Microsoft-Dokumentation
author: wenz
description: Das DynamicPopulate-Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement, auf t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d1c3b59896b8c509e9c62738ccd1b37c250a840
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832351"
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a>Dynamisches Auffüllen eines Steuerelements, die mit JavaScript-Code (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)

> Das DynamicPopulate-Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode) und füllt den resultierenden Wert in einem Zielsteuerelement auf der Seite, ohne eine seitenaktualisierung. Es ist auch möglich, um die Auffüllung mithilfe von benutzerdefinierten JavaScript-Code für die clientseitige auszulösen.


## <a name="overview"></a>Übersicht

Die `DynamicPopulate` -Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode), und füllt den resultierenden Wert in einem Zielsteuerelement auf der Seite, ohne eine seitenaktualisierung. Es ist auch möglich, um die Auffüllung mithilfe von benutzerdefinierten JavaScript-Code für die clientseitige auszulösen.

## <a name="steps"></a>Schritte

Zunächst einmal benötigen Sie eines ASP.NET-Webdiensts die implementiert wird, die Methode aufgerufen werden, indem Sie die `DynamicPopulateExtender` Steuerelement. Der Webdienst implementiert, die Methode `getDate()` erwartet ein Argument vom Typzeichenfolge, mit dem Namen `contextKey`, da die `DynamicPopulate` Steuerelement sendet ein Teil der Informationen zum Sitzungskontext mit jeder Aufruf des Webdiensts. Hier ist der Code (Datei `DynamicPopulate.vb.asmx`) die ruft des aktuellen Datums in der folgenden drei Formaten:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

Erstellen Sie im nächsten Schritt eine neue ASP.NET-Website aus, und beginnen Sie mit ASP.NET AJAX ScriptManager-Steuerelement:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

Fügen Sie dann ein Label-Steuerelement (z. B. über das HTML-Steuerelement mit dem gleichen Namen, oder die `<asp:Label />` Webserver-Steuerelement) zeigt die das Ergebnis des Webdienstaufrufs später noch mal.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

Als Nächstes fügen Sie eine `DynamicPopulateExtender` steuern und Bereitstellen von Informationen zu den Webdiensten, Zielsteuerelement, aber nicht den Namen des Steuerelements die löst die Auffüllung dies später ausgeführt werden wird, die mit benutzerdefinierten JavaScript-Code!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

Nun, der JavaScript-Teil. Die `$find()` Funktion, die von der ASP.NET AJAX-Bibliothek definierten gibt einen Verweis auf serverseitigen Objekten von der ASP.NET AJAX Control Toolkit, z. B. `DynamicPopulateExtender`. In der aktuellen Datei `$find("dpe")` gibt einen Verweis auf die `DynamicPopulateExtender` Steuerelement auf der Seite. Es stellt eine Methode namens `populate()` die wird ausgelöst, der dynamische Auffüllung. Die `populate()` Methode erfordert ein Argument: den Kontextschlüssel das dient als Argument für die `getDate()` Webmethode. Daher beispielsweise `$find("dpe").populate("format1")` würde die Bezeichnung mit dem aktuellen Datum im Format Monat-Tag-Jahr aufzufüllen.

Um das Beispiel etwas flexibler zu machen, kann der Benutzer nun verschiedene Datumsformate auswählen. Für jeden einzelnen wird ein Optionsfeld angezeigt. Einmal füllt der Benutzer auf ein Optionsfeld klickt, JavaScript-Code dynamisch auf die Bezeichnung mit dem ausgewählten Datum-Format. Hier sind die Optionsfelder aus:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

Beachten Sie, dass innerhalb des Kontexts eines Optionsfelds, der JavaScript-Ausdruck `this.value` verweist auf den Wert der aktuellen Schaltfläche, die genau die gleichen Informationen werden die `getDate()` Methode mit arbeiten kann.


[![Ein Klick auf die Schaltfläche mit den Ruft das Datum vom Server ab, in dem angegebenen Format ab.](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)

Ein Klick auf die Schaltfläche mit den Ruft das Datum ab, von dem Server in das angegebene Format ([klicken Sie, um das Bild in voller Größe anzeigen](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](dynamically-populating-a-control-vb.md)
> [Weiter](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)
