---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Ein Benutzersteuerelement und JavaScript (VB) DynamicPopulate mit | Microsoft Docs
author: wenz
description: "Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 495a3917ca5fa28bc833ccc666b1a5e6db7bf7f2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2018
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a>Verwenden von DynamicPopulate mit einem Benutzersteuerelement und JavaScript (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)

> Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung. Es ist auch möglich, die mit JavaScript-Code für benutzerdefinierte clientseitige Auffüllung auslösen. Besondere Sorgfalt muss jedoch ausgeführt werden, wenn der Extender in ein benutzerdefiniertes Steuerelement befindet.


## <a name="overview"></a>Übersicht

Die `DynamicPopulate` -Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung. Es ist auch möglich, die mit JavaScript-Code für benutzerdefinierte clientseitige Auffüllung auslösen. Besondere Sorgfalt muss jedoch ausgeführt werden, wenn der Extender in ein benutzerdefiniertes Steuerelement befindet.

## <a name="steps"></a>Schritte

Erstens, Sie benötigen ein ASP.NET-Webdienst implementiert die Methode aufgerufen werden, indem Sie die `DynamicPopulateExtender` Steuerelement. Der Webdienst implementiert die Methode `getDate()` erwartet ein Argument vom Typzeichenfolge aufgerufen `contextKey`, da die `DynamicPopulate` Steuerelement sendet ein Teil der Kontextinformationen mit jedem Aufruf der Web-Dienst. Hier ist der Code (Dateien `DynamicPopulate.vb.asmx`) die ruft des aktuellen Datums in einem der drei Formate:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

Im nächsten Schritt erstellen Sie ein neues Benutzersteuerelement (`.ascx` Datei), indem Sie die folgende Deklaration in der ersten Zeile bezeichnet:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

Ein &lt; `label` &gt; Element wird verwendet, um die Anzeige der Daten vom Server stammt.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

Auch in der Benutzersteuerelementdatei verwenden wir drei Optionsfelder, die jeweils einen der drei möglichen Datumsformate, die durch den Webdienst unterstützt darstellt. Klickt der Benutzer auf einem der Optionsfelder auf, wird der Browser JavaScript-Code ausführen, die wie folgt aussieht:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

Dieser Code greift auf die `DynamicPopulateExtender` (ungewöhnliche ID keine Gedanken noch, dies wird später behandelt werden) und löst die dynamische Auffüllung mit Daten. Im Kontext des aktuellen Optionsfelds `this.value` bezieht sich auf den Wert also `format1`, `format2` oder `format3` genau wie die Webmethode erwartet.

Das einzige noch fehlt im Benutzersteuerelement ist die `DynamicPopulateExtender` steuern, welche links von der Optionsfelder an den Webdienst.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

Erneut Beachten Sie möglicherweise die ungewöhnliche-ID, die im Steuerelement verwendete: `mcd1$myDate` anstelle von `myDate`. Zuvor den JavaScript-Code verwendet `mcd1_dpe1` für den Zugriff auf die `DynamicPopulateExtender` anstelle von `dpe1`. Diese Benennungsstrategie ist eine spezielle Anforderung bei Verwendung `DynamicPopulateExtender` innerhalb eines Benutzersteuerelements. Darüber hinaus müssen Sie die Benutzer-Verwaltungsgruppen in eine bestimmte Funktion, um damit alles funktioniert eingebettet werden sollen. Erstellen Sie eine neue ASP.NET-Seite, und registrieren Sie ein Tagpräfix für das Benutzersteuerelement, das Sie soeben implementiert haben:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

Fügen Sie dann auf die ASP.NET AJAX `ScriptManager` Steuerelement auf der neuen Seite:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

Fügen Sie schließlich das Benutzersteuerelement zur Seite. Nur festgelegt werden müssen, dessen `ID` Attribut (und `runat="server"`, natürlich), haben Sie aber auch auf einen bestimmten Namen festlegen: `mcd1` , da dies das Präfix in das Benutzersteuerelement für den Zugriff darauf mithilfe von JavaScript verwendet wird.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

Und das ist schon alles! Die Seite verhält sich wie erwartet: ein Benutzer auf eines der Optionsfelder klickt, das Steuerelement im Toolkit den Webdienst aufruft, und das aktuelle Datum in das gewünschte Format angezeigt.


[![Die Optionsfelder befinden sich in einem Benutzersteuerelement](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)

Die Optionsfelder in einem Benutzersteuerelement befinden ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))

>[!div class="step-by-step"]
[Vorherige](dynamically-populating-a-control-using-javascript-code-vb.md)
