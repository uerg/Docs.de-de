---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Hinzufügen eines neuen Felds zum Modell "Movie" und zur Tabelle | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Tutorials ist hier verfügbar, dass das ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist eine sicherere, viel einfacher zu folgen und demo...'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 2435bb50bb8124cce3150ba488ad76c012107b27
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840174"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Hinzufügen eines neuen Felds zum Modell "Movie" und zur Tabelle
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials finden Sie [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Dabei handelt es sich eine sicherere, einfacher, führen weitere Funktionen veranschaulicht.


In diesem Abschnitt verwenden Sie Entity Framework Code First-Migrationen, migrieren einige Änderungen in der ViewModel-Klassen, damit die Änderung auf die Datenbank angewendet wird.

In der Standardeinstellung, wenn Sie Entity Framework Code First verwenden, um automatisch eine Datenbank erstellen, wie weiter oben in diesem Tutorial Code First eine Tabelle der Datenbank hinzugefügt, um nachzuverfolgen, ob das Schema der Datenbank mit den Modellklassen synchron ist, die sie aus generiert wurde. Wenn sie nicht synchron sind, löst Entity Framework einen Fehler aus. Dies erleichtert es, die zum Zeitpunkt der Entwicklung Identifizieren von Problemen, die andernfalls nur (durch ungewöhnliche Fehler) zur Laufzeit auftreten.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Zum Einrichten von Code First-Migrationen Änderungen des Datenmodells

Wenn Sie Visual Studio 2012 verwenden, doppelklicken klicken Sie auf die *Movies.mdf* Datei im Projektmappen-Explorer, um das Tool zu öffnen. Visual Studio Express für Web wird Datenbank-Explorer angezeigt, Visual Studio 2012 Server-Explorer angezeigt werden. Wenn Sie Visual Studio 2010 verwenden, verwenden Sie die Objekt-Explorer von SQL Server.

Im Datenbanktool (Datenbank-Explorer, Server-Explorer oder Objekt-Explorer von SQL Server), klicken Sie mit der rechten Maustaste auf `MovieDBContext` , und wählen Sie **löschen** Filme Datenbank zu löschen.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Navigieren Sie zurück zum Projektmappen-Explorer. Klicken Sie mit der rechten Maustaste auf die *Movies.mdf* und wählen Sie **löschen** zum Entfernen der Datenbank Filme.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Erstellen Sie die Anwendung aus, um sicherzustellen, dass keine Fehler vorliegen.

Von der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager** und dann **-Paket-Manager-Konsole**.

![Pack Man hinzufügen](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

In der **-Paket-Manager-Konsole** Fenster auf die `PM>` Eingabeaufforderung Geben Sie "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

Die **Enable-Migrations** (siehe oben) Befehl erstellt eine *Configuration.cs* Datei in einem neuen *Migrationen* Ordner.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio öffnet die *Configuration.cs* Datei. Ersetzen Sie die `Seed` -Methode in der die *Configuration.cs* -Datei mit den folgenden Code:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Klicken Sie mit der rechten Maustaste auf die rote Wellenlinie unter `Movie` , und wählen Sie **beheben** dann **mit** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Auf diese Weise fügt die folgenden using-Anweisung:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First-Migrationen Ruft die `Seed` -Methode auf, nachdem jede Migration (d. h. Aufrufen **Update-Database** in der Paket-Manager-Konsole), und diese Methode aktualisiert die Zeilen, die bereits eingefügt wurde, oder fügt sie an, wenn sie nicht noch vorhanden ist.


**Drücken Sie STRG + UMSCHALT + B, um das Projekt erstellen.** (Die folgenden Schritte aus, schlägt fehl, wenn Ihre lassen sich nicht an diesem Punkt erstellen.)

Der nächste Schritt ist die Erstellung einer `DbMigration` -Klasse für die anfängliche Migration. Diese Migration zur erstellt einer neuen Datenbank, die daher wird Sie gelöscht der *movie.mdf* -Datei in einem vorherigen Schritt.

In der **-Paket-Manager-Konsole** Fenster, geben Sie den Befehl "add-Migration Initial" zum Erstellen der anfänglichen Migrations. Der Name "Initial" ist willkürlich und wird verwendet, um die Namen der Migrationsdatei erstellt.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Code First-Migrationen wird eine weitere Klassendatei in der *Migrationen* Ordner (mit dem Namen *{Datumsstempel}\_Initial.cs* ), und diese Klasse enthält Code, der das Schema der Datenbank erstellt. Der Dateiname für die Migration ist bereits mit einem Zeitstempel fest, um mit der Sortierung zu. Überprüfen Sie die *{Datumsstempel}\_Initial.cs* -Datei, er enthält Anweisungen zum Erstellen der Filme-Tabelle für die Movie-Datenbank. Beim Aktualisieren der Datenbank in den Anweisungen unten, diese *{Datumsstempel}\_Initial.cs* Datei ausgeführt wird und das Datenbankschema zu erstellen. Die **Ausgangswert** Methode wird ausgeführt, um die Datenbank mit Testdaten aufzufüllen.

In der **-Paket-Manager-Konsole**, geben Sie den Befehl "Update-Database" zum Erstellen der Datenbank und Ausführen der **Ausgangswert** Methode.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Wenn Sie eine Fehlermeldung, der angibt erhalten, eine Tabelle ist bereits vorhanden und kann nicht erstellt werden, es ist wahrscheinlich, da Sie die Anwendung ausgeführt, nachdem Sie die Datenbank gelöscht und bevor Sie ausgeführt `update-database`. In diesem Fall löschen Sie die *Movies.mdf* -Datei erneut, und wiederholen Sie den `update-database` Befehl. Wenn Sie weiterhin eine Fehlermeldung erhalten, löschen Sie den Ordner "Migrations" und den Inhalt, und beginnen Sie mit den Anweisungen am oberen Rand dieser Seite (d.h. Löschen der *Movies.mdf* -Datei, und fahren Sie mit der Enable-Migrations).

Führen Sie die Anwendung, und navigieren Sie zu der */Movies* URL. Die Seed-Daten werden angezeigt.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Hinzufügen einer Rating-Eigenschaft zum Movie-Modell

Starten Sie durch Hinzufügen eines neuen `Rating` Eigenschaft, um die vorhandene `Movie` Klasse. Öffnen der *Models\Movie.cs* -Datei und fügen die `Rating` Eigenschaft dieser Art:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

Die vollständige `Movie` -Klasse sieht wie im folgenden Code:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Erstellen der Anwendung mithilfe der **erstellen** &gt; **erstellen Film** Menü Befehl oder durch Drücken der STRG-UMSCHALT-b.

Nun, dass Sie aktualisiert haben die `Model` -Klasse, Sie müssen auch zum Aktualisieren der *\Views\Movies\Index.cshtml* und *\Views\Movies\Create.cshtml* Vorlagen anzeigen, um die neue anzuzeigen`Rating`Eigenschaft in der Browseransicht.

Öffnen der<em>\Views\Movies\Index.cshtml</em> -Datei und fügen eine `<th>Rating</th>` Spaltenüberschrift direkt nach der <strong>Preis</strong> Spalte. Fügen Sie dann eine `<td>` Spalte am Ende der Vorlage zum Rendern der `@item.Rating` Wert. Im folgenden finden Sie welche die aktualisierte <em>"Index.cshtml"</em> ansichtsvorlage sieht wie folgt aus:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Öffnen Sie als Nächstes die *\Views\Movies\Create.cshtml* Datei, und fügen Sie das folgende Markup in der Nähe des Formulars. Dies rendert ein Textfeld, sodass Sie eine Bewertung angeben können, wenn Sie ein neuer Film erstellt wird.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Sie haben jetzt den Anwendungscode so, dass die Unterstützung der neuen `Rating` Eigenschaft.

Die Anwendung jetzt ausführen, und navigieren Sie zu der */Movies* URL. Wenn Sie dies tun, wird jedoch eine der folgenden Fehlermeldungen angezeigt:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Sie sehen diesen Fehler, da die aktualisierte `Movie` Modellklasse, die in der Anwendung unterscheidet sich von dem Schema der der `Movie` Tabelle der vorhandenen Datenbank. (Die Datenbanktabelle enthält nicht die Spalte `Rating`.)


Es gibt mehrere Vorgehensweisen zum Beheben des Fehlers:

1. Lassen Sie die Datenbank von Entity Framework automatisch löschen und basierend auf dem neuen Modellklassenschema neu erstellen. Dieser Ansatz ist sehr praktisch, bei der aktiven Entwicklung in einer Testdatenbank; Sie können das Modell und das Datenbankschema schnell gemeinsam weiterzuentwickeln. Der Nachteil ist jedoch, dass Sie in der Datenbank vorhandene Daten verloren gehen, damit Sie *nicht* dieser Ansatz in einer Produktionsdatenbank verwendet werden soll. Verwenden eines Initialisierers ist zum automatisch Seeding für eine Datenbank mit Testdaten häufig eine produktive Möglichkeit zum Entwickeln einer Anwendung. Weitere Informationen zu Entity Framework-Datenbankinitialisierer, finden Sie unter der Tom Dykstra [ASP.NET MVC/Entity Framework-Tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Ändern Sie das Schema der vorhandenen Datenbank explizit so, dass es mit den Modellklassen übereinstimmt. Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten behalten. Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.
3. Verwenden Sie Code First-Migrationen, um das Datenbankschema zu aktualisieren.


Für dieses Tutorial verwenden wir Code First-Migrationen.

Aktualisieren Sie die Seed-Methode, sodass sie einen Wert für die neue Spalte bereitstellt. Öffnen Sie migrations\configuration. cs-Datei, und fügen ein Bewertungsfeld jedes Movie-Objekt hinzu.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Erstellen Sie die Projektmappe, und öffnen Sie die **-Paket-Manager-Konsole** Fenster, und geben Sie den folgenden Befehl aus:

`add-migration AddRatingMig`

Die `add-migration` Befehl weist das migrationsframework an das aktuelle Movie-Modell mit dem aktuellen Film DB Schema überprüfen und den erforderlichen Code zum Migrieren der Datenbank in das neue Modell zu erstellen. Die AddRatingMig ist willkürlich und wird verwendet, um die Migrationsdatei zu benennen. Es ist hilfreich, einen aussagekräftigen Namen für den Migrationsschritt zu verwenden.

Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMIgration` abgeleitete Klasse sein, und klicken Sie in der `Up` Methode sehen Sie den Code, der die neue Spalte erstellt.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Erstellen Sie die Projektmappe, und geben Sie dann den Befehl "Update-Database" in der **-Paket-Manager-Konsole** Fenster.

Die folgende Abbildung zeigt die Ausgabe in die **-Paket-Manager-Konsole** Fenster (der Datumsstempel voranstellen AddRatingMig unterscheiden.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Führen Sie die Anwendung erneut aus, und navigieren Sie zu der URL /Movies. Sie sehen, dass das neue Feld für die Bewertung.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Klicken Sie auf die **neu erstellen** Link, um einen neuen Film hinzuzufügen. Beachten Sie, dass Sie eine Bewertung hinzufügen können.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Klicken Sie auf **Erstellen**. Der neue Film, einschließlich "Rating", wird jetzt in den Filmen auflisten:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Sie sollten auch hinzufügen, die `Rating` Datumsfeld in den Bearbeitungsmodus, Details und SearchIndex Vorlagen anzeigen.

Könnten Sie eingeben, den Befehl "Update-Database" in der **-Paket-Manager-Konsole** Fenster erneut und keine Änderungen würden vorgenommen werden, da das Modell mit dem Schema übereinstimmt.

In diesem Abschnitt wurde erläutert, wie Sie Modellobjekte ändern und die Datenbank mit den Änderungen synchron zu halten. Sie haben zudem eine Möglichkeit, eine neu erstellte Datenbank mit Beispieldaten aufzufüllen, damit Sie die Szenarien ausprobieren können. Als Nächstes sehen wir uns, wie Sie die Modellklassen umfangreichere Validierungslogik hinzugefügt und einige Geschäftsregeln erzwungen werden zu aktivieren.

> [!div class="step-by-step"]
> [Zurück](examining-the-edit-methods-and-edit-view.md)
> [Weiter](adding-validation-to-the-model.md)
