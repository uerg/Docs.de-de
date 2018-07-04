---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
title: Verwenden von TextBoxWatermark in einem FormView-Steuerelement (c#) | Microsoft-Dokumentation
author: wenz
description: Das Steuerelement "TextBoxWatermark" im AJAX Control Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird. Klickt ein Benutzer in das Feld, es ich...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6ee90bf-32a5-4987-a384-15cc7dd30c8a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
msc.type: authoredcontent
ms.openlocfilehash: 91f904637f7efd34cbe031d3f1695ba418c6932e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368459"
---
<a name="using-textboxwatermark-in-a-formview-c"></a>Verwenden von TextBoxWatermark in einem FormView-Steuerelement (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1CS.pdf)

> Das Steuerelement "TextBoxWatermark" im AJAX Control Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird. Wenn ein Benutzer in das Feld klickt, wird es geleert. Wenn der Benutzer das Feld lässt sich ohne Eingabe von Text, wird der vorab ausgefüllte Text erneut. Dies ist auch möglich, in einem FormView-Steuerelement.


## <a name="overview"></a>Übersicht

Die `TextBoxWatermark` Steuerelement im AJAX Control Toolkit erweitert ein Textfeld aus, sodass ein Text in das Feld angezeigt wird. Wenn ein Benutzer in das Feld klickt, wird es geleert. Wenn der Benutzer das Feld lässt sich ohne Eingabe von Text, wird der vorab ausgefüllte Text erneut. Dies ist auch möglich, innerhalb einer `FormView` Steuerelement.

## <a name="steps"></a>Schritte

Zunächst einmal ist eine Datenquelle erforderlich. Dieses Beispiel verwendet die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition. Die Datenbank ist eine optionale Komponente von einer Visual Studio-Installation (einschließlich express Edition) und steht auch als separater Download unter [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Die AdventureWorks-Datenbank ist Teil der SQL Server 2005 Samples and Sample Databases (download unter [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) und fügen Sie der `AdventureWorks.mdf` Datenbankdatei.

In diesem Beispiel wird angenommen, dass, dass Sie die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch der standardeinrichtung. Wenn Ihr Setup unterscheidet, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample1.aspx)]

Fügen Sie eine Datenquelle klicken Sie dann auf der Seite unterstützt das `DELETE`, `INSERT` und `UPDATE` SQL-Anweisungen. Wenn Sie den Visual Studio-Assistenten zum Erstellen der Datenquelle verwenden, beachten Sie, ein Fehler in der aktuellen Version nicht den Tabellennamen als Präfix ist (`Vendor`) mit `Purchasing`. Das folgende Markup zeigt die korrekte Syntax:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample2.aspx)]

Notieren Sie sich den Namen (`ID`) der Datenquelle, da es in verwendet werden die `DataSourceID` Eigenschaft der `FormView` Steuerelement. Die `<InsertItemTemplate>` Teil der `FormView` enthält ein Textfeld, das von erweitert wurde die `TextBoxWatermarkExtender` Steuerelement. Stellen Sie sicher, dass der Extender innerhalb der Vorlage und nicht außerhalb davon befindet.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample3.aspx)]

Jetzt bei der Benutzer ändert, in den Modus der EINFG der `FormView` zu steuern, das Textfeld für den neuen Anbieter vorausgefüllt ist, vielen Dank an die `TextBoxWatermarkExtender` Steuerelement. Ein Klicken Sie im Textfeld können den füllzeichentext nicht mehr angezeigt.


[![Das Wasserzeichen in das Feld stammt der extender](using-textboxwatermark-in-a-formview-cs/_static/image2.png)](using-textboxwatermark-in-a-formview-cs/_static/image1.png)

Das Wasserzeichen in das Feld stammt der Extender ([klicken Sie, um das Bild in voller Größe anzeigen](using-textboxwatermark-in-a-formview-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Nächste](using-textboxwatermark-with-validation-controls-cs.md)
