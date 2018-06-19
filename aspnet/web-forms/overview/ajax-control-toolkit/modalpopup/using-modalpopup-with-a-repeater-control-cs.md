---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: Verwenden ModalPopup mit einem Wiederholungsmodul-Steuerelement (c#) | Microsoft Docs
author: wenz
description: ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Es ist auch möglich, verwenden Sie diese Vertr....
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7124ca3fc346d78c3b235d1756695b008afb47aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872942"
---
<a name="using-modalpopup-with-a-repeater-control-c"></a>Verwenden ModalPopup mit einem Wiederholungsmodul-Steuerelement (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)

> ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Es ist auch möglich, dieses Steuerelement in ein Repeater zu verwenden.


## <a name="overview"></a>Übersicht

ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Es ist auch möglich, dieses Steuerelement in ein Repeater zu verwenden.

## <a name="steps"></a>Schritte

Erstens ist eine Datenquelle erforderlich. Dieses Beispiel verwendet die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition. Die Datenbank ist ein optionaler Teil einer Visual Studio-Installation (einschließlich express Edition) und steht auch als separater Download unter [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Die AdventureWorks-Datenbank ist Teil der SQL Server 2005 Samples and Sample Databases (download [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)), und fügen die `AdventureWorks.mdf` Datenbankdatei. In diesem Beispiel gehen wir davon aus, dass die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch die Standardeinstellungen. Wenn Ihr Setup abweicht, müssen Sie passen Sie die Verbindungsinformationen für die Datenbank. Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

Fügen Sie dann eine Datenquelle auf der Seite "ein. Um eine begrenzte Menge von Daten zu verwenden, wählen wir nur die ersten fünf Einträge in der Vendor-Tabelle der AdventureWorks-Datenbank. Wenn Sie die Visual Studio-Assistent zum Erstellen der Datenquelle verwenden, beachten Sie, dass ein Fehler in der aktuellen Version nicht den Tabellennamen Präfixlänge ist (`Vendor`) mit `Purchasing`. Das folgende Markup zeigt die richtige Syntax:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

Fügen Sie ein Panel die als modales Popupdialogfeld dient. Er enthält eine `Button` Steuerelement erneut das Popup zu schließen:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

Um das Popup in der Repeater arbeiten stellen die `ModalPopupExtender` Steuerelement gesetzt werden muss, innerhalb der `<ItemTemplate>` Abschnitt des wiederholungsmoduls. Damit im Bereich außerhalb der Repeater ist allerdings der Extender befindet. So sieht das Markup für die Repeater aus:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

Anschließend wird jedes Element in der Datenquelle mit einer Schaltfläche daneben angezeigt, die das modale Popup auslöst.


[![Modale Popupdialogfeld kann für jede Datenquelleneintrag ausgelöst werden](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)

Modale Popupdialogfeld kann für jede Datenquelleneintrag ausgelöst werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](launching-a-modal-popup-window-from-server-code-cs.md)
> [Weiter](handling-postbacks-from-a-modalpopup-cs.md)
