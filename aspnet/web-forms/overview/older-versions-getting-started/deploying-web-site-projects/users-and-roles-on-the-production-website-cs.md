---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
title: Benutzer und Rollen in der Produktionswebsite (c#) | Microsoft Docs
author: rick-anderson
description: Die ASP.NET Website Administration Tool (WSAT) bietet eine webbasierte Benutzeroberfläche zum Konfigurieren von Mitgliedschaft und Rollen und zum Erstellen, bearbeiten, eine...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: dbc54313-5d05-4285-98b3-726edea6d0c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
msc.type: authoredcontent
ms.openlocfilehash: e3e1165959ae47715e0037db7a3bc6ac58807653
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888291"
---
<a name="users-and-roles-on-the-production-website-c"></a>Benutzer und Rollen in der Produktionswebsite (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_cs.pdf)

> Die ASP.NET Website Administration Tool (WSAT) bietet eine webbasierte Benutzeroberfläche zum Konfigurieren von Einstellungen für die Mitgliedschaft und Rollen und zum Erstellen, bearbeiten und Löschen von Benutzern und Rollen. Leider funktioniert die WSAT nur besucht von "localhost", was bedeutet, dass der Produktionswebsite Verwaltungstool über Ihren Browser nicht erreichen können. Die gute Nachricht ist, dass es problemumgehungen, bei die zum Verwalten von Benutzern und Rollen in der Produktion festgelegt. In diesem Lernprogramm überprüft diese umgehungen und andere.


## <a name="introduction"></a>Einführung

ASP.NET 2.0 verwendet eine Anzahl von *Anwendungsdienste*, wobei es sich um eine Suite von Baustein-Dienste, die Sie für Ihre Webanwendung hinzufügen können. Wir hinzugefügt, die Mitgliedschaft und Rollen Dienste auf der Website Buch Reviews zurück die [ *Konfigurieren einer Website, dass verwendet Anwendungsdienste* Lernprogramm](configuring-a-website-that-uses-application-services-cs.md). Der Dienst Mitgliedschaft erleichtert das Erstellen und Verwalten von Benutzerkonten; der Rollendienst bietet eine API zum Kategorisieren von Benutzern in Gruppen. Das Buch Reviews Standort verfügt über drei Benutzerkonten - Scott, Jisun, und Alice- und eine einzelne Rolle Administrator mit Scott und Jisun in der Rolle "Admin".

ASP IST. NET Application-Dienste sind nicht auf eine bestimmte Implementierung gebunden. Stattdessen weisen Sie die Anwendungsdienste für eine bestimmte *Anbieter*, und dieser Anbieter implementiert den Dienst mit einer bestimmten Technologie. Wir konfiguriert das Buch Reviews Web-Anwendung für die Verwendung der `SqlMembershipProvider` und `SqlRoleProvider` Anbieter für die Mitgliedschaft und Rollen. Diese zwei Anbieter Benutzerinformationen Konto und die entsprechende in einer SQL Server-Datenbank speichern und die am häufigsten verwendeten Anbieter für internetbasierte Webanwendungen, die auf einen Webhostinganbieter gehostet werden.

Eine allgemeine Herausforderung für Entwickler unter Verwendung des Diensts Mitgliedschaft und Rollen verwaltet die Benutzer und Rollen in der produktionsumgebung. Wie Sie Löschen eines Benutzerkontos aus der Produktionswebsite, hinzufügen eine neue Rolle oder hinzufügen einen vorhandenen Benutzer zu einer vorhandenen Rolle? In diesem Lernprogramm werden verschiedene Techniken zum Verwalten von Benutzern und Rollen in der Produktionswebsite untersucht.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Verwenden die ASP.NET Webseite-Administrationstool

ASP.NET umfasst einen [Websiteverwaltungs-Tool](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) können sie leicht erstellen und Verwalten von Benutzerkonten und Rollen und Benutzer und rollenbasierte Autorisierungsregeln an. Um die WSAT zu verwenden, klicken Sie auf das Symbol "ASP.NET-Konfiguration" im Projektmappen-Explorer oder wechseln Sie zu der Website oder ein Projekt im Menü, und wählen Sie die Konfigurationsoption für ASP.NET. Beide Vorgehensweisen startet einen Webbrowser und zeigt sie auf die WSAT an einer Adresse wie: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

Die WSAT ist in drei Abschnitte unterteilt:

- **Sicherheit** -Benutzer, Rollen und Autorisierungsregeln verwalten.
- **ApplicationConfiguration** -Verwalten der &lt;"appSettings"&gt; und SMTP-Einstellungen von hier aus. Sie können auch die Anwendung offline schalten und debugging und die Ablaufverfolgung Einstellungen hier verwalten sowie Geben Sie die standardmäßige benutzerdefinierte Fehlerseite.
- **ProviderConfiguration** -Anbieter verwendet, die für die Anwendungsdienste konfigurieren.

Im Abschnitt Sicherheit (angezeigt **Abbildung 1**) enthält Links für neue Benutzer erstellen, Verwalten von Benutzern, erstellen und Verwalten von Rollen, und erstellen und Verwalten von Regeln zugreifen. Von hier aus können Sie eine neue Rolle an das System einen vorhandenen Benutzer zu löschen oder hinzufügen oder Entfernen von Rollen aus einem bestimmten Benutzerkonto.

[![](users-and-roles-on-the-production-website-cs/_static/image2.png)](users-and-roles-on-the-production-website-cs/_static/image1.png)

**Abbildung 1**: Abschnitt Sicherheit WSAT enthält Optionen zum Verwalten von Benutzern und Rollen  
([Klicken Sie hier, um das Bild in voller Größe angezeigt](users-and-roles-on-the-production-website-cs/_static/image3.png))

Leider kann die WSAT nur zugegriffen werden lokal. Die WSAT kann nicht auf die entfernten Produktionswebsite besuchen werden; Wenn Sie besuchen `www.yoursite.com/asp.netwebadminfiles/default.aspx` erhalten Sie eine Antwort 404 Nichtgefunden. Der Code, der verwendet der WSAT Schaltet die `Membership` und `Roles` Klassen in .NET Framework zum Erstellen, bearbeiten und Löschen von Benutzern und Rollen. Diese Klassen finden Sie in der Webanwendung-Konfigurationsinformationen, um zu bestimmen, welche zu verwendenden Anbieter an; wieder in die [ *Konfigurieren einer Website, dass verwendet Anwendungsdienste* Lernprogramm](configuring-a-website-that-uses-application-services-cs.md) wir setup das Buch Reviews Website verwenden die `SqlMembershipProvider` und `SqlRoleProvider` Anbieter. Dies umfasste hinzufügen `<membership>` und `<roleManager>` Abschnitte zu `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-cs/samples/sample1.xml)]

Beachten Sie, dass die `<membership>` und `<roleManager>` Abschnitten Verweis der `SqlMembershipProvider` und `SqlRoleProvider` Anbieter in ihrer `type` -Attribut. Diese Anbieter werden die Rolleninformationen zu Benutzern und in einer angegebenen SQL Server-Datenbank speichern. Die Datenbank, die von diesen Anbietern gemäß der `connectionStringName` -Attribut, `ReviewsConnectionString`, definiert die `~/ConfigSections/databaseConnectionStrings.config` Datei. Bedenken Sie, dass die `databaseConnectionStrings.config` Datei in der Entwicklungsumgebung enthält die Verbindungszeichenfolge in der Entwicklungsdatenbank aus, wohingegen die `databaseConnectionStrings.config` für Produktions-Datei enthält die Verbindungszeichenfolge in der Produktionsdatenbank.

Kurz gesagt, der WSAT lokal über die Entwicklungsumgebung zugegriffen werden muss und funktioniert mit den Informationen für Benutzer und die Rolle in der Datenbank der `databaseConnectionStrings.config` Datei. Daher, wenn wir die Verbindungszeichenfolgeninformationen im Ändern der `databaseConnectionStrings.config` Datei in der Entwicklungsumgebung können Sie verwenden die WSAT lokal, Verwalten von Benutzern und Rollen in der produktionsumgebung.

Um diese Funktionalität zu veranschaulichen, öffnen Sie die `databaseConnectionStrings.config` -Datei in Visual Studio in der Entwicklungsumgebung, und Ersetzen Sie die Verbindungszeichenfolge für die Entwicklung mit der Verbindungszeichenfolge für die Produktion. Starten Sie den WSAT, wechseln Sie die Registerkarte "Sicherheit" und Hinzufügen eines neuen Benutzers mit dem Namen Sam, das Kennwort "Password"! (kleiner kennzeichnet die Anführungszeichen). **Abbildung 2** zeigt den WSAT-Bildschirm, wenn dieses Konto zu erstellen.

[![](users-and-roles-on-the-production-website-cs/_static/image5.png)](users-and-roles-on-the-production-website-cs/_static/image4.png)

**Abbildung 2**: Erstellen eines neuen Benutzers mit dem Namen Sam In der Produktionsumgebung  
([Klicken Sie hier, um das Bild in voller Größe angezeigt](users-and-roles-on-the-production-website-cs/_static/image6.png))

Da wir geändert, dass die Verbindungszeichenfolge in `databaseConnectionStrings.config` verweisen mit dem Produktions-Datenbankserver Sam als Benutzer in der produktionsumgebung hinzugefügt wurde. Um dies zu überprüfen, ändern Sie die Verbindungszeichenfolge in der `databaseConnectionStrings.config` Datei wieder in der Entwicklungsdatenbank aus, und besuchen dann die `Login.aspx` Seite in der Entwicklungsumgebung. Versuchen Sie, die als Sam anzumelden (siehe **Abbildung 3**).

[![](users-and-roles-on-the-production-website-cs/_static/image8.png)](users-and-roles-on-the-production-website-cs/_static/image7.png)

**Abbildung 3**: anmelden kann nicht als Sam in der Entwicklungsumgebung  
([Klicken Sie hier, um das Bild in voller Größe angezeigt](users-and-roles-on-the-production-website-cs/_static/image9.png))

Sie können nicht als Sam in der Entwicklungsumgebung anmelden, weil die Benutzerkontoinformationen in der lokalen Datenbank nicht vorhanden ist. Stattdessen wird der Produktionsdatenbank hinzugefügt wurde. Um dies zu überprüfen, zeigen Sie den Inhalt der `aspnet_Users` Tabelle in der Entwicklungs- und produktionsumgebungen-Datenbanken. In der Entwicklungsumgebung sollte nur drei Datensätze für Benutzer Scott Jisun und Alice vorhanden sein. Allerdings die `aspnet_Users` Tabelle in der Produktionsdatenbank hat vier Datensätze: Scott, Jisun Alice und Sam. Folglich kann Sam über die Website in der Produktion, aber nicht über die Entwicklungsumgebung anmelden.

[![](users-and-roles-on-the-production-website-cs/_static/image11.png)](users-and-roles-on-the-production-website-cs/_static/image10.png)

**Abbildung 4**: Sam kann melden Sie sich bei der Produktionswebsite  
([Klicken Sie hier, um das Bild in voller Größe angezeigt](users-and-roles-on-the-production-website-cs/_static/image12.png))

> [!NOTE]
> Vergessen Sie nicht so ändern Sie die Verbindungszeichenfolge in der `databaseConnectionStrings.config` Datei wieder in der Entwicklungsdatenbank der Zeichenfolge eine Verbindung herstellen, wenn Sie fertig sind arbeiten mit WSAT andernfalls Sie mit Produktionsdaten arbeiten, wenn der Standort über die Entwicklung zu testen Umgebung. Außerdem Bedenken Sie, dass Änderungen an anderen WSAT-Konfigurationsoptionen (Zugriffsregeln, SMTP-Einstellungen, Debug- oder Ablaufverfolgungsausgabe Einstellungen usw.) während der soeben erläuterten Verfahren wir uns die WSAT an Benutzer und Rollen remote verwalten kann, die Ändern`Web.config` Datei. Daher gelten alle Änderungen an den Einstellungen in der Entwicklungsumgebung und nicht in der produktionsumgebung.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Erstellen von benutzerdefinierten Benutzernamen- und Rolle Web Verwaltungsseiten

Die WSAT enthält eine Out-of Feld System zum Verwalten von Benutzern und Rollen, jedoch kann nur lokal gestartet werden und erfordert Änderungen an die Verbindungszeichenfolgeninformationen vorgenommen, um die Benutzer und Rollen für die Produktion zu verwalten. Die meisten Websites, die Benutzerkonten unterstützen enthalten auch eine Reihe von Benutzer- und Rolle Administration-Webseiten, auf denen Administratoren die Verwaltung von Benutzern und Rollen von Seiten innerhalb der Website zu aktivieren. Solche webbasierte Verwaltungsseiten erleichtern Benutzern und Rollen verwalten und für Standorte entscheidend sind, wobei es möglicherweise viele oder Administratoren, auf denen kein Zugriff auf oder die technische Hintergrundinformationen zu Visual Studio verwenden, um die WSAT zu starten.

ASP.NET umfasst eine Reihe von integrierten Anmeldung bezogene Web-Steuerelementen, die Implementierung zahlreicher diese administrativen Webseiten so einfach wie Drag & drop vornehmen. Beispielsweise können Sie eine Seite für Administratoren, um ein neues Benutzerkonto zu erstellen, indem das CreateUserWizard-Steuerelement auf die Seite ziehen und einige Einstellungseigenschaften erstellen. Tatsächlich ist die Seite zum Erstellen von Benutzern in der WSAT in angezeigten **Abbildung 2** verwendet die gleiche CreateUserWizard-Steuerelement, das Sie Seiten hinzufügen können. Darüber hinaus die Mitgliedschaft und Rollen-Services-Funktionen sind verfügbar, programmgesteuert über die `Membership` und `Roles` Klassen in .NET Framework. Mit diesen Klassen können Sie Code zum Erstellen, bearbeiten und Löschen von Benutzern und Rollen sowie zum Hinzufügen oder Entfernen von Benutzern zu Rollen, um zu bestimmen, welche Benutzer in der enthaltenen Rollen vorhanden sind, und für andere Benutzer und rollenbezogenen Aufgaben schreiben.

In der [ *Konfigurieren einer Website, dass verwendet Anwendungsdienste* Lernprogramm](configuring-a-website-that-uses-application-services-cs.md) ich hinzugefügt, dass eine Seite, um die `Admin` Ordner mit dem Namen `CreateAccount.aspx`. Auf dieser Seite können Sie einen Administrator ein neues Benutzerkonto mit dem Standort hinzufügen und angeben, ob der neu erstellte Benutzer in der Rolle "Admin" ist (siehe **Abbildung 5**).

[![](users-and-roles-on-the-production-website-cs/_static/image14.png)](users-and-roles-on-the-production-website-cs/_static/image13.png)

**Abbildung 5**: Administratoren können neue Benutzerkonten erstellen  
([Klicken Sie hier, um das Bild in voller Größe angezeigt](users-and-roles-on-the-production-website-cs/_static/image15.png))

Für eine detaillierte Darstellung erstellen Benutzer und die Rolle Verwaltungsseiten sowie detaillierte Anweisungen zum Verwenden der `Membership` und `Roles` Klassen und die Anmeldung bezogene ASP.NET Web-Steuerelemente, lesen Sie unbedingt die werden meine [Website-Sicherheit Lernprogramme](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Es finden Anleitungen zum Erstellen von Webseiten für neue Konten erstellt haben, erstellen und Verwalten von Rollen, Sie Zuweisen von Benutzern zu Rollen und andere häufige administrative Aufgaben.

Um die WSAT-ähnliche Funktionalität in der Produktionswebsite implementieren können Sie immer eigene Reihe von Webseiten erstellen, die die WSAT Funktionen zu implementieren. Um zu beginnen, sehen Sie sich im Quellcode WSAT, die sich im Ordner befindet `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Eine weitere Möglichkeit besteht, Dan Clems WSAT auch verwenden, da er in seinem Artikel gemeinsam verwendet wird, [parallelen Ihrer eigenen Websiteverwaltungs-Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan führt Leser über den Prozess der Erstellung eines benutzerdefinierten WSAT-ähnliche Tools, enthält seine Anwendungsquellcode (in c#) zum Herunterladen und beschriebenen schrittweisen Anleitungen für eine gehostete Website seiner benutzerdefinierten WSAT hinzugefügt.

## <a name="summary"></a>Zusammenfassung

Der ASP.NET Web Site Administration Tool (WSAT) kann zusammen mit der Anwendungsdienste Mitgliedschaft und Rollen zur Verwaltung der Benutzer und die Rolle Informationen für Ihre Website verwendet werden. Leider wird die WSAT ist nur lokal zugegriffen werden kann und kann nicht aus Ihrem Produktionswebsite besucht werden. Allerdings durch Ändern der Verbindungszeichenfolge in der Entwicklung können Umgebung, in der Produktionsdatenbank zeigen Sie die WSAT Sie die Benutzer und Rollen in der Produktionswebsite verwalten.

Während der WSAT Ansatz eine schnelle und einfache Möglichkeit zum Verwalten von Benutzern und Rollen anwendungsübergreifend, aktualisiert der WSAT aus Visual Studio als auch für temporäre Änderungen an den Verbindungsinformationen für die Zeichenfolge wird gestartet werden. Die WSAT bietet eine schnelle Möglichkeit zum Verwalten von Benutzern und Rollen in der Produktion jedoch sehr aufwändig ist und funktioniert nicht gut für Websites, mit mehreren Administratoren oder mit den Administratoren, die keine oder nicht mit Visual Studio und die WSAT vertraut sind. Aus diesen Gründen enthalten die meisten Websites, die Benutzerkonten unterstützen eine Reihe von administrativen Webseiten. Solche eine Reihe von Webseiten entfällt die Notwendigkeit für die WSAT und von verschiedenen Administratoren von einem beliebigen Computer verwendet.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [ASP wird untersucht. NET Mitgliedschaft, Rollen und Profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Paralleles eigene Websiteverwaltungs-Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Website-Tool (Übersicht)](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Lernprogramme für Website-Sicherheit](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Zurück](precompiling-your-website-cs.md)
> [Weiter](asp-net-hosting-options-vb.md)
