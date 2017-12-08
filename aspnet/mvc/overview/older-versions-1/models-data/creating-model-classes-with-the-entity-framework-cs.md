---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
title: Erstellen von Modellklassen mit dem Entity Framework (c#) | Microsoft Docs
author: microsoft
description: "In diesem Lernprogramm erfahren Sie, wie für die Verwendung von ASP.NET MVC mit Microsoft Entity Framework. Erfahren Sie, wie der Assistent für Entity verwenden, um ein ADO.NET Entity Da erstellen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 61644169-e8b1-45dd-bf96-9c2301b69879
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
msc.type: authoredcontent
ms.openlocfilehash: 8a897f671de73d9991189e32a5d86b513051ef05
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-model-classes-with-the-entity-framework-c"></a>Erstellen von Modellklassen mit dem Entity Framework (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> In diesem Lernprogramm erfahren Sie, wie für die Verwendung von ASP.NET MVC mit Microsoft Entity Framework. Erfahren Sie, wie der Assistent für Entity verwenden, um ein ADO.NET Entity Data Model zu erstellen. Im Verlauf dieses Lernprogramms erstellen wir eine Webanwendung, die veranschaulicht, wie auswählen, einfügen, aktualisieren und Löschen von Datenbankdaten mithilfe von Entity Framework.


Ziel dieses Lernprogramms wird erläutert, wie Sie Datenzugriffsklassen, die mithilfe von Microsoft Entity Framework beim Erstellen einer ASP.NET MVC-Anwendung erstellen können. In diesem Lernprogramm wird davon ausgegangen keine bisherigen Kenntnisse von Microsoft Entity Framework. Am Ende dieses Lernprogramms müssen informieren, wie Sie mithilfe von Entity Framework auswählen, einfügen, aktualisieren und Löschen von Datenbank-Datensätze.

Das Entity Framework von Microsoft ist ein Objekt Relational Mapping (O/RM)-Tool, das Ihnen ermöglicht, eine Datenzugriffsschicht automatisch aus einer Datenbank zu generieren. Das Entity Framework können Sie die aufwändig per hand katalogisiert wird zum Erstellen Ihrer Datenzugriffsklassen zu vermeiden.

Um zu veranschaulichen, wie Sie Microsoft Entity Framework mit ASP.NET MVC verwenden können, müssen wir eine einfache beispielanwendung erstellen. Wir erstellen eine Film-datenbankanwendung, die Sie zum Anzeigen und Bearbeiten von Datenbankdatensätzen Film ermöglicht.

In diesem Lernprogramm wird davon ausgegangen, dass Sie Visual Studio 2008 oder Visual Web Developer 2008 mit Service Pack 1 aufweisen. Sie benötigen die Service Pack 1, um die Verwendung von Entity Framework. Sie können Visual Studio 2008 Service Pack 1 oder Visual Web Developer mit Service Pack 1 aus der folgenden Adresse herunterladen:

> [https://www.ASP.NET/Downloads/](https://www.asp.net/downloads)


> [!NOTE] 
> 
> Es gibt keine wesentlichen Verbindung zwischen ASP.NET MVC und Microsoft Entity Framework. Es gibt verschiedene Alternativen zur Entity Framework, das Sie mit ASP.NET MVC verwenden können. Beispielsweise können Sie Ihre MVC-Modell-Klassen, die mit anderen O/RM-Tools, z. B. Microsoft LINQ to SQL, NHibernate oder SubSonic erstellen.


## <a name="creating-the-movie-sample-database"></a>Erstellen der Beispieldatenbank Film

Die Film-datenbankanwendung verwendet eine Tabelle mit dem Namen Filme, die die folgenden Spalten enthält:

| Spaltenname | Datentyp | Zulassen NULL-Werte? | Ist Primary Key? |
| --- | --- | --- | --- |
| Id | int | False | True |
| Titel | Nvarchar(100) | False | False |
| Director | Nvarchar(100) | False | False |

Sie können diese Tabelle zu einem ASP.NET MVC-Projekt hinzufügen, indem Sie folgende Schritte:

1. Mit der rechten Maustaste in der App\_Datenordner im Projektmappen-Explorer-Fenster, und wählen Sie die Menüoption **hinzufügen, neue Element.**
2. Aus der **neues Element hinzufügen** wählen Sie im Dialogfeld **SQL Server-Datenbank**, benennen Sie der Datenbank der MoviesDB.mdf, und klicken Sie auf die **hinzufügen** Schaltfläche.
3. Doppelklicken Sie auf die MoviesDB.mdf-Datei, um den Server-Explorer/Datenbank-Explorer-Fenster zu öffnen.
4. Erweitern Sie die Verbindung mit der MoviesDB.mdf, mit der rechten Maustaste in des Tabellen-Ordners und wählen Sie die Menüoption **neue Tabelle hinzufügen**.
5. Fügen Sie den Tabellen-Designer die Spalten-Id, Titel und Director hinzu.
6. Klicken Sie auf die **speichern** Schaltfläche (er hat das Symbol des Diskettenlaufwerk) zum Speichern der neuen Tabelle mit dem Namen Filme.

Nach der Erstellung der Datenbanktabelle Filme, sollten Sie einige Beispieldaten zur Tabelle hinzufügen. Mit der rechten Maustaste in der Tabelle Filme, und wählen Sie die Menüoption **Tabellendaten anzeigen**. Sie können in das Raster gefälschte Filmdaten eingeben, die angezeigt wird.

## <a name="creating-the-adonet-entity-data-model"></a>Erstellen das ADO.NET Entity Data Model

Um das Entity Framework zu verwenden, müssen Sie ein Entity Data Model erstellen. Profitieren Sie von Visual Studio *Entity Data Model-Assistenten* zum automatischen Generieren von ein Entity Data Model aus einer Datenbank.

Führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste im Projektmappen-Explorer-Fenster Ordner Models, und wählen Sie die Menüoption **hinzufügen, neue Element**.
2. In der **neues Element hinzufügen** Dialogfeld Wählen Sie die Kategorie der Daten (siehe Abbildung 1).
3. Wählen Sie die **ADO.NET Entity Data Model** Vorlage, benennen Sie das Entity Data Model die MoviesDBModel.edmx, und klicken Sie auf die **hinzufügen** Schaltfläche. Klicken auf die **hinzufügen** Data Model-Assistenten wird gestartet.
4. In der **Modellinhalte** Schritt: Wählen Sie die **aus einer Datenbank generieren** aus, und klicken Sie auf die **Weiter** Schaltfläche (siehe Abbildung 2).
5. In der **wählen Sie Ihre Datenverbindung** Schritt, wählen Sie eine Verbindung mit der Datenbank MoviesDB.mdf, geben Sie die Entitäten Verbindungseinstellungen MoviesDBEntities benennen, und klicken Sie auf die **Weiter** Schaltfläche (siehe Abbildung 3).
6. In der **Datenbankobjekte auswählen** Schritt, wählen Sie die Datenbanktabelle Film, und klicken Sie auf die **Fertig stellen** Schaltfläche (siehe Abbildung 4).

Nachdem Sie diese Schritte abgeschlossen haben, öffnet der ADO.NET Entity Data Model-Designer (Entity Designer).

**Abbildung 1 – Erstellen einer neuen Entity Data Models**

![clip_image002](creating-model-classes-with-the-entity-framework-cs/_static/image1.jpg)

**Abbildung 2 – Inhalt Schritt "Modell" auswählen**

![clip_image004](creating-model-classes-with-the-entity-framework-cs/_static/image2.jpg)

**Abbildung 3 – Wählen Sie Ihre Datenverbindung aus**

![clip_image006](creating-model-classes-with-the-entity-framework-cs/_static/image3.jpg)

**Abbildung 4 – Datenbankobjekte auswählen**

![clip_image008](creating-model-classes-with-the-entity-framework-cs/_static/image4.jpg)

#### <a name="modifying-the-adonet-entity-data-model"></a>Ändern das ADO.NET Entity Data Model

Nachdem Sie ein Entity Data Model erstellt haben, können Sie das Modell ändern, indem Sie im Entity Designer nutzen (siehe Abbildung 5). Sie können zu einem beliebigen Zeitpunkt im Entity Designer öffnen, durch Doppelklicken auf die MoviesDBModel.edmx-Datei im Ordner "Modelle" im Projektmappen-Explorer-Fenster enthalten.

**Abbildung 5 – das ADO.NET Entity Data Model-Designer**

![clip_image010](creating-model-classes-with-the-entity-framework-cs/_static/image5.jpg)

Beispielsweise können Sie im Entity Designer verwenden, so ändern Sie den Namen der Klassen, die der Assistent für Entity Model Daten generiert. Der Assistent erstellt eine neue Data Access-Klasse mit dem Namen Filme. In anderen Worten: der Assistent gegeben der Klasse den sehr gleichen Namen wie der Datenbanktabelle. Da wir diese Klasse verwenden, um eine Instanz eines bestimmten Film repräsentieren zu verwenden, sollten wir die Klasse aus Filme Film umbenennen.

Wenn Sie eine Entitätsklasse umbenennen möchten, können Sie doppelklicken Sie auf den Klassennamen im Entity Designer und geben Sie einen neuen Namen (siehe Abbildung 6). Alternativ können Sie den Namen einer Entität im Eigenschaftenfenster ändern, nachdem Sie eine Entität im Entity Designer ausgewählt haben.

**Abbildung 6: Ändern eines Entitätsnamens**

![clip_image012](creating-model-classes-with-the-entity-framework-cs/_static/image6.jpg)

Denken Sie daran, um Ihr Entity Data Model zu speichern, nachdem eine Änderung vornehmen, indem Sie auf die Schaltfläche "Speichern" (das Symbol für die Diskette). Im Hintergrund generiert im Entity Designer eine Reihe von C#-Klassen. Sie können diese Klassen anzeigen, indem die MoviesDBModel.Designer.cs-Datei aus dem Projektmappen-Explorer-Fenster zu öffnen.


Ändern Sie den Code in der Datei Designer.cs nicht, da die Änderungen beim nächsten überschrieben werden Sie im Entity Designer verwenden. Wenn Sie, zum Erweitern der Funktionalität der Entitätsklassen im.Designer.cs-Datei definiert möchten, die Sie erstellen können *Teilklassen* in separaten Dateien.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Auswählen von Datenbankdatensätzen mit dem Entity Framework

Beginnen Sie unsere Film-datenbankanwendung erstellen, durch das Erstellen einer Seite, die eine Liste mit Datensätzen Film anzeigt. Die Home-Controller 1 auflisten macht eine Aktion, die mit dem Namen Index() verfügbar. Die Index()-Aktion, die alle die Film-Datensätze aus der Datenbanktabelle Film durch die Nutzung von Entity Framework zurückgegeben.

**1 – Controllers\HomeController. cs auflisten**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample1.cs)]

Beachten Sie, dass der Controller im Codebeispiel 1 einen Konstruktor enthält. Der Konstruktor initialisiert ein Feld auf Klassenebene \_Db. Die \_Db Feld dargestellt von Microsoft Entity Framework generierten Datenbankentitäten. Die \_DB-Feld ist eine Instanz der MoviesDBEntities-Klasse, die vom Entity Designer generiert wurde.


Um TheMoviesDBEntities-Klasse in den Home-Controller verwenden, müssen Sie den Namespace MovieEntityApp.Models importieren (*MVCProjectName*. Modelle).


Die \_DB-Feld wird innerhalb der Aktion Index() verwendet, um die Datensätze aus der Datenbanktabelle Filme abzurufen. Der Ausdruck \_Db. MovieSet stellt alle Datensätze aus der Datenbanktabelle Filme dar. Die ToList()-Methode wird verwendet, um den Satz von Filmen in eine generische Auflistung von Film-Objekte zu konvertieren (Liste&lt;Film&gt;).

Mithilfe von LINQ to Entities werden Datensätze Film abgerufen. Die Aktion Index() im Codebeispiel 1 verwendet LINQ *Methodensyntax* die Gruppe von Datenbankdatensätzen abrufen. Falls gewünscht, können Sie LINQ *Abfragesyntax* stattdessen. Führen Sie die folgenden beiden Anweisungen sehr dieselben Schritte aus:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample2.cs)]

Verwenden Sie unabhängig davon, welche LINQ-Syntax – Methodensyntax oder Abfragesyntax –, die Sie besonders intuitiv finden. Es besteht kein Unterschied zwischen den beiden Ansätzen – der einzige Unterschied ist.

Die Ansicht im Codebeispiel 2 wird verwendet, die Film-Datensätze angezeigt.

**2 – Views\Home\Index.aspx auflisten**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample3.aspx)]

Die Ansicht im Codebeispiel 2 enthält einen **Foreach** Schleife, die jeden Datensatz Film durchläuft und zeigt die Werte eines Films Datensatzabschnitt Titel und Director-Eigenschaften. Beachten Sie, dass ein Link bearbeiten und Löschen neben jeder Datensatz angezeigt wird. Darüber hinaus ein Film hinzufügen Link am unteren Rand der Ansicht angezeigt wird (siehe Abbildung 7).

**Abbildung 7 – die Indexansicht**

![clip_image014](creating-model-classes-with-the-entity-framework-cs/_static/image7.jpg)

Die Indexansicht ist ein *typisierte Ansicht*. Die Indexansicht umfasst eine &lt;% @ Page %&gt; -Direktive zusammen mit einer *Inherits* -Attribut, das die Modell-Eigenschaft, um eine stark typisierte generische listenauflistung von Film-Objekte umgewandelt (Liste&lt;Film).

## <a name="inserting-database-records-with-the-entity-framework"></a>Einfügen von Datenbank-Datensätze mit dem Entity Framework

Sie können das Entity Framework verwenden, zum Einfügen neuer Datensätze in einer Datenbanktabelle zu erleichtern. Auflisten von 3 enthält zwei neue Aktionen hinzugefügt, der Home-Controller-Klasse, die Sie zum Einfügen neuer Datensätze in der Datenbanktabelle Film verwenden können.

**Auflisten von 3 – Controllers\HomeController. cs (Add-Methoden)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample4.cs)]

Die erste Add()-Aktion gibt einfach eine Ansicht zurück. Die Ansicht enthält ein Formular zum Hinzufügen einer neuen Film-Datenbank erfassen (siehe Abbildung 8). Die zweite Add() Aktion wird aufgerufen, wenn bei der Übermittlung des Formulars.

Beachten Sie, dass die zweite Add()-Aktion mit dem AcceptVerbs-Attribut ergänzt wird. Diese Aktion kann aufgerufen werden, nur, wenn einen HTTP POST-Vorgang ausführen. Diese Aktion kann also nur aufgerufen werden, bei der Buchung von einem HTML-Formular.

Die zweite Add()-Aktion erstellt eine neue Instanz der Entity Framework Film-Klasse mit Hilfe der ASP.NET MVC TryUpdateModel()-Methode. Die TryUpdateModel()-Methode akzeptiert die Felder in der FormCollection an die Add()-Methode übergeben und weist die Werte dieser Felder der HTML-Formular die Film-Klasse.


Bei Verwendung von Entity Framework müssen Sie eine "White-Liste" der Eigenschaften angeben, wenn die TryUpdateModel oder UpdateModel-Methoden verwendet, um die Eigenschaften einer Entitätsklasse zu aktualisieren.


Als Nächstes führt die Aktion Add() einige einfache Form-Überprüfung aus. Die Aktion stellt sicher, dass der Titel und die Director Eigenschaften Werte aufweisen. Wenn ein Validierungsfehler vorliegt, wird eine Validierungsfehlermeldung angezeigt auf ModelState hinzugefügt.

Wenn keine gültigkeitsprobleme bestehen wird ein neuer Film-Datensatz der Datenbanktabelle Filme mithilfe von Entity Framework hinzugefügt. Der neue Datensatz wird die Datenbank mit den folgenden zwei Codezeilen hinzugefügt:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample5.cs)]

Die erste Codezeile fügt die neue Film-Entität auf den Satz von Filmen, die vom Entity Framework nachverfolgt. Die zweite Zeile des Codes speichert die Änderungen an der zugrunde liegenden Datenbank nachverfolgten Kino vorgenommen wurden.

**Abbildung 8 – die Sicht hinzufügen**

![clip_image016](creating-model-classes-with-the-entity-framework-cs/_static/image8.jpg)

#### <a name="updating-database-records-with-the-entity-framework"></a>Aktualisieren von Datenbank-Datensätze mit dem Entity Framework

Sie können fast genauso vorgehen zum Datensatzes in einer Datenbank mit dem Entity Framework als Ansatz zu bearbeiten, die wir nur zum Einfügen eines neuen Datensatzes für die Datenbank bereits folgen. Auflisten von 4 enthält zwei neue Domänencontroller-Aktionen, die mit dem Namen Edit(). Die erste Edit()-Aktion gibt ein HTML-Formular zum Bearbeiten von eines Film Datensatzes zurück. Die zweite Edit() Aktion versucht, die Datenbank zu aktualisieren.

**Auflisten von 4 – Controllers\HomeController. cs (Bearbeiten-Methoden)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample6.cs)]

Die zweite Edit() Aktion startet durch Abrufen des Film-Datensatzes aus der Datenbank, die die Id des zu bearbeitenden Films entspricht. Die folgende LINQ to Entities-Anweisung, holt der ersten Datenbankeintrag, der eine bestimmte Id übereinstimmt:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample7.cs)]

Als Nächstes wird die TryUpdateModel()-Methode verwendet, die Werte der Felder der HTML-Formular an den Eigenschaften der Entität Film zugewiesen. Beachten Sie, dass eine weiße Liste bereitgestellt wird, um die genaue Eigenschaften aktualisieren anzugeben.

Als Nächstes wird einige einfache Überprüfung ausgeführt, um sicherzustellen, dass der Filmtitel und die Director Eigenschaften Werte aufweisen. Wenn entweder Eigenschaft einen Wert vorhanden ist, klicken Sie dann eine Validierungsfehlermeldung angezeigt wird ModelState hinzugefügt, und ModelState.IsValid gibt den Wert "false" zurück.

Abschließend, wenn keine gültigkeitsprobleme bestehen, wird die zugrunde liegenden Datenbanktabelle von Filmen mit den Änderungen aktualisiert durch Aufrufen der Methode SaveChanges().

Wenn Sie Datenbank-Datensätze zu bearbeiten, müssen Sie übergeben Sie die Id des Datensatzes bearbeitet werden, um die Controlleraktion, die das Datenbankupdate ausführt. Andernfalls wird Controlleraktion nicht bekannt ist der Datensatz, der in der zugrunde liegenden Datenbank aktualisiert. Die Bearbeitungsansicht in auflisten 5 enthaltenen enthält ein ausgeblendetes Formularfeld, das die Id des zu bearbeitenden Datenbank-Datensatzes darstellt.

**5 – Views\Home\Edit.aspx auflisten**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Löschen von Datenbankdatensätzen mit dem Entity Framework

Der endgültige Datenbankvorgang, die wir in diesem Lernprogramm befassen müssen, ist Datenbankdatensätze löschen. Auflisten von 6 können Controlleraktion Datensatzes in einer bestimmten Datenbank zu löschen.

**Auflisten von 6 – \Controllers\HomeController.cs (Delete-Aktion)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample9.cs)]

Die Delete()-Aktion ruft zuerst die Entität, die mit der Id übereinstimmt, an die Aktion übergeben Film ab. Als Nächstes wird der Film durch Aufrufen der DeleteObject()-Methode, gefolgt von der SaveChanges()-Methode aus der Datenbank gelöscht. Schließlich wird der Benutzer wieder an die Indexansicht umgeleitet.

## <a name="summary"></a>Zusammenfassung

Der Zweck dieses Lernprogramms wurde veranschaulicht, wie Sie die Datenbank-driven Webanwendungen erstellen können, durch die Nutzung von ASP.NET MVC und Microsoft Entity Framework. Sie haben gelernt, wie zum Erstellen einer Anwendung, mit dem Sie auswählen, einfügen, aktualisieren, und Löschen von Datenbank-Datensätze.

Zuerst wird erläutert, Verwendung des Entity Data Model-Assistenten zum Generieren eines Entity Data Model aus Visual Studio. Als Nächstes erfahren Sie, wie LINQ to Entities verwenden, um eine Gruppe von Datenbankdatensätzen aus einer Datenbanktabelle abrufen. Schließlich können wir das Entity Framework verwendet, um einfügen, aktualisieren und Löschen von Datenbank-Datensätze.

>[!div class="step-by-step"]
[Nächste](creating-model-classes-with-linq-to-sql-cs.md)
