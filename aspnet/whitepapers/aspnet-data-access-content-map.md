---
uid: whitepapers/aspnet-data-access-content-map
title: ASP.NET-Datenzugriff - empfohlene Ressourcen | Microsoft Docs
author: rick-anderson
description: Dieses Thema enthält Links zu Dokumentationsressourcen, zum Zugreifen auf Daten in ASP.NET-Webanwendungen wird in erster Linie mithilfe des Entity Framework und SQL-Se...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/25/2013
ms.topic: article
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 16364951544dff33c9cdb8c8e330cc93de192c47
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28048258"
---
<a name="aspnet-data-access---recommended-resources"></a>ASP.NET-Datenzugriff - Ressourcen empfohlen
====================
> Dieses Thema enthält Links zu Dokumentationsressourcen, zum Zugreifen auf Daten in ASP.NET-Webanwendungen wird in erster Linie mithilfe des Entity Framework und SQL Server.
> 
> Wenn Sie wissen, dass eine hervorragende BlogPost: [Stackoverflow](http://stackoverflow.com) Thread oder einen beliebigen anderen Link, der nützlich ist, wäre [senden Sie uns eine e-Mail-Nachricht](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) mit dem Link.
> 
> Aktualisierte 4/3/2014 letzten


Dieses Thema enthält folgende Abschnitte:

- [Erste Schritte mit-Datenzugriff in ASP.NET](#gettingstarted)
- [Mithilfe von Entity Framework](#ef)

    - [Zuerst mithilfe von Entity Framework-Code](#cf)
    - [Verwenden von Entity Framework Code First-Migrationen](#efcfmigrations)
    - [Zuerst mithilfe von Entity Framework-Datenbank oder Model First (EF-Designer)](#efdbf)
    - [Laden von verknüpften Daten im Entity Framework (Lazy Loading, Eager Loading, und das explizite laden)](#efrelateddata)
    - [Optimieren der Leistung von Entity Framework](#optimizingef)
    - [Behandeln von Parallelität in einer Entity Framework-Anwendung](#efconcurrency)
    - [Bücher über das Entity Framework](#efbooks)
    - [Zusätzliche Ressourcen für Entity Framework](#otherefresources)
- [Datenbindung in der ASP.NET Web Forms-Anwendungen](#wfdatabinding)

    - [Verwenden von Web Forms-Modellbindung](#wfmodelbinding)
    - [Verwenden der Web Forms-Steuerelementen für Datenquellen](#wfdsc)
    - [Verwenden der Web Forms-von datengebundenen Steuerelementen und Datenbindungsausdrücke](#wfdbc)
- [Arbeiten mit SQL Server-Datenbanken](#sqlserver)

    - [Arbeiten mit SQL Server Express LocalDB-Datenbanken](#sslocaldb)
    - [Arbeiten mit SQL Server Express-Datenbanken](#sse)
    - [Arbeiten mit Windows Azure SQL-Datenbank](#ssdb)
    - [Auswählen zwischen SQLServer und Windows Azure SQL-Datenbank](#ssdbchoosing)
- [Arbeiten mit NoSQL-Datenbank-Managementsysteme](#nosql)
- [Verwenden von LINQ-Abfragen in ASP.NET-Anwendungen](#linq)
- [Mithilfe von Dynamic Data-Gerüstbau](#dd)
- [Sichern von Datenzugriff](#securing)
- [Optimieren der Leistung im Zugriff](#optimizingdataaccess)
- [Bereitstellen einer Datenbank](#deploying)
- [Zugreifen auf Daten über einen Webdienst](#webservice)
- [Additional Resources](#additional) (Zusätzliche MSBuild-Ressourcen)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Erste Schritte mit-Datenzugriff in ASP.NET

- [Die Datenspeicheroptionen (Building Real-World Cloud-Apps mit Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Kapitel eine e-Book zum Entwickeln von für die Cloud. Führt ein NoSQL-Datenbanken als Alternative, die tendenziell, dass viele Entwickler mit relationalen Datenbanken vertraut übergangen werden. Stellt Richtlinien dazu, was im Wesentlichen bei der Auswahl relationale oder NoSQL oder eine bestimmte Plattform auswählen.
- [ASP.NET-Daten Zugriffsoptionen](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Einführung in die Datenzugriffsoptionen für relationale Datenbanken für ASP.NET und Anleitungen zum Auswählen von Plattformen und Zugriff auf Methoden, die für Ihr Szenario geeignet sind.
- [Relationale Datenbank](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Wenn Sie mit relationalen Datenbanken gearbeitet haben, finden Sie auf dieser Seite eine Einführung in die relationale datenbankterminologie und Konzepte. Eine Einführung in SQL Server finden Sie insbesondere [arbeiten mit SQL Server-Datenbanken](#sqlserver) weiter unten in diesem Thema.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Mithilfe von Entity Framework

- [Ansätze zur Entity Framework-Entwicklung](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Leitfaden zum Auswählen einer Entity Framework Entwicklungsansatz Database First Model First oder Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Zuerst mithilfe von Entity Framework-Code
  

Die folgenden Lernprogramme bieten herunterladbaren Beispiel-Anwendungen:

- [Erste Schritte mit EF 6 mit MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Deckt bietet eine Breite Palette von Entity Framework Code First-Szenarien, einschließlich Migrationen und EF 6 wie z. B. verbindungsstabilität Befehl abfangen und asynchrone. Dies ist eine aktualisierte Version der [EF 5 / MVC 4-Serie](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Die früheren Reihen umfasst ein Lernprogramm, auf das Repository und Unit of Work-Muster, die nicht in die neue Reihe enthalten ist.
- [Einführung in ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Einen engeren Bereich von Entity Framework-Code erste Szenarien behandelt, jedoch ist einen umfassenderen Auftrag Einführung in MVC-Funktionen.
- [Wurden die Modellbindung und WebForms](https://go.microsoft.com/fwlink/?LinkId=286117). Code First wird in einer Web Forms-Anwendung verwendet.
- [Erste Schritte mit ASP.NET 4.5 WebForms](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Eine Einführung in Web Forms mit einigen Abdeckung von Code First. Mithilfe der Modellbindung.
- [MVC-Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Verwendet die Code First in einer e-Commerce-MVC 3-Anwendung, die auch die Mitgliedschaft und Autorisierung implementiert. Das MVC-Version und (Authentifizierung und Autorisierung) ASP.NET-Mitgliedschaftssystem hier verwendete sind veraltet; Weitere aktuelle Informationen zu ASP.NET-Mitgliedschaft, finden Sie unter [https://asp.net/identity](https://asp.net/identity).

Weitere Ressourcen:

- [Entity Framework - Code zunächst zu einer vorhandenen Datenbank](https://msdn.microsoft.com/data/jj200620). MSDN. Video und exemplarische Vorgehensweise, die zeigt, wie Sie mithilfe von Code First mit einer vorhandenen Datenbank.
- [Data Developer Center - Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Anleitung zur Dokumentation zu Entity Framework, die vom Entity Framework-Team verwaltete und erstellt wurden, finden Sie unter der [Einstieg in die](https://msdn.microsoft.com/data/ee712907) Link.

Siehe auch [Bücher über das Entity Framework](#efbooks) und [zusätzliche Entity Framework-Ressourcen](#otherefresources) weiter unten in diesem Thema.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Verwenden von Entity Framework Code First-Migrationen
  

Die meisten der Code First-Migrationen Abdeckung oben Lernprogramme. Siehe auch die folgenden Ressourcen.

- [ASP.NET Web-Bereitstellung mit Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 2-Part Reihe von Lernprogrammen, die zeigt, wie Sie Code First-Migrationen verwenden, um eine Datenbank bereitzustellen.
- [Eine sichere ASP.NET MVC 5-app mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Windows Azure-Website bereitstellen](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Wie Sie die Migrationen zu verwenden, um die Mitgliedschaft und Anwendungsdaten in Azure bereitstellen.
- [Übersicht über die Bereitstellung für Visual Studio und ASP.NET Web](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Finden Sie unter der **Konfigurieren von Datenbank-Bereitstellung in Visual Studio** Abschnitt eine Erläuterung, wie Code First-Migrationen in Visual Studio Web Bereitstellungsfeatures integriert wird.
- [Data Developer Center - Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) (MSDN). Das Entity Framework-Team Migrationen-Dokumentation.
- [Migrationen Screencast Reihe](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). EF blog). Drei-Videos auf Erweiterte Themen im Code First-Migrationen.
- [Code First-Migrationen mit ASP.NET Web Pages-Websites](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Mikesdotnetting-Blog). Zeigt, wie Code First-Migrationen mit ASP.NET Web Pages-Websites verlegen Sie den Datenkontext in ein Visual Studio-Klassenbibliotheksprojekt.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Zuerst mithilfe von Entity Framework-Datenbank oder Model First (EF-Designer)

- [Erste Schritte mit Entity Framework 6 Database First mit MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Führen Sie ein Skript im Server-Explorer zum Erstellen einer Datenbank, und klicken Sie dann verwenden Sie den Entity Framework-Designer, um das Datenmodell zu erstellen. Zeigt, wie einfache CRUD-Web Erstellen von Seiten und für andere Funktionen, die Sie befolgen können, den Code First Tutorials, da alle EF Workflows verwenden Sie die gleiche DbContext-API zum Behandeln von Daten.

Die folgenden Ressourcen, die älter sind. Sie sind hilfreich, wenn Sie Entity Framework-Version 4.0 verwenden möchten, und ein Datenquellen-Steuerelement für die Datenbindung in Web Forms-Anwendung verwenden möchten.

- [Erste Schritte mit dem Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Zeigt, wie die **EntityDataSource** Steuerelement.
- [Fortfahren mit dem Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(zeigt, wie die **ObjectDataSource** Steuerelement. Enthält ein Lernprogramm zum Parallelitätsbehandlung, ein Lernprogramm zum EF-Leistung und ein Lernprogramm zu in EF 4.0 Neuigkeiten.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Behandeln von verknüpften Daten in Entity Framework (Lazy Loading, Eager Loading, und das explizite laden)

- [Lesen von verknüpften Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, MVC-beispielanwendung. Die aufgeführten Methoden gelten auch für modellbindung für Webformulare und Database First Workflow.
- [Data Developer Center - Laden von verknüpften Entitäten](https://msdn.microsoft.com/data/jj574232) (MSDN). Das Entity Framework-Team-Dokumentation zum Laden von Daten beziehen.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optimieren der Leistung von Entity Framework

- [Erweiterte Szenarien für Entity Framework für eine ASP.NET-Anwendung](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Zeigt, wie zum Ausführen von SQL-Anweisungen oder Ihre eigenen gespeicherten Prozeduren aufrufen, zum Deaktivieren des änderungserkennung und Gewusst wie: Deaktivieren der Validierung beim Speichern der Änderungen.
- [Überlegungen zur Leistung für Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Überlegungen zur Leistung (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Maximieren der Leistung mit Entity Framework in einer ASP.NET-Webanwendung](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Gilt für Entity Framework 4.0.
- Siehe auch [optimieren ASP.NET-Datenzugriff](#optimizingdataaccess) weiter unten in diesem Thema.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Behandeln von Parallelität in einer Entity Framework-Anwendung

- [Behandeln von Parallelität mit Entity Framework in einer ASP.NET MVC-Anwendung](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, DbContext-API, über ein MVC-beispielanwendung.
- [Data Developer Center – vollständige Parallelität Muster](https://msdn.microsoft.cus/data/jj592904) (MSDN). Das Entity Framework-Team Concurrency-Dokumentation.
- [Behandeln von Parallelität mit Entity Framework in einer ASP.NET-Webanwendung](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Gilt für Entity Framework 4.0. Datenbank zunächst ObjectContext-API, mit einer Web Forms-beispielanwendung.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Bücher über das Entity Framework

- [Programmierung Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) Julie Lerman und Rowan Miller.
- [Programmierung Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman und Rowan Miller.

Diese Bücher werden auf dem neuesten Stand mit den aktuellen empfohlene Verfahren. Sie bieten eine umfassende noch leicht verständliche Einführung in Entity Framework als verfügbar, auf das Internet. Eine andere Buch [Entity Framework-Programmierung](http://shop.oreilly.com/product/9780596807252.do) von Julie Lerman, größere und umfassendere, aber es ist älter und viele der Techniken Ereignistypen sind nicht mehr empfohlen, zur Verwendung von Entity Framework. Siehe auch die Liste der Bücher, die vom Entity Framework-Team an empfohlen [Data Developer Center - Bücher](https://msdn.microsoft.com/data/aa937716) auf der MSDN-Website.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Andere Entity Framework-Ressourcen

- [Entity Framework (ADO.NET)-Teamblog](https://blogs.msdn.com/b/adonet/). Eine der besten Ressourcen für die aktuellsten Informationen sowie Ankündigungen von neuen Erweiterungen des. Andere Blogs EF-bezogene finden Sie unter der Blogroll am [erste Schritte mit Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Finden Sie unter der **Datenpunkte** Spalte, die häufig zu Themen im Zusammenhang mit dem Entity Framework ist.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Datenbindung in der ASP.NET Web Forms-Anwendungen

- [ASP.NET Web Forms-Datenzugriffsoptionen](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Verwenden von Web Forms-Modellbindung

- [Wurden die Modellbindung und WebForms](https://go.microsoft.com/fwlink/?LinkId=286117). Mithilfe von EF Code First Reihe von Lernprogrammen.
- [Web Forms-Modell Bindung Teil 1: Auswählen von Daten](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (Scott Guthries Blog). In dieser älteren Blogbeiträge hieß diese Eigenschaft, die derzeit ItemType heißt ModelType, andernfalls die darin enthaltenen Informationen ist jedoch gültig.
- [Web Forms-Modell Bindung Teil 2: Filtern von Daten](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (Scott Guthries Blog).
- [Web Forms-Modell Bindung Teil 3: Aktualisieren und Validierung](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (Scott Guthries Blog).
- [ASP.NET 4.5 WebForms-Modellbindung](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (video).
- [Modell Bindung Teil 1: Auswählen von Daten](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (video).
- [Modell Bindung Teil 2 – Filtern](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (video).
- [Erste Schritte mit ASP.NET 4.5 WebForms --Anzeige Elementen und Details](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Verwenden der Web Forms-Steuerelementen für Datenquellen

- [Daten für Webserversteuerelemente](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Ankündigung der Version der Dynamic Data-Anbieter und EntityDataSource-Steuerelement für Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (Microsoft Web Development-Blog).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Verwenden der Web Forms-von datengebundenen Steuerelementen und Datenbindungsausdrücke

- [Wurden die Modellbindung und WebForms](https://go.microsoft.com/fwlink/?LinkId=286117). Reihe von Lernprogrammen, die von EF Code First verwendet.
- [Erste Schritte mit ASP.NET 4.5 WebForms --Anzeige Elementen und Details](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Datensteuerelemente stark typisierte](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (Scott Guthries Blog).
- [Datensteuerelemente stark typisierte](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [ASP.NET 4.5 Web Forms Strong typisierte-Datensteuerelemente](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [Datengebundene Webserversteuerelemente](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Ausdrücke Übersicht über Datenbindung](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Diese Seite nur deckt **Eval** und **binden**; wurde nicht onlinesignierung aktualisiert **Element** und **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Arbeiten mit SQL Server-Datenbanken

- [SQL Server-Datenbankfunktionen](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Eine allgemeine Einführung in einer Vielzahl von SQL Server-Themen werden soll finden Sie in den Einträgen unter diesem im Inhaltsverzeichnis angezeigt.
- [SQL Server-Editionen](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Eine Zusammenfassung der verfügbaren SQL Server-Editionen, mit Links zu weiteren Informationen über die einzelnen Aktionen.)
- [SQL Server-Verbindungszeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Mithilfe von SQL Server Compact für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: Database Product Samples](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). AdventureWorks-Beispieldatenbanken.
- [Installieren von Beispieldatenbanken](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Zusätzlich zu den hier aufgeführten Methoden, Sie können auch herunterladen eines Beispiels MDF-Dateien für die App\_Datenordner eines Webprojekts, konvertieren Sie die Datenbank auf "LocalDB", und erstellen Sie eine LocalDB-Verbindungszeichenfolge. Informationen hierzu finden Sie unter [Vorgehensweise: Aktualisieren auf "LocalDB"](https://msdn.microsoft.com/library/hh873188.aspx).

Siehe auch in den folgenden Abschnitten auf Auswählen zwischen SQL Server und SQL-Datenbank und Arbeiten mit SQL Server Express LocalDB.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Arbeiten mit SQL Server Express LocalDB-Datenbanken

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). Die offizielle auf "LocalDB" MSDN-Einführung.
- [SQL Server-Verbindungszeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Vorgehensweise: Aktualisieren auf "LocalDB"](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). So migrieren Sie eine MDF-Datei von einer früheren Version von SQL Server Express auf "LocalDB". Außerdem müssen Sie diesen Prozess durchlaufen, wenn Sie eine der Herunterladen der [Beispieldatenbanken für SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Einführung in LocalDB, eine verbesserte SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (SQL Server Express-Blog). Verfügt über weitere Hintergrundinformationen zu Warum LocalDB erstellt wurde, als in MSDN enthalten ist.
- [LocalDB: Wo befindet sich meine Datenbank?](https://go.microsoft.com/fwlink/?LinkId=234376) (SQL Server Express-Blog). Informationen zu, in dem LocalDB-Datenbankdateien erstellt werden.
- [Verwendung von LocalDB mit vollständigem IIS, Teil 1: Benutzerprofil](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (SQL Server Express-Blog). LocalDB dient nicht zum Arbeiten mit IIS. Diese Serie von Blogbeiträgen erläutert die Probleme und einige problemumgehungen.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Arbeiten mit SQL Server Express-Datenbanken

- [SQL Server-Verbindungszeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Wenn Sie die Einstellung für die AttachDBFileName Verbindungszeichenfolge mit SQL Server Express verwenden, finden Sie insbesondere die Benutzerinstanz Abschnitt auf dieser Seite.
- [Vorgehensweise des Besitzes für Ihre lokalen SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (SQL Server Express-Blog). Ein häufig auftretendes Problem ist, dass nicht mehr mit SQL Server Express-Datenbanken arbeiten, da Sie kein Administrator auf dem SQL Server Express-Instanz sind. Standardmäßig ist nur die Person, die SQL Server Express installiert ein Administrator an. Dieses Blog wird erläutert, wie selbst einen SQL Server Express-Administrator durchführen, wenn Sie ein Administrator auf dem Computer sind.
- [Kann meine ASP.NET-Webanwendung eine SQL Server Express-Datenbank in einer produktionsumgebung verwenden?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Arbeiten mit Windows Azure SQL-Datenbank

- [Bereitstellen eine Secure ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Windows Azure-Website](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (Microsoft Azure-Website).
- [SQL-Datenbanken](https://docs.microsoft.com/azure/sql-database/) (Microsoft Azure-Website). Beim Abrufen gestarteten Lernprogramme und Anleitungen.
- [Windows Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Der Knoten der obersten Ebene des Inhaltsverzeichnisses für SQL-Datenbank in MSDN.
- [Windows Azure SQL-Datenbank TechNet Wiki-Artikel Index](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (Microsoft TechNet-Website).
- [Anwendungsblock für die Behandlung vorübergehender Fehler](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Ein Framework, das Sie die vorübergehenden Netzwerkfehlern und Verbindungsfehler zu handhaben, die von der Einschränkung zu kann. In einem NuGet-Paket zur Verfügung: [Enterprise Library 5.0 - vorübergehenden Anwendungsblock zur Behandlung](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Erste Schritte mit SQL-Datenbank und Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Windows Azure-Trainingskit](https://www.microsoft.com/download/details.aspx?id=8396) (Microsoft Download Center). Enthält praktische Übungseinheiten für SQL-Datenbank.
- [Community-Forum für Windows Azure SQL-Datenbank](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Verschieben von Windows Azure SQL-Datenbank](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Ein Kapitel eine umfassende End-to-End-Szenario vom Microsoft Patterns and Practices-Team. Beschreibt, warum Sie möglicherweise migrieren möchten, und zum Migrieren von SQL Server mit SQL-Datenbank.
- [Migrieren von SQL Server-Datenbanken zu Windows Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [SQL-Datenbankmigrations-Assistent](http://sqlazuremw.codeplex.com/). Open Source-Tool für die Migration von Datenbanken in und aus SQL-Datenbank.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Auswählen zwischen SQLServer und Windows Azure SQL-Datenbank

- [Vergleichen von SQL Server mit Windows Azure SQL-Datenbank](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (Microsoft TechNet-Website).
- [Migration von Daten zu Windows Azure SQL-Datenbank: Tools und Techniken](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Enthält Abschnitte, die SQL Server mit SQL-Datenbank vergleichen und bieten eine Anleitung dazu, wann von SQL Server mit SQL-Datenbank zu migrieren.
- [Bereitstellungshandbuch für Windows Azure SQL-Datenbank](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (Microsoft TechNet-Website).
- [Einschränkungen für die SQL Server-Funktionen (Windows Azure SQL-Datenbank)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows Azure-Tabellenspeicher und Windows Azure SQL-Datenbank – Vergleich und Gegenüberstellung](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Für eine Anwendung, die Sie in Windows Azure bereitstellen, kann Windows Azure-Tabellenspeicher eine Alternative zum Windows Azure SQL-Datenbank sein. Dieses Thema hilft Ihnen bei der Entscheidung zwischen diesen alternativen.
- [Windows Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Richtlinien und Einschränkungen (Windows Azure SQL-Datenbank)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Arbeiten mit NoSQL-Datenbank-Managementsysteme

- [Windows Azure Data Services](https://www.windowsazure.com/develop/net/data/) (Microsoft Azure site). Finden Sie unter der [Tabellendienst funktionsleitfaden](https://docs.microsoft.com/azure/) und **Big Data** Abschnitt der Seite.
- [ASP.NET Multi-Tier-Anwendung mithilfe von-Speichertabellen, Warteschlangen und Blobs](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure-Website). End-to-End-Lernprogramm mit herunterladbaren Beispiel-Anwendung, die Windows Azure-Speichertabellen NoSQL verwendet.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Verwenden von LINQ-Abfragen in ASP.NET-Anwendungen

- [ASP.NET-Daten Zugriffsoptionen](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Enthält eine Einführung in LINQ.
- [LINQ-Trainingsvideos](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (Joe Stagner Blog).
- [Forum für ASP.NET Thread mit Links zu Ressourcen für dynamische LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Mithilfe von Dynamic Data-Gerüstbau

- [Dynamic Data-Projektvorlagen](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Hinweise dazu, wann Dynamic Data-Projekte verwendet.
- [ASP.NET Dynamic Data](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Sichern von Datenzugriff

- [Schützen des Datenzugriffs in ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Sicherheitsüberlegungen (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Vorgehensweise: Sichern von Verbindungszeichenfolgen bei Verwendung von Steuerelementen für Datenquellen](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optimieren der Leistung im Zugriff

- [Übersicht über die Leistung von ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [ASP.NET-Caching](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Verbessern der ASP.NET-Leistung](https://msdn.microsoft.com/library/ff647787) (MSDN). Eine "Inhalt veraltet" Warnung am oberen Rand dieser Seite jedoch die meisten Informationen noch relevant ist, und es gibt keine vergleichbare aktualisierte Ressource.
- [Verbessern der Leistung von SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Kommentar als der vorherige Link.

Siehe auch [Entity Framework Optimieren der Leistung](#optimizingef) weiter oben in diesem Thema.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Bereitstellen einer Datenbank

- [ASP.NET Web-Bereitstellung – empfohlene Ressourcen](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Zugreifen auf Daten über einen Webdienst

- [Zugreifen auf Daten über einen Webdienst](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Hinweise dazu, wann im Vergleich zu WCF-Web-API verwenden.
- [Erste Schritte mit ASP.NET Web API](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Häufig gestellte Fragen zum Datenzugriff mit ASP.NET](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [ASP.NET Web Forms-Lernprogramme - Daten](../web-forms/overview/data-access/index.md). Die meisten diese Lernprogramme sind relativ alte; Lesen Sie unbedingt [ASP.NET Datenzugriffsoptionen](https://msdn.microsoft.com/library/ms178359.aspx) und [Datenspeicheroptionen (Building Real-World Cloud Apps mit Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) erste, damit Sie nicht zu weit in einen Datenzugriffsmethode erhalten, die nicht von rechts für Ihr Szenario.
- [ASP.NET MVC-Inhaltszuordnung](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [ASP.NET Web Pages Lernprogramme - Daten](../web-pages/overview/data/index.md).
- [Zugreifen auf Daten in Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Enthält eine Liste der Links, die ähnlich wie auf diese inhaltszuordnung jedoch mit dem Schwerpunkt auf Visual Studio anstelle von ASP.NET.
