---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Erstellen die Datenzugriffsebene | Microsoft Docs
author: Erikre
description: "Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: ebace10dc8a861ab38bd5c834c2225e3373f13fe
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="create-the-data-access-layer"></a>Die Datenzugriffsebene erstellen
====================
Durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen des Wingtip Toys-Beispielprojekt (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [(PDF) E-Book herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die diese Reihe von Lernprogrammen begleitet.


In diesem Lernprogramm wird beschrieben, wie erstellen, zugreifen, und überprüfen Daten aus einer Datenbank mithilfe von ASP.NET Web Forms- und Entity Framework Code First wird. Dieses Lernprogramm baut auf dem vorherigen Lernprogramm "Erstellen das Projekt" und ist Teil der Wingtip Toys Store Reihe von Lernprogrammen. Wenn Sie dieses Lernprogramm abgeschlossen haben, Sie werden erstellt haben, eine Gruppe von Datenzugriffs-Klassen, die in der *Modelle* Ordner des Projekts.

## <a name="what-youll-learn"></a>Lernen Sie:

- Vorgehensweise: Erstellen der Datenmodelle.
- Informationen zum Initialisieren und Ausgangswert für die Datenbank.
- Informationen zum Aktualisieren und konfigurieren die Anwendung zur Unterstützung der Datenbank.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Dies sind die Funktionen, die im Lernprogramm eingeführt:

- Entity Framework Code First
- LocalDB
- Datenanmerkungen

## <a name="creating-the-data-models"></a>Erstellen von Data-Modellen

[Entity Framework](https://msdn.microsoft.com/en-us/data/aa937723) ist ein ORM (Objektrelationales Mapping)-Framework. Sie können damit die Arbeit mit relationalen Daten als Objekte, entfernen Sie die meisten der Datenzugriffs-Code, den Sie in der Regel schreiben müssen. Verwendung von Entity Framework, können Sie Abfragen mit ausgeben [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx), abrufen und Bearbeiten von Daten als stark typisierte Objekte. LINQ stellt Muster zum Abfragen und Aktualisieren von Daten bereit. Mithilfe von Entity Framework können Sie den Schwerpunkt auf den Rest der Anwendung erstellen, anstatt Schwerpunkt auf die Daten Zugriff Grundlagen. Weiter unten in diesem Lernprogramm Reihe zeigen wir Ihnen wie die Daten, die zum Auffüllen der Navigation und Abfragen verwenden.

Entity Framework unterstützt ein Entwicklung Paradigma aufgerufen *Code First*. Code First können Sie Ihre Datenmodelle mithilfe von Klassen zu definieren. Eine Klasse ist ein Konstrukt, das Ihnen ermöglicht, Ihre eigenen benutzerdefinierten Typen zu erstellen, indem Sie Variablen andere Typen, Methoden und Ereignisse zusammengefasst. Sie können Zuordnungsklassen zu einer vorhandenen Datenbank oder verwenden, um eine Datenbank zu generieren. In diesem Lernprogramm erstellen Sie die Datenmodelle von Modellklassen Daten schreiben. Klicken Sie dann, informieren Sie Entity Framework, die die Datenbank im Handumdrehen von dieser neuen Klassen zu erstellen.

Sie können jetzt durch das Erstellen der Entitätsklassen, die die Datenmodelle für die Web Forms-Anwendung zu definieren. Klicken Sie dann erstellen Sie eine Kontextklasse, die die Entitätsklassen verwaltet und ermöglicht den Datenzugriff auf die Datenbank. Erstellen Sie auch eine Initialisiererklasse, die Sie zum Auffüllen der Datenbank verwenden.

### <a name="entity-framework-and-references"></a>Entity Framework und Verweise

Standardmäßig Entity Framework ist enthalten, beim Erstellen einer neuen **ASP.NET-Webanwendung** mithilfe der **Web Forms** Vorlage. Entity Framework kann installiert, deinstalliert und als ein NuGet-Paket aktualisiert werden.

Dieses NuGet-Paket enthält die folgenden **Runtime** Assemblys im Projekt:

- EntityFramework.dll – alle common Runtime-Code von Entity Framework verwendet
- EntityFramework.SqlServer.dll – Microsoft SQL Server-Anbieter für Entity Framework

### <a name="entity-classes"></a>Entitätsklassen

Die Klassen, die Sie erstellen, um das Schema der Daten zu definieren, werden Entitätsklassen aufgerufen. Wenn Sie auf den Datenbankentwurf vertraut sind, stellen Sie sich die Entitätsklassen als Tabellendefinitionen einer Datenbank. Jede Eigenschaft in der Klasse gibt eine Spalte in der Tabelle der Datenbank. Diese Klassen bieten eine einfache objektrelationale Schnittstelle zwischen objektorientierte Code und die Struktur der relationalen Tabelle der Datenbank.

In diesem Lernprogramm beginnen Sie durch Hinzufügen von einfachen Entitätsklassen, die die Schemas für Kategorien und Produkte darstellt. Die Klasse Produkte enthält Definitionen für jedes Produkt. Der Name jedes der Elemente der Product-Klasse `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, und `Category`. Die Klasse der Kategorie enthält Definitionen für jede Kategorie, die ein Produkt zu, z. B. Auto, Boot oder Ebene gehören kann. Der Name der einzelnen Member der Klasse Kategorie `CategoryID`, `CategoryName`, `Description`, und `Products`. Jedes Produkt wird auf eine der Kategorien gehören. Diese Entitätsklassen werden hinzugefügt werden, auf des Projekts vorhandene *Modelle* Ordner.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Modelle* Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**. 

    ![Erstellen der Datenzugriffsschicht - neuen Menüelements](create_the_data_access_layer/_static/image1.png)

 Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Klicken Sie unter **Visual C#-** aus der **installiert** Bereich auf der linken Seite, wählen **Code**. 

    ![Erstellen der Datenzugriffsschicht - neuen Menüelements](create_the_data_access_layer/_static/image2.png)
3. Wählen Sie **Klasse** aus der Mitte und nennen Sie diese neue Art *Product.cs*.
4. Klicken Sie auf **Hinzufügen**.  
 Die neue Klassendatei wird im Editor angezeigt.
5. Ersetzen Sie den Standardcode durch folgenden Code:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Erstellen Sie eine andere Klasse durch Wiederholen der Schritte 1 bis 4, benennen Sie die neue Klasse jedoch *Category.cs* , und Ersetzen Sie den Standardcode durch folgenden Code:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Wie bereits erwähnt, die `Category` -Klasse stellt der Typ des Produkts, die Anwendung ist, verkaufen soll (z. B. <a id="a"> </a> &quot;Autos&quot;, &quot;Booten&quot;, &quot;Raketen&quot;usw.), und die `Product` Klasse darstellt, die einzelnen Produkte ("Toy") in der Datenbank. Jede Instanz einer `Product` Objekt wird eine Zeile in einer relationalen Datenbanktabelle entsprechen, und jede Eigenschaft der Product-Klasse auf eine Spalte in der relationalen Datenbanktabelle zugeordnet wird. Weiter unten in diesem Lernprogramm müssen Sie die in der Datenbank enthaltenen Produktdaten überprüfen.

### <a name="data-annotations"></a>Datenanmerkungen

Sie haben vielleicht festgestellt, dass bestimmte Mitglieder der Klassen angeben Memberdetails, z. B. Attribute `[ScaffoldColumn(false)]`. Hierbei handelt es sich *datenanmerkungen*. Die anmerkungsattribute können beschrieben, wie zum Validieren von Benutzereingaben für dieses Element, um anzugeben, Formatierung und um anzugeben, wie es bei der Datenbankerstellung modelliert wird.

### <a name="context-class"></a>Context-Klasse

Um mithilfe der Klassen für den Datenzugriff zu starten, müssen Sie eine Kontextklasse definieren. Wie bereits erwähnt, wird die Context-Klasse verwaltet die Entitätsklassen (z. B. die `Product` Klasse und die `Category` Klasse) und ermöglicht den Datenzugriff auf die Datenbank.

Diese Prozedur fügt eine neue C#-Context-Klasse, die *Modelle* Ordner.

1. Mit der rechten Maustaste die *Modelle* Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**.   
 Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie **Klasse** aus der Mitte, nennen Sie sie *ProductContext.cs* , und klicken Sie auf **hinzufügen**.
3. Ersetzen Sie den Standardcode, der in der Klasse mit den folgenden Code enthalten:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Dieser Code fügt der `System.Data.Entity` Namespace sodass Sie Zugriff auf die Kernfunktionen von Entity Framework, einschließlich die Funktion zum Abfragen, einfügen, aktualisieren und Löschen von Daten mit stark typisierten Objekten arbeiten.

Die `ProductContext` Klasse darstellt, Entity Framework-Produkt-Datenbankkontext, der verarbeitet abrufen, speichern und Aktualisieren von `Product` -Klasseninstanzen in der Datenbank. Die `ProductContext` Klasse leitet sich von der `DbContext` Basisklasse von Entity Framework bereitgestellt werden.

### <a name="initializer-class"></a>Initialisiererklasse

Sie benötigen, führen Sie die benutzerdefinierte Logik, um die Datenbank das erste Mal zu initialisieren, das der Kontext verwendet wird. Dadurch kann die Ausgangswerte der Datenbank hinzugefügt werden, damit Sie sofort Produkte und Kategorien angezeigt werden können.

Diese Prozedur fügt eine neue C#-Initialisierer-Klasse, die *Modelle* Ordner.

1. Erstellen Sie ein weiteres `Class` in der *Modelle* Ordner und nennen Sie sie *ProductDatabaseInitializer.cs*.
2. Ersetzen Sie den Standardcode, der in der Klasse mit den folgenden Code enthalten:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Wie im obigen Code sehen können, wenn die Datenbank erstellt und initialisiert, die `Seed` Eigenschaft überschreiben und festgelegt wird. Wenn die `Seed` Eigenschaft festgelegt ist, die Werte aus den Kategorien und Produkte werden verwendet, um die Datenbank aufzufüllen. Wenn Sie versuchen, aktualisieren Sie die Seed-Daten durch den oben aufgeführten Code ändern, nachdem die Datenbank erstellt wurde, wird keine Updates angezeigt werden, wenn Sie die Webanwendung ausführen. Der Grund hierfür ist der obige Code verwendet die Implementierung der `DropCreateDatabaseIfModelChanges` Klasse zu erkennen, wenn das Modell (Schema) geändert hat, vor dem Zurücksetzen der Seed-Daten. Wenn keine Änderungen, um vorgenommen werden die `Category` und `Product` Entitätsklassen, die Datenbank werden mit den Ausgangswert Daten nicht erneut initialisiert werden.

> [!NOTE] 
> 
> Mussten Sie die Datenbank jedes Mal neu erstellt werden, Sie haben die Anwendung ausgeführt, können Sie die `DropCreateDatabaseAlways` -Klasse statt der `DropCreateDatabaseIfModelChanges` Klasse. Verwenden Sie für diese Reihe von Lernprogrammen, jedoch die `DropCreateDatabaseIfModelChanges` Klasse.


Zu diesem Zeitpunkt in diesem Lernprogramm wird Ihnen eine *Modelle* Ordner mit vier neuen Klassen und ein Standard-Klasse:

![Erstellen der Datenzugriffsschicht - Ordner Models](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Konfigurieren die Anwendung zur Verwendung des Datenmodells

Nun, dass Sie die Klassen, die die Daten darstellen erstellt haben, müssen Sie die Anwendung zur Verwendung der Klassen konfigurieren. In der *"Global.asax"* -Datei fügen Sie Code hinzu, die das Modell initialisiert. In der *"Web.config"* Datei, die Sie hinzufügen, Informationen, die der Anwendung mitteilt, welche Datenbank verwenden, um die Daten zu speichern, die durch die neue Datenklassen dargestellt wird. Die *"Global.asax"* Datei kann zum Behandeln von Ereignissen oder Methoden verwendet werden. Die *"Web.config"* Datei können Sie die Konfiguration von Ihrer ASP.NET-Webanwendung steuern.

#### <a name="updating-the-globalasax-file"></a>Aktualisieren die Datei "Global.asax"

Um die Datenmodelle beim Starten der Anwendung zu initialisieren, aktualisieren Sie die `Application_Start` Ereignishandler in der *Global.asax.cs* Datei.

> [!NOTE] 
> 
> Im Projektmappen-Explorer, wählen Sie entweder die *"Global.asax"* Datei oder das *Global.asax.cs* Datei so bearbeiten Sie die *Global.asax.cs* Datei.


1. Fügen Sie den folgenden Code in gelb hervorgehoben werden die `Application_Start` Methode in der *Global.asax.cs* Datei.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Unterstützt Ihr Browser muss HTML5, um den Code in gelb hervorgehoben werden, wenn diese Reihe von Lernprogrammen in einem Browser anzeigen anzuzeigen.


Wie im obigen Code angezeigt, wenn die Anwendung gestartet wird, gibt die Anwendung an, dass die Initialisierung, die während der ersten Mal Daten ausgeführt werden, zugegriffen wird. Die zwei zusätzliche Namespaces sind erforderlich für den Zugriff auf die `Database` Objekt und die `ProductDatabaseInitializer` Objekt.

 Ändern die Datei "Web.config" 

Hinzufügen eigener Verbindungsinformationen für Ihre Anwendung zwar Entity Framework Code First, eine Datenbank für Sie an einem Standardspeicherort generieren wird Wenn die Datenbank mit Ausgangswert Daten aufgefüllt ist, erhalten Sie Kontrolle über den Speicherort der Datenbank. Geben Sie diese mithilfe einer Verbindungszeichenfolge in der Anwendungsverzeichnis datenbankverbindung *"Web.config"* Datei im Stammverzeichnis des Projekts. Durch Hinzufügen einer neuen Verbindungszeichenfolge, können Sie den Speicherort der Datenbank weiterleiten (*wingtiptoys.mdf*) im Datenverzeichnis der Anwendung erstellt werden sollen (*App\_Daten*), statt den Standardwert Speicherort. Diese Änderung können Sie suchen, und überprüfen die Datenbankdatei für die weiter unten in diesem Lernprogramm.

1. In **Projektmappen-Explorer**, suchen und öffnen Sie die *"Web.config"* Datei.
2. Fügen Sie die Verbindungszeichenfolge gelb hervorgehoben der `<connectionStrings>` Teil der *"Web.config"* Datei wie folgt:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Wenn die Anwendung zum ersten Mal ausgeführt wird, wird es von der Verbindungszeichenfolge angegebenen Speicherort die Datenbank erstellt werden. Jedoch vor dem Ausführen der Anwendung an, wir erstellen Sie sie zuerst.

## <a name="building-the-application"></a>Erstellen der Anwendung

Um sicherzustellen, dass alle Klassen und Änderungen an Ihrer Web-Anwendung funktioniert, sollte die Anwendung erstellt werden.

1. Aus der **Debuggen** klicken Sie im Menü **WingtipToys erstellen**.  
 Die **Ausgabe** Fenster wird angezeigt, und wenn alle gut funktioniert hat, sehen Sie eine *erfolgreich* Nachricht.  

    ![Erstellen der Datenzugriffsschicht - im Ausgabefenster](create_the_data_access_layer/_static/image4.png)

Wenn ein Fehler auftreten, überprüfen Sie die oben genannten Schritte erneut. Die Informationen in den **Ausgabe** Fenster wird angegeben, welche Datei ein Problem hat und, wenn in der Datei eine Änderung erforderlich ist. Diese Informationen können Sie bestimmen, welcher Teil der oben genannten Schritte überprüft und in Ihrem Projekt behoben werden müssen.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm der Reihe haben Sie das Datenmodell erstellt, als auch, den Code bereitgestellt, die zum Initialisieren und Ausgangswert für die Datenbank verwendet werden. Sie haben auch konfiguriert, dass die Anwendung die Datenmodelle verwendet, wenn die Anwendung ausgeführt wird.

In den nächsten Lernprogrammen aktualisiert die Benutzeroberfläche, Navigation hinzufügen und Abrufen von Daten aus der Datenbank. Dies führt in der Datenbank wird automatisch erstellt basierend auf der Entitätsklassen, die Sie in diesem Lernprogramm erstellt haben.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Übersicht über Entity Framework](https://msdn.microsoft.com/en-us/library/bb399567.aspx)   
[Einsteigerhandbuch zu ADO.NET Entity Framework](https://msdn.microsoft.com/en-us/data/ee712907)   
[Code First-Entwicklung mit Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (video)   
[Code First Beziehungen Fluent-API](https://msdn.microsoft.com/en-us/data/hh134698)   
[Erste Datenanmerkungen Code](https://msdn.microsoft.com/en-us/data/gg193958)  
[Steigerung der Produktivität für das Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

>[!div class="step-by-step"]
[Zurück](create-the-project.md)
[Weiter](ui_and_navigation.md)
