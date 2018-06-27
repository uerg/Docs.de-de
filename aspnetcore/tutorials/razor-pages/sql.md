---
title: Arbeiten mit SQL Server LocalDB und ASP.NET Core
author: rick-anderson
description: Informationen zum Arbeiten mit SQL Server LocalDB und ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 92a5965e7a535ca729c0bec13911b6bf051a7b19
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582868"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a>Arbeiten mit SQL Server LocalDB und ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette) 

Das `MovieContext`-Objekt übernimmt die Aufgabe der Herstellung der Verbindung mit der Datenbank und Zuordnung von `Movie`-Objekten zu Datensätzen in der Datenbank. Der Datenbankkontext wird mit dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in der Methode `ConfigureServices` in der Datei *Startup.cs* registriert:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Weitere Informationen über die in `ConfigureServices` verwendeten Methoden finden Sie unter:

* [Unterstützung für die Datenschutz-Grundverordnung (DSGVO) in ASP.NET Core](xref:security/gdpr) für `CookiePolicyOptions`.
* [SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

Das ASP.NET Core-[Konfigurationssystem](xref:fundamentals/configuration/index) liest die `ConnectionString`. Für die lokale Entwicklung wird die Verbindungszeichenfolge aus der Datei *appsettings.json* abgerufen. Der Name-Wert für die Datenbank (`Database={Database name}`) ist für den generierten Code anders. Beim Name-Wert handelt es sich um einen beliebigen Wert.

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

Wenn Sie die App auf einem Test- oder Produktionsserver bereitstellen, können Sie eine Umgebungsvariable oder eine andere Vorgehensweise zum Festlegen der Verbindungszeichenfolge für einen tatsächlichen SQL Server-Computer verwenden. Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB ist eine Lightweightversion der SQL Server Express-Datenbank-Engine, die für die Programmentwicklung geeignet ist. LocalDB wird bedarfsgesteuert gestartet und im Benutzermodus ausgeführt, sodass keine komplexe Konfiguration anfällt. Die LocalDB-Datenbank erstellt standardmäßig \*MDF-Dateien im Verzeichnis *C:/Benutzer/\<Benutzer\>*.

<a name="ssox"></a>
* Öffnen Sie im Menü **Ansicht** den **SQL Server-Objekt-Explorer** (SSOX).

  ![Menü Ansicht](sql/_static/ssox.png)

* Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie`, und wählen Sie **Designer anzeigen** aus:

  ![Für die Tabelle „Movie“ geöffnetes Kontextmenü](sql/_static/design.png)

  ![Im Designer geöffnete Tabelle „Movie“](sql/_static/dv.png)

Beachten Sie das Schlüsselsymbol neben `ID`. EF erstellt standardmäßig eine Eigenschaft namens `ID` für den Primärschlüssel.

* Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie`, und wählen Sie **Daten anzeigen** aus:

  ![Geöffnete Tabelle „Movie“ mit Tabellendaten](sql/_static/vd22.png)

## <a name="seed-the-database"></a>Ausführen eines Seedings für die Datenbank

Erstellen Sie im Ordner *Models* die neue Klasse `SeedData`. Ersetzen Sie den generierten Code durch den folgenden:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

Wenn in der Datenbank Filme vorhanden sind, wird der Initialisierer des Seedings zurückgegeben, und es werden keine Filme hinzugefügt.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Hinzufügen des Initialisierers des Seedings

Ändern Sie in der *Program.cs*-Datei die `Main`-Methode, um die folgenden Vorgänge auszuführen:

* Rufen Sie eine Datenbankkontextinstanz aus dem Dependency Injection-Container ab.
* Rufen Sie die Seedmethode auf, indem Sie den Kontext an diese übergeben.
* Löschen Sie den Kontext, wenn die Seedmethode abgeschlossen ist.

Der folgende Code zeigt die aktualisierte *Program.cs*-Datei.

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

Eine Produktions-App würde `Database.Migrate` nicht aufrufen. Es wird zum vorangehenden Code hinzugefügt, um die folgende Ausnahme zu verhindern, wenn `Update-Database` nicht ausgeführt wurde:

SqlException: Die bei der Anmeldung angeforderte Datenbank „RazorPagesMovieContext-21“ kann nicht geöffnet werden. Die Anmeldung ist fehlgeschlagen.
Die Anmeldung des Benutzers „Benutzername“ ist fehlgeschlagen.

### <a name="test-the-app"></a>Testen der App

* Löschen Sie alle Datensätze in der Datenbank. Dies ist über die Links „Löschen“ im Browser oder [SSOX](xref:tutorials/razor-pages/new-field#ssox) möglich.
* Zwingen Sie die App zur Initialisierung (rufen Sie die Methoden in der `Startup`-Klasse auf), damit die Seedmethode ausgeführt wird. Um die Initialisierung zu erzwingen, muss IIS Express beendet und neu gestartet werden. Hierzu können Sie einen der folgenden Ansätze verwenden:

  * Klicken Sie auf der Taskleiste im Infobereich mit der rechten Maustaste auf das Symbol von IIS Express, und wählen Sie **Beenden** oder **Website beenden** aus:

    ![IIS Express-Symbol auf der Taskleiste](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Kontextmenü](sql/_static/stopIIS.png)

    * Wenn Sie Visual Studio im Nicht-Debugmodus ausgeführt haben, drücken Sie F5, um den Debugmodus auszuführen.
    * Wenn Sie Visual Studio im Debugmodus ausgeführt haben, beenden Sie den Debugger, und drücken Sie F5.
   
Die App zeigt die per Seeding hinzugefügten Daten:

![In Chrome geöffnete Movie-Anwendung mit Filmdaten](sql/_static/m55.png)

Im nächsten Tutorial wird die Präsentation der Daten bereinigt.

> [!div class="step-by-step"]
> [Zurück: Gerüstbau mit Razor-Seiten](xref:tutorials/razor-pages/page)
> [Weiter: Aktualisieren der Seiten](xref:tutorials/razor-pages/da1)
