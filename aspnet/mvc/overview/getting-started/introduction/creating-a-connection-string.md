---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 746101344832793b199d2b3f3dfcfcd4e3b9a8da
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578522"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB
====================
durch [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB

Die `MovieDBContext` erstellten Klasse übernimmt die Aufgabe der Verbindung zur Datenbank und Zuordnung `Movie` Objekte mit Datenbank-Datensätzen. Eine Frage, die Sie wahrscheinlich gebeten, die ist jedoch, nach welcher Datenbank geben sie eine Verbindung herstellen. Sie müssen nicht unbedingt die zu verwendende Datenbank angeben, die Entity Framework standardmäßig [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). In diesem Abschnitt werden wir explizit hinzufügen eine Verbindungszeichenfolge in der *"Web.config"* -Datei der Anwendung.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) ist eine einfache Version von SQL Server Express-Datenbankmoduls, die wird bedarfsgesteuert gestartet und im Benutzermodus ausgeführt wird. LocalDB ausgeführt wird, in einem speziellen Ausführungsmodus von SQL Server Express, die Ihnen ermöglicht, die Arbeit mit Datenbanken als *mdf* Dateien. LocalDB-Datenbankdateien bleiben in der Regel der *App\_Daten* Ordner eines Webprojekts.

SQL Server Express wird für die Verwendung in Produktionsanwendungen für Web nicht empfohlen. LocalDB sollte insbesondere nicht für die Produktion mit einer Webanwendung verwendet werden, da es nicht ausgelegt ist, arbeiten Sie mit IIS. Eine LocalDB-Datenbank kann jedoch problemlos in SQL Server oder SQL Azure migriert werden.

In Visual Studio 2017 wird die LocalDB wird standardmäßig mit Visual Studio installiert.

Standardmäßig sucht die Entity Framework eine Verbindungszeichenfolge, die den gleichen Namen wie die Objektkontextklasse (`MovieDBContext` für dieses Projekt). Weitere Informationen finden Sie unter [SQL Server-Verbindungszeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx).

Öffnen Sie den Stammordner der Anwendung *"Web.config"* unten angezeigten Datei. (Nicht die *"Web.config"* Datei die *Ansichten* Ordner.)

![](creating-a-connection-string/_static/image1.png)

Suchen der `<connectionStrings>` Element:

![](creating-a-connection-string/_static/image2.png)

Fügen Sie die folgende Verbindungszeichenfolge für die `<connectionStrings>` Element in der *"Web.config"* Datei.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

Das folgende Beispiel zeigt einen Teil der *"Web.config"* -Datei mit der neuen Verbindungszeichenfolge hinzugefügt:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Die beiden Verbindungszeichenfolgen sind sehr ähnlich. Die erste Verbindungszeichenfolge wird mit dem Namen `DefaultConnection` und wird verwendet, für die Mitgliedschaftsdatenbank steuern, wer die Anwendung zugreifen kann. Gibt die Verbindungszeichenfolge, die Sie hinzugefügt haben, an einer LocalDB-Datenbank, die mit dem Namen *Movie.mdf* befindet sich in der *App\_Daten* Ordner. Es wird nicht die Mitgliedschaftsdatenbank in diesem Tutorial Weitere Informationen zur Mitgliedschaft, Authentifizierung und Sicherheit, finden Sie unter meinem Tutorial [eine ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank erstellen und Bereitstellen in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Der Name der Verbindungszeichenfolge muss den Namen entsprechen den ["DbContext"](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) Klasse.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Sie müssen nicht tatsächlich fügen die `MovieDBContext` Verbindungszeichenfolge. Wenn Sie eine Verbindungszeichenfolge angeben, erstellt Entity Framework eine LocalDB-Datenbank im Benutzerverzeichnis mit dem vollqualifizierten Namen der die ["DbContext"](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) Klasse (in diesem Fall `MvcMovie.Models.MovieDBContext`). Sie können name ist die Datenbank beliebig, solange sie verfügt über die *. MDF-Datei* Suffix. Beispielsweise könnten wir nennen Sie die Datenbank *MyFilms.mdf*.

Als Nächstes erstellen Sie ein neues `MoviesController` -Klasse, die Sie verwenden können, die Filmdaten angezeigt und können Benutzer neue Film Auflistungen zu erstellen.

> [!div class="step-by-step"]
> [Zurück](adding-a-model.md)
> [Weiter](accessing-your-models-data-from-a-controller.md)
