---
title: 'ASP.NET Core MVC mit Entity Framework Core (EF Core): Migrationen (4 von 10)'
author: rick-anderson
description: In diesem Tutorial verwenden Sie zunächst die EF Core-Migrationsfeatures für die Verwaltung von Datenmodelländerungen in einer ASP.NET Core MVC-Anwendung.
ms.author: tdykstra
ms.date: 03/15/2018
uid: data/ef-mvc/migrations
ms.openlocfilehash: 556d7d4ad05679ebfce6c909b29610482bb3f350
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011467"
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a>ASP.NET Core MVC mit Entity Framework Core (EF Core): Migrationen (4 von 10)

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Von [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen mit Entity Framework Core und Visual Studio erstellt werden. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](intro.md).

In diesem Tutorial verwenden Sie zunächst die EF Core-Migrationsfeature für die Verwaltung von Datenmodelländerungen. In späteren Tutorials fügen Sie weitere Migrationen hinzu, wenn Sie das Datenmodell ändern.

## <a name="introduction-to-migrations"></a>Einführung in die Migrationsfunktion

Wenn Sie eine neue Anwendung entwickeln, ändert sich Ihr Datenmodell häufig. Jedes Mal, wenn das Datenmodell geändert wird, ist es nicht mehr synchron mit der Datenbank. Sie haben zu Beginn dieser Tutorials Entity Framework für die Erstellung der Datenbank konfiguriert, sofern diese nicht vorhanden war. Anschließend können Sie bei jeder Datenmodelländerung (beim Hinzufügen, Entfernen oder Ändern von Entitätsklassen oder beim Ändern Ihrer DbContext-Klasse) die Datenbank löschen. EF erstellt dann eine neue Datenbank, die dem Modell entspricht, und startet diese mit Testdaten.

Diese Methode, bei der die Datenbank mit dem Datenmodell synchron gehalten wird, funktioniert so lange, bis Sie die Anwendung für die Produktion bereitstellen. Wenn sich die Anwendung in der Produktion befindet, speichert sie in der Regel die Daten, die Sie weiter verwenden möchten, da nicht bei jeder Änderung, wie z.B. dem Hinzufügen einer neuen Spalte, alle Daten verloren gehen sollen. Mit der EF Core-Migrationsfeature wird dieses Problem gelöst, indem EF das Datenbankschema aktualisiert, statt eine neue Datenbank zu erstellen.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>NuGet-Pakete für Migrationen in Entity Framework Core

Für die Arbeit mit Migrationen können Sie die **Paket-Manager-Konsole** (Package Manager Console, PMC) oder die Befehlszeilenschnittstelle (Command-Line Interface, CLI) verwenden.  In diesen Tutorials wird die Verwendungsweise von CLI-Befehlen erläutert. Informationen zur PMC finden Sie am [Ende dieses Tutorials](#pmc).

Die EF-Tools für die Befehlszeilenschnittstelle (CLI) werden unter [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) bereitgestellt. Fügen Sie das Paket zum Installieren wie dargestellt zu der `DotNetCliToolReference`-Sammlung in der *CSPROJ*-Datei hinzu. **Hinweis:** Sie müssen dieses Paket installieren, indem Sie die *CSPROJ*-Datei bearbeiten. Sie können den `install-package`-Befehl oder die Paket-Manager-GUI nicht verwenden. Sie können die *CSPROJ*-Datei bearbeiten, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen klicken und dann auf **„ContosoUniversity.csproj“ bearbeiten** klicken.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(Die Versionsnummern in diesem Beispiel waren zum Zeitpunkt der Verfassung des Tutorials aktuell.)

## <a name="change-the-connection-string"></a>Ändern der Verbindungszeichenfolge

Ändern Sie in der Datei *appsettings.json* in der Verbindungszeichenfolge den Namen der Datenbank in „ContosoUniversity2“ oder in einen anderen Namen, den Sie auf Ihrem Computer noch nicht verwendet haben.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Durch diese Änderung wird das Projekt eingerichtet, sodass bei der ersten Migration eine neue Datenbank erstellt wird. Dies ist für die ersten Schritte mit Migrationen nicht erforderlich, aber Sie werden später feststellen, weshalb es sich empfiehlt, entsprechend vorzugehen.

> [!NOTE]
> Alternativ zum Ändern des Datenbanknamens können Sie die Datenbank auch löschen. Verwenden Sie den **SQL Server-Objekt-Explorer** (SSOX) oder den CLI-Befehl `database drop`:
> ```console
> dotnet ef database drop
> ```
> Im folgenden Abschnitt wird erläutert, wie CLI-Befehle ausgeführt werden.

## <a name="create-an-initial-migration"></a>Erstellen einer ursprünglichen Migration

Speichern Sie Ihre Änderungen, und erstellen Sie das Projekt. Öffnen Sie anschließend ein Befehlsfenster, und navigieren Sie zu dem Projektordner. Sie können diesen Vorgang auf folgende Weise beschleunigen:

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie aus dem Kontextmenü die Option **In Datei-Explorer öffnen** aus.

  ![Menüelement „In Datei-Explorer öffnen“](migrations/_static/open-in-file-explorer.png)

* Geben Sie in die Adressleiste „cmd“ ein, und drücken Sie die EINGABETASTE.

  ![Öffnen des Befehlsfensters](migrations/_static/open-command-window.png)

Geben Sie im Befehlsfenster folgenden Befehl ein:

```console
dotnet ef migrations add InitialCreate
```

Im Befehlsfenster wird Ihnen eine Ausgabe wie die folgende angezeigt:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Wenn Ihnen die Fehlermeldung *Es wurde keine ausführbare Datei gefunden, die dem Befehl „dotnet-ef“ entspricht.* angezeigt wird, finden Sie unter [diesem Blogbeitrag](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) Hilfe zur Problembehandlung.

Wenn Ihnen die Fehlermeldung *Auf die Datei „ContosoUniversity.dll“ kann nicht zugegriffen werden, da sie von einem anderen Prozess verwendet wird.* angezeigt wird, suchen Sie in der Windows-Taskleiste nach dem Symbol „IIS Express“, klicken Sie mit der rechten Maustaste darauf, und klicken Sie anschließend auf **ContosoUniversity > Website beenden**.

## <a name="examine-the-up-and-down-methods"></a>Überprüfen der Methoden „Up“ und „Down“

Wenn Sie den Befehl `migrations add` ausgeführt haben, hat EF den Code generiert, mit dem die Datenbank von Grund auf neu erstellt wird. Dieser Code befindet sich im Ordner *Migrationen* in der Datei *\<timestamp>_InitialCreate.cs*. Mit der Methode `Up` der `InitialCreate`-Klasse werden die Datenbanktabellen erstellt, die den Datenmodellentitäten entsprechen, und mit der Methode `Down` werden die Datenbanktabellen gelöscht (siehe folgendes Beispiel).

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Die Migrationsfunktion ruft die Methode `Up` auf, um die Datenmodelländerungen für eine Migration zu implementieren. Wenn Sie einen Befehl eingeben, um ein Rollback für das Update auszuführen, ruft die Migrationsfunktion die Methode `Down` auf.

Dieser Code ist für die ursprüngliche Migration vorgesehen, die bei der Eingabe des Befehls `migrations add InitialCreate` erstellt wurde. Der Parameter für den Migrationsnamen (in dem Beispiel „InitialCreate“) wird für den Dateinamen verwendet und kann einen beliebigen Namen haben. Es wird empfohlen, ein Wort oder einen Ausdruck auszuwählen, durch das bzw. den der Hintergrund der Migration widergespiegelt wird. Sie können einer späteren Migration beispielsweise den Namen „AddDepartmentTable“ geben.

Wenn Sie die ursprüngliche Migration erstellt haben, als die Datenbank bereits vorhanden war, wird der Code für die Datenbankerstellung zwar generiert, allerdings muss er nicht ausgeführt werden, da die Datenbank bereits dem Datenmodell entspricht. Wenn Sie die App in einer anderen Umgebung bereitstellen, in der die Datenbank noch nicht vorhanden ist, wird dieser Code ausgeführt, um Ihre Datenbank zu erstellen. Daher sollte dieser zunächst getestet werden. Aus diesem Grund haben Sie den Namen der Datenbank zuvor in der Verbindungszeichenfolge geändert – damit eine Datenbank bei Migrationen von Grund auf neu erstellt werden kann.

## <a name="the-data-model-snapshot"></a>Die Momentaufnahme des Datenmodells

Die Migrationsfunktion erstellt unter *Migrations/SchoolContextModelSnapshot.cs* eine *Momentaufnahme* des aktuellen Datenbankschemas. Wenn Sie eine Migration hinzufügen, bestimmt EF die vorgenommenen Änderungen, indem das Datenmodell mit der Momentaufnahmedatei verglichen wird.

Verwenden Sie den Befehl [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove), wenn Sie eine Migration löschen. `dotnet ef migrations remove` löscht die Migration und stellt sicher, dass die Momentaufnahme ordnungsgemäß zurückgesetzt wird.

Weitere Informationen dazu, wie die Momentaufnahmedatei verwendet wird, finden Sie unter [EF Core Migrations in Team Environments (EF Core-Migrationen in Teamumgebungen)](/ef/core/managing-schemas/migrations/teams).

## <a name="apply-the-migration-to-the-database"></a>Anwenden der Migration auf die Datenbank

Geben Sie folgenden Befehl in das Befehlsfenster ein, um die Datenbank mit den darin enthaltenen Tabellen zu erstellen.

```console
dotnet ef database update
```

Die Ausgabe des Befehls ähnelt der des Befehls `migrations add`. Der Unterschied besteht darin, dass Ihnen Protokolle für die SQL-Befehle angezeigt werden, mit denen die Datenbank eingerichtet wird. In der folgenden Beispielausgabe werden die meisten Protokolle ausgelassen. Wenn diese Detailebene in Protokollnachrichten nicht angezeigt werden soll, können Sie die Protokollebene in der Datei *appsettings.Development.json* ändern. Weitere Informationen finden Sie unter [Introduction to Logging (Einführung in die Protokollierung)](xref:fundamentals/logging/index).

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

Verwenden Sie den **SQL Server-Objekt-Explorer**, um die Datenbank wie im ersten Tutorial zu untersuchen.  Sie werden feststellen, dass die Tabelle „__EFMigrationsHistory“ hinzugefügt wurde, in der nachverfolgt wird, welche Migrationen auf die Datenbank angewendet worden sind. Wenn Sie die Daten in dieser Tabelle anzeigen, ist eine Zeile für die erste Migration zu sehen. (Im letzten Protokoll im vorgehenden Beispiel der CLI-Ausgabe wird die INSERT-Anweisung angezeigt, mit der diese Zeile erstellt wird.)

Führen Sie die Anwendung aus, um zu überprüfen, ob alles wie gewohnt funktioniert.

![Indexseite „Studenten“](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Die Befehlszeilenschnittstelle (Command-line Interface, CLI) im Vergleich mit der Paket-Manager-Konsole (Package Manager Console, PMC)

Die EF-Tools für die Verwaltung von Migrationen sind über .NET Core-CLI-Befehle oder über PowerShell-Cmdlets im Visual Studio-Fenster **Paket-Manager-Console** (PMC) verfügbar. In diesem Tutorial wird die Verwendungsweise der CLI erläutert. Sie können aber auch die PMC verwenden, wenn Sie diese bevorzugen.

Die EF-Befehle für die PMC-Befehle sind im Paket [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) enthalten. Dieses Paket ist bereits im Metapaket [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) enthalten. Eine Installation ist daher nicht erforderlich.

**Wichtig:** Dieses Paket ist nicht mit dem Paket identisch, das Sie für die CLI durch Bearbeitung der *CSPROJ*-Datei installiert haben. Der Name dieses Pakets endet mit `Tools`, im Gegensatz zum Namen des CLI-Pakets, der mit `Tools.DotNet` endet.

Weitere Informationen zu CLI-Befehlen finden Sie unter [.NET Core-CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Weitere Informationen zu den PMC-Befehlen finden Sie unter [Paket-Manager-Konsole (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie Sie Ihre erste Migration erstellen und anwenden. Im nächsten Tutorial befassen Sie sich mit erweiterten Themen, indem Sie das Datenmodell erweitern. Dabei werden Sie weitere Migrationen erstellen und anwenden.

::: moniker-end

> [!div class="step-by-step"]
> [Zurück](sort-filter-page.md)
> [Weiter](complex-data-model.md)
