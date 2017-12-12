---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: "ASP.NET Web Pages (Razor) – häufig gestellte Fragen | Microsoft Docs"
author: tfitzmac
description: "In diesem Artikel werden einige häufig gestellten Fragen zu ASP.NET Web Pages (Razor) und WebMatrix aufgelistet. Versionen der Software verwendet, in dem Lernprogramm ASP.NET Web Pages (R..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 7f6dc3b56a33bcbe3e1e4086681ca1ba76d7d153
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages (Razor) – häufig gestellte Fragen
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix wird nicht mehr als eine integrierte Entwicklungsumgebung für ASP.NET Web Pages empfohlen. Verwendung [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) oder [Visual Studio-Code](https://code.visualstudio.com/).
>
> In diesem Artikel werden einige häufig gestellten Fragen zu ASP.NET Web Pages (Razor) und WebMatrix aufgelistet.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix-3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2, 2 von WebMatrix und Visual Studio 2012.


- [Was ist der Unterschied zwischen ASP.NET Web Pages, ASP.NET Web Forms und ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Benötige ich für die Arbeit mit Webseiten WebMatrix?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Kann ich die ASP.NET Web Forms-Steuerelemente auf einer Web Pages-Seite verwenden?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Kann ich eine ASP.NET Web Pages-Website bereitstellen, ohne mithilfe von WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [Habe ich das WebSecurity-Hilfsprogramm verwenden, um Anmeldungen zu unterstützen?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Unterstützt die ASP.NET Web Pages HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Kann ich JavaScript und jQuery mit Webseiten verwenden?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Additional Resources](#AdditionalResources) (Zusätzliche MSBuild-Ressourcen)

Fragen zu Fehlern und anderen Problemen finden Sie auf der [Problembehandlungshandbuch für ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Was ist der Unterschied zwischen ASP.NET Web Pages, ASP.NET Web Forms und ASP.NET MVC?

Alle drei sind ASP.NET-Technologien für die Erstellung dynamischer Webanwendungen:

- ASP.NET Web Pages konzentriert sich auf das Hinzufügen von dynamischen (serverseitige) Code und den Datenbankzugriff zu HTML-Seiten und Funktionen einfache und kompakte Syntax.
- ASP.NET Web Forms basiert auf einer Seite-Objektmodell und herkömmliche Fenstertyp Steuerelemente (Schaltflächen, Listen, usw.). WebForms verwendet eine ereignisbasierte Modell, vertraut, die bei der Entwicklung für Client-basierten (Windows Forms) gearbeitet haben.
- ASP.NET MVC implementiert das Model View Controller-Muster für ASP.NET. Die Betonung liegt auf "Trennung von Anliegen" (Verarbeitungs-, Daten- und UI-Ebenen).

Alle drei Frameworks vollständig unterstützt werden und weiterhin vom Team ASP.NET entwickelt werden. Die Wahl, welche-Framework verwenden im Allgemeinen abhängig ist, auf den Hintergrund und Erfahrung im Umgang mit ASP.NET.

ASP.NET Web Pages insbesondere ist so ausgelegt für Personen erleichtern, die HTML für die Verarbeitung zu ihren Seiten hinzufügen bereits vertraut. Es ist eine gute Wahl für Studenten, Tüftler, in der Regel Personen, die die Programmierung vertraut sind. Sie können auch eine gute Wahl für Entwickler sein, die Erfahrung mit nicht zu ASP.NET Web-Technologien verfügen.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Benötige ich für die Arbeit mit Webseiten WebMatrix?

Nein. WebMatrix wird nicht mehr als eine integrierte Entwicklungsumgebung für ASP.NET Web Pages empfohlen. Verwendung [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) oder [Visual Studio-Code](https://code.visualstudio.com/).

Wenn Sie nicht Visual Studio oder Visual Studio-Code verwenden möchten, können Sie die Komponente Produkte einzeln mit installieren [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Sie benötigen die folgenden Produkte:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (wodurch das ASP.NET Web Pages-Framework installiert wird)
- IIS Express (Webserver)
- Microsoft SQL Server Compact 4.0 (Datenbank)

Verwenden Sie einen Text-Editor, um bearbeiten *cshtml* (oder *vbhtml*) Seiten.

Verwalten von SQL Server Compact-Datenbanken (*.sdf* Dateien), ohne ein Tool etwas schwieriger ist. Visual Studio Containds Tools zum Verwalten von *.sdf* Datenbanken. Sie können auch SQL-Befehle im Code zur Ausführung zahlreicher SQL Server-Management-Aufgaben ausführen.

So testen Sie *cshtml* Seiten ohne eine integrierte Entwicklungsumgebung (IDE) verwenden, können Sie diese bereitstellen auf einem live-Server. (Siehe [können eine ASP.NET Web Pages-Website ohne mithilfe von WebMatrix bereitstellen?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Ausführen von IIS Express ohne Verwendung einer IDE

Bei der Installation von IIS Express auf dem Computer als Webserver können, der Sie die Seiten testen. Sie können IIS Express über die Befehlszeile ausführen und eine bestimmte Portnummer zuordnen. Sie klicken Sie dann diesen Port angeben, wenn Sie anfordern *cshtml* Dateien in Ihrem Browser.

Klicken Sie in Windows öffnen eine Eingabeaufforderung mit Administratorrechten, und ändern Sie in *c:\Programme\Microsoft Files\IIS Express.* (Für 64-Bit-Systemen verwenden Sie den Ordner *C:\Program Files (x86) \IIS Express.)* Geben Sie dann den folgenden Befehl ein, mit dem tatsächlichen Pfad zu Ihrer Website:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Sie können jede beliebige Portnummer verwenden, die bereits von einem anderen Prozess reserviert ist nicht. (Portnummern, die über 1024 sind in der Regel kostenlos.) Für die `path` Wert, verwenden Sie den Pfad des Website-Ordners, in dem die *cshtml* Dateien sind.

Nach dem Ausführen dieses Befehls IIS Express für Einrichten Ihrer Seiten können Sie einen Browser öffnen und navigieren Sie zu einem *cshtml* Datei. Verwenden Sie eine URL wie folgt:

`http://localhost:35896/default.cshtml`

Geben Sie Informationen zu IIS Express-Befehlszeilenoptionen, `iisexpress.exe /?` in der Befehlszeile.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Kann ich die ASP.NET Web Forms-Steuerelemente auf einer Web Pages-Seite verwenden?

Nein. Web Forms-Steuerelemente, z. B. die [Kontrollkästchen](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.checkbox) -Steuerelement, das [Validierungssteuerelemente](https://msdn.microsoft.com/en-us/library/bwd43d0x), und die [GridView](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview) steuern, nur die Arbeitsaufgaben in Web Forms-Seiten (*aspx* Dateien). Diese Steuerelemente erfordern das Web Forms-Seite-Framework.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Kann ich eine ASP.NET Web Pages-Website bereitstellen, ohne mithilfe von WebMatrix?

Ja. Sie können die Websitedateien manuell auf einem Server kopieren, (in der Regel mithilfe von FTP). Wenn Sie eine manuelle Kopie ausführen, müssen Sie auch die Dateien zu kopieren, die SQL Server Compact (Datenbank) unterstützen. Weitere Informationen finden Sie im Blogeintrag [Web Pages Bereitstellen von Anwendungen ohne ein Tool](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>Habe ich das WebSecurity-Hilfsprogramm verwenden, um Anmeldungen zu unterstützen?

Nein. Die `SimpleMembership` Anbieter, die Teil von ASP.NET Web Pages ist eine Option. Die Sicherheitsanbieter, die Teil von ASP.NET sind (die Sie verwendet werden können, arbeiten mit in Web Forms) sind ebenfalls verfügbar. Z. B. können Formularauthentifizierung in ASP.NET Web Pages Sie genau wie in Web Forms. Ein Beispiel zum Verwenden der Formularauthentifizierung finden Sie unter den Microsoft Support-Artikel [implementieren formularbasierter Authentifizierung in Ihrer ASP.NET-Anwendung erfolgt mithilfe von C# .NET](https://support.microsoft.com/kb/301240). Um ein einfaches Beispiel herunterzuladen, rufen Sie [ASP.NET-Version "Login &amp; Kennwort](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Informationen zur Verwendung von Windows-Authentifizierung finden Sie im Blogbeitrag [mithilfe der Windows-Authentifizierung in ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Unterstützt die ASP.NET Web Pages HTML5?

Ja. Die Seiten, die Sie mit ASP.NET Web Pages erstellen (*cshtml* oder *vbhtml* Seiten) sind im Wesentlichen HTML-Seiten, die auch Code enthalten, die auf dem Server ausgeführt wird, bevor die Seite gerendert wird. Solange der Browser des Benutzers HTML5 unterstützt, können Sie HTML5-Elemente in einem *cshtml* oder *vbhtml* Seite.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Kann ich JavaScript und jQuery mit Webseiten verwenden?

Unbedingt. Die Seiten, die Sie mit ASP.NET Web Pages erstellen (*cshtml* oder *vbhtml* Seiten) werden nur HTML-Seiten mit Servercode darin. Aus diesem Grund können erfolgt in einer normalen HTML-Seite, indem mithilfe von JavaScript oder jQuery Sie können auch nichts einer *cshtml* oder *vbhtml* Seite.

Die **Starter Site** Vorlage in WebMatrix enthält eine Reihe von jQuery-Bibliotheken. Wenn Sie eine Website erstellen, mit der Vorlage der *Skripts* Ordner enthält eine jQuery-Kernbibliothek (*Jquery-1.6.2.js)* und Bibliotheken für jQuery-Validierung (*jquery.validate.js*usw..).

Hier sind einige Blogbeiträge, die die Verwendungsmöglichkeiten von jQuery mit ASP.NET Web Pages veranschaulichen:

- [Hinzufügen von jQuery Eignung für ASP.NET Web Pages mithilfe von WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) von Rachel Appel
- [5 Minuten: WebMatrix + jQuery UI Json + jQuery Vorlagen](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) von Jonas Eriksson
- [WebMatrix und jQuery-Formulare](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) von Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


[ASP.NET Web Pages (Razor) Handbuch zur Problembehandlung](https://go.microsoft.com/fwlink/?LinkId=253001)

[WebMatrix und ASP.NET Web Pages-Forum](https://forums.asp.net/1224.aspx/1?WebMatrix) auf der ASP.NET-Website
