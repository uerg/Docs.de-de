---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: Erstellen eine Filmdatenbankanwendung innerhalb von 15 Minuten mit ASP.NET MVC (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther erstellt die gesamte datenbankgesteuerte ASP.NET MVC-Anwendungen von Anfang um den Vorgang abzuschließen. Dieses Tutorial ist eine hervorragende Einführung für Personen, die neue t...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: d852dd2797f6df40cd233759648ec442259d4d26
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831199"
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>Erstellen einer Filmdatenbankanwendung innerhalb von 15 Minuten mit ASP.NET MVC (VB)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

[Code herunterladen](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> Stephen Walther erstellt die gesamte datenbankgesteuerte ASP.NET MVC-Anwendungen von Anfang um den Vorgang abzuschließen. Dieses Tutorial ist eine hervorragende Einführung für Personen sind noch nicht mit der ASP.NET MVC-Framework und eine Vorstellung des Prozesses der Erstellung einer ASP.NET MVC-Anwendung abrufen möchten.


In diesem Tutorial erhalten Sie einen Überblick über die "was es heißt" dient zum Erstellen einer ASP.NET MVC-Anwendung. In diesem Tutorial übertragen ich durch die Erstellung der gesamten ASP.NET MVC-Anwendungen von Anfang um den Vorgang abzuschließen. Veranschaulicht, wie Sie eine einfache datenbankgesteuerte-Anwendung zu erstellen, die veranschaulicht, wie Sie auflisten, erstellen und Bearbeiten von Datenbank-Datensätzen können.

Um den Prozess der Entwicklung der Anwendung zu vereinfachen, werden wir die Gerüstbau-Features von Visual Studio 2008 nutzen. Wir lassen von Visual Studio, die den anfänglichen Code und Inhalt für den Controller, -Modelle und Ansichten zu generieren.

Wenn Sie für Active Server Pages oder ASP.NET gearbeitet haben, sollten finden Sie bei ASP.NET MVC sehr vertraut vorkommen. ASP.NET MVC-Ansichten sind sehr ähnlich wie die Seiten in einer Active Server Pages-Anwendung. Und genau wie eine herkömmliche ASP.NET Web Forms-Anwendung, ASP.NET MVC bietet vollen Zugriff auf den umfassenden Satz an Sprachen und Klassen, die von .NET Framework bereitgestellt.

Ich hoffe ist, dass in diesem Tutorial Sie erhalten einen Überblick darüber, wie der zum Erstellen einer ASP.NET MVC-Anwendung ähnliche und anders als die Benutzeroberfläche zum Erstellen einer Active Server Pages "oder" ASP.NET Web Forms-Anwendung verwendet wird.

## <a name="overview-of-the-movie-database-application"></a>Übersicht über die Movie-Datenbankanwendung

Da unser Ziel ist, die Dinge einfach zu halten, erstellen wir eine sehr einfache Filmdatenbank-Anwendung. Unsere einfache Filmdatenbank-Anwendung werden wir drei Dinge erreichen können:

1. Einen Satz von Film-Datenbank-Datensätzen
2. Erstellt einen neuen Film-Datenbank-Datensatz
3. Bearbeiten eines vorhandenen Datenbankdatensatzes des Films

In diesem Fall denn wir möchten die Dinge einfach zu halten, werden wir die minimale Anzahl von Funktionen von ASP.NET MVC-Framework zum Erstellen unserer Anwendung benötigt nutzen. Beispielsweise wird nicht wir testgetriebener Entwicklung nutzen.

Um die Anwendung zu erstellen, müssen wir jeden der folgenden Schritte ausführen:

1. Das ASP.NET MVC-Webanwendungsprojekt erstellen
2. Erstellen der Datenbank
3. Erstellen des Datenbankmodells
4. Erstellen des ASP.NET MVC-Controllers
5. Erstellen von ASP.NET MVC-Ansichten

## <a name="preliminaries"></a>Vorbereitungen

Sie benötigen Visual Studio 2008 oder Visual Web Developer 2008 Express, zum Erstellen einer ASP.NET MVC-Anwendung. Sie müssen auch die ASP.NET MVC-Framework herunterladen.

Wenn Sie Visual Studio 2008 nicht besitzen, können Sie eine 90-Tage-Testversion von Visual Studio 2008 von dieser Website herunterladen:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Alternativ können Sie ASP.NET MVC Anwendungen mit Visual Web Developer Express 2008 erstellen. Wenn Sie Visual Web Developer Express verwenden möchten, müssen Sie Service Pack 1 installiert haben. Sie können die Visual Web Developer-2008 Express mit Service Pack 1 von dieser Website herunterladen:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp; Displaylang = En](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Nachdem Sie Visual Studio 2008 oder Visual Web Developer 2008 installieren, müssen Sie ASP.NET MVC-Framework zu installieren. Sie können das ASP.NET MVC-Framework von folgender Website herunterladen:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> Anstelle von ASP.NET-Framework und ASP.NET MVC-Framework einzeln herunterladen, können Sie den Webplattform-Installer nutzen. Der Webplattform-Installer ist eine Anwendung, die Sie in der installierten Anwendungen problemlos verwalten kann, sind Ihre Computer:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Erstellen einer ASP.NET MVC-Webanwendungsprojekts

Wir erstellen zunächst ein neues ASP.NET MVC--Application-Webanwendungsprojekt in Visual Studio 2008. Wählen Sie die Menüoption **Datei, neues Projekt** und Sie sehen das Dialogfeld "Neues Projekt" in Abbildung 1. Wählen Sie die Visual Basic als Programmiersprache aus, und wählen Sie die Projektvorlage der ASP.NET MVC-Webanwendung. Geben Sie Ihrem Projekt den Namen MovieApp, und klicken Sie auf die Schaltfläche "OK".


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**Abbildung 01**: Dialogfeld Neues Projekt ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))


Stellen Sie sicher, dass Sie .NET Framework 3.5 in der Dropdownliste oben auf der das Dialogfeld "Neues Projekt" auswählen oder die Projektvorlage der ASP.NET MVC-Webanwendung wird nicht angezeigt.


Wenn Sie ein neues MVC-Webanwendungsprojekt erstellen, fordert Visual Studio Ihnen die Erstellung ein separaten Komponententestprojekts. Das Dialogfeld in Abbildung 2 angezeigt wird. Da wird nicht wir Tests in diesem Tutorial aufgrund unter Zeitdruck erstellen (und Ja, wir etwas dazu sind nicht lesbar können) Wählen Sie die **keine** aus, und klicken Sie auf die **OK** Schaltfläche.

> [!NOTE] 
> 
> Visual Web Developer unterstützt keine Projekte.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**Abbildung 02**: Dialogfeld für das Komponententestprojekt erstellen ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))


Eine ASP.NET MVC-Anwendung verfügt über einen standardmäßigen Satz von Ordnern: einen Ordner für Modelle, Ansichten und Controllern. Sie sehen, dass dieser Satz von Ordnern im Projektmappen-Explorer-Fenster. Wir müssen, um die Modelle, Ansichten und Controller-Ordner Dateien hinzuzufügen, um unsere Filmdatenbank-Anwendung zu erstellen.

Wenn Sie eine neue MVC-Anwendung mit Visual Studio erstellen, erhalten Sie eine Beispiel-App. Da wir von Grund auf neu starten möchten, müssen wir den Inhalt für diese Anwendung zu löschen. In diesem Fall müssen Sie eine der folgenden Datei und den folgenden Ordner löschen:

- Controllers\HomeController.vb
- Views\Home

## <a name="creating-the-database"></a>Erstellen der Datenbank

Wir müssen eine Datenbank zum Speichern von unserem Movie-Datenbank-Datensätzen zu erstellen. Glücklicherweise umfasst Visual Studio eine kostenlose Datenbank, die mit dem Namen SQL Server Express. Um die Datenbank zu erstellen, gehen Sie wie folgt vor:

1. Mit der rechten Maustaste in der App\_Ordner im Projektmappen-Explorer-Fenster, und wählen Sie die Menüoption **hinzufügen, neue Element**.
2. Wählen Sie die **Daten** Kategorie, und wählen die **SQL Server-Datenbank** Vorlage (siehe Abbildung 3).
3. Benennen Sie Ihre neue Datenbank *MoviesDB.mdf* , und klicken Sie auf die **hinzufügen** Schaltfläche.

Nachdem Sie Ihre Datenbank erstellt, Sie können eine Verbindung herstellen mit der Datenbank durch Doppelklicken auf die MoviesDB.mdf-Datei befindet sich in der App\_Datenordner. Auf die MoviesDB.mdf-Datei doppelklicken, wird das Server-Explorer-Fenster geöffnet.

> [!NOTE] 
> 
> Das Server-Explorer-Fenster wird das Datenbank-Explorer-Fenster im Fall von Visual Web Developer benannt.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**Abbildung 03**: Erstellen einer Microsoft SQL Server-Datenbank ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))


Als Nächstes müssen wir eine neue Datenbanktabelle zu erstellen. Von in Server Explorer-Fenster mit der rechten Maustaste in den Ordner "Tabellen", und wählen Sie die Menüoption **neue Tabelle hinzufügen**. Durch Auswählen dieser Menüoption wird das Datenbank-Tabellen-Designer geöffnet. Erstellen Sie die folgenden Spalten:

<a id="0.2_table01"></a>


| **Name der Spalte** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | Int | False |
| Titel | nvarchar(100) | False |
| Director | nvarchar(100) | False |
| DateReleased | DateTime | False |


Die erste Spalte, der Id-Spalte verfügt über zwei besondere Eigenschaften. Zunächst müssen Sie die Id-Spalte als primäre Schlüsselspalte zu markieren. Nachdem Sie die Id-Spalte ausgewählt haben, klicken Sie auf die **Primärschlüssel festlegen** Schaltfläche (Dies ist das Symbol, das einen Schlüssel aussieht). Zweitens müssen Sie die Id-Spalte als Identitätsspalte markieren. Klicken Sie im Fenster "Eigenschaften" Scrollen Sie im Abschnitt für die Spezifikation der Spaltenidentität, und erweitern Sie ihn. Ändern der **ist Identity** -Eigenschaft auf den Wert **Ja**. Wenn Sie fertig sind, sollte die Tabelle wie in Abbildung 4 aussehen.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**Abbildung 04**: der Filme Datenbanktabelle ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))


Der letzte Schritt ist die neue Tabelle zu speichern. Klicken Sie auf die Schaltfläche "Speichern" (das Symbol von der Diskette), und geben Sie der neuen Tabelle namens Filme.

Klicken Sie nach dem Erstellen der Tabelle einige Movie-Datensätze in der Tabelle hinzugefügt. Mit der rechten Maustaste in der Tabelle "Movies" im Server-Explorer-Fenster, und wählen Sie die Menüoption **Tabellendaten anzeigen**. Geben Sie eine Liste der bevorzugten Filme (siehe Abbildung 5).


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**Abbildung 05**: Eingabe von filmdatensätzen ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))


## <a name="creating-the-model"></a>Erstellen des Modells

Als Nächstes müssen wir eine Reihe von Klassen zum Darstellen der Datenbank zu erstellen. Wir müssen ein Datenbankmodell zu erstellen. Werfen wir nutzen Microsoft Entity Framework für die Klassen für unser Datenbankmodell automatisch zu generieren.

> [!NOTE] 
> 
> ASP.NET MVC-Framework ist nicht an das Microsoft Entity Framework gebunden. Sie können Ihre Datenbank ViewModel-Klassen erstellen, durch die Nutzung einer Vielzahl von Objekt relationale Zuordnung (oder / M) Tools, einschließlich LINQ to SQL und Subsonic NHibernate.


Um den Entity Data Model-Assistenten zu starten, gehen Sie wie folgt vor:

1. Mit der rechten Maustaste in den Ordner "Models" im Projektmappen-Explorer und Auswählen der Menüoption **hinzufügen, neue Element**.
2. Wählen Sie die **Daten** Kategorie, und wählen die **ADO.NET Entity Data Model** Vorlage.
3. Geben Sie den Namen Ihres Datenmodells *MoviesDBModel.edmx* , und klicken Sie auf die **hinzufügen** Schaltfläche.

Nachdem Sie die Schaltfläche "hinzufügen", klicken Sie auf Assistent für Entity Data Model wird angezeigt (siehe Abbildung 6). Um den Assistenten abgeschlossen haben, gehen Sie wie folgt vor:

1. In der **auswählen des Modellinhalts** Schritt wählen die **aus Datenbank generieren** Option.
2. In der **wählen Sie Ihre Datenverbindung** Schritt, verwenden Sie die *MoviesDB.mdf* Datenverbindung und dem Namen *MoviesDBEntities* für die Verbindungseinstellungen. Klicken Sie auf die **Weiter** Schaltfläche.
3. In der **Datenbankobjekte auswählen** Schritt, erweitern Sie den Knoten "Tabellen", wählen Sie die Tabelle für Filme. Geben Sie den Namespace *MovieApp.Models* , und klicken Sie auf die **Fertig stellen** Schaltfläche.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**Abbildung 06**: Generieren eines Modells mit dem Assistenten für Entity Data Model ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))


Nach Abschluss des Assistenten für Entity Data Model wird dem Entity Data Model-Designer geöffnet. Der Designer die Datenbanktabelle Filme sollte angezeigt werden (siehe Abbildung 7).


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**Abbildung 07**: das Entity Data Model Designer ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))


Wir müssen eine Änderung vornehmen, bevor der Vorgang fortgesetzt. Assistent für Entity Data generiert eine Modellklasse namens Filme, die die Tabelle der Datenbank Filme darstellt. Da wir die Filme-Klasse verwenden müssen, um einen bestimmten Film darzustellen, müssen wir den Namen der Klasse ändern *Film* anstelle von *Filme* (im singular statt im plural).

Doppelklicken Sie auf den Namen der Klasse auf der Designeroberfläche, und ändern Sie den Namen der Klasse von Filmen, Film. Nachdem Sie diese Änderung vornehmen, klicken Sie auf die **speichern** (das Symbol der Diskette) Schaltfläche, um die Movie-Klasse zu generieren.

## <a name="creating-the-aspnet-mvc-controller"></a>Erstellen die ASP.NET MVC-Controller

Der nächste Schritt ist den ASP.NET MVC-Controller zu erstellen. Ein Controller ist für die Steuerung der Interaktion eines Benutzers mit einer ASP.NET MVC-Anwendung verantwortlich.

Führen Sie folgende Schritte aus:

1. Klicken Sie im Fenster Projektmappen-Explorer mit der rechten Maustaste in den Ordner "Controllers", und wählen Sie die Menüoption **hinzufügen, Controller**.
2. Das Dialogfeld "Controller hinzufügen", geben Sie den Namen *HomeController* und aktivieren Sie das Kontrollkästchen mit der Bezeichnung **Hinzufügen von Aktionsmethoden für Create, Update und Details Szenarien** (siehe Abbildung 8).
3. Klicken Sie auf die **hinzufügen** , um den neuen Controller zu Ihrem Projekt hinzuzufügen.

Nachdem Sie diese Schritte abgeschlossen haben, wird der Controller in Codebeispiel 1 erstellt. Beachten Sie, dass sie Methoden, die mit dem Namen Index "," Details "erstellen, enthält, und bearbeiten. In den folgenden Abschnitten fügen wir den erforderlichen Code rufen Sie diese Methoden funktionieren.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**Abbildung 08**: Hinzufügen eines neuen ASP.NET MVC-Controllers ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))


**Codebeispiel 1 – Controllers\HomeController.vb**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>Auflisten von Datenbank-Datensätzen

Die Index()-Methode, der den Home-Controller ist die Standardmethode für eine ASP.NET MVC-Anwendung. Wenn Sie eine ASP.NET MVC-Anwendung ausführen, ist die Index()-Methode die erste Controllermethode, die aufgerufen wird.

Wir verwenden die Index()-Methode, um die Liste der Datensätze aus der Tabelle der Datenbank Filme anzuzeigen. Wir werden die Datenbank ViewModel-Klassen nutzen, die wir zuvor erstellt haben, um die Datenbankdatensätze Film mit der Methode Index() abzurufen.

Ich habe die HomeController-Klasse im Codebeispiel 2 geändert, sodass sie ein neues privates Feld mit dem Namen enthält \_Db. Die MoviesDBEntities-Klasse darstellt, unser Datenbankmodell, und wir verwenden diese Klasse für die Kommunikation mit der Datenbank.

Ich habe auch die Index()-Methode in Liste 2 geändert. Die Index()-Methode verwendet die MoviesDBEntities-Klasse, um alle Datensätze Film aus der Tabelle der Datenbank Filme abgerufen. Der Ausdruck  *\_Db. MovieSet.ToList()* gibt eine Liste aller Datensätze Film aus der Tabelle der Datenbank Filme zurück.

Die Liste der Filme wird an die Ansicht übergeben. Etwas, das an die View()-Methode übergeben wird ruft als Anzeigen von Daten an die Ansicht übergeben.

**Codebeispiel 2 – Controllers/HomeController.vb (geänderten Index-Methode)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

Die Index()-Methode gibt eine Ansicht namens Index zurück. Wir müssen diese Ansicht zum Anzeigen der Liste von filmdatensätzen für die Datenbank zu erstellen. Führen Sie folgende Schritte aus:


Sollten Sie Ihr Projekt erstellen (Wählen Sie die Menüoption **erstellen "," Projektmappe erstellen**) vor dem Öffnen der **Ansicht hinzufügen** Dialogfeld "oder" keine Klassen werden angezeigt, der **Datenklasse anzeigen** Dropdown-Liste.


1. Mit der rechten Maustaste der Index()-Methode im Code-Editor, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 9).
2. Klicken Sie im Dialogfeld "Ansicht hinzufügen" Stellen Sie sicher, dass das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen** aktiviert ist.
3. Von der **Inhalt anzeigen** Dropdown Liste, wählen Sie den Wert *Liste*.
4. Von der **Datenklasse anzeigen** Dropdown Liste, wählen Sie den Wert *MovieApp.Movie*.
5. Klicken Sie auf die Schaltfläche "hinzufügen" zum Erstellen des neuen anzeigen (siehe Abbildung 10).

Nachdem Sie diese Schritte abgeschlossen haben, wird der Ordner "Views\Home" eine neue Ansicht, die mit dem Namen Index.aspx hinzugefügt. Der Inhalt der Ansicht "Index" sind in Programmausdruck 3 enthalten.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**Abbildung 09**: Hinzufügen einer Ansicht in eine Controlleraktion ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**Abbildung 10**: erstellen eine neue Ansicht mit das Dialogfeld "Ansicht hinzufügen" ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))


[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

Ansicht "Index" zeigt alle Datensätze Film aus der Tabelle der Datenbank Filme innerhalb einer HTML-Tabelle. Die Sicht enthält eine For Each-Schleife, die jeder Film, dargestellt durch die Eigenschaft ViewData.Model durchläuft. Wenn Sie Ihre Anwendung ausführen, indem Sie F5 drücken, klicken Sie dann sehen Sie die Webseite in Abbildung 11.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**Abbildung 11**: der Index-Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))


## <a name="creating-new-database-records"></a>Erstellen neuer Datenbankdatensätze

Ansicht "Index", die wir im vorherigen Abschnitt erstellt haben, enthält einen Link zum Erstellen neuer Datenbankdatensätze. Wir also und implementieren Sie die Logik und die Ansicht erstellen, die zum Erstellen neuer Datenbankdatensätze in Film.

Die Home-Controller enthält zwei Methoden, die mit dem Namen Create(). Die erste Create()-Methode hat keine Parameter. Diese Überladung der Methode Create() wird verwendet, um das HTML-Formular zum Erstellen eines neuen Film-Datenbank-Datensatzes anzuzeigen.

Die zweite Create()-Methode verfügt über einen FormCollection-Parameter. Diese Überladung der Methode Create() wird immer dann aufgerufen, wenn das HTML-Formular zum Erstellen eines neuen Films an den Server zurückgesendet wird. Beachten Sie, dass diese zweite Create()-Methode ein AcceptVerbs-Attribut, das verhindert, dass die Methode aufgerufen wird, es sei denn, ein HTTP Post-Vorgang ausgeführt wird.

Diese zweite Create()-Methode wurde in der aktualisierten HomeController-Klasse in Listing 4 geändert. Die neue Version der Methode Create() einen Film-Parameter akzeptiert und enthält die Logik für einen neuen Film in der Tabelle der Datenbank Filme eingefügt.

> [!NOTE] 
> 
> Beachten Sie, dass die Bind-Attribut. Da wir nicht die Film-Id-Eigenschaft von HTML-Formular aktualisieren möchten, müssen wir diese Eigenschaft explizit ausschließen.


**Programmausdruck 4 – Controllers\HomeController.vb (geänderte Create-Methode)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Visual Studio erleichtert das zum Erstellen des Formulars zum Erstellen einer neuen filmdatenbank erfassen (siehe Abbildung 12). Führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste der Create()-Methode im Code-Editor, und wählen Sie die Menüoption **Ansicht hinzufügen**.
2. Stellen Sie sicher, dass das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen** aktiviert ist.
3. Von der **Inhalt anzeigen** Dropdown Liste, wählen Sie den Wert *erstellen*.
4. Von der **Datenklasse anzeigen** Dropdown Liste, wählen Sie den Wert *MovieApp.Movie*.
5. Klicken Sie auf die **hinzufügen** klicken, um die neue Ansicht erstellen.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**Abbildung 12**: Hinzufügen der Ansicht "erstellen" ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))


Visual Studio generiert automatisch die Ansicht in Listing 5. Diese Sicht enthält eine HTML-Formular, die Felder enthält, die die Eigenschaften der Movie-Klasse entsprechen.

**Programmausdruck 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> Das HTML-Formular generiert wird, indem Sie das Dialogfeld "Ansicht hinzufügen" wird ein Formular-Id-Feld generiert. Da die Id-Spalte eine Identitätsspalte ist, brauchen wir nicht dieses Formularfeld, und Sie können Sie problemlos entfernen.


Nachdem Sie die Erstellungsansicht hinzugefügt haben, können Sie neue Movie-Datensätze in der Datenbank hinzufügen. Führen Sie die Anwendung durch Drücken der Taste F5, und klicken Sie auf den Link, um das Formular in Abbildung 13 finden Sie unter neu erstellen. Wenn Sie abgeschlossen wurde, und senden Sie das Formular, wird ein neuer Datensatz der Movie-Datenbank erstellt.

Beachten Sie, dass Sie die formularvalidierung automatisch erhalten. Wenn Sie vernachlässigen das Veröffentlichungsdatum für einen Film eingeben, oder Sie ein ungültiges Release-Datum eingeben, klicken Sie dann das Formular wird erneut angezeigt, und das Release Date-Felds wird hervorgehoben.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**Abbildung 13**: Erstellen eines neuen Film-Datenbank-Datensatzes ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))


## <a name="editing-existing-database-records"></a>Bearbeiten vorhandene Datenbankdatensätze

In den vorherigen Abschnitten wird erläutert wie können Sie auflisten und Erstellen neuer Datenbankdatensätze. In diesem letzten Abschnitt wird beschrieben, wie Sie vorhandene Datenbankdatensätze bearbeiten können.

Zunächst müssen wir das Bearbeitungsformular zu generieren. Dieser Schritt ist einfach, da Visual Studio das Bearbeitungsformular für uns automatisch generiert. Öffnen Sie die HomeController.vb-Klasse im Visual Studio Code-Editor, und gehen Sie folgendermaßen vor:

1. Mit der rechten Maustaste der Edit()-Methode im Code-Editor, und wählen Sie die Menüoption **Ansicht hinzufügen** (siehe Abbildung 14).
2. Aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**.
3. Von der **Inhalt anzeigen** Dropdown Liste, wählen Sie den Wert *bearbeiten*.
4. Von der **Datenklasse anzeigen** Dropdown Liste, wählen Sie den Wert *MovieApp.Movie*.
5. Klicken Sie auf die **hinzufügen** klicken, um die neue Ansicht erstellen.

Diesen Schritten wird eine neue Ansicht, die mit dem Namen Edit.aspx, zu dem Ordner "Views\Home" hinzugefügt. Diese Sicht enthält eine HTML-Formular zum Bearbeiten eines Datensatzes Film.


[![Das Dialogfeld "Neues Projekt"](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**Abbildung 14**: Hinzufügen der Bearbeitungsansicht ([klicken Sie, um das Bild in voller Größe anzeigen](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))


> [!NOTE] 
> 
> Die Bearbeitungsansicht enthält ein HTML-Formular-Feld, das die Film-Id-Eigenschaft entspricht. Da Sie nicht, dass die Personen, die den Wert der Id-Eigenschaft bearbeiten möchten, sollten Sie dieses Formularfeld entfernen.


Abschließend müssen wir den Home-Controller zu ändern, damit sie unterstützt einen Datenbankdatensatz zu bearbeiten. Aktualisierte HomeController-Klasse ist im Codebeispiel 6 enthalten.

**Codebeispiel 6: Controllers\HomeController.vb (Edit-Methoden)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

Auflisten von 6 habe ich beide Überladungen der Methode Edit() zusätzlichen Logik hinzugefügt. Die erste Edit()-Methode der Film Datenbank gibt Eintrag zurück, entspricht der Id-Parameter, die an die Methode übergeben. Die zweite Überladung führt die Updates zu einem Film-Datensatz in der Datenbank.

Beachten Sie, dass rufen Sie den ursprünglichen Film, und dann ApplyPropertyChanges() rufen, um die vorhandenen Film in der Datenbank zu aktualisieren.

## <a name="summary"></a>Zusammenfassung

Der Zweck dieses Lernprogramms bestand darin, Ihnen einen Überblick über die Benutzeroberfläche zum Erstellen einer ASP.NET MVC-Anwendung geben. Ich hoffe, dass Sie ermittelt, dass das Erstellen einer ASP.NET MVC-Webanwendung auf die Benutzeroberfläche der Erstellung einer Anwendung für Active Server Pages "oder" ASP.NET sehr ähnlich ist.

In diesem Tutorial untersucht wir nur die grundlegenden Features von ASP.NET MVC-Framework. In späteren Tutorials gehen wir ausführlich darauf wie z. B. Controllern, Controlleraktionen, Ansichten, Anzeigen von Daten und HTML-Hilfsprogramme.

> [!div class="step-by-step"]
> [Vorherige](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
