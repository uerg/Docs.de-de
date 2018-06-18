---
title: 'Razor-Seiten mit Entity Framework Core in ASP.NET Core: Tutorial 1 von 8'
author: rick-anderson
description: Informationen zum Erstellen einer Razor Pages-App mit Entity Framework Core
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: d7cf4740f31f1e0ae56461efc4c1b3d91238270f
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2018
ms.locfileid: "35726016"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>Razor-Seiten mit Entity Framework Core in ASP.NET Core: Tutorial 1 von 8

Von [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Die Beispiel-Web-App der Contoso University veranschaulicht, wie mit Entity Framework Core 2.0 und Visual Studio 2017 ASP.NET Core 2.0 MVC-Webanwendungen erstellt werden.

Bei der Beispiel-App handelt es sich um eine Website für die fiktive Contoso University. Sie enthält Funktionen wie die Zulassung von Studenten, die Erstellung von Kursen und Aufgaben von Dozenten. Dies ist die erste Seite eines mehrseitigen Tutorials, in dem die Erstellung der Beispiel-App der Contoso University erläutert wird.

[Download or view the completed app (Herunterladen oder anzeigen der vollständigen App).](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Anweisungen zum Download.](xref:tutorials/index#how-to-download-a-sample)

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE [](~/includes/net-core-prereqs.md)]

Kenntnisse über [Razor Pages](xref:mvc/razor-pages/index). Anfänger sollten den Artikel [Erste Schritte mit Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start) lesen, bevor sie mit diesem Tutorial beginnen.

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie auf ein Problem stoßen, das Sie nicht lösen können, sollten Sie Ihren Code mit der [abgeschlossenen Phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) vergleichen. Eine Liste mit häufig auftretenden Fehlern und den jeweiligen Lösungen finden Sie im Abschnitt [zur Fehlerbehebung auf im letzten Tutorial dieser Tutorialreihe](xref:data/ef-mvc/advanced#common-errors). Wenn Sie dort nicht die gewünschten Informationen finden, können Sie unter [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) für [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) oder [Entity Framework Core](https://stackoverflow.com/questions/tagged/entity-framework-core) eine Frage posten.

> [!TIP]
> Dieses Tutorial basiert auf anderen älteren Tutorials. Sie sollten jedes Mal, wenn Sie erfolgreich ein Tutorial abgeschlossen haben, eine Kopie des Projekts erstellen. Wenn Sie dann auf Probleme stoßen, können Sie zurück zum vorherigen Tutorial gehen und müssen nicht wieder ganz von vorne beginnen. Stattdessen können Sie aber auch eine [abgeschlossene Phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) herunterladen und diese zum Starten verwenden.

## <a name="the-contoso-university-web-app"></a>Die Web-App der Contoso University

Bei der App, die mithilfe dieser Tutorials erstellt werden soll, handelt es sich um eine einfache Website einer Universität.

Benutzer können Informationen zu den Studenten, Kursen und Dozenten abrufen. Im Folgenden sind einige Anzeigen dargestellt, die mithilfe dieses Tutorials erstellt werden.

![Indexseite „Studenten“](intro/_static/students-index.png)

![Bearbeitungsseite für Studenten](intro/_static/student-edit.png)

Der Benutzeroberflächenstil dieser Website ähnelt den durch die integrierten Vorlagen generierten Seiten. In diesem Tutorial wird Entity Framework Core mit Razor Pages thematisiert. Die Benutzeroberfläche wird nicht erläutert.

## <a name="create-a-razor-pages-web-app"></a>Erstellen einer Razor Pages-Web-App

* Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** > **Projekt**.
* Erstellen Sie eine neue ASP.NET Core-Webanwendung. Geben Sie dem Projekt den Namen **ContosoUniversity**. Es ist wichtig, dass Sie dem Projekt exakt diesen Namen geben, sodass die Namespaces übereinstimmen, wenn der Code kopiert und eingefügt wird.
 ![neue ASP.NET Core-Webanwendung](intro/_static/np.png)
* Wählen Sie in der Dropdownliste **ASP.NET Core 2.0** aus, und klicken Sie anschließend auf **Webanwendung**.
 ![Webanwendung (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)

Drücken Sie **F5**, um die App im Debugmodus auszuführen, oder **STRG+F5** zur Ausführung ohne Anfügen des Debuggers.

## <a name="set-up-the-site-style"></a>Einrichten des Websitestils

Sie können das Websitemenü, das Layout und die Startseite über einige Änderungen einrichten.

Öffnen Sie *Pages/_Layout.cshtml*, und nehmen Sie die folgenden Änderungen vor:

* Ändern Sie jedes „ContosoUniversity“ in „Contoso University“. Diese Begriffskombination kommt dreimal vor.

* Fügen Sie Menüeinträge für **Studenten**, **Kurse**, **Dozenten** und **Abteilungen** hinzu, und löschen Sie den Menüeintrag **Kontakt**.

Die Änderungen werden hervorgehoben. (Es wird *nicht* das gesamte Markup angezeigt.)

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

Ersetzen Sie in *Pages/Index.cshtml* die Inhalte der Datei durch den folgenden Code. Dadurch ersetzen Sie den Text zu ASP.NET und MVC durch Text zu dieser App:

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

Drücken Sie STRG+F5, um das Projekt auszuführen. Im Folgenden wird die Startseite mit den Registerkarten angezeigt, die in den folgenden Tutorials erstellt wurden:

![Contoso University-Startseite](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>Erstellen des Datenmodells

Erstellen Sie Entitätsklassen für die Contoso University-App. Beginnen Sie mit dem folgenden drei Entitäten:

![Datenmodelldiagramm zur Kursanmeldung für Studenten](intro/_static/data-model-diagram.png)

Es besteht eine 1:n-Beziehung zwischen der `Student`- und der `Enrollment`-Entität. Es besteht eine 1:n-Beziehung zwischen der `Course`- und der `Enrollment`-Entität. Jeder Student kann sich für eine beliebige Anzahl von Kursen anmelden. Für jeden Kurs kann sich eine beliebige Anzahl von Studenten anmelden.

In den folgenden Abschnitten wird für jede dieser Entitäten eine Klasse erstellt.

### <a name="the-student-entity"></a>Die Entität „Student“

![Entitätsdiagramm „Student“](intro/_static/student-entity.png)

Erstellen Sie einen Ordner *Models* (Modelle). Erstellen Sie mit dem folgenden Code im Ordner *Models* (Modelle) eine Klassendatei mit dem Namen *Student.cs*:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

Die `ID`-Eigenschaft fungiert als Primärschlüsselspalte der Datenbanktabelle, die dieser Klasse entspricht. Standardmäßig interpretiert Entity Framework Core eine Eigenschaft mit dem Namen `ID` oder `classnameID` als Primärschlüssel. In `classnameID` ist `classname` der Name der Klasse, im vorherigen Beispiel z.B. `Student`.

Die `Enrollments`-Eigenschaft ist eine Navigationseigenschaft. Navigationseigenschaften werden mit anderen Entitäten verknüpft, die dieser Entität zugehörig sind. In diesem Fall enthält die `Enrollments`-Eigenschaft einer `Student entity` all diese `Enrollment`-Entitäten, die mit diesem `Student` in Zusammenhang stehen. Wenn es z.B. für eine „Student“-Zeile in der Datenbank zwei zugehörige „Enrollment“-Zeilen gibt, enthält die Navigationseigenschaft `Enrollments` zwei `Enrollment`-Entitäten. Bei einer zugehörigen `Enrollment`-Zeile handelt es sich um eine Zeile, die den Wert des Primärschlüssels des Studenten in der `StudentID`-Spalte enthält. Nehmen Sie z.B. an, dass es für den Studenten mit ID=1 zwei Zeilen in der `Enrollment`-Tabelle gibt. Die `Enrollment`-Tabelle verfügt über zwei Zeilen mit `StudentID`=1. Bei `StudentID` handelt es sich um einen Fremdschlüssel in der `Enrollment`-Tabelle, der den Studenten in der `Student`-Tabelle angibt.

Wenn in einer Navigationseigenschaft mehrere Entitäten gespeichert werden können, muss es sich um einen Listentyp wie `ICollection<T>` handeln. `ICollection<T>` oder ein Typ wie `List<T>` oder `HashSet<T>` können angegeben werden. Wenn `ICollection<T>` verwendet wird, erstellt Entity Framework Core standardmäßig eine `HashSet<T>`-Auflistung. Navigationseigenschaften, die mehrere Entitäten enthalten, entstehen auf der Grundlage von m:n- oder 1:n-Beziehungen.

### <a name="the-enrollment-entity"></a>Die Entität „Enrollment“

![Entitätsdiagramm „Enrollment“](intro/_static/enrollment-entity.png)

Erstellen Sie mit dem folgenden Code im Ordner *Models* (Modelle) eine *Enrollment.cs*-Datei:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

Bei der `EnrollmentID`-Eigenschaft handelt es sich um den Primärschlüssel. Diese Entität verwendet genau wie die `Student`-Entität das `classnameID`-Muster anstelle von `ID`. In der Regel wählen Entwickler ein Muster aus und verwenden dieses im gesamten Datenmodell. In einem der nächsten Tutorials wird erläutert, wie Sie eine ID ohne Klassennamen verwenden, um die Vererbung einfacher in das Datenmodell zu implementieren.

Bei der `Grade`-Eigenschaft handelt es sich um eine `enum`. Das Fragezeichen nach der `Grade`-Typdeklaration gibt an, dass die `Grade`-Eigenschaft NULL-Werte zulässt. Eine Grade-Eigenschaft mit dem Wert NULL unterscheidet sich von einer Grade-Eigenschaft mit dem Wert 0 (null). Der Wert NULL bedeutet, dass keine Grade-Eigenschaft bekannt ist oder noch keine zugewiesen wurde.

Bei der `StudentID`-Eigenschaft handelt es sich um einen Fremdschlüssel und `Student` ist die entsprechende Navigationseigenschaft. Die `Enrollment`-Entität wird einer `Student`-Entität zugewiesen. Das bedeutet, dass die Eigenschaft genau eine `Student`-Entität enthält. Die `Student`-Entität unterscheidet sich von der `Student.Enrollments`-Navigationseigenschaft, die mehrere `Enrollment`-Entitäten enthält.

Bei der `CourseID`-Eigenschaft handelt es sich um einen Fremdschlüssel, und `Course` ist die entsprechende Navigationseigenschaft. Die `Enrollment`-Entität wird einer `Course`-Entität zugeordnet.

Entity Framework Core interpretiert Eigenschaften als Fremdschlüssel, wenn diese den Namen `<navigation property name><primary key property name>` haben. Beispielsweise `StudentID` für die `Student`-Navigationseigenschaft, da `ID` der Primärschlüssel der `Student`-Entität ist. Fremdschlüsseleigenschaften können ebenfalls den Namen `<primary key property name>` haben. Beispielsweise `CourseID`, da `CourseID` der Primärschlüssel der `Course`-Entität ist.

### <a name="the-course-entity"></a>Die Entität „Course“

![Entitätsdiagramm „Course“](intro/_static/course-entity.png)

Erstellen Sie mit dem folgenden Code im Ordner *Models* (Modelle) eine *Course.cs*-Datei:

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

Die `Enrollments`-Eigenschaft ist eine Navigationseigenschaft. `Course`-Entitäten können sich auf jede beliebige Anzahl von `Enrollment`-Entitäten beziehen.

Das `DatabaseGenerated`-Attribut lässt es zu, dass die App den Primärschlüssel angibt, sodass die Datenbank diesen nicht generieren muss.

## <a name="create-the-schoolcontext-db-context"></a>Erstellen des Datenbankkontexts „SchoolContext“

Bei der Datenbankkontextklasse handelt es sich um die Hauptklasse, die die Entity Framework Core-Funktionen für ein angegebenes Datenmodell koordiniert. Der Datenkontext wird von `Microsoft.EntityFrameworkCore.DbContext` abgeleitet. Der Datenkontext gibt an, welche Entitäten im Datenmodell enthalten sind. In diesem Projekt heißt die Klasse `SchoolContext`.

Erstellen Sie im Projektordner einen Ordner mit dem Namen *Data* (Daten).

Erstellen Sie im Ordner *Data* (Daten) mit dem folgenden Code die *SchoolContext.cs*-Datei:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Dieser Code erstellt eine `DbSet`-Eigenschaft für jede Entitätenmenge. Das heißt für Entity Framework Core:

* Entitätenmengen entsprechen in der Regel einer Datenbanktabelle.
* Entitäten entsprechen Zeilen in Tabellen.

`DbSet<Enrollment>` und `DbSet<Course>` können ausgelassen werden. Diese sind implizit in Entity Framework Core enthalten, da die `Student`-Entität auf die `Enrollment`-Entität verweist, und die `Enrollment`-Entität auf die `Course`-Entität verweist. Behalten Sie für dieses Tutorial `DbSet<Enrollment>` und `DbSet<Course>` im `SchoolContext`-Kontext.

Wenn die Datenbank erstellt wird, erstellt Entity Framework Core Tabellen mit Namen, die den `DbSet`-Eigenschaftennamen entsprechen. Eigenschaftennamen für Auflistungen stehen in der Regel im Plural (Students anstelle von Student). Entwickler sind sich uneinig darüber, ob Tabellennamen im Plural stehen sollten. In diesen Tutorials wird das Standardverhalten außer Kraft gesetzt, indem im DbContext Tabellennamen im Singular angegeben werden. Fügen Sie den folgenden hervorgehobenen Code hinzu, um Tabellennamen im Singular anzugeben:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Registrieren des Kontexts durch Dependency Injection

[Dependency Injection](xref:fundamentals/dependency-injection) ist in ASP.NET Core enthalten. Dienste (z.B. der Datenbankkontext Entity Framework Core) werden über Dependency Injection beim Anwendungsstart registriert. Komponenten, die diese Dienste erfordern (z.B. Razor Pages), werden von diesen Diensten über Konstruktorparameter bereitgestellt. Der Konstruktorcode, der eine Datenbankkontextinstanz abruft, wird später in diesem Tutorial erläutert.

Öffnen Sie die *Startup.cs-Datei*, und fügen Sie der `ConfigureServices`-Methode die hervorgehobenen Zeilen hinzu, um `SchoolContext` als Dienst zu registrieren.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

Der Name der Verbindungszeichenfolge wird an den Kontext übergeben, indem Sie eine Methode auf einem `DbContextOptionsBuilder`-Objekt aufrufen. Für die lokale Entwicklung liest das [ASP.NET Core-Konfigurationssystem](xref:fundamentals/configuration/index) die Verbindungszeichenfolge aus der *appsettings.json*-Datei.

Fügen Sie für die Namespaces `ContosoUniversity.Data` und `Microsoft.EntityFrameworkCore` die `using`-Anweisungen hinzu. Erstellen Sie das Projekt.

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Öffnen Sie die *appsettings.json*-Datei, und fügen Sie wie im folgenden Code dargestellt eine Verbindungszeichenfolge hinzu:

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

Die darauf folgende Verbindungszeichenfolge verwendet `ConnectRetryCount=0`, um zu vermeiden, dass [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) nicht mehr reagiert.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Die Verbindungszeichenfolge gibt eine SQL Server-LocalDB-Datenbank an. LocalDB ist eine Basisversion der SQL Server Express-Datenbank-Engine, die zwar für die Anwendungsentwicklung, aber nicht für den Produktionseinsatz bestimmt ist. LocalDB wird bedarfsgesteuert gestartet und im Benutzermodus ausgeführt, sodass keine komplexe Konfiguration anfällt. Standardmäßig erstellt LocalDB *.mdf*-Datenbankdateien im `C:/Users/<user>`-Verzeichnis.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Hinzufügen von Code zum Initialisieren der Datenbank mithilfe von Testdaten

Entity Framework Core erstellt eine leere Datenbank. In diesem Abschnitt wird eine *Seed*-Methode geschrieben, um diese mit Testdaten aufzufüllen.

Erstellen Sie im Ordner *Data* eine neuen Klassendatei mit dem Namen *DbInitializer.cs*, und fügen Sie den folgenden Code hinzu:

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

Der Code überprüft, ob Studenten in der Datenbank enthalten sind. Wenn keine Studenten in der Datenbank enthalten sind, wird für diese ein Seeding mit Testdaten ausgeführt. Testdaten werden in Arrays anstelle von `List<T>`-Auflistungen geladen, um die Leistung zu optimieren.

Die `EnsureCreated`-Methode erstellt automatisch die Datenbank für den Datenbankkontext. Wenn die Datenbank vorhanden ist, wird `EnsureCreated` zurückgegeben, ohne Änderungen an der Datenbank vorzunehmen.

Ändern Sie in der *Program.cs*-Datei die `Main`-Methode, um die folgenden Vorgänge auszuführen:

* Rufen Sie eine Datenbankkontextinstanz aus dem Dependency Injection-Container ab.
* Rufen Sie die Seedmethode auf, indem Sie den Kontext an diese übergeben.
* Löschen Sie den Kontext, wenn die Seedmethode abgeschlossen ist.

Der folgende Code zeigt die aktualisierte *Program.cs*-Datei.

[!code-csharp[](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

Wenn die App das erste Mal ausgeführt wird, wird die Datenbank erstellt, und es wird ein Seeding mit Testdaten ausgeführt. Gehen Sie wie folgt vor, wenn ein Update für das Datenmodell ausgeführt wird:
* Löschen Sie die Datenbank.
* Führen Sie ein Update für die Seedmethode aus.
* Führen Sie die App aus. Dann wird eine neue Datenbank erstellt, für die ein Seeding ausgeführt wurde. 

In späteren Tutorials wird ein Update für die Datenbank ausgeführt, wenn das Datenmodell geändert wird, ohne die Datenbank zu löschen und neu zu erstellen.

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>Hinzufügen von Gerüsttools

In diesem Abschnitt wird die Paket-Manager-Konsole verwendet, um das Visual Studio-Paket zur Webcodegenerierung hinzuzufügen. Dieses Paket ist für die Ausführung der Gerüstbau-Engine erforderlich.

Öffnen Sie das Menü **Extras**, und wählen Sie **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.

Geben Sie die folgenden Befehle in die Paket-Manager-Konsole ein:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

Über den obenstehenden Befehl werden die NuGet-Pakete zu der *.csproj-Datei hinzugefügt:

[!code-xml[](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>Aufbauen des Modells

* Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).
* Führen Sie die folgenden Befehle aus:


 ```console
dotnet restore
dotnet tool install --global dotnet-aspnet-codegenerator --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```

Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).


Erstellen Sie das Projekt. Der Build generiert z.B. die folgenden Fehler:

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 Ändern Sie `_context.Student` allgemein in `_context.Students` (d.h., fügen Sie ein „s“ zu `Student` hinzu). Der Begriff wurde siebenmal gefunden und aktualisiert. [Dieser Fehler](https://github.com/aspnet/Scaffolding/issues/633) sollte im nächsten Release behoben werden.

[!INCLUDE [model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>Testen der App

Führen Sie die App aus, und klicken Sie auf den Link **Students**. Abhängig von der Breite Ihres Browsers wird der Link **Students** im oberen Bereich der Seite angezeigt. Wenn der Link **Students** nicht angezeigt wird, klicken Sie auf das Navigationssymbol in der oberen rechten Ecke.

![Contoso University-Startseite verkleinert](intro/_static/home-page-narrow.png)

Testen Sie die Links **Erstellen**, **Bearbeiten** und **Details**.

## <a name="view-the-db"></a>Abrufen der Datenbank

Wenn die App gestartet wird, ruft `DbInitializer.Initialize` `EnsureCreated` auf. `EnsureCreated` ermittelt, ob die Datenbank vorhanden ist, und erstellt ggf. eine. Wenn keine Studenten in der Datenbank enthalten sind, werden über die `Initialize`-Methode Studenten hinzugefügt.

Öffnen Sie über das Menü **Ansicht** im Visual Studio **SQL Server-Objekt-Explorer** (SSOX).
Klicken Sie im SSOX auf **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.

Erweitern Sie den Knoten **Tabellen**.

Klicken Sie mit der rechten Maustaste auf die Tabelle **Student**, und klicken Sie auf **Daten anzeigen**, um die erstellten Spalten und die in die Tabelle eingefügten Zeilen aufzurufen.

Die Datenbankdateien <em>.mdf</em> und <em>.ldf</em> befinden sich im Ordner <em>C:\Users\\<yourusername></em>.

`EnsureCreated` wird beim Starten der App aufgerufen, wodurch der folgende Workflow ermöglicht wird:

* Löschen Sie die Datenbank.
* Ändern Sie das Datenbankschema, also fügen Sie z.B. ein `EmailAddress`-Feld hinzu.
* Führen Sie die App aus.

Über `EnsureCreated` wird eine Datenbank mit der `EmailAddress`-Spalte erstellt.

## <a name="conventions"></a>Konventionen

Es wurde nur wenig Code in die Reihenfolge für Entity Framework Core geschrieben, um eine vollständige Datenbank zu erstellen, da Konventionen oder Annahmen von Entity Framework verwendet wurden.

* Die Namen der `DbSet`-Eigenschaften werden als Tabellennamen verwendet. Für Entitäten, auf die nicht über eine `DbSet`-Eigenschaft verwiesen wird, werden Entitätsklassennamen als Tabellennamen verwendet.

* Eigenschaftennamen von Entitäten werden als Spaltennamen verwendet.

* Entitätseigenschaften mit dem Namen „ID“ oder „classnameID“ werden als Primärschlüsseleigenschaften erkannt.

* Eigenschaften werden als Fremdschlüsseleigenschaften interpretiert, wenn Sie den Namen *<navigation property name><primary key property name>* haben – z.B. `StudentID` für die `Student`-Navigationseigenschaft, da der Primärschlüssel der `Student`-Entität `ID` lautet. Fremdschlüsseleigenschaften können den Namen *<primary key property name>* haben – z.B. `EnrollmentID`, da der Primärschlüssel der `Enrollment`-Entität `EnrollmentID` lautet.

Konventionelles Verhalten kann überschrieben werden. Beispielsweise können die Tabellennamen wie bereits in diesem Tutorial gezeigt explizit angegeben werden. Die Spaltennamen können explizit angegeben werden. Primärschlüssel und Fremdschlüssel können explizit festgelegt werden.

## <a name="asynchronous-code"></a>Asynchroner Code

Die asynchrone Programmierung ist der Standardmodus für ASP.NET Core und EF Core.

Der Webserver verfügt nur über eine begrenzte Anzahl von Threads. Daher werden bei hoher Auslastung möglicherweise alle verfügbaren Threads gleichzeitig verwendet. Wenn dies der Fall ist, kann der Server keine neuen Anforderungen verarbeiten, bis die Threads wieder freigegeben werden. Wenn synchroner Code verwendet wird, kann es sein, dass zwar viele Threads belegt sind, diese aber keine Vorgänge ausführen, da sie auf den Abschluss der E/A-Vorgänge warten. Wenn asynchroner Code verwendet wird, werden Threads für den Server freigegeben, wenn diese nur auf den Abschluss der E/A-Vorgänge warten, damit andere Anforderungen verarbeitet werden können. Das bedeutet, dass es durch asynchronen Code ermöglicht wird, Serverressourcen effizienter zu nutzen, und der Server kann ohne Verzögerungen eine größere Menge von Datenverkehr verarbeiten.

Zur Laufzeit bedeutet die Verwendung von asynchronem Code allerdings, dass ein wenig mehr Aufwand entsteht. Für Situationen mit wenig Datenverkehr haben diese Leistungseinbußen keine negativen Folgen. Wenn es jedoch eine große Menge an Datenverkehr gibt, ist eine potentielle Verbesserung der Leistung von Bedeutung.

Im folgenden Code haben das `async`-Schlüsselwort, der `Task<T>`-Rückgabewert, das `await`-Schlüsselwort und die `ToListAsync`-Methode zur Folge, dass der Code auf asynchrone Weise ausgeführt wird.

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* Das `async`-Schlüsselwort gibt dem Compiler folgende Anweisungen:

  * Er soll Rückrufe für Teile des Methodentexts generieren.
  * Er soll automatisch das [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7)-Objekt erstellen, das zurückgegeben wird. Weitere Informationen finden Sie unter [Aufgabenrückgabetyp](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Der implizite Rückgabetyp `Task` steht für die derzeit ausgeführte Arbeit.

* Das `await`-Schlüsselwort hat zur Folge, dass der Compiler die Methode in zwei Teile unterteilt. Der erste Teil endet mit dem Vorgang, der auf asynchrone Weise gestartet wird. Der zweite Teil wird in eine Rückrufmethode übertragen, die aufgerufen wird, wenn der Vorgang abgeschlossen wird.

* Bei `ToListAsync` handelt es sich um die asynchrone Version der `ToList`-Erweiterungsmethode.

Behalten Sie Folgendes im Hinterkopf, wenn Sie asynchronen Code schreiben, der Entity Framework Core verwendet:

* Es werden nur Anweisungen auf asynchrone Weise ausgeführt, die Abfragen oder Befehle auslösen, die an die Datenbank gesendet werden sollen. Dazu gehören `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` und `SaveChangesAsync`. Anweisungen wie `var students = context.Students.Where(s => s.LastName == "Davolio")`, die nur eine `IQueryable`-Instanz ändern, sind davon ausgeschlossen.

* Entity Framework Core-Kontexte sind nicht threadsicher. Versuchen Sie daher nicht, mehrere Vorgänge gleichzeitig auszuführen. 

* Wenn Sie von den Leistungsvorteilen durch asynchronen Code profitieren möchten, überprüfen Sie, ob Bibliothekspakete (z.B. zum Paging) asynchronen Code verwenden, wenn sie Entity Framework Core-Methoden aufrufen, die Abfragen an die Datenbank senden.

Weitere Informationen zur asynchronen Programmierung in .NET finden Sie unter [Async (Übersicht)](/dotnet/articles/standard/async).

Im nächsten Tutorial erfahren Sie mehr über die CRUD-Vorgänge (Create, Read, Update, Delete = Erstellen, Lesen, Aktualisieren, Löschen).

> [!div class="step-by-step"]
> [Nächste](xref:data/ef-rp/crud)
