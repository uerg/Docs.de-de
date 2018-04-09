---
uid: web-pages/readme/beta3
title: Web Matrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei | Microsoft Docs
author: rick-anderson
description: WebMatrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5ef7a6f44758cf94fc19d6fbab3cc4b7bce8e8e5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>WebMatrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei
====================
> WebMatrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei

9 November 2010

## <a name="contents"></a>Inhalt

- [Übersicht](#Overview)
- [Installation](#Installation_Notes)
- [Neue Features, Änderungen und bekannte Probleme in der Beta 3-Version](#Known_Issues)

    - [Probleme bei der Installation von WebMatrix](#Known_Issues_Installation)
    - [ASP.NET-Webseiten 2](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Installieren von Anwendungen](#Known_Issues_Installing_Applications)
    - [Veröffentlichen von Anwendungen](#Known_Issues_Publishing_Applications)
    - [Andere Probleme](#Known_Issues_Other_Issues)
- [Weitere Informationen](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Übersicht

> Betaversion von Microsoft WebMatrix ist ein kostenloses Web Development Stapel, der in Minuten installiert. Einen Webserver integriert mit der Datenbank und die Programmierung Frameworks So erstellen eine einzige, integrierte Benutzeroberfläche. Sie können WebMatrix Beta verwenden, um die Möglichkeit zu optimieren, die Sie schreiben, testen und Ihre eigenen ASP.NET- oder PHP-Website veröffentlichen, oder Sie können WebMatrix Beta verwenden, zum Starten einer neuen Website mithilfe beliebte Open Source-Anwendungen z. B. DotNetNuke, Umbraco, WordPress oder Joomla. Betaversion von WebMatrix verwendet den gleichen leistungsfähigen Webserver, Datenbank-Engine und frameworkumgebung, die Ihre Website im Internet ausführen werden, gestaltet sich der Übergang von der Entwicklung bis hin zur Produktion reibungs- und nahtlos.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Installation

> Verwenden Sie für die Installation von WebMatrix Beta 3 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Nachdem Sie den Webplattform-Installer installiert haben, können Sie es zur Installation von WebMatrix Beta 3 verwenden.
> 
> Wenn Sie während der Installation Probleme auftreten, finden Sie in [Behandlung von Problemen mit Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Anweisungen zum Veröffentlichen von Anwendungen

> Finden Sie unter [schrittweise Anleitungen zum Veröffentlichen von Anwendungen](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Neue Funktionen, die Änderungen, AndKnown Probleme

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>WebMatrix Beta 3-Installation

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problem: WebMatrix Beta 3 ist nur verfügbar für Plattformen, die Microsoft .NET Framework 4 zu unterstützen

> .NET Framework, Version 4 ist erforderlich für die Betaversion von WebMatrix. In bestimmten Fällen kann können der WebMatrix-Beta-Installer Sie versuchen, auf einer anderen Plattform zu installieren, die nicht Teil der unterstützte Konfiguration ist. Insbesondere Windows Vista ohne das Update für SP1 können Sie die Installation von WebMatrix Beta zu starten, aber die .NET Framework 4-Komponente fehl, und blockieren die Installation.
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


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problem: WebMatrix Beta 3 kann nicht installiert werden, wenn Microsoft Visual Studio 2008 ohne Microsoft Visual Studio 2008 SP1 installiert ist

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

Dieser Abschnitt des Dokuments Beschreibt neue Funktionen, Änderungen und bekannte Probleme bei der Beta 3-Version von ASP.NET Web Pages mit Razor-Syntax.

- [Neue Funktionen](#NewFeatures)
- [Änderungen](#Changes)
- [Probleme](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Neue Funktionen in der Beta 3 für ASP.NET Web Pages mit Razor-Syntax

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Neu: "Html.Raw"-Methode nicht codierte Markup rendert

> Die neue `Html.Raw` -Methode ermöglicht es Ihnen das Rendern von HTML-Markup als Markup und codierte Ausgabe rendern. (Standardmäßig codiert ASP.NET Razor Zeichenfolgen vor dem Rendern sie.) Die Syntax lautet:
> 
> `Html.Raw(value)`
> 
> Das folgende Beispiel veranschaulicht die Verwendung von `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Änderungen in der Beta 3 für ASP.NET Web Pages mit Razor-Syntax

#### <a name="change-hrefattribute-method-removed"></a>Änderung: "HrefAttribute"-Methode entfernt

> Die `HrefAttribute` Methode der `WebPage` Klasse wurde entfernt. Dieses Hilfsprogramm wurde verwendet, um unsichere Zeichen in URLs zu codieren. Es ist nicht mehr erforderlich, da ASP.NET Razor automatisch Zeichenfolgen codiert. (Verwenden Sie die neue `Html.Raw` aufzurufende Methode nicht codierte Zeichenfolgen zu rendern.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Änderung: Syntax für deklarative "@helper" Hilfsprogramme geändert

> In der Beta 3-Version ASP.NET ändert wie diese Hilfsprogramme analysiert, die erstellt werden, mithilfe der `@helper` Syntax. Im Wesentlichen die `@helper` Syntax wird als Codeblock statt als Block des Markups, die Code enthalten, kann jetzt analysiert. Daher Code in das Hilfsprogramm muss nicht in eingeschlossen werden `@{ }` blockiert. Umgekehrt Markup in das Hilfsprogramm muss explizit eingefügt, in HTML-Elemente oder in ASP.NET Razor `<text></text>` Tags.
> 
> Beispielsweise die folgenden `@helper` Syntax funktioniert in der Beta 3-Version:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> In der Beta 3-Version muss dieses Hilfsprogramm geändert werden, um wie im folgenden Beispiel aussehen:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Beachten Sie, dass die `@{ }` Zeichen um den ursprünglichen Code in das Hilfsprogramm wird nicht mehr verwendet. Dies ist, da der Inhalt der Hilfsprogramme standardmäßig als Codeblock behandelt werden. Das Hilfsobjekt rendert Markup, der mit dem öffnenden beginnt `<a>` Tag. Wenn das Hilfsprogramm gerendert werden muss, nur-Text oder Tags, die kein schließendes Tag (z. B. `<meta>` Tags), muss der zu rendernden Inhalt `<text></text>` Tags.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Change: "WebPageContext.HttpContext" removed

> Die `WebPageContext.HttpContext` Eigenschaft entfernt wurde. Verwenden Sie stattdessen `HttpContext.Current`. (Die `WebPageContext.HttpContext` -Eigenschaft umschlossen dies einfach.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Änderung: "Facebook"-Hilfsprogramm zum neuen Paket verschoben

> Die `Facebook` Hilfsprogramm wurde in der *Facebook.Helper* Klassenbibliothek mit dem `Facebook` -Hilfsobjekt und zusätzliche Funktionen. Sie müssen Installieren dieser Bibliothek als separates Paket, wie unter "Installieren von Hilfsprogrammen Paket-Manager" im Lernprogramm [erste Schritte mit ASP.NET Webseiten](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Ändern: Wechselt Mitgliedschaft, Rollen und Sicherheit der Typen zur neuen assembly

> Die folgenden Typen wurden verschoben, um die `WebMatrix.WebData` Assembly:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Änderung: "TagBuilder"-Klasse System.Web.WebPages.dll Assembly verschoben

> Die `TagBuilde` R-Klasse auf die Assembly System.Web.WebPages.dll verschoben wurde. Bisher war dies in einer Assembly, die Teil von ASP.NET MVC waren. Diese Änderung bedeutet, dass Sie nicht ASP.NET MVC zu installieren, damit Sie verwenden die `TagBuilder` Klasse.
> 
> Die Klasse ist jedoch weiterhin in der `System.Web.Mvc` Namespace. Zum Verwenden der `TagBuilder` -Klasse (z. B. in einem benutzerdefinierten ASP.NET Razor-Hilfsprogramm), müssen Sie den Namespace verweisen (z. B. durch Hinzufügen von `@using System.Web.Mvc` für Ihren Code).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>: Änderungsanforderung Überprüfung Syntax geändert; Entfernt "Validation"-Klasse

> In der Beta 3-Version zum Deaktivieren der Validierung für ein einzelnes Feld oder eine Gruppe von Feldern, Sie erreichen die `Validation.Exclude` -Methode auf und übergibt den oder die Namen der Felder, die aus der Überprüfung ausgeschlossen. Eine neue Syntax ist verfügbar in der Beta 3-Version für die Überprüfung umgehen. Die `Validation` in der Beta 3 verwendete Methode entfernt wurde.
> 
> > [!NOTE]
> > Wenn Sie die anforderungsüberprüfung, nicht deaktivieren, wenn Benutzer versuchen, die HTML-Markup (z. B. mithilfe eines rich-Text-Editors auf einer Seite) hochladen, meldet die Website eine Fehlermeldung wie *ein potenziell gefährlicher Request.Form-Wert wurde erkannt, vom Client*und die Benutzereingabe wird nicht akzeptiert. Wenn Anforderungsvalidierung Sie deaktivieren, müssen Sie manuell überprüfen von Benutzereingaben, um sicherzustellen, dass nicht potenziell gefährlichen Markup enthalten oder Skript verwenden, etwa die [Microsoft Anti-Cross-Site Scripting Bibliothek v4. 0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Um automatische Anforderungsvalidierung deaktivieren, rufen die `Request.Unvalidated` -Methode, und übergeben sie den Namen des Felds oder andere Post-Objekt, das Anforderungsvalidierung für umgangen werden soll. Sie können diese Methode verwenden, umgehen die Validierung für alle Elemente in der `Form`, `QueryString`, `Cookies`, und `ServerVariables` Sammlungen. Den folgenden Beispielen wird veranschaulicht, wie Sie die `Unvalidated` Methode:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Bekannte Probleme bei der ASP.NET Web Pages mit Razor-Syntax

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problem: Unerwartetes Verhalten bei Verwendung einer benutzerdefinierten Tabelle für die Mitgliedschaft

> Um der Mitgliedschaftsanbieter für eine ASP.NET Razor-Website zu initialisieren, rufen Sie die `WebSecurity.InitializeDatabaseConnection` Methode. (In WebMatrix die Vorlage Starter Site enthält einen Aufruf dieser Methode in der  *\_AppStart.cshtml* Datei.) Wenn die `autoCreateTables` Parameter dieser Methode wird festgelegt auf "true" (, es ist standardmäßig festgelegt in der Vorlage Starter Site "true"), und wenn der Name einer nicht erkannten an die Methode (der zweite Parameter) übergeben wird, wird die Methode keinen Fehler ausgelöst. Stattdessen wird automatisch der Tabelle erstellt.
> 
> Dies kann ein Problem sein, wenn Sie beabsichtigen, verwenden eine benutzerdefinierte Tabelle, für die Mitgliedschaft jedoch übergeben den Namen der falschen Tabelle die `WebSecurity.InitializeDatabaseConnection` Methode. Da die Methode nicht standardmäßig einen Fehler ausgelöst wird, wenn die Tabelle, die Sie angeben, nicht vorhanden ist und sie stattdessen eine neue Tabelle erstellt, kann die Anwendung angezeigt, zu funktionieren. Anwendungscode, der für die benutzerdefinierte Tabelle (und auf Felder) nutzt kann jedoch letztendlich mit unerwarteten Fehlern fehlschlagen.
> 
> **Dieses Problem zu umgehen**  
> Stellen Sie sicher, dass der Name im übergeben der `InitializeDatabaseConnection` Methode entspricht das Benutzerprofil in der Mitgliedschaftsdatenbank-Tabelle ab, oder stellen Sie sicher, dass die `autoCreateTables` Parameter auf "false" festgelegt ist.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problem: "Fehler beim Generieren einer Benutzerinstanz von SQLServer"-Fehler

> Wenn eine Webanwendung mit WebMatrix SQL Server Express verwendet und IIS 7.5 auf Windows 7 oder Windows Server 2008 R2 ausgeführt wird, wird möglicherweise einen Fehler angezeigt, der angibt, dass SQL Server nicht lokalen Anwendungspfad des Benutzers zur Laufzeit abrufen.
> 
> **Problemumgehung** stellen Sie sicher, dass das Windows-Konto, das die Anwendung unter (i. d. r. NETWORK SERVICE) ausgeführt wird, wie z. B. Lese-/Schreibberechtigungen für den Stammordner der Anwendung und für Unterordner hat *App\_Daten*. Ausführlichere Informationen finden Sie im Knowledge Base-Artikel [bei Problemen mit SQL Server Express Benutzerinstanziierung und ASP.net Web-Anwendungsprojekten](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problem: In Visual Studio-Namespaces für benutzerdefinierte Assemblys (DLLs) nicht automatisch importiert

> Wenn Sie benutzerdefinierte Assemblys in einem Projekt in Visual Studio verwenden, werden die Namespaces deklariert, die in diesen Assemblys zur Entwurfszeit nicht automatisch importiert. Folglich Verweise auf benutzerdefinierte Typen möglicherweise nicht zur Entwurfszeit erkannt werden und werden als nicht erkannt, in Visual Studio (mit einem "Wellenlinie") gekennzeichnet. Dieses Problem tritt nur zur Entwurfszeit in Visual Studio; die Anwendung selbst wird ordnungsgemäß ausgeführt.
> 
> **Dieses Problem zu umgehen**  
> Einschließen einer `using` Anweisung (`imports` in Visual Basic), die auf die Entitäten, die zur Entwurfszeit nicht erkannt werden.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problem: Visual Studio IntelliSense-Projekt Vorlagen und nur in ASP.NET MVC 3-Version verfügbar

> Installieren ASP.NET Web Pages installiert nicht auch Tools für Visual Studio z. B. IntelliSense und Projekt Vorlagen für ASP.NET Web Pages-Anwendungen.
> 
> **Problemumgehung** um IntelliSense und Projekt Vorlagen für ASP.NET Web Pages-Anwendungen in Visual Studio verwenden, installieren Sie ASP.NET MVC 3 RC entweder über den Webplattform-Installer oder [eigenständiges Installationsprogramm](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problem: "&lt;Helper&gt; Klasse wurde nicht gefunden" Fehler

> Nach dem upgrade auf Beta 3 wird möglicherweise eines Fehlers, die eine Hilfsklasse (z. B. die `Facebook` Klasse) kann nicht gefunden werden. Beginnend in Beta 2 und 3 Beta, wurden Hilfen in Paketen verschoben, die Sie explizit installieren müssen. Vorhandene Websites nicht aktualisiert, um diese Pakete enthalten; Dazu gehören Standorte in der *\My Documents\IISExpress* oder *\My Documents\My Websites* Ordner. Insbesondere, sehen Sie diesen Fehler bei der Verwendung der Standardwebsite in *Meine Standorte* (WebSite1), enthält einen Verweis auf die `Twitter` Helper.
> 
> **Dieses Problem zu umgehen**  
> Kommentieren Sie Aufrufe an alle Hilfsprogramme am Standort, und führen Sie die  *\_Admin* Seite, und installieren Sie das Paket oder Pakete, die die Hilfsprogramme enthalten, die Sie verwenden möchten. Nachdem Sie das Paket installiert haben, können Sie die auskommentierung der Zeilen, die Hilfsmethoden verweisen.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problem: Bereitstellen von Beta 3 ASP.NET Razor-Assemblys in den Ordner "Bin" funktioniert möglicherweise nicht auf das Hosten von Websites

> Wenn Sie eine ASP.NET Web Pages-Website für eine hosting-Website bereitstellen, und wenn Sie die ASP.NET Razor Beta 3-Assemblys für die Website bereit *"bin"* Ordner, treten möglicherweise Fehler auf, einschließlich der folgenden:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Dies kann geschehen, wenn der Hostinganbieter die ASP.NET Web Pages Beta 1-Assemblys in der Server globale Cache (GAC) installiert hat. Assemblys im GAC erhalten Vorrang gegenüber Assemblys, die lokal im installiert die *"bin"* Ordner.
> 
> **Problemumgehung** wenden Sie sich an Ihrem Hostinganbieter, um sicherzustellen, dass der Fehler wird angezeigt, werden aufgrund eines Konflikts zwischen verschiedenen Versionen des Anbieters werden die Assemblys und Ihren Instanzen-. Wenn dies der Fall ist, fordern Sie, dass es sich bei der Hostinganbieter die Assemblys im GAC der Server aktualisieren.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problem: Lesen von Feeds oder andere externen Daten über einen Proxyserver

> Wenn der Server mit dem Standort hinter einem Proxyserver ist, müssen Sie möglicherweise konfigurieren Proxyinformationen in die *"Web.config"* Datei, um in der Lage, Informationen zu lesen, die sich außerhalb Ihrer Website stammen. Angenommen, Sie verwenden die `ReCaptcha` Hilfsprogramm, das Hilfsprogramm kommuniziert mit dem ReCAPTCHA-Dienst, aber vom Proxyserver blockiert werden. Auf ähnliche Weise möglicherweise Feeds, die in ASP.NET Web Pages, z. B. den Feed vom Paketmanager verwendet verwendet werden die Proxykonfiguration.
> 
> Wenn Sie Probleme beim Arbeiten mit einem externen Dienst oder Arbeiten mit dem feed-Paket auf, Stammverzeichnis Ihrer Anwendung die folgenden Elemente abgelegt *"Web.config"* Datei:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Weitere Informationen zum Konfigurieren eines Proxyservers finden Sie unter [ &lt;Proxy&gt; -Element (Netzwerkeinstellungen)](https://msdn.microsoft.com/library/sa91de1e.aspx) auf der MSDN-Website.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problem: "Die Microsoft.Web.Infrastructure.dll kann nicht geladen werden" angezeigt

> Wenn Sie zuvor die Beta 1-Version von ASP.NET Web Pages mit Razor-Syntax installiert und dann die Beta 3-Version installieren, werden alle entsprechenden Assemblys im GAC außer installiert *Microsoft.Web.Infrastructure.dll*. Daher bei der Ausführung des ASP.NET Razor Seiten Fehlermeldung angezeigt wird, der angibt, *Microsoft.Web.Infrastructure.dll* konnte nicht geladen werden.
> 
> Dieses Problem tritt nicht auf, wenn Sie über die Beta 3-Version auf einem "sauberen" Computer geladen werden.
> 
> **Dieses Problem zu umgehen**  
> Deinstallieren Sie ASP.NET Web Pages, in der Systemsteuerung. Installieren Sie die Beta 3-Version erneut.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problem: Deinstallieren Sie .NET Framework, Version 4 von ASP.NET Web Pages mit Razor-Syntax deaktiviert

> Wenn Sie .NET Framework, Version 4 deinstallieren und anschließend neu installieren, ist ASP.NET Web Pages mit Razor-Syntax deaktiviert. Seiten mit den *cshtml* Erweiterung werden nicht ordnungsgemäß ausgeführt. ASP.NET Web Pages registriert eine Assembly im Stammverzeichnis Computer *"Web.config"* -Datei, und Entfernen von .NET Framework entfernt diese Datei. Installieren .NET Framework installiert eine neue Version der Konfigurationsdatei, aber den Verweis wird nicht für die ASP.NET Web Pages-Assembly hinzufügen.
> 
> **Problemumgehung** installieren Sie nach der Neuinstallation von .NET Framework, ASP.NET Web Pages mit Razor-Syntax. Dadurch wird das folgende Element auf der *"Web.config"* Datei im Stammverzeichnis Computer, i. d. r. an folgendem Speicherort:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problem: Treten Fehler auf Anwendungen, die zuvor mit ASP.NET-Assemblys im Ordner "Bin" bereitgestellt.

> Während der Bereitstellung von Kopien der ASP.NET Web Pages-Assemblys (z. B. *Microsoft.WebPages.dll*), die *"bin"* Ordner der Website auf dem Server. (Dies kann während der Bereitstellung automatisch Ursache sein oder weil der Entwickler explizit die Assemblys kopiert.) Wenn die Beta 3-Version installiert ist, Fehler erfolgt jedoch, wie z. B. Fehler, die bestimmte Typen nicht gefunden werden können. Dies tritt auf, da eine Reihe von ASP.NET Web Pages-Datentypen in verschiedenen Namespaces für Beta 3-Version verschoben wurden.
> 
> **Dieses Problem zu umgehen**   
> Deaktivieren der *"bin"* Ordner der bereitgestellten Anwendung anzeigen, kopieren Sie den neuen Assemblys in den Ordner (oder erneute Bereitstellung der Anwendung), und klicken Sie dann die Anwendung neu starten.


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
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problem: Verwenden von Webanwendungsprojekt oder ASP.NET MVC und ASP.NET Web Pages in derselben Anwendung

> Wenn Sie ASP.NET Web Pages in einem Webanwendungsprojekt oder ASP.NET MVC-Anwendung verwendet haben, wird möglicherweise einen Fehler, *WebPageHttpApplication* kann nicht gefunden werden.
> 
> **Dieses Problem zu umgehen**  
> Wenn Sie diesen Fehler erhalten, ändern Sie die Basisklasse, von der die Anwendung abgeleitet wird. In der *"Global.asax"* file, ändern Sie die folgende Zeile:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> folgendermaßen:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Dies kehrt faktisch eine Änderung, die eingeführt wurde für die Beta 1-Version von ASP.NET Web Pages mit Razor-Syntax.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problem: Bereitstellen einer Anwendung auf einem Computer, die keine SQL Server Compact installiert

> Anwendungen mit SQL Server Compact-Datenbanken können auf einem Computer ausführen, auf dem SQL Server Compact nicht installiert. Microsoft WebMatrix Beta 3 automatisch kopiert diese Binärdateien für Sie und führt die entsprechenden *"Web.config"* Datei Transformationen.
> 
> **Problemumgehung** Wenn müssen Sie diese Dateien zu kopieren, und stellen die *"Web.config"* dateiänderungen manuell, gehen Sie folgendermaßen vor:
> 
> 1. Kopieren Sie die Datenbank-Engine-Assemblys, die *"bin"* Ordner (und Unterordner) der Anwendung auf dem Zielcomputer: 
> 
>     - Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*
>     - Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **to** *\Bin\x86*
>     - Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **to** *\Bin\amd64*
> 2. Klicken Sie im Stammordner der Website, erstellen oder öffnen Sie eine *"Web.config"* Datei. (In WebMatrix Beta 3 dieser Dateityp ist verfügbar, wenn Sie auf **alle** in der **wählen Sie einen Dateityp** (Dialogfeld).)
> 3. Fügen Sie das folgende Element als untergeordnetes Element von der **&lt;Konfiguration&gt;** Element (befindet sich nicht in der **&lt;system.web&gt;** Element):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problem: Datenbank- und WebGrid Hilfsprogramme funktionieren nicht in in Visual Basic mit mittlerer Vertrauenswürdigkeit

> Bei Verwendung von Visual Basic (Erstellen von *vbhtml* Dateien), wird die `Database` und `WebGrid` Hilfsprogramme funktioniert nicht, wenn die Anwendung, die Verwendung mit mittlerer Vertrauenswürdigkeit festgelegt ist.
> 
> **Dieses Problem zu umgehen**  
> Legen Sie vorübergehend die Anwendung mit voller Vertrauenswürdigkeit aus.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problem: Die Eigenschaft "Encrypt" wird nicht erkannt.

> SQL Server Compact 4.0 nicht erkennt die `Encrypt` Eigenschaft von der `SqlCeConnection` Klasse. Sie sollten diese Eigenschaft nicht verwenden, zum Verschlüsseln von Datenbankdateien. Die `Encrypt` Eigenschaft wurde in SQLServer Compact 3.5-Version als veraltet markiert und nur für Abwärtskompatibilität beibehalten wurde. 
> 
> **Dieses Problem zu umgehen**  
> Verwenden der `Encryption Mode` Eigenschaft von der `SqlCeConnection` Klasse, um SQL Server Compact 4.0-Datenbankdateien zu verschlüsseln. Im folgende Beispiel wird gezeigt, wie zum Erstellen einer verschlüsselten SQL Server Compact 4.0-Datenbank, mit der `Encryption Mode` Eigenschaft:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Um den Verschlüsselungsmodus einer vorhandenen SQL Server Compact 4.0-Datenbank zu ändern, führen Sie folgende Schritte aus:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Um eine nicht verschlüsselte SQL Server Compact 4.0-Datenbank zu verschlüsseln, führen Sie folgende Schritte aus:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problem: Der Microsoft Visual C++ 2008-Laufzeitbibliotheken sind erforderlich

> Die systemeigene DLLs von SQL Server Compact 4.0 benötigen, die Microsoft Visual C++ 2008-Laufzeitbibliotheken (x 86, IA64,- und x 64), Servicepack 1.
> 
> **Dieses Problem zu umgehen**  
> Installieren Sie .NET Framework 3.5 SP1. Dies wird auch das Visual C++ 2008-Runtime-Bibliotheken SP1 installiert. Sie können die Bibliotheken von folgendem Speicherort herunterladen:   
>   
> [ATL-Sicherheitsupdate für Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Beachten Sie diese Installation von .NET Framework 2.0, 3.0, oder 4 ist *nicht* Visual C++ 2008-Runtime-Bibliotheken SP1 installieren.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problem: Wenn SQL Server Compact vor der Installation von .NET Framework auf dem Computer installiert, ist der invariante Anbietername nicht in der .NET Framework-Datei "Machine.config" registriert

> SQL Server Compact kann auf einem Computer installiert werden, die nicht .NET Framework installiert werden, da SQL Server Compact .NET Framework erforderlich sind. Wenn weder .NET Framework Version 3.5 noch 4 installiert ist, bevor Sie SQL Server Compact installieren, die SQL Server Compact-Setup registriert keine der invariante Name des Anbieters in der *"Machine.config"* Datei. Jede Anwendung, auf dem SQL Server Compact Eintrag in beruht, der *"Machine.config"* Datei schlägt fehl. Der invariante Name des Registrierungseintrags in *"Machine.config"* sieht wie im folgende Beispiel:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Dieses Problem zu umgehen**  
> Deinstallieren Sie SQL Server Compact 4.0 CTP1 an. Herunterladen Sie und installieren Sie die Vollversion von .NET Framework von folgendem Speicherort:
> 
> [Microsoft .NET Framework 3.5 Service Pack 1 (gesamtes Paket)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Microsoft .NET Framework 4.0-Version (vollständiges Paket)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Neuinstallieren von [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Installieren von Anwendungen

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problem: Installieren einer Anwendung kann sehr lange dauern, wenn Ordner Eigene Dokumente des Benutzers zu einer Netzwerkfreigabe umgeleitet wird

> **Dieses Problem zu umgehen**  
> Keine Die Anwendung möglicherweise einige Zeit dauern zu installieren, jedoch ordnungsgemäß installiert.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Veröffentlichen von Anwendungen

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


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Andere Probleme

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problem: Suchfilter/funktioniert nicht in Berichten für Group By: Problemtyp

> Beim Ausführen eines Berichts für einen Standort bei der Eingabe von Text in der *Filter durch URL* Feld, und klicken Sie auf *Suche*, geschieht nichts. Dies ist, da dieses Steuerelement nicht funktionsfähig ist die *Group By* Zustand des Berichts wird festgelegt, um *Problemtyp*, dies ist die Standardeinstellung.
> 
> **Problemumgehung** In der *Group By* Registerkarte des Menübands, klicken Sie auf *URL* um die Einträge durch ihre Quell-URL zu gruppieren. Das Textfeld und einer Schaltfläche zum Filtern der Einträge werden in diesem Zustand funktionsfähig.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problem: WCF-Anwendungen nicht mit IIS Express ausgeführt werden soll.

> Navigieren zu einer WCF-Anwendung führt zu einem Fehler wie den folgenden Ausdruck:
> 
> *Konnte nicht geladen werden, Datei oder Assembly "" Microsoft.Web.Administration ", Version = 7.0.0.0, Culture = Neutral, PublicKeyToken = 31bf3856ad364e35" oder eine ihrer Abhängigkeiten. Das System konnte die angegebene Datei nicht finden.*
> 
> Dies tritt auf, da IIS Express-Betaversion WCF standardmäßig nicht unterstützt.
> 
> **Problemumgehung** eine der folgenden problemumgehungen verwenden (problemumgehung #2 erfordert Microsoft Windows Vista oder höher):
> 
> 
> 1. Kopieren der *Microsoft.Web.dll* und *Microsoft.Web.Administration.dll* Assemblys aus dem WebMatrix-Installationsverzeichnis in die *"bin"* Verzeichnis WCF die Anwendung. Standardmäßig WebMatrix installiert ist, der *Microsoft WebMatrix* Unterordner des Systems *Programmdateien* Ordner.
> 2. Unter Microsoft Windows Vista oder höher, erstellen Sie eine "Symlink" für die Assemblys in der *"bin"* Verzeichnis mit den folgenden Befehlen. (Dieser Ansatz hat den Vorteil, dass er eine Kopie der Assemblys keine erstellt wird.)
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Installieren Sie die zwei Assemblys im GAC. Führen Sie in einem Eingabeaufforderungsfenster mit erhöhten Rechten die folgenden Befehle ein:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problem: WebMatrix Beta 3 kann bestimmte Aufgaben ausführen, die Erhöhung der Rechte erfordern.

> WebMatrix Beta 3 kann bestimmte Aufgaben ausführen, für die Erhöhung der Rechte, z. B. beim Installieren von zusätzlicher Komponenten in den folgenden Situationen erforderlich:
> 
> - Unter Windows Vista oder Windows 7 Sie mit einem Konto, das über keine Administratorrechte angemeldet sind und (User Account Control, UAC) ist deaktiviert.
> - Verwenden Sie Microsoft Windows XP oder Microsoft Windows Server 2003.
> 
> **Dieses Problem zu umgehen**  
> Die meisten Tasks in WebMatrix Beta 3 sind keine Administratorrechte erforderlich. Sie können für diejenigen, die auch ausführen, führen Sie den Vorgang als Administrator oder gehen Sie folgendermaßen vor:
> 
> - Unter Windows Vista oder Windows 7 aktiviert.
> - Fügen Sie unter Windows XP die Benutzer der Sicherheitsgruppe "Administratoren" hinzu.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problem: "Website aus Web Gallery" ist deaktiviert.

> Die **Web Gallery-Website** Option ist deaktiviert, wenn der Webplattform-Installer 3.0 nicht installiert ist.
> 
> **Dieses Problem zu umgehen**  
> Installieren der [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problem: Unter Windows Server 2003, IIS Express nicht für einen Benutzer ohne Administratorrechte gestartet

> Unter Windows Server 2003 beim Starten einer Seite oder starten IIS Express wird IIS Express nicht gestartet. Für Webseiten wird eine Fehlermeldung angezeigt, der angibt, dass die Anwendung durch einen Benutzer ohne Administratorrechte gestartet wurde.
> 
> **Dieses Problem zu umgehen**  
> Starten Sie WebMatrix Beta 3 als Administrator an. Weitere Informationen finden Sie unter den folgenden Knowledge Base-Artikel:  
>   
> [Eine Anwendung, die von einem Benutzer ohne Administratorrechte gestartet wird, kann nicht für den HTTP-Datenverkehr des Computers Lauschen auf dem die Anwendung in Windows Vista, Windows Server 2003 oder Windows XP ausgeführt wird.](https://support.microsoft.com/kb/939786)


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


#### <a name="issue-the-relationships-button-is-disabled"></a>Problem: Die Schaltfläche "Beziehungen" ist deaktiviert.

> Die **Beziehungen** Schaltfläche unter der **Tabelle** Registerkarte der **Datenbanken** Arbeitsbereich ist für SQL Server Compact-Datenbanken deaktiviert.
> 
> **Dieses Problem zu umgehen**  
> Keine SQL Server Compact unterstützt keine Beziehungen zwischen Tabellen.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problem: Parametrisierte SQL-Abfragen lösen Ausnahmen aus.

> In SQL Server Compact 4.0, wenn Sie einen Datentyp nicht wie z. B. angeben `SqlDbType` oder `DbType` für Parameter in parametrisierten Abfragen, wird eine Ausnahme ausgelöst, wenn die Abfrage ausgeführt wird.
> 
> **Dieses Problem zu umgehen**  
> Legen Sie den Datentyp für Parameter explizit wie z. B. `SqlDbType` oder `DbType`. Dies ist wichtig, im Fall von BLOB-Datentypen (`image` und `ntext`). Verwenden Sie Code wie folgt:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>Weitere Informationen

Weitere Informationen zu WebMatrix Beta 3 finden Sie unter den folgenden Websites:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. Alle Rechte vorbehalten. [Nutzungsbedingungen](https://msdn.microsoft.cos/cc300389.aspx).
