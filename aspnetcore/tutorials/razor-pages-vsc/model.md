---
title: Hinzufügen eines Modells zu einer Razor-Seiten-App in ASP.NET Core mit Visual Studio Code
author: rick-anderson
description: Erfahren Sie, wie Sie ein Modell mithilfe von Visual Studio Code zu einer Razor-Seiten-App in ASP.NET Core hinzufügen.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 20282b491162e9f35e40702655532a78edceb89a
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a>Hinzufügen eines Modells zu einer Razor-Seiten-App in ASP.NET Core mit Visual Studio Code

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Hinzufügen eines Datenmodells

* Fügen Sie einen Ordner namens *Modelle* hinzu.
* Fügen Sie zum Ordner *Modelle* eine Klasse mit dem Namen *Movie.cs* hinzu.
* Fügen Sie der Datei *Models/Movie.cs* den folgenden Code hinzu:

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>NuGet-Pakete für Migrationen in Entity Framework Core

Die EF-Tools für die Befehlszeilenschnittstelle (CLI) werden unter [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) bereitgestellt. Fügen Sie das Paket zum Installieren zu der `DotNetCliToolReference`-Sammlung in der *CSPROJ*-Datei hinzu. **Hinweis:** Sie müssen dieses Paket installieren, indem Sie die *CSPROJ*-Datei bearbeiten. Sie können den `install-package`-Befehl oder die Paket-Manager-GUI nicht verwenden.

Bearbeiten Sie die Datei *RazorPagesMovie.csproj*:

* Klicken Sie auf **Datei** > **Datei öffnen**, und wählen Sie anschließend die Datei *RazorPagesMovie.csproj* aus.
* Fügen Sie der zweiten **\<ItemGroup>** einen Toolverweis für `Microsoft.EntityFrameworkCore.Tools.DotNet` hinzu:

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Aufbauen des Filmmodells

* Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).
* Führen Sie den folgenden Befehl aus:

**Hinweis: Führen Sie den folgenden Befehl unter Windows aus. Für MacOS und Linux gilt der nächste Befehl.**

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* Führen Sie unter macOS und Linux den folgenden Befehl aus:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Wenn Sie eine Fehlermeldung erhalten:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Beenden Sie Visual Studio, und führen Sie den Befehl erneut aus.

[!INCLUDE [model 4](../../includes/RP/model4.md)]

Im nächsten Tutorial finden Sie Erläuterungen zu den Dateien, die durch den Gerüstbau erstellt wurden.

> [!div class="step-by-step"]
> [Zurück: Erste Schritte](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Weiter: Gerüstbau mit Razor-Seiten](xref:tutorials/razor-pages-vsc/page)
