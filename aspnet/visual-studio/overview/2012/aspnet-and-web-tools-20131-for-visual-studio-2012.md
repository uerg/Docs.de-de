---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Versionshinweise für ASP.NET und Webtools 2013.1 für Visual Studio 2012 | Microsoft-Dokumentation
author: microsoft
description: Dieses Dokument beschreibt die Version von ASP.NET und Web Tools 2013.1 für Visual Studio 2012.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 85cd45c25e0f2ad3c8d6d6de73a1a493533e7f7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374259"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Anmerkungen zu dieser Version für ASP.NET und Webtools 2013.1 für Visual Studio 2012
====================
durch [Microsoft](https://github.com/microsoft)

> Dieses Dokument beschreibt die Version von ASP.NET und Web Tools 2013.1 für Visual Studio 2012.


## <a name="contents"></a>Inhalt

- [Installationshinweise](#install)
- [Softwareanforderungen](#requirements)
- Neue Features in ASP.NET und Webtools 2013.1 für Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Vorlagen](#templates)

        - [ASP.NET MVC 5-Vorlage](#mvc5template)
        - [ASP.NET Web API 2-Vorlage](#apitemplate)
        - [Elementvorlagen](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.NET-Gerüstbau](#scaffold)
    - [Razor-Editor](#razor)
    - [NuGet 2.7](#nuget)
- Bekannte Probleme und aktueller Änderungen

    - [ASP.NET-Gerüstbau](#issuescaffolding)

        - [MVC und Web-API-Gerüstbau - HTTP 404-Antwort, wurde nicht gefunden-Fehler](#404issue)
        - [Visual Studio Express 2012 für das Web funktioniert nach dem Hinzufügen eines Elements mit Gerüst](#expressissue)
    - [ASP.NET Razor-3](#issuerazor)

        - [Anzeigen von Cshtml-Datei öffnen oder F5 bewirkt, dass einen Serverfehler](#browseissue)
        - [URL Rewrite und Tilde(~)](#rewriteissue)
    - [Vorlagen](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Installationshinweise

[Installieren Sie](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET und Web Tools 2013.1 für Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Softwareanforderungen

Sie müssen entweder Visual Studio 2012 oder Visual Studio Express 2012 für Web verfügen.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Neue Features in ASP.NET und Webtools 2013.1 für Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap-Stil

Wenn Sie MVC 5-Controller und Ansichten erstellen, verwendet das Markup für die Ansichten [Bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Vorlagen

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>ASP.NET MVC 5-Vorlage

Wir haben eine neue MVC 5-Vorlage hinzugefügt. Die neuesten MVC 5-NuGet-Pakete verweist können, und Sie Gerüstbau um Controller und Ansichten hinzuzufügen.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>ASP.NET Web API 2-Vorlage

Wir haben eine neue Web-API 2-Vorlage hinzugefügt. Die neuesten Web API 2-NuGet-Pakete verweist können, und Sie Gerüstbau um Controller und Ansichten hinzuzufügen.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Elementvorlagen

Wir haben neue Vorlagen für MVC 5-Ansichten, Web Pages (Razor-3) und Web-API 2-Controller hinzugefügt. Installieren sie die entsprechenden NuGet-Pakete für das Projekt, beim Hinzufügen neuer Elemente.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Wenn Sie einen MVC oder Web-API-Controller, die mithilfe von Entity Framework aufbauen, verwenden wir Framework 6. Weitere Informationen über Entity Framework finden Sie unter den [Entity Framework-Versionsverlaufs](https://msdn.com/data/jj574253).

Sie können auch herunterladen und installieren die Entity Framework 6-Tools für Visual Studio 2012. Finden Sie unter den [beziehen von Entitätsframework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET-Gerüstbau

ASP.NET-Gerüstbau ist ein Code-Generierung-Framework für Webanwendungen mit ASP.NET. Es erleichtert die Codebausteine zu Ihrem Projekt hinzufügen, die mit einem Datenmodell interagiert.

In früheren Versionen von Visual Studio konnte Gerüstbau ASP.NET MVC-Projekte. Mit diesem Update können Sie jetzt Gerüstbau für ASP.NET Projekte, einschließlich Web Forms verwenden. Dieses Update unterstützt nicht das Generieren von Seiten für eine Web Forms-Projekt, aber Sie können weiterhin Gerüstbau mit Web Forms verwenden, durch das Hinzufügen von MVC-Abhängigkeiten auf das Projekt. Unterstützung für das Generieren von Seiten für Web Forms wird in einem späteren Update hinzugefügt werden.

Wenn Gerüstbau verwenden zu können, stellen wir sicher, dass alle erforderlichen Abhängigkeiten im Projekt installiert sind. Z. B. Wenn Sie mit einem ASP.NET Web Forms-Projekt beginnen, und klicken Sie dann mithilfe von Gerüstbau einer Web-API-Controller hinzu, werden die erforderlichen NuGet-Pakete und Verweise dem Projekt automatisch hinzugefügt.

MVC-Gerüstbau um eine Web Forms-Projekt hinzuzufügen, fügen einen **neues Gerüstelement** , und wählen Sie **MVC 5-Abhängigkeiten** im Dialogfenster. Es gibt zwei Optionen für den Gerüstbau für MVC. Minimaler und vollständiger. Wenn Sie die minimale auswählen, werden nur die NuGet-Pakete und Verweise für ASP.NET MVC zu Ihrem Projekt hinzugefügt. Bei Auswahl der Option "vollständig", die mindestens erforderliche Abhängigkeiten hinzugefügt werden, sowie die erforderlichen Inhaltsdateien für ein MVC-Projekt.

Unterstützung für den Gerüstbau für Async-Controller verwendet, die neuen asynchronen Features von Entity Framework 6.

Weitere Informationen und Lernprogramme finden Sie unter [Gerüstbau-Übersicht über ASP.NET](../2013/aspnet-scaffolding-overview.md). Diese Tutorials erfahren Sie, Gerüstbau, mit Visual Studio 2013, aber sie gelten auch für ASP.NET und Web Tools 2013.1 für Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor-Editor

Mit diesem Update unterstützt Visual Studio 2012 nun Razor-3-Tools/bearbeiten.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 umfasst einen umfangreichen Satz von neuen Features, die beschrieben werden, im Detail [Anmerkungen zu NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Diese Version von NuGet besteht keine Notwendigkeit für Benutzer explizit zulassen von NuGet auf fehlende Pakete wiederhergestellt werden. Bei der Installation von NuGet 2.7 Stimmen der Benutzer implizit auf fehlende Pakete automatisch wiederhergestellt. Paket wiederherstellen über die NuGet-Einstellungen in Visual Studio können Benutzer explizit deaktivieren. Diese Änderung wird vereinfacht, wie die Paket-Wiederherstellung funktioniert.

## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET-Gerüstbau

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC und Web-API-Gerüstbau - HTTP 404-Antwort, wurde nicht gefunden-Fehler

Wenn Sie beim Hinzufügen eines Elements mit Gerüst zu einem Projekt ein Fehler auftritt, ist es möglich, die Ihr Projekt in einem inkonsistenten Zustand gelassen werden. Einige der Gerüstbau werden die Änderungen werden rückgängig gemacht werden, aber andere Änderungen, z. B. die installierten NuGet-Pakete werden nicht zurückgesetzt werden. Wenn die Weiterleitung Änderungen an der Konfiguration ein Rollback ausgeführt werden, erhalten Benutzer einen HTTP 404-Fehler bei der Navigation zu Elementen erstellt haben.

Klicken Sie zum Beheben dieses Fehlers für MVC, fügen Sie ein neues gerüstelement hinzu, und wählen Sie MVC 5-Abhängigkeiten (minimale oder vollständige). Dabei werden alle erforderlichen Änderungen zu Ihrem Projekt hinzufügen.

So beheben Sie diesen Fehler für Web-API:

1. Fügen Sie die folgende Klasse "webapiconfig" zu Ihrem Projekt hinzu.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Konfigurieren Sie in der Anwendung WebApiConfig.Register\_Start-Methode in "Global.asax" wie folgt:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 für das Web funktioniert nach dem Hinzufügen eines Elements mit Gerüst

Wenn Visual Studio Express 2012 für Web nicht mehr funktioniert nach dem Hinzufügen des Elements mit Gerüst mit Entity Framework (z. B. Web-API 2-Controller mit Aktionen unter Verwendung von Entity Framework), ist es möglich, dass Visual Studio Express beim Laden Sie das systemeigene Image der Assembly Fehler. Abhängig von "System.Web.Extensions".

Um dieses Problem zu beheben, konfigurieren Sie die Visual Studio Express die MSIL-Image von "System.Web.Extensions" funktioniert:

1. Öffnen Sie-Eingabeaufforderung im Administratormodus.
2. Wechseln Sie zur %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE oder % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (für 64-Bit-Windows).
3. Öffnen Sie VWDExpress.exe.config in einem Text-Editor ein.
4. Fügen Sie die folgende Zeile unter der &lt;Konfiguration&gt;/&lt;Runtime&gt; Element:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Starten Sie Visual Studio Express 2012 für das Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor-3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Anzeigen von Cshtml-Datei WithBrowse WithorF5causes ein Serverfehler

Beim Erstellen einer MVC 5-Projekt in Visual Studio 2012 (oder öffnen Sie in Visual Studio 2012 ein MVC 5-Projekt, das in Visual Studio 2013 erstellt wurde) und versucht, eine Cshtml-Datei mit Browserauswahl oder F5 anzeigen, erhalten Sie eine Fehlermeldung mit - **in Server-Fehlers Anwendung '/'**. Der Server versucht, das Navigieren zu `http://localhost:XXXX/Views/../XXXX.cshtml`

Um dieses Problem zu beheben, Ändern der **Startaktion** festlegen, die in Ihrem Projekt so **bestimmte Seite**. Sie müssen sich nicht um einen Wert für die Seite bereitzustellen.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Nach dieser Änderung, die in das Stammverzeichnis der Anwendung navigiert Auswählen der F5 (`http://localhost:XXXX`). Dieses Verhalten ist nicht identisch mit das Verhalten für MVC 5-Projekte in Visual Studio 2013, in denen die **aktuelle Seite** Einstellung startet die Seite geöffnet.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>URL Rewrite und Tilde(~)

Nach dem Upgrade auf ASP.NET Razor-3 oder ASP.NET MVC 5, kann die tilde(~) Notation nicht mehr ordnungsgemäß funktioniert, bei Verwendung von URL-Umschreibungen. Die URL-Rewrite beeinflusst die Notation tilde(~) in HTML-Elementen wie &lt;ein /&gt;, &lt;Skript /&gt;, &lt;LINK /&gt;, und daher die Tilde nicht mehr in das Stammverzeichnis zugeordnet wird.

Angenommen, Sie umschreiben, dass die Anforderungen für **asp.net/content** zu **asp.net**, das Href-Attribut im &lt;A Href = "~/content/" /&gt; löst in **/content/ Inhalt /** anstelle von **/**. Um diese Änderung zu unterdrücken, Sie können festlegen, die **IIS\_WasUrlRewritten** Kontext auf "false" auf jeder Webseite oder in **Anwendung\_BeginRequest** in "Global.asax".

<a id="templateissue"></a>
### <a name="templates"></a>Vorlagen

Wenn Sie ASP.NET MVC Projekte mit Visual Studio 2012 unter Windows 8.1 oder Windows Server 2012 R2, Visual Studio erstellen, zeigt eine Fehlermeldung, die besagt "Konfigurieren von Web [Url] für ASP.NET 4.5 fehlgeschlagen".

![Fehler bei der Konfiguration](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Dieser Fehler wird angezeigt, da Visual Studio 2012 die ASP.NET 4.5-Funktion nicht aktiviert ist, wenn sie auf diese Versionen von Windows installiert ist. Um ASP.NET 4.5 zu aktivieren, führen Sie die Schritte in [Aktivieren von Windows-Funktionen ein- oder ausschalten](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![Aktivieren Sie oder deaktivieren Sie Windows-features](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Alternativ können Sie ASP.NET 4.5 über die Befehlszeile aktivieren.

1. Öffnen Sie-Eingabeaufforderung im Administratormodus.
2. Führen Sie den folgenden Befehl zum Aktivieren von ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
