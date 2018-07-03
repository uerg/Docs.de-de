---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Erstellen die Datenzugriffsebene | Microsoft-Dokumentation
author: Erikre
description: Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 5b4bb5bc89938836bc37e8ebd385fa966bc6f511
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390029"
---
<a name="create-the-data-access-layer"></a>Erstellen der Datenzugriffsebene
====================
durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen der Wingtip Toys-Beispielprojekts (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [E-Book (PDF) herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die dieser tutorialreihe begleitet.


Dieses Tutorial beschreibt das Erstellen, Zugriff auf und überprüfen Daten aus einer Datenbank mithilfe von ASP.NET Web Forms und Entity Framework Code First. Dieses Tutorial baut auf dem vorherigen Lernprogramm "Dem Projekt erstellen" und ist Teil der tutorialreihe Wingtip Toys-Store. Wenn Sie dieses Tutorial abgeschlossen haben, Sie erstellt eine Gruppe von Datenzugriffs-Klassen, die in der *Modelle* -Ordner des Projekts.

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- Vorgehensweise: Erstellen der Datenmodelle.
- Informationen zum Initialisieren und das Seeding für die Datenbank.
- Informationen zum Aktualisieren und konfigurieren Sie die Anwendung aus, um die Datenbank zu unterstützen.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Dies sind die Funktionen, die in diesem Lernprogramm eingeführt:

- Entitätsframework Code First
- LocalDB
- Datenanmerkungen

## <a name="creating-the-data-models"></a>Erstellen die Datenmodelle

[Entitätsframework](https://msdn.microsoft.com/data/aa937723) ist ein objektrelationales Mapping (ORM)-Framework. Sie können die Arbeit mit relationalen Daten als Objekte, die Eliminierung der meisten des Datenzugriffs-Codes, den Sie in der Regel schreiben müssen. Sie können mithilfe von Entity Framework-Abfragen mit ausgeben [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), abrufen und Bearbeiten von Daten als stark typisierte Objekte. LINQ stellt Muster zum Abfragen und Aktualisieren von Daten bereit. Mithilfe von Entity Framework können Sie sich auf den Rest der Anwendung erstellen, anstatt sich auf die Daten Zugriff Grundlagen konzentrieren. Weiter unten in dieser tutorialreihe zeigen wir Ihnen so die Daten zum Auffüllen der Navigation und Produkt-Abfragen verwenden.

Entitätsframework unterstützt ein Development-Paradigma namens *Code First*. Code First können Sie Ihre Datenmodelle, die mithilfe von Klassen zu definieren. Eine Klasse ist ein Konstrukt, das Ihnen ermöglicht, eigene benutzerdefinierten Typen zu erstellen, durch das Gruppieren von Variablen anderer Typen, Methoden und Ereignisse. Sie können die map-Klassen zu einer vorhandenen Datenbank oder verwenden, um eine Datenbank zu erstellen. In diesem Tutorial erstellen Sie die Datenmodelle von datenmodellklassen schreiben. Anschließend können Sie Entity Framework die Datenbank dynamisch von diesen neuen Klassen zu erstellen.

Sie beginnt, durch das Erstellen von Entitätsklassen, die die Datenmodellen für die Web Forms-Anwendung zu definieren. Klicken Sie dann erstellen Sie eine Kontextklasse, die die Entitätsklassen verwaltet und ermöglicht den Datenzugriff in der Datenbank. Sie erstellen auch eine Initialisiererklasse, die Sie verwenden, um die Datenbank zu füllen.

### <a name="entity-framework-and-references"></a>Entitätsframework und Verweise

Standardmäßig Entity Framework ist enthalten, beim Erstellen einer neuen **ASP.NET-Webanwendung** mithilfe der **Web Forms** Vorlage. Entitätsframework kann werden installiert, deinstalliert und als NuGet-Paket aktualisiert werden.

Dieses NuGet-Paket enthält die folgenden **Runtime** Assemblys in Ihrem Projekt:

- EntityFramework.dll – alle, die von Entity Framework verwendet häufig Laufzeitcode
- EntityFramework.SqlServer.dll – die Microsoft SQL Server-Anbieter für Entity Framework

### <a name="entity-classes"></a>Entitätsklassen

Die Klassen, die Sie erstellen, um das Schema der Daten zu definieren, werden als Entitätsklassen bezeichnet. Wenn Sie auf den Datenbankentwurf vertraut sind, stellen Sie sich die Entitätsklassen wie Tabellendefinitionen einer Datenbank. Jede Eigenschaft in der Klasse gibt eine Spalte in der Tabelle der Datenbank. Diese Klassen bieten eine einfache, objektrelationale-Schnittstelle zwischen objektorientiertem Code und die Struktur der relationalen Tabelle der Datenbank.

In diesem Tutorial beginnen Sie damit durch das Hinzufügen von einfachen Entitätsklassen, die die Schemas für Kategorien und Produkte darstellt. Die Products-Klasse enthält die Definitionen für jedes Produkt. Der Name der einzelnen der Elemente der Product-Klasse `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, und `Category`. Die Kategorie-Klasse enthält Definitionen für jede Kategorie, die ein Produkt, z.B. Auto, also oder Datenebene gehören können. Der Name der einzelnen Member der Klasse Kategorie `CategoryID`, `CategoryName`, `Description`, und `Products`. Jedes Produkt werden zu einer der Kategorien gehören. Diese Entitätsklassen werden hinzugefügt, vorhandene des Projekts *Modelle* Ordner.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Modelle* Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**. 

    ![Erstellen der Datenzugriffsebene - Menü "Neues Element"](create_the_data_access_layer/_static/image1.png)

   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Klicken Sie unter **Visual C#-** aus der **installiert** Bereich auf der linken Seite auf **Code**. 

    ![Erstellen der Datenzugriffsebene - Menü "Neues Element"](create_the_data_access_layer/_static/image2.png)
3. Wählen Sie **Klasse** im mittleren Bereich, und nennen Sie diese neue Klasse *Product.cs*.
4. Klicken Sie auf **Hinzufügen**.  
   Die neue Klassendatei wird im Editor angezeigt.
5. Ersetzen Sie den Standardcode durch folgenden Code:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Erstellen Sie eine andere Klasse durch Wiederholen der Schritte 1 bis 4, nennen Sie die neue Klasse allerdings *Category.cs* , und Ersetzen Sie den Standardcode durch folgenden Code:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Wie bereits erwähnt, die `Category` -Klasse stellt der Typ des Produkts, das die Anwendung ist verkauft werden soll (z. B. <a id="a"> </a> &quot;Autos&quot;, &quot;Boote&quot;, &quot;Raketen&quot;und so weiter), und die `Product` Klasse darstellt, die einzelnen Produkte (Toys) in der Datenbank. Jede Instanz einer `Product` Objekt wird eine Zeile in einer relationalen Datenbanktabelle entsprechen, und jede Eigenschaft der Product-Klasse an eine Spalte in der relationalen Datenbanktabelle zugeordnet wird. Prüfen Sie weiter unten in diesem Tutorial die Produktdaten an, in der Datenbank enthalten.

### <a name="data-annotations"></a>Datenanmerkungen

Ihnen möglicherweise aufgefallen, dass bestimmte Member der Klassen angeben von Details zum Element, z. B. Attribute haben `[ScaffoldColumn(false)]`. Hierbei handelt es sich *datenanmerkungen*. Die datenanmerkungsattribute können beschrieben, wie zum Validieren von Benutzereingaben für diesen Member, die Formatierung für es festlegen und angeben, wie es modelliert wird, wenn die Datenbank erstellt wird.

### <a name="context-class"></a>Context-Klasse

Um mithilfe der Klassen für den Datenzugriff zu starten, müssen Sie eine Kontextklasse definieren. Wie bereits erwähnt, wird die Context-Klasse verwaltet die Entitätsklassen (z. B. die `Product` Klasse und die `Category` Klasse) und ermöglicht den Datenzugriff in der Datenbank.

Diese Prozedur fügt eine neue C#-Kontext-Klasse, die *Modelle* Ordner.

1. Mit der rechten Maustaste die *Modelle* Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**.   
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie **Klasse** im mittleren Bereich Namen *ProductContext.cs* , und klicken Sie auf **hinzufügen**.
3. Ersetzen Sie den Standardcode, der in der Klasse durch den folgenden Code enthalten:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Dieser Code fügt die `System.Data.Entity` Namespace sodass Sie Zugriff auf die Kernfunktionen des Entity Framework, gehört die Möglichkeit, Abfragen, einfügen, aktualisieren und Löschen von Daten durch Verwendung stark typisierter Objekte.

Die `ProductContext` Klasse darstellt, Entity Framework-Produkt-Datenbankkontext, der verarbeitet wird, abrufen, speichern und Aktualisieren von `Product` -Klasseninstanzen in der Datenbank. Die `ProductContext` Klasse leitet sich von der `DbContext` Basisklasse, die von Entity Framework bereitgestellt werden.

### <a name="initializer-class"></a>Initialisiererklasse

Sie müssen zum Ausführen der benutzerdefinierten Logik zum Zeitpunkt der Datenbank die erste zu initialisieren, die der Kontext verwendet wird. Dies ermöglicht die Seed-Daten der Datenbank hinzugefügt werden, damit Sie sofort Produkte und Kategorien angezeigt werden können.

Diese Prozedur fügt eine neue C#-Initialisierer-Klasse, die *Modelle* Ordner.

1. Erstellen Sie eine weitere `Class` in die *Modelle* Ordner und nennen Sie sie *ProductDatabaseInitializer.cs*.
2. Ersetzen Sie den Standardcode, der in der Klasse durch den folgenden Code enthalten:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Wie Sie aus dem obigen Code sehen können, wenn die Datenbank erstellt und initialisiert, die `Seed` Eigenschaft außer Kraft gesetzt und festgelegt wird. Wenn die `Seed` -Eigenschaft festgelegt ist, die Werte aus den Kategorien und Produkte werden verwendet, um die Datenbank zu füllen. Wenn Sie versuchen, aktualisieren die Seed-Daten, indem Sie im oben stehenden Code ändern, nachdem die Datenbank erstellt wurde, wird beim Ausführen der Webanwendung keine Updates angezeigt. Der Grund hierfür ist der obige Code verwendet die Implementierung der `DropCreateDatabaseIfModelChanges` Klasse, wenn das Modell (Schema) geändert wurde, vor dem Zurücksetzen der Seed-Daten zu erkennen. Wenn keine Änderungen, um vorgenommen werden die `Category` und `Product` Entitätsklassen, die Datenbank werden mit den seedingdaten nicht erneut initialisiert werden.

> [!NOTE] 
> 
> Wenn Sie beispielsweise, dass die Datenbank jedes Mal neu erstellt werden, Sie haben die Anwendung ausgeführt, könnten Sie mit der `DropCreateDatabaseAlways` -Klasse anstelle der `DropCreateDatabaseIfModelChanges` Klasse. Verwenden Sie für diese tutorialreihe, jedoch die `DropCreateDatabaseIfModelChanges` Klasse.


An diesem Punkt in diesem Tutorial haben Sie haben eine *Modelle* Ordner mit vier neuen Klassen und eine Standardklasse:

![Erstellen der Datenzugriffsebene - Ordner "Models"](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Die Anwendung konfigurieren so, verwenden Sie das Datenmodell

Nun, dass Sie die Klassen erstellt haben, die Daten darstellen, müssen Sie die Anwendung zur Verwendung der Klassen konfigurieren. In der *"Global.asax"* -Datei, fügen Sie Code hinzu, die das Modell initialisiert. In der *"Web.config"* Datei, die Sie hinzufügen, Informationen, die der Anwendung mitteilt, welche Datenbank verwenden, um die Daten zu speichern, die durch die neuen Datenklassen dargestellt wird. Die *"Global.asax"* Datei kann verwendet werden, um Anwendungsereignisse oder Methoden zu behandeln. Die *"Web.config"* Datei können Sie die Konfiguration von Ihrer ASP.NET-Webanwendung steuern.

#### <a name="updating-the-globalasax-file"></a>Aktualisieren die Datei "Global.asax"

Um die Datenmodelle beim Starten der Anwendung zu initialisieren, aktualisieren Sie die `Application_Start` Ereignishandler in der *"Global.asax.cs"* Datei.

> [!NOTE] 
> 
> Im Projektmappen-Explorer können Sie wählen Sie entweder die *"Global.asax"* Datei oder das *"Global.asax.cs"* Datei so bearbeiten Sie die *"Global.asax.cs"* Datei.


1. Fügen Sie folgenden Code in Gelb zu markiert die `Application_Start` -Methode in der die *"Global.asax.cs"* Datei.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Ihr Browser muss HTML5 zum Anzeigen des Codes in gelb hervorgehoben werden, wenn dieser tutorialreihe in einem Browser anzeigen unterstützen.


Wie im obigen Code wird angezeigt, wenn die Anwendung gestartet wird, gibt die Anwendung an, dass der Initialisierer, der beim ersten Mal Daten ausgeführt wird, zugegriffen wird. Die beiden zusätzlichen Namespaces sind erforderlich, für den Zugriff auf die `Database` Objekt und die `ProductDatabaseInitializer` Objekt.

 Die Datei "Web.config" ändern 

Hinzufügen eigener Verbindungsinformationen zu Ihrer Anwendung auch Entity Framework Code First, eine Datenbank für Sie an einem Standardspeicherort generieren wird Wenn die Datenbank mit Seed-Daten aufgefüllt wird, erhalten Sie Kontrolle über den Speicherort der Datenbank. Sie geben diese datenbankverbindung mithilfe einer Verbindungszeichenfolge in der Anwendung *"Web.config"* Datei im Stammverzeichnis des Projekts. Hinzufügen eine neue Verbindungszeichenfolge an, Sie können den Speicherort der Datenbank verweisen (*wingtiptoys.mdf*), die im Datenverzeichnis der Anwendung erstellt werden (*App\_Daten*), anstatt der Standardeinstellung Speicherort. Durch diese Änderung können Sie zum Suchen und überprüfen die Datenbankdatei für die weiter unten in diesem Tutorial.

1. In **Projektmappen-Explorer**suchen oder öffnen Sie die *"Web.config"* Datei.
2. Hinzufügen der Verbindungszeichenfolge in Gelb zu markiert die `<connectionStrings>` Teil der *"Web.config"* -Datei wie folgt:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Wenn die Anwendung zum ersten Mal ausgeführt wird, wird es die Datenbank an dem von der Verbindungszeichenfolge angegebenen Speicherort erstellt werden. Aber vor dem Ausführen der Anwendung, erstellen Sie es zuerst.

## <a name="building-the-application"></a>Erstellen der Anwendung

Um sicherzustellen, dass alle Klassen und Änderungen an Ihrer Webanwendung arbeiten, sollten Sie die Anwendung zu erstellen.

1. Von der **Debuggen** , wählen Sie im Menü **erstellen WingtipToys**.  
 Die **Ausgabe** Fenster angezeigt wird, und wenn alles geklappt hat, sehen Sie eine *erfolgreich* Nachricht.  

    ![Erstellen der Datenzugriffsebene - Ausgabe-Windows](create_the_data_access_layer/_static/image4.png)

Wenn ein Fehler auftreten, sollten überprüfen Sie die obigen Schritte erneut. Die Informationen in den **Ausgabe** Fenster wird angegeben, welche Datei auf ein Problem hat und, wenn in der Datei eine Änderung erforderlich ist. Diese Informationen können Sie bestimmen, welcher Teil der oben genannten Schritte überprüft und in Ihrem Projekt behoben werden müssen.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial der Reihe haben Sie das Datenmodell erstellt, als auch, den Code, der verwendet wird, initialisieren und das Seeding der Datenbank hinzugefügt. Sie haben auch konfiguriert, dass die Anwendung die Datenmodelle zu verwenden, wenn die Anwendung ausgeführt wird.

Im nächsten Tutorial müssen Sie die Benutzeroberfläche aktualisiert, Navigation hinzufügen und Abrufen von Daten aus der Datenbank. Dies führt in der Datenbank wird automatisch erstellt basierend auf der Entitätsklassen, die Sie in diesem Tutorial erstellt haben.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Übersicht über Entity Framework](https://msdn.microsoft.com/library/bb399567.aspx)   
[Einführung in das ADO.NET Entity Framework](https://msdn.microsoft.com/data/ee712907)   
[Code First-Entwicklung mit Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (video)   
[Code First Beziehungen Fluent-API](https://msdn.microsoft.com/data/hh134698)   
[Code der ersten Datenanmerkungen](https://msdn.microsoft.com/data/gg193958)  
[Die Produktivitätsverbesserungen für das Entitätsframework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Zurück](create-the-project.md)
> [Weiter](ui_and_navigation.md)
