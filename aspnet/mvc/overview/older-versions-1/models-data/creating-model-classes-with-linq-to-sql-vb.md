---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: Erstellen von Modellklassen mit LINQ to SQL) (VB) | Microsoft Docs
author: microsoft
description: Das Ziel dieses Lernprogramms wird eine Methode zum Erstellen von Modellklassen für eine ASP.NET MVC-Anwendung erläutert. In diesem Lernprogramm erfahren Sie, wie zum Modell c erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 5438838123c40d82afbda191a48878d6dca80736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874810"
---
<a name="creating-model-classes-with-linq-to-sql-vb"></a>Erstellen von Modellklassen mit LINQ to SQL) (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> Das Ziel dieses Lernprogramms wird eine Methode zum Erstellen von Modellklassen für eine ASP.NET MVC-Anwendung erläutert. In diesem Lernprogramm erfahren Sie, wie zum Erstellen von Modellklassen und Datenbankzugriff durch nutzen Microsoft LINQ to SQL ausführen.


Das Ziel dieses Lernprogramms wird eine Methode zum Erstellen von Modellklassen für eine ASP.NET MVC-Anwendung erläutert. In diesem Lernprogramm erfahren Sie, wie zum Erstellen von Modellklassen und Datenbankzugriff durch nutzen Microsoft LINQ to SQL ausführen.

In diesem Lernprogramm erstellen wir eine einfache Anwendung der Film-Datenbank. Erstellen zunächst wir die Film-datenbankanwendung in die schnellste und einfachste Möglichkeit möglich. Wir ausführen alle unsere Datenzugriff direkt aus unserem Controlleraktionen.

Als Nächstes erfahren Sie, wie das Repositorymuster verwenden. Verwendung des Repositorymusters erfordert etwas mehr Arbeit. Der Vorteil einer dieses Muster ist jedoch, dass er ermöglicht es Ihnen, Anwendungen zu erstellen, die angepasst werden zu ändern und können problemlos getestet werden kann.

## <a name="what-is-a-model-class"></a>Was ist eine Modellklasse?

Ein MVC-Modell enthält alle der Anwendungslogik, die nicht in einer MVC-Ansicht oder das MVC-Controller enthalten ist. Insbesondere enthält ein MVC-Modell all Ihre Anwendung Business und darauf von Datenzugriffslogik.

Eine Vielzahl von verschiedenen Technologien können Sie um Ihre Datenzugriffslogik zu implementieren. Beispielsweise können Sie Ihre Datenzugriffsklassen, die mithilfe der Microsoft Entity Framework, NHibernate, Subsonic oder ADO.NET Klassen erstellen.

In diesem Lernprogramm mit der ich LINQ to SQL abzufragen und die Datenbank zu aktualisieren. LINQ to SQL bietet Ihnen eine sehr einfache Methode der Interaktion mit einer Microsoft SQL Server-Datenbank. Allerdings ist es wichtig zu verstehen, dass das ASP.NET-MVC-Framework nicht in irgendeiner Weise zu LINQ to SQL gebunden ist. ASP.NET MVC ist mit alle datenzugriffstechnologie kompatibel.

## <a name="create-a-movie-database"></a>Erstellen Sie eine Film-Datenbank

In diesem Lernprogramm--um veranschaulichen, wie Sie Modellklassen--erstellen können erstellen wir eine einfache Film-datenbankanwendung. Der erste Schritt ist zum Erstellen einer neuen Datenbank. Mit der rechten Maustaste in der App\_Datenordner im Projektmappen-Explorer-Fenster, und wählen Sie die Menüoption **hinzufügen, neue Element**. Wählen Sie die SQL Server-Datenbank aus, geben Sie ihm den Namen MoviesDB.mdf, und klicken Sie auf die **hinzufügen** Schaltfläche (siehe Abbildung 1).


[![Hinzufügen einer neuen SQL Server-Datenbank](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Abbildung 01**: Hinzufügen einer neuen SQL Server-Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))


Nachdem Sie die neue Datenbank erstellt haben, können Sie die Datenbank öffnen, durch Doppelklicken auf die Datei MoviesDB.mdf in der App\_Datenordner. Durch Doppelklicken auf die Datei MoviesDB.mdf wird das Server-Explorer-Fenster geöffnet (siehe Abbildung 2).


|   | Das Server-Explorer-Fenster wird das Datenbank-Explorer-Fenster aufgerufen, wenn es sich bei Visual Web Developer verwenden. |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

[![Mithilfe des Server-Explorer-Fensters](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Abbildung 02**: mit dem Server-Explorer-Fenster ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))


Wir möchten unsere Datenbank eine Tabelle hinzufügen, die unsere Filme darstellt. Mit der rechten Maustaste in des Tabellen-Ordners, und wählen Sie die Menüoption **neue Tabelle hinzufügen**. Diese Option im Menü Tabellen-Designer öffnet (siehe Abbildung 3).


[![Mithilfe des Server-Explorer-Fensters](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Abbildung 03**: der Tabellen-Designer ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))


Wir müssen die folgenden Spalten mit unserer Datenbanktabelle hinzufügen:

| **Spaltenname** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | Int | False |
| Titel | Nvarchar(200)-Datentyp gepackt ist | False |
| Director | Nvarchar(50) | False |

Sie müssen zwei spezielle Aktionen auf den ID-Spalte ausgeführt werden. Zunächst müssen Sie die Id-Spalte als primäre Schlüsselspalte zu markieren, indem Sie die Spalte im Tabellen-Designer auswählen, und klicken auf das Symbol eines Schlüssels. LINQ to SQL erfordert, dass Sie die Primärschlüsselspalten an, wenn eingefügt oder aktualisiert werden, für die Datenbank ausführen.

Als Nächstes müssen Sie die Id-Spalte als eine Identity-Spalte zu markieren, durch Zuweisen des Werts Ja, um die **ist Identity** -Eigenschaft (siehe Abbildung 3). Eine Identitätsspalte ist eine Spalte, die automatisch eine neue Zahl zugewiesen wird, wenn Sie eine neue Zeile mit Daten zu einer Tabelle hinzufügen.

Nachdem Sie diese Änderungen vorgenommen haben, speichern Sie die Tabelle mit dem Namen TblMovie. Sie können die Tabelle speichern, indem Sie auf die Schaltfläche "Speichern".

## <a name="create-linq-to-sql-classes"></a>Erstellen von LINQ to SQL-Klassen

Unsere MVC-Modell enthält LINQ to SQL-Klassen, die die Datenbanktabelle TblMovie darstellen. Am einfachsten erstellen Sie diese LINQ to SQL-Klassen ist, wählen Sie zum Ordner Models Maustaste **hinzufügen, neue Element**, die LINQ to SQL-Klassen-Vorlage auswählen, benennen Sie den Klassen der Movie.dbml, und klicken Sie auf die **hinzufügen**Schaltfläche (siehe Abbildung 4).


[![Erstellen von LINQ to SQL-Klassen](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Abbildung 04**: Erstellen von LINQ to SQL-Klassen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))


Sofort, nachdem Sie die Film-LINQ to SQL-Klassen erstellt haben, wird der Object Relational Designer angezeigt. Sie können Datenbanktabellen ziehen, aus dem Server-Explorer-Fenster auf den Object Relational Designer zum Erstellen von LINQ to SQL-Klassen, die bestimmten Datenbanktabellen darstellen. Wir müssen die TblMovie Datenbanktabelle auf den Object Relational Designer (siehe Abbildung 4).


[![Verwenden den Object Relational Designer](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Abbildung 05**: verwenden den Object Relational Designer ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))


Standardmäßig erstellt der Object Relational Designer eine Klasse mit dem sehr denselben Namen wie der Datenbanktabelle, die Sie in den Designer ziehen. Wir möchte jedoch nicht unsere TblMovie Klasse aufrufen. Aus diesem Grund klicken Sie auf den Namen der Klasse in den Designer, und ändern Sie den Namen der Klasse zu Film.

Schließlich müssen Sie auf die **speichern** (Bild Diskettenlaufwerk)-Schaltfläche, um die LINQ to SQL-Klassen zu speichern. Andernfalls wird nicht die LINQ to SQL-Klassen von der Object Relational Designer generiert werden.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Verwenden von LINQ to SQL in eine Controlleraktion

Nun, da wir unsere LINQ to SQL-Klassen haben, können wir diese Klassen verwenden, zum Abrufen von Daten aus der Datenbank. In diesem Abschnitt erfahren Sie, wie Sie LINQ to SQL-Klassen direkt innerhalb eine Controlleraktion verwenden. Wir müssen die Liste von Filmen aus der Datenbanktabelle TblMovies in einer MVC-Ansicht anzuzeigen.

Zunächst müssen wir die HomeController-Klasse zu ändern. Diese Klasse kann im Ordner "Controller" der Anwendung gefunden werden. Ändern Sie die Klasse, sieht es wie im Codebeispiel 1-Klasse.

**Auflisten von 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

Die Index()-Aktion im Codebeispiel 1 verwendet eine LINQ to SQL-DataContext-Klasse (MovieDataContext), um MoviesDB darstellen. Die MoveDataContext-Klasse wurde von Visual Studio Object Relational Designer generiert.

Eine LINQ-Abfrage erfolgt für den DataContext hinzu, um alle Filme aus der Datenbanktabelle TblMovies abzurufen. Die Liste von Filmen wird eine lokale Variable namens Filme zugewiesen. Schließlich wird die Liste von Filmen durch Anzeigen von Daten an die Ansicht übergeben.

Um die Filme anzuzeigen, müssen wir als Nächstes die Indexansicht ändern. Sie finden die Indexansicht im Ordner "Views\Home\". Aktualisieren Sie die Indexansicht, sodass es die Ansicht im Codebeispiel 2 aussieht.

**Auflisten von 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Beachten Sie, das die geänderte Indexansicht enthält ein &lt;% @ Import Namespace %&gt; -Direktive am oberen Rand der Ansicht. Diese Direktive importiert den Namespace "mvcapplication1". Wir benötigen diesen Namespace, um mit der Modellklassen – insbesondere die Film-Klasse – in der Ansicht arbeiten.

Die Ansicht im Codebeispiel 2 enthält eine For Each-Schleife, die alle Elemente, dargestellt durch die Eigenschaft ViewData.Model durchläuft. Der Wert der Title-Eigenschaft ist für jede Film angezeigt.

Beachten Sie, dass der Wert der Eigenschaft ViewData.Model in ein IEnumerable umgewandelt wird. Dies ist erforderlich, um den Inhalt der ViewData.Model durchlaufen. Hier eine weitere Option ist eine stark typisierte Ansicht erstellen. Wenn Sie eine stark typisierte Ansicht erstellen, wandeln Sie die ViewData.Model-Eigenschaft auf einen bestimmten Typ in einer Ansicht Code-Behind-Klasse.

Wenn Sie die Anwendung nach dem Ändern von HomeController-Klasse und die Indexansicht ausgeführt werden, erhalten Sie eine leere Seite. Sie erhalten eine leere Seite, da keine Film-Datensätze in der Datenbanktabelle TblMovies vorliegen.

Um die Datenbanktabelle TblMovies Datensätze hinzugefügt haben, mit der rechten Maustaste in der Datenbanktabelle TblMovies im Server-Explorer-Fenster (Datenbank-Explorer-Fenster in Visual Web Developer), und wählen Sie die Menüoption **Tabellendaten anzeigen**. Sie können die Film Datensätze einfügen, mit dem Raster, das angezeigt wird (siehe Abbildung 5).


[![Einfügen von Filmen](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Abbildung 06**: Einfügen von Filmen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))


Nachdem Sie einige Datenbankdatensätze in die Tabelle TblMovies hinzufügen und Sie die Anwendung ausführen, sehen Sie die Seite in Abbildung 7. Alle Datensätze aus der Datenbank Film werden in einer Aufzählung angezeigt.


[![Anzeigen von Filmen mit die Indexansicht](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Abbildung 07**: Anzeigen von Filmen mit die Indexansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Verwendung des Repositorymusters

Im vorherigen Abschnitt haben wir die LINQ to SQL-Klassen direkt innerhalb eine Controlleraktion verwendet. Es verwendet die MovieDataContext-Klasse direkt aus Index() Controlleraktion. Es ist nichts dies bei einer einfachen Anwendung. Das direkte Arbeiten mit LINQ to SQL in einer Controllerklasse erstellt jedoch Probleme, wenn Sie eine komplexere Anwendung erstellen möchten.

Mit LINQ to SQL innerhalb einer Controllerklasse erschwert die Technologien für den Datenzugriff in der Zukunft zu wechseln. Sie könnten z. B. Microsoft LINQ to SQL Verwendung von Microsoft Entity Framework als Ihre datenzugriffstechnologie wechseln. In diesem Fall müssten Sie jede Controller neu zu schreiben, der Zugriff auf die Datenbank innerhalb der Anwendung.

Mit LINQ to SQL innerhalb einer Controllerklasse erschwert auch zum Erstellen von Komponententests für Ihre Anwendung. In der Regel möchten Sie nicht mit einer Datenbank interagieren, beim Ausführen von Komponententests. Sie möchten Komponententests verwenden, um die Anwendungslogik und nicht auf dem Datenbankserver zu testen.

Zum Erstellen einer MVC-Anwendung mehr anwendbar ist, um zukünftige ändern und, die einfacher getestet werden können, sollten Sie das Repositorymuster verwenden. Wenn Sie das Repositorymuster verwenden, erstellen Sie eine separate Repository-Klasse, die alle Ihre Datenbank-Access-Logik enthält.

Wenn Sie die Repository-Klasse erstellen, erstellen Sie eine Schnittstelle, die alle von der Repository-Klasse verwendeten Methoden darstellt. In Ihren Domänencontrollern schreiben Sie Code mit der Schnittstelle anstatt im Repository. Auf diese Weise können Sie das Repository in der Zukunft mithilfe von verschiedenen datenzugriffstechnologien implementieren.

Die Schnittstelle im Codebeispiel 3 heißt IMovieRepository, und stellt eine einzelne Methode namens ListAll() dar.

**Auflisten von 3: `Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

Auflisten von 4 die Repository-Klasse implementiert die IMovieRepository-Schnittstelle. Beachten Sie, dass er eine Methode mit dem Namen ListAll() enthält, das die Methode vorhanden sein, durch die IMovieRepository-Schnittstelle entspricht.

**Auflisten von 4 – `Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Schließlich wird die Klasse MoviesController Auflisten von 5 des Repositorymusters verwendet. Es verwendet nicht mehr LINQ to SQL-Klassen direkt.

**Auflisten von 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Beachten Sie, dass die Klasse MoviesController Auflisten von 5 zwei Konstruktoren aufweist. Der erste Konstruktor, der parameterlose Konstruktor wird aufgerufen, wenn die Anwendung ausgeführt wird. Dieser Konstruktor erstellt eine Instanz der Klasse MovieRepository und übergibt sie an der zweite Konstruktor.

Der zweite Konstruktor verfügt über einen einzelnen Parameter: ein IMovieRepository-Parameter. Dieser Konstruktor weist den Wert des Parameters einfach auf ein Feld auf Klassenebene \_Repository.

Die Klasse MoviesController ist ein Software-Entwurfsmuster wird aufgerufen, die Abhängigkeitsinjektion Muster ausnutzen. Insbesondere wird so genannte Abhängigkeitsinjektion Konstruktor verwendet. Erfahren Sie mehr zu diesem Muster im folgenden Artikel Smell lesen:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Beachten Sie, dass der gesamte Code in der Klasse MoviesController (mit Ausnahme von der erste Konstruktor) interagiert mit der Schnittstelle IMovieRepository anstelle der tatsächlichen MovieRepository-Klasse. Der Code interagiert mit einer abstrakten Schnittstelle anstatt eine konkrete Implementierung der Schnittstelle.

Wenn Sie die datenzugriffstechnologie, die von der Anwendung verwendeten ändern möchten können Sie einfach die IMovieRepository-Schnittstelle mit einer Klasse implementieren, die die alternative Datenbank-Access-Technologie verwendet. Sie konnten z. B. eine EntityFrameworkMovieRepository Klasse oder eine SubSonicMovieRepository erstellen. Da Controllerklasse mit der Schnittstelle so programmiert ist, können Sie eine neue Implementierung der IMovieRepository an die Controllerklasse übergeben und die Klasse weiterhin funktionieren.

Darüber hinaus, wenn Sie die Klasse MoviesController testen möchten, können dann Sie eine gefälschte Film-Repository-Klasse an die MoviesController übergeben. Sie können die IMovieRepository-Klasse mit einer Klasse implementieren, die greift nicht tatsächlich auf die Datenbank jedoch alle erforderlichen Methoden der Schnittstelle IMovieRepository enthält. Auf diese Weise können Sie Komponententests MoviesController Klasse ohne tatsächlich eine reale Datenbank zuzugreifen.

## <a name="summary"></a>Zusammenfassung

Das Ziel dieses Lernprogramms wurde veranschaulicht, wie Sie nutzen Microsoft-LINQ to SQL MVC Modellklassen erstellen können. Zwei Strategien zum Anzeigen von Daten in einer ASP.NET MVC-Anwendung untersucht. Erstens LINQ to SQL-Klassen erstellt und verwendet die Klassen direkt innerhalb eine Controlleraktion. Verwendung von LINQ to SQL-Klassen innerhalb eines Controllers ermöglicht es Ihnen, schnell und problemlos Datenbankdaten in einer MVC-Anwendung angezeigt.

Als Nächstes untersucht wir einen etwas schwieriger ist jedoch definitiv mehr virtuous Pfad für die Anzeige von Datenbankdaten. Wir haben genutzt, des Repositorymusters und alle unsere Datenbankzugriffslogik in einer separaten Repository-Klasse platziert werden. In unserem Controller haben wir alle unsere Code für eine Schnittstelle anstelle einer konkreten Klasse geschrieben. Der Vorteil des Repositorymusters ist, dass wir können einfach datenzugriffstechnologien in der Zukunft ändern und es uns erlaubt, unsere Controllerklassen problemlos zu testen.

> [!div class="step-by-step"]
> [Zurück](creating-model-classes-with-the-entity-framework-vb.md)
> [Weiter](displaying-a-table-of-database-data-vb.md)
