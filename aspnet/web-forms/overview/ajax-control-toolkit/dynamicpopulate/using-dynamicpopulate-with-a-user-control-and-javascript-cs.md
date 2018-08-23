---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Verwenden von DynamicPopulate mit einem Benutzersteuerelement und JavaScript (c#) | Microsoft-Dokumentation
author: wenz
description: Das DynamicPopulate-Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement, auf t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 110f6dd05d038438bc061d3ee907a5e2da8968c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835648"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>Verwenden von DynamicPopulate mit einem Benutzersteuerelement und JavaScript (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> Das DynamicPopulate-Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode) und füllt den resultierenden Wert in einem Zielsteuerelement auf der Seite, ohne eine seitenaktualisierung. Es ist auch möglich, um die Auffüllung mithilfe von benutzerdefinierten JavaScript-Code für die clientseitige auszulösen. Jedoch besondere Sorgfalt ausgeführt werden, wenn der Extender in einem Benutzersteuerelement befinden muss.


## <a name="overview"></a>Übersicht

Die `DynamicPopulate` -Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode), und füllt den resultierenden Wert in einem Zielsteuerelement auf der Seite, ohne eine seitenaktualisierung. Es ist auch möglich, um die Auffüllung mithilfe von benutzerdefinierten JavaScript-Code für die clientseitige auszulösen. Jedoch besondere Sorgfalt ausgeführt werden, wenn der Extender in einem Benutzersteuerelement befinden muss.

## <a name="steps"></a>Schritte

Zunächst einmal benötigen Sie eines ASP.NET-Webdiensts die implementiert wird, die Methode aufgerufen werden, indem Sie die `DynamicPopulateExtender` Steuerelement. Der Webdienst implementiert, die Methode `getDate()` erwartet ein Argument vom Typzeichenfolge, mit dem Namen `contextKey`, da die `DynamicPopulate` Steuerelement sendet ein Teil der Informationen zum Sitzungskontext mit jeder Aufruf des Webdiensts. Hier ist der Code (Datei `DynamicPopulate.cs.asmx`) die ruft des aktuellen Datums in der folgenden drei Formaten:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

Im nächsten Schritt erstellen Sie ein neues Benutzersteuerelement (`.ascx` Datei), indem Sie die folgende Deklaration in der ersten Zeile bezeichnet:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

Ein &lt; `label` &gt; Element zum Anzeigen der Daten vom Server verwendet werden.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

Auch in die Benutzersteuerelement-Datei verwenden wir drei Optionsfeldern, die jeweils einen der drei mögliche vom Webdienst unterstütztes Datumsformat darstellt. Klickt der Benutzer auf eines der Optionsfelder auf, wird der Browser JavaScript-Code ausführen, die wie folgt aussieht:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

Dieser Code greift auf die `DynamicPopulateExtender` (aber keine Sorge, über die ungewöhnliche-ID noch, dies wird später behandelt) und löst die dynamische Auffüllung mit Daten. Im Kontext des aktuellen Optionsfelds `this.value` bezieht sich auf den Wert handelt es sich `format1`, `format2` oder `format3` genau wie die Webmethode erwartet.

Ist das einzige, was fehlt im Steuerelement noch die `DynamicPopulateExtender` steuern, welche die Optionsfelder mit dem Webdienst verknüpft ist.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

Sie können erneut Notieren Sie die ungewöhnliche ID, die im Steuerelement verwendete: `mcd1$myDate` anstelle von `myDate`. Zuvor den JavaScript-Code verwendet `mcd1_dpe1` für den Zugriff auf die `DynamicPopulateExtender` anstelle von `dpe1`. Diese Benennungskonvention Strategie ist eine besondere Anforderung, bei Verwendung `DynamicPopulateExtender` in einem Benutzersteuerelement. Darüber hinaus müssen Sie die Benutzer-Verwaltungsgruppen in bestimmte Funktion, um alle dafür erforderlichen einzubetten. Erstellen Sie eine neue ASP.NET-Seite und registrieren Sie ein Tagpräfix für das Benutzersteuerelement, die, das Sie gerade implementiert haben:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

Schließen Sie dann auf der ASP.NET AJAX `ScriptManager` Steuerelement auf der neuen Seite:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

Fügen Sie abschließend das Benutzersteuerelement auf der Seite. Sie müssen nur Festlegen der `ID` Attribut (und `runat="server"`und natürlich), Sie haben jedoch auch zu einem bestimmten Namen festlegen: `mcd1` da dies das Präfix in das Benutzersteuerelement verwendet, um die Zugriffsberechtigung mithilfe von JavaScript ist.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

Und das ist schon alles! Die Seite wie erwartet verhält: ein Benutzer klickt auf eines der Optionsfelder, die das Steuerelement im Toolkit den Webdienst aufruft, und das aktuelle Datum im gewünschten Format angezeigt.


[![Die Optionsfelder befinden sich in einem Benutzersteuerelement](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

Die Optionsfelder in einem Benutzersteuerelement befinden ([klicken Sie, um das Bild in voller Größe anzeigen](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Weiter](dynamically-populating-a-control-vb.md)
