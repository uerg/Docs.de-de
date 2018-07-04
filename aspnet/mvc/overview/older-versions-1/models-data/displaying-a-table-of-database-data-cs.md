---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Anzeigen einer Tabelle von Datenbankdaten (c#) | Microsoft-Dokumentation
author: microsoft
description: In diesem Tutorial zeige ich zwei Methoden zum Anzeigen eines Satzes von Datenbank-Datensätzen. Aufgezeigt werden zwei Methoden einen Satz von Datenbank-Datensätzen in einem HTML-ta formatieren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 06dc5c9398adb45d5a5ff8f57ff42816c983ee04
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395979"
---
<a name="displaying-a-table-of-database-data-c"></a>Anzeigen einer Tabelle von Datenbankdaten (c#)
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> In diesem Tutorial zeige ich zwei Methoden zum Anzeigen eines Satzes von Datenbank-Datensätzen. Ich zeigen, zwei Methoden einen Satz von Datenbank-Datensätzen in einer HTML-Tabelle zu formatieren. Zunächst zeige ich, wie Sie Datensätze aus der Datenbank direkt in einer Ansicht formatieren können. Als Nächstes zeige ich, wie Sie von Teilansichten profitieren bei der Formatierung von Datenbank-Datensätzen.


Das Ziel in diesem Tutorial wird beschrieben, wie Sie eine HTML-Tabelle von Datenbankdaten in ASP.NET MVC-Anwendungen anzeigen können. Zunächst erfahren Sie, wie Sie mit, dass die Gerüstbau-Tools in Visual Studio enthalten um eine Sicht zu generieren, die eine Gruppe von Datensätzen automatisch anzeigt. Als Nächstes erfahren Sie, wie Sie eine partielle als Vorlage verwenden, bei der Formatierung von Datenbank-Datensätzen.

## <a name="create-the-model-classes"></a>Erstellen Sie die Modell-Klasse

Wir werden eine Reihe von Datensätzen aus der Tabelle der Datenbank Filme angezeigt. Die Filme-Datenbank-Tabelle enthält die folgenden Spalten:

<a id="0.3_table01"></a>


| **Name der Spalte** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | Int | False |
| Titel | Nvarchar(200)-Datentyp gepackt ist | False |
| Director | NVarchar(50) | False |
| DateReleased | DateTime | False |


Um die Filme-Tabelle in der ASP.NET MVC-Anwendung darstellen zu können, müssen wir eine Model-Klasse zu erstellen. In diesem Tutorial verwenden wir das Microsoft Entity Framework zum Erstellen von unserem Modellklassen aus.

> [!NOTE] 
> 
> In diesem Tutorial verwenden wir das Microsoft Entity Framework. Allerdings ist es wichtig, zu verstehen, dass Sie eine Vielzahl verschiedener Technologien für die Interaktion mit einer Datenbank aus einer ASP.NET MVC-Anwendung, einschließlich LINQ to SQL, NHibernate oder ADO.NET verwenden können.


Um den Entity Data Model-Assistenten zu starten, gehen Sie wie folgt vor:

1. Mit der rechten Maustaste in den Ordner "Models" im Projektmappen-Explorer und Auswählen der Menüoption **hinzufügen, neue Element**.
2. Wählen Sie die **Daten** Kategorie, und wählen die **ADO.NET Entity Data Model** Vorlage.
3. Geben Sie den Namen Ihres Datenmodells *MoviesDBModel.edmx* , und klicken Sie auf die **hinzufügen** Schaltfläche.

Nachdem Sie die Schaltfläche "hinzufügen", klicken Sie auf Assistent für Entity Data Model wird angezeigt (siehe Abbildung 1). Um den Assistenten abgeschlossen haben, gehen Sie wie folgt vor:

1. In der **auswählen des Modellinhalts** Schritt wählen die **aus Datenbank generieren** Option.
2. In der **wählen Sie Ihre Datenverbindung** Schritt, verwenden Sie die *MoviesDB.mdf* Datenverbindung und dem Namen *MoviesDBEntities* für die Verbindungseinstellungen. Klicken Sie auf die **Weiter** Schaltfläche.
3. In der **Datenbankobjekte auswählen** Schritt, erweitern Sie den Knoten "Tabellen", wählen Sie die Tabelle für Filme. Geben Sie den Namespace *Modelle* , und klicken Sie auf die **Fertig stellen** Schaltfläche.


[![Erstellen von LINQ to SQL-Klassen](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)

**Abbildung 01**: Erstellen von LINQ to SQL-Klassen ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image2.png))


Nach Abschluss des Assistenten für Entity Data Model wird dem Entity Data Model-Designer geöffnet. Der Designer sollte die Filme-Entität angezeigt (siehe Abbildung 2).


[![Der Entity Data Model-Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)

**Abbildung 02**: das Entity Data Model Designer ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image4.png))


Wir müssen eine Änderung vornehmen, bevor der Vorgang fortgesetzt. Assistent für Entity Data generiert eine Modellklasse namens *Filme* , die die Tabelle der Datenbank Filme darstellt. Da wir die Filme-Klasse verwenden müssen, um einen bestimmten Film darzustellen, müssen wir den Namen der Klasse ändern *Film* anstelle von *Filme* (im singular statt im plural).

Doppelklicken Sie auf den Namen der Klasse auf der Designeroberfläche, und ändern Sie den Namen der Klasse von Filmen, Film. Nachdem Sie diese Änderung vornehmen, klicken Sie auf die **speichern** (das Symbol der Diskette) Schaltfläche, um die Movie-Klasse zu generieren.

## <a name="create-the-movies-controller"></a>Erstellen des "Movies"-Controllers

Nun, da wir eine Methode zur Darstellung von unseren Unterlagen Datenbank haben, können wir einen Controller erstellen, der die Auflistung von Filmen zurückgibt. Klicken Sie im Fenster Projektmappen-Explorer von Visual Studio mit der rechten Maustaste in den Ordner "Controllers", und wählen Sie die Menüoption **hinzufügen, Controller** (siehe Abbildung 3).


[![Die Controller-Menü "hinzufügen"](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)

**Abbildung 03**: die Controller-Menü hinzufügen ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image6.png))


Wenn die **Controller hinzufügen** Dialogfeld angezeigt wird, geben Sie den Namen des Controllers MovieController (siehe Abbildung 4). Klicken Sie auf die **hinzufügen** , um den neuen Controller hinzuzufügen.


[![Das Dialogfeld "Controller hinzufügen"](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)

**Abbildung 04**: die Controller der hinzufügen-Dialogfeld ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image8.png))


Wir müssen so ändern Sie die Index()-Aktion, die von der Movie-Controller verfügbar gemacht werden, sodass sie den Satz von Datenbank-Datensätzen zurückgibt. Ändern Sie den Controller, sodass es den Controller in der Liste 1 aussieht.

**Codebeispiel 1 – Controllers\MovieController.cs**

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

In Codebeispiel 1 ist die MoviesDBEntities-Klasse verwendet, um die Datenbank MoviesDB darzustellen. Um diese Klasse verwenden zu können, müssen Sie wie folgt den MvcApplication1.Models-Namespace zu importieren:

Verwenden von MvcApplication1.Models;

Der Ausdruck *Entitäten. MovieSet.ToList()* gibt den Satz aller Filme aus der Tabelle der Datenbank Filme.

## <a name="create-the-view"></a>Erstellen Sie die Sicht

Die einfachste Möglichkeit zum Anzeigen eines Satzes von Datenbank-Datensätzen in einer HTML-Tabelle ist das Gerüst, das von Visual Studio bereitgestellten nutzen.

Erstellen Sie Ihre Anwendung durch Auswählen der Menüoption **erstellen "," Projektmappe erstellen**. Bauen Sie Ihre Anwendung vor dem Öffnen der **Ansicht hinzufügen** Dialogfeld oder die Datenklassen nicht in das Dialogfeld angezeigt.

Mit der rechten Maustaste der Index()-Aktion, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 5).


[![Hinzufügen einer Ansicht](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)

**Abbildung 05**: Hinzufügen einer Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image10.png))


In der **Ansicht hinzufügen** Dialogfeld aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**. Wählen Sie die Movie-Klasse als die **Datenklasse anzeigen**. Wählen Sie *Liste* als die **Inhalt anzeigen** (siehe Abbildung 6). Wählen diese Optionen generiert eine stark typisierte Ansicht, in dem eine Liste von Filmen angezeigt.


[![Das Dialogfeld "Ansicht hinzufügen"](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)

**Abbildung 06**: die Ansicht der hinzufügen-Dialogfeld ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image12.png))


Nachdem Sie auf die **hinzufügen** Schaltfläche, die Ansicht im Codebeispiel 2 wird automatisch generiert. Diese Ansicht enthält den Code, der zum Durchlaufen der Auflistung von Filmen und Anzeigen aller Eigenschaften eines Films erforderlich sind.

**Codebeispiel 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

Sie können die Anwendung ausführen, indem Sie durch Auswählen der Menüoption **Debuggen, Debugging starten** (oder F5 drücken). Internet Explorer wird gestartet, wenn Sie die Anwendung ausführen. Wenn Sie an die /Movie URL navigieren, und klicken Sie dann die Seite in Abbildung 7 angezeigt werden.


[![Eine Tabelle mit den Filmen](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)

**Abbildung 07**: eine Tabelle mit den Filmen ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-a-table-of-database-data-cs/_static/image14.png))


Wenn Sie etwas über das Erscheinungsbild des Rasters der Datenbank-Datensätzen in Abbildung 7 nicht zufrieden sind können Sie einfach die Ansicht "Index" ändern. Sie können z. B. Ändern der *DateReleased* Header *Veröffentlichungsdatum* durch Ändern der Ansicht "Index".

## <a name="create-a-template-with-a-partial"></a>Erstellen Sie eine Vorlage mit einer partiellen

Wenn eine Ansicht zu kompliziert wird, ist es eine gute Idee, starten Sie die Ansicht in Teilansichten aufteilen. Verwenden von Teilansichten erleichtert Ihre Ansichten verstehen und zu verwalten. Wir erstellen eine partielle, wir können jedes von der Movie-Datenbank-Datensätzen formatiert als Vorlage verwenden.

Um die partielle erstellen, gehen Sie wie folgt vor:

1. Mit der rechten Maustaste in des Ordners Views\Movie, und wählen Sie die Menüoption **Ansicht hinzufügen**.
2. Aktivieren Sie das Kontrollkästchen mit der Bezeichnung *erstellen Sie eine Teilansicht (.ascx)*.
3. Benennen Sie die partielle *MovieTemplate*.
4. Aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**.
5. Wählen Sie Film als die *Datenklasse anzeigen*.
6. Wählen Sie die leere als die *Inhalt anzeigen*.
7. Klicken Sie auf die **hinzufügen** , um die partielle zu Ihrem Projekt hinzuzufügen.

Nachdem Sie diese Schritte abgeschlossen haben, ändern Sie die partielle MovieTemplate Codebeispiel 3 aussehen.

**Codebeispiel 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

Die partielle in Programmausdruck 3 enthält eine Vorlage für eine einzelne Zeile von Datensätzen.

Geänderte Ansicht "Index" in Listing 4 wird der partiellen MovieTemplate verwendet.

**Programmausdruck 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

Die Ansicht in Listing 4 enthält eine Foreach-Schleife, die alle Filme durchläuft. Für jeden Film wird der partiellen MovieTemplate verwendet, um den Film zu formatieren. Die MovieTemplate wird durch Aufrufen der Hilfsmethode RenderPartial() gerendert.

Geänderte Ansicht "Index" rendert, die dieselbe HTML-Tabelle der Datenbank-Datensätzen. Allerdings wurde die Ansicht stark vereinfacht.


Die RenderPartial()-Methode ist anders als die meisten anderen Hilfsmethoden, da sie keine Zeichenfolge zurückgibt. Aus diesem Grund müssen Sie die Methode mit RenderPartial() aufrufen &lt;% Html.RenderPartial(); %&gt; anstelle von &lt;% = Html.RenderPartial(); %&gt;.


## <a name="summary"></a>Zusammenfassung

Das Ziel in diesem Tutorial wurde erläutert, wie Sie einen Satz von Datenbank-Datensätzen in einer HTML-Tabelle anzeigen können. Zuerst, wie Sie einen Satz von Datenbank-Datensätzen in eine Controlleraktion zurückgegeben wird, durch die Nutzung von Microsoft Entity Framework. Als Nächstes, wie Sie Visual Studio-Gerüst zu verwenden, um eine Sicht zu generieren, die eine Auflistung von Elementen automatisch anzeigt. Abschließend wie Sie die Ansicht zu vereinfachen, indem Sie eine partielle zu nutzen. Sie haben gelernt, wie eine partielle als Vorlage zu verwenden, damit Sie jeden Datensatz der Datenbank formatieren können.

> [!div class="step-by-step"]
> [Zurück](creating-model-classes-with-linq-to-sql-cs.md)
> [Weiter](performing-simple-validation-cs.md)
