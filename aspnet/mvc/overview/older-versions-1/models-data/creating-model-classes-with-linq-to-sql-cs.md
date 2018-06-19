---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: Erstellen von Modellklassen mit LINQ to SQL (c#) | Microsoft Docs
author: microsoft
description: Das Ziel dieses Lernprogramms wird eine Methode zum Erstellen von Modellklassen für eine ASP.NET MVC-Anwendung erläutert. In diesem Lernprogramm erfahren Sie, wie zum Modell c erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: 9a56ceb9eab5774906ecc89ce9da570d4f691a82
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555312"
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>Erstellen von Modellklassen mit LINQ to SQL (c#)
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> Das Ziel dieses Lernprogramms wird eine Methode zum Erstellen von Modellklassen für eine ASP.NET MVC-Anwendung erläutert. In diesem Lernprogramm erfahren Sie, wie zum Erstellen von Modellklassen und Datenbankzugriff durch nutzen Microsoft LINQ to SQL ausführen.


Das Ziel dieses Lernprogramms wird eine Methode zum Erstellen von Modellklassen für eine ASP.NET MVC-Anwendung erläutert. In diesem Lernprogramm erfahren Sie, wie zum Erstellen von Modellklassen und Zugriff auf die Datenbank ausführen, indem Sie die Nutzung der Microsoft-LINQ to SQL

In diesem Lernprogramm erstellen wir eine einfache Anwendung der Film-Datenbank. Erstellen zunächst wir die Film-datenbankanwendung in die schnellste und einfachste Möglichkeit möglich. Wir ausführen alle unsere Datenzugriff direkt aus unserem Controlleraktionen.

Als Nächstes erfahren Sie, wie das Repositorymuster verwenden. Verwendung des Repositorymusters erfordert etwas mehr Arbeit. Der Vorteil einer dieses Muster ist jedoch, dass er ermöglicht es Ihnen, Anwendungen zu erstellen, die angepasst werden zu ändern und können problemlos getestet werden kann.

## <a name="what-is-a-model-class"></a>Was ist eine Modellklasse?

Ein MVC-Modell enthält alle der Anwendungslogik, die nicht in einer MVC-Ansicht oder das MVC-Controller enthalten ist. Insbesondere enthält ein MVC-Modell all Ihre Anwendung Business und darauf von Datenzugriffslogik.

Eine Vielzahl von verschiedenen Technologien können Sie um Ihre Datenzugriffslogik zu implementieren. Beispielsweise können Sie Ihre Datenzugriffsklassen, die mithilfe der Microsoft Entity Framework, NHibernate, Subsonic oder ADO.NET Klassen erstellen.

In diesem Lernprogramm mit der ich LINQ to SQL abzufragen und die Datenbank zu aktualisieren. LINQ to SQL bietet Ihnen eine sehr einfache Methode der Interaktion mit einer Microsoft SQL Server-Datenbank. Allerdings ist es wichtig zu verstehen, dass das ASP.NET-MVC-Framework nicht in irgendeiner Weise zu LINQ to SQL gebunden ist. ASP.NET MVC ist mit alle datenzugriffstechnologie kompatibel.

## <a name="create-a-movie-database"></a>Erstellen Sie eine Film-Datenbank

In diesem Lernprogramm--um veranschaulichen, wie Sie Modellklassen--erstellen können erstellen wir eine einfache Film-datenbankanwendung. Der erste Schritt ist zum Erstellen einer neuen Datenbank. Mit der rechten Maustaste in der App\_Datenordner im Projektmappen-Explorer-Fenster, und wählen Sie die Menüoption **hinzufügen, neue Element**. Wählen Sie die **SQL Server-Datenbank** Vorlage, geben Sie ihm den Namen MoviesDB.mdf, und klicken Sie auf die **hinzufügen** Schaltfläche (siehe Abbildung 1).


[![Hinzufügen einer neuen SQL Server-Datenbank](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Abbildung 01**: Hinzufügen einer neuen SQL Server-Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


Nachdem Sie die neue Datenbank erstellt haben, können Sie die Datenbank öffnen, durch Doppelklicken auf die Datei MoviesDB.mdf in der App\_Datenordner. Durch Doppelklicken auf die Datei MoviesDB.mdf wird das Server-Explorer-Fenster geöffnet (siehe Abbildung 2).

Das Server-Explorer-Fenster wird das Datenbank-Explorer-Fenster aufgerufen, wenn es sich bei Visual Web Developer verwenden.


[![Mithilfe des Server-Explorer-Fensters](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Abbildung 02**: mit dem Server-Explorer-Fenster ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


Wir möchten unsere Datenbank eine Tabelle hinzufügen, die unsere Filme darstellt. Mit der rechten Maustaste in des Tabellen-Ordners, und wählen Sie die Menüoption **neue Tabelle hinzufügen**. Diese Option im Menü Tabellen-Designer öffnet (siehe Abbildung 3).


[![Mithilfe des Server-Explorer-Fensters](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Abbildung 03**: der Tabellen-Designer ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


Wir müssen die folgenden Spalten mit unserer Datenbanktabelle hinzufügen:

| **Spaltenname** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | Int | False |
| Titel | Nvarchar(200)-Datentyp gepackt ist | False |
| Director | Nvarchar(50) | False |

Sie müssen zwei spezielle Aktionen auf den ID-Spalte ausgeführt werden. Zunächst müssen Sie die Id-Spalte als primäre Schlüsselspalte zu markieren, indem Sie die Spalte im Tabellen-Designer auswählen, und klicken auf das Symbol eines Schlüssels. LINQ to SQL erfordert, dass Sie die Primärschlüsselspalten an, wenn eingefügt oder aktualisiert werden, für die Datenbank ausführen.

Als Nächstes müssen Sie die Id-Spalte als eine Identity-Spalte zu markieren, durch Zuweisen des Werts Ja, um die **ist Identity** -Eigenschaft (siehe Abbildung 3). Eine Identitätsspalte ist eine Spalte, die automatisch eine neue Zahl zugewiesen wird, wenn Sie eine neue Zeile mit Daten zu einer Tabelle hinzufügen.

## <a name="create-linq-to-sql-classes"></a>Erstellen von LINQ to SQL-Klassen

Unsere MVC-Modell enthält LINQ to SQL-Klassen, die die Datenbanktabelle TblMovie darstellen. Am einfachsten erstellen Sie diese LINQ to SQL-Klassen ist, wählen Sie zum Ordner Models Maustaste **hinzufügen, neue Element**, die LINQ to SQL-Klassen-Vorlage auswählen, benennen Sie den Klassen der Movie.dbml, und klicken Sie auf die **hinzufügen**Schaltfläche (siehe Abbildung 4).


[![Erstellen von LINQ to SQL-Klassen](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Abbildung 04**: Erstellen von LINQ to SQL-Klassen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


Sofort, nachdem Sie die Film-LINQ to SQL-Klassen erstellt haben, wird der Object Relational Designer angezeigt. Sie können Datenbanktabellen ziehen, aus dem Server-Explorer-Fenster auf den Object Relational Designer zum Erstellen von LINQ to SQL-Klassen, die bestimmten Datenbanktabellen darstellen. Wir müssen die TblMovie Datenbanktabelle auf den Object Relational Designer (siehe Abbildung 5).


[![Verwenden den Object Relational Designer](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**Abbildung 05**: verwenden den Object Relational Designer ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


Standardmäßig erstellt der Object Relational Designer eine Klasse mit dem sehr denselben Namen wie der Datenbanktabelle, die Sie in den Designer ziehen. Allerdings möchten wir unsere Klasse aufrufen, `tblMovie`. Aus diesem Grund klicken Sie auf den Namen der Klasse in den Designer, und ändern Sie den Namen der Klasse zu Film.

Schließlich müssen Sie auf die **speichern** (Bild Diskettenlaufwerk)-Schaltfläche, um die LINQ to SQL-Klassen zu speichern. Andernfalls wird nicht die LINQ to SQL-Klassen von der Object Relational Designer generiert werden.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Verwenden von LINQ to SQL in eine Controlleraktion

Nun, da wir unsere LINQ to SQL-Klassen haben, können wir diese Klassen verwenden, zum Abrufen von Daten aus der Datenbank. In diesem Abschnitt erfahren Sie, wie Sie LINQ to SQL-Klassen direkt innerhalb eine Controlleraktion verwenden. Wir müssen die Liste von Filmen aus der Datenbanktabelle TblMovies in einer MVC-Ansicht anzuzeigen.

Zunächst müssen wir die HomeController-Klasse zu ändern. Diese Klasse kann im Ordner "Controller" der Anwendung gefunden werden. Ändern Sie die Klasse, sieht es wie im Codebeispiel 1-Klasse.

**Auflisten von 1 – `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

Die `Index()` Aktion im Codebeispiel 1 verwendet eine LINQ to SQL-DataContext-Klasse (die `MovieDataContext`) zur Darstellung der `MoviesDB` Datenbank. Die `MoveDataContext` Klasse durch den Visual Studio Object Relational Designer generiert wurde.

Eine LINQ-Abfrage wird ausgeführt, für den DataContext hinzu, zum Abrufen aller Filme aus der `tblMovies` Datenbanktabelle. Die Liste von Filmen wird zugewiesen, um eine lokale Variable namens `movies`. Schließlich wird die Liste von Filmen durch Anzeigen von Daten an die Ansicht übergeben.

Um die Filme anzuzeigen, müssen wir als Nächstes die Indexansicht ändern. Sie finden die Indexansicht in den `Views\Home\` Ordner. Aktualisieren Sie die Indexansicht, sodass es die Ansicht im Codebeispiel 2 aussieht.

**Auflisten von 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Beachten Sie, das die geänderte Indexansicht enthält ein `<%@ import namespace %>` -Direktive am oberen Rand der Ansicht. Diese Direktive importiert der `MvcApplication1.Models namespace`. Wir benötigen diesen Namespace für die Arbeit mit der `model` Klassen – insbesondere die `Movie` Klasse--in der Ansicht.

Die Ansicht im Codebeispiel 2 enthält einen `foreach` -Schleife, die alle Elemente dargestellte durchläuft die `ViewData.Model` Eigenschaft. Der Wert, der die `Title` Eigenschaft wird angezeigt, für die einzelnen `movie`.

Beachten Sie, dass der Wert der `ViewData.Model` Eigenschaft umgewandelt wird ein `IEnumerable`. Dies ist erforderlich, um den Inhalt des durchlaufen `ViewData.Model`. Hier eine andere Möglichkeit ist die Erstellung einer stark typisierten `view`. Beim Erstellen einer stark typisierten `view`, wandeln Sie die `ViewData.Model` Eigenschaft auf einen bestimmten Typ in einer Ansicht Code-Behind-Klasse.

Wenn das Ausführen der Anwendung nach dem Ändern der `HomeController` Klasse und den Index anzeigen, erhalten Sie eine leere Seite. Sie erhalten eine leere Seite, da es keine Film-Datensätze in gibt der `tblMovies` Datenbanktabelle.

Zum Hinzufügen von Datensätzen, die `tblMovies` Datenbanktabelle der rechten Maustaste auf die `tblMovies` Datenbanktabelle im Server-Explorer-Fenster (Datenbank-Explorer-Fenster in Visual Web Developer), und wählen Sie die Menüoption Tabellendaten anzeigen. Sie können einfügen `movie` Datensätze mithilfe des Rasters, der angezeigt wird (siehe Abbildung 6).


[![Einfügen von Filmen](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Abbildung 06**: Einfügen von Filmen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


Nach dem Hinzufügen von einigen Datenbank-Datensätze auf den `tblMovies` Tabelle, und Sie die Anwendung ausführen, sehen Sie die Seite in Abbildung 7. Alle Datensätze aus der Datenbank Film werden in einer Aufzählung angezeigt.


[![Anzeigen von Filmen mit die Indexansicht](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Abbildung 07**: Anzeigen von Filmen mit die Indexansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Verwendung des Repositorymusters

Im vorherigen Abschnitt haben wir die LINQ to SQL-Klassen direkt innerhalb eine Controlleraktion verwendet. Es verwendet die `MovieDataContext` -Klasse direkt aus der `Index()` Controlleraktion. Es ist nichts dies bei einer einfachen Anwendung. Das direkte Arbeiten mit LINQ to SQL in einer Controllerklasse erstellt jedoch Probleme, wenn Sie eine komplexere Anwendung erstellen möchten.

Mit LINQ to SQL innerhalb einer Controllerklasse erschwert die Technologien für den Datenzugriff in der Zukunft zu wechseln. Sie könnten z. B. Microsoft LINQ to SQL Verwendung von Microsoft Entity Framework als Ihre datenzugriffstechnologie wechseln. In diesem Fall müssten Sie jede Controller neu zu schreiben, der Zugriff auf die Datenbank innerhalb der Anwendung.

Mit LINQ to SQL innerhalb einer Controllerklasse erschwert auch zum Erstellen von Komponententests für Ihre Anwendung. In der Regel möchten Sie nicht mit einer Datenbank interagieren, beim Ausführen von Komponententests. Sie möchten Komponententests verwenden, um die Anwendungslogik und nicht auf dem Datenbankserver zu testen.

Zum Erstellen einer MVC-Anwendung mehr anwendbar ist, um zukünftige ändern und, die einfacher getestet werden können, sollten Sie das Repositorymuster verwenden. Wenn Sie das Repositorymuster verwenden, erstellen Sie eine separate Repository-Klasse, die alle Ihre Datenbank-Access-Logik enthält.

Wenn Sie die Repository-Klasse erstellen, erstellen Sie eine Schnittstelle, die alle von der Repository-Klasse verwendeten Methoden darstellt. In Ihren Domänencontrollern schreiben Sie Code mit der Schnittstelle anstatt im Repository. Auf diese Weise können Sie das Repository in der Zukunft mithilfe von verschiedenen datenzugriffstechnologien implementieren.

Die Schnittstelle im Codebeispiel 3 heißt `IMovieRepository` und es stellt eine einzelne Methode namens `ListAll()`.

**Auflisten von 3: `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

Auflisten von 4 der Repository-Klasse implementiert die `IMovieRepository` Schnittstelle. Beachten Sie, dass es sich um eine Methode namens enthält `ListAll()` , entspricht der Methode vorhanden sein, durch die `IMovieRepository` Schnittstelle.

**Auflisten von 4 – `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Schließlich die `MoviesController` Klasse auflisten 5 verwendet das Repositorymuster. Es verwendet nicht mehr LINQ to SQL-Klassen direkt.

**Auflisten von 5 – `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Beachten Sie, dass die `MoviesController` -Klasse auflisten 5 verfügt über zwei Konstruktoren. Der erste Konstruktor, der parameterlose Konstruktor wird aufgerufen, wenn die Anwendung ausgeführt wird. Dieser Konstruktor erstellt eine Instanz von der `MovieRepository` -Klasse und übergibt sie an der zweite Konstruktor.

Der zweite Konstruktor verfügt über einen einzelnen Parameter: eine `IMovieRepository` Parameter. Dieser Konstruktor weist den Wert des Parameters einfach auf ein Feld auf Klassenebene `_repository`.

Die `MoviesController` Klasse ist die Nutzung von ein Software-Entwurfsmuster, die die Abhängigkeitsinjektion Muster aufgerufen. Insbesondere wird so genannte Abhängigkeitsinjektion Konstruktor verwendet. Erfahren Sie mehr zu diesem Muster im folgenden Artikel Smell lesen:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Beachten Sie, dass der gesamte Code in der `MoviesController` Klasse (mit Ausnahme von der erste Konstruktor) interagiert mit den `IMovieRepository` Schnittstelle anstelle des tatsächlichen `MovieRepository` Klasse. Der Code interagiert mit einer abstrakten Schnittstelle anstatt eine konkrete Implementierung der Schnittstelle.

Wenn Sie die datenzugriffstechnologie, die von der Anwendung verwendeten ändern möchten, und klicken Sie dann Sie einfach implementieren die `IMovieRepository` Schnittstelle mit einer Klasse, die alternative Datenbank-Access-Technologie verwendet. Sie können z. B. Erstellen einer `EntityFrameworkMovieRepository` Klasse oder eine `SubSonicMovieRepository` Klasse. Da die Controllerklasse mit der Schnittstelle so programmiert ist, können Sie eine neue Implementierung übergeben `IMovieRepository` an den Controller-Klasse und die Klasse würde funktionieren weiterhin.

Darüber hinaus, wenn Sie testen möchten die `MoviesController` Klasse, und klicken Sie dann eine gefälschte Film-Repository-Klasse übergeben werden können die `HomeController`. Sie implementieren können die `IMovieRepository` mit einer Klasse, die nicht tatsächlich Zugriff der Datenbank enthält alle erforderlichen Methoden der Klasse die `IMovieRepository` Schnittstelle. Auf diese Weise können Sie Komponententests der `MoviesController` Klasse ohne tatsächlich eine reale Datenbank zuzugreifen.

## <a name="summary"></a>Zusammenfassung

Das Ziel dieses Lernprogramms wurde veranschaulicht, wie Sie nutzen Microsoft-LINQ to SQL MVC Modellklassen erstellen können. Zwei Strategien zum Anzeigen von Daten in einer ASP.NET MVC-Anwendung untersucht. Erstens LINQ to SQL-Klassen erstellt und verwendet die Klassen direkt innerhalb eine Controlleraktion. Verwendung von LINQ to SQL-Klassen innerhalb eines Controllers ermöglicht es Ihnen, schnell und problemlos Datenbankdaten in einer MVC-Anwendung angezeigt.

Als Nächstes untersucht wir einen etwas schwieriger ist jedoch definitiv mehr virtuous Pfad für die Anzeige von Datenbankdaten. Wir haben genutzt, des Repositorymusters und alle unsere Datenbankzugriffslogik in einer separaten Repository-Klasse platziert werden. In unserem Controller haben wir alle unsere Code für eine Schnittstelle anstelle einer konkreten Klasse geschrieben. Der Vorteil des Repositorymusters ist, dass wir können einfach datenzugriffstechnologien in der Zukunft ändern und es uns erlaubt, unsere Controllerklassen problemlos zu testen.

> [!div class="step-by-step"]
> [Zurück](creating-model-classes-with-the-entity-framework-cs.md)
> [Weiter](displaying-a-table-of-database-data-cs.md)
