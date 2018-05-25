---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Versionshinweise für ASP.NET und Webtools 2013.1 für Visual Studio 2012 | Microsoft Docs
author: microsoft
description: Dieses Dokument beschreibt die Version von ASP.NET und 2013.1 für Web-Tools für Visual Studio 2012.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Versionshinweise für ASP.NET und Webtools 2013.1 für Visual Studio 2012
====================
durch [Microsoft](https://github.com/microsoft)

> Dieses Dokument beschreibt die Version von ASP.NET und 2013.1 für Web-Tools für Visual Studio 2012.


## <a name="contents"></a>Inhalt

- [Hinweise zur installationsnachbereitung](#install)
- [Softwareanforderungen](#requirements)
- Neue Funktionen in ASP.NET und Webtools 2013.1 für Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Vorlagen](#templates)

        - [ASP.NET MVC 5-Vorlage](#mvc5template)
        - [ASP.NET Web API 2-Vorlage](#apitemplate)
        - [Elementvorlagen](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.NET Gerüstbau](#scaffold)
    - [Razor Editor](#razor)
    - [NuGet 2.7](#nuget)
- Bekannte Probleme und aktueller Änderungen

    - [ASP.NET Gerüstbau](#issuescaffolding)

        - [MVC und Web-API Gerüstbau - HTTP 404, wurde nicht gefunden.](#404issue)
        - [Visual Studio Express 2012 für das Web funktioniert nicht mehr nach dem Hinzufügen eines gerüstelements](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [Anzeigen von Cshtml-Datei Browserauswahl oder F5 bewirkt, dass einen Serverfehler](#browseissue)
        - [URL Rewrite und Tilde(~)](#rewriteissue)
    - [Vorlagen](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Hinweise zur installationsnachbereitung

[Installieren Sie](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET und Tools für Visual Studio 2012 2013.1.

<a id="requirements"></a>
## <a name="software-requirements"></a>Softwareanforderungen

Sie benötigen Visual Studio 2012 oder Visual Studio Express 2012 für das Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Neue Funktionen in ASP.NET und Webtools 2013.1 für Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

Wenn Sie das Gerüst für MVC 5-Controller und Ansichten erstellen, verwendet das Markup für die Ansichten [Bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Vorlagen

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>ASP.NET MVC 5-Vorlage

Eine neue MVC 5-Vorlage wurde hinzugefügt. Er verweist auf die neueste MVC 5-NuGet-Pakete, und Sie können Gerüstbau Controller und Ansichten hinzufügen.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>ASP.NET Web API 2-Vorlage

Eine neue Web-API 2-Vorlage wurde hinzugefügt. Er verweist auf die neueste Web API 2-NuGet-Pakete, und Sie können Gerüstbau Controller und Ansichten hinzufügen.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Elementvorlagen

Wir haben neue Elementvorlagen für MVC 5-Ansichten, Web Pages (Razor-3) und Web-API 2-Controller hinzugefügt. Sie installieren die verwandte NuGet-Pakete für das Projekt beim Hinzufügen neuer Elemente.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Wenn Sie einen MVC oder Web-API-Controller, die Verwendung von Entity Framework zu erstellen, verwenden wir Framework 6. Weitere Informationen über Entity Framework finden Sie unter der [Entity Framework-Versionsverlaufs](https://msdn.com/data/jj574253).

Sie können auch herunterladen und installieren den Entity Framework 6-Tools für Visual Studio 2012. Finden Sie unter der [Get Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Gerüstbau

ASP.NET Gerüstbau ist ein Code-Generierung-Framework für ASP.NET-Webanwendungen. Es vereinfacht den Standardcode zu Ihrem Projekt hinzufügen, die mit einem Datenmodell interagiert.

In früheren Versionen von Visual Studio konnte Gerüstbau ASP.NET MVC-Projekte. Mit diesem Update können Sie jetzt Gerüst für eine beliebige ASP.NET-Projekt, einschließlich Web Forms verwenden. Dieses Update Generieren von Seiten für eine Web Forms-Projekt nicht unterstützt, aber Sie können weiterhin Gerüstbau mit Web Forms verwenden, indem Sie dem Projekt MVC-Abhängigkeiten hinzugefügt. Unterstützung für das Generieren von Seiten für Web Forms wird in einem zukünftigen Update hinzugefügt werden.

Mithilfe von Gerüstbau stellen wir sicher, dass alle erforderlichen Abhängigkeiten im Projekt installiert sind. Z. B. Wenn Sie mit einer ASP.NET Web Forms-Projekt beginnen und dann mithilfe von Gerüstbau einer Web-API-Controller hinzufügen, werden die erforderlichen NuGet-Pakete und Verweise dem Projekt automatisch hinzugefügt.

Um Gerüstbau für MVC eine Web Forms-Projekt hinzuzufügen, fügen einen **neues Gerüstelement** , und wählen Sie **MVC 5-Abhängigkeiten** im Dialogfenster. Es gibt zwei Optionen für MVC Gerüstbau; Minimal "und" vollständig ". Wenn Sie die minimale auswählen, werden nur die NuGet-Pakete und Verweise für ASP.NET MVC zu Ihrem Projekt hinzugefügt. Wenn Sie die Option "vollständig", wählen Sie die minimale Abhängigkeiten hinzugefügt werden, sowie die erforderlichen Inhaltsdateien für ein MVC-Projekt.

Unterstützung für asynchrone Controller Gerüstbau verwendet die neue asynchrone Funktionen von Entity Framework 6.

Weitere Informationen und Lernprogramme finden Sie unter [Gerüstbau-Übersicht über ASP.NET](../2013/aspnet-scaffolding-overview.md). In diesen Lernprogrammen erfahren Gerüstbau mit Visual Studio 2013, aber sie gelten auch für ASP.NET und 2013.1 von Web Tools für Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor Editor

Mit diesem Update unterstützt Visual Studio 2012 jetzt 3 Razor-Tools/bearbeiten.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 enthält einen umfangreichen Satz von neuen Features, die beschrieben werden ausführlich auf [NuGet 2.7-Anmerkungen zu dieser Version](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Diese Version von NuGet beseitigt die Notwendigkeit für Benutzer ermöglichen, dass explizit NuGet wiederherstellen fehlender Pakete. Bei der Installation von NuGet 2.7 stimmen Benutzer implizit automatisch wiederherstellen fehlender Pakete. Benutzer können außerhalb des Pakets Wiederherstellung über die NuGet-Einstellungen in Visual Studio explizit deaktivieren. Diese Änderung vereinfacht die Funktionsweise der Wiederherstellung des Pakets.

## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Gerüstbau

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC und Web-API Gerüstbau - HTTP 404, wurde nicht gefunden.

Wenn Sie beim Hinzufügen eines gerüstelements zu einem Projekt ein Fehler auftritt, ist es möglich, die Ihr Projekt in einem inkonsistenten Zustand gelassen werden. Einige Änderungen vorgenommen werden Gerüstbau wird ein Rollback, aber andere Änderungen, z. B. die installierten NuGet-Pakete werden nicht zurückgesetzt werden. Falls die routing konfigurationsänderungen zurückgesetzt werden, erhalten Benutzer einen HTTP 404-Fehler beim Navigieren zu Elemente Gerüstbau.

Klicken Sie zum Beheben dieses Fehlers für MVC eine neue Elements mit Gerüst hinzufügen, und wählen Sie MVC 5-Abhängigkeiten (minimale oder vollständige). Dabei werden alle erforderlichen Änderungen zu Ihrem Projekt hinzufügen.

So beheben Sie diesen Fehler für die Web-API:

1. Fügen Sie die folgende Klasse "webapiconfig" zu Ihrem Projekt hinzu.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Konfigurieren Sie in der Anwendung WebApiConfig.Register\_Start-Methode in "Global.asax" wie folgt:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 für das Web funktioniert nicht mehr nach dem Hinzufügen eines gerüstelements

Wenn Visual Studio Express 2012 für das Web nicht mehr funktioniert nach dem Hinzufügen des Elements mit Gerüst mit Entity Framework (z. B. Web-API 2-Controller mit Aktionen unter Verwendung von Entity Framework), ist es möglich, dass Visual Studio Express nicht das systemeigene Image einer Assembly zu laden. Abhängig von System.Web.Extensions hinzu.

Um dieses Problem zu beheben, konfigurieren Sie Visual Studio Express mit MSIL-Images System.Web.Extensions arbeiten:

1. Öffnen Sie Eingabeaufforderung im Administratormodus.
2. Wechseln Sie zu %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE oder % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (für 64-Bit-Windows).
3. VWDExpress.exe.config in einem Text-Editor zu öffnen.
4. Fügen Sie die folgende Zeile unter der &lt;Konfiguration&gt;/&lt;Runtime&gt; Element:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Neustart des Visual Studio Express 2012 für das Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Anzeigen von Cshtml-Datei WithBrowse WithorF5causes eines Serverfehlers

Beim Erstellen einer MVC 5-Projekt in Visual Studio 2012 (oder öffnen Sie in Visual Studio 2012 ein MVC 5-Projekt, das in Visual Studio 2013 erstellt wurde) und versucht, eine Cshtml-Datei mithilfe von Browserauswahl oder F5 anzeigen, wird eine Fehlermeldung, die besagt - **Serverfehler in '/' Anwendung**. Der Server versucht, zu navigieren`http://localhost:XXXX/Views/../XXXX.cshtml`

Um dieses Problem zu beheben, ändern Sie die **Startaktion** in Ihrem Projekt zum Festlegen von **bestimmte Seite**. Sie müssen nicht auf einen Wert für die Seite bereitstellen.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Nach dieser Änderung, die in das Stammverzeichnis der Anwendung navigiert F5 auswählen (`http://localhost:XXXX`). Dieses Verhalten ist nicht mit dem Verhalten für MVC 5-Projekte in Visual Studio 2013 identisch, in dem die **aktuelle Seite** Einstellung startet die Seite öffnen.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>URL Rewrite und Tilde(~)

Nach dem Upgrade auf ASP.NET Razor-3 oder ASP.NET MVC 5, kann die Notation tilde(~) nicht mehr ordnungsgemäß funktioniert, bei Verwendung von URL-schreibt. Der URL-Rewrite beeinflusst die tilde(~) Notation in HTML-Elementen wie &lt;A /&gt;, &lt;Skript /&gt;, &lt;LINK /&gt;, und daher die Tilde nicht mehr in das Stammverzeichnis zugeordnet ist.

Angenommen, Sie schreiben Sie Anforderungen für **asp.net/content** auf **asp.net**, Href-Attribut im &lt;A Href = "~/content/" /&gt; löst in **/content/ Inhalt /** anstelle von **/**. Um diese Änderung zu unterdrücken, legen Sie die **IIS\_WasUrlRewritten** Kontext auf "false" in jeder Webseite oder in **Anwendung\_BeginRequest** in "Global.asax".

<a id="templateissue"></a>
### <a name="templates"></a>Vorlagen

Wenn Sie ASP.NET MVC Projekte mit Visual Studio 2012 für Windows 8.1 oder Windows Server 2012 R2, Visual Studio zu erstellen, zeigt eine Fehlermeldung, die angibt, "Konfigurieren von Web [Url] für ASP.NET 4.5 fehlgeschlagen".

![Fehler bei der Konfiguration](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Dieser Fehler wird angezeigt, weil die ASP.NET 4.5-Funktion von Visual Studio 2012 nicht aktiviert wird, wenn sie auf diese Versionen von Windows installiert ist. Um ASP.NET 4.5 zu aktivieren, führen Sie die Schritte, die in beschriebenen [Windows-Funktionen ein- oder ausschalten](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![Aktivieren Sie oder deaktivieren Sie Windows-features](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Alternativ können Sie ASP.NET 4.5 über die Befehlszeile aktivieren.

1. Öffnen Sie Eingabeaufforderung im Administratormodus.
2. Führen Sie den folgenden Befehl zum Aktivieren von ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
