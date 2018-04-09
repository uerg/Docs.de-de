---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Teil 4: Modelle und Datenzugriff | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 4 werden die Modelle und Datenzugriff behandelt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-4-models-and-data-access"></a>Teil 4: Modelle und Datenzugriff
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.  
>   
> Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.
> 
> Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 4 werden die Modelle und Datenzugriff behandelt.


Bisher haben wir nur "dummy-Daten" durch unsere Controller an unsere Ansichtsvorlagen übergeben wurde. Nun können wir eine echte Datenbank verknüpft. In diesem Lernprogramm fügen wir zum Verwenden von SQL Server Compact Edition (häufig als SQL CE bezeichnet) als unsere Datenbankmodul abdecken. SQL CE ist eine kostenlose, eingebettete, dateibasierte Datenbank, die keine erfordert Installation oder Konfiguration, wodurch das tatsächlich für lokale Entwicklung praktisch.

## <a name="database-access-with-entity-framework-code-first"></a>Datenbankzugriff mit Entity Framework Code First

Wir verwenden die Entity Framework (EF)-Unterstützung, die in ASP.NET MVC 3-Projekte, Abfragen und Aktualisieren der Datenbank enthalten ist. EF ist ein flexibles Objekt relationalen zuordnen (ORM) Daten-API, die Entwicklern, Abfragen und Aktualisieren von Daten in einer Datenbank auf objektorientierte Weise gespeichert ermöglicht.

Entity Framework, Version 4 unterstützt ein Code First aufgerufen Entwicklung-Paradigma. Code First können Sie Modellobjekt zu erstellen, indem Sie das Schreiben von einfachen Klassen (auch bekannt als POCO aus CLR-Objekte "Plain Old") und kann auch die Datenbank im Handumdrehen von Klassen erstellen.

### <a name="changes-to-our-model-classes"></a>Änderungen an unseren Modellklassen

Es wird die Erstellung Datenbankfunktion in Entity Framework in diesem Lernprogramm genutzt werden. Bevor wir dies tun, aber wir einige geringfügige Änderungen daran vornehmen zu unserem Modellklassen hinzufügen in einige Dinge, die wir später verwenden.

#### <a name="adding-the-artist-model-classes"></a>Die Interpret-Modellklassen hinzufügen

Unsere Alben werden Künstler, zugeordnet werden, damit wir eine einfache Modellklasse, um ein Künstler beschreiben hinzufügen. Fügen Sie eine neue Klasse mit dem Namen mit dem unten gezeigten Code Artist.cs Ordner Models.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Unsere Modellklassen aktualisieren

Aktualisieren Sie die Album-Klasse, wie unten dargestellt.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Stellen Sie anschließend die folgenden Updates auf die Klasse "Genre" ein.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Hinzufügen der App\_Datenordner

Fügen wir eine App\_Datenverzeichnis unsere Projekt, um unsere SQL Server Express-Datenbankdateien zu halten. App\_Daten ist ein besonderes Verzeichnis in ASP.NET bereits über die richtigen sicherheitszugriffsberechtigungen für den Datenbankzugriff. Wählen Sie im Menü Projekt ASP.NET-Ordner hinzufügen und dann die App\_Daten.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Erstellen einer Verbindungszeichenfolge in der Datei "Web.config"

Wir werden einige Zeilen Konfigurationsdatei der Website hinzufügen, damit Entity Framework Herstellen einer Verbindung mit der Datenbank bekannt ist. Doppelklicken Sie auf die Datei "Web.config" im Stammverzeichnis des Projekts.

![](mvc-music-store-part-4/_static/image2.png)

Führen Sie einen Bildlauf zum unteren Rand dieser Datei, und fügen eine &lt;ConnectionStrings&gt; direkt über die letzte Zeile im Abschnitt, wie unten dargestellt.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Hinzufügen von Context-Klasse

Mit der rechten Maustaste in den Ordner Models, und fügen Sie eine neue Klasse namens MusicStoreEntities.cs hinzu.

![](mvc-music-store-part-4/_static/image3.png)

Diese Klasse wird den Datenbankkontext Entity Framework darstellen wird behandeln erstellen, lesen, aktualisieren und delete-Operationen für uns. Der Code für diese Klasse wird unten gezeigt.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

Das war schon alles – es ist keine weitere Konfiguration, spezielle Schnittstellen usw. Durch die Erweiterung der Basisklasse ' DbContext ', ist unsere MusicStoreEntities-Klasse können unsere Datenbankvorgänge für uns zu behandeln. Nun, da wir, die eingebunden haben, fügen Sie einigen weitere Eigenschaften zu unserem Modellklassen einige zusätzliche Informationen in die Datenbank nutzen.

### <a name="adding-our-store-catalog-data"></a>Unsere Katalogdaten Speicher hinzufügen

Es wird eine Funktion in Entity Framework nutzen, "Seed" Daten in eine neu erstellte Datenbank hinzugefügt. Dadurch wird unsere Store-Katalog mit einer Liste von Genres Künstler und Alben vorab aufgefüllt. MvcMusicStore Assets.zip Download des - unserer Website Design-Dateien, die weiter oben in diesem Lernprogramm verwendet enthalten – verfügt über eine Klassendatei mit dieser Ausgangswerte befindet sich in einem Ordner namens Code.

Innerhalb des Codes / Ordner Models, suchen Sie die Datei SampleData.cs, und legen Sie sie in der Ordner Models in unserem Projekt aus, wie unten dargestellt.

![](mvc-music-store-part-4/_static/image4.png)

Nun müssen wir eine Codezeile anzuweisen, Entity Framework zu SampleData Klasse hinzufügen. Doppelklicken Sie auf die Datei "Global.asax" im Stammverzeichnis des Projekts geöffnet, und fügen die folgende Zeile am Anfang die Anwendung\_Start-Methode.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

An diesem Punkt haben wir die erforderlichen Schritte zum Konfigurieren von Entity Framework für unsere Projekt abgeschlossen.

## <a name="querying-the-database"></a>Abfragen der Datenbank

Jetzt aktualisieren wir unsere StoreController, sodass anstelle von "dummy Daten" stattdessen in die Datenbank alle seine Informationen abgefragt wird. Wir beginnen mit deklarieren ein Feld für die **StoreController** um eine Instanz der MusicStoreEntities-Klasse, die mit dem Namen StoreDB aufzunehmen:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Aktualisieren den Columnstore-Index zum Abfragen der Datenbank

Die MusicStoreEntities-Klasse wird vom Entity Framework verwaltet und stellt die Auflistungseigenschaft für jede Tabelle in der Datenbank. Wir aktualisieren unsere StoreController Index-Aktion, um alle Genres in der Datenbank abrufen. Zuvor haben wir dies durch eine feste Programmierung Zeichenfolgendaten. Nun können wir die Entity Framework-Datenkontext Generes Auflistung stattdessen nur verwenden:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Keine Änderungen müssen an unsere Ansichtenvorlage durchgeführt werden soll, da wir noch die gleiche StoreIndexViewModel zurückgeben, wird zurückgegeben, bevor - wir gerade Livedaten aus der Datenbank jetzt zurückgeben.

Wenn wir führen das Projekt erneut aus, und besuchen Sie "/ Speichern", sehen wir nun eine Liste von Genres alle in der Datenbank:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Aktualisieren von Speicher zu durchsuchen und Details, für die Verwendung von Livedaten

Mit der/Store/durchsuchen? "Genre" =*[einige Genre]* Aktionsmethode, wir haben eine "Genre" nach Namen suchen. Wird nur ein Ergebnis erwartet, da wir jemals zwei Einträge für denselben Namen "Genre" verfügen, sollten nicht, weshalb wir verwenden können, die. Single() Erweiterung in LINQ-Abfrage für das entsprechende Objekt für "Genre" wie folgt (nicht vom Typ dies noch):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

Die einzige Methode nimmt einen Lambda-Ausdruck als Parameter, der angibt, dass ein einzelnes "Genre"-Objekt, dessen Name mit dem Wert übereinstimmt, den es definiert haben soll. Im obigen Fall werden wir ein einzelnes Objekt für "Genre" mit einem Namenswert, der Abgleich Disco geladen.

Es wird eine Entity Framework-Funktion nutzen, die uns ermöglicht, die andere verknüpften Entitäten angezeigt werden, die ebenfalls geladen werden soll, wenn das Objekt "Genre" abgerufen wird. Dieses Feature heißt Abfrage Ergebnis strukturiert werden, und ermöglicht es, zu die Anzahl der zu verringern, müssen wir Zugriff auf die Datenbank aus, um alle Informationen abzurufen, die Sie benötigen. Wir möchten die Alben für "Genre" Wir abzurufen, vorab abzurufen, damit wir unsere Abfrage von Genres.Include("Albums"), um anzugeben, dass wir auch verwandte Alben möchten aktualisiert werden. Dies ist effizienter, da sie unsere "Genre" und die Album Daten in einer einzelnen Datenbank-Anforderung abgerufen werden.

Mit den Erklärungen aus dem Weg sieht wie unsere aktualisierte durchsuchen Controlleraktion aussieht:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Wir haben jetzt aktualisieren der Store durchsuchen-Ansicht zum Anzeigen von Alben in jeder "Genre" zur Verfügung stehen. Öffnen Sie die Vorlage anzeigen (im /Views/Store/Browse.cshtml), und fügen Sie eine Aufzählung von Alben hinzu, wie unten dargestellt.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Unsere Anwendung ausführen und das Navigieren zu/Store/durchsuchen? "Genre" = Jazz an, dass der aus der Datenbank, Anzeigen von alle Alben in unserer ausgewählten "Genre" jetzt unsere Ergebnisse abgerufen werden.

![](mvc-music-store-part-4/_static/image2.jpg)

Wir stellen identisch zu unserem/Store/Informationen / [Id] URL ändern, und Ersetzen Sie unsere Platzhalterdaten mit einer Datenbankabfrage, die ein Album lädt, deren ID der Wert des Parameters übereinstimmt.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Unsere Anwendung ausführen und das Navigieren zu /Store/Details/1 zeigt, dass unsere Ergebnisse jetzt aus der Datenbank abgerufen werden.

![](mvc-music-store-part-4/_static/image5.png)

Nun, dass unsere Store Detailseite so eingerichtet ist, ein Album mit der ID Album anzeigen, aktualisieren wir die **Durchsuchen** Sicht um mit der Ansicht zu verknüpfen. Wir verwenden Html.ActionLink, genau so wie wir Verknüpfung im Columnstore-Index Store durchsuchen am Ende des vorherigen Abschnitts. Die vollständige Quelle für die Ansicht durchsuchen wird unten angezeigt.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Jetzt können wir aus unserer Seite "Store" zu einer Seite "Genre" suchen, die die verfügbaren Alben aufgelistet sind, und durch Klicken auf ein Album können wir Details für diese Album anzeigen.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-3.md)
> [Weiter](mvc-music-store-part-5.md)
