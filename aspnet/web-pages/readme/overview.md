---
uid: web-pages/readme/overview
title: WebMatrix-Infodatei | Microsoft Docs
author: rick-anderson
description: WebMatrix und ASP.NET Web Pages (Razor) Version 1.0-Infodatei
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: c65ee58b8c13b0b4acb6e7c9b631c8235e791506
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="webmatrix-readme"></a>WebMatrix-Infodatei
====================
13 Januar 2011

## <a name="contents"></a>Inhalt

> [!NOTE]
> Diese Infodatei gilt für die Version 1.0 von WebMatrix.


- [Übersicht](#Overview)
- [Installation](#Installation_Notes)
- [Gewusst wie: Veröffentlichen von Anwendungen](#InstructionsForPublishingApplications)
- [Änderungen und Probleme](#ChangesAndIssues)

    - [WebMatrix 1.0-Installation](#Known_Issues_Installation)
    - [ASP.NET-Webseiten 2](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Installieren von Anwendungen](#Known_Issues_Installing_Applications)
    - [Veröffentlichen von Anwendungen](#Known_Issues_Publishing_Applications)
- [Weitere Informationen](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Übersicht

> Microsoft WebMatrix 1.0 ist ein kostenloses Web Development Stapel, der in Minuten installiert. Einen Webserver integriert mit der Datenbank und die Programmierung Frameworks So erstellen eine einzige, integrierte Benutzeroberfläche. Sie können WebMatrix verwenden, um die Möglichkeit zu optimieren, die Sie schreiben, testen und Ihre eigenen ASP.NET- oder PHP-Website veröffentlichen, oder verwenden Sie WebMatrix, um einer neuen Website mithilfe beliebte Open Source-Anwendungen z. B. DotNetNuke, Umbraco, WordPress oder Joomla zu starten. WebMatrix verwendet den gleichen leistungsfähigen Webserver, Datenbank-Engine und frameworkumgebung, die Ihre Website im Internet ausführen werden, gestaltet sich der Übergang von der Entwicklung bis hin zur Produktion reibungs- und nahtlos.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Installation

> Zum Installieren von WebMatrix 1.0 installieren Sie zuerst die [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Nachdem Sie den Webplattform-Installer installiert haben, können Sie es zum Installieren von WebMatrix verwenden.
> 
> Wenn Sie während der Installation Probleme auftreten, finden Sie in [Behandlung von Problemen mit Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Gewusst wie: Veröffentlichen von Anwendungen

> Finden Sie unter [schrittweise Anleitungen zum Veröffentlichen von Anwendungen](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Änderungen und Probleme

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>WebMatrix 1.0-Installationsprobleme

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problem: WebMatrix 1.0 steht nur für Plattformen, die Microsoft .NET Framework 4 zu unterstützen

> .NET Framework, Version 4 ist erforderlich für WebMatrix. In bestimmten Fällen kann können das Installationsprogramm WebMatrix 1.0 Sie versuchen, auf einer anderen Plattform zu installieren, die nicht Teil der unterstützte Konfiguration ist. Insbesondere Windows Vista ohne das SP1-Update können Sie die Installation von WebMatrix zu starten, aber die .NET Framework 4-Komponente fehl, und blockieren die Installation.
> 
> **Dieses Problem zu umgehen**  
> Installieren Sie auf einer unterstützten Plattform, darunter:
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


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problem: Einige Assemblys für SQL Server Compact 4.0 werden nicht im GAC installiert.

> Die verwalteten Assemblys für SQL Server Compact 4.0 werden nicht im globalen Assemblycache (GAC) abgelegt, wenn Sie SQL Server Compact 4.0 auf einem 64-Bit-Computer installieren, und der Computer nur .NET Framework 3.5 SP1-Client Profile installiert verfügt. Sind die verwalteten Assemblys, die nicht im GAC installiert werden:
> 
> - *"System.Data.SqlServerCe.dll"* (ADO.NET-Anbieter)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )
> 
> **Dieses Problem zu umgehen**  
> Deinstallieren von SQLServer Compact 4.0. Herunterladen Sie und installieren Sie die Vollversion von .NET Framework 3.5 SP1 vom folgenden Speicherort:  
>   
> [Microsoft .NET Framework 3.5 Service Pack 1 (gesamtes Paket)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Neuinstallieren von SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problem: Nicht SQL Server Compact über die Befehlszeile deinstallieren.

> Deinstallation von SQL Server Compact über Befehlszeilenoptionen funktioniert nicht in dieser Version.
> 
> **Dieses Problem zu umgehen**  
> Verwendung *Programme und Funktionen* in der Windows-Systemsteuerung zum Deinstallieren von Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

Dieser Abschnitt des Dokuments Beschreibt neue Funktionen, Änderungen und bekannte Probleme mit der Version 1.0 der ASP.NET Web Pages mit Razor-Syntax.

- [Neue Funktionen](#NewFeatures)
- [Änderungen](#Changes)
- [Probleme](#Issues)

#### <a id="NewFeatures"></a>  Neue Funktionen

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Neu: Konfigurationseinstellung hinzugefügt, um die Paket-Manager zu deaktivieren.

> Ein neues `asp:AdminManagerEnabled` Schlüssel steht für die `<appSettings>` Element in der *"Web.config"* -Datei, die Sie die Paket-Manager vollständig deaktivieren kann. Der Standardwert für dieses Element ist "true" bedeutet, dass, wenn er nicht in eingeschlossen ist der *"Web.config"* Datei, die Paket-Manager aktiviert ist. Um die Paket-Manager deaktivieren möchten, fügen Sie das folgende Element auf der *"Web.config"* Datei im Stammverzeichnis der Website:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  Änderungen

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Änderung: "WebPages:AdminFolderVirtualPath" Schlüssel wurde der Begriff "Asp: AdminFolderVirtualPath"

> Die `webPages:AdminFolderVirtualPath` Schlüssel, der hinzugefügt werden kann die *"Web.config"* Datei an den Speicherort des Paket-Managers Verwendung umbenannt wurde die `asp:` -Namespace anstelle des der `webPages` Namespace. Wenn Sie dieses Element verwendet haben, müssen Sie es in der Konfigurationsdatei umbenennen.


#### <a id="Issues"></a>  Bekannte Probleme

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problem: Kennwörtern für Mitgliedschaftsbenutzer, die nicht mehr erkannt.

> Der Algorithmus zum Erstellen und Speichern von Kennwörtern Mitgliedschaft (Anmeldenamen) wurde geändert, um sicherer zu sein. Für Mitglieder (Benutzer) erstellt, in der Beta-Versionen von ASP.NET Razor gespeicherten Kennwörter werden deshalb nicht erkannt. 
> 
> **Problemumgehung** Wenn der Standort noch nicht in der Produktion verwendet wurde, entfernen Sie die Benutzerdatensätze aus der Mitgliedschaftsdatenbank. Wenn die Datenbank aktiv ist, generieren Sie vorhandene Kennwörter in der Mitgliedschaftsdatenbank programmgesteuert neu.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problem: Unerwartetes Verhalten bei Verwendung einer benutzerdefinierten Tabelle für die Mitgliedschaft

> Um der Mitgliedschaftsanbieter für eine ASP.NET Razor-Website zu initialisieren, rufen Sie die `WebSecurity.InitializeDatabaseConnection` Methode. (In WebMatrix die Vorlage Starter Site enthält einen Aufruf dieser Methode in der  *\_AppStart.cshtml* Datei.) Wenn die `autoCreateTables` Parameter dieser Methode wird festgelegt auf "true" (, es ist standardmäßig festgelegt in der Vorlage Starter Site "true"), und wenn der Name einer nicht erkannten an die Methode (der zweite Parameter) übergeben wird, wird die Methode keinen Fehler ausgelöst. Stattdessen wird automatisch der Tabelle erstellt.
> 
> Dies kann ein Problem sein, wenn Sie beabsichtigen, verwenden eine benutzerdefinierte Tabelle, für die Mitgliedschaft jedoch übergeben den Namen der falschen Tabelle die `WebSecurity.InitializeDatabaseConnection` Methode. Da die Methode nicht standardmäßig einen Fehler ausgelöst wird, wenn die Tabelle, die Sie angeben, nicht vorhanden ist und sie stattdessen eine neue Tabelle erstellt, kann die Anwendung angezeigt, zu funktionieren. Anwendungscode, der für die benutzerdefinierte Tabelle (und auf Felder) nutzt kann jedoch letztendlich mit unerwarteten Fehlern fehlschlagen.
> 
> **Dieses Problem zu umgehen**  
> Stellen Sie sicher, dass der Name im übergeben der `InitializeDatabaseConnection` Methode entspricht das Benutzerprofil in der Mitgliedschaftsdatenbank-Tabelle ab, oder stellen Sie sicher, dass die `autoCreateTables` Parameter auf "false" festgelegt ist.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>Problem: Fehlermeldung "das Admin-Modul benötigt Zugriff auf ~/App\_Daten"

> Unter bestimmten Umständen Benutzer erstellen, oder andernfalls mit dem ASP.NET-Mitgliedschaftssystem arbeiten möchten kann dazu führen, dass die Seite, um den Fehler anzuzeigen, *das Admin-Modul benötigt Zugriff auf ~/App\_Daten*. Dies tritt auf, wenn das Konto, unter dem IIS- oder IIS Express ausgeführt wird, nicht über Berechtigungen zum Erstellen und Schreiben in die *App\_Daten* Ordner unter dem Stammverzeichnis der Website. 
> 
> **Problemumgehung** erstellen Sie manuell eine *App\_Daten* Ordner für die Website. Stellen Sie sicher, dass das Windows-Konto, das die Anwendung unter (i. d. r. NETWORK SERVICE) ausgeführt wird, über Lese-/Schreibberechtigungen für den Stammordner der Anwendung und für Unterordner, z. B. eine App verfügt\_Daten. Ausführlichere Informationen finden Sie im Knowledge Base-Artikel [bei Problemen mit SQL Server Express Benutzerinstanziierung und ASP.net Web-Anwendungsprojekten](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problem: "Fehler beim Generieren einer Benutzerinstanz von SQLServer"-Fehler

> Wenn eine Webanwendung mit WebMatrix SQL Server Express verwendet und IIS 7.5 auf Windows 7 oder Windows Server 2008 R2 ausgeführt wird, wird möglicherweise einen Fehler angezeigt, der angibt, dass SQL Server nicht lokalen Anwendungspfad des Benutzers zur Laufzeit abrufen.
> 
> **Problemumgehung** stellen Sie sicher, dass das Windows-Konto, das die Anwendung unter (i. d. r. NETWORK SERVICE) ausgeführt wird, wie z. B. Lese-/Schreibberechtigungen für den Stammordner der Anwendung und für Unterordner hat *App\_Daten*. Ausführlichere Informationen finden Sie im Knowledge Base-Artikel [bei Problemen mit SQL Server Express Benutzerinstanziierung und ASP.net Web-Anwendungsprojekten](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problem: Die Paket-Manager-Ressourcen oder die Paket-Manager-Kennwörter enthaltenen Dateien sind servable unter IIS 6.0 und früher

> Wenn Sie eine ASP.NET Web Pages (Razor)-Anwendung bereitstellen, die mit dem RC2-Version erstellt wurde, und wenn die Anwendung enthält eine *password.txt* oder *packagesources.txt* Datei unter */App\_ Daten/Admin*, IIS 6.0, die Datei ggf. potenziell verfügbar macht die Kennwörter für die Paket-Manager-Instanz bereit. 
> 
> **Problemumgehung** Umbenennen der *password.txt* oder *packagesources.txt* Datei *password.config* oder *packagesources.config*. Wird standardmäßig IIS 6.0 nicht dienen Dateien mit der *config* Erweiterung. (In IIS 7 können keine Dateien in den *App\_Daten* Ordner verarbeitet werden, damit Sie nicht benötigen, werden die Dateien umbenannt.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problem: Deinstallieren Pakete installiert, über die Beta 3-Version entfernt jedoch nicht vollständig Paketkomponenten

> Wenn Sie ein Paket mithilfe des Paket-Managers in der Beta 3-Version installiert, und wiederholen Sie dann mit der aktuellen Version deinstallieren, wird das Paket wurde nicht vollständig deinstalliert. Mithilfe des Paket-Managers **Deinstallieren** Schaltfläche einige Komponenten entfernt, aber bewirkt, dass das Paket Bibliothekscode und nicht mehr aktualisiert die *package.config* Datei.
> 
> **Dieses Problem zu umgehen**   
> Gehen Sie wie folgt vor:  
> 1. Löschen der *App\_Data\packages* Ordner. Dadurch werden alle Pakete entfernt.   
> 2. Löschen der *"Packages.config"* Datei im Stammverzeichnis der Website.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problem: In Visual Studio Aufrufen der webbasierten-Paket-Manager die Anwendung offline geschaltet

> Wenn Sie in Visual Studio (nicht WebMatrix) arbeiten, und verwenden Sie die  *\_Admin* Funktionalität zum Starten des Paketmanager, Visual Studio die Anwendung offline geschaltet und sendet die *app\_ offline.htm* in den Stammordner der Website, die die Möglichkeiten zum Verwenden des Package Manager werden getrennt.
> 
> [!NOTE]
> Obwohl i. d. r. dieses Verhalten angezeigt werden, wenn der webbasierte Paket-Manager-Oberfläche verwenden, tritt das gleiche Verhalten auf, wenn Sie hinzufügen, entfernen oder ändern alle Dateien in der *App\_Daten* Ordner.
> 
> **Dieses Problem zu umgehen**   
> Verwenden Sie zum Arbeiten mit Paketen in Visual Studio der NuGet-Erweiterungs anstelle der webbasierten-Paket-Manager ein. Informationen finden Sie unter der [NuGet-Dokumentation](https://docs.microsoft.com/nuget/). Wenn es sich bei der Arbeit mit anderen Dateien in der *App\_Daten* Ordner, sollten Sie die Dateien behalten an anderer Stelle, um dieses Problem zu vermeiden. Wenn dies nicht möglich ist, löschen Sie die *app\_offline.htm* -Datei manuell ein, oder warten Sie, bis der Standort automatisch (standardmäßig nach 30 Sekunden) wieder online geschaltet wird.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problem: Visual Studio IntelliSense-Projekt Vorlagen und nur in ASP.NET MVC 3-Version verfügbar

> Installieren ASP.NET Web Pages installiert nicht auch Tools für Visual Studio z. B. IntelliSense und Projekt Vorlagen für ASP.NET Web Pages-Anwendungen.
> 
> **Problemumgehung** um IntelliSense und Projekt Vorlagen für ASP.NET Web Pages-Anwendungen in Visual Studio verwenden, installieren Sie ASP.NET MVC 3 RC entweder über den Webplattform-Installer oder [eigenständiges Installationsprogramm](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problem: Lesen von Feeds oder andere externen Daten über einen Proxyserver

> Wenn der Server mit dem Standort hinter einem Proxyserver ist, müssen Sie möglicherweise konfigurieren Proxyinformationen in die *"Web.config"* Datei, um in der Lage, Informationen zu lesen, die sich außerhalb Ihrer Website stammen. Angenommen, Sie verwenden die `ReCaptcha` Hilfsprogramm, das Hilfsprogramm kommuniziert mit dem ReCAPTCHA-Dienst, aber vom Proxyserver blockiert werden. Auf ähnliche Weise möglicherweise Feeds, die in ASP.NET Web Pages, z. B. den Feed vom Paketmanager verwendet verwendet werden die Proxykonfiguration.
> 
> Wenn Sie Probleme beim Arbeiten mit einem externen Dienst oder Arbeiten mit dem feed-Paket auf, Stammverzeichnis Ihrer Anwendung die folgenden Elemente abgelegt *"Web.config"* Datei:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Weitere Informationen zum Konfigurieren eines Proxyservers finden Sie unter [ &lt;Proxy&gt; -Element (Netzwerkeinstellungen)](https://msdn.microsoft.com/library/sa91de1e.aspx) auf der MSDN-Website.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problem: Deinstallieren Sie .NET Framework, Version 4 von ASP.NET Web Pages mit Razor-Syntax deaktiviert

> Wenn Sie .NET Framework, Version 4 deinstallieren und anschließend neu installieren, ist ASP.NET Web Pages mit Razor-Syntax deaktiviert. Seiten mit den *cshtml* Erweiterung werden nicht ordnungsgemäß ausgeführt. ASP.NET Web Pages registriert eine Assembly im Stammverzeichnis Computer *"Web.config"* -Datei, und Entfernen von .NET Framework entfernt diese Datei. Installieren .NET Framework installiert eine neue Version der Konfigurationsdatei, aber den Verweis wird nicht für die ASP.NET Web Pages-Assembly hinzufügen.
> 
> **Problemumgehung** installieren Sie nach der Neuinstallation von .NET Framework, ASP.NET Web Pages mit Razor-Syntax. Dadurch wird das folgende Element auf der *"Web.config"* Datei im Stammverzeichnis Computer, i. d. r. an folgendem Speicherort:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problem: URLs ohne Erweiterung.cshtml/.vbhtml Dateien unter IIS 7 oder IIS 7.5 nicht gefunden

> Für IIS 7 oder IIS 7.5 mit einer URL wie die folgende Anforderungen können sich nicht mehr Seiten zu suchen, die *cshtml* oder *vbhtml* Erweiterung:  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Das Problem tritt auf, da die URLs nicht standardmäßig für IIS 7 oder IIS 7.5 aktiviert ist. Das likeliest Szenario ist, dass das Problem nicht angezeigt werden, werden Wenn es sich bei Tests lokal mit IIS Express jedoch auftreten, wenn Sie Ihre Website für eine hosting-Website bereitstellen.
> 
> **Dieses Problem zu umgehen**
> 
> - Wenn Sie die Kontrolle über den Server-Computer verfügen, installieren Sie auf dem Servercomputer in beschriebenen Updates [ein Update ist verfügbar, die ermöglicht, die bestimmte IIS 7.0 oder IIS 7.5-Handler behandelt Anforderungen, deren URLs nicht mit einem Punkt enden](https://support.microsoft.com/kb/980368).
> - Wenn Sie keine Kontrolle über den Server-Computer verfügen (z. B., dass Sie die Bereitstellung für eine hosting-Website), fügen Sie Folgendes auf der Website *"Web.config"* Datei: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problem: Bereitstellen einer Anwendung auf einem Computer, die keine SQL Server Compact installiert

> Anwendungen mit SQL Server Compact-Datenbanken können auf einem Computer ausführen, auf dem SQL Server Compact nicht installiert. Microsoft WebMatrix 1.0 automatisch kopiert diese Binärdateien für Sie und führt die entsprechenden *"Web.config"* Datei Transformationen.
> 
> **Problemumgehung** Wenn müssen Sie diese Dateien zu kopieren, und stellen die *"Web.config"* dateiänderungen manuell, gehen Sie folgendermaßen vor:
> 
> 1. Kopieren Sie die Datenbank-Engine-Assemblys, die *"bin"* Ordner (und Unterordner) der Anwendung auf dem Zielcomputer:  
> 
>    - Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **to** *\Bin*
>    - Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86*
>    - Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>to</strong><em>\Bin\amd64</em>
> 
> 2. Klicken Sie im Stammordner der Website, erstellen oder öffnen Sie eine *"Web.config"* Datei. (In WebMatrix 1.0 wird dieser Dateityp ist verfügbar, wenn Sie auf **alle** in der **wählen Sie einen Dateityp** (Dialogfeld).)
> 3. Fügen Sie das folgende Element als untergeordnetes Element von der `<configuration>` Element (befindet sich nicht in der `<system.web>` Element):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problem: "Datenbank" und "WebGrid" Hilfsprogramme funktionieren nicht in in Visual Basic mit mittlerer Vertrauenswürdigkeit

> Bei Verwendung von Visual Basic (Erstellen von *vbhtml* Dateien), wird die `Database` und `WebGrid` Hilfsprogramme funktioniert nicht, wenn die Anwendung, die Verwendung mit mittlerer Vertrauenswürdigkeit festgelegt ist.
> 
> **Dieses Problem zu umgehen**  
> Wenn Sie Visual Studio 2010 verwenden, können Sie dieses Problem beheben, installieren Sie das Service Pack 1-Version. Bis die endgültige Version der SP1-Version verfügbar ist, können Sie die Beta-Version von SP1 von Herunterladen der [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) Seite im Microsoft Download Center.   
>   
> Dies ist nicht praktisch, oder wenn Sie nicht Visual Studio 2010 verwenden, Sie vorübergehend können legen die Anwendung mit voller Vertrauenswürdigkeit aus.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problem: "ApplicationPart" Ressourcen extern zugänglich sind

> Wenn eine Assembly Objekte, die enthält von abgeleitet ist die `ApplicationPart` Klasse, die Ressourcen-Assembly verfügbar gemacht werden, durch die `ResourceRouteHandler` Klasse. Nehmen wir beispielsweise die folgende URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Diese Anforderung lädt alle Ressourcenzeichenfolgen in der *System.Web.WebPages.Administration.dll* Assembly. Alle eingebetteten Ressourcen (auch diejenigen, die nicht als statische Inhalte bereitgestellt werden sollen) werden heruntergeladen. Wenn das eingebetteten Ressourcen vertrauliche Informationen enthalten, kann dies ein Sicherheitsrisiko darstellen. 
> 
> **Dieses Problem zu umgehen**   
> Wenn Sie erstellen ein **ApplicationPart** Objekt, stellen Sie sicher, dass die eingebetteten Ressourcen, die zugeordnet **ApplicationPart** Assembly Objekts enthalten keine vertraulichen Informationen.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Informationen über Probleme bei der Installation für WebMatrix finden Sie unter [Probleme bei der Installation von WebMatrix](#Known_Issues_Installation) weiter oben in diesem Dokument.


Dieser Abschnitt des Dokuments beschreibt bekannte Probleme bei der WebMatrix-Entwicklungsumgebung.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problem: Änderungen in der Benutzername oder das Kennwort für eine Datenbank-Verbindungszeichenfolge in einer Datei "Web.config" sind nicht im Arbeitsbereich "Datenbanken" übernommen.

> **Dieses Problem zu umgehen**  
> 
> 1. In der *"Web.config"* Datei, in der Verbindungszeichenfolge den Datenbanknamen ändern (z. B. "1" ihm hinzugefügt).
> 2. Speichern Sie die *"Web.config"* Datei.
> 3. Klicken Sie auf **Datenbanken** und aktualisieren.
> 4. Ändern Sie den Datenbanknamen in der Verbindungszeichenfolge in der *"Web.config"* wieder auf den Namen der ursprünglichen Datei.
> 5. Speichern Sie die *"Web.config"* Datei.
> 6. Klicken Sie auf **Datenbanken** und aktualisieren.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problem: Der Ordner erstellt, die von WebMatrix können nicht gelöscht werden

> Wenn WebMatrix mithilfe erhöhter Berechtigungen ausgeführt wird (d. h., Sie bei Schritten mit WebMatrix die **als Administrator ausführen** -Option in Windows), Ordnern, die von WebMatrix erstellt werden können nicht gelöscht werden, mithilfe von Windows Explorer.
> 
> **Dieses Problem zu umgehen**  
> Führen Sie Windows-Explorer mit erweiterten Berechtigungen. Führen Sie folgende Schritte aus:  
> 
> 1. Klicken Sie in Windows, **starten**.
> 2. Geben Sie "Windows-Explorer" der rechten Maustaste auf den Eintrag für **Windows Explorer**.
> 3. Klicken Sie auf **als Administrator ausführen**. Sie können dann den Ordner löschen.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problem: WebMatrix 1.0 kann bestimmte Aufgaben ausführen, die Erhöhung der Rechte erfordern.

> WebMatrix 1.0 ist nicht möglich, bestimmte Aufgaben ausführen, für die Erhöhung der Rechte, z. B. beim Installieren von zusätzlicher Komponenten in den folgenden Situationen erforderlich:
> 
> - Unter Windows Vista oder Windows 7 Sie mit einem Konto, das über keine Administratorrechte angemeldet sind und (User Account Control, UAC) ist deaktiviert.
> - Verwenden Sie Microsoft Windows XP oder Microsoft Windows Server 2003.
> 
> **Dieses Problem zu umgehen**  
> Die meisten Tasks in WebMatrix 1.0 erfordern keine Administratorberechtigungen. Sie können für diejenigen, die auch ausführen, führen Sie den Vorgang als Administrator oder gehen Sie folgendermaßen vor:
> 
> - Unter Windows Vista oder Windows 7 aktiviert.
> - Fügen Sie unter Windows XP die Benutzer der Sicherheitsgruppe "Administratoren" hinzu.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problem: "Website aus Web Gallery" ist deaktiviert.

> Die **Web Gallery-Website** Option ist deaktiviert, wenn der Webplattform-Installer 3.0 nicht installiert ist.
> 
> **Dieses Problem zu umgehen**  
> Installieren der [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problem: Google Chrome besteht keine Verfügbarkeit als eine Option ausführen

> Google Chrome wird nicht angezeigt, in der Liste der Browser unter **ausführen** auf die **Home** Registerkarte.
> 
> **Dieses Problem zu umgehen**  
> Einige Versionen von Google Chrome registrieren nicht selbst ordnungsgemäß mit dem Standardprogramme-Feature in Windows. Dieses Problem zu umgehen, starten Sie Google Chrome, klicken Sie auf die *anpassen und Google Chrome-Steuerelement* Menü klicken Sie auf *Optionen*, und klicken Sie dann auf *stellen Google Chrome Standardbrowser*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problem: Das Dialogfeld "Foreign Key" lässt nicht die Eingabe eines Primärschlüssels

> Die **Foreign Key** Dialogfeld können Sie der Primärschlüsselname geben an der Primärschlüsseltabelle.
> 
> **Dieses Problem zu umgehen**  
> Dies ist beabsichtigt. Sie müssen nicht den Namen des primären Schlüssels auf die Primärschlüsseltabelle eingeben.


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problem: IntelliSense ist nicht verfügbar in WebMatrix für Razor-Syntax, c# oder Visual Basic

> IntelliSense wird für HTML und CSS in WebMatrix unterstützt. Allerdings ist es nicht für andere Sprachen verfügbar. 
> 
> **Dieses Problem zu umgehen**   
> Keine


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problem: IntelliSense für HTML und CSS schlägt Elemente, die nicht angemessen sind.

> HTML mithilfe von IntelliSense für Markup in WebMatrix unterstützt die [XHTML 1.0 Transitional Schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) und CSS verwenden die [CSS 2.1 Schema](http://www.w3.org/TR/CSS2/). Da IntelliSense für diese bestimmte Schemas basiert, können bestimmte Tags, Attribute oder Eigenschaften vorgeschlagen, die nicht für die aktuelle Seite oder Definition geeignet sind. Für HTML können sie auch zu unerwarteten Vorschläge im Inhalt führen, die als falsch formatierte XHTML (z. B., wenn Tags nicht geschlossen werden) interpretiert werden kann. Dieses Problem möglicherweise deutlicher, wenn die Einfügemarke befindet sich in einem unvollständigen Objekttag; In diesem Fall kann IntelliSense vorschlagen neue öffnenden Tags oder andere falsche Vorschläge. 
> 
> **Dieses Problem zu umgehen**   
> HTML stellen Sie sicher, dass Sie in eine wohlgeformte, vollständige XHTML-Seite arbeiten. Für CSS gibt es keine problemumgehung.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problem: IntelliSense wird nicht aufgerufen werden, während der Eingabe

> In einigen Fällen kann IntelliSense nicht aufgerufen werden, wie HTML- oder CSS-im-Editor eingegeben wird. Insbesondere, dies kann passieren, wenn die Einfügemarke an der Stelle direkt neben einem anderen Element oder am Ende einer Datei ist. 
> 
> **Dieses Problem zu umgehen**   
> Stellen Sie sicher, dass Leerraum um die Einfügemarke vorhanden ist und die Einfügemarke nicht am Ende einer Datei vorhanden ist. Sie können IntelliSense auch manuell durch Drücken von STRG + LEERTASTE aufrufen.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problem: Keine Benutzeroberfläche steht für die Deaktivierung von IntelliSense

> WebMatrix 1.0 stellt keine Benutzeroberfläche oder einer Geste zum Deaktivieren von IntelliSense bereit. 
> 
> **Dieses Problem zu umgehen**   
> Starten Sie WebMatrix mithilfe des folgenden Befehls, die einen Switch umfasst, der IntelliSense deaktiviert:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express hat eine eigene Readme-Datei, die unter folgender URL verfügbar ist:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact verfügt über einen eigenen Readme-Datei, die unter folgender URL verfügbar ist:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Weitere Informationen zu Problemen, bei denen Installation von SQL Server Compact als Teil von WebMatrix, finden Sie unter [Probleme bei der Installation von WebMatrix](#Known_Issues_Installation) weiter oben in diesem Dokument.

### <a id="Known_Issues_Installing_Applications"></a>  Installieren von Anwendungen

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problem: Installieren einer Anwendung kann sehr lange dauern, wenn Ordner Eigene Dokumente des Benutzers zu einer Netzwerkfreigabe umgeleitet wird

> **Dieses Problem zu umgehen**  
> Keine Die Anwendung möglicherweise einige Zeit dauern zu installieren, jedoch ordnungsgemäß installiert.


### <a id="Known_Issues_Publishing_Applications"></a>  Veröffentlichen von Anwendungen

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problem: "verlangt, dass die Berechtigungen können nicht abgerufen werden" Fehler beim Veröffentlichen von SQL Compact-Datenbank

> WebMatrix unterstützt das Bereitstellen von unterstützenden Binärdateien für SQL Server Compact auf einem Server mit .NET Framework Version 3.5 mit einer Konfiguration für mittlere Vertrauenswürdigkeit nicht vollständig.
> 
> **Dieses Problem zu umgehen**  
> Die bevorzugte umgangen werden, die .NET Framework 4 auf dem Server zu installieren. Führen Sie alternativ folgende Schritte aus:
> 
> 1. Hinzufügen der folgenden Elemente in der `SecurityClasses` im Abschnitt *Web\_MediumTrust.config* Datei:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Erstellen Sie einen neuen Berechtigungssatz, der der *Web\_MediumTrust.config* -Datei mit den folgenden Berechtigungen:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Übernehmen Sie den Berechtigungssatz zu SQL Server Compact verlegen Sie die folgenden Elemente der *Web\_MediumTrust.config* Datei:
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problem: Katalog und PhpBB Webanwendungen anzeigen einen Fehler "Der Dienst ist nicht verfügbar" nach der Veröffentlichung

> In einigen Fällen verursacht das Veröffentlichen einer Anwendung einen Fehler "der Dienst ist nicht verfügbar" aus.
> 
> **Dieses Problem zu umgehen**  
> Fügen Sie einen umgekehrten Schrägstrich in WebMatrix (\) bis zum Ende des Servernamens in der **Veröffentlichungseinstellungen** Fenster, und klicken Sie dann die Anwendung erneut veröffentlichen.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problem: Moodle Website Layout und die Links sind defekt nach der Veröffentlichung

> Nachdem Sie eine Moodle-Anwendung veröffentlichen, funktioniert die Anwendung möglicherweise nicht ordnungsgemäß.
> 
> **Dieses Problem zu umgehen**  
> In WebMatrix, fügen Sie einen Schrägstrich (/) am Ende der **Standortname** -Feld in der **Veröffentlichungseinstellungen** Fenster, und klicken Sie dann die Anwendung erneut veröffentlichen.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problem: NopCommerce Veröffentlichen eines Datenbankfehlers Clientfehler

> Veröffentlichung NopCommerce schlägt fehl, und gibt einen Fehler wie "Einfügen in die Nop\_Protokolltabelle ist fehlgeschlagen."
> 
> **Dieses Problem zu umgehen**  
> 
> 1. Klicken Sie in WebMatrix auf **ausführen** NopCommerce lokal zu starten.
> 2. Melden Sie sich die Seite "Verwaltung".
> 3. Klicken Sie auf die **System** Menü.
> 4. Klicken Sie auf die **Protokoll** Option.
> 5. Klicken Sie auf die **Protokoll löschen** Schaltfläche.
> 6. Veröffentlichen Sie NopCommerce erneut.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problem: Silverstripe CMS zeigt ein "HTTP 500 PHP FCGI Error", wenn Sie eine veröffentlichte Site herunterladen

> **Dieses Problem zu umgehen**  
> Nachdem Sie auf **Download veröffentlichte Website**, überspringen Sie `silverstripe-cache/manifest_main` in **veröffentlichen Preview**. Diese Datei dient zum Zwischenspeichern und bezieht sich auf jedem Computer.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problem: Subtext zeigt "Serverfehler in '/' Anwendung" Wenn Sie eine veröffentlichte Site herunterladen

> **Dieses Problem zu umgehen**  
> Öffnen Sie die Website *"Web.config"* -Datei und Ersetzen Sie den Benutzer-ID und das Kennwort in der Datenbank-Verbindungszeichenfolge mit den SQL Server-Administrator-Anmeldeinformationen (die Anmeldeinformationen für "sa").
> 
> Alternativ müssen Sie folgendermaßen vorgehen, um das Benutzerkonto zu erteilen, mit dem Sie angemeldet sind `db_owner` Berechtigungen:
> 
> 1. Installieren Sie SQL Server Management Studio mit dem Webplattform-Installer.
> 2. Herstellen einer Verbindung mit der lokalen SQL Server Express-Instanz (standardmäßig `.\SQLEXPRESS`).
> 3. Klicken Sie auf **Datenbanken** &gt; *[LocalSubtextDatabase]* &gt; **Sicherheit** &gt; **Benutzer** &gt; *[LocalSubtextUser*] (Standardeinstellung ist `subtextuser`], mit der rechten Maustaste, und klicken Sie auf **Eigenschaften**.
> 4. Wählen Sie **Db\_Besitzer** im Abschnitt Mitgliedschaft Rolle.


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problem: Standort funktioniert möglicherweise nicht nach dem Veröffentlichen, wenn das Feld "Ziel-URL" nicht mit http:// oder https:// vorangestellt ist

> In der **Veröffentlichungseinstellungen** (Dialogfeld), wenn die Ziel-URL nicht mit beginnt `http://` oder `https://`, der Standort funktioniert möglicherweise nicht nach der Bereitstellung.
> 
> **Dieses Problem zu umgehen**  
> Stellen Sie sicher, dass vor dem Veröffentlichen eines Standorts, die Ziel-URL in die **Veröffentlichungseinstellungen** Dialogfeld beginnt mit `http://` oder `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problem: Veröffentlichen einer MySQL-Datenbank tritt der Fehler "Fehler beim Veröffentlichen der Datenbank. Dies kann geschehen, wenn die Remotedatenbank das Skript ausgeführt werden kann."

> Der Fehler kann eine Reihe von Gründen auftreten. Ein Grund, dass Sie diese Fehlermeldung ist Datenbankskripts enthält eine einfache Anführungszeichen (') und Standard-Zeichensatzes für das Ziel MySQL-Datenbank ist nicht in UTF-8.
> 
> **Dieses Problem zu umgehen**  
> Legen Sie den Standardzeichensatz für die MySQL-Remotedatenbank in UTF-8.


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problem: Einige Verknüpfungen sind nicht sichtbar in DotNetNuke nach dem Veröffentlichen oder die Website herunterladen

> Wenn Sie veröffentlichen oder eine DotNetNuke-Website herunterladen, müssen Sie möglicherweise Löschen des Zwischenspeichers aus, um die neuen Links auf der Website angezeigt werden.
> 
> **Dieses Problem zu umgehen**
> 
> 1. Melden Sie sich als "Host".
> 2. Wechseln Sie zur das Hostmenü, und wählen Sie **Hosteinstellungen**.
> 3. Führen Sie einen Bildlauf nach unten, und klicken Sie unter **Erweiterte Einstellungen**, erweitern Sie **Leistungseinstellungen**.
> 4. Klicken Sie auf die **Cache löschen** Link, um Seiten.
> 5. Wechseln Sie zum unteren Rand der Seite, und starten Sie die Anwendung neu.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problem: Einige Links in AtomSite sind fehlerhaft, nachdem Sie eine veröffentlichte Website heruntergeladen haben.

> **Dieses Problem zu umgehen**  
> In der *service.config* Datei *users.config* Datei und alle *XML* Dateien, ersetzen Sie die URL-Zeichenfolge (z. B. `http://myhost.com/atomsite`) mit der lokalen Version (z. B. `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problem: MySQL-basierten Anwendungen wie WordPress nicht veröffentlichen und einen Fehler gemeldet

> WebMatrix installiert standardmäßig MySQL mit dem UTF-8-Zeichensatz. Wenn Sie MySQL selbst installieren und der Zeichensatz nicht UTF-8 ist (z. B. befindet er sich Latin1), der Veröffentlichungsprozess für Datenbanken möglicherweise fehl.
> 
> **Dieses Problem zu umgehen**
> 
> 1. Ändern Sie den Zeichensatz für MySQL in UTF-8. (Weitere Informationen finden Sie unter [Server Zeichensatz und Sortierreihenfolge](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) der MySQL-Website.)
> 2. Installieren Sie die Anwendung erneut.
> 3. Veröffentlichen der Anwendung an.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problem: "Download Standort veröffentlicht" für Anwendungen, die browserbasierten eingerichtet haben, ein Fehler auftritt

> Einige Anwendungen (z. B. Kentico CMS), müssen Sie sie im Browser zu starten, damit Setup nach der Installation, z. B. das Erstellen einer Datenbank ausführen. Wenn Sie eine Anwendung wie folgt ohne Abschluss des Setups browserbasierte veröffentlichen, schlägt fehl, bei dem Versuch, die am selben Standort von einem Remoteserver herunterladen.
> 
> **Dieses Problem zu umgehen**  
> Abschließen der Einrichtung browserbasierte vor dem Veröffentlichen der Website.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problem: "Download Standort veröffentlicht" eines Datenbankfehlers für DotNetNuke und Kooboo CMS Clientfehler

> Wenn Sie versuchen, eine Anwendung von einem Server herunterladen und Sie über Administratoranmeldeinformationen, in der Datenbank-Verbindungszeichenfolge in verfügen der **Veröffentlichungseinstellungen** Dialogfeld sehen Sie möglicherweise folgenden Fehler im Protokoll veröffentlichen:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Dieses Problem zu umgehen**  
> Wenn praktikabel, veröffentlichen die Website (oder dessen Veröffentlichung) mit nicht-Administrator-Anmeldeinformationen für die Datenbank.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Weitere Informationen

Weitere Informationen zu WebMatrix 1.0 finden Sie unter den folgenden Websites:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Alle Rechte vorbehalten. [Nutzungsbedingungen](https://msdn.microsoft.cos/cc300389.aspx).
