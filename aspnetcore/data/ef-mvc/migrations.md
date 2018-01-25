---
title: ASP.NET Core MVC mit EF-Kern - Migrationen - 4 von 10
author: tdykstra
description: "In diesem Lernprogramm beginnen Sie mit der EF-Core-Migrationen-Funktion für die Verwaltung von datenmodelländerungen in einer ASP.NET-MVC-Anwendung Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/migrations
ms.openlocfilehash: a2f8b01e16d1be818b4338455a40605fcbdb3400
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a>Migrationen - EF-Core mit ASP.NET Core MVC-Lernprogramm (4 von 10)

Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen, die mit Entity Framework Core und Visual Studio. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](intro.md).

In diesem Lernprogramm beginnen Sie mit der EF-Core-Migrationen-Funktion für die Verwaltung von datenmodelländerungen. In späteren Lernprogrammen fügen Sie weitere Migrationen, wenn Sie das Datenmodell ändern.

## <a name="introduction-to-migrations"></a>Einführung in die Migrationen

Wenn Sie eine neue Anwendung entwickeln, Ihr Datenmodell ändert sich häufig, und jedes Mal das Modell ändert, ruft nicht synchron mit der Datenbank ab. Mit diesen Lernprogrammen wird gestartet, durch die Konfiguration des Entity Framework zum Erstellen der Datenbank, wenn er nicht vorhanden ist. Klicken Sie dann jedes Mal, die Sie ändern das Datenmodell – hinzufügen, entfernen, Entitätsklassen oder ändern die DbContext-Klasse können Sie die Datenbank löschen und EF ein neues Zertifikat an, das das Modell entspricht und startet ihn mit Testdaten erstellt haben.

Synchronisieren der Datenbank mit dem Datenmodell diese Methode funktioniert gut, bis Sie die Anwendung bis hin zur Produktion bereitstellen. Wenn die Anwendung in der produktionsumgebung ausgeführt wird, es in der Regel, die Daten, die Sie beibehalten möchten gespeichert sind, und Sie nicht alles, was bei jedem verlieren möchten, nehmen Sie eine Änderung wie z. B. das Hinzufügen einer neuen Spalte. Das Feature EF Core Migrationen löst dieses Problem durch Aktivieren von EF zum Aktualisieren des Datenbankschemas statt eine neue Datenbank erstellen.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>NuGet-Pakete für Migrationen in Entity Framework Core

Um mit Migrationen arbeiten, können Sie die **Package Manager Console** (PMC) oder die Befehlszeilenschnittstelle (CLI).  Diese Lernprogramme veranschaulichen, wie CLI-Befehlen. Informationen zu der Systemmonitor ist am [Ende dieses Lernprogramms](#pmc).

Die EF-Tools für die Befehlszeilenschnittstelle (CLI) werden unter [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) bereitgestellt. Um dieses Paket zu installieren, fügen sie der `DotNetCliToolReference` Sammlung in der *csproj* Datei wie gezeigt. **Hinweis:** Sie müssen dieses Paket installieren, indem Sie die *CSPROJ*-Datei bearbeiten. Sie können den `install-package`-Befehl oder die Paket-Manager-GUI nicht verwenden. Können Sie bearbeiten die *csproj* Datei mit der rechten Maustaste auf den Projektnamen im **Projektmappen-Explorer** auswählen und **ContosoUniversity.csproj bearbeiten**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(Die Versionsnummern in diesem Beispiel wurden beim Erstellen des Lernprogramms geschrieben wurde.) 

## <a name="change-the-connection-string"></a>Ändern der Verbindungszeichenfolge

In der *appsettings.json* Datei, ändern Sie den Namen der Datenbank in der Verbindungszeichenfolge ContosoUniversity2 oder einen anderen Namen, die Sie verwenden, die Sie nicht auf dem Computer verwendet haben.

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

Diese Änderung richtet das Projekt ein, damit die erste Migration eine neue Datenbank erstellt wird. Dies ist nicht erforderlich für erste Schritte mit Migrationen, aber Sie werden später angezeigt, daher ist es eine gute Idee bleiben.

> [!NOTE]
> Als Alternative zum Ändern des Datenbanknamens können Sie die Datenbank löschen. Verwendung **Objekt-Explorer von SQL Server** (SSOX) oder die `database drop` CLI-Befehl:
> ```console
> dotnet ef database drop
> ```
> Im folgende Abschnitt wird erläutert, wie CLI-Befehle ausgeführt werden.

## <a name="create-an-initial-migration"></a>Erstellen einer anfänglichen Migrations

Speichern Sie die Änderungen zu, und erstellen Sie das Projekt. Klicken Sie dann öffnen Sie ein Befehlsfenster, und navigieren Sie zum Projektordner. Hier ist eine schnelle Möglichkeit hierzu:

* In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **im Datei-Explorer öffnen** aus dem Kontextmenü.

  ![Öffnen Sie im Datei-Explorer-Menüelement](migrations/_static/open-in-file-explorer.png)

* Geben Sie "Cmd" in der Adressleiste ein, und drücken Sie die EINGABETASTE.

  ![Befehlsfenster öffnen](migrations/_static/open-command-window.png)

Geben Sie im Befehlsfenster folgenden Befehl ein:

```console
dotnet ef migrations add InitialCreate
```

Die Ausgabe wie folgt im Befehlsfenster angezeigt:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Wenn Sie eine Fehlermeldung *keine ausführbare Datei gefunden übereinstimmenden Befehl "Dotnet-Ef"*, finden Sie unter [diesem Blogbeitrag](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) für Hilfe zur Problembehandlung.

Wenn Sie eine Fehlermeldung angezeigt "*... die Datei kann nicht zugegriffen werden. ContosoUniversity.dll, da sie von einem anderen Prozess verwendet wird.* ", suchen Sie das Symbol" IIS Express "in der Windows-Taskleiste der rechten Maustaste darauf klicken, und klicken Sie auf **ContosoUniversity > Stop Standort**.

## <a name="examine-the-up-and-down-methods"></a>Überprüfen Sie die nach-oben und nach-unten Sie-Methoden

Wenn Sie die Ausführung der `migrations add` Befehl EF generiert den Code, der die Datenbank von Grund auf neu erstellt wird. Dieser Code befindet sich in der *Migrationen* Ordner, in der Datei mit dem Namen  *\<Zeitstempel > _InitialCreate.cs*. Die `Up` Methode der `InitialCreate` Klasse erstellt, die Datenbanktabellen, die die Daten Modell Entitätenmengen, entsprechen und die `Down` Methode löscht, wie im folgenden Beispiel gezeigt.

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Migrationen Aufrufe der `Up` Methode für die Implementierung des Datenmodells für eine Migration. Bei der Eingabe eines Befehls ein Rollback der Updates, die Migrationen Aufrufe der `Down` Methode.

Dieser Code ist für den anfänglichen Migration, die erstellt wurde, wenn Sie eingegeben haben die `migrations add InitialCreate` Befehl. Der Namensparameter der Migration (im Beispiel "InitialCreate") wird verwendet, für den Dateinamen und kann beliebig sein. Es wird empfohlen, wählen ein Wort oder Ausdruck, die zusammengefasst, was bei der Migration durchgeführt wird. Sie können z. B. eine spätere Migrationen "AddDepartmentTable" nennen.

Wenn Sie die anfängliche Migration erstellt, wenn die Datenbank bereits vorhanden ist, wird die Erstellung Datenbankcode generiert, aber keinen ausführen, da die Datenbank bereits das Datenmodell entspricht. Während der Bereitstellung der app in eine andere Umgebung, in dem die Datenbank ist nicht vorhanden, noch dieser Code wird ausgeführt, um Ihre Datenbank zu erstellen, daher ist es eine gute Idee, zuerst zu testen. Warum ist Sie den Namen der Datenbank in der Verbindungszeichenfolge zuvor--geändert, sodass Migrationen eine neue von Grund auf neu erstellen können.

## <a name="examine-the-data-model-snapshot"></a>Überprüfen Sie die datenmomentaufnahme für das Modell

Migrationen erstellt außerdem eine *Momentaufnahme* des aktuellen Datenbankschemas in *Migrations/SchoolContextModelSnapshot.cs*. Dieses Codes sieht z. B. aus:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Da das Schema der aktuellen Datenbank im Code dargestellt wird, verwendet nicht EF Kern, für die Interaktion mit der Datenbank, um Migrationen zu erstellen. Wenn Sie eine Migration hinzufügen, bestimmt EF an, was durch Vergleichen Datenmodell der Datenbankmomentaufnahme-Datei geändert. EF interagiert mit der Datenbank nur, wenn sie zum Aktualisieren der Datenbank hat. 

Die Datenbankmomentaufnahme-Datei muss mit der Migrationen synchron gehalten werden, die sie erstellt haben, damit Sie eine Migration nicht entfernen können, indem Sie einfach das Löschen der Datei mit dem Namen  *\<Zeitstempel > _\<Migrationname > .cs*. Wenn Sie diese Datei löschen, werden die übrigen Migrationen werden nicht mit der Datenbankmomentaufnahme-Datei synchron. Um der letzten Migration löschen, die Sie hinzugefügt haben, verwenden die [Dotnet Ef Migrationen entfernen](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) Befehl.

## <a name="apply-the-migration-to-the-database"></a>Die Migration auf die Datenbank anwenden.

Geben Sie in das Befehlsfenster den folgenden Befehl an der Datenbank und die Tabellen erstellt werden.

```console
dotnet ef database update
```

Die Ausgabe des Befehls ähnelt der `migrations add` Befehl, mit dem Unterschied, dass Protokolle anzuzeigen, für die SQL-, die die Datenbank eingerichtet Befehle. Die meisten Protokolle werden in der folgenden Beispielausgabe weggelassen. Wenn Sie nicht diese Detailebene in Protokollnachrichten anzeigen möchten, können, ändern Sie die Protokollebene in den *"appSettings". Development.JSON* Datei. Weitere Informationen finden Sie unter [Einführung in die Protokollierung](xref:fundamentals/logging/index).

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

Verwendung **Objekt-Explorer von SQL Server** , die Datenbank zu überprüfen, wie Sie im ersten Lernprogramm ausgeführt haben.  Sie werden das Hinzufügen einer Tabelle __EFMigrationsHistory bemerken, die überwacht die Migrationen auf die Datenbank angewendet wurden. Zeigen Sie die Daten in dieser Tabelle, und sehen Sie eine Zeile für die erste Migration. (Das letzte Protokoll im vorherigen Beispiel der CLI-Ausgabe zeigt die INSERT-Anweisung, die diese Zeile erstellt.)

Führen Sie die Anwendung aus, um sicherzustellen, dass alles noch funktioniert gleichermaßen wie vor.

![Indexseite für Studenten](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Im Vergleich zur Befehlszeilenschnittstelle (CLI) Paket-Manager-Konsole (PMC)

Der EF-Tools für die Verwaltung von Migrationen aus .NET Core-CLI-Befehlen oder PowerShell-Cmdlets in der Visual Studio verfügbar ist **Package Manager Console** (PMC) Fenster. Dieses Lernprogramm zeigt, wie die CLI, aber Sie können die PMC verwenden, falls gewünscht.

Der EF-Befehle für die PMC-Befehle werden in der [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) Paket. Dieses Paket ist bereits in enthalten die [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) Metapackage, daher ist es nicht, es zu installieren.

**Wichtig:** dieses Element wird nicht das gleiche Paket mit dem Sie für die CLI, indem Sie die Bearbeitung Installieren der *csproj* Datei. Der Name dieser endet `Tools`, im Gegensatz zu den CLI-Paketnamen die endet in `Tools.DotNet`.

Weitere Informationen zu CLI-Befehlen finden Sie unter [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet). 

Weitere Informationen zu den PMC-Befehlen finden Sie unter [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gesehen, wie zum Erstellen und Anwenden der ersten Migrations. In den nächsten Lernprogrammen beginnen Sie erweiterte Themen ansehen, erweitern Sie das Datenmodell. Nebenbei erstellen und Anwenden von zusätzliche Migrationen.

>[!div class="step-by-step"]
[Zurück](sort-filter-page.md)
[Weiter](complex-data-model.md)  
