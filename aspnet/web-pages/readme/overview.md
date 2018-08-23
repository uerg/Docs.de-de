---
uid: web-pages/readme/overview
title: WebMatrix Readme | Microsoft Docs
author: rick-anderson
description: WebMatrix und ASP.NET Web Pages (Razor) Version 1.0-Infodatei
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: aa852e7bbd93622154d59e0d0a13ffa680812df2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838697"
---
<a name="webmatrix-readme"></a>WebMatrix-Infodatei
====================
13 Januar 2011

## <a name="contents"></a>Inhalt

> [!NOTE]
> Diese Infodatei gilt für Version 1.0 von WebMatrix.


- [Übersicht](#Overview)
- [Installation](#Installation_Notes)
- [Gewusst wie: Veröffentlichen von Anwendungen](#InstructionsForPublishingApplications)
- [Änderungen und Probleme](#ChangesAndIssues)

    - [WebMatrix-1.0-Installation](#Known_Issues_Installation)
    - [ASP.NET-Webseiten 2](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Installieren von Anwendungen](#Known_Issues_Installing_Applications)
    - [Veröffentlichen von Anwendungen](#Known_Issues_Publishing_Applications)
- [Weitere Informationen](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Übersicht

> Microsoft WebMatrix 1.0 ist ein kostenloser Web-entwicklungsstapel, die innerhalb von Minuten installiert. Einen Webserver integriert mit der Datenbank und Programmierframeworks erstellen Sie eine einzige, integrierte Benutzeroberfläche. Sie können mithilfe von WebMatrix die zur Optimierung die Sie programmieren, testen und veröffentlichen Ihre eigene Website ASP.NET oder PHP, oder Sie können WebMatrix verwenden, um eine neue Website mithilfe beliebter Open-Source-apps, z. B. DotNetNuke, Umbraco, WordPress oder Joomla. WebMatrix verwendet den gleichen leistungsfähigen Webserver, Datenbank-Engine und frameworkumgebung, die Ihre Website im Internet ausgeführt wird, gestaltet sich der Übergang von der Entwicklung zur Produktion reibungs- und nahtlos.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Installation

> Um WebMatrix 1.0 installieren, installieren Sie zuerst die [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Nachdem Sie den Webplattform-Installer installiert haben, können Sie es zum Installieren von WebMatrix verwenden.
> 
> Wenn Sie während der Installation Probleme auftreten, finden Sie unter [Behandlung von Problemen mit Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Gewusst wie: Veröffentlichen von Anwendungen

> Finden Sie unter [schrittweise Anleitungen zum Veröffentlichen von Anwendungen](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Änderungen und Probleme

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>WebMatrix 1.0 Installation Issues

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problem: WebMatrix 1.0 steht nur auf Plattformen, die Unterstützung von Microsoft .NET Framework 4

> .NET Framework, Version 4 ist erforderlich für WebMatrix. In bestimmten Fällen kann können der WebMatrix-1.0-Installer Sie versuchen, auf einer Plattform zu installieren, die nicht Teil der unterstützte Konfiguration ist. Insbesondere Windows Vista ohne das SP1-Update können Sie die Installation von WebMatrix beginnen, aber die .NET Framework 4-Komponente fehl und die Installation blockieren.
> 
> **Dieses Problem zu umgehen**  
> Installieren Sie auf eine unterstützte Plattform, einschließlich:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 oder höher
> - Windows XP SP3
> - Windows Server 2003 SP2


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problem: WebMatrix 1.0 kann nicht installiert werden, wenn Microsoft Visual Studio 2008 ohne Microsoft Visual Studio 2008 SP1 installiert ist

> **Dieses Problem zu umgehen**  
> Installieren Sie [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) aus dem Microsoft Download Center.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problem: Einige Assemblys für SQL Server Compact 4.0 sind nicht im globalen Assemblycache installiert.

> Die verwalteten Assemblys für SQL Server Compact 4.0 werden nicht im globalen Assemblycache (GAC) abgelegt, wenn Sie SQL Server Compact 4.0 auf einem 64-Bit-Computer installieren, und der Computer nur das .NET Framework 3.5 SP1-Client Profile installiert verfügt. Sind die verwalteten Assemblys, die nicht im GAC installiert werden:
> 
> - *System.Data.SqlServerCe.dll* (ADO.NET-Anbieter)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **Dieses Problem zu umgehen**  
> Deinstallieren von SQLServer Compact 4.0. Herunterladen Sie und installieren Sie die Vollversion von .NET Framework 3.5 SP1 von folgendem Speicherort:  
>   
> [Microsoft .NET Framework 3.5 Service Pack 1 (vollständiges Paket)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Installieren Sie SQL Server Compact 4.0 klicken Sie dann erneut.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problem: Nicht SQL Server Compact über die Befehlszeile deinstallieren.

> Deinstallation von SQL Server Compact mithilfe von Befehlszeilenoptionen funktioniert nicht in dieser Version.
> 
> **Dieses Problem zu umgehen**  
> Verwendung *Programme und Funktionen* in der Windows-Systemsteuerung zum Deinstallieren von Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

Dieser Abschnitt des Dokuments Beschreibt neue Features, Änderungen und bekannte Probleme mit der Version 1.0 von ASP.NET Web Pages mit Razor-Syntax.

- [Neue Features](#NewFeatures)
- [Änderungen](#Changes)
- [Probleme](#Issues)

#### <a id="NewFeatures"></a>  Neue Features

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Neu: Konfigurationseinstellung hinzugefügt, um den Paket-Manager zu deaktivieren.

> Ein neues `asp:AdminManagerEnabled` Schlüssel steht für die `<appSettings>` Element in der *"Web.config"* -Datei, die den Paket-Manager vollständig deaktivieren kann. Der Standardwert für dieses Element ist "true", was bedeutet, dass, wenn es nicht, in enthalten ist der *"Web.config"* der Paket-Manager-Datei aktiviert ist. Fügen Sie das folgende Element hinzu, um den Paket-Manager Deaktivieren der *"Web.config"* Datei im Stammverzeichnis der Website:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  Änderungen

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Änderung: "WebPages:AdminFolderVirtualPath" Schlüssel "Asp: AdminFolderVirtualPath" umbenannt

> Die `webPages:AdminFolderVirtualPath` Schlüssel, der hinzugefügt werden kann die *"Web.config"* Datei an den Speicherort des Paket-Managers Verwendung umbenannt wurde die `asp:` -Namespace anstelle des der `webPages` Namespace. Wenn Sie dieses Element verwendet haben, müssen Sie es in der Konfigurationsdatei umbenennen.


#### <a id="Issues"></a>  Bekannte Probleme

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problem: Von Kennwörtern für Mitgliedschaftsbenutzer, die nicht mehr erkannt

> Der Algorithmus zum Erstellen und Speichern von Kennwörtern für Mitgliedschaft (Anmeldename) wurde geändert, um Sie sicherer werden. Die für Mitglieder (Benutzer) in der Beta-Versionen von ASP.NET Razor erstellte gespeicherte Kennwörter werden daher nicht erkannt werden. 
> 
> **Problemumgehung** Wenn der Standort noch nicht in der Produktion gespeichert wurde, entfernen Sie die Benutzerdatensätze aus der Mitgliedschaftsdatenbank von. Wenn die Datenbank aktiv ist, generieren Sie vorhandene Kennwörter in der Mitgliedschaftsdatenbank programmgesteuert neu.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problem: Unerwartetes Verhalten bei Verwendung einer benutzerdefinierten Tabelle für die Mitgliedschaft

> Um der Mitgliedschaftsanbieter für eine ASP.NET Razor-Website zu initialisieren, rufen Sie die `WebSecurity.InitializeDatabaseConnection` Methode. (In WebMatrix die Vorlage Starter Site enthält einen Aufruf dieser Methode in der  *\_AppStart.cshtml* Datei.) Wenn der `autoCreateTables` Parameter dieser Methode wird festgelegt, auf "true" (standardmäßig es nastaven NA hodnotu true in der Vorlage Starter Site), und wenn der Name einer nicht erkannten an die Methode (der zweite Parameter) übergeben wird, wird die Methode keinen Fehler ausgelöst. Stattdessen wird automatisch in der Tabelle erstellt.
> 
> Dies kann ein Problem sein, wenn Sie beabsichtigen, übergeben den falschen Tabellennamen an aber verwenden eine benutzerdefinierte Tabelle, für die Mitgliedschaft der `WebSecurity.InitializeDatabaseConnection` Methode. Da die Methode nicht standardmäßig einen Fehler ausgelöst wird, wenn die Tabelle, die Sie angeben, nicht vorhanden ist und sie stattdessen eine neue Tabelle erstellt, kann die Anwendung zu funktionieren scheinen. Allerdings kann Anwendungscode, der für die benutzerdefinierte Tabelle (und darin enthaltenen Felder) stützt sich schließlich unerwartete Fehlern auftreten.
> 
> **Dieses Problem zu umgehen**  
> Stellen Sie sicher, dass der Name in übergeben die `InitializeDatabaseConnection` Methode entspricht, das Benutzerprofil in der Mitgliedschaftsdatenbank Tabelle, oder stellen Sie sicher, dass, die `autoCreateTables` Parameter auf "false" festgelegt ist.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>Problem: Fehlermeldung "das Admin-Modul erfordert Zugriff auf ~/App\_Daten"

> Unter bestimmten Umständen möchten Benutzer erstellen oder arbeiten andernfalls mit dem ASP.NET-Mitgliedschaftssystem kann dazu führen, dass die Seite zum Anzeigen des Fehlers *der Admin-Modul erfordert Zugriff auf ~/App\_Daten*. Dies tritt auf, wenn das Konto, das unter IIS oder IIS Express ausgeführt wird nicht über Berechtigungen zum Erstellen und Schreiben in die *App\_Daten* Ordner unter dem Stammverzeichnis der Website. 
> 
> **Problemumgehung** erstellen Sie manuell eine *App\_Daten* Ordner für die Website. Stellen Sie sicher, dass das Windows-Konto, das die Anwendung ausgeführt, unter dem Konto (normalerweise Netzwerkdienst wird) Lese-/Schreibberechtigungen für den Stammordner der Anwendung und für Unterordner, z. B. App hat\_Daten. Ausführlichere Informationen finden Sie im Knowledge Base-Artikel [Probleme mit der SQL Server Express Benutzerinstanziierung und ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problem: "Fehler beim Generieren einer Benutzerinstanz von SQLServer"-Fehler

> Wenn eine WebMatrix-Web-Anwendung SQL Server Express verwendet und IIS 7.5 auf Windows 7 oder Windows Server 2008 R2 ausgeführt wird, können Sie ein Fehler angezeigt, der angibt, dass SQL Server des Pfads des Benutzers lokale Anwendung zur Laufzeit abrufen kann.
> 
> **Problemumgehung** stellen sicher, dass das Windows-Konto, das die Anwendung ausgeführt, unter dem Konto (normalerweise Netzwerkdienst wird) Lese-/Schreibberechtigungen für den Stammordner der Anwendung und für Unterordner wie z. B. *App\_Daten*. Ausführlichere Informationen finden Sie im Knowledge Base-Artikel [Probleme mit der SQL Server Express Benutzerinstanziierung und ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problem: Dateien, die Paket-Manager-Ressourcen oder Paket-Manager-Kennwörter enthalten sind, bereitstellbar unter IIS 6.0 und früher

> Wenn Sie eine ASP.NET Web Pages (Razor)-Anwendung bereitstellen, die mit dem RC2-Release erstellt wurde, und wenn die Anwendung enthält eine *password.txt* oder *packagesources.txt* Datei   */App\_ Daten/Admin*, IIS 6.0 wird die Datei, falls angefordert, wodurch möglicherweise die Kennwörter für die Paket-Manager-Instanz, dienen. 
> 
> **Problemumgehung** Umbenennen der *password.txt* oder *packagesources.txt* Datei *password.config* oder *packagesources.config*. Standardmäßig IIS 6.0 nicht dienen Dateien, die die *config* Erweiterung. (In IIS 7 können keine Dateien in die *App\_Daten* Ordner verarbeitet werden, damit Sie nicht benötigen, um die Dateien umzubenennen.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problem: Deinstallieren von Paketen installiert, mit der Beta 3-Version nicht vollständig Paketkomponenten entfernt

> Wenn Sie ein Paket mit der Paket-Manager in der Beta 3-Version installiert, und wiederholen Sie dann die aktuelle Version deinstallieren, wird das Paket wurde nicht vollständig deinstalliert. Mithilfe des Paket-Managers **Deinstallieren** Schaltfläche entfernt einige Komponenten, jedoch bleibt der Code für die Paket Bibliothek und aktualisiert nicht die *Datei "Package.config"* Datei.
> 
> **Dieses Problem zu umgehen**   
> Gehen Sie wie folgt vor:  
> 1. Löschen der *App\_Data\packages* Ordner. Dadurch werden alle Pakete entfernt.   
> 2. Löschen der *"Packages.config"* Datei im Stammverzeichnis der Website.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problem: In Visual Studio Aufrufen der Web-basierte Paket-Manager die Anwendung offline geschaltet

> Wenn Sie in Visual Studio (nicht WebMatrix) arbeiten, und verwenden Sie die  *\_Admin* Funktionalität, um den Paket-Manager, Visual Studio zu starten, wird die Anwendung offline geschaltet, und stellt die *app\_ offline.htm* in den Stammordner der Website, unterbricht die Ihre Fähigkeit, den Paket-Manager verwenden.
> 
> [!NOTE]
> Obwohl i. d. r. dieses Verhalten angezeigt werden, wenn die Benutzeroberfläche des webbasierten-Paket-Manager verwenden, tritt das gleiche Verhalten auf, wenn Sie hinzufügen, entfernen oder ändern alle Dateien in die *App\_Daten* Ordner.
> 
> **Dieses Problem zu umgehen**   
> Um mit Pakete in Visual Studio arbeiten, verwenden Sie die NuGet-Erweiterung anstelle der webbasierten-Paket-Manager. Weitere Informationen finden Sie unter den [NuGet-Dokumentation](https://docs.microsoft.com/nuget/). Bei Verwendung mit anderen Dateien in die *App\_Daten* Ordner, sollten Sie die Dateien behalten an einem anderen Ort, um dieses Problem zu vermeiden. Wenn dies nicht möglich ist, löschen Sie die *app\_offline.htm* Datei manuell, oder warten Sie, bis der Standort automatisch (standardmäßig nach 30 Sekunden) wieder online geschaltet wird.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problem: Visual Studio IntelliSense und Projektvorlagen nur in ASP.NET MVC 3-Version verfügbar

> Installieren von ASP.NET Web Pages wird nicht auch Tools für Visual Studio wie IntelliSense und Projektvorlagen für ASP.NET Web Pages-Anwendungen installiert.
> 
> **Problemumgehung** um IntelliSense und Projektvorlagen für ASP.NET Web Pages-Anwendungen in Visual Studio zu verwenden, installieren Sie ASP.NET MVC 3 RC entweder über den Webplattform-Installer oder [eigenständiges Installationsprogramm](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problem: Lesen von Feeds oder andere externen Daten über einen Proxyserver

> Ist der Server mit der Website hinter einem Proxyserver befinden, müssen Sie möglicherweise konfigurieren Informationen zum Proxy in der *"Web.config"* Datei, um in der Lage, Informationen zu lesen, die von außerhalb Ihrer Website stammt. Angenommen, Sie verwenden die `ReCaptcha` Hilfsprogramm, das Hilfsprogramm kommuniziert mit dem ReCAPTCHA-Dienst, aber von Ihrem Proxyserver blockiert werden. Auf ähnliche Weise möglicherweise Feeds, die in ASP.NET Web Pages, z. B. den Feed ein, die im Paket-Manager verwendet werden-Proxykonfiguration.
> 
> Bei Problemen in arbeiten mit einem externen Dienst oder Arbeiten mit den paketfeed ein, fügen Sie die folgenden Elemente in Ihrer Anwendung Stamm *"Web.config"* Datei:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Weitere Informationen zum Konfigurieren eines Proxyservers finden Sie unter [ &lt;Proxy&gt; -Element (Netzwerkeinstellungen)](https://msdn.microsoft.com/library/sa91de1e.aspx) auf der MSDN-Website.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problem: Deinstallieren Sie .NET Framework, Version 4 von ASP.NET Web Pages mit Razor-Syntax deaktiviert

> Wenn Sie .NET Framework, Version 4 deinstallieren und anschließend neu installieren, ist die ASP.NET Web Pages mit Razor-Syntax deaktiviert. Seiten mit den *.cshtml* Erweiterung werden nicht ordnungsgemäß ausgeführt werden. ASP.NET Web Pages registriert eine Assembly im Stammverzeichnis Computer *"Web.config"* Datei und Entfernen von .NET Framework wird diese Datei entfernt. Neuinstallation von .NET Framework installiert eine neue Version der Konfigurationsdatei, aber den Verweis wird nicht für die ASP.NET Web Pages-Assembly hinzufügen.
> 
> **Problemumgehung** installieren Sie nach der Neuinstallation von .NET Framework, ASP.NET Web Pages mit Razor-Syntax. Dadurch wird das folgende Element zum hinzugefügt. die *"Web.config"* -Datei in das Stammverzeichnis des Computer, der in der Regel an folgendem Speicherort ist:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problem: URLs ohne Erweiterung.cshtml/.vbhtml-Dateien auf IIS 7 oder IIS 7.5 nicht finden

> Auf IIS 7 oder IIS 7.5, Anforderungen mit einer URL wie im folgenden sind nicht Seiten zu finden, die die *.cshtml* oder *vbhtml* Erweiterung:  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Das Problem tritt auf, weil der URL-Umschreibung nicht standardmäßig für IIS 7 oder IIS 7.5 aktiviert ist. Das likeliest Szenario ist, dass das Problem nicht angezeigt werden, wird wenn Tests lokal mithilfe von IIS Express, aber Sie entdecken Sie es beim Bereitstellen Ihrer Websites auf einer hosting-Website.
> 
> **Dieses Problem zu umgehen**
> 
> - Wenn Sie die Kontrolle über den Server-Computer verfügen, installieren Sie auf dem Computer das Update, das beschrieben ist [ein Update ist verfügbar, können Sie bestimmte IIS 7.0 oder IIS 7.5-Handler behandelt Anforderungen, deren URLs nicht mit einem Punkt enden](https://support.microsoft.com/kb/980368).
> - Wenn Sie keine Kontrolle über den Server-Computer verfügen (z. B. Sie bereitstellen auf einer hosting-Website), fügen Sie Folgendes auf der Website *"Web.config"* Datei: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problem: Bereitstellen einer Anwendung auf einem Computer, die keinen SQL Server Compact installiert

> Anwendungen mit SQL Server Compact-Datenbanken können auf einem Computer ausführen, in dem SQL Server Compact nicht installiert ist. Microsoft WebMatrix 1.0 automatisch diese Binärdateien für die Sie kopiert und führt die entsprechende *"Web.config"* Datei transformiert.
> 
> **Problemumgehung** Wenn müssen Sie diese Dateien zu kopieren, und stellen die *"Web.config"* dateiänderungen manuell, gehen Sie folgendermaßen vor:
> 
> 1. Kopieren Sie die Datenbank-Engine Assemblys, die *Bin* Ordner (sowie Unterordner) der Anwendung auf dem Zielcomputer:  
> 
>    - Kopie *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **um** *\Bin*
>    - Kopie <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>zu</em></strong>\Bin\x86*
>    - Kopie <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>zu</strong><em>\Bin\amd64</em>
> 
> 2. Klicken Sie im Stammordner der Website, erstellen oder öffnen Sie eine *"Web.config"* Datei. (In WebMatrix 1.0 dieser Dateityp ist verfügbar, wenn Sie auf **alle** in die **wählen Sie einen Dateityp** Dialogfeld.)
> 3. Fügen Sie das folgende Element als untergeordnetes Element der `<configuration>` Element (nicht in der `<system.web>` Element):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problem: "Database" und "WebGrid" Hilfsprogramme funktionieren nicht in der mittleren Vertrauensebene in Visual Basic

> Bei Verwendung von Visual Basic (Erstellen von *vbhtml* Dateien), wird die `Database` und `WebGrid` Hilfsprogramme funktioniert nicht, wenn die Anwendung festgelegt wurde, auf die mittlere Vertrauenswürdigkeit verwenden.
> 
> **Dieses Problem zu umgehen**  
> Wenn Sie Visual Studio 2010 verwenden, können Sie dieses Problem beheben, installieren Sie die Service Pack 1-Version. Bis die endgültige Version von der SP1-Version verfügbar ist, können Sie die Betaversion von SP1 von der [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) Seite im Microsoft Download Center.   
>   
> Legen die Anwendung mit voller Vertrauenswürdigkeit, wenn dies nicht praktikabel ist, oder wenn Sie nicht Visual Studio 2010 verwenden, Sie vorübergehend können.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problem: "ApplicationPart" Ressourcen extern zugänglich sind

> Wenn eine Assembly Objekte, die enthält von abgeleitet ist die `ApplicationPart` Klasse, die, die Ressourcen-Assembly verfügbar gemacht werden, indem die `ResourceRouteHandler` Klasse. Nehmen wir beispielsweise die folgende URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Diese Anforderung lädt alle Ressourcenzeichenfolgen in der *System.Web.WebPages.Administration.dll* Assembly. Alle von eingebetteten Ressourcen (einschließlich der nicht als statische Inhalte bereitgestellt werden sollen) werden heruntergeladen. Wenn die eingebetteten Ressourcen sensible Informationen enthalten, kann dies ein Sicherheitsrisiko darstellen. 
> 
> **Dieses Problem zu umgehen**   
> Bei der Erstellung einer **ApplicationPart** Objekt, stellen Sie sicher, dass die eingebetteten Ressourcen, die zugeordnet **ApplicationPart** Assembly Objekts enthalten keine vertraulichen Informationen.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Weitere Informationen zu Installationsproblemen für WebMatrix, finden Sie unter [Probleme bei der Installation von WebMatrix](#Known_Issues_Installation) weiter oben in diesem Dokument.


In diesem Abschnitt des Dokuments beschreibt bekannte Probleme bei der WebMatrix-Entwicklungsumgebung.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problem: Änderungen in der Benutzername oder das Kennwort für eine Datenbank-Verbindungszeichenfolge in einer Datei "Web.config" werden im Arbeitsbereich "Datenbanken" nicht widergespiegelt.

> **Dieses Problem zu umgehen**  
> 
> 1. In der *"Web.config"* Datei, die Namen der Datenbank, in der Verbindungszeichenfolge ändern (z. B. "1" darin hinzufügen).
> 2. Speichern Sie die *"Web.config"* Datei.
> 3. Klicken Sie auf **Datenbanken** und zu aktualisieren.
> 4. Ändern Sie den Datenbanknamen in der Verbindungszeichenfolge in der *"Web.config"* Datei, die den ursprünglichen Datenbanknamen zurück.
> 5. Speichern Sie die *"Web.config"* Datei.
> 6. Klicken Sie auf **Datenbanken** und zu aktualisieren.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problem: Von WebMatrix erstellt Ordner können nicht gelöscht werden

> Wenn WebMatrix mithilfe erhöhter Berechtigungen ausgeführt wird (d. h. Schritten mit der WebMatrix die **als Administrator ausführen** -Option in Windows), Ordnern, die von WebMatrix erstellt werden können nicht gelöscht werden, mit dem Windows-Explorer.
> 
> **Dieses Problem zu umgehen**  
> Führen Sie Windows-Explorer mit erweiterten Berechtigungen. Führen Sie folgende Schritte aus:  
> 
> 1. Klicken Sie in Windows, auf **starten**.
> 2. Geben Sie "Windows-Explorer", und mit der rechten Maustaste in des Eintrags für **Windows Explorer**.
> 3. Klicken Sie auf **als Administrator ausführen**. Anschließend können Sie die Ordner löschen.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problem: WebMatrix 1.0 ist nicht möglich, bestimmte Aufgaben ausführen, die erhöhte Rechte erfordern.

> WebMatrix 1.0 kann nicht für bestimmte Aufgaben, die eine Erhöhung der Rechte wie z. B. die Installation zusätzlicher Komponenten in den folgenden Situationen erfordern:
> 
> - Klicken Sie auf Windows Vista oder Windows 7 Sie angemeldet sind, sich mit einem Konto an, die nicht über Administratorberechtigungen verfügt, und (User Account Control, UAC) ist deaktiviert.
> - Verwenden Sie Microsoft Windows XP oder Microsoft Windows Server 2003.
> 
> **Dieses Problem zu umgehen**  
> Die meisten Aufgaben in WebMatrix 1.0 sind keine Administratorberechtigungen erforderlich. Für diejenigen, die ausführen, können Sie den Vorgang als Administrator ausführen, oder gehen Sie folgendermaßen vor:
> 
> - Auf Windows Vista oder Windows 7 aktiviert.
> - Unter Windows XP müssen fügen Sie den Benutzer auf die Sicherheitsgruppe "Administratoren hinzu".


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problem: "Site from Web Gallery" ist deaktiviert.

> Die **Standort Web Gallery** Option ist deaktiviert, wenn der Webplattform-Installer 3.0 nicht installiert ist.
> 
> **Dieses Problem zu umgehen**  
> Installieren Sie die [Microsoft Webplattform-Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problem: Google Chrome ist nicht verfügbar ist, als eine ausführoption

> Google Chrome wird nicht angezeigt, in der Liste der Browser unter **ausführen** auf die **Startseite** Registerkarte.
> 
> **Dieses Problem zu umgehen**  
> Einige Versionen von Google Chrome ist nicht selbst ordnungsgemäß mit dem Default Programs-Feature in Windows registriert werden. Dieses Problem zu umgehen, starten Sie Google Chrome, klicken Sie auf die *anpassen und Google Chrome-Steuerelement* Menü klicken Sie auf *Optionen*, und klicken Sie dann auf *stellen Google Chrome mein Standardbrowser*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problem: Das Dialogfeld "Foreign Key" lässt keine zu einen primären Schlüssel eingeben

> Die **Fremdschlüssel** Dialogfeld lässt nicht zu der Primärschlüsselname geben an der Primärschlüsseltabelle.
> 
> **Dieses Problem zu umgehen**  
> Dies ist beabsichtigt. Sie müssen nicht den Namen des primären Schlüssels auf der Primärschlüsseltabelle eingeben.


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problem: IntelliSense ist nicht in WebMatrix für Razor-Syntax, c# oder Visual Basic verfügbar sind

> IntelliSense für HTML- und CSS in WebMatrix unterstützt. Allerdings ist sie nicht für andere Sprachen verfügbar. 
> 
> **Dieses Problem zu umgehen**   
> Keine


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problem: IntelliSense für HTML- und CSS-vorgeschlagen Elemente, die nicht angemessen sind.

> IntelliSense für Markup in WebMatrix unterstützt die Verwendung von HTML-die [XHTML 1.0 Transitional Schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) und CSS verwenden die [CSS 2.1-Schema](http://www.w3.org/TR/CSS2/). Da IntelliSense für diese bestimmte Schemas basiert, können bestimmte Tags, Attribute oder Eigenschaften vorgeschlagen, die nicht für die aktuelle Seite oder Definition geeignet sind. Für HTML kann es auch zu unerwartete Vorschläge in Inhalten führen, die interpretiert werden kann, als falsch formatierte XHTML (z. B., wenn Tags nicht geschlossen werden). Dieses Problem möglicherweise deutlicher bemerkbar, wenn die Einfügemarke innerhalb eines Tags nicht vollständig ist; IntelliSense kann in diesem Fall empfohlen, neue öffnenden Tags oder bieten andere falsche Vorschläge. 
> 
> **Dieses Problem zu umgehen**   
> Für HTML sicher, dass Sie in eine wohlgeformte, vollständige XHTML-Seite arbeiten. Für CSS gibt es keine problemumgehung.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problem: IntelliSense wird nicht aufgerufen werden, während der Eingabe

> IntelliSense kann in einigen Fällen nicht aufgerufen werden, wie HTML oder CSS-Code im Editor eingegeben wird. Dies kann insbesondere bei die Einfügemarke direkt neben einem anderen Element oder am Ende einer Datei ist vorkommen. 
> 
> **Dieses Problem zu umgehen**   
> Stellen Sie sicher, dass Leerstellen um die Einfügemarke vorhanden ist, dass die Einfügemarke nicht am Ende einer Datei vorhanden ist. Sie können IntelliSense auch manuell aufrufen, durch Drücken von STRG + LEERTASTE.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problem: Keine Benutzeroberfläche wird zum Deaktivieren von IntelliSense zur Verfügung.

> WebMatrix-1.0 stellt keine Benutzeroberfläche oder einer Geste zum Deaktivieren von IntelliSense bereit. 
> 
> **Dieses Problem zu umgehen**   
> Starten Sie WebMatrix mithilfe des folgenden Befehls, einen Switch enthält, der IntelliSense deaktiviert:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express hat eine eigene Readme-Datei, die unter der folgenden URL verfügbar ist:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp; Clcid = 0 x 409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact verfügt über eine eigene Readme-Datei, die unter der folgenden URL verfügbar ist:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Weitere Informationen zu Problemen, bei denen Installation von SQL Server Compact als Teil von WebMatrix finden Sie unter [Probleme bei der Installation von WebMatrix](#Known_Issues_Installation) weiter oben in diesem Dokument.

### <a id="Known_Issues_Installing_Applications"></a>  Installieren von Anwendungen

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problem: Installieren einer Anwendung kann eine lange dauern, wenn Ordner "Eigene Dateien" des Benutzers auf einer Netzwerkfreigabe umgeleitet wird

> **Dieses Problem zu umgehen**  
> Keine Die Anwendung möglicherweise einige Zeit dauern installieren, jedoch ordnungsgemäß installiert wird.


### <a id="Known_Issues_Publishing_Applications"></a>  Veröffentlichen von Anwendungen

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problem: "erforderlich, dass die Berechtigungen können nicht abgerufen werden" Fehler bei der Veröffentlichung von SQL Compact-Datenbank

> WebMatrix unterstützt das Bereitstellen von unterstützender Binärdateien für SQL Server Compact auf einem Server, der .NET Framework, Version 3.5 mit einer Konfiguration für mittlere Vertrauenswürdigkeit ausgeführt wird nicht vollständig.
> 
> **Dieses Problem zu umgehen**  
> Die bevorzugte umgangen werden, die .NET Framework 4 auf dem Server zu installieren. Führen Sie alternativ folgende Schritte aus:
> 
> 1. Die folgenden Elemente zum Hinzufügen der `SecurityClasses` im Abschnitt *Web\_MediumTrust.config* Datei:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Erstellen Sie einen neuen Berechtigungssatz, der der *Web\_MediumTrust.config* -Datei mit den folgenden Berechtigungen:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Wenden Sie die Berechtigung auf SQL Server Compact festgelegt werden, indem Sie die folgenden Elemente im Platzieren der *Web\_MediumTrust.config* Datei:
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problem: Katalog und PhpBB Webanwendungen zeigt einen Fehler "Dienst ist nicht verfügbar" nach der Veröffentlichung

> In einigen Fällen verursacht das Veröffentlichen einer Anwendung einen Fehler "Dienst ist nicht verfügbar" aus.
> 
> **Dieses Problem zu umgehen**  
> Fügen Sie einen umgekehrten Schrägstrich in WebMatrix (\) am Ende den Servernamen in der **Veröffentlichungseinstellungen** Fenster, und klicken Sie dann die Anwendung erneut veröffentlichen.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problem: Moodle-Website-Layout und die Links sind defekt nach der Veröffentlichung

> Nachdem Sie eine Moodle-Anwendung veröffentlichen, funktioniert die Anwendung nicht ordnungsgemäß.
> 
> **Dieses Problem zu umgehen**  
> Fügen Sie in WebMatrix einen Schrägstrich (/) am Ende der **Standortname** Feld der **Veröffentlichungseinstellungen** Fenster, und klicken Sie dann die Anwendung erneut veröffentlichen.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problem: Veröffentlichen von NopCommerce tritt ein Fehler

> Veröffentlichung NopCommerce schlägt fehl, und meldet ein Fehler wie "Fügen Sie in der Nop\_Protokolltabelle ist fehlgeschlagen."
> 
> **Dieses Problem zu umgehen**  
> 
> 1. Klicken Sie in WebMatrix auf **ausführen** NopCommerce lokal zu starten.
> 2. Melden Sie sich die Seite "Verwaltung".
> 3. Klicken Sie auf die **System** Menü.
> 4. Klicken Sie auf die **Log** Option.
> 5. Klicken Sie auf die **Protokoll löschen** Schaltfläche.
> 6. Veröffentlichen Sie NopCommerce erneut.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problem: Silverstripe CMS zeigt eine "HTTP 500 PHP FCGI Error", wenn Sie eine veröffentlichte Website herunterladen

> **Dieses Problem zu umgehen**  
> Nachdem Sie auf **Download veröffentlichte Website**, überspringen Sie `silverstripe-cache/manifest_main` in **Vorschau veröffentlichen**. Diese Datei wird zum Zwischenspeichern verwendet und bezieht sich auf jedem Computer.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problem: Subtext wird "Serverfehler in Anwendung '/'" angezeigt. Wenn Sie eine veröffentlichte Site herunterladen

> **Dieses Problem zu umgehen**  
> Öffnen Sie die Website des *"Web.config"* -Datei und Ersetzen Sie den Benutzer-ID und das Kennwort in der Datenbank-Verbindungszeichenfolge mit den SQL Server-Administrator-Anmeldeinformationen (die Anmeldeinformationen von "sa").
> 
> Sie können auch wie folgt vorgehen, um das Benutzerkonto zu bieten, mit dem Sie angemeldet sind `db_owner` Berechtigungen:
> 
> 1. Installieren Sie SQL Server Management Studio mit dem Webplattform-Installer.
> 2. Verbinden mit der lokalen SQL Server Express-Instanz (standardmäßig `.\SQLEXPRESS`).
> 3. Klicken Sie auf **Datenbanken** &gt; *[LocalSubtextDatabase]* &gt; **Sicherheit** &gt; **Benutzer** &gt; *[LocalSubtextUser*] (der Standardwert ist `subtextuser`], mit der rechten Maustaste, und klicken Sie auf **Eigenschaften**.
> 4. Wählen Sie **Db\_Besitzer** im Abschnitt Mitgliedschaft Rolle.


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problem: Website funktioniert möglicherweise nicht nach dem Veröffentlichen, wenn das Feld "Ziel-URL" nicht mit http:// oder https:// vorangestellt ist

> In der **Veröffentlichungseinstellungen** Dialogfeld, wenn die Ziel-URL nicht mit beginnt `http://` oder `https://`, die Website funktioniert möglicherweise nicht nach der Bereitstellung.
> 
> **Dieses Problem zu umgehen**  
> Stellen Sie sicher, dass vor dem Veröffentlichen einer Website, die Ziel-URL in die **Veröffentlichungseinstellungen** Dialogfeld beginnt mit `http://` oder `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problem: Veröffentlichen einer MySQL-Datenbank tritt der Fehler "Fehler beim Veröffentlichen der Datenbank. Dies kann passieren, wenn die Remotedatenbank das Skript ausgeführt werden kann."

> Der Fehler kann aus verschiedenen Gründen auftreten. Ein möglicher Grund kann dieser Fehler angezeigt wird, wenn das Datenbankskript enthält eine einfache Anführungszeichen (') und der Ziel-MySQL-Datenbank der Standard-Zeichensatz nicht in UTF-8.
> 
> **Dieses Problem zu umgehen**  
> Legen Sie den Standardzeichensatz für die MySQL-Remotedatenbank in UTF-8.


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problem: Einige Links sind nicht sichtbar in DotNetNuke nach dem Veröffentlichen oder die Website herunterladen

> Wenn Sie veröffentlichen oder eine DotNetNuke-Website herunterladen, müssen Sie zum Löschen des Caches, rufen Sie die neuen Links auf der Website angezeigt werden.
> 
> **Dieses Problem zu umgehen**
> 
> 1. Melden Sie sich als "Host".
> 2. Wechseln Sie zum Hostmenü, und wählen Sie **Hosteinstellungen**.
> 3. Führen Sie einen Bildlauf nach unten, und klicken Sie unter **Erweiterte Einstellungen**, erweitern Sie **Leistungseinstellungen**.
> 4. Klicken Sie auf die **Cache löschen** Link, um Seiten.
> 5. Wechseln Sie zum unteren Rand der Seite, und starten Sie die Anwendung neu.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problem: Einige Links in AtomSite werden unterbrochen, nachdem Sie eine veröffentlichte Website heruntergeladen haben.

> **Dieses Problem zu umgehen**  
> In der *service.config* Datei *users.config* -Datei, und alle *XML* -Dateien, ersetzen Sie die URL-Zeichenfolge (z. B. `http://myhost.com/atomsite`) mit den lokalen Endpunkt (z. B. `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problem: MySQL-basierte Anwendungen wie WordPress nicht veröffentlichen und einen Fehler gemeldet

> WebMatrix installiert standardmäßig MySQL mit dem UTF-8-Zeichensatz. Wenn Sie MySQL selbst installieren, und der Zeichensatz nicht UTF-8 (z. B. befindet er sich Latin1), der Veröffentlichungsprozess für Datenbanken fehlschlagen.
> 
> **Dieses Problem zu umgehen**
> 
> 1. Ändern Sie den Zeichensatz für MySQL in UTF-8. (Weitere Informationen finden Sie unter [Server Zeichensatz und Sortierreihenfolge](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) auf der MySQL-Website.)
> 2. Installieren Sie die Anwendung neu.
> 3. Veröffentlichen Sie die Anwendung erneut.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problem: "Download Standort veröffentlicht" für Anwendungen, Browser-basiertes Setup ein Fehler auftritt

> Einige Anwendungen (z. B. Kentico CMS), müssen Sie sie im Browser zu starten, um nach der Installation von z. B. das Erstellen einer Datenbank auszuführen. Wenn Sie eine Anwendung wie folgt, ohne das Browser-basierte Setup abschließen veröffentlichen, schlägt fehl, es wird versucht, die am selben Standort von einem Remoteserver herunterladen.
> 
> **Dieses Problem zu umgehen**  
> Schließen Sie vor dem Veröffentlichen der Website-Browser-basiertes Setup.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problem: "Download Standort veröffentlicht" schlägt fehl mit eines Datenbankfehlers DotNetNuke und Kooboo-CMS

> Wenn Sie versuchen, eine Anwendung von einem Server herunterzuladen, und Sie über Administratoranmeldeinformationen, in der Datenbank-Verbindungszeichenfolge in verfügen der **Veröffentlichungseinstellungen** Dialogfeld möglicherweise den folgenden Fehler im Protokoll veröffentlichen angezeigt:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Dieses Problem zu umgehen**  
> Sollte möglichst die Website erneut zu veröffentlichen (oder dessen Veröffentlichung) mit nicht-Administrator-Anmeldeinformationen für die Datenbank.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Weitere Informationen

Weitere Informationen zu WebMatrix 1.0 finden Sie unter den folgenden Websites:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Alle Rechte vorbehalten. [Nutzungsbedingungen](https://msdn.microsoft.cos/cc300389.aspx).
