---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
title: Erstellen von Modellklassen mit dem Entitätsframework (c#) | Microsoft-Dokumentation
author: microsoft
description: In diesem Tutorial erfahren Sie, wie Sie ASP.NET MVC mit dem Microsoft Entity Framework zu verwenden. Erfahren Sie, wie mit dem Assistenten für Entity ein ADO.NET Entity Da erstellen...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 61644169-e8b1-45dd-bf96-9c2301b69879
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b88aaaae21323fe3e3e8548cc04110c9caafef7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808141"
---
<a name="creating-model-classes-with-the-entity-framework-c"></a>Erstellen von Modellklassen mit dem Entitätsframework (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> In diesem Tutorial erfahren Sie, wie Sie ASP.NET MVC mit dem Microsoft Entity Framework zu verwenden. Erfahren Sie, wie der Assistent für Entity verwenden, um ein ADO.NET Entity Data Model zu erstellen. Im Verlauf dieses Tutorials erstellen wir eine Anwendung, die veranschaulicht, wie Sie auswählen, einfügen, aktualisieren und Löschen von Datenbankdaten mithilfe von Entity Framework.


Das Ziel in diesem Tutorial wird beschrieben, wie Sie Datenzugriffsklassen, die mithilfe von Microsoft Entity Framework beim Erstellen einer ASP.NET MVC-Anwendung erstellen können. In diesem Tutorial wird davon ausgegangen, keine vorherige Kenntnisse von Microsoft Entity Framework. Am Ende dieses Lernprogramms werden Sie verstehen, wie Sie verwenden das Entity Framework auswählen, einfügen, aktualisieren und Löschen von Datenbankdatensätzen.

Das Microsoft Entity Framework ist ein Objekt-relationale Zuordnung (O/RM)-Tool, das Ihnen ermöglicht, eine Datenzugriffsschicht automatisch aus einer Datenbank zu generieren. Das Entity Framework können Sie die aufwändig Ihr Datenzugriffsklassen manuell erstellen zu vermeiden.

Um zu veranschaulichen, wie Sie das Microsoft Entity Framework mit ASP.NET MVC verwenden können, erstellen wir eine einfache beispielanwendung. Wir erstellen eine Filmdatenbank-Anwendung, die Sie zum Anzeigen und Bearbeiten von filmdatensätzen-Datenbank ermöglicht.

In diesem Tutorial wird davon ausgegangen, dass Sie Visual Studio 2008 oder Visual Web Developer 2008 mit Service Pack 1 verfügen. Benötigen Sie Service Pack 1, um die Verwendung von Entity Framework. Sie können Visual Studio 2008 Service Pack 1 oder Visual Web Developer mit Service Pack 1 aus der folgenden Adresse herunterladen:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


> [!NOTE] 
> 
> Es gibt keine wesentlichen Verbindung zwischen ASP.NET MVC und das Microsoft Entity Framework. Es gibt mehrere Alternativen für das Entity Framework, die Sie mit ASP.NET MVC verwenden können. Beispielsweise können Sie Ihre MVC-Modell-Klassen, die mit anderen O/RM-Tools wie Microsoft-LINQ to SQL, NHibernate und SubSonic erstellen.


## <a name="creating-the-movie-sample-database"></a>Erstellen der Movie-Beispieldatenbank

Die Movie-datenbankanwendung verwendet eine Tabelle mit dem Namen Filme, die die folgenden Spalten enthält:

| Spaltenname | Datentyp | Zulassen NULL-Werte? | Ist der primäre Schlüssel? |
| --- | --- | --- | --- |
| Id | int | False | True |
| Titel | nvarchar(100) | False | False |
| Director | nvarchar(100) | False | False |

Sie können diese Tabelle zu einem ASP.NET MVC-Projekt hinzufügen, indem Sie folgende Schritte:

1. Mit der rechten Maustaste in der App\_Ordner im Projektmappen-Explorer-Fenster, und wählen Sie die Menüoption **hinzufügen, neue Element.**
2. Von der **neues Element hinzufügen** wählen Sie im Dialogfeld **SQL Server-Datenbank**, nennen Sie der Datenbank MoviesDB.mdf, und klicken Sie auf die **hinzufügen** Schaltfläche.
3. Doppelklicken Sie auf die MoviesDB.mdf-Datei, um das Server-Explorer/Datenbank-Explorer-Fenster zu öffnen.
4. Erweitern Sie die Verbindung mit der MoviesDB.mdf, mit der rechten Maustaste in den Ordner "Tabellen" und wählen Sie die Menüoption **neue Tabelle hinzufügen**.
5. Fügen Sie im Tabellen-Designer die Spalten-Id, Titel und Director hinzu.
6. Klicken Sie auf die **speichern** Schaltfläche (über das Symbol für die Diskette) um die neue Tabelle mit den Filmen Namen zu speichern.

Nach der Erstellung der Tabelle der Datenbank Filme sollten Sie einige Beispieldaten in die Tabelle hinzufügen. Mit der rechten Maustaste in der Tabelle "Movies", und wählen Sie die Menüoption **Tabellendaten anzeigen**. Sie können falsche Daten in das Raster eingeben, die angezeigt wird.

## <a name="creating-the-adonet-entity-data-model"></a>Erstellen das ADO.NET Entity Data Model

Um das Entity Framework zu verwenden, müssen Sie ein Entity Data Model zu erstellen. Profitieren Sie von Visual Studio *Entity Data Model-Assistenten* eines Entity Data Model aus einer Datenbank automatisch zu generieren.

Führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste im Projektmappen-Explorer den Ordner "Models", und wählen Sie die Menüoption **hinzufügen, neue Element**.
2. In der **neues Element hinzufügen** Dialogfeld Wählen Sie die Kategorie der Daten (siehe Abbildung 1).
3. Wählen Sie die **ADO.NET Entity Data Model** -Vorlage aus, nennen Sie das Entity Data Model MoviesDBModel.edmx, und klicken Sie auf die **hinzufügen** Schaltfläche. Klicken auf die **hinzufügen** Data Model-Assistenten wird gestartet.
4. In der **auswählen des Modellinhalts** Schritt, wählen Sie die **aus einer Datenbank generieren** aus, und klicken Sie auf die **Weiter** Schaltfläche (siehe Abbildung 2).
5. Als Nächstes erfahren Sie, wie Sie LINQ to Entities verwenden, um einen Satz von Datenbank-Datensätzen aus einer Datenbanktabelle zu abzurufen.
6. Schließlich können wir das Entity Framework verwendet, um einfügen, aktualisieren und Löschen von Datenbankdatensätzen.

Schließlich können wir das Entity Framework verwendet, um einfügen, aktualisieren und Löschen von Datenbankdatensätzen.

**Abbildung 1 – erstellen ein neues Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-cs/_static/image1.jpg)

**Abbildung 2 – Schritt des Aktivitätsmodells Inhalt auswählen**

![clip_image004](creating-model-classes-with-the-entity-framework-cs/_static/image2.jpg)

**Abbildung 3 – Wählen Sie Ihre Datenverbindung**

![clip_image006](creating-model-classes-with-the-entity-framework-cs/_static/image3.jpg)

**Abbildung 4 – Datenbankobjekte auswählen**

![clip_image008](creating-model-classes-with-the-entity-framework-cs/_static/image4.jpg)

#### <a name="modifying-the-adonet-entity-data-model"></a>Ändern das ADO.NET Entity Data Model

Nachdem Sie ein Entity Data Model erstellen, können Sie das Modell ändern, durch die Nutzung des Entity Designers (siehe Abbildung 5). Sie können der Entity Designer zu einem beliebigen Zeitpunkt durch Doppelklicken auf die MoviesDBModel.edmx-Datei im Ordner "Models" im Projektmappen-Explorer-Fenster öffnen.

**Abbildung 5: der ADO.NET Entity Data Model-Designer**

![clip_image010](creating-model-classes-with-the-entity-framework-cs/_static/image5.jpg)

Sie können z. B. der Entity Designer verwenden, so ändern Sie den Namen der Klassen, die der Assistent für Entity Model Daten generiert. Der Assistent erstellt eine neue Data Access-Klasse, mit dem Namen Filme hat. Das heißt, hat der Assistent der Klasse den sehr gleichen Namen wie der Datenbanktabelle. Da wir diese Klasse zum Darstellen einer bestimmten Film-Instanz verwenden, sollten wir die Klasse von Filmen in Film umbenennen.

Wenn Sie eine Entitätsklasse umbenennen möchten, können Sie das Doppelklicken Sie auf den Klassennamen im Entity Designer und geben Sie einen neuen Namen (siehe Abbildung 6). Alternativ können Sie den Namen einer Entität im Fenster Eigenschaften ändern, nach dem Auswählen einer Entität im Entity Designer.

**Abbildung 6: Ändern eines Entitätsnamens**

![clip_image012](creating-model-classes-with-the-entity-framework-cs/_static/image6.jpg)

Denken Sie daran, Ihre Entity Data Model speichern, nach dem vornehmen einer Änderung durch Klicken auf die Schaltfläche "Speichern" (das Symbol der Diskette). Hinter den Kulissen generiert der Entity Designer einen Satz von Klassen in c#. Sie können diese Klassen anzeigen, indem Sie die MoviesDBModel.Designer.cs-Datei aus dem Projektmappen-Explorer-Fenster zu öffnen.


Ändern Sie den Code in der Datei "Designer.cs" nicht, da Ihre Änderungen beim nächsten überschrieben werden Sie auf der Entity Designer verwendet. Wenn Sie die Entitätsklassen, die in der Datei "Designer.cs" definiert die Funktionalität erweitern möchten, und klicken Sie dann können Sie erstellen *Teilklassen* in separaten Dateien.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Auswählen von Datensätzen einer Datenbank mit Entitätsframework

Beginnen wir erstellen unsere Filmdatenbank-Anwendung durch Erstellen einer Seite, die eine Liste von filmdatensätzen anzeigt. Die Home-Controller in Codebeispiel 1 stellt eine Aktion, die mit dem Namen Index(). Die Index()-Aktion, die alle die Movie-Datensätze aus der Tabelle "Movie"-Datenbank durch die Nutzung von Entity Framework zurückgegeben.

**Codebeispiel 1 – Controllers\HomeController. cs**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample1.cs)]

Beachten Sie, dass der Controller in Codebeispiel 1 einen Konstruktor enthält. Der Konstruktor initialisiert das Feld auf Klassenebene \_Db. Die \_Db-Feld darstellt, die Datenbankentitäten, die vom Microsoft Entity Framework generiert. Die \_Db Feld ist eine Instanz der MoviesDBEntities-Klasse, die vom Entity Designer generiert wurde.


Um TheMoviesDBEntities-Klasse in den Home-Controller verwenden zu können, müssen Sie den MovieEntityApp.Models-Namespace importieren (*MVCProjectName*. Modelle).


Die \_Db-Feld wird innerhalb der Index()-Aktion zum Abrufen der Datensätze aus der Tabelle der Datenbank Filme verwendet. Der Ausdruck \_Db. MovieSet stellt alle Datensätze aus der Tabelle der Datenbank Filme dar. Die ToList()-Methode wird verwendet, um den Satz von Filmen in eine generische Auflistung von Movie-Objekten zu konvertieren (Liste&lt;Film&gt;).

Die Movie-Datensätze werden mithilfe von LINQ to Entities abgerufen. Die Aktion Index() in Codebeispiel 1 verwendet LINQ *Methodensyntax* auf das Abrufen des Satzes von Datenbank-Datensätzen. Falls gewünscht, können Sie LINQ *Abfragesyntax* stattdessen. Führen Sie die folgenden beiden Anweisungen sehr dieselben Schritte aus:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample2.cs)]

Verwenden Sie jeweils LINQ-Syntax – Methodensyntax oder der Abfragesyntax –, die Sie die intuitivste finden. Es gibt keinen Unterschied zwischen den beiden Ansätzen – der einzige Unterschied ist.

Die Ansicht im Codebeispiel 2 wird verwendet, die Film-Datensätze angezeigt.

**Codebeispiel 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample3.aspx)]

Die Ansicht im Codebeispiel 2 enthält eine **Foreach** Schleife, die jeden Datensatz Film durchläuft und zeigt die Werte der Eigenschaften, die Titel und Director der Movie-Datensatz. Beachten Sie, dass ein Link zum Bearbeiten und Löschen neben jedem Datensatz angezeigt wird. Darüber hinaus ein Film hinzufügen Link angezeigt wird, am unteren Rand der Ansicht (siehe Abbildung 7).

**Abbildung 7 – Ansicht "Index"**

![clip_image014](creating-model-classes-with-the-entity-framework-cs/_static/image7.jpg)

Ansicht "Index" ist ein *typisierte Ansicht*. Ansicht "Index" enthält einen &lt;% @ Page %&gt; -Direktive zusammen mit einer *Inherits* -Attribut, das die Modelleigenschaft in eine stark typisierte, generische listenauflistung von Filmobjekten wandelt (Liste&lt;Film).

## <a name="inserting-database-records-with-the-entity-framework"></a>Einfügen von Datensätzen einer Datenbank mit Entitätsframework

Sie können das Entity Framework verwenden, zum Einfügen neuer Datensätze in einer Datenbanktabelle erleichtern. Codebeispiel 3 enthält zwei neue Aktionen hinzugefügt, der Home-Controller-Klasse, die Sie zum Einfügen neuer Datensätze in der Tabelle "Movie"-Datenbank verwenden können.

**Codebeispiel 3 – Controllers\HomeController. cs (Methoden "Add")**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample4.cs)]

Die erste Add()-Aktion gibt einfach eine Ansicht zurück. Die Ansicht enthält ein Formular zum Hinzufügen einer neuen filmdatenbank erfassen (siehe Abbildung 8). Wenn Sie das Formular senden, wird die zweite Add() Aktion aufgerufen.

Beachten Sie, dass die zweite Add()-Aktion mit dem AcceptVerbs-Attribut ergänzt wird. Diese Aktion kann aufgerufen werden, nur, wenn einen HTTP POST-Vorgang ausführen. Das heißt, kann diese Aktion nur aufgerufen werden, beim Buchen von einem HTML-Formular.

Die zweite Add()-Aktion erstellt eine neue Instanz der Entity Framework Movie-Klasse mithilfe der ASP.NET MVC TryUpdateModel()-Methode. Die TryUpdateModel()-Methode akzeptiert die Felder in der Add()-Methode übergeben FormCollection und weist die Werte dieser Felder der HTML-Formular die Movie-Klasse.


Wenn Sie das Entity Framework verwenden, müssen Sie eine "weißen Liste" von Eigenschaften angeben, wenn die TryUpdateModel oder UpdateModel-Methoden verwendet, um die Eigenschaften einer Entitätsklasse zu aktualisieren.


Als Nächstes führt die Aktion Add() einige einfache formularvalidierung. Die Aktion stellt sicher, dass die title- und Director Eigenschaften Werte besitzen. Wenn ein Überprüfungsfehler vorliegt, wird eine Validierungsfehlermeldung auf ModelState hinzugefügt.

Treten keine Validierungsfehler mehr auftreten wird ein neuen Film-Datensatz in die Datenbanktabelle "Movies" mithilfe von Entity Framework hinzugefügt. Der neue Datensatz wird die Datenbank mit den folgenden zwei Codezeilen hinzugefügt:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample5.cs)]

Die erste Zeile des Codes fügt die neue Movie-Entität, auf den Satz von Filmen, die vom Entity Framework verfolgt werden. Die zweite Zeile des Codes speichert alle Änderungen, die Kino nachverfolgt wird, an der zugrunde liegenden Datenbank vorgenommen wurden.

**Abbildung 8 – die Ansicht hinzufügen**

![clip_image016](creating-model-classes-with-the-entity-framework-cs/_static/image8.jpg)

#### <a name="updating-database-records-with-the-entity-framework"></a>Aktualisieren von Datenbankdatensätzen mit dem Entitätsframework

Sie können fast den gleichen Ansatz, um eine Datenbank-Datensatz mit dem Entity Framework als Ansatz zu bearbeiten, die wir gerade gefolgt, um einen neuen Datenbankdatensatz einzufügen, befolgen. Programmausdruck 4 enthält zwei neue Controlleraktionen, die mit dem Namen Edit(). Die erste Edit()-Aktion gibt ein HTML-Formular zum Bearbeiten eines Datensatzes Film zurück. Die zweite Edit() Aktion versucht, die die Datenbank zu aktualisieren.

**Programmausdruck 4 – Controllers\HomeController. cs (Edit-Methoden)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample6.cs)]

Die zweite Edit() Aktion startet durch Abrufen des Datensatzes Film aus der Datenbank, die mit der Id des Films bearbeiteten übereinstimmt. Die folgende LINQ to Entities-Anweisung, holt der ersten Datenbankdatensatzes, der eine bestimmte Id entspricht:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample7.cs)]

Als Nächstes wird die TryUpdateModel()-Methode verwendet, die Eigenschaften der Entität Film die Werte der HTML-Formularfelder zuweisen. Beachten Sie, dass eine Zulassungsliste bereitgestellt wird, um die genaue Eigenschaften aktualisieren anzugeben.

Als Nächstes wird eine einfache Prüfung ausgeführt, um sicherzustellen, dass sowohl der Filmtitel und Director Eigenschaften Werte besitzen. Wenn eine der Eigenschaften einen Wert vorhanden ist, klicken Sie dann eine Validierungsfehlermeldung angezeigt wird ModelState hinzugefügt, und ModelState.IsValid gibt den Wert "false" zurück.

Abschließend treten keine Validierungsfehler mehr auftreten, wird dann die zugrunde liegenden Datenbanktabelle von Filmen mit den Änderungen aktualisiert durch Aufrufen der Methode SaveChanges().

Wenn Sie Datenbank-Datensätze bearbeiten zu können, müssen Sie übergeben die Id des Datensatzes wird bearbeitet werden, um die Controlleraktion, die die datenbankaktualisierung durchführt. Andernfalls wird die Controlleraktion nicht bekannt, welcher Datensatz in der zugrunde liegenden Datenbank zu aktualisieren. In Listing 5, enthalten die Edit-Ansicht enthält ein ausgeblendetes Formularfeld, das die Id der Datenbank-Datensatz bearbeitet wird darstellt.

**Programmausdruck 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Löschen von Datensätzen einer Datenbank mit Entitätsframework

Der endgültige Datenbankvorgang, die wir in diesem Tutorial angehen müssen, ist Datenbank-Datensätzen wird gelöscht. Sie können die Controlleraktion in Codebeispiel 6 verwenden, um eine bestimmte Datenbank-Datensatz zu löschen.

**Codebeispiel 6: \Controllers\HomeController.cs (Delete-Aktion)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample9.cs)]

Die Aktion Delete() ruft zuerst den Film Entität mit der Id übereinstimmt, an die Aktion übergeben ab. Als Nächstes wird der Film aus der Datenbank durch Aufrufen der DeleteObject()-Methode, gefolgt von der Methode SaveChanges() gelöscht. Schließlich wird der Benutzer an die Ansicht "Index" umgeleitet.

## <a name="summary"></a>Zusammenfassung

Der Zweck dieses Lernprogramms wurde veranschaulicht, wie Sie einen datenbankgesteuerten Webanwendungen erstellen können, durch die Nutzung von ASP.NET MVC und das Microsoft Entity Framework. Sie haben gelernt, wie zum Erstellen einer Anwendung, mit der Sie auswählen, einfügen, aktualisieren, und Löschen von Datenbankdatensätzen.

Zunächst wird erläutert, wie Sie Entity Data Model-Assistenten verwenden können, um ein Entity Data Model aus Visual Studio zu generieren. Als Nächstes erfahren Sie, wie Sie LINQ to Entities verwenden, um einen Satz von Datenbank-Datensätzen aus einer Datenbanktabelle zu abzurufen. Schließlich können wir das Entity Framework verwendet, um einfügen, aktualisieren und Löschen von Datenbankdatensätzen.

> [!div class="step-by-step"]
> [Nächste](creating-model-classes-with-linq-to-sql-cs.md)
