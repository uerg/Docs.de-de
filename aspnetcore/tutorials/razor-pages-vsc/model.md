---
title: Hinzufügen eines Modells zu einer Razor Pages-App in ASP.NET Core mit Visual Studio Code
author: rick-anderson
description: Erfahren Sie, wie Sie ein Modell mithilfe von Visual Studio Code zu einer Razor Pages-App in ASP.NET Core hinzufügen.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244722"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a>Hinzufügen eines Modells zu einer Razor Pages-App in ASP.NET Core mit Visual Studio Code

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Hinzufügen eines Datenmodells

* Fügen Sie einen Ordner namens *Modelle* hinzu.
* Fügen Sie zum Ordner *Modelle* eine Klasse mit dem Namen *Movie.cs* hinzu.
* Fügen Sie der Datei *Models/Movie.cs* den folgenden Code hinzu:

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a>NuGet-Pakete für SQLite für Entity Framework Core

Führen Sie in der Befehlszeile den folgenden .NET Core-CLI-Befehl aus:

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a>Registrieren des Datenbankkontexts

Registrieren Sie den Datenbankkontext mit dem [Abhängigkeitsinjektionscontainer](xref:fundamentals/dependency-injection) in der Datei *Startup.cs*.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

Fügen Sie am Anfang der Datei *Startup.cs* die folgenden `using`-Anweisungen ein.

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a>Aufbauen des Filmmodells

* Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).
* **Für Windows**: Führen Sie den folgenden Befehl aus:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **Für macOS und Linux**: Führen Sie den folgenden Befehl aus:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

Im nächsten Tutorial finden Sie Erläuterungen zu den Dateien, die durch den Gerüstbau erstellt wurden.

> [!div class="step-by-step"]
> [Zurück: Erste Schritte](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Weiter: Gerüstbau mit Razor Pages](xref:tutorials/razor-pages-vsc/page)
