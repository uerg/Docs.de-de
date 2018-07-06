---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Datenbindung an Accordion (c#) | Microsoft-Dokumentation
author: wenz
description: Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel deklariert, w...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 05adc7158725bd5a6ba276b81222de04158d3c64
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833539"
---
<a name="databinding-to-an-accordion-c"></a>Datenbindung an Accordion (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel auf der Seite selbst deklariert, aber die Bindung an eine Datenquelle bietet mehr Flexibilität.


## <a name="overview"></a>Übersicht

Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel auf der Seite selbst deklariert, aber die Bindung an eine Datenquelle bietet mehr Flexibilität.

## <a name="steps"></a>Schritte

Zunächst einmal ist eine Datenquelle erforderlich. Dieses Beispiel verwendet die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition. Die Datenbank ist eine optionale Komponente von einer Visual Studio-Installation (einschließlich express Edition) und steht auch als separater Download unter [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Die AdventureWorks-Datenbank ist Teil der SQL Server 2005 Samples and Sample Databases (download unter [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) und fügen Sie der `AdventureWorks.mdf` Datenbankdatei.

In diesem Beispiel wird angenommen, dass, dass Sie die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch der standardeinrichtung. Wenn Ihr Setup unterscheidet, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Fügen Sie eine Datenquelle klicken Sie dann auf der Seite. Um eine begrenzte Menge an Daten zu verwenden, wählen wir nur die ersten fünf Einträge in der Vendor-Tabelle der AdventureWorks-Datenbank. Wenn Sie den Visual Studio-Assistenten zum Erstellen der Datenquelle verwenden, beachten Sie, ein Fehler in der aktuellen Version nicht den Tabellennamen als Präfix ist (`Vendor`) mit `Purchasing`. Das folgende Markup zeigt die korrekte Syntax:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Beachten Sie den Namen (ID) der Datenquelle aus. Diese sehr Identifikation muss dann verwendet werden, der `DataSourceID` -Eigenschaft des Steuerelements ' Accordion ':

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

Sie können Vorlagen für verschiedene Teile des Steuerelements, einschließlich des Headers bereitstellen, innerhalb des Steuerelements ' Accordion ' (`<HeaderTemplate>`) und den Inhalt (`<ContentTemplate>`). In der folgenden Elemente, die die Daten aus den Daten ausgeben Datenquelle, mit der `DataBinder.Eval()` Methode:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Wenn die Seite geladen wird, muss die Datenquelle an das ' Accordion ' durch den folgenden serverseitigen Code gebunden werden:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Um dieses Beispiel zu beenden, müssen Sie die beiden CSS-Klassen, auf die verwiesen wird, werden im Steuerelement ' Accordion ' definieren (in seinen Eigenschaften `HeaderCssClass` und `ContentCssClass`). Fügen Sie das folgende Markup in der `<head>` Abschnitt der Seite:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![Die Daten in der ' Accordion ' stammt direkt aus der Datenquelle](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Die Daten in der ' Accordion ' stammt direkt aus der Datenquelle ([klicken Sie, um das Bild in voller Größe anzeigen](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Nächste](dynamically-adding-an-accordion-pane-cs.md)
