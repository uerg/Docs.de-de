---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Teil 4: Modelle und Datenzugriff | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 4 sind die Modelle und Datenzugriff.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: ea8fe623a1b59b80fd7f087036b9ed716eafadbe
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402037"
---
<a name="part-4-models-and-data-access"></a>Teil 4: Modelle und Datenzugriff
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.  
>   
> Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.
> 
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 4 sind die Modelle und Datenzugriff.


Bisher haben wir nur "unechten Daten" aus unserer Controllern an unsere Vorlagen anzeigen übergeben wurde. Wir nun können eine echte Datenbank verknüpft. In diesem Tutorial werden wir mit SQL Server Compact Edition (häufig SQL CE genannt) als unsere Datenbank-Engine behandelt. SQL CE ist eine kostenlose, eingebettete, dateibasierte Datenbank, die keine erfordert Installation oder Konfiguration, dadurch wird es wirklich praktisch, für die lokale Entwicklung.

## <a name="database-access-with-entity-framework-code-first"></a>Zugriff auf die Datenbank mit Entity Framework Code First

Wir verwenden die Unterstützung von Entity Framework (EF), die in ASP.NET MVC 3-Projekten, zum Abfragen und aktualisieren Sie die Datenbank enthalten ist. EF ist ein flexibles Objekt relational mapping (ORM), Daten-API, die Entwicklern, Abfragen und Aktualisieren von Daten in einer Datenbank in eine objektorientierte Weise gespeichert ermöglicht.

Entitätsframework, Version 4 unterstützt ein bezeichnet der Code-First-Entwicklung-Paradigma. Code First können Sie Model-Objekts zu erstellen, durch das Schreiben von einfachen Klassen (auch bekannt als POCO von "Plain-Old" CLR Objects), und Sie können auch die Datenbank während der Bearbeitung von Klassen erstellen.

### <a name="changes-to-our-model-classes"></a>Änderungen an unserem ViewModel-Klassen

Es wird das Datenbank-erstellen-Feature in Entity Framework in diesem Tutorial nutzen. Bevor wir dies tun, stellen jedoch einige kleinere Änderungen lassen Sie uns auf unsere Modellklassen Punkte hinzufügen, die wir später verwenden.

#### <a name="adding-the-artist-model-classes"></a>Hinzufügen von Modellklassen der Künstler diese geändert hat

Unsere Alben werden Künstler, zugeordnet werden, damit wir eine einfache Modellklasse, die zum Beschreiben eines Interpreten hinzufügen. Fügen Sie eine neue Klasse, um den Ordner "Models", die mit dem Namen Artist.cs mit den folgenden Code.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Aktualisieren die Modellklassen

Aktualisieren Sie die Album-Klasse, wie unten dargestellt.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Stellen Sie als Nächstes die folgenden Updates auf die Klasse "Genre" ein.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Hinzufügen der App\_Datenordner

Fügen wir eine App\_Datenverzeichnis auf das Projekt aus, um unsere SQL Server Express-Datenbankdateien speichern. App\_Daten ist ein besonderes Verzeichnis in ASP.NET die bereits die richtige Sicherheits-Zugriffsberechtigungen für den Zugriff auf die Datenbank hat. Wählen Sie im Menü Projekt ASP.NET-Ordner hinzufügen und dann die App\_Daten.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Erstellen einer Verbindungszeichenfolge in der Datei "Web.config"

Wir werden einige Zeilen der Website-Konfigurationsdatei hinzufügen, damit Entity Framework erkennt, Herstellen einer Verbindung mit der Datenbank. Doppelklicken Sie auf die Datei "Web.config" im Stammverzeichnis des Projekts.

![](mvc-music-store-part-4/_static/image2.png)

Führen Sie einen Bildlauf zum unteren Rand dieser Datei, und fügen eine &lt;ConnectionStrings&gt; direkt oberhalb der letzten Zeile im Abschnitt, wie unten dargestellt.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Hinzufügen einer Context-Klasse

Mit der rechten Maustaste in den Ordner "Models", und fügen Sie eine neue Klasse namens MusicStoreEntities.cs hinzu.

![](mvc-music-store-part-4/_static/image3.png)

Diese Klasse wird den Entity Framework-Datenbankkontext darstellen und wird verarbeiten unserer erstellen, lesen, aktualisieren und delete-Operationen für uns. Der Code für diese Klasse ist unten dargestellt.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

Das ist schon alles – es ist keine weitere Konfiguration, spezielle Schnittstellen usw. Unsere MusicStoreEntities-Klasse kann durch die Erweiterung der DbContext-Basisklasse, unsere Datenbankvorgänge für uns zu behandeln. Nun, da wir, die eingebunden haben, fügen Sie einigen weitere Eigenschaften auf unserem Modellklassen einige zusätzliche Informationen in unserer Datenbank nutzen.

### <a name="adding-our-store-catalog-data"></a>Unsere Daten der Store-Katalog hinzufügen

Es wird ein Feature in Entity Framework nutzen "Seed"-Daten in eine neu erstellte Datenbank hinzufügt. Unsere Store-Katalog mit einer Liste von Genres und Interpreten, Alben ausgefüllt. Der MvcMusicStore-Assets.zip Download – was unsere Website Design-Dateien, die weiter oben in diesem Tutorial verwendete enthalten – verfügt über eine Klassendatei mit Seed-Daten, befindet sich in einem Ordner namens Code.

Innerhalb des Codes / Ordner "Models", suchen Sie die SampleData.cs-Datei, und legen Sie sie in den Ordner "Models" in Ihrem Projekt, wie unten dargestellt.

![](mvc-music-store-part-4/_static/image4.png)

Nun müssen wir eine Codezeile erforderlich, um mitzuteilen, Entity Framework über die SampleData-Klasse hinzufügen. Doppelklicken Sie auf die Datei "Global.asax" im Stammverzeichnis des Projekts, öffnen es, und fügen die folgende Zeile am Anfang die Anwendung\_Start-Methode.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

An diesem Punkt haben wir die erforderlichen Schritte zum Konfigurieren von Entity Framework für unser Projekt abgeschlossen.

## <a name="querying-the-database"></a>Abfragen der Datenbank

Jetzt aktualisieren wir unsere StoreController, damit anstelle von "dummy-Daten" stattdessen unsere Datenbank alle zugehörigen Informationen Abfragen aufrufen. Wir beginnen mit Deklarieren eines Feldes in der **StoreController** , die eine Instanz der MusicStoreEntities-Klasse, mit dem Namen StoreDB enthalten soll:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Aktualisieren des Store-Index zum Abfragen der Datenbank

Die MusicStoreEntities-Klasse wird vom Entity Framework verwaltet und stellt eine Auflistungseigenschaft für jede Tabelle in der Datenbank zur Verfügung. Wir aktualisieren unsere StoreControllers Index-Aktion zum Abrufen von allen Genres in der Datenbank. Zuvor haben wir dies von Zeichenfolgendaten fest zu programmieren. Jetzt können wir die Entity Framework-Datenkontext Generes Sammlung stattdessen nur verwenden:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Es müssen keine Änderungen, unsere Vorlage für die Ansicht ausgeführt werden, da wir weiterhin die gleichen StoreIndexViewModel zurückgeben, bevor: wir nur Livedaten aus unserer Datenbank jetzt zurückgeben, zurückgegeben.

Wenn wir führen das Projekt erneut aus, und rufen Sie die URL "/ Store", sehen wir jetzt eine Liste mit allen Genres in unserer Datenbank:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Aktualisieren von Store durchsuchen und Details zur Verwendung von live-Daten

Durch den/Store/durchsuchen? Genre =*[einige Genre]* Aktionsmethode suchen wir nach einer "Genre" anhand des Namens. Wir nur ein Ergebnis, erwarten, da wir immer zwei Einträge für denselben Namen "Genre" haben darf nicht, weshalb wir verwenden können, die. Single() Erweiterung in LINQ-Abfrage für das entsprechende Objekt für "Genre" wie folgt (nicht-Typ dies noch):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

Die einzelne Methode nimmt einen Lambdaausdruck als Parameter, der angibt, dass ein einzelnes "Genre"-Objekt, dessen Name mit dem Wert übereinstimmt, die, den wir definiert haben, soll. Im obigen Fall werden wir ein einzelnes "Genre"-Objekt mit dem Namenswert, der übereinstimmende Disco geladen.

Werfen wir einen Entity Framework-Feature nutzen, mit der wir an, dass andere verbundene Entitäten, die ebenfalls geladen werden soll, wenn sich das Genre-Objekt abgerufen wird. Dieses Feature heißt die Abfrage Ergebnis strukturieren und ermöglicht es uns, um die Anzahl der zu verringern, müssen wir Zugriff auf die Datenbank, um alle Informationen abrufen, die wir benötigen. Wir möchten die Alben für "Genre" Wir abzurufen, vorab abgerufen werden, damit wir unsere Abfrage durch Hinzufügen von Genres.Include("Albums") aktualisieren, um anzugeben, dass auch die zugehörigen Alben werden soll. Dies ist effizienter, da es sowohl unser "Genre" als auch das Album Daten in einer einzelnen Datenbank-Anforderung abgerufen werden.

Mit den Erklärungen nicht im Weg sieht wie unsere aktualisierte durchsuchen-Controlleraktion:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Wir können jetzt aktualisieren der Store durchsuchen-Ansicht zum Anzeigen von Alben, die in jeder "Genre" verfügbar sind. Öffnen Sie die ansichtsvorlage (gefunden in /Views/Store/Browse.cshtml), und fügen Sie eine Aufzählung von Alben, wie unten dargestellt.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Ausführung unserer Anwendung, und navigieren zu/Store/durchsuchen? Genre = Jazz zeigt an, die aus der Datenbank, Anzeigen von alle Alben in unserer ausgewählte Genre jetzt unsere Ergebnisse abgerufen werden.

![](mvc-music-store-part-4/_static/image2.jpg)

Wir stellen die gleiche ändern Sie in unserem/Store/Details / [Id] URL, und Ersetzen Sie unsere unechten Daten durch die Abfrage einer Datenbank, die ein Album lädt, deren ID mit dem Wert des Parameters übereinstimmt.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Unsere Anwendung ausführen, und Navigieren zum /Store/Details/1 zeigt, dass unsere Ergebnisse jetzt aus der Datenbank abgerufen werden.

![](mvc-music-store-part-4/_static/image5.png)

Nun, da unsere Seite "Store-Details" zum Anzeigen eines Albums durch die ID des Albums eingerichtet ist, aktualisieren wir die **Durchsuchen** anzeigen, um mit der Detailansicht zu verknüpfen. Wir verwenden Html.ActionLink, genau wie aus dem Store Index Store durchsuchen am Ende des vorherigen Abschnitts verknüpfen. Der vollständige Quellcode für die Ansicht durchsuchen wird unten angezeigt.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Jetzt können wir aus unserer Seite "Store" auf die Seite "Genre" suchen, die die Listen der verfügbaren Alben und durch Klicken auf ein Album wir können Details anzeigen, für die jeweiligen Album zu sehen.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-3.md)
> [Weiter](mvc-music-store-part-5.md)
