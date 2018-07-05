---
title: Arbeiten mit SQL Server LocalDB in einer ASP.NET Core MVC-App
author: rick-anderson
description: Erfahren Sie mehr über die Verwendung von SQL Server LocalDB in einer einfachen ASP.NET Core MVC-App.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 05bd76038e5856d2a3e392e3b00e589dbc26fcc6
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961057"
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a>Arbeiten mit SQL Server LocalDB in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Das `MvcMovieContext`-Objekt übernimmt die Aufgabe der Herstellung der Verbindung mit der Datenbank und Zuordnung von `Movie`-Objekten zu Datensätzen in der Datenbank. Der Datenbankkontext wird mit dem Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) in der Methode `ConfigureServices` in der Datei *Startup.cs* registriert:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]
::: moniker-end

Das ASP.NET Core-[Konfigurationssystem](xref:fundamentals/configuration/index) liest die `ConnectionString`. Für die lokale Entwicklung wird die Verbindungszeichenfolge aus der Datei *appsettings.json* abgerufen:

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

Wenn Sie die App auf einem Test- oder Produktionsserver bereitstellen, können Sie eine Umgebungsvariable oder eine andere Vorgehensweise zum Festlegen der Verbindungszeichenfolge für einen tatsächlichen SQL Server-Computer verwenden. Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB ist eine Lightweightversion der SQL Server Express-Datenbank-Engine, die für die Programmentwicklung geeignet ist. LocalDB wird bedarfsgesteuert gestartet und im Benutzermodus ausgeführt, sodass keine komplexe Konfiguration anfällt. Die LocalDB-Datenbank erstellt standardmäßig \*MDF-Dateien im Verzeichnis *C:/Benutzer/\<Benutzer\>*.

* Öffnen Sie im Menü **Ansicht** den **SQL Server-Objekt-Explorer** (SSOX).

  ![Menü Ansicht](working-with-sql/_static/ssox.png)

* Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie` **> Ansicht-Designer**.

  ![Für die Tabelle „Movie“ geöffnetes Kontextmenü](working-with-sql/_static/design.png)

  ![Im Designer geöffnete Tabelle „Movie“](working-with-sql/_static/dv.png)

Beachten Sie das Schlüsselsymbol neben `ID`. EF macht die Eigenschaft `ID` standardmäßig zum Primärschlüssel.

* Klicken Sie mit der rechten Maustaste auf die Tabelle `Movie` **> Daten anzeigen**.

  ![Für die Tabelle „Movie“ geöffnetes Kontextmenü](working-with-sql/_static/ssox2.png)

  ![Geöffnete Tabelle „Movie“ mit Tabellendaten](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a>Ausführen eines Seedings für die Datenbank

Erstellen Sie im Ordner *Models* die neue Klasse `SeedData`. Ersetzen Sie den generierten Code durch den folgenden:

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Wenn in der Datenbank Filme vorhanden sind, wird der Initialisierer des Seedings zurückgegeben, und es werden keine Filme hinzugefügt.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Hinzufügen des Initialisierers des Seedings

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Fügen Sie den Initialisierer des Seedings in der Datei *Program.cs* zur `Main`-Methode hinzu:

[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Fügen Sie den Initialisierer des Seedings in der Datei *Startup.cs* am Ende der `Configure`-Methode hinzu.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---
::: moniker-end

Testen der App

* Löschen Sie alle Datensätze in der Datenbank. Dies ist über die Links „Löschen“ im Browser oder SSOX möglich.
* Zwingen Sie die App zur Initialisierung (rufen Sie die Methoden in der `Startup`-Klasse auf), damit die Seed-Methode ausgeführt wird. Um die Initialisierung zu erzwingen, muss IIS Express beendet und neu gestartet werden. Hierzu können Sie einen der folgenden Ansätze verwenden:

  * Klicken Sie auf der Taskleiste im Infobereich mit der rechten Maustaste auf das Symbol von IIS Express, und wählen Sie **Beenden** oder **Website beenden** aus.

    ![IIS Express-Symbol auf der Taskleiste](working-with-sql/_static/iisExIcon.png)

    ![Kontextmenü](working-with-sql/_static/stopIIS.png)

    * Wenn Sie Visual Studio im Nicht-Debugmodus ausgeführt haben, drücken Sie F5, um den Debugmodus auszuführen.
    * Wenn Sie Visual Studio im Debugmodus ausgeführt haben, beenden Sie den Debugger, und drücken Sie F5.

Die App zeigt die per Seeding hinzugefügten Daten.

![In Microsoft Edge geöffnete MVC Movie-Anwendung mit Filmdaten](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> [Zurück](adding-model.md)
> [Weiter](controller-methods-views.md)  
