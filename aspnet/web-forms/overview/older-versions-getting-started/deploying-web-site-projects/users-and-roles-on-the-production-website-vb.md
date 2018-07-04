---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
title: Benutzer und Rollen auf der Produktionswebsite (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Die ASP.NET Website Administration Tool (WSAT) bietet eine webbasierte Benutzeroberfläche zum Konfigurieren von Einstellungen für Mitgliedschaft und Rollen und zum Erstellen, bearbeiten, ein...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 491ed5ae-9be1-4191-87be-65e4e1c57690
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
msc.type: authoredcontent
ms.openlocfilehash: 5b2a5ea84c5c16ad5bf4e3041d31ad29660d965f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380375"
---
<a name="users-and-roles-on-the-production-website-vb"></a>Benutzer und Rollen auf der Produktionswebsite (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_vb.pdf)

> Die ASP.NET Website Administration Tool (WSAT) bietet eine webbasierte Benutzeroberfläche zum Konfigurieren von Einstellungen für Mitgliedschaft und Rollen und zum Erstellen, bearbeiten und Löschen von Benutzern und Rollen. Leider funktioniert das WSAT nur, wenn besucht von "localhost", was bedeutet, dass Sie über Ihren Browser nicht der Produktionswebsite Verwaltungstool erreichen können. Die gute Nachricht ist, dass es problemumgehungen, die zum Verwalten von Benutzern und Rollen in der Produktion ermöglichen. In diesem Tutorial untersucht die folgenden Vorgehensweisen und andere.


## <a name="introduction"></a>Einführung

ASP.NET 2.0 verwendet eine Anzahl von *Anwendungsdienste*, Hierbei handelt es sich um eine Sammlung von Baustein-Dienste, die Sie Ihre Web-Anwendung hinzufügen können. Wir hinzugefügt, die Dienste für Mitgliedschaft und Rollen auf der Website Book Reviews zurück die [ *konfigurieren eine Website, dass verwendet Anwendungsdienste* Tutorial](configuring-a-website-that-uses-application-services-vb.md). Der Mitgliedschaftsdienst erleichtert die Erstellung und Verwaltung von Benutzerkonten; der Rollendienst bietet eine API für die Benutzer in Gruppen kategorisieren. Die Book Reviews-Website verfügt über drei Benutzerkonten - Scott, Jisun, und Alice- und eine Rolle, mit Scott und Jisun in der Rolle "Administrator"-Administrator.

ASP. NET Application-Dienste sind nicht auf eine bestimmte Implementierung gebunden werden. Stattdessen weisen Sie den Anwendungsdiensten, um eine bestimmte *Anbieter*, und dieser Anbieter implementiert den Dienst mit einer bestimmten Technologie. Konfiguriert die Book Reviews-Web-Anwendung zur Verwendung der `SqlMembershipProvider` und `SqlRoleProvider` Anbieter für die Mitgliedschaft und Rollen. Diese zwei Anbieter Benutzerkonto-und Rolle in einer SQL Server-Datenbank zu speichern und die am häufigsten verwendeten Anbieter für Internet-basierte Webanwendungen, die an ein Webhostingunternehmen gehostet werden.

Eine gängige Herausforderung für Entwickler, die mit den Diensten Mitgliedschaften und Rollen verwaltet die Benutzer und Rollen in der produktionsumgebung. Wie Sie das Löschen eines Benutzerkontos aus der Produktionswebsite, eine neue Rolle hinzufügen oder Hinzufügen eines vorhandenen Benutzers zu einer vorhandenen Rolle? Dieses Tutorial werden verschiedene Verfahren zum Verwalten von Benutzern und Rollen auf der Produktionswebsite behandelt.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Mithilfe der ASP.NET Webseite-Administrationstool

ASP.NET umfasst eine [Websiteverwaltungs-Tool](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) können sie leicht erstellen und Verwalten von Benutzerkonten und Rollen und Autorisierungsregeln für Benutzer und rollenbasierte an. Um die WSAT verwenden möchten, klicken Sie auf das Symbol "ASP.NET-Konfiguration" im Projektmappen-Explorer, oder wechseln Sie zu der Website oder das Projekt im Menü aus, und wählen Sie die ASP.NET-Konfiguration-Option. Beide Vorgehensweisen startet einen Webbrowser, und zeigt es auf die WSAT an eine Adresse wie: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

Die WSAT ist in drei Abschnitte unterteilt:

- **Sicherheit** -Verwalten von Benutzern, Rollen und Autorisierungsregeln.
- **ApplicationConfiguration** -Verwalten der &lt;"appSettings"&gt; und SMTP-servereinstellungen von hier aus. Sie können auch die Anwendung offline schalten und Debuggen und Überwachung der Einstellungen von hier aus zu verwalten, sowie die Standardseite für die benutzerdefinierten Fehler angeben.
- **ProviderConfiguration** -konfigurieren Sie die Anbieter, die von der die Anwendungsdienste verwendet.

Abschnitt "Sicherheit" (siehe **Abbildung 1**) enthält Links, für die Erstellung von neuen Benutzern, Verwalten von Benutzern, erstellen und Verwalten von Rollen, und erstellen und Verwalten von Regeln zugreifen. Von hier aus können Sie das System eine neue Rolle hinzufügen, Löschen einen vorhandenen Benutzer hinzufügen oder Entfernen von Rollen von einem bestimmten Benutzerkonto.

[![](users-and-roles-on-the-production-website-vb/_static/image2.png)](users-and-roles-on-the-production-website-vb/_static/image1.png)

**Abbildung 1**: WSAT Abschnitt "Sicherheit" enthält Optionen zum Verwalten von Benutzern und Rollen  
([Klicken Sie, um das Bild in voller Größe anzeigen](users-and-roles-on-the-production-website-vb/_static/image3.png))

Leider ist die WSAT nur zugänglich lokal. Sie können nicht die WSAT auf Ihrer Produktionswebsite remote besuchen; Wenn Sie besuchen `www.yoursite.com/asp.netwebadminfiles/default.aspx` erhalten Sie mit einer 404 nicht gefunden. Der Code, der die WSAT verwendet steuert die `Membership` und `Roles` Klassen in .NET Framework zu erstellen, bearbeiten und Löschen von Benutzern und Rollen. Diese Klassen finden Sie in der Webanwendung-Konfigurationsinformationen, um zu bestimmen, welche Anbieter verwendet; in der [ *konfigurieren eine Website, dass verwendet Anwendungsdienste* Tutorial](configuring-a-website-that-uses-application-services-vb.md) wir verwenden die Book Reviews-Website Einrichten der `SqlMembershipProvider` und `SqlRoleProvider` Anbieter. Dies umfasste hinzufügen `<membership>` und `<roleManager>` zu Abschnitten `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-vb/samples/sample1.xml)]

Beachten Sie, dass die `<membership>` und `<roleManager>` Abschnitte Verweis der `SqlMembershipProvider` und `SqlRoleProvider` Anbieter in ihrer `type` -Attribut. Diese Anbieter werden die Rolleninformationen zu Benutzern und in einer angegebenen SQL Server-Datenbank speichern. Anhand die Datenbank, die von diesen Anbietern verwendet die `connectionStringName` Attribut `ReviewsConnectionString`, definiert in der `~/ConfigSections/databaseConnectionStrings.config` Datei. Bedenken Sie, dass die `databaseConnectionStrings.config` -Datei in der Entwicklungsumgebung enthält die Verbindungszeichenfolge in der Entwicklungsdatenbank aus, während die `databaseConnectionStrings.config` auf Produktions-Datei enthält die Verbindungszeichenfolge in der Produktionsdatenbank.

Kurz gesagt, die WSAT muss lokal zugegriffen werden, über die Entwicklungsumgebung, und es funktioniert mit den Informationen für Benutzer und die Rolle in der Datenbank der `databaseConnectionStrings.config` Datei. Daher, wenn wir die Informationen in der Verbindungszeichenfolge ändern der `databaseConnectionStrings.config` Datei in der Entwicklungsumgebung können verwenden wir die WSAT lokal zum Verwalten von Benutzern und Rollen in der produktionsumgebung.

Um diese Funktion zu veranschaulichen, öffnen Sie die `databaseConnectionStrings.config` Datei in Visual Studio in der Entwicklungsumgebung aus, und Ersetzen Sie die Verbindungszeichenfolge für die Entwicklung mit der Verbindungszeichenfolge für die Produktion. Starten Sie dann die WSAT, wechseln Sie die Registerkarte "Sicherheit" und Hinzufügen eines neuen Benutzers mit dem Namen "Sam", und das Kennwort "Password". (kleiner markiert die Anführungszeichen). **Abbildung 2** zeigt die WSAT-Bildschirm, wenn Sie dieses Konto zu erstellen.

[![](users-and-roles-on-the-production-website-vb/_static/image5.png)](users-and-roles-on-the-production-website-vb/_static/image4.png)

**Abbildung 2**: Erstellen eines neuen Benutzers mit dem Namen Sam In der Produktionsumgebung  
([Klicken Sie, um das Bild in voller Größe anzeigen](users-and-roles-on-the-production-website-vb/_static/image6.png))

Da wir geändert, dass die Verbindungszeichenfolge in `databaseConnectionStrings.config` um mit dem Produktions-Datenbankserver zu verweisen, Sam als Benutzer in der produktionsumgebung hinzugefügt wurde. Um dies zu überprüfen, ändern Sie die Verbindungszeichenfolge in der `databaseConnectionStrings.config` Datei wieder in der Entwicklungsdatenbank aus, und besuchen dann die `Login.aspx` Seite in der Entwicklungsumgebung. Versuchen Sie es für die Anmeldung als Sam (finden Sie unter **Abbildung 3**).

[![](users-and-roles-on-the-production-website-vb/_static/image8.png)](users-and-roles-on-the-production-website-vb/_static/image7.png)

**Abbildung 3**: Sie können nicht melden Sie sich als Sam in der Entwicklungsumgebung  
([Klicken Sie, um das Bild in voller Größe anzeigen](users-and-roles-on-the-production-website-vb/_static/image9.png))

Sie können nicht als Sam in der Entwicklungsumgebung sich anmelden, da die Benutzerkontoinformationen in der lokalen Datenbank nicht vorhanden ist. Es ist in der Produktionsdatenbank hinzugefügt wurde. Um dies zu überprüfen, zeigen Sie den Inhalt der `aspnet_Users` Tabelle in der Entwicklungs- und produktionsumgebungen Datenbanken. In der Entwicklungsumgebung sollte nur drei Datensätze für die Benutzer Alice, Scott und Jisun vorhanden sein. Allerdings die `aspnet_Users` Tabelle in der Produktionsdatenbank hat vier Datensätze: Scott, Jisun, Alice und Sam. Daher kann Sam über die Website in der Produktion, jedoch nicht über die Entwicklungsumgebung anmelden.

[![](users-and-roles-on-the-production-website-vb/_static/image11.png)](users-and-roles-on-the-production-website-vb/_static/image10.png)

**Abbildung 4**: Sam kann melden Sie sich bei der Produktionswebsite  
([Klicken Sie, um das Bild in voller Größe anzeigen](users-and-roles-on-the-production-website-vb/_static/image12.png))

> [!NOTE]
> Vergessen Sie nicht so ändern Sie die Verbindungszeichenfolge in der `databaseConnectionStrings.config` Datei wieder in der Entwicklungsdatenbank der Zeichenfolge eine Verbindung herstellen, wenn Sie fertig sind arbeiten mit WSAT andernfalls Sie mit Produktionsdaten beim Testen der Website durch die Entwicklung arbeiten Umgebung. Auch Bedenken Sie, dass Änderungen von anderen WSAT-Konfigurationsoptionen (Zugriffsregeln, SMTP-Einstellungen, Debug- und Ablaufverfolgungsfunktionen Einstellungen usw.) während der soeben erläuterten Verfahren wir uns die WSAT an Remote-Benutzer und Rollen verwalten kann, die Ändern`Web.config` Datei. Infolgedessen gelten alle Änderungen an den Einstellungen der Entwicklungsumgebung und nicht in der produktionsumgebung bereit.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Erstellen von benutzerdefinierten und Rolle Management-Webseiten

Die WSAT enthält einen Out-of System Feld für die Verwaltung von Benutzern und Rollen, aber nur lokal gestartet werden und erforderlich ist, dass Änderungen auf die Informationen in Verbindungszeichenfolgen, um die Benutzer und Rollen in der Produktion zu verwalten. Die meisten Websites, die die Benutzerkonten unterstützen umfassen auch eine Reihe von Benutzer und Rolle Verwaltung von Webseiten, mit denen Administratoren, Benutzer und Rollen von Seiten innerhalb der Website zu verwalten. Solche Seiten webbasierte Verwaltung machen es einfacher, Benutzer und Rollen verwalten und für Websites von entscheidender Bedeutung sind, gibt es möglicherweise viele oder Administratoren, die nicht den Zugriff auf oder dem technischen Hintergrund zur Visual Studio zu verwenden, um die WSAT starten verfügen.

ASP.NET umfasst eine Reihe von integrierten Web anmeldebezogene-Steuerelementen, die Implementierung vieler diese administrativen Webseiten, die so einfach wie Drag & drop. Beispielsweise können Sie eine Seite für Administratoren, um ein neues Benutzerkonto zu erstellen, durch Ziehen des Steuerelements CreateUserWizard auf der Seite und Festlegen einiger Eigenschaften erstellen. Tatsächlich ist die Seite für das Erstellen von Benutzern in der WSAT Siehe **abbildung2** verwendet die gleiche CreateUserWizard-Steuerelement, das Sie Ihren Seiten hinzufügen können. Darüber hinaus sind die Mitgliedschaft und Rollen-Services-Funktionen verfügbar, programmgesteuert über die `Membership` und `Roles` Klassen in .NET Framework. Mit diesen Klassen können Sie schreiben Code zum Erstellen, bearbeiten und Löschen von Benutzern und Rollen auch hinzufügen oder Entfernen von Benutzern zu Rollen, um zu bestimmen, welche Benutzer welche Rollen werden und andere Benutzer und Rollen-bezogenen Aufgaben auszuführen.

In der [ *konfigurieren eine Website, dass verwendet Anwendungsdienste* Tutorial](configuring-a-website-that-uses-application-services-vb.md) ich hinzugefügt, dass eine Seite, um die `Admin` Ordner mit dem Namen `CreateAccount.aspx`. Auf dieser Seite können Sie einen Administrator ein neues Benutzerkonto mit dem Standort hinzufügen und angeben, ob der neu erstellte Benutzer in der Rolle "Administrator" ist (siehe **Abbildung 5**).

[![](users-and-roles-on-the-production-website-vb/_static/image14.png)](users-and-roles-on-the-production-website-vb/_static/image13.png)

**Abbildung 5**: Administratoren können neue Benutzerkonten erstellen  
([Klicken Sie, um das Bild in voller Größe anzeigen](users-and-roles-on-the-production-website-vb/_static/image15.png))

Ausführlichere Informationen zu erstellen sowie eine schrittweise Anleitung zur Verwendung von Benutzer und die Rolle Verwaltungsseiten der `Membership` und `Roles` Klassen und der ASP.NET Web anmeldebezogene Steuerelemente, achten Sie darauf, lesen Sie meine [Website-Sicherheit Lernprogramme](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Gibt es finden Anleitungen zum Erstellen von Webseiten für neue Konten erstellen, erstellen und Verwalten von Rollen, Sie Zuweisen von Benutzern zu Rollen und andere allgemeinen Verwaltungsaufgaben.

Zum Implementieren von WSAT-ähnliche Funktionen auf der Produktionswebsite können Sie immer eigene Reihe von Webseiten erstellen, die die WSAT Funktionen zu implementieren. Um zu beginnen, sehen Sie sich den Quellcode WSAT, der sich im Ordner befindet `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Eine weitere Möglichkeit ist die Verwendung des Dan Clem WSAT Alternative, die er in seinem Artikel freigegeben hat, [parallelen Ihrer eigenen Web Site Administration Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan führt die Leser durch die Erstellung von benutzerdefinierten WSAT-ähnlichen Tools, enthält seine Anwendungsquellcode (in c#) zum Herunterladen und bietet eine ausführliche Anleitung zum Hinzufügen von seiner benutzerdefinierten WSAT an eine gehostete Website.

## <a name="summary"></a>Zusammenfassung

Der ASP.NET Web Site Administration Tool (WSAT) zur Verfügung kann zusammen mit der die Anwendungsdienste Mitgliedschaften und Rollen verwendet werden, um Benutzer und die Rolle Informationen für Ihre Website zu verwalten. Leider die WSAT ist nur lokal verfügbar und kann nicht aus Ihrer Produktionswebsite besucht werden. Durch Ändern der Verbindungszeichenfolge in die Entwicklung können Umgebung für die Sie in der Produktionsdatenbank zu verweisen die WSAT jedoch die Benutzer und Rollen auf der Produktionswebsite zu verwalten.

Während der WSAT Ansatz eine schnelle und einfache Möglichkeit zum Verwalten von Benutzern und Rollen ermöglicht, erfordert es die WSAT aus Visual Studio als auch für temporäre Änderungen auf die Informationen in Verbindungszeichenfolgen zu starten. Die WSAT bietet eine schnelle Möglichkeit zum Verwalten von Benutzern und Rollen für die Produktion, aber ist mühsam und funktioniert nicht gut für Websites mit mehreren Administratoren und Administratoren, die keine oder nicht mit Visual Studio und die WSAT vertraut sind. Aus diesen Gründen sind die meisten Websites, die Benutzerkonten unterstützt einen Satz von Webseiten zur Verwaltung. Solche ein Satz von Webseiten beseitigt die Notwendigkeit der WSAT und von verschiedenen Administratoren von jedem Computer verwendet.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Untersuchen ASP. NET Mitgliedschaft, Rollen und Profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Paralleles eigene Websiteverwaltungs-Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Website-Tool (Übersicht)](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Website-Lernprogramme zur ASP.NET-Sicherheit](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Vorherige](precompiling-your-website-vb.md)
