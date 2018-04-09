---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
title: Verwenden von TextBoxWatermark in einem FormView (c#) | Microsoft Docs
author: wenz
description: TextBoxWatermark-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird. Klickt ein Benutzer in das Feld es ich...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6ee90bf-32a5-4987-a384-15cc7dd30c8a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
msc.type: authoredcontent
ms.openlocfilehash: 90e532d33756a995a85994254c48889517c5bb8d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="using-textboxwatermark-in-a-formview-c"></a>Verwenden von TextBoxWatermark in einem FormView (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1CS.pdf)

> TextBoxWatermark-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird. Klickt ein Benutzer in das Feld, wird es geleert. Wenn der Benutzer das Feld verlässt, ohne Text eingeben, wird das vorab ausgefüllte Text erneut. Dies ist auch möglich, in einem FormView-Steuerelement.


## <a name="overview"></a>Übersicht

Die `TextBoxWatermark` Steuerelement im AJAX-Steuerelement-Toolkit erweitert ein Textfeld aus, sodass ein Text in das Feld angezeigt wird. Klickt ein Benutzer in das Feld, wird es geleert. Wenn der Benutzer das Feld verlässt, ohne Text eingeben, wird das vorab ausgefüllte Text erneut. Dies ist auch möglich, innerhalb einer `FormView` Steuerelement.

## <a name="steps"></a>Schritte

Erstens ist eine Datenquelle erforderlich. Dieses Beispiel verwendet die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition. Die Datenbank ist ein optionaler Teil einer Visual Studio-Installation (einschließlich express Edition) und steht auch als separater Download unter [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Die AdventureWorks-Datenbank ist Teil der SQL Server 2005 Samples and Sample Databases (download [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)), und fügen die `AdventureWorks.mdf` Datenbankdatei.

In diesem Beispiel gehen wir davon aus, dass die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch die Standardeinstellungen. Wenn Ihr Setup abweicht, müssen Sie passen Sie die Verbindungsinformationen für die Datenbank.

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample1.aspx)]

Fügen Sie eine Datenquelle auf der Seite unterstützt die `DELETE`, `INSERT` und `UPDATE` SQL-Anweisungen. Wenn Sie die Visual Studio-Assistent zum Erstellen der Datenquelle verwenden, beachten Sie, dass ein Fehler in der aktuellen Version nicht den Tabellennamen Präfixlänge ist (`Vendor`) mit `Purchasing`. Das folgende Markup zeigt die richtige Syntax:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample2.aspx)]

Beachten Sie den Namen (`ID`) der Datenquelle, da es in verwendet werden soll die `DataSourceID` Eigenschaft von der `FormView` Steuerelement. Die `<InsertItemTemplate>` Teil der `FormView` enthält ein Textfeld, das vom erweitert wurde die `TextBoxWatermarkExtender` Steuerelement. Stellen Sie sicher, dass der Extender innerhalb der Vorlage und nicht außerhalb davon befindet.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample3.aspx)]

Jetzt bei der Benutzer ändert sich in den Einfügemodus, der die `FormView` steuern, das Textfeld "für der neue Anbieter Dank an vorab mit Daten aufgefüllt ist die `TextBoxWatermarkExtender` Steuerelement. Ein Klick im Textfeld kann der füllzeichentext nicht mehr angezeigt.


[![Das Wasserzeichen in das Feld ergibt sich aus der extender](using-textboxwatermark-in-a-formview-cs/_static/image2.png)](using-textboxwatermark-in-a-formview-cs/_static/image1.png)

Das Wasserzeichen in das Feld ergibt sich aus der Extender ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-textboxwatermark-in-a-formview-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Nächste](using-textboxwatermark-with-validation-controls-cs.md)
