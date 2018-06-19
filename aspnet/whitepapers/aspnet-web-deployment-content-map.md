---
uid: whitepapers/aspnet-web-deployment-content-map
title: ASP.NET Web-Bereitstellung – empfohlene Ressourcen | Microsoft Docs
author: rick-anderson
description: Dieses Thema enthält Links zu Dokumentationen über das Bereitstellen von Ressourcen (veröffentlichen) ASP.NET web-Anwendungen in IIS mithilfe von Visual Studio 2010, Visual Web De...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2014
ms.topic: article
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 78ff183394b5ff92f789b50551d01d28f9bff93b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28048167"
---
<a name="aspnet-web-deployment---recommended-resources"></a>ASP.NET Web-Bereitstellung – empfohlene Ressourcen
====================
> Dieses Thema enthält Links zu Dokumentationen über das Bereitstellen von Ressourcen (veröffentlichen) ASP.NET web-Anwendungen in IIS mithilfe von Visual Studio 2010, Visual Web Developer 2010 und höheren Versionen.
> 
> Wenn Sie wissen, dass eine hervorragende BlogPost: [Stackoverflow](http://stackoverflow.com) Thread oder einen beliebigen anderen Link, der nützlich ist, wäre [senden Sie uns eine e-Mail-Nachricht](mailto:aspnetue@microsoft.com?subject=Deployment Content Map) mit dem Link.
> 
> > [!NOTE] 
> > 
> > Viele dieser Ressourcen Bereitstellungsfunktionen, nur verfügbar sind, wenn Sie eine aktuelle Version der installieren, beschreiben die [Visual Studio Web veröffentlichen Update](https://go.microsoft.com/fwlink/?LinkID=208120). Einige der Funktionen sind nur in Visual Studio 2012 oder Visual Studio 2013 verfügbar.


Dieses Thema enthält folgende Abschnitte:

- [Grundlegendes zu Bereitstellungsoptionen für Webprojekte](#understanding)
- [Suchen von Hostinganbieter für eine ASP.NET-Anwendung](#findinghosting)
- [Bereitstellen einer Webanwendung aus Visual Studio](#fromvs)
- [Bereitstellen einer Web-Anwendung erstellen und installieren Sie ein Webbereitstellungspaket](#package)
- [Bereitstellen einer Webanwendung, die über eine fortlaufende Integration (CI) Prozess](#ci)
- [Mithilfe von "Web.config" Transformationen zum Ändern von Einstellungen in der Datei "Web.config" der Zieldatei oder die Datei "App.config" während der Bereitstellung](#transforms)
- [Verwenden von Parametern Web Deploy zum Ändern der Einstellungen in der Ziel-Webanwendung während der Bereitstellung](#webdeployparms)
- [Sicherstellen einer Anwendung ist offline, während der Bereitstellung](#appoffline)
- [Bereitstellen einer Datenbank oder Änderungen in einer Datenbank als Teil der Bereitstellung von Webanwendungen](#databasewithweb)
- [Bereitstellen einer Datenbank getrennt von der Bereitstellung von Webanwendungen](#databaseseparate)
- [Bereitstellen einer Webanwendung, die ASP.NET-Anwendung verwendet Diensten, z. B. Mitgliedschaft und profilerstellung](#aspnetmembership)
- [Vorkompilieren für die Bereitstellung](#precompiling)
- [Bereitstellen einer Intranet-Web-Anwendung](#intranet)
- [Allgemeine Bereitstellungsaufgaben automatisieren, die der einsatzbereiten nicht automatisiert werden](#automating)
- [Webserver konfigurieren, sodass Entwickler Webanwendungen mithilfe der Web Deploy bereitstellen können](#configuringservers)
- [Konfigurieren von Servern für einen Hostinganbieter](#hostingprovider)
- [Problembehandlung bei Bereitstellungsproblemen](#troubleshooting)
- [Abrufen von Hilfe mit einer bestimmten Bereitstellung Frage](#gettinghelp)
- [Additional Resources](#additional) (Zusätzliche MSBuild-Ressourcen)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Grundlegendes zu Bereitstellungsoptionen für Webprojekte

- [Übersicht über die Bereitstellung für Visual Studio und ASP.NET Web](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Gewusst wie: Bereitstellen eine Windows Azure-Website](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Erläutert die Optionen und Links für Ressourcen zum Bereitstellen von Webprojekten auf Windows Azure-Websites, einschließlich [fortlaufende Übermittlung](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (aus automatisierten [quellcodeverwaltung](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) sowie mit Visual Studio.
- [Visual Studio 2012 Web veröffentlichen Verbesserungen](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (Video von Scott Hanselman).
- [Übersicht über Post für die Bereitstellung in Visual Studio 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (Vishal Joshi Blog). Eine ältere Blogbeitrag, jedoch werden einige der Visual Studio 2010-Ressourcen verknüpft es sich um Informationen zu erhalten, die für Visual Studio 2012 noch relevant ist.


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Suchen von Hostinganbieter für eine ASP.NET-Anwendung

- [Hosten von ASP.NET](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Bereitstellen einer Webanwendung aus Visual Studio

- [Gewusst wie: Bereitstellen eine Windows Azure-Website](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Erläutert Optionen, und enthält Links zu Ressourcen für die Bereitstellung von Webprojekten auf Windows Azure-Websites. Enthält einen Abschnitt zur Bereitstellung von Visual Studio.
- [ASP.NET Web-Bereitstellung mit Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12-Teil Reihe von Lernprogrammen, zeigt, wie zum Bereitstellen von Webanwendungen mit SQL Server-Datenbanken. Für die Datenbank verwendet die Bereitstellung der DbDacFx-Anbieter und die Entity Framework Code First-Migrationen. Enthält auch Informationen zu [Transformationen für die Datei "Web.config"](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [bereitstellen einzelne Dateien](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [befehlszeilenbereitstellung](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md), und [wie Anpassen der Visual Studio Web veröffentlichen Pipeline durch Bearbeiten der pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Gilt für alle ASP.NET-Webprojekte, einschließlich Web Forms, MVC und Web-API.)
- [Vorgehensweise: Bereitstellen einer Web-Projekt mithilfe von One-Click Publish in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (Referenzinformationen für den Visual Studio Web Publish-Assistenten)
- [Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Dies ist eine frühere Version von **ASP.NET Web-Bereitstellung mit Visual Studio** ganz oben in diesem Abschnitt. Vor allem nützlich jetzt Informationen zum Bereitstellen von SQL Server Compact-Datenbanken und zum Migrieren von SQL Server Compact auf eine Vollversion von SQL Server.
- [.NET Multi-Tier-Anwendung mithilfe von-Speichertabellen, Warteschlangen und Blobs](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure-Website). Reihe von Lernprogrammen-Teil 5, zeigt, wie ein MVC-Projekt erstellen und auf einem Windows Azure-Cloud-Dienst bereitgestellt wird.


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Bereitstellen einer Web-Anwendung erstellen und installieren Sie ein Webbereitstellungspaket

- [Vorgehensweise: erstellen ein Webbereitstellungspakets in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Vorgehensweise: Installieren Sie ein Bereitstellungspaket mit der Datei deploy.cmd erstellt die Visual Studio](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [Verwenden Web Deploy-Pakete für die Bereitstellung von IIS auf dem Dev-Feld und einem Drittanbieter-Host](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (Sayed Hashimi-Blog). So verwenden Sie IIS-Manager zum Installieren eines Bereitstellungspakets in IIS auf dem lokalen Computer und auf ein hosting des Unternehmens, die unterstützt IIS-Manager für die Remoteverwaltung.
- [Erstellen eine Web bereitstellen Paket von Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (Website IIS.NET). Enthält Anweisungen zur Erstellung von Paketen über die Befehlszeile und die Installation.
- [Paket einmal veröffentlichen überall](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (Sayed Hashimi-Blog). Stellt ein NuGet-Paket, das transformiert die Datei "Web.config" für mehrere zielumgebungen, automatisiert, damit Sie ein Paket mit mehreren Servern bereitstellen können. Siehe auch die [PackageWeb Video](https://www.youtube.com/watch?v=-LvUJFI8CzM) von Sayed Hashimi.

Siehe auch im folgenden Abschnitt.


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Bereitstellen einer Webanwendung, die über eine fortlaufende Integration (CI) Prozess

- [Fortlaufende Integration und kontinuierlichen Bereitstellung (Building Real-World Cloud-Apps mit Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) E-Book-Kapitel, die fortlaufende Integration und kontinuierlichen Bereitstellung eingeführt werden.
- [Gewusst wie: Bereitstellen eine Windows Azure-Website](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Erläutert die Optionen und Links für Ressourcen zum Bereitstellen von Webprojekten auf Windows Azure-Websites. Enthält einen Abschnitt zum Automatisieren der Bereitstellung über quellcodeverwaltung.
- [Bereitstellen von Webanwendungen in Enterprise-Szenarios](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). 40 zweiteilige Reihe von Lernprogrammen, veranschaulicht die Bereitstellung in einem mit Visual Studio 2010 und Team Foundation Server 2010 CI-Prozess zu automatisieren.
- [In der Microsoft-Buildmodul: Verwenden von MSBuild und Team Foundation Build von Sayed Hashimi und William Bartholomew](http://msbuildbook.com). Dies ist ein Buch, nicht auf eine Webressource, aber es ist ein Leitfaden für das Erlernen von MSBuild für die fortlaufende Integrationsszenarien konfigurieren.
- [MSBuild-Erweiterung Pack](https://github.com/mikefourie/MSBuildExtensionPack). Enthält die Bereitstellungsaufgaben.
- [Team Foundation Build Anpassungshandbuch](https://aka.ms/vsarsolutions). Dokumentation von ALM Rangers zum Einrichten der Team Foundation Server behandelt webbereitstellung und umfasst, Lernprogramme und Videos.
- [SlowCheetah XML transformiert werden, von einem Server CI](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (Sayed Hashimi-Blog). Erklärt, wie SlowCheetah, ein Visual Studio-add-in für das Transformieren von "App.config" und anderen XML-Dateien.

Siehe auch [sicherstellen einer Anwendung offline ist, während der Bereitstellung](aspnet-web-deployment-content-map.md#appoffline) weiter unten auf dieser Seite.


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Mithilfe von "Web.config" Transformationen zum Ändern von Einstellungen in der Datei "Web.config" der Zieldatei oder die Datei "App.config" während der Bereitstellung

- [Datei "Web.config" Transformationen](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Syntax von "Web.config" Transformation für die Bereitstellung mithilfe von Visual Studio-Projekt](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Web Tools 2012.2 - Datei "Web.config" Transformationen](https://www.youtube.com/watch?v=HdPK8mxpKEI) (YouTube-Video von Sayed Hashimi). Veranschaulicht das Einrichten und die Datei "Web.config" Transformationen in der Vorschau anzeigen.
- [Vorgehensweise: Deaktivieren von Web.config-transformation](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Wann sollten Web Deploy-Parameter anstelle von "Web.config" Transformationen werden verwendet?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [XDT (XML-Dokuments transformieren) veröffentlicht, auf codeplex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (Webentwicklung .NET und Tools-Blog). Kündigt die Verfügbarkeit des Quellcodes für die Datei "Web.config" Datei Transformationsmodul und listet einige Tools, die sie verwenden.
- [Windows Azure-Websites: Anwendung wie Zeichenfolgen und Verbindung Zeichenfolgen können](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (Microsoft Azure-Blog). Eine Alternative zur Datei "Web.config" transformiert werden, wenn die zielumgebung Windows Azure-Websites ist und transformiert werden sollen `appSettings` oder `connectionStrings`.


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Verwenden von Parametern Web Deploy zum Ändern der Einstellungen in der Ziel-Webanwendung während der Bereitstellung

- [Vorgehensweise: verwenden Web Deploy-Parameter in einem Webbereitstellungspaket](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy: Zum Aktualisieren von app-Einstellungen auf Veröffentlichen basierend auf dem Veröffentlichungsprofil ein](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (Sayed Hashimi-Blog). Zeigt, wie Web deploy Parameter zur Integration in Visual Studio Profile veröffentlichen.
- [Web-Parametrisierung bereitstellen](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (Website IIS.NET).
- [Bereitstellen der Parametrisierung in Aktion Web](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (Vishal Joshi Blog).
- [Web-Parametrisierung Bereitstellen im Vergleich zu Web.config-Transformation](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (Vishal Joshi Blog).
- [Windows Azure-Websites: Anwendung wie Zeichenfolgen und Verbindung Zeichenfolgen können](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (Microsoft Azure-Blog). Eine Alternative zum Web bereitstellen Parameter aus, wenn die zielumgebung ist Windows Azure-Websites und Sie parametrisieren möchten `appSettings` oder `connectionStrings`.


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Sicherstellen einer Anwendung ist offline, während der Bereitstellung

- [ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen eines Updates Code](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Finden Sie im Abschnitt **wird die Anwendung offline, während der Bereitstellung.**
- [Offlineschalten einer Anwendung vor der Veröffentlichung](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.net Standort). Erläutert, ein Feature in Web Deploy 3.0, die Behandlung einer App automatisiert integriert\_offline.htm-Datei. Dieses Feature funktioniert nicht mit benutzerdefinierten app\_offline.htm Dateien.
- [Wie Ihre Web-app während der Veröffentlichung werden](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (Sayed Hashimi-Blog). Gewusst wie: verwenden eine benutzerdefinierte app automatisieren\_offline.htm-Datei.
- [Web veröffentlichen von Updates für offline-app und Usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (Microsoft Web Development-Blog). Eine weitere Option für die Automatisierung der Verwendung der app\_offline.htm-Datei.
- [Bereitstellen von 3.5 RTW Web](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.net Standort). Neues Feature in Web bereitstellen 3.5 für benutzerdefinierte app\_offline.htm Dateien.


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Bereitstellen einer Datenbank oder Änderungen in einer Datenbank als Teil der Bereitstellung von Webanwendungen

- [Konfigurieren die Bereitstellung in Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Übersicht über die Optionen für die Bereitstellung einer Datenbank mit einem Webprojekt.
- [ASP.NET Web-Bereitstellung mit Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 zweiteilige Reihe von Lernprogrammen, zeigt die Bereitstellung mithilfe von DbDacFx-Anbieter und Entity Framework Code First-Migrationen.
- [Vorgehensweise: Bereitstellen eine Web-Projekts mit einem Klick in Visual Studio veröffentlichen](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Eine sichere ASP.NET MVC 5-app mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Windows Azure-Website bereitstellen](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ein long-Lernprogramm, das Erstellen und Bereitstellen eine Anwendung, die eine einzelne SQL Server verwendet-Datenbank sowohl für die Mitgliedschaft und Anwendungsdaten.
- [Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). 12-Teil Reihe von Lernprogrammen, zeigt die zum Bereitstellen von SQL Server Compact-Datenbanken und zum Migrieren von SQL Server Compact auf eine Vollversion von SQL Server.

Siehe auch durch erstellen ein Webbereitstellungspakets installieren und Bereitstellen einer Webanwendung, die über eine fortlaufende Integration (CI)-Prozess zuvor auf dieser Seite Bereitstellen einer Webanwendung.


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Bereitstellen einer Datenbank getrennt von der Bereitstellung von Webanwendungen

- [SQL Server Datatools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Einschließen von Daten in SQL Server-Datenbankprojekt](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (SQL Server Data Tools-Teamblog). Wie das Schema und Daten bereitstellen, wenn eine Datenbank bereitstellen.
- [Gewusst wie: Bereitstellen einer Datenbank auf Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (Microsoft Azure-Website)
- [Migrieren von Datenbanken zu Windows Azure SQL-Datenbank (früher SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Migrieren einer Datenbank zu SQL Azure mithilfe von SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (SQL Server Data Tools-Teamblog).
- [Migrieren von datenorientierten Anwendungen zu Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [Migrieren von SQL Server-Datenbanken zu Windows Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Bereitstellen einer Webanwendung, die ASP.NET-Anwendung verwendet Diensten, z. B. Mitgliedschaft und profilerstellung

- [Eine sichere ASP.NET MVC 5-app mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Windows Azure-Website bereitstellen](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ein long-Lernprogramm, das Erstellen und Bereitstellen eine Anwendung, die eine einzelne SQL Server verwendet-Datenbank sowohl für die Mitgliedschaft und Anwendungsdaten.
- [ASP.NET Identity](https://asp.net/identity/). Ressourcen für ASP.NET Identity.
- [ASP.NET Web-Bereitstellung mit Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12-Teil Reihe von Lernprogrammen, zeigt, wie eine ASP.NET-Mitgliedschaftsdatenbank bereitstellen.
- [Konfigurieren einer Website, die Anwendungsdienste verwendet](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Für die Website Projekte jedoch ist auch relevant für Webanwendungsprojekte.
- [Benutzer und Rollen in der Produktionswebsite](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Für die Website Projekte jedoch ist auch relevant für Webanwendungsprojekte.


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>Vorkompilieren für die Bereitstellung

- [ASP.NET Web Application Project Vorkompilierung Overview](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Registerkarte "Web" packen/veröffentlichen, die Eigenschaften des Projekts](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Erweiterte Einstellungen (Dialogfeld) Vorkompilieren](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>Bereitstellen einer Intranet-Web-Anwendung

- [Verwenden Sie die Organisationseinheit lokalen Authentifizierungsoption (ADFS) mit ASP.NET in Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (Blog von Vittorio Bertocci.).
- [Vorgehensweise: erstellen eine Intranetsite mit ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). Ältere Exemplarische Vorgehensweise Writen für Visual Studio 2010 reflektiert keinen wichtige Änderungen an der Intranet-Projektvorlagen in Visual Studio 2013 eingeführt wurden.


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Allgemeine Bereitstellungsaufgaben automatisieren, die der einsatzbereiten nicht automatisiert werden

- [ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen von zusätzlichen Dateien](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Festlegen von Berechtigungen für Ordner im Web veröffentlichen](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (Sayed Hashimi-Blog).
- [Gewusst wie: Erweitern der Targets-Datei, um die registrierungseinstellungen für eine Web-Projekt-Paket einschließen](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (Webentwicklungstools-Blog).
- [Erweitern die Transformation für XML ("Web.config")](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (Sayed Hashimi-Blog). Veranschaulicht das Erstellen benutzerdefinierter XDT-Transformationen.
- [Web-Bereitstellungstool (MSDeploy) benutzerdefinierte Anbieter Take 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (Sayed Hashimi-Blog). Zeigt, wie einen benutzerdefinierte Web Deploy-Anbieter zu erstellen.
- [Zum Verpacken und Bereitstellen von COM-Komponenten](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (Webentwicklungstools-Blog).
- [Paket .NET Assemblys wie](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (Webentwicklungstools-Blog). Zum Bereitstellen von Assemblys im GAC.
- [Geben Sie alles - Initialisierung des Windows Azure-VM für Ihre Web-Server mit IIS, Web Deploy und weitere interessante](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (Tugberk Ugurlu Blog).


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Webserver konfigurieren, sodass Entwickler Webanwendungen mithilfe der Web Deploy bereitstellen können

- [Installieren und Konfigurieren von Web Deploy für Administrator- und das Nichtadministrator-Bereitstellungen](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.net Standort).


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>Konfigurieren von Servern für einen Hostinganbieter

- [Microsoft ASP.NET 4-Bereitstellungshandbuch](https://go.microsoft.com/fwlink/?LinkId=191365) (Microsoft Download Center).
- [Generieren Sie ein Profil-XML-Datei](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (IIS.net Standort).


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>Problembehandlung bei Bereitstellungsproblemen

- [Problembehandlung bei Windows Azure-Websites in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (Microsoft Azure-Website).
- [ASP.NET Web-Bereitstellung mit Visual Studio: Problembehandlung bei](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Problembehandlung bei allgemeine Problemen mit Web bereitstellen](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Webdienst-Fehlercodes bereitstellen](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.net Standort).
- [FAQ zur Bereitstellung für Visual Studio und ASP.NET Web](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Unterschiede zwischen IIS und ASP.NET Development Server Core](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Allgemeine Konfigurationsunterschiede zwischen Entwicklungs- und Produktionsumgebungen](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Hosten von ASP.NET-Anwendungen in mit mittlerer Vertrauenswürdigkeit](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 Guys von Rolla-Website).


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>Abrufen von Hilfe mit einer bestimmten Bereitstellung Frage

- [ASP.NET-Konfiguration und Bereitstellung Forum](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).


<a id="additional"></a>


## <a name="additional-resources"></a>Zusätzliche Ressourcen

Dieser Abschnitt enthält Links zu zusätzlichen Ressourcen, die für Weitere Informationen zur Verwendung von Visual Studio und IIS-Bereitstellungstools nützlich sind.

Die folgenden Blogs enthalten häufig Informationen zur Bereitstellung von Visual Studio:

- [Web-Entwicklungstools am Microsoft-Blog](https://blogs.msdn.com/b/webdevtools/).
- [Sayed Hashimi Blog](http://www.sedodream.com/).

Die folgenden Ressourcen bieten Dokumentation zu Web Deploy, das IIS-Framework, das Visual Studio verwendet wird, um Web Application Project Bereitstellungsaufgaben auszuführen. Stellen Sie Fragen zu Web Deploy, in der [Webbereitstellungstool Forum](https://go.microsoft.com/fwlink/?LinkId=149411) auf der Website IIS.net.

- [Einführung in Web Deploy](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Installieren und Konfigurieren der Webadresse bereitstellen](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [PowerShell-Skripts zum Automatisieren von Web Setup bereitstellen](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Web-Bereitstellungstool](https://go.microsoft.com/fwlink/?LinkId=151481). Tabelle der obersten Ebene Inhaltsverzeichnisknoten für Web Deploy-Dokumentation auf der TechNet-Website. Enthält nützliche Informationen jedoch die meisten der TechNet-Seiten nicht Jahre aktualisiert wurden.
- ["Microsoft.Web.Deployment" Namespace](https://go.microsoft.com/fwlink/?LinkId=148630). API-Dokumentation wurde nicht seit Version 1.0 aktualisiert.
- [Microsoft Web Bereitstellung Teamblog](https://blogs.iis.net/msdeploy/default.aspx).
- [Registerkarte "in der Website IIS.net veröffentlichen](https://www.iis.net/learn/publish).
