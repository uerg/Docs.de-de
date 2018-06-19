---
title: 'Razor-Seiten mit EF Core in ASP.NET Core: Migrationen (4 von 8)'
author: rick-anderson
description: In diesem Tutorial verwenden Sie zunächst das EF Core-Migrationsfeature für die Verwaltung von Datenmodelländerungen in einer ASP.NET Core MVC-App.
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: 690beaabeab098cf9b764730b1bf1bd04bf6b003
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "32740074"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Razor-Seiten mit EF Core in ASP.NET Core: Migrationen (4 von 8)

Von [Tom Dykstra](https://github.com/tdykstra), [Jon P. Smith](https://twitter.com/thereformedprog) und [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

In diesem Tutorial wird das EF Core-Migrationsfeature zur Verwaltung von Datenmodelländerungen verwendet.

Wenn nicht zu lösende Probleme auftreten, laden Sie die [abgeschlossene Anwendung für diese Phase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations) herunter.

Wenn eine neue App entwickelt wird, ändert sich das Datenmodell häufig. Nach jeder Modelländerung ist das Modell nicht mehr synchron mit der Datenbank. In diesem Tutorial wurde zunächst Entity Framework für die Erstellung der Datenbank konfiguriert, falls diese nicht vorhanden ist. Bei jeder Datenmodelländerung:

* Wird die Datenbank gelöscht.
* Erstellt EF eine neue, dem Modell entsprechende Datenbank.
* Füllt die App die Datenbank mit Testdaten auf.

Dieser Ansatz, bei dem die Datenbank mit dem Datenmodell synchron gehalten wird, funktioniert so lange, bis Sie die App für die Produktion bereitstellen. Wenn die App in der Produktionsumgebung ausgeführt wird, speichert sie in der Regel Daten, die verwaltet werden müssen. Jedes Mal, wenn eine Änderung vorgenommen wird (z.B. wenn eine neue Spalte hinzugefügt wird), kann die App nicht mit einer Testdatenbank starten. Mit der EF Core-Migrationsfeature wird dieses Problem gelöst, indem EF Core das Datenbankschema aktualisiert, anstatt eine neue Datenbank zu erstellen.

Statt die Datenbank bei den Datenmodelländerungen zu löschen und neu zu erstellen, wird bei der Migration eine Aktualisierung des Schemas und eine Beibehaltung vorhandener Daten angestrebt.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>NuGet-Pakete für Migrationen in Entity Framework Core

Verwenden Sie für die Arbeit mit Migrationen die **Paket-Manager-Konsole** (Package Manager Console, PMC) oder die Befehlszeilenschnittstelle (Command-Line Interface, CLI). In diesen Tutorials wird die Verwendungsweise von CLI-Befehlen erläutert. Informationen zur PMC finden Sie am [Ende dieses Tutorials](#pmc).

Die EF Core-Tools für die Befehlszeilenschnittstelle (Command-Line Interface, CLI) werden unter [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) bereitgestellt. Fügen Sie das Paket zum Installieren wie dargestellt zu der `DotNetCliToolReference`-Auflistung in der *.csproj*-Datei hinzu. **Hinweis:** Dieses Paket muss durch Bearbeitung der *.csproj*-Datei installiert werden. Dieses Paket kann nicht mit dem Befehl `install-package` oder über die grafische Benutzeroberfläche des Paket-Managers installiert werden. Bearbeiten Sie die *.csproj*-Datei, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen klicken und **Edit ContosoUniversity.csproj** („ContosoUniversity.csproj“ bearbeiten) auswählen.

Das folgende Markup zeigt die aktualisierte *.csproj*-Datei mit hervorgehobenen EF Core-CLI-Tools an:

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
Die Versionsnummern im vorgehenden Beispiel waren zum Zeitpunkt der Verfassung des Tutorials aktuell. Verwenden Sie die gleiche Version für die EF Core-CLI-Tools, die auch in den anderen Paketen gefunden wurde.

## <a name="change-the-connection-string"></a>Ändern der Verbindungszeichenfolge

Ändern Sie in der Datei *appsettings.json* den Namen der Datenbank in der Verbindungszeichenfolge in „ContosoUniversity2“.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Durch das Ändern des Datenbanknamens in der Verbindungszeichenfolge wird bei der ersten Migration eine neue Datenbank erstellt. Es wird eine neue Datenbank erstellt, da keine Datenbank mit diesem Namen vorhanden ist. Die Verbindungszeichenfolge muss für die ersten Schritte mit Migrationen nicht geändert werden.

Alternativ zum Ändern des Datenbanknamens kann die Datenbank auch gelöscht werden. Verwenden Sie den **SQL Server-Objekt-Explorer** (SSOX) oder den CLI-Befehl `database drop`:

 ```console
 dotnet ef database drop
 ```

Im folgenden Abschnitt wird erläutert, wie CLI-Befehle ausgeführt werden.

## <a name="create-an-initial-migration"></a>Erstellen einer ursprünglichen Migration

Erstellen Sie das Projekt.

Öffnen Sie ein Befehlsfenster, und navigieren Sie zu dem Projektordner. Der Projektordner enthält die Datei *Startup.cs*.

Geben Sie im Befehlsfenster Folgendes ein:

```console
dotnet ef migrations add InitialCreate
```

Im Befehlsfenster werden Informationen angezeigt, die in etwa wie folgt aussehen:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Wenn die Migration fehlschlägt und die Nachricht „*Auf die Datei „ContosoUniversity.dll“ kann nicht zugegriffen werden, da sie von einem anderen Prozess verwendet wird.*“ angezeigt wird:

* Beenden Sie IIS Express.

   * Beenden Sie Visual Studio, und führen Sie einen Neustart durch, oder
   * Suchen Sie in der Windows-Taskleiste nach dem Symbol „IIS Express“.
   * Klicken Sie mit der rechten Maustaste auf das Symbol „IIS Express“, und klicken Sie anschließend auf **ContosoUniversity > Website beenden**.

Wenn die Fehlermeldung „Fehler beim Buildvorgang.“ angezeigt wird, führen Sie den Befehl erneut aus. Hinterlassen Sie am Ende dieses Tutorials eine Notiz, wenn Ihnen diese Fehlermeldung angezeigt wird.

### <a name="examine-the-up-and-down-methods"></a>Überprüfen der Methoden „Up“ und „Down“

Der EF Core-Befehl `migrations add` hat Code generiert, aus dem die Datenbank erstellt werden soll. Dieser Migrationscode ist in der Datei *Migrations\<timestamp>_InitialCreate.cs* enthalten. Die Methode `Up` der Klasse `InitialCreate` erstellt die Datenbanktabellen, die den Datenmodellentitätenmengen entsprechen. Die Methode `Down` löscht diese, wie im folgenden Beispiel dargestellt:

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

Die Migrationsfunktion ruft die Methode `Up` auf, um die Datenmodelländerungen für eine Migration zu implementieren. Wenn Sie einen Befehl eingeben, um ein Rollback für das Update auszuführen, ruft die Migrationsfunktion die Methode `Down` auf.

Der vorangehende Code ist für die ursprüngliche Migration bestimmt. Dieser Code wurde bei der Ausführung des Befehls `migrations add InitialCreate` erstellt. Der Parameter für den Migrationsnamen (in dem Beispiel „InitialCreate“) wird für den Dateinamen verwendet. Der Migrationsname kann ein beliebiger gültiger Dateiname sein. Es wird empfohlen, ein Wort oder einen Ausdruck auszuwählen, durch das bzw. den der Hintergrund der Migration widergespiegelt wird. Eine Migration, bei der eine Tabelle mit Fachbereichen hinzugefügt wurde, könnte beispielsweise als „AddDepartmentTable“ bezeichnet werden.

Wenn die ursprüngliche Migration erstellt wird und die Datenbank vorhanden ist:

* Wird der Code für die Datenbankerstellung generiert.
* Muss der Code für die Datenbankerstellung nicht ausgeführt werden, da die Datenbank bereits mit dem Datenmodell übereinstimmt. Wenn der Code für die Datenbankerstellung ausgeführt wird, werden keine Änderungen vorgenommen, da die Datenbank bereits mit dem Datenmodell übereinstimmt.

Wenn die App in einer neuen Umgebung bereitgestellt wird, muss der Code für die Datenbankerstellung ausgeführt werden, damit die Datenbank erstellt wird.

Zuvor wurde die Verbindungszeichenfolge geändert, damit ein neuer Name für die Datenbank verwendet werden kann. Die angegebene Datenbank ist nicht vorhanden, daher wird die Datenbank bei der Migration erstellt.

### <a name="the-data-model-snapshot"></a>Die Momentaufnahme des Datenmodells

Die Migrationsfunktion erstellt unter *Migrations/SchoolContextModelSnapshot.cs* eine *Momentaufnahme* des aktuellen Datenbankschemas. Wenn Sie eine Migration hinzufügen, bestimmt EF die vorgenommenen Änderungen, indem das Datenmodell mit der Momentaufnahmedatei verglichen wird.

Verwenden Sie den Befehl [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove), wenn Sie eine Migration löschen. `dotnet ef migrations remove` löscht die Migration und stellt sicher, dass die Momentaufnahme ordnungsgemäß zurückgesetzt wird.

Weitere Informationen dazu, wie die Momentaufnahmedatei verwendet wird, finden Sie unter [EF Core Migrations in Team Environments (EF Core-Migrationen in Teamumgebungen)](/ef/core/managing-schemas/migrations/teams).

## <a name="remove-ensurecreated"></a>Entfernen von EnsureCreated

Zu einem frühen Entwicklungszeitpunkt wurde der Befehl `EnsureCreated` verwendet. In diesem Tutorial werden Migrationen verwendet. Der Befehl `EnsureCreated` weist folgende Einschränkungen auf:

* Er umgeht Migrationen und erstellt die Datenbank und das Schema.
* Er erstellt keine Migrationstabelle.
* Er kann *nicht* mit Migrationen verwendet werden.
* Er dient zum Testen oder schnellen Erstellen von Prototypen an Positionen, an denen die Datenbank häufig gelöscht und neu erstellt wird.

Entfernen Sie die folgende Zeile aus `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>Anwenden der Migration auf die Datenbank in der Entwicklung

Geben Sie im Befehlsfenster Folgendes ein, um die Datenbank und Tabellen zu erstellen.

```console
dotnet ef database update
```

Hinweis: Wenn der Befehl `update` den Fehler „Fehler beim Buildvorgang.“ zurückgibt:

* Führen Sie den Befehl erneut aus.
* Schlägt er erneut fehl, beenden Sie Visual Studio, und führen Sie anschließend den Befehl `update` aus.
* Hinterlassen Sie unten auf der Seite eine Nachricht.

Die Ausgabe des Befehls ähnelt der des Befehls `migrations add`. Im vorangehenden Befehl werden Protokolle für die SQL-Befehle angezeigt, mit denen die Datenbank eingerichtet wurde. In der folgenden Beispielausgabe werden die meisten Protokolle ausgelassen:

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

Wenn diese Detailebene in Protokollnachrichten nicht angezeigt werden soll, ändern Sie die Protokollebene in der Datei *appsettings.Development.json*. Weitere Informationen finden Sie unter [Introduction to Logging (Einführung in die Protokollierung)](xref:fundamentals/logging/index).

Verwenden Sie den **SQL Server-Objekt-Explorer** zur Untersuchung der Datenbank. Beachten Sie die zusätzliche Tabelle`__EFMigrationsHistory`. In der Tabelle `__EFMigrationsHistory` wird nachverfolgt, welche Migrationen auf die Datenbank angewendet wurden. Wenn Sie die Daten in der Tabelle `__EFMigrationsHistory` anzeigen, wird eine Zeile für die erste Migration angezeigt. Im letzten Protokoll im vorherigen Beispiel der CLI-Ausgabe wird die INSERT-Anweisung angezeigt, mit der diese Zeile erstellt wird.

Führen Sie die App aus, und überprüfen Sie, ob alles funktioniert.

## <a name="applying-migrations-in-production"></a>Anwenden von Migrationen in der Produktionsumgebung

Es wird empfohlen, dass Produktions-Apps [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) beim Anwendungsstart **nicht** aufrufen. `Migrate` sollte in der Serverfarm nicht über eine App aufgerufen werden. Beispielsweise wenn die App über eine Cloud mit horizontaler Skalierung bereitgestellt wurde (mehrere Instanzen der App werden ausgeführt).

Die Datenbankmigration sollte im Rahmen der Bereitstellung und auf kontrollierte Weise erfolgen. Zu den Ansätzen für die Migration von Produktionsdatenbanken zählen die folgenden:

* Verwendung von Migrationen zur Erstellung von SQL-Skripts und Verwendung der SQL-Skripts bei der Bereitstellung.
* Ausführen von `dotnet ef database update` über eine kontrollierte Umgebung.

EF Core kann über die Tabelle `__MigrationsHistory` sehen, ob Migrationen ausgeführt werden müssen. Wenn die Datenbank auf dem neuesten Stand ist, wird keine Migration ausgeführt.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Die Befehlszeilenschnittstelle (Command-line Interface, CLI) im Vergleich mit der Paket-Manager-Konsole (Package Manager Console, PMC) im Vergleich

Die EF Core-Tools für die Verwaltung von Migrationen sind verfügbar über:

* .NET Core-CLI-Befehle.
* Die PowerShell-Cmdlets im Visual Studio-Fenster **Paket-Manager-Konsole** (Package Manager Console, PMC).

In diesem Tutorial wird die Verwendungsweise der CLI erläutert. Einige Entwickler bevorzugen die Verwendung der PMC.

Die EF Core-Befehle für die PMC sind im Paket [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) enthalten. Dieses Paket ist im Metapaket [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) enthalten. Eine Installation ist daher nicht erforderlich.

**Wichtig:** Dieses Paket ist nicht mit dem Paket identisch, das Sie für die CLI durch Bearbeitung der Datei *.csproj* installiert haben. Der Name dieses Pakets endet mit `Tools`, im Gegensatz zum Namen des CLI-Pakets, der mit `Tools.DotNet` endet.

Weitere Informationen zu CLI-Befehlen finden Sie unter [.NET Core-CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Weitere Informationen zu den PMC-Befehlen finden Sie unter [Paket-Manager-Konsole (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Problembehandlung

Laden Sie die [abgeschlossene App für diese Phase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations) herunter.

Die App generiert folgende Ausnahme:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Lösung: Führen Sie `dotnet ef database update` aus.

Wenn der Befehl `update` den Fehler „Fehler beim Buildvorgang“ zurückgibt:

* Führen Sie den Befehl erneut aus.
* Hinterlassen Sie unten auf der Seite eine Nachricht.

> [!div class="step-by-step"]
> [Zurück](xref:data/ef-rp/sort-filter-page)
> [Weiter](xref:data/ef-rp/complex-data-model)
