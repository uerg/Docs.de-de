---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Hinzufügen eines neuen Felds | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 0dac798eba586cdcc232cedd262e610b954004df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field"></a>Hinzufügen eines neuen Felds
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In diesem Abschnitt verwenden Sie Entity Framework Code First-Migrationen zu einige Änderungen an der Modellklassen migrieren, damit die Änderung auf die Datenbank angewendet wird.

Standardmäßig Wenn Sie Entity Framework Code First verwenden, um automatisch eine Datenbank erstellen, wie Sie weiter oben in diesem Lernprogramm ausgeführt haben Code First eine Tabelle der Datenbank hinzugefügt, um nachzuverfolgen, ob das Schema der Datenbank mit der Modellklassen synchron ist, die sie aus generiert wurde. Wenn sie nicht synchron sind, löst das Entity Framework einen Fehler auf. Dies erleichtert das Identifizieren von Problemen zur Entwicklungszeit zu verfolgen, die andernfalls nur (durch kryptische Fehler) zur Laufzeit auftreten.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Einrichten von Code First-Migrationen für Modelländerungen

Navigieren Sie zum Projektmappen-Explorer. Klicken Sie mit der rechten Maustaste auf die *Movies.mdf* Datei, und wählen Sie **löschen** zum Entfernen der Datenbank für Filme. Wenn Sie sehen die *Movies.mdf* Datei, klicken Sie auf die **alle Dateien anzeigen** unten im roten Symbol.

![](adding-a-new-field/_static/image1.png)

Erstellen Sie die Anwendung aus, um sicherzustellen, dass keine Fehler vorliegen.

Aus der **Tools** Menü klicken Sie auf **NuGet Package Manager** und dann **Package Manager Console**.

![Pack Man hinzufügen](adding-a-new-field/_static/image2.png)

In der **Package Manager Console** -Fensters am der `PM>` Eingabeaufforderung eingeben.

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

Die **Enable-Migrations** (siehe oben) Befehl erstellt eine *"Configuration.cs"* Datei in einem neuen *Migrationen* Ordner.

![](adding-a-new-field/_static/image4.png)

Visual Studio öffnet die *"Configuration.cs"* Datei. Ersetzen Sie die `Seed` Methode in der *"Configuration.cs"* Datei durch den folgenden Code:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Zeigen Sie auf die rote Wellenlinie unter `Movie` , und klicken Sie auf `Show Potential Fixes` , und klicken Sie dann auf **mit** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

Auf diese Weise fügt die folgende Anweisung:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Code First-Migrationen Ruft die `Seed` Methode nach jeder Migration (d. h. Aufrufen **Update-Database '** in der Paket-Manager-Konsole), und diese Methode aktualisiert die Zeilen, die bereits eingefügt wurde, oder fügt sie an, wenn sie nicht noch vorhanden ist.
> 
> Die [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) Methode in der folgende Code führt einen Vorgang "Upsert":
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Da die [Ausgangswert](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) Methode, die mit jeder Migration ausgeführt wird, die Daten können nicht nur eingefügt werden, da die Zeilen, die Sie hinzufügen möchten bereits es nach der ersten Migration entsprechen, die die Datenbank erstellt. Die "[Upsert](http://en.wikipedia.org/wiki/Upsert)" Vorgang wird verhindert, dass Fehler, die eintreten würden, wenn Sie versuchen, eine Zeile einzufügen, die bereits vorhanden ist, aber sie überschreibt alle Änderungen an den Daten, die Sie möglicherweise beim Testen der Anwendung vorgenommen haben. Mit Testdaten in einigen Tabellen nicht empfiehlt, die durchgeführt werden soll: in einigen Fällen beim Ändern von Daten beim Testen soll Ihre Änderungen bleiben nach einer Aktualisierung der Datenbank. In diesem Fall einen bedingten Einfügevorgang erstellt werden sollen: Fügen Sie eine Zeile nur dann, wenn er nicht bereits vorhanden.   
> 
> Der erste Parameter übergeben wird, um die [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) Methode gibt die Eigenschaft zu verwenden, um zu überprüfen, ob bereits eine Zeile vorhanden. Für die Film-Testdaten, die Sie bereitstellen, die `Title` Eigenschaft kann für diesen Zweck verwendet werden, da jeder Titel in der Liste eindeutig ist:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Mit diesem Code wird davon ausgegangen, dass der Titel eindeutig sind. Wenn Sie einen doppelten Titel manuell hinzufügen, erhalten Sie die folgende Ausnahme das nächste Mal, die Sie eine Migration ausführen.   
> 
>  *Die Sequenz enthält mehr als ein element*  
> 
> Weitere Informationen zu den [kann nicht](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) -Methode finden Sie unter [Achten Sie darauf mit EF 4.3 AddOrUpdate-Methode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**Drücken Sie STRG-UMSCHALT + B, um das Projekt zu erstellen.** (Die folgenden Schritte schlägt fehl, wenn Sie zu diesem Zeitpunkt nicht erstellt werden.)

Der nächste Schritt ist die Erstellung einer `DbMigration` der anfänglichen Migration-Klasse. Diese Migration erstellt eine neue Datenbank, die verdeutlichen, warum Sie die gelöschte der *movie.mdf* Datei in einem vorherigen Schritt.

In der **Package Manager Console** Fenster, geben Sie den Befehl `add-migration Initial` die anfängliche Migration erstellen. Der Name "Initial" ist ein beliebiger und wird verwendet, um die Namen der Migrationsdatei erstellt.

![](adding-a-new-field/_static/image6.png)

Code First-Migrationen erstellt einen anderen Klassendatei in der *Migrationen* Ordner (mit dem Namen *{Datumsstempel}\_Initial.cs* ), und diese Klasse enthält Code, der das Schema der Datenbank erstellt. Der Dateiname für die Migration ist bereits mit einem Zeitstempel fest, um mit der Reihenfolge zu. Überprüfen Sie die *{Datumsstempel}\_Initial.cs* Datei enthält Sie Anweisungen zum Erstellen der `Movies` Tabelle für die Film-DB. Beim Aktualisieren der Datenbank in den Anweisungen unten, dies *{Datumsstempel}\_Initial.cs* Datei ausgeführt wird und der DB-Schema erstellen. Die **Ausgangswert** Methode zum Auffüllen der Datenbank mit Testdaten ausgeführt wird.

In der **Package Manager Console**, geben Sie den Befehl `update-database` die Datenbank erstellen und Ausführen der `Seed` Methode.

![](adding-a-new-field/_static/image7.png)

Wenn Sie eine Fehlermeldung, der angibt erhalten, eine Tabelle ist bereits vorhanden und kann nicht erstellt werden, es ist wahrscheinlich, da die Anwendung ausgeführt wird, nachdem Sie die Datenbank gelöscht und bevor Sie ausgeführt `update-database`. In diesem Fall löschen Sie die *Movies.mdf* Datei erneut, und wiederholen Sie den `update-database` Befehl. Wenn Sie weiterhin eine Fehlermeldung erhalten, löschen Sie den Ordner und den Inhalt dann mit den Anweisungen am oberen Rand dieser Seite beginnen (also Löschen der *Movies.mdf* Datei, und klicken Sie dann den Vorgang fortzusetzen, Enable-Migrations). Wenn Sie einen Fehler zurück weiterhin erhalten, öffnen Sie SQL Server-Objekt-Explorer, und entfernen Sie die Datenbank aus der Liste.

Führen Sie die Anwendung, und navigieren Sie zu der */Movies* URL. Die Seed-Daten werden angezeigt.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Hinzufügen einer Rating-Eigenschaft zum Movie-Modell

Starten, indem er ein neues `Rating` Eigenschaft, um die vorhandene `Movie` Klasse. Öffnen der *Models\Movie.cs* und fügen die `Rating` wie die folgende Eigenschaft:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Die vollständige `Movie` -Klasse sieht wie der folgende Code:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Erstellen Sie die Anwendung (STRG + UMSCHALT + B).

Da Sie ein neues Feld hinzugefügt haben die `Movie` -Klasse, müssen Sie auch die Bindung zu aktualisieren *weiße Liste* , damit diese neue Eigenschaft eingeschlossen werden. Update der `bind` -Attribut für `Create` und `Edit` Aktionsmethoden enthalten die `Rating` Eigenschaft:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

Sie müssen auch die Ansichtsvorlagen aktualisieren, um die neue `Rating`-Eigenschaft in der Browseransicht anzuzeigen, zu erstellen und zu bearbeiten.

Öffnen der *\Views\Movies\Index.cshtml* und fügen eine `<th>Rating</th>` Spaltenüberschrift direkt nach der **Preis** Spalte. Fügen Sie dann eine `<td>` Spalte am Ende der Vorlage zum Rendern der `@item.Rating` Wert. Im folgenden finden Sie welche die aktualisierte *Index.cshtml* Vorlage anzeigen sieht wie folgt aus:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Öffnen Sie als Nächstes die *\Views\Movies\Create.cshtml* und fügen die `Rating` Feld durch Folgendes Markup Aktivitätsbaum hervorgehoben. Dies rendert ein Textfeld, sodass Sie eine Bewertung angeben können, wenn ein neue Film erstellt wird.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Sie haben jetzt den Code der Anwendung, um die Unterstützung der neuen `Rating` Eigenschaft.

Führen Sie die Anwendung, und navigieren Sie zu der */Movies* URL. Wenn Sie dies tun, wird jedoch eine der folgenden Fehlermeldungen angezeigt:

![](adding-a-new-field/_static/image9.png)  
  
Das Modell, sichern den Kontext "MovieDBContext" wurde geändert, seit der Erstellung der Datenbank. Erwägen Sie Code First-Migrationen zum Aktualisieren der Datenbank (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Dieser Fehler gemeldet wird, da die aktualisierte `Movie` Modellklasse in der Anwendung unterscheidet sich von dem Schema der der `Movie` Tabelle der vorhandenen Datenbank. (Die Datenbanktabelle enthält nicht die Spalte `Rating`.)


Es gibt mehrere Vorgehensweisen zum Beheben des Fehlers:

1. Lassen Sie die Datenbank von Entity Framework automatisch löschen und basierend auf dem neuen Modellklassenschema neu erstellen. Dieser Ansatz ist früh im Entwicklungszyklus sehr praktisch, wenn die aktive Entwicklung in einer Testdatenbank erfolgt. Er ermöglicht Ihnen, das Modell und das Datenbankschema schnell weiterzuentwickeln. Der Nachteil ist jedoch, dass Sie in der Datenbank vorhandene Daten verloren gehen – damit Sie *nicht* dieser Ansatz in einer Produktionsdatenbank verwendet werden soll. Das Verwenden eines Initialisierers zum automatischen Ausführen eines Seedings für eine Datenbank mit Testdaten ist häufig eine produktive Möglichkeit zum Entwickeln einer Anwendung. Weitere Informationen zu Entity Framework Database Initialisierer, finden Sie unter [ASP.NET MVC/Entity Framework-Lernprogramm](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Ändern Sie das Schema der vorhandenen Datenbank explizit so, dass es mit den Modellklassen übereinstimmt. Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten behalten. Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.
3. Verwenden Sie Code First-Migrationen, um das Datenbankschema zu aktualisieren.


Für dieses Tutorial verwenden wir Code First-Migrationen.

Aktualisieren Sie Seed-Methode, sodass sie einen Wert für die neue Spalte bereitstellt. Öffnen Sie Migrations\Configuration.cs-Datei und fügen Sie ein Bewertungsfeld auf jedes Movie-Objekt.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Erstellen Sie die Projektmappe, und öffnen Sie die **Package Manager Console** Fenster, und geben Sie den folgenden Befehl aus:

`add-migration Rating`

Die `add-migration` Befehl weist das Framework Migration, überprüfen das aktuelle Film-Modell mit dem aktuellen Film DB Schema und den erforderlichen Code zum Migrieren der Datenbank in das neue Modell zu erstellen. Der Name *Bewertung* ist willkürlich und wird verwendet, um die Migrationsdatei angegeben werden. Es ist hilfreich, einen aussagekräftigen Namen für den Schritt der Migration zu verwenden.

Wenn dieser Befehl abgeschlossen ist, handelt es sich bei Visual Studio öffnet die Klassendatei, die die neue definiert `DbMigration` abgeleitete Klasse, und klicken Sie in der `Up` Methode sehen Sie den Code, der die neue Spalte erstellt.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Erstellen Sie die Projektmappe, und geben Sie dann die `update-database` -Befehl in der **Package Manager Console** Fenster.

Die folgende Abbildung zeigt die Ausgabe in die **Package Manager Console** Fenster (dem Datum Stempel vorangestellt wird *Bewertung* unterscheiden.)

![](adding-a-new-field/_static/image11.png)

Führen Sie die Anwendung erneut aus, und navigieren Sie zu der URL /Movies. Sehen Sie die neue Rating-Felds an.

![](adding-a-new-field/_static/image12.png)

Klicken Sie auf die **neu erstellen** Link, um einen neuen Film hinzufügen. Beachten Sie, dass Sie eine Bewertung hinzufügen können.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Klicken Sie auf **Erstellen**. Der neue Film, einschließlich der Bewertung wird jetzt im Filme auflisten:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Nun, dass das Projekt Migrationen verwendet wird, müssen Sie wird nicht die Datenbank zu löschen, wenn Sie ein neues Feld hinzufügen oder aktualisieren Sie andernfalls das Schema. Im nächsten Abschnitt werden wir weitere schemaänderungen vornehmen und Migrationen verwenden, um die Datenbank zu aktualisieren.

Sie sollten auch hinzufügen, die `Rating` Feld bearbeiten, Details und Delete-Ansichtsvorlagen.

Konnte, geben Sie den Befehl "Update-Database" in der **Package Manager Console** Fenster erneut und ohne Code für die Migration ausgeführt werden, da das Modell mit dem Schema übereinstimmt. Ausführen von "Update-Database" werden jedoch ausgeführt, der `Seed` -Methode erneut auf, und wenn Sie eine der Ausgangswert Daten geändert haben, die Änderungen verloren da der `Seed` Upserts Methodendaten. Sie können erfahren Sie mehr über die `Seed` Methode in der Tom Dykstra beliebten [ASP.NET MVC/Entity Framework-Lernprogramm](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

In diesem Abschnitt haben gesehen, wie Sie Modellobjekte ändern und die Datenbank mit den Änderungen synchron bleiben. Außerdem haben Sie gelernt, eine Möglichkeit, eine neu erstellte Datenbank mit Beispieldaten aufzufüllen, damit Sie die Szenarien ausprobieren können. Dies war nur eine kurze Einführung in Code First, finden Sie unter [Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) ein mehr umfassendes Lernprogramm zu diesem Thema. Als Nächstes sehen wir uns wie können Sie der Modellklassen umfangreichere Validierungslogik hinzu, und aktivieren einige Geschäftsregeln erzwungen werden.

> [!div class="step-by-step"]
> [Zurück](adding-search.md)
> [Weiter](adding-validation.md)
