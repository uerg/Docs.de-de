---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Anzeigen einer Tabelle von Datenbankdaten (VB) | Microsoft Docs
author: microsoft
description: In diesem Lernprogramm werden ich zwei Methoden zum Anzeigen von einer Gruppe von Datenbankdatensätzen gezeigt. Ich anzeigen zwei Methoden für einen Satz von Datenbank-Datensätze in einem HTML-ta Formatierung
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: dd871520f98aaae2d7b33d83b1646eb9eee88821
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="displaying-a-table-of-database-data-vb"></a>Anzeigen einer Tabelle von Datenbankdaten (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> In diesem Lernprogramm werden ich zwei Methoden zum Anzeigen von einer Gruppe von Datenbankdatensätzen gezeigt. Ich zeigen zwei Methoden zur Formatierung der eines Satz von Datenbankdatensätze in einer HTML-Tabelle. Zunächst wird aufgezeigt, wie Sie Datensätze aus der Datenbank direkt in einer Ansicht formatieren können. Als Nächstes veranschaulichen ich, wie Sie Replikatsgruppenelemente nutzen können beim Formatieren von Datenbank-Datensätze.


Ziel dieses Lernprogramms wird erläutert, wie Sie eine HTML-Tabelle der Datenbankdaten in einer ASP.NET MVC-Anwendung anzeigen können. Zunächst erfahren Sie, wie mit der Gerüstbau-Tools in Visual Studio enthalten um eine Ansicht zu generieren, in dem eine Gruppe von Datensätzen automatisch angezeigt. Als Nächstes erfahren Sie, wie eine partielle als Vorlage verwenden, bei der Formatierung von Datenbank-Datensätze.

## <a name="create-the-model-classes"></a>Erstellen Sie die Modellklassen

Wir sind den Satz von Datensätzen aus der Datenbanktabelle Filme anzeigen möchten. Die Datenbanktabelle Filme enthält die folgenden Spalten:

<a id="0.4_table01"></a>


| **Spaltenname** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | Int | False |
| Titel | Nvarchar(200)-Datentyp gepackt ist | False |
| Director | NVarchar(50) | False |
| DateReleased | DateTime | False |


Um die Filme-Tabelle in unseren ASP.NET MVC-Anwendung darstellen zu können, muss eine Modellklasse erstellt werden. In diesem Lernprogramm verwenden wir das Entity Framework von Microsoft, um unsere Modellklassen zu erstellen.

> [!NOTE] 
> 
> In diesem Lernprogramm verwenden wir die Microsoft Entity Framework. Allerdings ist es wichtig zu verstehen, dass Sie eine Vielzahl von verschiedenen Technologien für die Interaktion mit einer Datenbank von einer ASP.NET MVC-Anwendung, einschließlich LINQ to SQL, NHibernate oder ADO.NET verwenden können.


Führen Sie folgende Schritte des Assistenten für Entity Data Model zu starten:

1. Mit der rechten Maustaste im Projektmappen-Explorer, und wählen Sie die Menüoption Ordner Models **hinzufügen, neue Element**.
2. Wählen Sie die **Daten** Kategorie, und wählen die **ADO.NET Entity Data Model** Vorlage.
3. Geben Sie den Namen Ihres Datenmodells *MoviesDBModel.edmx* , und klicken Sie auf die **hinzufügen** Schaltfläche.

Nachdem Sie die Schaltfläche "hinzufügen" klicken, wird der Assistent für Entity Data Model (siehe Abbildung 1) angezeigt. Gehen Sie zum Abschließen des Assistenten wie folgt vor:

1. In der **Modellinhalte** Schritt wählen Sie die **aus Datenbank generieren** Option.
2. In der **wählen Sie Ihre Datenverbindung** Schritt: Verwenden der *MoviesDB.mdf* Datenverbindung und den Namen *MoviesDBEntities* für die Verbindungseinstellungen. Klicken Sie auf die **Weiter** Schaltfläche.
3. In der **Datenbankobjekte auswählen** Schritt, erweitern Sie den Knoten "Tabellen", wählen Sie die Tabelle für Filme. Geben Sie den Namespace *Modelle* , und klicken Sie auf die **Fertig stellen** Schaltfläche.


[![Erstellen von LINQ to SQL-Klassen](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**Abbildung 01**: Erstellen von LINQ to SQL-Klassen ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-vb/_static/image2.png))


Nach Abschluss des Assistenten für Entity Data Model wird im Entity Data Model-Designer geöffnet. Anzeigen des Designers die Filme Entität (siehe Abbildung 2).


[![Im Entity Data Model-Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**Abbildung 02**: The Entity Data Model-Designer ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-vb/_static/image4.png))


Wir müssen eine Änderung vornehmen, bevor der Vorgang fortgesetzt werden kann. Der Assistent für Entity Data generiert eine Modellklasse namens *Filme* , die die Datenbanktabelle Filme darstellt. Da wir die Filme-Klasse verwenden werden, um einen bestimmten Film darzustellen, müssen wir den Namen der Klasse zu ändern *Film* anstelle von *Filme* (singular statt im plural).

Doppelklicken Sie auf den Namen der Klasse auf der Designeroberfläche aus, und ändern Sie den Namen der Klasse von Filmen Film. Nachdem Sie diese Änderung vornehmen, klicken Sie auf die **speichern** (das Symbol für die Diskette)-Schaltfläche, um die Film-Klasse zu generieren.

## <a name="create-the-movies-controller"></a>Erstellen Sie den Controller Filme

Nun mit dem eine Möglichkeit, die registrierten Datenbank Daten darstellen, können wir einen Controller erstellen, der die Auflistung von Filmen zurückgibt. Klicken Sie im Projektmappen-Explorer von Visual Studio-Fenster mit der rechten Maustaste in des Ordners Controller, und wählen Sie die Menüoption **hinzufügen, Controller** (siehe Abbildung 3).


[![Die Controller-Menü "hinzufügen"](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**Abbildung 03**: die Controller-Menü hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-vb/_static/image6.png))


Wenn die **Controller hinzufügen** Dialogfeld angezeigt wird, geben Sie den Namen des Controllers MovieController (siehe Abbildung 4). Klicken Sie auf die **hinzufügen** Schaltfläche, um den neuen Controller hinzuzufügen.


[![Das Dialogfeld "Controller hinzufügen"](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**Abbildung 04**: der Controller der hinzufügen-Dialogfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-vb/_static/image8.png))


Wir müssen so ändern Sie die Index()-Aktion, die vom Controller Film verfügbar gemacht, sodass er den Satz von Datenbankdatensätzen zurück. Ändern Sie den Controller, sodass es den Controller im Codebeispiel 1 aussieht.

**1 – Controllers\MovieController.vb auflisten**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

Im Codebeispiel 1 dient die Klasse MoviesDBEntities MoviesDB darstellen. Der Ausdruck *Entitäten. MovieSet.ToList()* gibt den Satz aller Filme aus der Datenbanktabelle Filme.

## <a name="create-the-view"></a>Erstellen Sie die Sicht

Die einfachste Möglichkeit zum Anzeigen eines Satzes von Datenbankdatensätzen in einer HTML-Tabelle ist das Gerüst von Visual Studio bereitgestellten nutzen.

Die Anwendung erstellen, indem Sie im Menü die Option **erstellen, Projektmappe**. Bauen Sie Ihre Anwendung vor dem Öffnen der **Ansicht hinzufügen** Dialog- oder Ihre Datenklassen wird nicht im Dialogfeld angezeigt.

Mit der rechten Maustaste der Index()-Aktion, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 5).


[![Hinzufügen einer Ansicht](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**Abbildung 05**: Hinzufügen einer Ansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-vb/_static/image10.png))


In der **Ansicht hinzufügen** Dialogfeld, aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**. Wählen Sie die Film-Klasse als die **Datenklasse anzeigen**. Wählen Sie *Liste* als die **Inhalt anzeigen** (siehe Abbildung 6). Bei Auswahl dieser Optionen wird eine stark typisierte Ansicht generiert, in dem eine Liste von Filmen angezeigt.


[![Das Dialogfeld "Ansicht hinzufügen"](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**Abbildung 06**: die Ansicht der hinzufügen-Dialogfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-vb/_static/image12.png))


Nachdem Sie auf die **hinzufügen** Schaltfläche, die Ansicht im Codebeispiel 2 wird automatisch generiert. Diese Ansicht enthält den Code, der zum Durchlaufen der Auflistung von Filmen und Anzeigen der Eigenschaften eines Films erforderlich sind.

**Auflisten von 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

Sie können die Anwendung ausführen, indem Sie im Menü die Option **Debuggen, Debugging starten** (oder drücken die F5-Taste). Ausführen der Anwendung startet Internet Explorer. Wenn Sie /Movie URL navigieren, klicken Sie dann sehen die Seite in Abbildung 7 Sie.


[![Eine Tabelle von Filmen](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**Abbildung 07**: eine Tabelle von Filmen ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-a-table-of-database-data-vb/_static/image14.png))


Wenn Sie etwas über die Darstellung des Rasters der Datenbankdatensätze in Abbildung 7 nicht gefallen können Sie einfach die Indexansicht ändern. Sie können z. B. Ändern der *DateReleased* Header *Veröffentlichungsdatum* durch die Indexansicht ändern.

## <a name="create-a-template-with-a-partial"></a>Erstellen Sie eine Vorlage mit einer partiellen

Wenn eine Sicht zu kompliziert wird, ist es eine gute Idee, starten Sie die Ansicht in Replikatsgruppenelemente unterteilt. Replikatsgruppenelemente können Ihre Ansichten einfacher verstehen und zu verwalten. Wir erstellen eine partielle, die wir können als Vorlage verwenden, um die Datenbankdatensätze Film zu formatieren.

Führen Sie diese Schritte aus, um die partielle erstellen:

1. Mit der rechten Maustaste in des Ordners Views\Movie, und wählen Sie die Menüoption **Ansicht hinzufügen**.
2. Aktivieren Sie das Kontrollkästchen mit der Bezeichnung *erstellen Sie eine Teilansicht (.ascx)*.
3. Name der partiellen *MovieTemplate*.
4. Aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**.
5. Wählen Sie Film als die *Datenklasse anzeigen*.
6. Wählen Sie als leere der *Inhalt anzeigen*.
7. Klicken Sie auf die **hinzufügen** Schaltfläche, um die partielle zu Ihrem Projekt hinzuzufügen.

Nachdem Sie diese Schritte abgeschlossen haben, ändern Sie die teilweise MovieTemplate auflisten 3 aussehen.

**3 – Views\Movie\MovieTemplate.ascx auflisten**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

Der partiellen auflisten 3 enthält eine Vorlage für eine einzelne Zeile von Datensätzen.

Die geänderte Indexansicht auflisten 4 verwendet die partielle MovieTemplate.

**4 – Views\Movie\Index.aspx auflisten**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

Die Ansicht im Codebeispiel 4 enthält eine For Each-Schleife, die alle der Filme durchläuft. Für jeden Film wird die partielle MovieTemplate verwendet, um den Film formatieren. Die MovieTemplate wird durch Aufrufen der Hilfsmethode RenderPartial() gerendert.

Die geänderte Indexansicht rendert die selbe HTML-Tabelle der Datenbankdatensätze. Allerdings wurde die Sicht erheblich vereinfacht.


Die RenderPartial()-Methode ist anders als die meisten anderen Hilfsmethoden, da dies eine Zeichenfolge zurückgegeben wird. Aus diesem Grund müssen Sie die Methode mithilfe RenderPartial() aufrufen &lt;Html.RenderPartial() %&gt; anstelle von &lt;% = Html.RenderPartial() %&gt;.


## <a name="summary"></a>Zusammenfassung

Das Ziel dieses Lernprogramms wurde zur Erläuterung, wie Sie einen Satz von Datenbank-Datensätze in einer HTML-Tabelle anzeigen können. Zunächst haben Sie gelernt, wie einen Satz von Datenbank-Datensätze in eine Controlleraktion zurückgegeben, durch die Nutzung von Microsoft Entity Framework. Als Nächstes haben Sie gelernt, wie Visual Studio Gerüstbau zu verwenden, um eine Ansicht zu generieren, die eine Auflistung von Elementen werden automatisch angezeigt. Schließlich haben Sie gelernt, wie Sie die Sicht zu vereinfachen, indem Sie eine partielle nutzen. Sie haben gelernt, wie eine partielle als Vorlage verwenden, damit Sie jede Datenbankeintrag formatieren können.

> [!div class="step-by-step"]
> [Zurück](creating-model-classes-with-linq-to-sql-vb.md)
> [Weiter](performing-simple-validation-vb.md)
