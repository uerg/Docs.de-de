---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Verwenden eines ConfirmButton-Steuerelements In einem Wiederholungssteuerelement (VB) | Microsoft-Dokumentation
author: wenz
description: Einen ConfirmButton-Extender im AJAX Control Toolkit erstellt eine Ja/kein Popupfenster angezeigt, wenn der Benutzer auf eine Schaltfläche klickt (einschließlich LinkButton-Steuerelement). Es ist Ja nur, wenn...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 38ac40776c2fdd14af046b5e13df0701c71a4ad0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825928"
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a>Verwenden eines ConfirmButton-Steuerelements In einem Wiederholungssteuerelement (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> Einen ConfirmButton-Extender im AJAX Control Toolkit erstellt eine Ja/kein Popupfenster angezeigt, wenn der Benutzer auf eine Schaltfläche klickt (einschließlich LinkButton-Steuerelement). Nur wird Wenn Ja klicken, wird der Schaltfläche Aktion ausgeführt, andernfalls abgebrochen. Dies ist auch möglich, in einem wiederholungssteuerelement.


## <a name="overview"></a>Übersicht

Einen ConfirmButton-Extender im AJAX Control Toolkit erstellt eine Ja/kein Popupfenster angezeigt, wenn der Benutzer auf eine Schaltfläche klickt (einschließlich LinkButton-Steuerelement). Nur wird Wenn Ja klicken, wird der Schaltfläche Aktion ausgeführt, andernfalls abgebrochen. Dies ist auch möglich, in einem wiederholungssteuerelement.

## <a name="steps"></a>Schritte

Zunächst einmal ist eine Datenquelle erforderlich. Dieses Beispiel verwendet die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition. Die Datenbank ist eine optionale Komponente von einer Visual Studio-Installation (einschließlich express Edition) und steht auch als separater Download unter [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Die AdventureWorks-Datenbank ist Teil der SQL Server 2005 Samples and Sample Databases (download unter [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) und fügen Sie der `AdventureWorks.mdf` Datenbankdatei.

In diesem Beispiel wird angenommen, dass, dass Sie die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch der standardeinrichtung. Wenn Ihr Setup unterscheidet, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

Dann ist eine Datenquelle erforderlich. Der Einfachheit halber werden nur die ersten fünf Einträge in der Tabelle der AdventureWorks-Händler abgerufen. Beachten Sie, dass bei Verwendung des Visual Studio-Assistenten, um die Datenquelle, die den Namen der Tabelle zu erstellen (`Vendors`) ist derzeit nicht ordnungsgemäß mit dem Präfix `Purchasing`. Das folgende Markup ist die richtige Auswahl:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

Diese Datenquelle kann dann in einem wiederholungssteuerelement verwendet werden. Wie üblich die `DataBinder.Eval()` Methode ruft Daten aus der Datenquelle ab. Die `ConfirmButtonExtender` Steuerelement muss dann platziert werden, in der `<ItemTemplate>` Teil der Repeater, damit er für jeden Eintrag in der Datenquelle angezeigt wird.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


[![Die Schaltfläche "bestätigen" wird neben jeder Eintrag aus der Datenquelle angezeigt.](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

Die Schaltfläche "bestätigen" wird neben jeder Eintrag aus der Datenquelle angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](using-a-confirmbutton-in-a-repeater-cs.md)
