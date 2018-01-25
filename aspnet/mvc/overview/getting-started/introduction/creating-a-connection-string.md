---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 25d1c1c9954baaca9ef91eff3dd3c853930a5893
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Erstellen einer Verbindungszeichenfolge und Arbeiten mit SQL Server LocalDB

Die `MovieDBContext` erstellten Klasse behandelt die Aufgabe der Herstellen einer Verbindung mit der Datenbank und die Zuordnung `Movie` -Objekten, die Datenbankdatensätze. Eine Frage, die möglicherweise aufgefordert, ist jedoch, wie Sie zur Angabe der Datenbank hergestellt wird. Nicht tatsächlich müssen Sie die zu verwendende Datenbank angeben, Entity Framework werden standardmäßig [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). In diesem Abschnitt wird explizit fügen eine Verbindungszeichenfolge in der *"Web.config"* -Datei der Anwendung.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) ist eine vereinfachte Version von SQL Server Express-Datenbankmoduls, die bedarfsgesteuert gestartet und im Benutzermodus ausgeführt wird. LocalDB ausgeführt wird, in eine spezielle Ausführungsmodus von SQL Server Express, die Ihnen ermöglicht, die Arbeit mit Datenbanken als *mdf* Dateien. LocalDB-Datenbankdateien bleiben in der Regel der *App\_Daten* Ordner eines Webprojekts.

SQL Server Express wird nicht für die Verwendung in Web Produktionsanwendungen empfohlen. LocalDB sollte insbesondere nicht für die Produktion mit einer Webanwendung verwendet werden, da er nicht zum Arbeiten mit IIS ausgelegt ist. Eine LocalDB-Datenbank kann jedoch problemlos auf SQL Server- oder SQL Azure migriert werden.

In Visual Studio 2017 ist LocalDB, die standardmäßig mit Visual Studio installiert.

Standardmäßig sucht die Entity Framework für eine Verbindungszeichenfolge, die den gleichen Namen wie die Kontext-Objektklasse (`MovieDBContext` für dieses Projekt). Weitere Informationen finden Sie unter [SQL Server-Verbindungszeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx).

Öffnen Sie das Stammverzeichnis der Anwendung *"Web.config"* unten dargestellten Datei. (Nicht die *"Web.config"* in der Datei die *Ansichten* Ordner.)

![](creating-a-connection-string/_static/image1.png)

Suchen der `<connectionStrings>` Element:

![](creating-a-connection-string/_static/image2.png)

Fügen Sie die folgende Verbindungszeichenfolge, um die `<connectionStrings>` Element in der *"Web.config"* Datei.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

Das folgende Beispiel zeigt einen Teil der *"Web.config"* Datei mit der neuen Verbindungszeichenfolge hinzugefügt:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Die zwei Verbindungszeichenfolgen sind sehr ähnlich. Die erste Verbindungszeichenfolge lautet `DefaultConnection` dient für die Mitgliedschaftsdatenbank steuern, wer die Anwendung zugreifen kann. Die Verbindungszeichenfolge, die Sie hinzugefügt haben, gibt eine LocalDB-Datenbank mit dem Namen *Movie.mdf* befindet sich in der *App\_Daten* Ordner. Es wird nicht die Mitgliedschaftsdatenbank in diesem Lernprogramm für Weitere Informationen zur Mitgliedschaft, Authentifizierung und Sicherheit, finden Sie unter meinem Lernprogramm [eine ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank erstellen und Bereitstellen von Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Der Name der Verbindungszeichenfolge muss den Namen des entsprechen den [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) Klasse.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Sie müssen nicht tatsächlich Hinzufügen der `MovieDBContext` Verbindungszeichenfolge. Wenn Sie eine Verbindungszeichenfolge angeben, erstellen Entity Framework eine LocalDB-Datenbank im Benutzerverzeichnis mit dem vollqualifizierten Namen der die [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) Klasse (in diesem Fall `MvcMovie.Models.MovieDBContext`). Sie können die Datenbank einen beliebigen Namen gewünscht, solange er hat die *. MDF* Suffix. Wir nennen z. B. die Datenbank *MyFilms.mdf*.

Als Nächstes müssen Sie ein neues erstellen `MoviesController` -Klasse, die Sie verwenden können, an die Filmdaten angezeigt und ermöglichen Benutzern die neuen Film-Angebote erstellen.

>[!div class="step-by-step"]
[Zurück](adding-a-model.md)
[Weiter](accessing-your-models-data-from-a-controller.md)
