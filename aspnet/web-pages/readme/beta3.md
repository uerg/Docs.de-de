---
uid: web-pages/readme/beta3
title: WebMatrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei | Microsoft-Dokumentation
author: rick-anderson
description: WebMatrix und ASP.NET Web Pages (Razor) Beta 3-Version-Infodatei
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5aa609bf31499485dc7a1298fa689f3a7cee4774
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363532"
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

> Microsoft WebMatrix Beta ist einen kostenloser Web Development-Stapel, der innerhalb von Minuten installiert. Einen Webserver integriert mit der Datenbank und Programmierframeworks erstellen Sie eine einzige, integrierte Benutzeroberfläche. Können Sie die Möglichkeit zu optimieren, die Sie programmieren, testen und veröffentlichen Ihre eigene Website ASP.NET oder PHP, WebMatrix Beta, oder Sie können WebMatrix Beta verwenden, um eine neue Website mithilfe beliebter Open-Source-apps, z. B. DotNetNuke, Umbraco, WordPress oder Joomla. WebMatrix-Beta verwendet den gleichen leistungsfähigen Webserver, Datenbank-Engine und frameworkumgebung, die Ihre Website im Internet ausgeführt wird, gestaltet sich der Übergang von der Entwicklung zur Produktion reibungs- und nahtlos.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Installation

> Verwenden Sie zum Installieren von WebMatrix Beta 3 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Nachdem Sie den Webplattform-Installer installiert haben, können Sie es zum Installieren von WebMatrix Beta 3 verwenden.
> 
> Wenn Sie während der Installation Probleme auftreten, finden Sie unter [Behandlung von Problemen mit Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Anweisungen zum Veröffentlichen von Anwendungen

> Finden Sie unter [schrittweise Anleitungen zum Veröffentlichen von Anwendungen](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Neue Features, Änderungen AndKnown Probleme

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>WebMatrix-Beta 3-Installation

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problem: WebMatrix Beta 3 steht nur auf Plattformen, die Unterstützung von Microsoft .NET Framework 4

> .NET Framework, Version 4 ist erforderlich, für die Betaversion von WebMatrix. In bestimmten Fällen kann können der WebMatrix-Beta-Installer Sie versuchen, auf einer Plattform zu installieren, die nicht Teil der unterstützte Konfiguration ist. Insbesondere Windows Vista ohne das SP1-Update können Sie die Installation von WebMatrix Beta zu starten, aber die .NET Framework 4-Komponente fehl und die Installation blockieren.
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


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problem: WebMatrix Beta 3 kann nicht installiert werden, wenn Microsoft Visual Studio 2008 ohne Microsoft Visual Studio 2008 SP1 installiert ist

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

Dieser Abschnitt des Dokuments Beschreibt neue Features, Änderungen und bekannte Probleme bei der Beta 3-Version von ASP.NET Web Pages mit Razor-Syntax.

- [Neue features](#NewFeatures)
- [Änderungen](#Changes)
- [Probleme](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Neue Funktionen in Beta 3 für ASP.NET Web Pages mit Razor-Syntax

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Neu: "Von Html.Raw"-Methode nicht codierte Markup rendert

> Die neue `Html.Raw` -Methode können Sie das Rendern von HTML-Markup als Markup und nicht codierte Ausgabe rendern. (Standardmäßig codiert ASP.NET Razor Zeichenfolgen vor dem Rendern sie.) Die Syntax lautet:
> 
> `Html.Raw(value)`
> 
> Das folgende Beispiel veranschaulicht die Verwendung von `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Änderungen in der Betaversion 3 für ASP.NET Web Pages mit Razor-Syntax

#### <a name="change-hrefattribute-method-removed"></a>Änderung: "HrefAttribute"-Methode wurde entfernt

> Die `HrefAttribute` Methode der `WebPage` Klasse wurde entfernt. Dieses Hilfsprogramm wurde verwendet, um unsicheren Zeichen in URLs zu codieren. Es ist nicht mehr erforderlich, da ASP.NET Razor automatisch Zeichenfolgen codiert. (Verwenden Sie die neue `Html.Raw` Methode, um eine nicht codierte Zeichenfolgen zu rendern.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Änderung: Syntax für die deklarative "@helper" Hilfsprogramme geändert

> In der Beta 3-Version ASP.NET ändert wie diese Hilfsprogramme analysiert, die erstellt werden, mit der `@helper` Syntax. Im Wesentlichen die `@helper` Syntax wird als Codeblock anstatt als Block des Markups, die Code enthalten, kann jetzt analysiert. Aus diesem Grund Code in der Hilfe muss nicht eingeschlossen werden `@{ }` Blöcke. Im Gegensatz dazu Markup in der Hilfe muss explizit eingefügt, in HTML-Elemente oder in ASP.NET Razor `<text></text>` Tags.
> 
> Beispielsweise die folgenden `@helper` Syntax funktioniert in der Beta 3-Version:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> In der Beta 3-Version muss dieses Hilfsprogramm geändert werden, um wie im folgenden Beispiel aussehen:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Beachten Sie, dass die `@{ }` Zeichen für den anfänglichen Code in das Hilfsprogramm wird nicht mehr verwendet. Dies ist, da der Inhalt der Hilfsprogramme standardmäßig als Codeblock behandelt werden. Das Hilfsprogramm rendert Markup, das mit dem öffnenden beginnt `<a>` Tag. Wenn das Hilfsprogramm Rendern muss, nur-Text oder Tags, die keinen schließendes Tag enthalten (z. B. `<meta>` Tags), die zu rendernden Inhalt muss sich im `<text></text>` Tags.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Änderung: "WebPageContext.HttpContext" entfernt

> Die `WebPageContext.HttpContext` Eigenschaft entfernt wurde. Verwenden Sie stattdessen `HttpContext.Current`. (Die `WebPageContext.HttpContext` Eigenschaft umschlossen einfach dadurch.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Änderung: Neue Paket verschoben "Facebook"-Hilfsprogramm

> Die `Facebook` Hilfsprogramm wurde in der *Facebook.Helper* -Bibliothek, die umfasst die `Facebook` -Hilfsobjekt und zusätzliche Funktionen. Müssen Sie installieren diese Bibliothek als separates Paket, wie in "Hilfsprogramme mit Paket-Manager installieren" beschrieben im Tutorial [erste Schritte mit ASP.NET Webseiten](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Änderung: Typen Mitgliedschaft, Rollen und Sicherheit verschiebt, in der neuen assembly

> Die folgenden Typen wurden verschoben, um die `WebMatrix.WebData` Assembly:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Änderung: System.Web.WebPages.dll Assembly verschoben "TagBuilder"-Klasse

> Die `TagBuilde` R-Klasse auf die Assembly System.Web.WebPages.dll verschoben wurde. Früher war dies in einer Assembly, die Teil von ASP.NET MVC war. Diese Änderung bedeutet, dass Sie keine ASP.NET MVC zu installieren, um Sie verwenden die `TagBuilder` Klasse.
> 
> Die Klasse ist jedoch nach wie vor in den `System.Web.Mvc` Namespace. Zum Verwenden der `TagBuilder` -Klasse (z. B. in einem benutzerdefinierten ASP.NET Razor-Hilfsprogramm), müssen Sie den Namespace verweisen (z. B. durch Hinzufügen von `@using System.Web.Mvc` an Ihrem Code).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>: Änderungsanforderung Überprüfung Syntax geändert; Entfernt "Validation"-Klasse

> In der Version Beta 3 zum Deaktivieren der Validierung für ein einzelnes Feld oder einen Satz von Feldern, können Sie Aufrufen der `Validation.Exclude` Methode und übergeben den Namen oder die Namen der Felder zum Ausschließen der Validierung. Eine neue Syntax wird in der Beta 3-Version zum Umgehen der Validierung. Die `Validation` Methode für Beta 3 wurde entfernt.
> 
> > [!NOTE]
> > Wenn Sie Request-Überprüfung nicht deaktivieren, wenn Benutzer versuchen, das Hochladen von HTML-Markup (z. B. mithilfe eines rich-Text-Editors auf einer Seite), die Website meldet einen Fehler, z. B. *ein potenziell gefährlicher Request.Form-Wert wurde erkannt, auf dem Client*und die Benutzereingabe wird nicht akzeptiert. Wenn die Anforderungsvalidierung Sie deaktivieren, müssen Sie manuell überprüfen, auf Benutzereingaben, um sicherzustellen, dass nicht potenziell gefährliche Markup enthalten oder Skript, z. B. die [Microsoft Anti-Cross Site Scripting Library v4. 0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Um automatische Request-Überprüfung zu deaktivieren, rufen die `Request.Unvalidated` -Methode, und übergeben sie den Namen des Felds oder andere Post-Objekt, das Sie Anforderungsvalidierung für umgehen möchten. Sie können diese Methode verwenden, umgehen die Überprüfung für alle Elemente in der `Form`, `QueryString`, `Cookies`, und `ServerVariables` Sammlungen. Die folgenden Beispiele zeigen, wie Sie mit der `Unvalidated` Methode:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Bekannte Probleme bei der ASP.NET Web Pages mit Razor-Syntax

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problem: Unerwartetes Verhalten bei Verwendung einer benutzerdefinierten Tabelle für die Mitgliedschaft

> Um der Mitgliedschaftsanbieter für eine ASP.NET Razor-Website zu initialisieren, rufen Sie die `WebSecurity.InitializeDatabaseConnection` Methode. (In WebMatrix die Vorlage Starter Site enthält einen Aufruf dieser Methode in der  *\_AppStart.cshtml* Datei.) Wenn der `autoCreateTables` Parameter dieser Methode wird festgelegt, auf "true" (standardmäßig es nastaven NA hodnotu true in der Vorlage Starter Site), und wenn der Name einer nicht erkannten an die Methode (der zweite Parameter) übergeben wird, wird die Methode keinen Fehler ausgelöst. Stattdessen wird automatisch in der Tabelle erstellt.
> 
> Dies kann ein Problem sein, wenn Sie beabsichtigen, übergeben den falschen Tabellennamen an aber verwenden eine benutzerdefinierte Tabelle, für die Mitgliedschaft der `WebSecurity.InitializeDatabaseConnection` Methode. Da die Methode nicht standardmäßig einen Fehler ausgelöst wird, wenn die Tabelle, die Sie angeben, nicht vorhanden ist und sie stattdessen eine neue Tabelle erstellt, kann die Anwendung zu funktionieren scheinen. Allerdings kann Anwendungscode, der für die benutzerdefinierte Tabelle (und darin enthaltenen Felder) stützt sich schließlich unerwartete Fehlern auftreten.
> 
> **Dieses Problem zu umgehen**  
> Stellen Sie sicher, dass der Name in übergeben die `InitializeDatabaseConnection` Methode entspricht, das Benutzerprofil in der Mitgliedschaftsdatenbank Tabelle, oder stellen Sie sicher, dass, die `autoCreateTables` Parameter auf "false" festgelegt ist.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problem: "Fehler beim Generieren einer Benutzerinstanz von SQLServer"-Fehler

> Wenn eine WebMatrix-Web-Anwendung SQL Server Express verwendet und IIS 7.5 auf Windows 7 oder Windows Server 2008 R2 ausgeführt wird, können Sie ein Fehler angezeigt, der angibt, dass SQL Server des Pfads des Benutzers lokale Anwendung zur Laufzeit abrufen kann.
> 
> **Problemumgehung** stellen sicher, dass das Windows-Konto, das die Anwendung ausgeführt, unter dem Konto (normalerweise Netzwerkdienst wird) Lese-/Schreibberechtigungen für den Stammordner der Anwendung und für Unterordner wie z. B. *App\_Daten*. Ausführlichere Informationen finden Sie im Knowledge Base-Artikel [Probleme mit der SQL Server Express Benutzerinstanziierung und ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problem: In Visual Studio werden die Namespaces für benutzerdefinierte Assemblys (DLLs) nicht automatisch importiert

> Wenn Sie benutzerdefinierte Assemblys in einem Projekt in Visual Studio verwenden, werden die Namespaces deklariert, die in diesen Assemblys zur Entwurfszeit nicht automatisch importiert. Verweise auf benutzerdefinierte Typen wird daher möglicherweise nicht zur Entwurfszeit erkannt werden und als nicht erkannt, in Visual Studio (unter Verwendung einer "Wellenlinie") gekennzeichnet sind. Dieses Problem tritt nur zur Entwurfszeit in Visual Studio; die Anwendung selbst ordnungsgemäß ausgeführt.
> 
> **Dieses Problem zu umgehen**  
> Einschließen einer `using` Anweisung (`imports` in Visual Basic), die auf die Entitäten, die zur Entwurfszeit nicht erkannt werden.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problem: Visual Studio IntelliSense und Projektvorlagen nur in ASP.NET MVC 3-Version verfügbar

> Installieren von ASP.NET Web Pages wird nicht auch Tools für Visual Studio wie IntelliSense und Projektvorlagen für ASP.NET Web Pages-Anwendungen installiert.
> 
> **Problemumgehung** um IntelliSense und Projektvorlagen für ASP.NET Web Pages-Anwendungen in Visual Studio zu verwenden, installieren Sie ASP.NET MVC 3 RC entweder über den Webplattform-Installer oder [eigenständiges Installationsprogramm](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problem: "&lt;Helper&gt; Klasse wurde nicht gefunden" Fehler

> Nach dem upgrade auf Beta 3 können Sie möglicherweise ein Fehler angezeigt, die eine Hilfsklasse (z. B. die `Facebook` Klasse) kann nicht gefunden werden. In Beta 2 wird gestartet und Weiter geht es im Beta 3 wurden Hilfsprogramme zu Paketen verschoben, die Sie explizit installieren müssen. Vorhandene Websites werden nicht aktualisiert, um diese Pakete enthalten; Dies schließt die Standorte in der *\My Documents\IISExpress* oder *\My Documents\My Websites* Ordner. Insbesondere werden Ihnen dieser Fehler angezeigt, wenn Sie die Standardwebsite in verwenden *My Sites* (WebSite1), enthält einen Verweis auf die `Twitter` Helper.
> 
> **Dieses Problem zu umgehen**  
> Kommentieren Sie Aufrufe an alle Hilfsprogramme in der Website, und führen Sie die  *\_Admin* Seite, und installieren Sie das Paket oder Pakete, die die Hilfsprogramme enthalten, die Sie verwenden möchten. Nachdem Sie das Paket installiert haben, können Sie die auskommentierung der Zeilen, die Hilfsmethoden zu verweisen.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problem: Bereitstellen von Beta 3 ASP.NET Razor-Assemblys in den Ordner "Bin" funktioniert möglicherweise nicht auf das Hosten von Websites

> Wenn Sie eine ASP.NET Web Pages-Website auf einer hosting-Site bereitstellen und Sie die ASP.NET Razor-Beta 3-Assemblys auf der Website bereitstellen *Bin* Ordner treten möglicherweise Fehler auf, einschließlich der folgenden:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Dies kann vorkommen, wenn der Hostinganbieter die ASP.NET Web Pages Beta 1-Assemblys des Servers globalen Anwendungscache (GAC) installiert hat. Assemblys im GAC erhalten Vorrang gegenüber Assemblys, die lokal im installiert die *Bin* Ordner.
> 
> **Problemumgehung** wenden Sie sich an Ihr Hostinganbieter, um sicherzustellen, dass der Fehler wird angezeigt, aufgrund eines Konflikts zwischen der Anbieter Versionen sind der Assemblys und können frei wählen. Wenn dies der Fall ist, fordern Sie, dass der Hostinganbieter die Assemblys im GAC mit dem Server aktualisiert.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problem: Lesen von Feeds oder andere externen Daten über einen Proxyserver

> Ist der Server mit der Website hinter einem Proxyserver befinden, müssen Sie möglicherweise konfigurieren Informationen zum Proxy in der *"Web.config"* Datei, um in der Lage, Informationen zu lesen, die von außerhalb Ihrer Website stammt. Angenommen, Sie verwenden die `ReCaptcha` Hilfsprogramm, das Hilfsprogramm kommuniziert mit dem ReCAPTCHA-Dienst, aber von Ihrem Proxyserver blockiert werden. Auf ähnliche Weise möglicherweise Feeds, die in ASP.NET Web Pages, z. B. den Feed ein, die im Paket-Manager verwendet werden-Proxykonfiguration.
> 
> Bei Problemen in arbeiten mit einem externen Dienst oder Arbeiten mit den paketfeed ein, fügen Sie die folgenden Elemente in Ihrer Anwendung Stamm *"Web.config"* Datei:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Weitere Informationen zum Konfigurieren eines Proxyservers finden Sie unter [ &lt;Proxy&gt; -Element (Netzwerkeinstellungen)](https://msdn.microsoft.com/library/sa91de1e.aspx) auf der MSDN-Website.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problem: "Microsoft.Web.Infrastructure.dll kann nicht geladen werden"-Fehler

> Wenn Sie zuvor die Beta 1-Version von ASP.NET Web Pages mit Razor-Syntax installiert, und klicken Sie dann die Beta 3-Version installieren, werden alle entsprechenden Assemblys im GAC mit Ausnahme von installierten *Microsoft.Web.Infrastructure.dll*. Infolgedessen wird bei der ASP.NET Razor Pages ausführen Fehlermeldung angezeigt wird, der angibt, *Microsoft.Web.Infrastructure.dll* konnte nicht geladen werden.
> 
> Dieses Problem tritt nicht auf, wenn Sie die Beta-3-Version auf einem Computer ohne geladen.
> 
> **Dieses Problem zu umgehen**  
> In der Systemsteuerung Deinstallieren von ASP.NET Web Pages. Installieren Sie Beta 3-Version erneut.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problem: Deinstallieren Sie .NET Framework, Version 4 von ASP.NET Web Pages mit Razor-Syntax deaktiviert

> Wenn Sie .NET Framework, Version 4 deinstallieren und anschließend neu installieren, ist die ASP.NET Web Pages mit Razor-Syntax deaktiviert. Seiten mit den *.cshtml* Erweiterung werden nicht ordnungsgemäß ausgeführt werden. ASP.NET Web Pages registriert eine Assembly im Stammverzeichnis Computer *"Web.config"* Datei und Entfernen von .NET Framework wird diese Datei entfernt. Neuinstallation von .NET Framework installiert eine neue Version der Konfigurationsdatei, aber den Verweis wird nicht für die ASP.NET Web Pages-Assembly hinzufügen.
> 
> **Problemumgehung** installieren Sie nach der Neuinstallation von .NET Framework, ASP.NET Web Pages mit Razor-Syntax. Dadurch wird das folgende Element zum hinzugefügt. die *"Web.config"* -Datei in das Stammverzeichnis des Computer, der in der Regel an folgendem Speicherort ist:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problem: Treten Fehler auf Anwendungen, die zuvor mit ASP.NET-Assemblys im Ordner "Bin" bereitgestellt.

> Während der Bereitstellung, Kopien der ASP.NET Web Pages-Assemblys (z. B. *Microsoft.WebPages.dll*) auf die *Bin* Ordner der Website auf dem Server. (Dies könnte bei der Bereitstellung automatisch stattgefunden haben, oder weil der Entwickler explizit auf die Assemblys kopiert.) Wenn die Beta-3-Version installiert ist, Fehler erfolgt jedoch, wie z. B. Fehler, die bestimmte Typen nicht gefunden. Dies tritt auf, da eine Anzahl von ASP.NET Web Pages-Typen in verschiedene Namespaces für Beta 3-Version verschoben wurden.
> 
> **Dieses Problem zu umgehen**   
> Deaktivieren der *Bin* Ordner der bereitgestellten Anwendung, kopieren Sie die neuen Assemblys in den Ordner (oder erneute Bereitstellung der Anwendung), und klicken Sie dann die Anwendung neu zu starten.


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
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problem: Mithilfe von Web Application Project oder ASP.NET MVC und ASP.NET Web Pages in derselben Anwendung

> Wenn Sie ASP.NET Web Pages in einem Webanwendungsprojekt oder ASP.NET MVC-Anwendung verwendet haben, können Sie möglicherweise ein Fehler angezeigt, die *WebPageHttpApplication* wurde nicht gefunden.
> 
> **Dieses Problem zu umgehen**  
> Wenn Sie diesen Fehler erhalten, ändern Sie die Basisklasse, von der die Anwendung abgeleitet wird. In der *"Global.asax"* Datei, die folgende Zeile ändern:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> Folgendermaßen:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Dadurch werden wirksam, eine Änderung, die eingeführt wurde für die Beta 1-Version von ASP.NET Web Pages mit Razor-Syntax.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problem: Bereitstellen einer Anwendung auf einem Computer, die keinen SQL Server Compact installiert

> Anwendungen mit SQL Server Compact-Datenbanken können auf einem Computer ausführen, in dem SQL Server Compact nicht installiert ist. Microsoft WebMatrix Beta 3 wird automatisch diese Binärdateien für die Sie kopiert und führt die entsprechende *"Web.config"* Datei transformiert.
> 
> **Problemumgehung** Wenn müssen Sie diese Dateien zu kopieren, und stellen die *"Web.config"* dateiänderungen manuell, gehen Sie folgendermaßen vor:
> 
> 1. Kopieren Sie die Datenbank-Engine Assemblys, die *Bin* Ordner (sowie Unterordner) der Anwendung auf dem Zielcomputer: 
> 
>     - Kopie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **zu** *\Bin*
>     - Kopie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **zu** *\Bin\x86*
>     - Kopie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **zu** *\Bin\amd64*
> 2. Klicken Sie im Stammordner der Website, erstellen oder öffnen Sie eine *"Web.config"* Datei. (In WebMatrix Beta 3 dieser Dateityp ist verfügbar, wenn Sie auf **alle** in die **wählen Sie einen Dateityp** Dialogfeld.)
> 3. Fügen Sie das folgende Element als untergeordnetes Element der **&lt;Konfiguration&gt;** Element (nicht in der **&lt;"System.Web"&gt;** Element):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problem: Datenbank- und WebGrid-Hilfsprogramme funktionieren nicht in der mittleren Vertrauensebene in Visual Basic

> Bei Verwendung von Visual Basic (Erstellen von *vbhtml* Dateien), wird die `Database` und `WebGrid` Hilfsprogramme funktioniert nicht, wenn die Anwendung festgelegt wurde, auf die mittlere Vertrauenswürdigkeit verwenden.
> 
> **Dieses Problem zu umgehen**  
> Legen Sie vorübergehend die Anwendung volle Vertrauenswürdigkeit verwenden.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problem: "Encrypt"-Eigenschaft wurde nicht erkannt.

> SQL Server Compact 4.0 nicht erkennt die `Encrypt` Eigenschaft der `SqlCeConnection` Klasse. Sie sollten diese Eigenschaft nicht verwenden, um Datenbankdateien zu verschlüsseln. Die `Encrypt` Eigenschaft wurde in SQLServer Compact 3.5-Version als veraltet markiert und nur für die Abwärtskompatibilität beibehalten wurde. 
> 
> **Dieses Problem zu umgehen**  
> Verwenden der `Encryption Mode` Eigenschaft der `SqlCeConnection` Klasse, um SQL Server Compact 4.0-Datenbankdateien zu verschlüsseln. Das folgende Beispiel zeigt die Vorgehensweise: Erstellen einer verschlüsselten SQL Server Compact 4.0-Datenbank, mithilfe der `Encryption Mode` Eigenschaft:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Führen Sie folgende Schritte aus, um den Verschlüsselungsmodus für eine vorhandene SQL Server Compact 4.0-Datenbank zu ändern:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Um eine unverschlüsselte SQL Server Compact 4.0-Datenbank zu verschlüsseln, führen Sie folgende Schritte aus:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problem: Microsoft Visual C++ 2008-Runtime-Bibliotheken sind erforderlich

> Die systemeigene DLLs von SQL Server Compact 4.0 benötigen, die Microsoft Visual C++ 2008-Laufzeitbibliotheken (x 86, IA64 und x 64), Servicepack 1.
> 
> **Dieses Problem zu umgehen**  
> Installieren Sie .NET Framework 3.5 SP1. Dadurch wird auch die Visual C++ 2008-Runtime-Bibliotheken SP1 installiert. Sie können die Bibliotheken von folgendem Speicherort herunterladen:   
>   
> [Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Beachten Sie, dass die Installation der .NET Framework 2.0, 3.0 oder 4 ist *nicht* Visual C++ 2008-Runtime-Bibliotheken SP1 zu installieren.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problem: Wenn SQL Server Compact vor der Installation von .NET Framework auf dem Computer installiert, ist der invariante Anbietername nicht in der .NET Framework-Datei "Machine.config" registriert

> SQL Server Compact kann auf einem Computer installiert werden, die keine .NET Framework installiert werden, da SQL Server Compact .NET Framework erforderlich ist. Wenn weder .NET Framework, Version 3.5 noch 4 installiert ist, bevor Sie SQL Server Compact installieren, registriert den SQL Server Compact-Setup keine der invariante Name des Anbieters in der *"Machine.config"* Datei. Jede Anwendung, die abhängig von den SQL Server Compact-Eintrag in der *"Machine.config"* Datei schlägt fehl. Der invariante Name des Registrierungseintrags in *"Machine.config"* sieht wie im folgende Beispiel:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Dieses Problem zu umgehen**  
> Deinstallieren Sie SQL Server Compact 4.0-CTP1. Herunterladen Sie und installieren Sie den Vollversionen von .NET Framework von folgendem Speicherort:
> 
> [Microsoft .NET Framework 3.5 Service Pack 1 (vollständiges Paket)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Microsoft .NET Framework 4.0-Version (vollständiges Paket)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Installieren Sie erneut [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Installieren von Anwendungen

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problem: Installieren einer Anwendung kann eine lange dauern, wenn Ordner "Eigene Dateien" des Benutzers auf einer Netzwerkfreigabe umgeleitet wird

> **Dieses Problem zu umgehen**  
> Keine Die Anwendung möglicherweise einige Zeit dauern installieren, jedoch ordnungsgemäß installiert wird.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Veröffentlichen von Anwendungen

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


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Andere Probleme

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problem: Search-Filtern funktioniert nicht in Berichten für Group By: Problemtyp

> Beim Ausführen eines Berichts für einen Standort, wenn Sie Text in eingeben der *Filter nach URL* ein, und klicken Sie auf *Suche*, geschieht nichts. Dies ist, da dieses Steuerelement nicht funktionsfähig ist die *Group By* Zustand des Berichts wird festgelegt, um *Problemtyp*, dies ist die Standardeinstellung.
> 
> **Problemumgehung** In die *Group By* Registerkarte des Menübands, klicken Sie auf *URL* um die Einträge nach ihrer Quell-URL zu gruppieren. Das Textfeld und eine Schaltfläche zum Filtern der Einträge werden in diesem Zustand funktionsfähig.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problem: WCF-Anwendungen nicht, mit IIS Express ausgeführt werden

> Navigieren zu einer WCF-Anwendung tritt ein Fehler wie den folgenden Ausdruck:
> 
> *Konnte nicht geladen werden, Datei oder Assembly ' "Microsoft.Web.Administration", Version = 7.0.0.0, Kultur = Neutral, PublicKeyToken = 31bf3856ad364e35' oder eine ihrer Abhängigkeiten. Das System konnte die angegebene Datei nicht finden.*
> 
> Dies tritt auf, da IIS Express-Betaversion WCF standardmäßig nicht unterstützt.
> 
> **Problemumgehung** eine der folgenden problemumgehungen verwenden (problemumgehung #2 erfordert Microsoft Windows Vista oder höher):
> 
> 
> 1. Kopieren der *Microsoft.Web.dll* und *Microsoft.Web.Administration.dll* Assemblys aus dem WebMatrix-Installationsverzeichnis in die *Bin* WCF Verzeichnis die Anwendung. In der Standardeinstellung WebMatrix installiert ist, der *Microsoft WebMatrix* Unterordner des Systems *Programmdateien* Ordner.
> 2. Unter Microsoft Windows Vista oder höher verwenden, erstellen Sie einen Symlink zu den Assemblys in der *Bin* Verzeichnis mit den folgenden Befehlen. (Dieser Ansatz hat den Vorteil, dass sie eine Kopie der Assemblys nicht erstellt werden.)
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Die beiden Assemblys im GAC installiert. Führen Sie eine Eingabeaufforderung mit erhöhten Rechten die folgenden Befehle aus:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problem: WebMatrix Beta 3 ist nicht möglich, bestimmte Aufgaben ausführen, die erhöhte Rechte erfordern.

> WebMatrix Beta 3 kann nicht für bestimmte Aufgaben, die eine Erhöhung der Rechte wie z. B. die Installation zusätzlicher Komponenten in den folgenden Situationen erfordern:
> 
> - Klicken Sie auf Windows Vista oder Windows 7 Sie angemeldet sind, sich mit einem Konto an, die nicht über Administratorberechtigungen verfügt, und (User Account Control, UAC) ist deaktiviert.
> - Verwenden Sie Microsoft Windows XP oder Microsoft Windows Server 2003.
> 
> **Dieses Problem zu umgehen**  
> Die meisten Aufgaben in WebMatrix Beta 3 sind keine Administratorberechtigungen erforderlich. Für diejenigen, die ausführen, können Sie den Vorgang als Administrator ausführen, oder gehen Sie folgendermaßen vor:
> 
> - Auf Windows Vista oder Windows 7 aktiviert.
> - Unter Windows XP müssen fügen Sie den Benutzer auf die Sicherheitsgruppe "Administratoren hinzu".


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problem: "Site from Web Gallery" ist deaktiviert.

> Die **Standort Web Gallery** Option ist deaktiviert, wenn der Webplattform-Installer 3.0 nicht installiert ist.
> 
> **Dieses Problem zu umgehen**  
> Installieren Sie die [Microsoft Webplattform-Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problem: Unter Windows Server 2003, IIS Express nicht für einen Benutzer ohne Administratorrechte gestartet

> Unter Windows Server 2003 beim Starten von einer Seite oder starten IIS Express wird IIS Express nicht gestartet. Für Webseiten wird eine Fehlermeldung angezeigt, der angibt, dass die Anwendung von einem Benutzer ohne Administratorrechte gestartet wurde.
> 
> **Dieses Problem zu umgehen**  
> Starten Sie WebMatrix Beta 3 als Administrator an. Weitere Informationen finden Sie unter den folgenden Knowledge Base-Artikel:  
>   
> [Eine Anwendung, die von einem Benutzer ohne Administratorrechte gestartet wird kann den HTTP-Datenverkehr des Computers nicht Lauschen auf dem die Anwendung in Windows Vista, Windows Server 2003 oder Windows XP ausgeführt wird.](https://support.microsoft.com/kb/939786)


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


#### <a name="issue-the-relationships-button-is-disabled"></a>Problem: Die Schaltfläche "Beziehungen" ist deaktiviert.

> Die **Beziehungen** Schaltfläche der **Tabelle** Registerkarte die **Datenbanken** Arbeitsbereich ist für SQL Server Compact-Datenbanken deaktiviert.
> 
> **Dieses Problem zu umgehen**  
> Keine SQL Server Compact unterstützt keine Beziehungen zwischen Tabellen.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problem: Parametrisierte SQL-Abfragen Auslösen von Ausnahmen

> In SQL Server Compact 4.0, wenn Sie einen Datentyp nicht wie z. B. angeben `SqlDbType` oder `DbType` für Parameter in parametrisierten Abfragen, wird eine Ausnahme ausgelöst, wenn die Abfrage ausgeführt wird.
> 
> **Dieses Problem zu umgehen**  
> Legen Sie den Datentyp für Parameter explizit z.B. `SqlDbType` oder `DbType`. Dies ist besonders wichtig, im Fall von BLOB-Datentypen (`image` und `ntext`). Verwenden Sie Code wie folgt:
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
