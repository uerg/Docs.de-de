---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Web Pages (Razor) – häufig gestellte Fragen | Microsoft-Dokumentation
author: tfitzmac
description: In diesem Artikel werden einige häufig gestellten Fragen zu ASP.NET Web Pages (Razor) und WebMatrix aufgeführt. Software-Versionen verwendet, in dem Tutorial ASP.NET Web Pages (R...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 03e89adc2415808ea49107616f529fa895d6e52a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835643"
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages (Razor) – häufig gestellte Fragen
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix wird nicht mehr als eine integrierte Entwicklungsumgebung für ASP.NET Web Pages empfohlen. Verwendung [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) oder [Visual Studio-Code](https://code.visualstudio.com/).
>
> In diesem Artikel werden einige häufig gestellten Fragen zu ASP.NET Web Pages (Razor) und WebMatrix aufgeführt.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2 WebMatrix 2 und Visual Studio 2012.


- [Was ist der Unterschied zwischen ASP.NET Web Pages, ASP.NET Web Forms und ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Benötige ich für die Arbeit mit Web Pages WebMatrix?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Kann ich ASP.NET Web Forms-Steuerelemente auf einer Seite für die Webseiten verwenden?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Kann ich eine ASP.NET Web Pages-Website bereitstellen, ohne mithilfe von WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [Habe ich, um die WebSecurity-Hilfsprogramm verwenden, um Anmeldungen zu unterstützen?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Unterstützt die ASP.NET Web Pages HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Kann ich JavaScript und jQuery mit Webseiten verwenden?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Additional Resources](#AdditionalResources) (Zusätzliche MSBuild-Ressourcen)

Fragen zu Fehlern und anderen Problemen finden Sie auf die [ASP.NET Web Pages (Razor) – Handbuch zur Problembehandlung](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Was ist der Unterschied zwischen ASP.NET Web Pages, ASP.NET Web Forms und ASP.NET MVC?

Alle drei sind ASP.NET-Technologien für das Erstellen dynamischer Webanwendungen:

- Hinzufügen von Code der dynamischen (serverseitig) und Zugriff auf die Datenbank zu HTML-Seiten und Features einfache und Syntax ASP.NET Web Pages im Mittelpunkt.
- ASP.NET Web Forms basiert auf einer Page-Objektmodell und herkömmlichen Fenstertyp-Steuerelemente (Schaltflächen, Listen, usw.). Web Forms verwendet eine ereignisbasierte Modell, das vertraut ist, die mit der Entwicklung für Client-basierten (Windows Forms) gearbeitet haben.
- ASP.NET MVC implementiert das Model-View-Controller-Muster für ASP.NET. Der Schwerpunkt liegt auf "Trennung von Belangen" (Verarbeitungs-, Daten- und UI-Ebenen).

Alle drei Frameworks werden vollständig unterstützt und weiterhin vom ASP.NET-Team entwickelt werden. Im Allgemeinen die Wahl des welches Framework verwenden, hängt von Ihrem Hintergrund und Erfahrung mit ASP.NET.

ASP.NET Web Pages insbesondere soll es einfach für Personen, die HTML für die Verarbeitung ihrer Seiten hinzufügen, um bereits kennen. Es ist eine gute Wahl für Schüler/Studenten, Hobbyprogrammierer, in der Regel Personen, die in der Programmierung sind. Es kann auch eine gute Wahl für Entwickler sein, die sich mit nicht zu ASP.NET Web-Technologien auskennen.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Benötige ich für die Arbeit mit Web Pages WebMatrix?

Nein. WebMatrix wird nicht mehr als eine integrierte Entwicklungsumgebung für ASP.NET Web Pages empfohlen. Verwendung [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) oder [Visual Studio-Code](https://code.visualstudio.com/).

Wenn Sie nicht Visual Studio oder Visual Studio Code verwenden möchten, können Sie einzeln mit Komponentenprodukte installieren [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Sie benötigen die folgenden Produkte:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (der auch die ASP.NET Web Pages-Framework installiert wird)
- IIS Express (der Webserver)
- Microsoft SQL Server Compact 4.0 (Datenbank)

Sie können einen Text-Editor zum Bearbeiten *.cshtml* (oder *vbhtml*) Seiten.

Verwalten von SQL Server Compact-Datenbanken (*.sdf* Dateien), ohne ein Tool etwas schwieriger ist. Visual Studio Containds-Tools für die Verwaltung von *.sdf* Datenbanken. Sie können auch SQL-Befehle im Code, um viele SQL Server-Verwaltungsaufgaben ausführen.

So testen Sie *.cshtml* Seiten ohne Verwendung einer integrierten Entwicklungsumgebung (IDE), können Sie sie bereitstellen auf einem live-Server. (Finden Sie unter [kann ich eine ASP.NET Web Pages-Website ohne mithilfe von WebMatrix bereitstellen?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>IIS Express ausführen, ohne mit einer IDE

Bei der Installation von IIS Express auf Ihrem Computer als Webserver können, der Sie um die Seiten zu testen. Sie können IIS Express ausführen, von der Befehlszeile aus und ordnen es mit einer bestimmten Portnummer. Sie klicken Sie dann diesen Port angeben, wenn Sie anfordern *.cshtml* Dateien in Ihrem Browser.

Klicken Sie in Windows, öffnen Sie eine Eingabeaufforderung mit Administratorrechten, und ändern Sie in *c:\Programme\Microsoft Files\IIS Express.* (Für 64-Bit-Systemen verwenden Sie den Ordner *C:\Program Files (x86) \IIS Express.)* Geben Sie dann, um den tatsächlichen Pfad zu Ihrer Website über den folgenden Befehl aus:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Sie können eine beliebige Portnummer verwenden, die bereits von einem anderen Prozess reserviert ist nicht. (Portnummern über 1024 sind in der Regel kostenlos.) Für die `path` -Wert aufweist, verwenden Sie den Pfad des Ordners Website, in denen die *.cshtml* Dateien sind.

Nach dem Ausführen dieses Befehls auf IIS Express so einrichten, Ihre Seiten angeboten werden, können Sie diese öffnen Sie einen Browser und navigieren Sie zu einem *.cshtml* Datei. Verwenden Sie eine URL wie folgt:

`http://localhost:35896/default.cshtml`

Geben Sie für Hilfe zur IIS Express-Befehlszeilenoptionen, `iisexpress.exe /?` an der Befehlszeile eingeben.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Kann ich ASP.NET Web Forms-Steuerelemente auf einer Seite für die Webseiten verwenden?

Nein. Web Forms-Steuerelemente, wie die [Kontrollkästchen](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) -Steuerelement, das [Steuerelemente zur gültigkeitsprüfung](https://msdn.microsoft.com/library/bwd43d0x), und die [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) funktioniert nur in Web Forms-Seiten steuern (*aspx* Dateien). Diese Steuerelemente erfordern des Web Forms-Seiten-Frameworks.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Kann ich eine ASP.NET Web Pages-Website bereitstellen, ohne mithilfe von WebMatrix?

Ja. Sie können manuell Websitedateien auf einem Server (in der Regel mithilfe von FTP) kopieren. Wenn Sie eine manuelle Kopie ausführen, müssen Sie auch die Dateien zu kopieren, die SQL Server Compact (Datenbank) unterstützen. Weitere Informationen finden Sie im Blogeintrag [Bereitstellen von Web Pages-Anwendungen, ohne ein Tool](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>Habe ich, um die WebSecurity-Hilfsprogramm verwenden, um Anmeldungen zu unterstützen?

Nein. Die `SimpleMembership` -Anbieter, die Teil von ASP.NET Web Pages ist eine Option. Der Sicherheitsanbieter, die Teil von ASP.NET sind (die Sie verwendet werden können, mit der in Web Forms arbeiten) sind ebenfalls verfügbar. Z. B. können Formularauthentifizierung in ASP.NET Web Pages Sie genau wie in Web Forms. Ein Beispiel zur Verwendung der Formularauthentifizierung finden Sie unter den Microsoft Support-Artikel [implementieren formularbasierter Authentifizierung in Ihrer ASP.NET-Anwendung erfolgt mithilfe von c#.NET](https://support.microsoft.com/kb/301240). Ein einfaches Beispiel finden Sie unter [ASP.NET-Version "Anmeldung &amp; Kennwort](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Informationen zur Verwendung von Windows-Authentifizierung finden Sie im Blogbeitrag [mithilfe von Windows-Authentifizierung in ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Unterstützt die ASP.NET Web Pages HTML5?

Ja. Die Seiten, die Sie mit ASP.NET Web Pages erstellen (*.cshtml* oder *vbhtml* Seiten) sind im Wesentlichen HTML-Seiten, die auch Code enthalten, die auf dem Server ausgeführt wird, bevor die Seite gerendert wird. Solange der Browser des Benutzers HTML5 unterstützt, können Sie HTML5-Elementen in einem *.cshtml* oder *vbhtml* Seite.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Kann ich JavaScript und jQuery mit Webseiten verwenden?

Unbedingt. Die Seiten, die Sie mit ASP.NET Web Pages erstellen (*.cshtml* oder *vbhtml* Seiten) werden nur HTML-Seiten mit Servercode darin. Aus diesem Grund alles was Sie tun können durch eine normale HTML-Seite mit JavaScript oder jQuery ist ebenfalls möglich ein *.cshtml* oder *vbhtml* Seite.

Die **Starter Site** Vorlage in WebMatrix enthält eine Reihe von jQuery-Bibliotheken. Wenn Sie eine Website erstellen, mit der Vorlage, mit der *Skripts* Ordner enthält eine jQuery-Kernbibliothek (*Jquery-1.6.2.js)* und-Bibliotheken für die jQuery-Validierung (*"jQuery.Validate.js"* usw..).

Hier sind einige Blogbeiträge, die veranschaulichen, wie jQuery mit ASP.NET Web Pages verwendet:

- [Durch Hinzufügen von jQuery-Vorteile für ASP.NET Web Pages mithilfe von WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) von Rachel Appel
- [5 Min.: WebMatrix und jQuery UI + Json und die jQuery-Vorlagen](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) von Jonas Eriksson
- [WebMatrix und jQuery-Formulare](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) von Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Leitfaden zur Behandlung von Problemen mit ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[WebMatrix und ASP.NET Web Pages-Forum](https://forums.asp.net/1224.aspx/1?WebMatrix) auf der ASP.NET-Website
