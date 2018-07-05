---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Erstellen von Seitenlayouts mit Ansichtsmasterseiten (c#) | Microsoft-Dokumentation
author: microsoft
description: In diesem Tutorial erfahren Sie, wie Sie ein gebräuchliches Seitenlayout für mehrere Seiten in Ihrer Anwendung zu erstellen, indem Sie der Ansicht Masterseiten nutzen. Sie können ein...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: c23f397ad9dd5c26278d654ef2aec66d201166a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387847"
---
<a name="creating-page-layouts-with-view-master-pages-c"></a>Erstellen von Seitenlayouts mit Ansichtsmasterseiten (c#)
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> In diesem Tutorial erfahren Sie, wie Sie ein gebräuchliches Seitenlayout für mehrere Seiten in Ihrer Anwendung zu erstellen, indem Sie der Ansicht Masterseiten nutzen. Sie können eine Masterseite Ansicht z. B. verwenden, definieren ein Seitenlayout mit zwei Spalten und verwenden die zweispalten-Layout für alle Seiten in Ihrer Webanwendung.


## <a name="creating-page-layouts-with-view-master-pages"></a>Erstellen von Seitenlayouts mit Masterseiten für Ansichten

In diesem Tutorial erfahren Sie, wie Sie ein gebräuchliches Seitenlayout für mehrere Seiten in Ihrer Anwendung zu erstellen, indem Sie der Ansicht Masterseiten nutzen. Sie können eine Masterseite Ansicht z. B. verwenden, definieren ein Seitenlayout mit zwei Spalten und verwenden die zweispalten-Layout für alle Seiten in Ihrer Webanwendung.

Sie können auch der Ansicht Masterseiten Freigeben von gemeinsamen Inhalten über mehrere Seiten in Ihrer Anwendung nutzen. Beispielsweise können Sie Ihr Logo der Website, Navigationslinks und Banner Ankündigungen auf einer Masterseite der Ansicht platzieren. Auf diese Weise wird jeder Seite in Ihrer Anwendung diese Inhalte automatisch angezeigt.

In diesem Tutorial erfahren Sie, wie eine neue Ansicht Masterseite und erstellt eine neue Ansichtsseite basierend auf der Masterseite.

### <a name="creating-a-view-master-page"></a>Erstellen einer Ansicht-Masterseite

Wir erstellen zunächst eine Masterseite anzeigen, die einem zweispalten Layout definiert. Sie fügen eine neue Ansicht master Seite mit einer MVC-Projekt durch Rechtsklick auf den Ordner "Views\Shared", durch Auswählen der Menüoption **hinzufügen "," Neues Element**, und wählen die **MVC-Ansicht-Masterseite** Vorlage (siehe Abbildung 1).


[![Hinzufügen einer Masterseite anzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Abbildung 01**: Hinzufügen einer Ansicht Masterseite ([klicken Sie, um das Bild in voller Größe anzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))


Sie können eine Masterseite mit mehr als eine Ansicht in einer Anwendung erstellen. Jede Ansicht Masterseite kann eine andere Seitenlayout definieren. Beispielsweise sollten Sie bestimmte Seiten in einem zweispalten Layout haben und andere Seiten um eine dreispalten-Layout zu erhalten.

Eine Masterseite Ansicht entspricht weitgehend einer standardmäßigen ASP.NET MVC-Ansicht. Im Gegensatz zu einer normalen Ansicht, eine Masterseite für die Ansicht enthält jedoch mindestens eine `<asp:ContentPlaceHolder>` Tags. Die `<contentplaceholder>` werden verwendet, um die Bereiche der Masterseite zu kennzeichnen, die in einer einzelnen Inhaltsseite überschrieben werden kann.

Die Ansicht Masterseite in Codebeispiel 1 wird z. B. einem zweispalten Layout definiert. Es enthält zwei `<contentplaceholder>` Tags. Eine `<ContentPlaceHolder>` für jede Spalte.

**Codebeispiel 1: `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

Der Inhalt der Masterseite in Codebeispiel 1 enthält zwei Sicht `<div>` Tags, die die beiden Spalten entsprechen. Die Cascading Style Sheet-Spaltenklasse gilt für beide `<div>` Tags. Diese Klasse wird im Stylesheet am oberen Rand der Masterseite deklariert definiert. Sie können anzeigen, wie die Masterseite für die Ansicht gerendert wird, durch den Wechsel zur Entwurfsansicht. Klicken Sie auf der Registerkarte "Entwurf", unter der unteren linken Ecke des der Quellcode-Editor (siehe Abbildung 2).


[![Anzeigen einer Vorschau eine Masterseite im designer](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Abbildung 02**: Anzeigen einer Vorschau einer Masterseite in den Designer ([klicken Sie, um das Bild in voller Größe anzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Erstellen eine Inhaltsseite anzeigen

Nach der Erstellung einer Masterseite Ansicht können Sie mindestens eine Ansicht Inhaltsseiten basierend auf der Masterseite Ansicht erstellen. Sie können z. B. eine Index der Inhaltsseite anzeigen, für den Home-Controller erstellen, indem Sie mit der rechten Maustaste in den Ordner Views\Home den Ordner auswählen **hinzufügen, neue Element**, wählen die **Inhalt von MVC-Ansichtsseite** Vorlage eingeben der Name Index.aspx, und klicken Sie auf die **hinzufügen** Schaltfläche (siehe Abbildung 3).


[![Hinzufügen einer Inhaltsseite anzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Abbildung 03**: Hinzufügen einer ansichtsinhaltsseite ([klicken Sie, um das Bild in voller Größe anzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))


Nachdem Sie auf die Schaltfläche "hinzufügen" klicken, ein neues Dialogfeld wird angezeigt, die es Ihnen die ermöglicht Auswahl eine Ansicht Masterseite der Inhaltsseite Ansicht zugeordnet werden soll (siehe Abbildung 4). Sie können auf die Ansicht Site.master-Masterseite navigieren, die wir im vorherigen Abschnitt erstellt haben.


[![Eine Masterseite auswählen](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Abbildung 04**: Auswählen einer Masterseite ([klicken Sie, um das Bild in voller Größe anzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))


Nachdem Sie eine neue Ansichtsseite basierend auf der Masterseite Site.master erstellt haben, erhalten Sie die Datei im Codebeispiel 2.

**Codebeispiel 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Beachten Sie, die diese Sicht enthält eine `<asp:Content>` Tag, das auf die einzelnen entspricht der `<asp:ContentPlaceHolder>` -Tags in der master-Seite anzeigen. Jede `<asp:Content>` Tag enthält ein ContentPlaceHolderID-Attribut, das auf den jeweiligen zeigt `<asp:ContentPlaceHolder>` , die sie überschreibt.

Beachten Sie außerdem, dass die Ansicht "Inhalt"-Seite in Liste 2 nicht der normalen öffnende und schließende HTML-Tags enthält. Angenommen, sie enthält keine den öffnenden und schließenden `<html>` oder `<head>` Tags. Alle normalen öffnenden und schließenden Tags befinden sich in der master-Seite anzeigen.

Alle Inhalte, die in einer ansichtsinhaltsseite angezeigt werden soll, muss innerhalb von platziert werden eine `<asp:Content>` Tag. Wenn Sie die HTML-Elemente oder andere Inhalte außerhalb dieser Tags setzen, klicken Sie dann erhalten einen Fehler Sie beim Versuch, die Seite anzuzeigen.

Sie müssen nicht außer Kraft setzen alle `<asp:ContentPlaceHolder>` Tag von einer Masterseite in einer Ansicht "Inhalt"-Seite. Sie müssen nur Überschreiben einer `<asp:ContentPlaceHolder>` markieren, wenn Sie das Tag mit bestimmten Inhalt zu ersetzen möchten.

Beispielsweise geänderte Ansicht "Index" in Programmausdruck 3 enthält nur zwei `<asp:Content>` Tags. Jede der `<asp:Content>` Tags enthält Text.

**Codebeispiel 3: `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Wenn die Ansicht in Programmausdruck 3 angefordert wird, rendert sie die Seite in Abbildung 5. Beachten Sie, dass die Ansicht eine Seite mit zwei Spalten gerendert wird. Beachten Sie außerdem, dass der Inhalt der Inhaltsseite anzeigen mit dem Inhalt der Ansicht-Masterseite zusammengeführt wird


[![Der Seite für den Inhalt zum Anzeigen des Index](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Abbildung 05**: der Index-ansichtsinhaltsseite ([klicken Sie, um das Bild in voller Größe anzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Ändern von Seiteninhalt Masterseiten

Ein Problem, mit denen Sie fast konfrontiert ist, sofort bei der Arbeit mit Masterseiten anzeigen, des Problems ändern Sie die Masterseite Inhalt anzeigen, wenn andere Ansicht Inhaltsseiten angefordert werden. Beispielsweise möchten Sie jede Seite in der Webanwendung für einen eindeutigen Titel aufweisen. Der Titel wird jedoch in der master-Seite anzeigen und nicht in der Ansicht Inhaltsseite deklariert. Also, wie Sie den Seitentitel für jede-ansichtsinhaltsseite anpassen?

Es gibt zwei Möglichkeiten, die Sie, den von einer ansichtsinhaltsseite angezeigten Titel ändern können. Sie können zuerst einen Titel der Seite auf das Title-Attribut des Zuweisen der `<%@ page %>` Richtlinie deklariert werden, am oberen Rand einer Inhaltsseite anzeigen. Z. B. Wenn Sie den Titel der Seite "Super großartige Website" Ansicht "Index" zuweisen möchten, können Sie die folgende Direktive am oberen Rand der Ansicht "Index" einschließen:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Wenn die Ansicht "Index" an den Browser gerendert wird, wird der gewünschte Titel in der Titelleiste des Browsers angezeigt:


[![Konfigurationsbrowser, Startseite](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)


Es ist eine wichtige Anforderung, die in der Reihenfolge für das Title-Attribut, funktioniert eine Masteransichtsseite erfüllen müssen. Die Masterseite für die Sicht darf eine `<head runat="server">` Tag anstelle einer normalen `<head>` Tag für Header. Wenn die `<head>` Tag enthält keine Runat = "Server"-Attribut aus, und klicken Sie dann der Titel nicht angezeigt werden. Die Standardansicht, die Masterseite enthält die erforderlichen `<head runat="server">` Tag.

Eine alternative Methode zum Ändern der Inhalt der Masterseite von der Inhaltsseite eine einzelne Ansicht besteht darin, die Region, die Sie ändern möchten eine `<asp:ContentPlaceHolder>` Tag. Angenommen Sie, dass Sie nicht nur den Titel, sondern auch die Meta-Tags, die durch eine Masteransichtsseite gerendert ändern möchten. Die Masteransicht-Seite in Listing 4 enthält eine `<asp:ContentPlaceHolder>` tag innerhalb der `<head>` Tag.

**Codebeispiel 4: `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Beachten Sie, dass die `<asp:ContentPlaceHolder>` Tag im Codebeispiel 4 enthält Standardinhalt: eine Standardtitel und die Standard-Meta-Tags. Wenn Sie diese nicht außer Kraft setzen `<asp:ContentPlaceHolder>` markieren Sie auf eine einzelne Ansicht Inhaltsseite, und klicken Sie dann der standardmäßigen Inhalt angezeigt wird.

Die Ansicht "Inhalt"-Seite in Listing 5 überschreibt die `<asp:ContentPlaceHolder>` Tag um einen benutzerdefinierten Titel und benutzerdefinierte Metatags für anzuzeigen.

**Codebeispiel 5: `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Zusammenfassung

In diesem Tutorial werden Sie mit einer grundlegenden Einführung in anzeigen Masterseiten und Inhaltsseiten angegeben. Sie haben gelernt, wie neue Ansicht Masterseiten und erstellt basierende auf diesen Inhalt Ansichtsseiten. Es überprüft auch, wie Sie den Inhalt einer Ansicht Masterseite von einer bestimmten ansichtsinhaltsseite ändern können.

> [!div class="step-by-step"]
> [Zurück](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [Weiter](passing-data-to-view-master-pages-cs.md)
