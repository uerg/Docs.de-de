---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
title: Erstellen Sie eine Datenbankanwendung Film innerhalb von 15 Minuten mit ASP.NET MVC (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther erstellt eine gesamte Datenbank-driven ASP.NET MVC-Anwendung von Grund auf Fertig stellen. Dieses Lernprogramm ist eine gute Einführung für Personen, die neue t werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: dd1be137-91c5-47a8-8137-fecf0789c7f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
msc.type: authoredcontent
ms.openlocfilehash: 81e0ae42bc3e7656c933ba70920eaeeffa4c4bd6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877033"
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-c"></a>Erstellen Sie eine Datenbankanwendung Film innerhalb von 15 Minuten mit ASP.NET MVC (c#)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

[Herunterladen von Code](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_CS.zip)

> Stephen Walther erstellt eine gesamte Datenbank-driven ASP.NET MVC-Anwendung von Grund auf Fertig stellen. Dieses Lernprogramm ist eine gute Einführung für Personen, die mit dem ASP.NET MVC-Framework vertraut sind und wer einen Eindruck von den Prozess der Erstellung einer ASP.NET MVC-Anwendung abgerufen werden soll.


In diesem Lernprogramm bieten einen Eindruck von "Was sie z. B. ist" dient zum Erstellen einer ASP.NET MVC-Anwendung. In diesem Lernprogramm dargestellt ich über das Erstellen einer vollständigen ASP.NET MVC-Anwendung von Grund auf Fertig stellen. Ich zeigen, wie eine einfache Datenbank basierenden Anwendung zu erstellen, die veranschaulicht, wie Sie auflisten, erstellen und Bearbeiten von Datensätzen.

Um den Prozess der Erstellung der Anwendung zu vereinfachen, müssen wir die Gerüstbau Funktionen von Visual Studio 2008 nutzen. Visual Studio, die den anfänglichen Code und den Inhalt für unsere Controller, Modelle und Ansichten generieren informieren.

Wenn Sie mit der Active Server Pages oder ASP.NET gearbeitet haben, sollten dann Sie ASP.NET MVC vertraut gefunden werden. ASP.NET MVC-Ansichten sind sehr ähnlich wie die Seiten in einer Active Server Pages-Anwendung. Und, wie eine herkömmliche ASP.NET Web Forms-Anwendung, ASP.NET MVC bietet Ihnen vollen Zugriff auf den umfangreichen Satz von Sprachen und Klassen, die von .NET Framework bereitgestellt werden.

Ich hoffe ist, dass dieses Lernprogramm Sie einen Eindruck davon, wie der erhalten Erstellen einer ASP.NET MVC-Anwendung vergleichbar und unterscheidet die Erfahrung der Erstellung einer Active Server Pages oder ASP.NET Web Forms-Anwendung verwendet wird.

## <a name="overview-of-the-movie-database-application"></a>Übersicht über die Film-Datenbankanwendung

Da unser Ziel ist es, die Dinge einfach zu halten, müssen wir eine sehr einfache Datenbank Film-Anwendung erstellen. Unsere einfache Film-datenbankanwendung werden wir drei Aktionen ausgeführt werden können:

1. Eine Gruppe von Datenbankdatensätzen Film auflisten
2. Erstellen eines neuen Datensatzes des Film-Datenbank
3. Bearbeiten eines vorhandenen Film-Datenbank-Datensatzes

Erneut, da wir Dinge einfach zu halten möchten, müssen wir die minimale Anzahl von Funktionen von ASP.NET MVC-Framework zum Erstellen der Anwendung benötigt nutzen. Beispielsweise wird nicht wir Vorteil des Test-Driven Development weitergeleitet werden.

Um die Anwendung zu erstellen, müssen wir jeden der folgenden Schritte ausführen:

1. Das ASP.NET MVC-Webanwendungsprojekt erstellen
2. Erstellen der Datenbank
3. Erstellen Sie das Datenbankmodell
4. Erstellen von ASP.NET MVC-Controllers
5. Erstellen von ASP.NET MVC-Ansichten

## <a name="preliminaries"></a>Vorbereitende Maßnahmen

Sie benötigen Visual Studio 2008 oder Visual Web Developer 2008 Express, zum Erstellen einer ASP.NET MVC-Anwendung. Sie müssen auch zum download von ASP.NET MVC-Framework.

Wenn Sie Visual Studio 2008 nicht besitzen, können Sie eine 90-Tage-Testversion von Visual Studio 2008 dieser Website herunterladen:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Alternativ können Sie ASP.NET MVC Anwendungen mit Visual Web Developer Express 2008 erstellen. Wenn Sie Visual Web Developer Express verwenden möchten, müssen Sie Service Pack 1 installiert haben. Sie können Visual Web Developer-2008 Express mit Service Pack 1 von dieser Website herunterladen:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;Displaylang = En](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Nach der Installation von Visual Studio 2008 oder Visual Web Developer 2008 müssen Sie ASP.NET MVC-Framework zu installieren. Sie können das ASP.NET MVC-Framework von folgender Website herunterladen:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> Anstelle von ASP.NET-Framework und ASP.NET MVC-Framework einzeln herunterladen, können Sie den Webplattform-Installer nutzen. Der Webplattform-Installer ist eine Anwendung, die Ihnen ermöglicht, die die installierten Anwendungen problemlos zu verwalten sind Ihrem Computer:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Erstellen einer ASP.NET MVC-Webanwendungsprojekts

Zunächst erstellen ein neues ASP.NET MVC-Webanwendung-Projekt in Visual Studio 2008. Wählen Sie die Menüoption **Datei, neues Projekt** und sehen Sie im Dialogfeld "Neues Projekt" in Abbildung 1. Wählen Sie c# als Programmiersprache aus, und wählen Sie die Projektvorlage für ASP.NET MVC-Webanwendung. Geben Sie Ihrem Projekt den Namen MovieApp, und klicken Sie auf die Schaltfläche "OK".


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.png)

**Abbildung 01**: das neue Projekt (Dialogfeld) ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.png))


Stellen Sie sicher, dass Sie .NET Framework 3.5 in der Dropdownliste am oberen Rand mit dem Dialogfeld "Neues Projekt" auswählen oder die Projektvorlage für ASP.NET MVC-Webanwendung wird nicht angezeigt.


Wenn Sie ein neues MVC-Webanwendungsprojekt erstellen, werden Sie von Visual Studio aufgefordert, ein separates Komponententestprojekt erstellen. Das Dialogfeld in Abbildung 2 wird angezeigt. Da wir wird nicht von Tests in diesem Lernprogramm aufgrund von Einschränkungen erstellen werden (und Ja, wir zu diesem etwas schuldig können) Wählen Sie die **keine** aus, und klicken Sie auf die **OK** Schaltfläche.

> [!NOTE] 
> 
> Testprojekte unterstützt Visual Web Developer nicht.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.png)

**Abbildung 02**: das Komponententestprojekt erstellen Dialogfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.png))


ASP.NET MVC-Anwendung verfügt über einen standardmäßigen Satz von Ordnern: einen Ordner für Modelle, Ansichten und Controllern. Sie können diese standardmäßigen Satz von Ordnern im Projektmappen-Explorer-Fenster sehen. Wir müssen auf jeden Ordner Modelle, Ansichten und Controllern Dateien hinzufügen, um unsere Film-datenbankanwendung erstellen.

Wenn Sie eine neue MVC-Anwendung mit Visual Studio erstellen, erhalten Sie eine beispielanwendung aus. Da wir von Grund auf neu starten möchten, müssen wir den Inhalt für diese Anwendung zu löschen. Sie müssen die folgende Datei und den folgenden Ordner löschen:

- Controllers\HomeController.cs
- Views\Home den

## <a name="creating-the-database"></a>Erstellen der Datenbank

Wir müssen eine Datenbank zum Speichern von unseren Film Datenbankdatensätze zu erstellen. Glücklicherweise gibt es enthält Visual Studio eine kostenlose Datenbank mit dem Namen SQL Server Express. Führen Sie diese Schritte aus, um die Datenbank zu erstellen:

1. Mit der rechten Maustaste in der App\_Datenordner im Projektmappen-Explorer-Fenster, und wählen Sie die Menüoption **hinzufügen, neue Element**.
2. Wählen Sie die **Daten** Kategorie, und wählen die **SQL Server-Datenbank** Vorlage (siehe Abbildung 3).
3. Benennen Sie Ihre neue Datenbank *MoviesDB.mdf* , und klicken Sie auf die **hinzufügen** Schaltfläche.

Nachdem Sie Ihre Datenbank erstellt, Sie können eine Verbindung herstellen mit der Datenbank durch Doppelklicken auf die MoviesDB.mdf-Datei befindet sich in der App\_Datenordner. Auf die MoviesDB.mdf-Datei doppelklicken, wird das Server-Explorer-Fenster geöffnet.

> [!NOTE] 
> 
> Das Server-Explorer-Fenster wird das Datenbank-Explorer-Fenster im Fall von Visual Web Developer benannt.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.png)

**Abbildung 03**: Erstellen einer Microsoft SQL Server-Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.png))


Als Nächstes müssen wir eine neue Datenbanktabelle zu erstellen. In Server Explorer-Fenster mit der rechten Maustaste in des Tabellen-Ordners, und wählen die Option des Menüs **neue Tabelle hinzufügen**. Auswählen dieser Option wird die Datenbank-Tabellen-Designer geöffnet. Die folgenden Datenbankspalten zu erstellen:

<a id="0.1_table01"></a>


| **Spaltenname** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | Int | False |
| Titel | Nvarchar(100) | False |
| Director | Nvarchar(100) | False |
| DateReleased | DateTime | False |


Die erste Spalte, die Id-Spalte verfügt über zwei besondere Eigenschaften. Zunächst müssen Sie die ID-Spalte als Primärschlüsselspalte zu markieren. Nachdem Sie die Id-Spalte auswählen, klicken Sie auf die **Primärschlüssel festlegen** Schaltfläche (er befindet sich das Symbol, das einen Schlüssel aussieht). Zweitens müssen Sie die ID-Spalte als eine Identity-Spalte zu markieren. Klicken Sie im Fenster Spalteneigenschaften Bildlauf nach unten zur Abschnitt Identitätsspezifikation, und erweitern Sie ihn. Ändern der **ist Identity** -Eigenschaft auf den Wert **Ja**. Wenn Sie fertig sind, sollte die Tabelle wie in Abbildung 4 aussehen.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.png)

**Abbildung 04**: die Filme Datenbanktabelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.png))


Der letzte Schritt ist die neue Tabelle zu speichern. Klicken Sie auf die Schaltfläche "Speichern" (das Symbol des Diskettenlaufwerk), und geben Sie der neuen Tabelle namens Filme.

Nachdem Sie die Tabelle erstellt haben, fügen Sie einige Datensätze Film der Tabelle hinzu. Mit der rechten Maustaste in der Tabelle Filme im Server-Explorer-Fenster, und wählen Sie die Menüoption **Tabellendaten anzeigen**. Geben Sie eine Liste der bevorzugten Filme (siehe Abbildung 5).


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.png)

**Abbildung 05**: Film Datensätze eingeben ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.png))


## <a name="creating-the-model"></a>Erstellen des Modells

Als Nächstes müssen wir eine Reihe von Klassen zum Darstellen der Datenbank zu erstellen. Wir müssen zum Erstellen eines Datenbankmodells. Wir führen die Vorteile des Microsoft Entity Framework die Klassen für unsere Datenbankmodell automatisch zu generieren.

> [!NOTE] 
> 
> ASP.NET MVC-Framework ist nicht an Microsoft Entity Framework gebunden. Sie können Ihre Datenbank Modellklassen erstellen, durch die Nutzung einer Vielzahl von Objekt Relational Mapping (oder / M) einschließlich LINQ to SQL, Subsonic und NHibernate Tools.


Führen Sie folgende Schritte des Assistenten für Entity Data Model zu starten:

1. Mit der rechten Maustaste im Projektmappen-Explorer, und wählen Sie die Menüoption Ordner Models **hinzufügen, neue Element**.
2. Wählen Sie die **Daten** Kategorie, und wählen die **ADO.NET Entity Data Model** Vorlage.
3. Geben Sie den Namen Ihres Datenmodells *MoviesDBModel.edmx* , und klicken Sie auf die **hinzufügen** Schaltfläche.

Nachdem Sie die Schaltfläche "hinzufügen" klicken, wird der Assistent für Entity Data Model (siehe Abbildung 6) angezeigt. Gehen Sie zum Abschließen des Assistenten wie folgt vor:

1. In der **Modellinhalte** Schritt wählen Sie die **aus Datenbank generieren** Option.
2. In der **wählen Sie Ihre Datenverbindung** Schritt: Verwenden der *MoviesDB.mdf* Datenverbindung und den Namen *MoviesDBEntities* für die Verbindungseinstellungen. Klicken Sie auf die **Weiter** Schaltfläche.
3. In der **Datenbankobjekte auswählen** Schritt, erweitern Sie den Knoten "Tabellen", wählen Sie die Tabelle für Filme. Geben Sie den Namespace *MovieApp.Models* , und klicken Sie auf die **Fertig stellen** Schaltfläche.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.png)

**Abbildung 06**: Generieren eines Modells mit dem Assistenten für Entity Data Model ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.png))


Nach Abschluss des Assistenten für Entity Data Model wird im Entity Data Model-Designer geöffnet. Anzeigen des Designers die Datenbanktabelle Filme (siehe Abbildung 7).


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.png)

**Abbildung 07**: The Entity Data Model-Designer ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.png))


Wir müssen eine Änderung vornehmen, bevor der Vorgang fortgesetzt werden kann. Der Assistent für Entity Data generiert eine Modellklasse namens *Filme* , die die Datenbanktabelle Filme darstellt. Da wir die Filme-Klasse verwenden werden, um einen bestimmten Film darzustellen, müssen wir den Namen der Klasse zu ändern *Film* anstelle von *Filme* (singular statt im plural).

Doppelklicken Sie auf den Namen der Klasse auf der Designeroberfläche aus, und ändern Sie den Namen der Klasse von Filmen Film. Nachdem Sie diese Änderung vornehmen, klicken Sie auf die **speichern** (das Symbol für die Diskette)-Schaltfläche, um die Film-Klasse zu generieren.

## <a name="creating-the-aspnet-mvc-controller"></a>Erstellen von ASP.NET MVC-Controllers

Der nächste Schritt besteht darin die ASP.NET MVC-Controller zu erstellen. Ein Domänencontroller ist verantwortlich für die steuern, wie ein Benutzer mit einer ASP.NET MVC-Anwendung interagiert.

Führen Sie folgende Schritte aus:

1. Klicken Sie im Fenster Projektmappen-Explorer mit der rechten Maustaste in des Ordners Controller, und wählen Sie die Menüoption **hinzufügen, Controller**.
2. Geben Sie den Namen, klicken Sie im Dialogfeld "Controller hinzufügen" *HomeController* und aktivieren Sie das Kontrollkästchen mit der Bezeichnung **Hinzufügen von Aktionsmethoden für erstellen, aktualisieren und Details Szenarien** (siehe Abbildung 8).
3. Klicken Sie auf die **hinzufügen** Schaltfläche, um den neuen Domänencontroller zu Ihrem Projekt hinzuzufügen.

Nachdem Sie diese Schritte abgeschlossen haben, wird der Controller im Codebeispiel 1 erstellt. Beachten Sie, dass es sich um Methoden, die mit dem Namen Index, Details, erstellen, enthält, und bearbeiten. In den folgenden Abschnitten fügen wir den erforderlichen Code, um diese Methoden funktionieren zu erhalten.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image15.png)

**Abbildung 08**: Hinzufügen einer neuen ASP.NET MVC-Controller ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image16.png))


**1 – Controllers\HomeController. cs auflisten**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample1.cs)]

## <a name="listing-database-records"></a>Auflisten von Datenbankdatensätzen

Die Methode Index() des Home-Controllers ist die Standardmethode für eine ASP.NET MVC-Anwendung. Wenn Sie eine ASP.NET MVC-Anwendung ausführen, ist die Index()-Methode der erste Controllermethode, die aufgerufen wird.

Wir verwenden die Index()-Methode zum Anzeigen der Liste mit Datensätzen aus der Datenbanktabelle Filme. Wir müssen die Datenbank Modellklassen nutzen, die wir zuvor erstellt haben, um die Datenbankdatensätze Film mit der Methode Index() abzurufen.

Ich HomeController-Klasse in der Liste 2 verändert wurde, sodass sie ein neues privates-Feld mit dem Namen enthält \_Db. Die MoviesDBEntities-Klasse stellt unsere Datenbankmodell, und wir verwenden diese Klasse für die Kommunikation mit der Datenbank.

Ich haben auch die Index()-Methode im Codebeispiel 2 geändert. Die Index()-Methode verwendet die MoviesDBEntities-Klasse zum Abrufen aller Datensätze Film aus der Datenbanktabelle Filme. Der Ausdruck  *\_Db. MovieSet.ToList()* gibt eine Liste aller Film Datensätze aus der Datenbanktabelle Filme zurück.

Die Liste von Filmen wird an die Ansicht übergeben. Elemente, die an die View()-Methode übergeben werden ruft als Ansichtsdaten an die Ansicht übergeben.

**Auflisten von 2 – Controllers/HomeController.cs (geänderte Index-Methode)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample2.cs)]

Die Index()-Methode gibt eine Ansicht mit dem Namen Index. Wir müssen diese Ansicht zum Anzeigen der Liste von Datenbankdatensätzen Film zu erstellen. Führen Sie folgende Schritte aus:


Sollten Sie das Projekt erstellen (im Menü die Option **erstellen, Projektmappe**) vor dem Öffnen der **Ansicht hinzufügen** Dialogfeld oder keine Klassen erscheinen der **Datenklasse anzeigen** Dropdown-Liste.


1. Mit der rechten Maustaste der Index()-Methode im Code-Editor, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 9).
2. Klicken Sie im Dialogfeld "Ansicht hinzufügen" Stellen Sie sicher, dass das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen** aktiviert ist.
3. Aus der **Inhalt anzeigen** Dropdown Liste, wählen Sie den Wert *Liste*.
4. Aus der **Datenklasse anzeigen** Dropdown Liste, wählen Sie den Wert *MovieApp.Models.Movie*.
5. Klicken Sie auf die Schaltfläche "hinzufügen", um die Erstellung des neuen anzeigen (siehe Abbildung 10).

Nachdem Sie diese Schritte abgeschlossen haben, wird eine neue Sicht mit dem Namen Index.aspx Views\Home den Ordner hinzugefügt. Der Inhalt, der die Indexansicht sind in 3 auflisten enthalten.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image17.png)

**Abbildung 09**: Hinzufügen einer Ansicht aus eine Controlleraktion ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image18.png))


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image19.png)

**Abbildung 10**: eine neue Sicht erstellen und das Dialogfeld "Ansicht hinzufügen" ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image20.png))


**3 – Views\Home\Index.aspx auflisten**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample3.aspx)]

Die Indexansicht zeigt alle Film Datensätze aus der Datenbanktabelle Filmen innerhalb einer HTML-Tabelle. Die Sicht enthält eine foreach-Schleife, die jede Film, dargestellt durch die Eigenschaft ViewData.Model durchläuft. Wenn Sie Ihre Anwendung ausführen, indem Sie die F5-Taste drücken, werden Sie der Webseite in Abbildung 11 angezeigt.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image21.png)

**Abbildung 11**: die Indexansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image22.png))


## <a name="creating-new-database-records"></a>Erstellen neuer Datenbankdatensätze

Die Indexansicht, die wir im vorherigen Abschnitt erstellt enthält einen Link zum Erstellen neuer Datenbankdatensätze. Fahren Sie den fort und implementieren Sie die Logik und erstellen Sie die Sicht zum Erstellen neuer Datenbankdatensätze für Film.

Die Home-Controller enthält zwei Methoden, die mit dem Namen eine Signatures-Auflistung. Die erste Create()--Methode hat keine Parameter. Diese Überladung der Methode Create()-wird verwendet, um das HTML-Formular für das Erstellen eines neuen Datensatzes des Film-Datenbank anzuzeigen.

Die zweite Create()--Methode hat den FormCollection-Parameter. Diese Überladung der Methode Create()-wird aufgerufen, wenn das HTML-Formular für das Erstellen eines neuen Films an den Server zurückgesendet wird. Beachten Sie, dass die zweite Methode Create()-ein AcceptVerbs-Attribut verfügt, die verhindert, dass die Methode aufgerufen wird, es sei denn, ein HTTP POST-Vorgang ausgeführt wird.

Diese zweite Create()--Methode wurde in der aktualisierten HomeController-Klasse in der Liste 4 geändert. Die neue Version der Methode Create()-einen Film-Parameter akzeptiert und enthält die Logik zum Einfügen eines neuen Films in die Datenbanktabelle Filme.

> [!NOTE] 
> 
> Beachten Sie der Bind-Attribut. Da wir nicht die Film-Id-Eigenschaft von HTML-Formular aktualisieren möchten, muss diese Eigenschaft explizit ausschließen.


**Auflisten von 4 – Controllers\HomeController. cs (geänderte Create-Methode)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample4.cs)]

Visual Studio erleichtert das zum Erstellen des Formulars zum Erstellen einer neuen Film-Datenbank erfassen (siehe Abbildung 12). Führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste der Create()--Methode im Code-Editor, und wählen Sie die Menüoption **Ansicht hinzufügen**.
2. Stellen Sie sicher, dass das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen** aktiviert ist.
3. Aus der **Inhalt anzeigen** Dropdown Liste, wählen Sie den Wert *erstellen*.
4. Aus der **Datenklasse anzeigen** Dropdown Liste, wählen Sie den Wert *MovieApp.Models.Movie*.
5. Klicken Sie auf die **hinzufügen** Schaltfläche, um die neue Ansicht erstellen.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image23.png)

**Abbildung 12**: die Erstellungsansicht hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image24.png))


Visual Studio generiert automatisch die Ansicht in 5 auflisten. Diese Ansicht enthält ein HTML-Formular, die Felder enthält, die die Eigenschaften der Klasse Film entsprechen.

**5 – Views\Home\Create.aspx auflisten**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample5.aspx)]

> [!NOTE] 
> 
> Das HTML-Formular, das generiert wird, indem Sie das Dialogfeld "Ansicht hinzufügen" generiert eine Id Formularfelds. Da die Id-Spalte eine Identitätsspalte ist, dieser Formularfeld nicht erforderlich, und Sie können Sie problemlos entfernen.


Nachdem Sie die Erstellungsansicht hinzugefügt haben, können Sie neue Film-Datensätze in der Datenbank hinzufügen. Führen Sie die Anwendung durch Drücken der F5-Taste, und klicken Sie auf den Link, um das Formular in Abbildung 13 finden Sie unter neu erstellen. Wenn Sie abschließen und das Formular senden, wird ein neuer Datensatz der Film-Datenbank erstellt.

Beachten Sie, dass die formularvalidierung automatisch erhalten. Wenn kein Veröffentlichungsdatum für einen Film eingeben, oder Sie eine ungültige Veröffentlichungsdatum geben, klicken Sie dann das Formular wird erneut angezeigt, und das Datumsfeld Version wird hervorgehoben.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image25.png)

**Abbildung 13**: Erstellen eines neuen Datensatzes des Film-Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image26.png))


## <a name="editing-existing-database-records"></a>Bearbeiten die vorhandene Datenbankdatensätze

In den vorherigen Abschnitten wird erläutert wie können Sie auflisten und Erstellen neuer Datenbankdatensätze. In diesem letzten Abschnitt wird erläutert, wie Sie die vorhandene Datenbankdatensätze bearbeiten können.

Zunächst müssen wir das Formular bearbeiten zu generieren. Dieser Schritt ist einfach, da Visual Studio bearbeiten Form automatisch für uns generiert wird. Öffnen Sie die HomeController.cs-Klasse in Visual Studio Code-Editor, und gehen Sie folgendermaßen vor:

1. Mit der rechten Maustaste der Edit()-Methode im Code-Editor, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 14).
2. Aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**.
3. Aus der **Inhalt anzeigen** Dropdown Liste, wählen Sie den Wert *bearbeiten*.
4. Aus der **Datenklasse anzeigen** Dropdown Liste, wählen Sie den Wert *MovieApp.Models.Movie*.
5. Klicken Sie auf die **hinzufügen** Schaltfläche, um die neue Ansicht erstellen.

Mit diesen Schritten wird eine neue Ansicht namens Edit.aspx zum Views\Home den Ordner hinzugefügt. Diese Ansicht enthält ein HTML-Formular zum Bearbeiten von eines Film-Datensatzes.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image27.png)

**Abbildung 14**: Hinzufügen der Bearbeitungsansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image28.png))


> [!NOTE] 
> 
> Die Bearbeitungsansicht enthält ein HTML-Formularfeld, das die Film-Id-Eigenschaft entspricht. Da Sie nicht, dass die Personen, die den Wert der Id-Eigenschaft bearbeiten möchten, sollten Sie dieses Formularfeld entfernen.


Schließlich müssen wir den Home-Controller ändern, damit sie die Bearbeitung des Datensatzes in einer Datenbank unterstützt. Aktualisierte HomeController-Klasse ist im Codebeispiel 6 enthalten.

**Auflisten von 6 – Controllers\HomeController. cs (Bearbeiten-Methoden)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample6.cs)]

Auflisten von 6 habe ich beide Überladungen der Methode Edit() zusätzliche Logik hinzugefügt. Auf den Id-Parameter an die Methode übergeben, gibt die erste Edit()-Methode der Film-Datenbankeintrag, der entspricht. Die zweite Überladung führt die Updates auf einen Film-Datensatz in der Datenbank.

Beachten Sie, dass müssen Sie den ursprünglichen Film abrufen und dann ApplyPropertyChanges() rufen, um den vorhandenen Film in der Datenbank zu aktualisieren.

## <a name="summary"></a>Zusammenfassung

Der Zweck dieses Lernprogramms wurde um einen Eindruck von der Umgebung zum Erstellen einer ASP.NET MVC-Anwendung zu erteilen. Ich hoffe, dass Sie ermittelt, dass das Erstellen einer ASP.NET MVC-Webanwendung auf die Benutzeroberfläche zum Erstellen einer Anwendung Active Server Pages oder ASP.NET sehr ähnlich ist.

In diesem Lernprogramm untersucht wir nur die grundlegenden Features von ASP.NET MVC-Framework. In zukünftigen Lernprogrammen eingehendere wir in Themen wie z. B. Controller, Controlleraktionen, Ansichten, Anzeigen von Daten und HTML-Hilfsmethoden.

> [!div class="step-by-step"]
> [Nächste](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb.md)
