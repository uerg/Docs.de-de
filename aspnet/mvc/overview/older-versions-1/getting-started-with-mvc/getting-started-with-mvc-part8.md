---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Hinzufügen einer Spalte mit dem Modell | Microsoft Docs
author: shanselman
description: Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
ms.locfileid: "30879256"
---
<a name="adding-a-column-to-the-model"></a>Hinzufügen einer Spalte mit dem Modell
====================
durch [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank. Besuchen Sie die [ASP.NET MVC-Trainingscenter](../../../index.md) andere ASP.NET-MVC Lernprogramme und Beispiele finden.


In diesem Abschnitt werden wir exemplarisch erklärt, wie wir nehmen Sie Änderungen an das Schema der unsere Datenbank und die Änderungen innerhalb der Anwendung behandeln können.

Fügen Sie einen Columnstore "Bewertung" der Film-Tabelle ein. Wechseln Sie zurück in die IDE, und klicken Sie auf die Datenbank-Explorer. Klicken Sie mit der mit der rechten Maustaste auf die Film-Tabelle aus, und wählen Sie die Tabellendefinition öffnen.

Fügen Sie eine Bewertungsspalte "" aus, wie unten dargestellt. Da wir nun alle Bewertungen haben, kann die Spalte NULL-Werte zulassen. Klicken Sie auf Speichern.

[![Filme Tabelle bearbeiten](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Klicken Sie dann zurück zum Projektmappen-Explorer, und öffnen Sie die Movies.edmx-Datei (die im Ordner "\Models"). Klicken Sie mit der rechten Maustaste auf die Entwurfsoberfläche (die weiße Fläche), und wählen Sie Updatemodell aus der Datenbank.

[![Filme – Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Hierdurch wird der Assistent"Update". Klicken Sie auf der Registerkarte "Aktualisierung" darin, und klicken Sie auf ' Fertig stellen '. Unsere Film Modellklasse wird dann mit der neuen Spalte aktualisiert werden.

![Update-Assistenten (2)](getting-started-with-mvc-part8/_static/image5.png)

Nach dem Klicken auf "Fertig stellen" können Sie sehen, dass die neue Spalte für die Bewertung für die Entität Film in unserem Modell hinzugefügt wurde.

[![Film-Entität](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Wir haben eine Spalte hinzugefügt, in das Datenbankmodell, aber die Sichten nicht kennt.

## <a name="update-views-with-model-changes"></a>Aktualisieren von Sichten mit Modelländerungen

Es gibt einige Möglichkeiten, die wir unsere Ansichtsvorlagen entsprechend die neue Spalte für die Bewertung aktualisieren konnte. Da wir diese Sichten generiert werden, über das Dialogfeld "Ansicht hinzufügen" erstellt haben, konnten wir löschen und erneut neu erstellen. Allerdings in der Regel Personen werden Änderungen an ihrer Ansichtsvorlagen aus der scaffolded anfangsgenerierung bereits vorgenommen haben und möchten hinzufügen oder Löschen von Felder manuell, genau wie mit dem ID-Feld zu erstellen.

Öffnen Sie die Vorlage \Views\Movies\Index.aspx und Hinzufügen einer &lt;ten&gt;Bewertung&lt;/th&gt; an den Anfang der Film-Tabelle. Ich hinzugefügt Meine nach "Genre". Fügen Sie Sie dann in der gleichen Position der Spalte jedoch weiter unten einer Zeile, um unsere neue Bewertung ausgegeben.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Unsere letzte Index.aspx Vorlage wird wie folgt aussehen:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Wir dann öffnen Sie die Vorlage \Views\Movies\Create.aspx und fügen Sie eine Bezeichnung und ein Textfeld für unsere neue Eigenschaft für die Bewertung:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Unsere letzte Create.aspx Vorlage wie folgt aussehen soll, und Ändern des Browsers Titel und sekundäre &lt;h2&gt; Titel, z. B. "Erstellen eines Films", während wir hier sind!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Ausführen der app, und jetzt haben Sie ein neues Feld in der Datenbank, die auf der Seite "erstellen" hinzugefügt wurde. Fügen Sie einen neuen Film - diesmal mit einer Bewertung –, und klicken Sie auf erstellen.

[![Erstellen Sie einen Film - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Nachdem Sie auf "erstellen" klicken, werden Sie auf der Seite "Index" gesendet, wo Sie neue Film mit aufgeführt ist die neue Spalte Bewertung in der Datenbank

[![Film List – Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Dieses Lernprogramm wurde Ihnen den Einstieg, Controller, Ansichten zuordnen, und übergeben hartcodierte Daten vornehmen. Dann wird erstellt, und eine Datenbank entworfen, und fügen Sie einige Daten in. Wir die Daten aus der Datenbank abgerufen und in einer HTML-Tabelle angezeigt. Klicken Sie dann hinzugefügt wird ein Erstellungsformular, mit denen den Benutzer, die Daten in die Datenbank selbst aus der Web-Anwendung hinzufügen kann. Wir Validierung hinzugefügt, und die Überprüfung auf der Clientseite JavaScript zu verwenden versucht. Schließlich die Datenbank um eine neue Spalte mit Daten enthalten geändert, anschließend führen wir unsere zwei Seiten zum Erstellen und zeigen Sie diese neuen Daten aktualisiert.

Jetzt sollten Sie auf unser Tutorial aus, der Intermediate-Stufe zu verschieben, auf "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" sowie den zahlreichen Videos und Ressourcen zur [ https://asp.net/mvc ](https://asp.net/mvc) , sogar noch stärker ASP.NET MVC vertraut zu machen!

Viel Erfolg!

- Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) und [ @shanselman ](http://twitter.com/shanselman) auf Twitter.

> [!div class="step-by-step"]
> [Vorherige](getting-started-with-mvc-part7.md)
