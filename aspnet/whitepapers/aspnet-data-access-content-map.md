---
uid: whitepapers/aspnet-data-access-content-map
title: 'ASP.NET: Datenzugriff – empfohlene Ressourcen | Microsoft-Dokumentation'
author: rick-anderson
description: Dieses Thema enthält Links zu Dokumentationsressourcen zur Verwendung für den Datenzugriff in ASP.NET-Webanwendungen wird in erster Linie mithilfe des Entity Framework und SQL-Se...
ms.author: aspnetcontent
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: fb0cea94d82cc8f59ec56a5445ee84d38325995e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832499"
---
<a name="aspnet-data-access---recommended-resources"></a>ASP.NET: Datenzugriff – empfohlene Ressourcen
====================
> Dieses Thema enthält Links zu Dokumentationsressourcen zur Verwendung für den Datenzugriff in ASP.NET-Webanwendungen wird in erster Linie mithilfe des Entity Framework und SQL Server.
> 
> Wenn Sie wissen, dass ein tolles Blog Posten, [Stackoverflow](http://stackoverflow.com) Thread oder einen anderen Link, die nützlich wären [senden Sie uns eine e-Mail](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) mit dem Link.
> 
> Aktualisierte 4/3/2014 letzten


Dieses Thema enthält folgende Abschnitte:

- [Erste Schritte mit den Datenzugriff in ASP.NET](#gettingstarted)
- [Mithilfe von Entitätsframework](#ef)

    - [Mithilfe von Entity Frameworkcode First](#cf)
    - [Mithilfe von Entity Framework Code First-Migrationen](#efcfmigrations)
    - [Mit Entity Framework Database First oder Model First (der EF-Designer)](#efdbf)
    - [Laden zugehöriger Daten im Entity Framework (Lazy Loading, Eager Loading und explizite laden)](#efrelateddata)
    - [Optimieren der Leistung von Entity Framework](#optimizingef)
    - [Behandeln von Parallelität in einer Entity Framework-Anwendung](#efconcurrency)
    - [Bücher über das Entitätsframework](#efbooks)
    - [Weitere Entity Framework-Ressourcen](#otherefresources)
- [Datenbindung in ASP.NET Web Forms-Anwendungen](#wfdatabinding)

    - [Verwenden von Web Forms-Modellbindung](#wfmodelbinding)
    - [Verwenden von Web Forms-Datenquellen-Steuerelemente](#wfdsc)
    - [Verwenden von Web Forms-von datengebundenen Steuerelementen und Datenbindungsausdrücke](#wfdbc)
- [Arbeiten mit SQL Server-Datenbanken](#sqlserver)

    - [Arbeiten mit SQL Server Express LocalDB-Datenbanken](#sslocaldb)
    - [Arbeiten mit SQL Server Express-Datenbanken](#sse)
    - [Arbeiten mit Windows Azure SQL-Datenbank](#ssdb)
    - [Entscheidung zwischen SQLServer und Windows Azure SQL-Datenbank](#ssdbchoosing)
- [Arbeiten mit NoSQL-Datenbank-Managementsysteme](#nosql)
- [Verwenden von LINQ-Abfragen in ASP.NET-Anwendungen](#linq)
- [Mithilfe von Dynamic Data-Gerüstbau](#dd)
- [Schützen des Datenzugriffs](#securing)
- [Optimieren von Datenzugriffsleistung](#optimizingdataaccess)
- [Bereitstellen einer Datenbank](#deploying)
- [Zugreifen auf Daten über einen Webdienst](#webservice)
- [Additional Resources](#additional) (Zusätzliche MSBuild-Ressourcen)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Erste Schritte mit den Datenzugriff in ASP.NET

- [Datenspeicheroptionen (erstellen realer Cloud-Apps mit Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Kapitel ein e-Book zur Entwicklung für die Cloud. Führt die NoSQL-Datenbanken als eine Alternative, die viele Entwickler, die mit relationalen Datenbanken vertraut sind häufig übersehen. Enthält Richtlinien dazu, was Sie denken bei der Auswahl relationale oder NoSQL oder eine bestimmte Plattform auswählen.
- [ASP.NET Datenzugriffsoptionen](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Eine Einführung in die Datenzugriffsoptionen für relationale Datenbanken für ASP.NET und Anleitungen zum Auswählen der Plattformen und Zugriff auf Methoden, die für Ihr Szenario geeignet sind.
- [Relationale Datenbank](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Wenn Sie mit relationalen Datenbanken gearbeitet haben, finden Sie unter dieser Seite finden Sie eine Einführung in die Terminologie von relationalen Datenbanken und Konzepte. Eine Einführung in SQL Server finden Sie insbesondere [arbeiten mit SQL Server-Datenbanken](#sqlserver) weiter unten in diesem Thema.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Mithilfe von Entitätsframework

- [Entity Framework-Webentwicklungsansätzen](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Leitfaden zum Auswählen einer Entity Framework Entwicklungsansatz Database First, Model First oder Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Mithilfe von Entity Frameworkcode First
  

Die folgenden Lernprogramme bieten herunterladbaren Beispielanwendungen:

- [Erste Schritte mit EF 6 anhand von MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Hintergrund bietet eine Vielzahl von Entity Framework Code First Szenarios, einschließlich Migrationen und EF 6 wie verbindungsresilienz, Abfangen von Befehlen und Async. Dies ist eine aktualisierte Version der [EF 5 / MVC 4-Serie](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Die früheren Reihe enthält ein Tutorial, in das Repository und Arbeitseinheit Muster, die nicht in die neue Reihe enthalten ist.
- [Einführung in ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Umfasst einen engeren Bereich der Entity Framework Code erste Testszenarios, jedoch ist eine umfassendere Einführung in MVC-Features.
- [Modellbindung und Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Verwendet Code First in einer Web Forms-Anwendung.
- [Erste Schritte mit ASP.NET 4.5 Web Forms](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Eine Einführung in Web Forms mit einigen Abdeckung von Code First. Verwendet der Modellbindung.
- [MVC Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Verwendet Code First in einer e-Commerce-MVC 3-Anwendung, die auch die Mitgliedschaft und Autorisierung implementiert. Die MVC-Version und (Authentifizierung und Autorisierung) ASP.NET-Mitgliedschaftssystem hier verwendete sind veraltet; Weitere aktuelle Informationen zu ASP.NET-Mitgliedschaft, finden Sie unter [ https://asp.net/identity ](https://asp.net/identity).

Weitere Ressourcen:

- [Entitätsframework – Code First für eine vorhandene Datenbank](https://msdn.microsoft.com/data/jj200620). MSDN. Videos und exemplarische Vorgehensweise, die zeigt, wie Sie mithilfe von Code First mit einer vorhandenen Datenbank.
- [Data Developer Center – Entitätsframework](https://msdn.microsoft.com/data/ef). MSDN. Eine Anleitung zur Dokumentation zu Entity Framework, die erstellt wurde und vom Entity Framework-Team verwaltet werden soll, finden Sie unter den [Einstieg](https://msdn.microsoft.com/data/ee712907) Link.

Siehe auch [Bücher über das Entity Framework](#efbooks) und [Weitere Entity Framework-Ressourcen](#otherefresources) weiter unten in diesem Thema.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Mithilfe von Entity Framework Code First-Migrationen
  

Die meisten Cover Migrationen angeführten Code First-Tutorials. Siehe auch die folgenden Ressourcen.

- [ASP.NET-webbereitstellung mithilfe von Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 2-Part tutorialserie, die zeigt, wie Sie mit Code First-Migrationen zum Bereitstellen einer Datenbank.
- [Bereitstellen eine sicheren ASP.NET MVC 5-app mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Windows Azure-Web-Website](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). So verwenden Sie Migrationen Mitgliedschafts-und Anwendungsdaten in Azure bereitstellen.
- [Übersicht über die Bereitstellung für Visual Studio und ASP.NET Web](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Finden Sie unter den **Konfigurieren von Datenbank-Bereitstellung in Visual Studio** Abschnitt eine Erläuterung der wie Code First-Migrationen in Visual Studio Web-Bereitstellungsfunktionen integriert wird.
- [Data Developer Center – Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) (MSDN). Dokumentation für das Entity Framework-Team Migrationen.
- [Migrationen Screencast-Reihe](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). EF-Blog). Drei-Videos auf erweiterten Themen in Code First-Migrationen.
- [Code First-Migrationen mit ASP.NET Web Pages-Websites](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Mikesdotnetting-Blog). Zeigt, wie Code First-Migrationen mit einer ASP.NET Web Pages-Website zu verwenden, indem Sie den Datenkontext in ein Visual Studio-Klassenbibliotheksprojekt einfügen.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Mit Entity Framework Database First oder Model First (der EF-Designer)

- [Erste Schritte mit Entity Framework 6 Database First anhand von MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Ausführen eines Skripts in Server-Explorer zum Erstellen einer Datenbank, und klicken Sie dann den Entity Framework-Designer verwenden, um das Datenmodell zu erstellen. Zeigt, wie zum Erstellen von einfachen CRUD-Web-Seiten und für andere Funktionen, die Sie befolgen können, die Code First-Tutorials, da alle EF-Workflows verwenden Sie die gleichen DbContext-API für die Datenverarbeitung.

Die folgenden Ressourcen, die älter sind. Sie sind hilfreich, wenn Sie Version 4.0 von Entity Framework verwenden möchten, und ein Datenquellen-Steuerelement für die Datenbindung in Web Forms-Anwendungen verwenden möchten.

- [Erste Schritte mit Entitätsframework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Zeigt, wie die **EntityDataSource** Steuerelement.
- [Fortfahren mit dem Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(zeigt, wie die **"ObjectDataSource"** Steuerelement. Enthält ein Tutorial zur Behandlung der Parallelität, ein Tutorial für EF-Leistung und ein Lernprogramm zu in EF 4.0 Neuigkeiten.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Behandlung von verknüpften Daten in Entity Framework (Lazy Loading, Eager Loading und explizite laden)

- [Lesen von relevanten Daten mit dem Entitätsframework in einer ASP.NET MVC-Anwendung](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, MVC-beispielanwendung. Die aufgeführten Methoden gelten auch für Web Forms-modellbindung und Database First-Workflow verwendet werden.
- [Data Developer Center – Laden verwandter Entitäten](https://msdn.microsoft.com/data/jj574232) (MSDN). Das Entity Framework-Team Dokumentation zum Laden zugehöriger Daten.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optimieren der Leistung von Entity Framework

- [Erweiterte Entity Framework-Szenarien für eine ASP.NET-Anwendung](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Zeigt, wie Sie eigene SQL-Anweisungen ausführen, oder rufen Sie Ihre eigenen gespeicherten Prozeduren, zum Deaktivieren der Erkennung von Änderungen und Gewusst wie: Deaktivieren der Validierung beim Speichern von Änderungen.
- [Leistungsüberlegungen zu Entitätsframework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Überlegungen zur Leistung (Entitätsframework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Maximieren der Leistung bei Entitätsframework in einer ASP.NET-Webanwendung](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Gilt für Entitätsframework 4.0.
- Siehe auch [optimieren ASP.NET-Datenzugriff](#optimizingdataaccess) weiter unten in diesem Thema.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Behandeln von Parallelität in einer Entity Framework-Anwendung

- [Verarbeiten von Parallelität bei Entitätsframework in einer ASP.NET MVC-Anwendung](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, DbContext-API mit einer MVC-beispielanwendung.
- [Data Developer Center – vollständige Parallelitätsmuster](https://msdn.microsoft.cus/data/jj592904) (MSDN). Concurrency-Dokumentation des Entity Framework-Teams.
- [Verarbeiten von Parallelität bei Entitätsframework in einer ASP.NET-Webanwendung](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Gilt für Entitätsframework 4.0. Datenbank zunächst ObjectContext-API mit einer Web Forms-beispielanwendung.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Bücher über das Entitätsframework

- [Programming Entitätsframework: die "DbContext"](http://shop.oreilly.com/product/0636920022237.do) von Julie Lerman und Rowan Miller.
- [Programming Entitätsframework: Code First](http://shop.oreilly.com/product/0636920022220.do) von Julie Lerman und Rowan Miller.

Diese Bücher sind auf dem neuesten Stand mit aktuellen empfohlenen Verfahren. Sie bieten eine umfassende aber leicht verständliche Einführung in Entity Framework als alle Möglichkeiten zur Verfügung, über das Internet. Eine andere Buch [Programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do) von Julie Lerman, größerer und umfassender jedoch ist sie älter ist und viele der Techniken, die hierin sind nicht mehr die empfohlene Vorgehensweise zur Verwendung von Entity Framework. Siehe auch die Liste der Bücher, die von Entity Framework-Teams auf empfohlene [Data Developer Center – Bücher](https://msdn.microsoft.com/data/aa937716) auf der MSDN-Website.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Weitere Entity Framework-Ressourcen

- [Teamblog für Entity Framework (ADO.NET)-Anbieter](https://blogs.msdn.com/b/adonet/). Einer der besten Ressourcen für die aktuelle Informationen und Ankündigungen von neuen Erweiterungen zur Verfügung. Für andere EF-bezogenen Blogs, finden Sie unter den – Blogroll am [erste Schritte mit Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN-Magazin](https://msdn.microsoft.com/magazine/default.aspx). Finden Sie unter den **Datenpunkte** Spalte, die häufig über Themen im Zusammenhang mit Entity Framework ist.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Datenbindung in ASP.NET Web Forms-Anwendungen

- [ASP.NET Web Forms-Datenzugriffsoptionen](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Verwenden von Web Forms-Modellbindung

- [Modellbindung und Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Tutorial mit EF Code First-Reihe.
- [Web Forms Modellbindung, Teil 1: Auswählen von Daten](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (Scott Guthries Blog). In diese älteren Blogbeiträge die Eigenschaft, die derzeit ItemType heißt hieß ModelType, andernfalls die darin enthaltenen Informationen ist jedoch gültig.
- [Web Forms Modellbindung, Teil 2: Filtern von Daten](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (Scott Guthries Blog).
- [Web Forms Modellbindung, Teil 3: Aktualisieren und Validierung](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (Scott Guthries Blog).
- [ASP.NET 4.5 Web Forms – Modellbindung](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (Video).
- [Modellbindung, Teil 1: Auswählen von Daten](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (video).
- [Modellbindung, Teil 2: Filterung](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (video).
- [Erste Schritte mit ASP.NET 4.5 Web Forms - Daten anzeigen, Elemente und Details](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Verwenden von Web Forms-Datenquellen-Steuerelemente

- [Daten-Webserversteuerelemente](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Ankündigung der Version von Dynamic Data-Anbieter und EntityDataSource-Steuerelement für Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (Blog zur Microsoft-Web-Entwicklung).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Verwenden von Web Forms-von datengebundenen Steuerelementen und Datenbindungsausdrücke

- [Modellbindung und Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Reihe von Tutorials, die von EF Code First verwendet.
- [Erste Schritte mit ASP.NET 4.5 Web Forms - Daten anzeigen, Elemente und Details](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Stark typisierte Datensteuerelemente](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (Scott Guthries Blog).
- [Stark typisierte Datensteuerelemente](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [ASP.NET 4.5 Web Forms stark typisierte-Datensteuerelemente](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [Datengebundene Webserversteuerelemente](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Datenbindung – Übersicht über die Ausdrücke](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Diese Seite nur Hintergrund **Eval** und **binden**; es wurde nicht aktualisiert und umfassen **Element** und **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Arbeiten mit SQL Server-Datenbanken

- [Features von SQL Server-Datenbank](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Eine allgemeine Einführung in eine Vielzahl von SQL Server-Themen finden Sie in den Einträgen unter diesem im Inhaltsverzeichnis.
- [SQL Server-Editionen](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Eine Zusammenfassung der verfügbaren SQL Server-Editionen, Links zu weiteren Informationen über die einzelnen Aktionen.)
- [SQL Server-Verbindungszeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Verwenden von SQL Server Compact für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: Datenbank-Produktbeispiele](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). AdventureWorks-Beispieldatenbanken.
- [Installieren von Beispieldatenbanken](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Neben den hier gezeigten Methoden können Sie auch eines der Beispiel-MDF-Dateien auf Herunterladen der App\_Ordner "Data" eines Webprojekts, konvertieren Sie die Datenbank in LocalDB, und erstellen Sie eine LocalDB-Verbindungszeichenfolge. Informationen dazu, wie Sie dies tun, finden Sie unter [wie: Upgrade auf LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Siehe auch die folgenden Abschnitten zum Arbeiten mit SQL Server Express und LocalDB, und zwischen SQL Server und SQL-Datenbank auswählen.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Arbeiten mit SQL Server Express LocalDB-Datenbanken

- [SQLServer Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). Die offizielle MSDN Einführung in LocalDB.
- [SQL Server-Verbindungszeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Vorgehensweise: Upgrade auf LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Wie Sie eine MDF-Datei von einer früheren Version von SQL Server Express auf LocalDB zu migrieren. Außerdem müssen Sie diesen Prozess durchlaufen zu lassen, wenn Sie eine der Herunterladen der [Beispieldatenbanken für SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Einführung in LocalDB, ein verbessertes SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (SQL Server Express-Blog). Verfügt über weitere Hintergrundinformationen dazu, warum LocalDB erstellt wurde, als in MSDN enthalten sind.
- [LocalDB: Wo ist meine Datenbank?](https://go.microsoft.com/fwlink/?LinkId=234376) (SQL Server Express-Blog). Informationen zur, in dem LocalDB-Datenbankdateien erstellt werden.
- [Verwenden von LocalDB mit vollständigem IIS, Teil 1: Benutzerprofil](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (SQL Server Express-Blog). LocalDB ist nicht mit IIS arbeiten soll. Diese Serie von Blogeinträgen veröffentlicht wird erläutert, die Probleme und einigen problemumgehungen.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Arbeiten mit SQL Server Express-Datenbanken

- [SQL Server-Verbindungszeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Wenn Sie die Einstellung "attatchdbfilename" Verbindung mit SQL Server Express verwenden, finden Sie insbesondere die Benutzerinstanz-Abschnitt der Seite.
- [Wie Sie Ihre lokalen SQL Server Express 2008 Besitz](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (SQL Server Express-Blog). Ein häufiges Problem ist nicht das SQL Server Express-Datenbanken verwenden, da Sie nicht auf die SQL Server Express-Instanz als Administrator angemeldet sind. Standardmäßig ist nur die Person, die SQL Server Express installiert, ein Administrator an. Dieser Blog wird erläutert, wie sich einen Administrator SQL Server Express machen möchten, wenn Sie ein Administrator auf dem Computer sind.
- [Kann meine Webanwendung ASP.NET eine SQL Server Express-Datenbank in einer produktionsumgebung verwenden?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Arbeiten mit Windows Azure SQL-Datenbank

- [Bereitstellen eine sicheren ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank auf einem Windows Azure-Website](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (Microsoft Azure-Website).
- [SQL-Datenbanken](https://docs.microsoft.com/azure/sql-database/) (Microsoft Azure-Website). Erste Schritte-Lernprogramme und Anleitungen.
- [Windows Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Der Knoten der obersten Ebene des Inhaltsverzeichnisses für SQL-Datenbank in MSDN.
- [Windows Azure SQL-Datenbank TechNet-Wiki-Artikel Index](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (Microsoft TechNet-Website).
- [Anwendungsblock für die Behandlung vorübergehender Fehler](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Ein Framework zur Behandlung von vorübergehenden Netzwerkfehlern und Verbindungsfehler in das Ergebnis von der Einschränkung. In einem NuGet-Paket verfügbar: [Enterprise Library 5.0 - Transient Fault Handling Application Block](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Erste Schritte mit SQL-Datenbank und Entitätsframework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Windows Azure-Trainingskit](https://www.microsoft.com/download/details.aspx?id=8396) (Microsoft Download Center). Enthält praktische Übungseinheiten für SQL-Datenbank.
- [Community-Forum für Windows Azure SQL-Datenbank](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [In Windows Azure SQL-Datenbank verschieben](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Ein Kapitel eines umfassende End-to-End-Szenarios vom Microsoft Patterns and Practices-Team. Warum Sie ggf. zu migrierenden behandelt und wie von SQL Server mit SQL-Datenbank migrieren.
- [Migrieren von SQL Server-Datenbanken zu Windows Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [SQL-Datenbankmigrations-Assistent](http://sqlazuremw.codeplex.com/). Open Source-Tool für die Migration von Datenbanken in und aus SQL-Datenbank.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Entscheidung zwischen SQLServer und Windows Azure SQL-Datenbank

- [Vergleichen Sie SQL Server mit Windows Azure SQL-Datenbank](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (Microsoft TechNet-Website).
- [Migration von Daten zu Windows Azure SQL-Datenbank: Tools und Techniken](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Enthält Abschnitte, die SQL Server mit SQL-Datenbank verglichen werden soll, und bieten eine Anleitung dazu, wann Sie von SQL Server mit SQL-Datenbank migrieren.
- [Bereitstellungshandbuch für Windows Azure SQL-Datenbank](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (Microsoft TechNet-Website).
- [Einschränkungen für die SQL Server-Funktionen (Windows Azure SQL-Datenbank)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows Azure-Tabellenspeicher und Windows Azure SQL-Datenbank – Vergleich und Gegenüberstellung](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Für eine Anwendung, die Sie in Windows Azure bereitstellen, kann die Windows Azure-Tabellenspeicher eine Alternative zum Windows Azure SQL-Datenbank sein. Dieses Thema hilft Ihnen bei der Entscheidung zwischen diesen alternativen.
- [Windows Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Richtlinien und Einschränkungen (Windows Azure SQL-Datenbank)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Arbeiten mit NoSQL-Datenbank-Managementsysteme

- [Windows Azure-Datendienste](https://www.windowsazure.com/develop/net/data/) (Microsoft Azure-Website). Finden Sie unter den [Tabellendienst-featureleitfaden](https://docs.microsoft.com/azure/) und **Big Data** Abschnitt der Seite.
- [ASP.NET Multi-Tier-Anwendung mithilfe von Storage-Tabellen, Warteschlangen und Blobs](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure-Website). End-to-End-Tutorial mit herunterladbare Beispielversion-Anwendung, die Windows Azure Storage-NoSQL-Tabellen verwendet.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Verwenden von LINQ-Abfragen in ASP.NET-Anwendungen

- [ASP.NET Datenzugriffsoptionen](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Enthält eine Einführung in LINQ.
- [LINQ-Schulungsvideos](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (Joe Stagner Blog).
- [ASP.NET-Forum-Thread mit Links zu Ressourcen für dynamische LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Mithilfe von Dynamic Data-Gerüstbau

- [Dynamic Data-Projektvorlagen](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Richtlinien zur Dynamic Data-Projekte verwenden.
- [ASP.NET Dynamic Data](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Schützen des Datenzugriffs

- [Zugriffsschutz für Daten in ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Sicherheitsüberlegungen (Entitätsframework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Gewusst wie: Sichern von Verbindungszeichenfolgen bei Verwendung von Datenquellensteuerelementen](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optimieren von Datenzugriffsleistung

- [Übersicht über die Leistung von ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [Zwischenspeicherung in ASP.NET](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Verbessern der Leistung von ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Liegt eine Warnung vor "Veralteter Inhalt" am oberen Rand dieser Seite, aber die meisten Informationen immer noch relevant ist, und es gibt keine vergleichbare aktualisierte Ressource.
- [Verbessern der Leistung von SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Kommentar als der vorherige Link.

Siehe auch [Entity Framework Optimieren der Leistung](#optimizingef) weiter oben in diesem Thema.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Bereitstellen einer Datenbank

- [ASP.NET: Webbereitstellung – empfohlene Ressourcen](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Zugreifen auf Daten über einen Webdienst

- [Zugreifen auf Daten über einen Webdienst](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Richtlinien zur Web-API im Vergleich zu WCF verwenden.
- [Erste Schritte mit ASP.NET Web-API](../web-api/index.md).
- [WCF-Datendienste](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [ASP.NET: Datenzugriff – häufig gestellte Fragen](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [ASP.NET Web Forms-Tutorials: Daten](../web-forms/overview/data-access/index.md). Die meisten dieser Tutorials sind relativ alter; Lesen Sie unbedingt [ASP.NET Datenzugriffsoptionen](https://msdn.microsoft.com/library/ms178359.aspx) und [Datenspeicheroptionen (Building Real-World Cloud Apps mit Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) zuerst, damit Sie nicht zu weit in einem Datenzugriffsmethode erhalten, die nicht von rechts für Ihr Szenario.
- [ASP.NET MVC-Inhaltszuordnung](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [ASP.NET Web Pages-Tutorials: Daten](../web-pages/overview/data/index.md).
- [Datenzugriff in Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Enthält eine Liste der Links, die ähnlich wie diese inhaltszuordnung, aber mit dem Schwerpunkt auf Visual Studio anstelle von ASP.NET.
