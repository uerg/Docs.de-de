---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: Erstellen von Modellklassen mit LINQ to SQL (c#) | Microsoft-Dokumentation
author: microsoft
description: Das Ziel in diesem Tutorial wird eine Methode zum Erstellen von Modellklassen für eine ASP.NET MVC-Anwendung beschrieben. In diesem Tutorial erfahren Sie, wie Sie das Modell c erstellen...
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: 35fa1e882268eaa1bfbd3589c8173e853ad5c5a7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815849"
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>Erstellen von Modellklassen mit LINQ to SQL (c#)
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> Das Ziel in diesem Tutorial wird eine Methode zum Erstellen von Modellklassen für eine ASP.NET MVC-Anwendung beschrieben. In diesem Tutorial erfahren Sie, wie zum Erstellen von Modellklassen aus, und führen Sie Zugriff auf die Datenbank durch die Nutzung von Microsoft-LINQ to SQL.


Das Ziel in diesem Tutorial wird eine Methode zum Erstellen von Modellklassen für eine ASP.NET MVC-Anwendung beschrieben. In diesem Tutorial erfahren Sie, wie zum Erstellen von Modellklassen aus, und führen Sie Zugriff auf die Datenbank durch die Nutzung von Microsoft-LINQ to SQL

In diesem Tutorial erstellen wir eine grundlegende filmdatenbankanwendung. Zunächst durch das Erstellen der filmdatenbankanwendung in die schnellste und einfachste Möglichkeit möglich. Wir ausführen alle unsere direkt über unsere Controlleraktionen.

Als Nächstes erfahren Sie, wie Sie das Repositorymuster verwenden. Verwenden das Repository-Muster erfordert etwas mehr Arbeit. Der Vorteil der Einführung dieses Muster ist jedoch darin, dass zum Erstellen von Anwendungen, die so angepasst werden, ändern und einfach getestet werden kann.

## <a name="what-is-a-model-class"></a>Was ist eine Modellklasse?

Ein MVC-Modell enthält alle an der Anwendungslogik, die nicht in einer MVC-Ansicht oder MVC-Controller enthalten ist. Insbesondere enthält ein MVC-Modell alle Anwendung Unternehmensziele und der Logik für den Datenzugriff.

Sie können eine Vielzahl von verschiedenen Technologien verwenden, um Ihre Logik für den Datenzugriff zu implementieren. Beispielsweise können Sie die Access-Datenklassen, die mithilfe der Microsoft Entity Framework, NHibernate, Subsonic oder ADO.NET Klassen erstellen.

In diesem Tutorial verwende ich LINQ to SQL zum Abfragen und aktualisieren Sie die Datenbank an. LINQ to SQL bietet Sie eine sehr einfache Methode für die Interaktion mit einer Microsoft SQL Server-Datenbank. Allerdings ist es wichtig, zu verstehen, dass das ASP.NET-MVC-Framework nicht in irgendeiner Weise zu LINQ to SQL gebunden ist. ASP.NET MVC ist kompatibel mit jeder datenzugriffstechnologie.

## <a name="create-a-movie-database"></a>Erstellen Sie eine Filmdatenbank

In diesem Tutorial – um zu veranschaulichen, wie Sie Modellklassen – entwickeln erstellen wir eine einfache filmdatenbankanwendung. Der erste Schritt ist die Erstellung eine neue Datenbank. Mit der rechten Maustaste in der App\_Ordner im Projektmappen-Explorer-Fenster, und wählen Sie die Menüoption **hinzufügen, neue Element**. Wählen Sie die **SQL Server-Datenbank** -Vorlage aus, geben Sie ihm den Namen MoviesDB.mdf, und klicken Sie auf die **hinzufügen** Schaltfläche (siehe Abbildung 1).


[![Hinzufügen einer neuen SQL Server-Datenbank](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Abbildung 01**: Hinzufügen einer neuen SQL Server-Datenbank ([klicken Sie, um das Bild in voller Größe anzeigen](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


Nachdem Sie die neue Datenbank erstellt haben, können Sie die Datenbank durch Doppelklicken auf die MoviesDB.mdf-Datei in der App öffnen\_Datenordner. Durch Doppelklicken auf die Datei MoviesDB.mdf wird das Server-Explorer-Fenster geöffnet (siehe Abbildung 2).

Das Server-Explorer-Fenster wird das Datenbank-Explorer-Fenster aufgerufen, wenn Sie Visual Web Developer verwenden.


[![Verwenden des Server-Explorer-Fensters](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Abbildung 02**: Verwenden des Server-Explorer-Fensters ([klicken Sie, um das Bild in voller Größe anzeigen](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


Wir müssen unsere Datenbank eine Tabelle hinzufügen, die unsere Filme darstellt. Mit der rechten Maustaste in den Ordner "Tabellen", und wählen Sie die Menüoption **neue Tabelle hinzufügen**. Durch Auswählen dieser Menüoption wird der Tabellen-Designer geöffnet (siehe Abbildung 3).


[![Verwenden des Server-Explorer-Fensters](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Abbildung 03**: der Tabellen-Designer ([klicken Sie, um das Bild in voller Größe anzeigen](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


Wir müssen unsere Datenbanktabelle die folgenden Spalten hinzugefügt:

| **Name der Spalte** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | Int | False |
| Titel | Nvarchar(200)-Datentyp gepackt ist | False |
| Director | nvarchar(50) | False |

Sie müssen zwei spezielle Schritte, die Id-Spalte ausgeführt werden. Zunächst müssen Sie die Id-Spalte als eine primäre Schlüsselspalte zu markieren, indem Sie die Spalte im Tabellen-Designer auswählen, und klicken auf das Symbol eines Schlüssels. LINQ to SQL müssen Sie Ihre Primärschlüsselspalten an, wenn eingefügt oder aktualisiert werden, für die Datenbank ausführen.

Als Nächstes müssen Sie die Id-Spalte als eine Identity-Spalte zu markieren, indem Sie Ja zum Zuweisen der **ist Identity** Eigenschaft (siehe Abbildung 3). Eine Identitätsspalte handelt es sich um eine Spalte, die automatisch eine neue Nummer zugewiesen wird, wenn Sie eine neue Zeile mit Daten in eine Tabelle hinzufügen.

## <a name="create-linq-to-sql-classes"></a>Erstellen von LINQ to SQL-Klassen

Unsere MVC-Modell enthält LINQ to SQL-Klassen, die die TblMovie Datenbank darstellen. Die einfachste Möglichkeit zum Erstellen dieser LINQ to SQL-Klassen ist, mit der rechten Maustaste in den Ordner "Models", wählen Sie **hinzufügen, neue Element**, wählen Sie die LINQ to SQL-Klassen-Vorlage, nennen Sie den Klassen Movie.dbml, und klicken Sie auf die **hinzufügen**Schaltfläche (siehe Abbildung 4).


[![Erstellen von LINQ to SQL-Klassen](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Abbildung 04**: Erstellen von LINQ to SQL-Klassen ([klicken Sie, um das Bild in voller Größe anzeigen](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


Sofort, nachdem Sie die Movie-LINQ to SQL-Klassen erstellt haben, wird der Object Relational Designer angezeigt. Sie können Datenbanktabellen ziehen, aus dem Server-Explorer-Fenster auf den Object Relational Designer zum Erstellen von LINQ to SQL-Klassen, die bestimmten Datenbanktabellen darstellen. Wir müssen die TblMovie-Datenbanktabelle auf den Object Relational Designer (siehe Abbildung 5).


[![Verwenden den Object Relational Designer](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**Abbildung 05**: verwenden den Object Relational Designer ([klicken Sie, um das Bild in voller Größe anzeigen](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


Standardmäßig erstellt der Object Relational Designer eine Klasse mit dem sehr gleichen Namen wie die Datenbanktabelle, die Sie in den Designer ziehen. Aber wir wollen nicht unsere Klasse aufrufen `tblMovie`. Aus diesem Grund, klicken Sie auf den Namen der Klasse in den Designer, und ändern Sie den Namen der Klasse Film.

Beachten Sie schließlich, klicken Sie auf die **speichern** Schaltfläche (das Bild von der Diskette) zum Speichern von LINQ to SQL-Klassen. Andernfalls wird nicht die LINQ to SQL-Klassen, durch den Object Relational Designer generiert werden.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Mit LINQ to SQL in eine Controlleraktion

Nun, da wir unsere LINQ to SQL-Klassen haben, können wir diese Klassen verwenden, um Daten aus der Datenbank abzurufen. In diesem Abschnitt erfahren Sie, wie Sie LINQ to SQL-Klassen direkt in eine Controlleraktion zu verwenden. Wir werden die Liste der Filme aus der Datenbanktabelle TblMovies in einer MVC-Ansicht anzeigen.

Zunächst müssen wir die HomeController-Klasse ändern. Diese Klasse finden Sie im Ordner "Controllers" Ihrer Anwendung. Ändern Sie die Klasse, sodass sie wie die Klasse in der Liste 1 aussieht.

**Codebeispiel 1: `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

Die `Index()` Aktion in Codebeispiel 1 verwendet eine LINQ to SQL-DataContext-Klasse (die `MovieDataContext`) zur Darstellung der `MoviesDB` Datenbank. Die `MoveDataContext` Klasse von der Visual Studio Object Relational Designer generiert wurde.

Eine LINQ-Abfrage wird ausgeführt, für den DataContext zum Abrufen aller Filme aus der `tblMovies` Datenbanktabelle. Die Liste der Filme wird zugewiesen, um eine lokale Variable namens `movies`. Schließlich wird die Liste der Filme durch Anzeigen von Daten an die Ansicht übergeben.

Um die Filme anzuzeigen, müssen wir als Nächstes ändern Sie die Ansicht "Index". Sie finden die Ansicht "Index" in der `Views\Home\` Ordner. Aktualisieren Sie die Ansicht "Index", sodass sie aussieht wie die Ansicht in Listing 2 dargestellt.

**Codebeispiel 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Beachten Sie, die die geänderte Index-Ansicht enthält eine `<%@ import namespace %>` Direktive am oberen Rand der Ansicht. Diese Richtlinie importiert die `MvcApplication1.Models namespace`. Wir benötigen diesen Namespace für die Arbeit mit der `model` Klassen – insbesondere die `Movie` Klasse – in der Ansicht.

Die Ansicht im Codebeispiel 2 enthält eine `foreach` Schleife, die alle Elemente durch dargestellt durchläuft die `ViewData.Model` Eigenschaft. Der Wert des der `Title` Eigenschaft wird angezeigt, für die einzelnen `movie`.

Beachten Sie, dass der Wert des der `ViewData.Model` Eigenschaft umgewandelt wird ein `IEnumerable`. Dies ist erforderlich, um den Inhalt des durchlaufen `ViewData.Model`. Hier eine weitere Möglichkeit ist die Erstellung einer stark typisierten `view`. Beim Erstellen einer stark typisierten `view`, wandeln Sie die `ViewData.Model` Eigenschaft auf einen bestimmten Typ in eine Ansicht des Code-Behind-Klasse.

Wenn das Ausführen der Anwendung nach dem Ändern der `HomeController` -Klasse und den Index anzeigen, erhalten Sie eine leere Seite. Sie erhalten eine leere Seite, da es keine Movie-Datensätze in gibt der `tblMovies` Datenbanktabelle.

Zum Hinzufügen von Datensätzen, die die `tblMovies` Datenbanktabelle der rechten Maustaste auf die `tblMovies` Datenbanktabelle im Server-Explorer-Fenster (Datenbank-Explorer-Fenster in Visual Web Developer), und wählen Sie die Menüoption "-" Tabellendaten anzeigen. Sie können einfügen `movie` Datensätze mithilfe des Rasters, das angezeigt wird (siehe Abbildung 6).


[![Einfügen von Filmen](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Abbildung 06**: Einfügen von Filmen ([klicken Sie, um das Bild in voller Größe anzeigen](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


Nach dem Hinzufügen von einigen Datenbank-Datensätzen, die `tblMovies` Tabelle, und Sie die Anwendung ausführen, sehen Sie die Seite in Abbildung 7. Alle von der Movie-Datenbank-Datensätzen werden in einer Aufzählung angezeigt.


[![Anzeigen von Videos mit Ansicht "Index"](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Abbildung 07**: Anzeigen von Videos mit Ansicht "Index" ([klicken Sie, um das Bild in voller Größe anzeigen](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Verwenden das Repository-Muster

Im vorherigen Abschnitt haben wir die LINQ to SQL-Klassen direkt in eine Controlleraktion verwendet. Verwendet die `MovieDataContext` -Klasse direkt aus der `Index()` Controlleraktion. Es gibt nichts auszusetzen dadurch bei einer einfachen Anwendung. Arbeiten mit LINQ to SQL direkt in eine Controllerklasse erstellt jedoch Probleme, wenn Sie eine komplexere Anwendung erstellen möchten.

Mit LINQ to SQL innerhalb einer Controllerklasse erschwert die Technologien für den Datenzugriff in der Zukunft zu wechseln. Sie könnten z. B. von der Verwendung von Microsoft-LINQ to SQL die Verwendung von Microsoft Entity Framework als Ihre datenzugriffstechnologie wechseln. In diesem Fall müssen Sie jeder Controller neu zu schreiben, die Zugriff auf die Datenbank in Ihrer Anwendung.

Mit LINQ to SQL innerhalb einer Controllerklasse erschwert auch zum Erstellen von Komponententests für Ihre Anwendung. Normalerweise möchten Sie nicht mit einer Datenbank interagieren, wenn Komponententests ausgeführt. Sie möchten Ihre Komponententests verwenden, um die Anwendungslogik und nicht auf Ihrem Datenbankserver zu testen.

Um eine MVC-Anwendung zu erstellen, die besser anpassen, um zukünftige Änderungen und, die leichter getestet werden kann, sollten Sie das Repositorymuster verwenden. Wenn Sie das Repository-Muster verwenden, erstellen Sie eine separate repositoryklasse, die alle Ihre Datenbankzugriffslogik enthält.

Wenn Sie die "Repository"-Klasse erstellen, erstellen Sie eine Schnittstelle, die alle von der repositoryklasse verwendeten Methoden darstellt. Innerhalb der Controller schreiben Sie Ihren Code für die Schnittstelle statt dem Repository. Auf diese Weise können Sie das Repository mithilfe von verschiedenen datenzugriffstechnologien in der Zukunft implementieren.

Den Namen der Schnittstelle in Programmausdruck 3 `IMovieRepository` und es stellt eine einzelne Methode namens `ListAll()`.

**Codebeispiel 3: `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

In Listing 4 "Repository"-Klasse implementiert die `IMovieRepository` Schnittstelle. Beachten Sie, dass es sich um eine Methode namens enthält `ListAll()` , die die erforderliche Methode entspricht der `IMovieRepository` Schnittstelle.

**Codebeispiel 4: `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Zum Schluss die `MoviesController` Klasse in Listing 5 wird das Repository-Muster verwendet. Es verwendet keine LINQ to SQL-Klassen direkt.

**Codebeispiel 5: `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Beachten Sie, dass die `MoviesController` Klasse in Listing 5 besitzt zwei Konstruktoren. Der erste Konstruktor, den parameterlosen Konstruktor wird aufgerufen, wenn Ihre Anwendung ausgeführt wird. Dieser Konstruktor erstellt eine Instanz der `MovieRepository` -Klasse und übergibt sie an der zweite Konstruktor.

Der zweite Konstruktor verfügt über einen einzigen Parameter: eine `IMovieRepository` Parameter. Dieser Konstruktor weist den Wert des Parameters einfach auf ein Feld auf Klassenebene `_repository`.

Die `MoviesController` Klasse ist die nutzen ein Software-Entwurfsmuster, die das Dependency Injection-Muster bezeichnet. Insbesondere wird so genannte Konstruktorbasierte Dependency Injection verwendet. Erfahren Sie mehr zu diesem Muster im folgenden Artikel von Martin Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Beachten Sie, dass der gesamte Code in die `MoviesController` Klasse (mit Ausnahme der erste Konstruktor) interagiert mit der `IMovieRepository` Schnittstelle anstelle des tatsächlichen `MovieRepository` Klasse. Der Code interagiert mit einer abstrakten Schnittstelle anstelle einer konkreten Implementierung der Schnittstelle.

Wenn Sie die neue datenzugriffstechnologie, die von der Anwendung verwendeten ändern möchten, und klicken Sie dann Sie einfach implementieren die `IMovieRepository` Schnittstelle mit einer Klasse, die die alternative Datenbank-Access-Technologie verwendet. Sie können z. B. Erstellen einer `EntityFrameworkMovieRepository` Klasse oder ein `SubSonicMovieRepository` Klasse. Da die Controller-Klasse für die Schnittstelle programmiert ist, können Sie eine neue Implementierung übergeben `IMovieRepository` an den Controller-Klasse und die Klasse würde auch weiterhin funktionieren.

Darüber hinaus sollten Sie testen die `MoviesController` Klasse, die Sie eine gefälschte Movie-Repository-Klasse zu übergeben, können die `HomeController`. Können Sie implementieren die `IMovieRepository` Klasse mit einer Klasse, die nicht tatsächlich Zugriff der Datenbank, aber enthält alle erforderlichen Methoden von der `IMovieRepository` Schnittstelle. Auf diese Weise können Sie Komponententests der `MoviesController` Klasse, ohne tatsächlich Zugriff auf eine echte Datenbank.

## <a name="summary"></a>Zusammenfassung

Das Ziel dieses Lernprogramms wurde veranschaulicht, wie Sie die MVC-ViewModel-Klassen erstellen können, durch die Nutzung von Microsoft-LINQ to SQL. Untersuchten wir zwei Strategien für die Anzeige von Daten in einer ASP.NET MVC-Anwendung. Erstens LINQ to SQL-Klassen erstellt und verwendet die Klassen direkt in eine Controlleraktion. Mithilfe von LINQ to SQL-Klassen in einem Controller ermöglicht es Ihnen, schnell und einfach Daten in einer MVC-Anwendung angezeigt.

Als Nächstes vorgestellt, die einen etwas schwieriger, aber definitiv mehr virtuous Pfad für die Anzeige von Datenbankdaten. Wir nutzten die Repository-Muster und alle unsere Datenbankzugriffslogik in einer separaten Repository-Klasse platziert. In unserem Controller haben wir unseren Code mit einer Schnittstelle anstelle einer konkreten Klasse geschrieben. Der Vorteil des Repositorymusters ist, dass wir können einfach datenzugriffstechnologien in der Zukunft ändern, und wir können unseren Controller-Klassen auf einfache Weise testen.

> [!div class="step-by-step"]
> [Zurück](creating-model-classes-with-the-entity-framework-cs.md)
> [Weiter](displaying-a-table-of-database-data-cs.md)
