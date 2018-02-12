---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: "Hinzufügen eines neuen Felds in die Film-Modell und die Tabelle | Microsoft Docs"
author: Rick-Anderson
description: "Hinweis: Eine aktualisierte Version dieses Lernprogramms ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher zu verfolgen und demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 9965c8a755857a8e8cb8ecbc6c467a6c856aa83d
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Das Movie-Modell und die Tabelle hinzugefügt ein neues Feld
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Lernprogramms steht [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.


In diesem Abschnitt verwenden Sie Entity Framework Code First-Migrationen zu einige Änderungen an der Modellklassen migrieren, damit die Änderung auf die Datenbank angewendet wird.

Standardmäßig Wenn Sie Entity Framework Code First verwenden, um automatisch eine Datenbank erstellen, wie Sie weiter oben in diesem Lernprogramm ausgeführt haben Code First eine Tabelle der Datenbank hinzugefügt, um nachzuverfolgen, ob das Schema der Datenbank mit der Modellklassen synchron ist, die sie aus generiert wurde. Wenn sie nicht synchron sind, löst das Entity Framework einen Fehler auf. Dies erleichtert das Identifizieren von Problemen zur Entwicklungszeit zu verfolgen, die andernfalls nur (durch kryptische Fehler) zur Laufzeit auftreten.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Einrichten von Code First-Migrationen für Modelländerungen

Wenn Sie Visual Studio 2012 verwenden, doppelklicken klicken Sie auf die *Movies.mdf* Datei im Projektmappen-Explorer, um das Datenbanktool zu öffnen. Visual Studio Express für Web wird Datenbank-Explorer anzeigen, Visual Studio 2012 Server-Explorer angezeigt werden. Wenn Sie Visual Studio 2010 verwenden, verwenden Sie SQL Server-Objekt-Explorer.

In der Datenbanktool (Datenbank-Explorer, Server-Explorer oder Objekt-Explorer von SQL Server), klicken Sie mit der mit der rechten Maustaste auf `MovieDBContext` , und wählen Sie **löschen** beim Löschen der Datenbank für Filme.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Navigieren Sie zurück zum Projektmappen-Explorer. Klicken Sie mit der rechten Maustaste auf die *Movies.mdf* Datei, und wählen Sie **löschen** zum Entfernen der Datenbank für Filme.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Erstellen Sie die Anwendung aus, um sicherzustellen, dass keine Fehler vorliegen.

Aus der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager** und dann **Package Manager Console**.

![Pack Man hinzufügen](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

In der **Package Manager Console** Fensters auf die `PM>` Eingabeaufforderung Geben Sie "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

Die **Enable-Migrations** (siehe oben) Befehl erstellt eine *"Configuration.cs"* Datei in einem neuen *Migrationen* Ordner.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio öffnet die *"Configuration.cs"* Datei. Ersetzen Sie die `Seed` Methode in der *"Configuration.cs"* Datei durch den folgenden Code:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Klicken Sie mit der rechten Maustaste auf die rote Wellenlinie unter `Movie` , und wählen Sie **beheben** dann **mit** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Auf diese Weise fügt die folgende Anweisung:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First-Migrationen Ruft die `Seed` Methode nach jeder Migration (d. h. Aufrufen **Update-Database '** in der Paket-Manager-Konsole), und diese Methode aktualisiert die Zeilen, die bereits eingefügt wurde, oder fügt sie an, wenn sie nicht noch vorhanden ist.


**Drücken Sie STRG-UMSCHALT + B, um das Projekt zu erstellen.** (Die folgenden Schritte aus, schlägt fehl, wenn die nicht zu diesem Zeitpunkt erstellen.)

Der nächste Schritt ist die Erstellung einer `DbMigration` der anfänglichen Migration-Klasse. Diese Migration zur erstellt einer neuen Datenbank, die verdeutlichen, warum Sie gelöschte der *movie.mdf* Datei in einem vorherigen Schritt.

In der **Package Manager Console** Fenster, geben Sie den Befehl "hinzufügen Migration anfängliche" zum Erstellen der anfänglichen Migrations. Der Name "Initial" ist ein beliebiger und wird verwendet, um die Namen der Migrationsdatei erstellt.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Code First-Migrationen erstellt einen anderen Klassendatei in der *Migrationen* Ordner (mit dem Namen *{Datumsstempel}\_Initial.cs* ), und diese Klasse enthält Code, der das Schema der Datenbank erstellt. Der Dateiname für die Migration ist bereits mit einem Zeitstempel fest, um mit der Reihenfolge zu. Überprüfen Sie die *{Datumsstempel}\_Initial.cs* Datei, er enthält eine Anleitung zum Erstellen der Filme-Tabelle für die Film-DB. Beim Aktualisieren der Datenbank in den Anweisungen unten, dies *{Datumsstempel}\_Initial.cs* Datei ausgeführt wird und der DB-Schema erstellen. Die **Ausgangswert** Methode zum Auffüllen der Datenbank mit Testdaten ausgeführt wird.

In der **Package Manager Console**, geben Sie den Befehl "Update-Database" zum Erstellen der Datenbank, und führen Sie die **Ausgangswert** Methode.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Wenn Sie eine Fehlermeldung, der angibt erhalten, eine Tabelle ist bereits vorhanden und kann nicht erstellt werden, es ist wahrscheinlich, da die Anwendung ausgeführt wird, nachdem Sie die Datenbank gelöscht und bevor Sie ausgeführt `update-database`. In diesem Fall löschen Sie die *Movies.mdf* Datei erneut, und wiederholen Sie den `update-database` Befehl. Wenn Sie weiterhin eine Fehlermeldung erhalten, löschen Sie den Ordner und den Inhalt dann mit den Anweisungen am oberen Rand dieser Seite beginnen (also Löschen der *Movies.mdf* Datei, und klicken Sie dann den Vorgang fortzusetzen, Enable-Migrations).

Führen Sie die Anwendung, und navigieren Sie zu der */Movies* URL. Die Seed-Daten werden angezeigt.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Hinzufügen einer Rating-Eigenschaft zum Movie-Modell

Starten, indem er ein neues `Rating` Eigenschaft, um die vorhandene `Movie` Klasse. Öffnen der *Models\Movie.cs* und fügen die `Rating` wie die folgende Eigenschaft:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

Die vollständige `Movie` -Klasse sieht wie der folgende Code:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Erstellen der Anwendung über die **erstellen** &gt; **Film erstellen** Menü den Befehl oder durch Drücken von STRG-UMSCHALT-b.

Nun, dass Sie aktualisiert haben die `Model` -Klasse, Sie müssen auch zum Aktualisieren der *\Views\Movies\Index.cshtml* und *\Views\Movies\Create.cshtml* Vorlagen anzeigen, um die neue anzuzeigen`Rating`Eigenschaft in der Browseransicht.

Öffnen der*\Views\Movies\Index.cshtml* und fügen eine `<th>Rating</th>` Spaltenüberschrift direkt nach der **Preis** Spalte. Fügen Sie dann eine `<td>` Spalte am Ende der Vorlage zum Rendern der `@item.Rating` Wert. Im folgenden finden Sie welche die aktualisierte *Index.cshtml* Vorlage anzeigen sieht wie folgt aus:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Öffnen Sie als Nächstes die *\Views\Movies\Create.cshtml* Datei, und fügen Sie das folgende Markup gegen Ende des Formulars. Dies rendert ein Textfeld, sodass Sie eine Bewertung angeben können, wenn ein neue Film erstellt wird.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Sie haben jetzt den Code der Anwendung, um die Unterstützung der neuen `Rating` Eigenschaft.

Die Anwendung jetzt ausführen, und navigieren Sie zu der */Movies* URL. Wenn Sie dies tun, wird jedoch eine der folgenden Fehlermeldungen angezeigt:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Dieser Fehler gemeldet wird, da die aktualisierte `Movie` Modellklasse in der Anwendung unterscheidet sich von dem Schema der der `Movie` Tabelle der vorhandenen Datenbank. (Die Datenbanktabelle enthält nicht die Spalte `Rating`.)


Es gibt mehrere Vorgehensweisen zum Beheben des Fehlers:

1. Lassen Sie die Datenbank von Entity Framework automatisch löschen und basierend auf dem neuen Modellklassenschema neu erstellen. Dieser Ansatz ist sehr praktisch, bei der aktiven Entwicklung auf eine Testdatenbank; Sie können das Modell und Datenbank-Schema schnell zusammen weiterentwickelt. Der Nachteil ist jedoch, dass Sie in der Datenbank vorhandene Daten verloren gehen – damit Sie *nicht* dieser Ansatz in einer Produktionsdatenbank verwendet werden soll. Automatisch Ausgangswert für eine Datenbank mit Testdaten mit einem Initialisierer ist häufig eine produktive Weg zur Entwicklung einer Anwendung. Weitere Informationen zu Entity Framework Database Initialisierer, finden Sie unter der Tom Dykstra [ASP.NET MVC/Entity Framework-Lernprogramm](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Ändern Sie das Schema der vorhandenen Datenbank explizit so, dass es mit den Modellklassen übereinstimmt. Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten behalten. Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.
3. Verwenden Sie Code First-Migrationen, um das Datenbankschema zu aktualisieren.


Für dieses Tutorial verwenden wir Code First-Migrationen.

Aktualisieren Sie Seed-Methode, sodass sie einen Wert für die neue Spalte bereitstellt. Öffnen Sie Migrations\Configuration.cs-Datei und fügen Sie ein Bewertungsfeld auf jedes Movie-Objekt.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Erstellen Sie die Projektmappe, und öffnen Sie die **Package Manager Console** Fenster, und geben Sie den folgenden Befehl aus:

`add-migration AddRatingMig`

Die `add-migration` Befehl weist das Framework Migration, überprüfen das aktuelle Film-Modell mit dem aktuellen Film DB Schema und den erforderlichen Code zum Migrieren der Datenbank in das neue Modell zu erstellen. Die AddRatingMig ist willkürlich und wird verwendet, um die Migrationsdatei angegeben werden. Es ist hilfreich, einen aussagekräftigen Namen für den Schritt der Migration zu verwenden.

Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMIgration` abgeleitete Klasse, und klicken Sie in der `Up` Methode sehen Sie den Code, der die neue Spalte erstellt.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Erstellen Sie die Projektmappe, und geben Sie dann den Befehl "Update-Database" in der **Package Manager Console** Fenster.

Die folgende Abbildung zeigt die Ausgabe in die **Package Manager Console** Fenster (der Datumsstempel vorangestellt wird AddRatingMig unterscheiden.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Führen Sie die Anwendung erneut aus, und navigieren Sie zu der URL /Movies. Sehen Sie die neue Rating-Felds an.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Klicken Sie auf die **neu erstellen** Link, um einen neuen Film hinzufügen. Beachten Sie, dass Sie eine Bewertung hinzufügen können.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Klicken Sie auf **Erstellen**. Der neue Film, einschließlich der Bewertung wird jetzt im Filme auflisten:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Sie sollten auch hinzufügen, die `Rating` Feld bearbeiten, Details und SearchIndex Vorlagen anzeigen.

Konnte, geben Sie den Befehl "Update-Database" in der **Package Manager Console** Fenster erneut und keine Änderungen würde vorgenommen werden, da das Modell mit dem Schema übereinstimmt.

In diesem Abschnitt haben gesehen, wie Sie Modellobjekte ändern und die Datenbank mit den Änderungen synchron bleiben. Außerdem haben Sie gelernt, eine Möglichkeit, eine neu erstellte Datenbank mit Beispieldaten aufzufüllen, damit Sie die Szenarien ausprobieren können. Als Nächstes sehen wir uns wie können Sie der Modellklassen umfangreichere Validierungslogik hinzu, und aktivieren einige Geschäftsregeln erzwungen werden.

>[!div class="step-by-step"]
[Zurück](examining-the-edit-methods-and-edit-view.md)
[Weiter](adding-validation-to-the-model.md)
