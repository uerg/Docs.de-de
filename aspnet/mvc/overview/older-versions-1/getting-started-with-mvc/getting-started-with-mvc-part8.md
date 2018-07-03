---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Hinzufügen einer Spalte mit dem Modell | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Erstellen Sie eine einfache Webanwendung, die aus einer Datenbank liest und schreibt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 241f9108394561fecb15db5b6efa2661fdb128d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361759"
---
<a name="adding-a-column-to-the-model"></a>Hinzufügen einer Spalte mit dem Modell
====================
durch [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Sie erstellen eine einfache Webanwendung, die aus einer Datenbank liest und schreibt. Besuchen Sie die [ASP.NET MVC-Informationscenter](../../../index.md) anderen ASP.NET MVC anhand von Tutorials und Beispiele finden.


In diesem Abschnitt werden wir erläutert, wie wir können Änderungen vornehmen, um das Schema der Datenbank, und behandeln die Änderungen in unserer Anwendung.

Fügen Sie eine Spalte "Rating", auf die Tabelle "Movie". Wechseln Sie zurück zur IDE, und klicken Sie auf die Datenbank-Explorer. Klicken Sie mit der rechten Maustaste auf die Tabelle "Movie" aus, und wählen Sie die Tabellendefinition öffnen.

Fügen Sie eine Spalte "Rating" hinzu, wie unten dargestellt. Da wir jetzt nicht über alle Bewertungen verfügen, kann die Spalte NULL-Werte zulassen. Klicken Sie auf Speichern.

[![Filme Tabelle bearbeiten](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Klicken Sie dann zurück zum Projektmappen-Explorer, und öffnen Sie die Movies.edmx-Datei (die im Ordner "\Models"). Klicken Sie mit der rechten Maustaste auf die Entwurfsoberfläche (der weiße Fläche), und wählen Sie Modell aktualisieren, aus der Datenbank.

[![Filme – Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Hierdurch wird der Assistent"Update". Klicken Sie auf der Registerkarte "Aktualisieren", darin, und klicken Sie auf "Fertig stellen". Unsere Movie-Modell-Klasse wird dann mit der neuen Spalte aktualisiert werden.

![Update-Assistenten (2)](getting-started-with-mvc-part8/_static/image5.png)

Nach dem Klicken auf "Fertig stellen", sehen Sie sich, dass die Entität "Movie" in unserem Modell der neuen Spalte für die Bewertung hinzugefügt wurde.

[![Film-Entität](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Wir haben eine Spalte hinzugefügt, in das Datenbankmodell, aber die Ansichten nicht darüber zu informieren.

## <a name="update-views-with-model-changes"></a>Aktualisieren von Sichten mit Änderungen des Datenmodells

Es gibt mehrere Möglichkeiten, die wir unsere Vorlagen anzeigen, entsprechend die neue Spalte für die Bewertung aktualisieren können. Da wir diese Sichten erstellt, indem sie über das Dialogfeld "Ansicht hinzufügen" zu generieren, können wir löschen und wieder neu erstellen. Allerdings in der Regel Personen werden Änderungen an ihrer Vorlagen anzeigen, aus der eingerüstete anfangsgenerierung bereits vorgenommen haben und hinzufügen oder Löschen von Feldern manuell, wie wir mit dem ID-Feld erstellen möchten.

Öffnen Sie die \Views\Movies\Index.aspx-Vorlage und fügen eine &lt;th&gt;Bewertung&lt;/th&gt; an den Anfang der Tabelle "Movie". Ich habe hinzugefügt mir nach "Genre". Fügen Sie dann in die gleiche Spaltenposition aber weiter unten, einer Zeile, um unsere neue Bewertung ausgegeben.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Unsere Vorlage für endgültige Index.aspx wird wie folgt aussehen:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Lassen Sie uns dann öffnen Sie die \Views\Movies\Create.aspx-Vorlage und fügen Sie eine Bezeichnung und ein Textfeld für unsere neue Rating-Eigenschaft hinzu:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Unsere Vorlage für endgültige Create.aspx wird wie folgt aussehen, und lassen Sie uns Titel und die sekundäre Datenbank des Browsers &lt;h2&gt; Titel, z. B. "Erstellen einer Film", während wir hier sind!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Führen Sie die app, und Sie haben jetzt ein neues Feld in der Datenbank, die auf der Seite "erstellen" hinzugefügt wurden, ist. Fügen Sie einen neuen Film - diesmal mit einer Bewertung –, und klicken Sie auf erstellen.

[![Erstellen Sie einen Film - Windows InternetExplorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Nachdem Sie auf "erstellen" klicken, werden Sie zur Indexseite gesendet, wo Sie mit neuen Film aufgeführt ist die neue Bewertung-Spalte in der Datenbank

[![Filmliste – Windows InternetExplorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Dieses Lernprogramm haben Sie die Schritte der Controller mit Ansichten verknüpft werden, und übergeben hartcodierte Daten vornehmen. Anschließend wurde erstellt und eine Datenbank entwickelt und einige Daten legt in. Wir die Daten aus der Datenbank abgerufen und diese in eine HTML-Tabelle angezeigt. Dann haben wir ein Erstellungsformular, mit denen der Benutzer, die Daten in der Datenbank selbst aus der Web-Anwendung hinzufügen hinzugefügt. Wir Überprüfung hinzugefügt, und klicken Sie dann die Überprüfung mithilfe von JavaScript auf der Clientseite vorgenommen. Abschließend wir die Datenbank enthält eine neue Spalte mit Daten geändert und unsere zwei Seiten zum Erstellen und zum Anzeigen dieser neuen Daten aktualisiert.

Jetzt sollten Sie in unserem Tutorial für fortgeschrittene zu verschieben, auf "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" sowie die vielen Videos und Ressourcen zu [ https://asp.net/mvc ](https://asp.net/mvc) auch weitere Informationen zur ASP.NET MVC!

Viel Erfolg!

- Scott Hanselman – [ http://hanselman.com ](http://hanselman.com) und [ @shanselman ](http://twitter.com/shanselman) auf Twitter.

> [!div class="step-by-step"]
> [Vorherige](getting-started-with-mvc-part7.md)
