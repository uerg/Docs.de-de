---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Erstellen von Seitenlayouts mit Masterseiten für Ansichten (c#) | Microsoft Docs
author: microsoft
description: In diesem Lernprogramm erfahren Sie, wie ein gebräuchliches Seitenlayout für mehrere Seiten in der Anwendung zu erstellen, durch die Nutzung der Ansicht Masterseiten. Sie können ein...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 82500a311e1110713a60604027d018ba16539b65
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="creating-page-layouts-with-view-master-pages-c"></a>Erstellen von Seitenlayouts mit Masterseiten für Ansichten (c#)
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> In diesem Lernprogramm erfahren Sie, wie ein gebräuchliches Seitenlayout für mehrere Seiten in der Anwendung zu erstellen, durch die Nutzung der Ansicht Masterseiten. Sie können eine Masterseite anzeigen, z. B. verwenden, definieren eine zweispaltige Seitenlayout und verwenden das zweispalten-Layout für alle Seiten in der Webanwendung.


## <a name="creating-page-layouts-with-view-master-pages"></a>Erstellen von Seitenlayouts mit Masterseiten für Ansichten

In diesem Lernprogramm erfahren Sie, wie ein gebräuchliches Seitenlayout für mehrere Seiten in der Anwendung zu erstellen, durch die Nutzung der Ansicht Masterseiten. Sie können eine Masterseite anzeigen, z. B. verwenden, definieren eine zweispaltige Seitenlayout und verwenden das zweispalten-Layout für alle Seiten in der Webanwendung.

Sie können auch Ansicht Masterseiten Freigeben von gemeinsamen Inhalten über mehrere Seiten in Ihrer Anwendung nutzen. Beispielsweise können Sie Ihre Website-Logo, Navigationslinks und Banner Ankündigungen auf einer Masterseite Ansicht platzieren. Auf diese Weise würde jeder Seite in der Anwendung dieser Inhalt automatisch angezeigt.

In diesem Lernprogramm erfahren Sie, wie zum Erstellen einer neuen Ansicht Masterseite und erstellen eine neue Ansicht Inhaltsseite basierend auf der Masterseite.

### <a name="creating-a-view-master-page"></a>Erstellen einer Ansicht Masterseite

Beginnen Sie durch das Erstellen einer Masterseite anzeigen, die einem zweispalten Layout definiert. Sie hinzufügen eine neuen Ansicht Masterseite ein MVC-Projekt durch Rechtsklick auf den Ordner Views\Shared im Menü die Option **hinzufügen, neue Element**, und wählen die **MVC-Ansicht-Gestaltungsvorlage** Vorlage (siehe Abbildung 1).


[![Hinzufügen einer Masterseite anzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Abbildung 01**: Hinzufügen einer Masterseite Ansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))


Sie können mehr als eine Ansicht Masterseite in einer Anwendung erstellen. Jede Ansicht Masterseite kann ein anderes Seitenlayout definieren. Beispielsweise sollten Sie zu einem zweispalten-Layout bestimmte Seiten und anderen Seiten zu einem dreispalten-Layout.

Eine Masterseite Sicht sucht sehr ähnlich wie eine Standardansicht für ASP.NET MVC. Im Gegensatz zu einer normalen Ansicht eine Masterseite Ansicht enthält jedoch eine oder mehrere `<asp:ContentPlaceHolder>` Tags. Die `<contentplaceholder>` Tags werden verwendet, um die Bereiche der Masterseite zu markieren, die in einer einzelnen Inhaltsseite überschrieben werden kann.

Die Ansicht Gestaltungsvorlage in Codebeispiel 1 definiert z. B. einem zweispalten Layout. Es enthält zwei `<contentplaceholder>` Tags. Eine `<ContentPlaceHolder>` für jede Spalte.

**Auflisten von 1 – `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

Der Inhalt der Masterseite in Codebeispiel 1 enthält zwei Sicht `<div>` Transpondern, die die beiden Spalten entsprechen. Die Spalte Cascading Style Sheet-Klasse angewendet wird, sowohl `<div>` Tags. Diese Klasse wird im Stylesheet am oberen Rand der Masterseite deklariert definiert. Sie können eine Vorschau anzeigen, wie die Gestaltungsvorlage Ansicht gerendert wird, durch den Wechsel zur Entwurfsansicht. Klicken Sie auf der Registerkarte "Entwurf" auf der linken unteren Ecke des Quellcode-Editors (siehe Abbildung 2).


[![Anzeigen einer Vorschau einer Masterseite im designer](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Abbildung 02**: Anzeigen einer Vorschau einer Masterseite im Designer ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Erstellen einer Ansichtsinhaltsseite

Nach der Erstellung einer Masterseite Ansicht können Sie eine oder mehrere Ansicht Inhaltsseiten basierend auf der Masterseite Ansicht erstellen. Sie können z. B. eine Index Inhaltsseite Ansicht für den Home-Controller erstellen, indem Sie mit der rechten Maustaste in des Ordners Views\Home den auswählen **hinzufügen, neue Element**auswählen die **MVC Inhalt Ansichtsseite** Vorlage eingeben der Name Index.aspx, und klicken Sie auf die **hinzufügen** Schaltfläche (siehe Abbildung 3).


[![Hinzufügen einer Inhaltsseite anzeigen](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Abbildung 03**: Hinzufügen einer-ansichtsinhaltsseite ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))


Nachdem Sie die Schaltfläche "hinzufügen" klicken, wird ein neuer Dialog angezeigt, die es Ihnen die ermöglicht Auswahl eine Ansicht Masterseite der Inhaltsseite Sicht zugeordnet werden soll (siehe Abbildung 4). Sie können zur Masterseite Ansicht Site.master navigieren, die im vorherigen Abschnitt erstellt wurde.


[![Auswählen einer Masterseite](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Abbildung 04**: Auswählen einer Masterseite ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))


Nach der Erstellung einer neuen Ansicht Inhaltsseite basierend auf der Stammwebsite erhalten Sie die Datei im 2 aufgelistet.

**Auflisten von 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Beachten Sie, die diese Sicht enthält eine `<asp:Content>` -Tags, das auf die einzelnen entspricht der `<asp:ContentPlaceHolder>` Tags in der master-Seite anzeigen. Jede `<asp:Content>` Tag enthält eine ContentPlaceHolderID-Attribut, das auf den jeweiligen zeigt `<asp:ContentPlaceHolder>` , die sie überschreibt.

Beachten Sie außerdem, dass die Inhaltsansicht-Seite in 2 auflisten nicht der normalen öffnende und schließende HTML-Tags enthält. Angenommen, es enthält keine Start- und Endtag `<html>` oder `<head>` Tags. Alle normalen öffnenden und schließenden Tags sind in der Ansicht Masterseite enthalten.

Alle Inhalte, die in einer ansichtsinhaltsseite angezeigt werden soll, muss innerhalb von platziert werden eine `<asp:Content>` Tag. Wenn Sie die HTML-Elemente oder andere Inhalte außerhalb dieser Tags einfügen, klicken Sie dann erhalten einen Fehler Sie beim Versuch, die Seite anzuzeigen.

Sie müssen nicht außer Kraft setzen alle `<asp:ContentPlaceHolder>` Tag von einer Masterseite in eine Seite "Inhaltsansicht". Sie müssen nur überschreiben eine `<asp:ContentPlaceHolder>` kennzeichnen, wenn Sie das Tag mit bestimmten Inhalt ersetzt werden soll.

Die geänderte Indexansicht auflisten 3 enthält z. B. nur zwei `<asp:Content>` Tags. Jede der `<asp:Content>` Tags enthält Text.

**Auflisten von 3: `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Wenn die Ansicht im Codebeispiel 3 angefordert wird, rendert sie die Seite in Abbildung 5. Beachten Sie, dass die Sicht eine Seite mit zwei Spalten gerendert wird. Beachten Sie außerdem, dass der Inhalt aus der Content-Ansichtsseite mit dem Inhalt der Ansicht Masterseite zusammengeführt wird


[![Content Index Ansichtsseite](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Abbildung 05**: der Index-ansichtsinhaltsseite ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Ändern von Seiteninhalt Master anzeigen

Ein Problem, die fast auftreten ist sofort bei der Arbeit mit Masterseiten Ansicht das Problem der Ansicht Masterseite Inhalt ändern, wenn andere Ansicht Inhaltsseiten angefordert werden. Sie möchten z. B. jede Seite in der Webanwendung in einen eindeutigen Titel aufweisen. Allerdings wird der Titel in der master-Seite anzeigen und nicht in der Ansicht Inhaltsseite deklariert. So, wie Sie die Seitentitel für jede ansichtsinhaltsseite anpassen?

Es gibt zwei Möglichkeiten, die Sie, die von einer ansichtsinhaltsseite angezeigten Titel ändern können. Sie können zuerst einen Seitentitel auf das Title-Attribut des Zuweisen der `<%@ page %>` Richtlinie am oberen Rand der ansichtsinhaltsseite deklariert. Angenommen, können Sie die Indexansicht Seitentitel "Super hervorragende Website" zuweisen möchten, klicken Sie dann Sie die folgende Direktive am Anfang die Indexansicht umfassen:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Wenn die Indexansicht an den Browser gerendert wird, wird der gewünschte Titel in der Titelleiste des Browsers angezeigt:


[![Konfigurationsbrowser, Startseite](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)


Es ist eine wichtige Anforderung, die eine Masteransichtsseite, in der Reihenfolge für die Title-Attribut erfüllen muss zu arbeiten. Die Gestaltungsvorlage Sicht darf eine `<head runat="server">` Tag anstelle einer normalen `<head>` Tag für Header. Wenn die `<head>` Tag schließt nicht die Runat = "Server"-Attribut, und klicken Sie dann der Titel angezeigt werden. Die Standardansicht Masterseite enthält die erforderlichen `<head runat="server">` Tag.

Eine alternative Methode zum Ändern der Masterseite Inhalt aus einer einzelnen ansichtsinhaltsseite ist, um den Bereich zu umschließen, die Sie ändern möchten eine `<asp:ContentPlaceHolder>` Tag. Nehmen Sie z. B., dass Sie nicht nur den Titel, sondern auch die Meta-Tags gerendert werden, indem Sie eine Masteransichtsseite ändern möchten. Enthält die Masteransichtsseite 4 Auflisten einer `<asp:ContentPlaceHolder>` tag innerhalb seiner `<head>` Tag.

**Auflisten von 4 – `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Beachten Sie, dass die `<asp:ContentPlaceHolder>` Tag 4 auflisten enthält Standardinhalt: ein Standardtitel und Standard-Meta-Tags. Wenn Sie diese nicht außer Kraft setzen `<asp:ContentPlaceHolder>` in eine einzelne Ansicht Inhaltsseite zu kennzeichnen, und klicken Sie dann der Standardinhalt angezeigt wird.

Die Inhaltsansicht Seite 5 auflisten überschreibt die `<asp:ContentPlaceHolder>` Tag um einen benutzerdefinierten Titel und benutzerdefinierte Metatags anzuzeigen.

**Auflisten von 5 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm werden Sie mit eine grundlegende Einführung in Masterseiten anzeigen sowie Inhaltsseiten bereitgestellt. Sie haben gelernt, wie neue Ansicht erstellen, Masterseiten und erstellen basierende auf deren Inhalt Ansichtsseiten. Es wird untersucht, wie Sie den Inhalt einer Sicht Masterseite von einer bestimmten ansichtsinhaltsseite ändern können.

> [!div class="step-by-step"]
> [Zurück](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [Weiter](passing-data-to-view-master-pages-cs.md)
