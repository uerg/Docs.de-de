---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft-Dokumentation
author: rick-anderson
description: Dieses Dokument beschreibt die Version von ASP.NET MVC 4.
ms.author: aspnetcontent
ms.date: 09/09/2011
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: 9e4d73349c4c01e0717aabe7da9cb126d2c545c0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826573"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Dieses Dokument beschreibt die Version von ASP.NET MVC 4.


- [Installationshinweise](#_Toc303253802)
- [Dokumentation](#_Toc303253803)
- [Unterstützung](#_Toc303253804)
- [Softwareanforderungen](#_Toc303253805)
- [Neue Features in ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET-Web-API](#_Toc317096197)
    - [Verbesserungen bei der Standard-Projektvorlagen](#_Toc303253808)
    - [Mobile-Projektvorlage](#_Toc303253809)
    - [Anzeigemodi](#_Toc303253810)
    - [jQuery Mobile, der Ansichtumschaltung und Browser überschreiben](#_Toc303253811)
    - [Task-Unterstützung für asynchrone Controller](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Datenbankmigrationen](#_Toc303253818)
    - [Leere Projektvorlage](#_Toc303253819)
    - [Hinzufügen des Controllers, einem Projektordner](#_Toc303253820)
    - [Bündelung und Minimierung](#_Toc303253821)
    - [Aktivieren Anmeldungen von Facebook und andere Standorte mithilfe von OAuth und OpenID](#_Toc303253822)
- [Aktualisieren eines ASP.NET MVC 3-Projekts zu ASP.NET MVC 4](#_Toc303253806)
- [Änderungen von ASP.NET MVC 4-Release Candidate](#_Toc303253817)
- [Bekannte Probleme und aktueller Änderungen](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Installationshinweise

ASP.NET MVC 4 für Visual Studio 2010 installiert werden können, aus der [ASP.NET MVC 4-Startseite](../mvc/mvc4.md) mithilfe des Webplattform-Installers.

Es wird empfohlen, deinstallieren alle zuvor installierten Preview-Versionen von ASP.NET MVC 4 vor der Installation von ASP.NET MVC 4. Sie können die ASP.NET MVC 4 Beta und Release Candidate zu ASP.NET MVC 4 ohne Deinstallation aktualisieren.

Diese Version ist nicht kompatibel mit jeder Preview-Versionen von .NET Framework 4.5. Sie müssen separat die alle installierten Preview-Versionen von .NET Framework 4.5 auf die endgültige Version vor der Installation von ASP.NET MVC 4 aktualisieren.

ASP.NET MVC 4 können installiert und zur Seite-an-Seite mit ASP.NET MVC 3 sein.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentation

Dokumentation für ASP.NET MVC ist auf der MSDN-Website unter der folgenden URL verfügbar:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Lernprogramme und Weitere Informationen zu ASP.NET MVC finden Sie auf der MVC 4-Seite der ASP.NET-Website ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Unterstützung

ASP.NET MVC 4 wird vollständig unterstützt. Wenn Sie Fragen zum Arbeiten mit dieser Version haben auch bereitstellen können, das ASP.NET MVC-Forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), wobei Mitglieder der ASP.NET-Community informelle Unterstützung bieten. oftmals können sind.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Softwareanforderungen

Die ASP.NET MVC 4-Komponenten für Visual Studio erfordern PowerShell 2.0 und Visual Studio 2010 mit Service Pack 1 oder Visual Web Developer Express 2010 mit Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Neue Features in ASP.NET MVC 4

In diesem Abschnitt wird beschrieben, Funktionen, die eingeführt wurden, in der ASP.NET MVC 4-Version.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET-Web-API

ASP.NET MVC 4 umfasst ASP.NET Web-API, ein neues Framework zum Erstellen von HTTP-Diensten, die eine Vielzahl von Clients einschließlich Browsern und mobilen Geräten erreichen kann. ASP.NET Web-API ist auch eine ideale Plattform zum Erstellen von RESTful-Dienste.

ASP.NET Web-API unterstützt die folgenden Funktionen:

- **Moderne HTTP-Programmiermodell:** direkten Zugriff auf und Bearbeiten von HTTP-Anforderungen und Antworten in Ihre Web-APIs, die mit einer neuen, stark typisierte HTTP-Objektmodell. Das gleiche Programmiermodell sowie HTTP-Pipeline ist symmetrisch verfügbar ist, auf dem Client über die neue *"HttpClient"* Typ.
- **Vollständige Unterstützung für Routen:** ASP.NET Web-API unterstützt den vollständigen Satz von Route-Funktionen von ASP.NET-Routing, einschließlich Routenparametern und Einschränkungen. Darüber hinaus verwenden Sie einfache Konventionen, um Aktionen auf HTTP-Methoden zuzuordnen.
- **Inhaltsaushandlung:** Client und Server zusammenarbeiten, um das richtige Format für eine Web-API zurückgegebenen Daten zu bestimmen. ASP.NET Web-API bietet standardmäßig Unterstützung für XML, JSON und Formate Formular-URL-codiert und Sie können diese Unterstützung durch Hinzufügen Ihrer eigenen Formatierer zu erweitern oder sogar ersetzen die Standardstrategie für die Aushandlung von Inhalten.
- **Modellbindung und Validierung:** Modellbinder bieten eine einfache Möglichkeit zum Extrahieren von Daten aus verschiedenen Teilen einer HTTP-Anforderung und konvertiert die Nachrichtenteile in .NET-Objekte auf die durch die Web-API-Aktionen verwendet werden kann. Action-Parameter, die anhand von datenanmerkungen wird auch Validierung durchgeführt.
- **Filter:** ASP.NET Web-API unterstützt die Filter, einschließlich bekannter Filter wie z. B. die *[Authorize]* Attribut. Sie erstellen und schließen Sie Ihren eigenen Filter für Aktionen, Autorisierung und Behandlung von Ausnahmen.
- **Der abfragekomposition:** verwenden die *[Queryable]* Standardsicherheitsfilter-Attribut auf eine Aktion, die gibt *"IQueryable"* Aktivieren der Unterstützung für Ihre Web-API über den Konventionen der OData-Abfrage Abfragen.
- **Verbesserte Prüfbarkeit:** statt HTTP-Details in statischen Kontext-Objekten, web-API-Aktionen-arbeiten mit Instanzen von *HttpRequestMessage* und *HttpResponseMessage*. Erstellen Sie ein Komponententestprojekt zusammen mit Ihrer Web-API-Projekt für den Einstieg schnell Schreiben von Komponententests für Ihre Web-API-Funktionen.
- **Codebasierte Konfiguration:** ASP.NET Web-API-Konfiguration erfolgt ausschließlich über Code, lassen Ihre Konfiguration Dateien zu bereinigen. Verwenden Sie die bereitgestellte Service Locator-Muster, um Erweiterungspunkte zu konfigurieren.
- **Verbesserte Unterstützung für Inversion of Control (IoC) Container:** ASP.NET Web-API bietet hervorragende Unterstützung für IoC-Container über eine verbesserte Abhängigkeit Resolver-Abstraktion
- **Self-Hosting:** Web-APIs kann in Ihrem eigenen Prozess neben IIS gehostet werden, während Sie immer noch das volle Potenzial von Routen und anderen Funktionen von Web-API.
- **Erstellen Sie benutzerdefinierte Hilfe und Testen von Seiten:** Sie jetzt ganz einfach benutzerdefinierte Hilfe zu erstellen und testen können Seiten für Ihre Web-APIs mithilfe des neuen *IApiExplorer* Service um eine vollständige Laufzeit-Beschreibung Ihrer Web-APIs zu erhalten.
- **Überwachung und Diagnose:** ASP.NET Web-API bietet jetzt eine Lightweight-Ablaufverfolgung-Infrastruktur, die es einfach macht, die mit vorhandenen protokollierungslösungen wie System.Diagnostics, ETW- und Protokollierungsframeworks von Drittanbietern integrieren. Sie können die Ablaufverfolgung aktivieren, durch die Bereitstellung einer *ITraceWriter* Implementierung und Ihrer Web-API-Konfiguration hinzufügen.
- **Link generieren:** der ASP.NET Web-API *UrlHelper* zum Generieren von Links zu verwandten Ressourcen in der gleichen Anwendung.
- **Web-API-Projektvorlage:** wählen Sie das neue Formular für Web-API-Projekt der neue MVC 4-Projekt-Assistent, schnell einsatzbereit mit ASP.NET Web-API.
- **Gerüstbau:** verwenden die **Controller hinzufügen** Dialogfeld, um schnell erstellen des Gerüsts für eines Web-API-Controllers basierend auf einem Entity Framework-basierten Typ des Modells.

Weitere Informationen zu ASP.NET Web-API finden Sie unter [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Verbesserungen bei der Standard-Projektvorlagen

Die Vorlage, die zum Erstellen der neuen ASP.NET MVC 4-Projekte wurde aktualisiert, um einen moderner aussehenden-Website zu erstellen:

![](mvc4-release-notes/_static/image1.png)

Zusätzlich zu den Verbesserungen kosmetischer Natur hat es in der neuen Vorlage verbessert. Die Vorlage verwendet eine Technik namens adaptives Rendering in aktivierten Desktopbrowsern und mobile Browser ohne Anpassung gut aussehen.

![](mvc4-release-notes/_static/image2.png)

Um adaptive Rendering in Aktion zu sehen, können einen mobilen-Emulator verwenden oder Ändern der Größe des desktop-Browser-Fensters, um kleinere es einfach ausprobieren. Wenn das Browserfenster klein genug erhält, wird das Layout der Seite ändern.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Mobile-Projektvorlage

Wenn Sie ein neues Projekt beginnen und einen Standort speziell für mobile Geräte und Tablets erstellen möchten, können Sie die neue Projektvorlage für die Mobile-Anwendung. Dieser Wert basiert auf jQuery Mobile, ein Open Source-Bibliothek für touchbedienung optimierten Benutzeroberfläche zu erstellen:

![](mvc4-release-notes/_static/image3.png)

Diese Vorlage enthält die gleiche Anwendungsstruktur wie Internet Application-Vorlage (und der controllercode ist nahezu identisch), aber es ist ein Format mit jQuery Mobile gut aussehen und Verhalten sich auch auf mobilen Geräten touchbasierte. Weitere Informationen finden Sie Informationen zum Strukturieren und mobilen Benutzeroberfläche formatieren, finden Sie unter den [mobilen jQuery-Projektwebsite](http://jquerymobile.com/).

Wenn bereits eine Desktop-orientierten-Website, für Mobilgeräte optimierten Ansichten auf hinzugefügt werden soll, oder wenn Sie einen einzelnen Standort erstellen möchten, die unterschiedlich formatierte Ansichten für Desktop- und mobile Browser dient, können Sie den neuen Anzeigemodi. (Siehe nächsten Abschnitt.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Anzeigemodi

Die neuen Anzeigemodi kann es sich um eine Anwendung, die Ansichten, je nach Browser auswählen, die die Anforderung stammt. Wenn ein desktop-Browser auf der Startseite anfordert, kann die Anwendung z. B. die Views\Home\Index.cshtml-Vorlage verwenden. Wenn auf der Startseite von ein mobiler Browser angefordert wird, kann die Anwendung die Views\Home\Index.mobile.cshtml Vorlage zurück.

Layouts und Teilansichten können auch für bestimmte Browsertypen überschrieben werden. Zum Beispiel:

- Wenn Ihr Ordner "Views\Shared" enthält die \_Layout.cshtml und \_Layout.mobile.cshtml Vorlagen, wird die Anwendung standardmäßig \_Layout.mobile.cshtml während der Anfragen von mobilen Browsern und \_Layout.cshtml, während andere Anforderungen.
- Wenn ein Ordner enthält \_MyPartial.cshtml und \_MyPartial.mobile.cshtml, die Anweisung @Html.Partial("\_MyPartial") rendert \_MyPartial.mobile.cshtml während der Anforderungen von Mobile Browser und \_MyPartial.cshtml, während andere Anforderungen.

Wenn Sie spezifischere Ansichten, Layouts und Teilansichten für andere Geräte erstellen möchten, können Sie ein neues registrieren *DefaultDisplayMode* Instanz zum Angeben der um zu suchenden, wenn eine Anforderung bestimmte Bedingungen erfüllt. Beispielsweise können Sie den folgenden Code hinzu Hinzufügen der *Anwendung\_starten* -Methode in der Datei "Global.asax", um die Zeichenfolge "iPhone" als einen Anzeigemodus zu registrieren, die gilt, wenn Sie der Apple iPhone-Browser eine Anforderung sendet:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Nachdem dieser Code ausgeführt, wenn ein Apple iPhone-Browser eine Anforderung sendet, verwendet Ihre Anwendung die Views\Shared\\_Layout.iPhone.cshtml Layout (falls vorhanden). Weitere Informationen zu den Anzeigemodus befindet, finden Sie unter [Features von ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Mithilfe von DisplayModeProvider Clientanwendungen sollten installieren, die [festen DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet-Paket. Die [ASP.NET Herbst 2012-Update](https://go.microsoft.com/fwlink/?LinkID=271322) enthält die [festen DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet-Paket in die neuen Projektvorlagen. Finden Sie unter [ASP.NET MVC 4 Mobile Zwischenspeichern Fehler Fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) Details für diese Korrektur.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile und Mobile-Features

Informationen zum Erstellen von mobilen Anwendungen mit ASP.NET MVC 4 mithilfe von jQuery Mobile, finden Sie im Tutorial [Features von ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Task-Unterstützung für asynchrone Controller

Sie können jetzt schreiben asynchroner Aktionsmethoden als einzelne Methoden, die ein Objekt des Typs zurückgeben *Aufgabe* oder *Aufgabe&lt;ActionResult&gt;*.

 Weitere Informationen finden Sie unter [mithilfe von asynchronen Methoden in ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 unterstützt die 1.6 und neueren Versionen von Windows Azure SDK.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Datenbankmigrationen

ASP.NET MVC 4-Projekte umfassen jetzt Entity Framework 5. Eines der großartigen Features in Entity Framework 5 ist Unterstützung für datenbankmigrationen. Dieses Feature können Sie Ihr Datenbankschema mit einer Migration codeorientiert Beibehaltung der Daten in der Datenbank auf einfache Weise entwickeln. Weitere Informationen zu datenbankmigrationen, finden Sie unter [Hinzufügen eines neuen Felds zum Modell "Movie" und zur Tabelle](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) in die [Einführung in ASP.NET MVC 4-Tutorial](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Leere Projektvorlage

Die leeren MVC-Projektvorlage ist jetzt wirklich leer, damit Sie von Grund auf völlig neu beginnen können. Die frühere Version von die leere Projektvorlage wurde in einen Basisdatenträger umbenannt.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Hinzufügen des Controllers, einem Projektordner

Sie können jetzt mit der rechten Maustaste und wählen Sie **Controller hinzufügen** aus einem beliebigen Ordner in Ihrem MVC-Projekt. Dies bietet Ihnen mehr Flexibilität, um Ihre Controller zu organisieren, aber Sie, einschließlich Ihrer MVC und Web-API-Controller in separaten Ordnern zu speichern möchten.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Bündelung und Minimierung

Das Framework Bündelung und Minimierung können Sie die Anzahl der HTTP-Anforderungen zu reduzieren, die eine Webseite durch Kombinieren von einzelnen Dateien in einer einzelnen, gebündelte Datei für Skripts und CSS-vornehmen muss. Sie können dann die Gesamtgröße dieser Anforderungen reduzieren, durch Minimierung der Inhalt des Pakets. Minimieren kann Aktivitäten wie das Entfernen von Leerraum kürzen Variablennamen an, reduzieren auch CSS-Selektoren, die auf der Grundlage ihrer Semantik enthalten. Pakete deklariert und konfiguriert Code und sind einfach auf die verwiesen wird in Ansichten über Hilfsmethoden, die entweder generieren kann, die dem Paket eines einzelnen Links oder, beim Debuggen von mehreren links, auf den einzelnen Inhalt des Pakets. Weitere Informationen finden Sie unter [Bündelung und Minimierung](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Aktivieren Anmeldungen von Facebook und andere Standorte mithilfe von OAuth und OpenID

Die Standardvorlagen in ASP.NET MVC 4 Internet-Projektvorlage und bietet nun Unterstützung für OAuth und OpenID melden Sie sich mit den DotNetOpenAuth-Bibliothek aus. Weitere Informationen zur Konfiguration einer OAuth- oder OpenID-Anbieters finden Sie unter [OAuth-/OpenID-Unterstützung für WebForms, MVC und Webseiten](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) und [OAuth und OpenID-feature in ASP.NET Web Pages-Dokumentation](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Aktualisieren eines ASP.NET MVC 3-Projekts zu ASP.NET MVC 4

ASP.NET MVC 4 kann auf demselben Computer ausführen, wodurch Sie die Flexibilität bei der Entscheidung, wann zum Aktualisieren einer ASP.NET MVC 3-Anwendung zu ASP.NET MVC 4, parallel zu ASP.NET MVC 3 installiert werden.

Die einfachste Möglichkeit, ein upgrade ist, um ein neues ASP.NET MVC 4-Projekt und die Kopie zu erstellen. alle Ansichten, Controller, Code und Inhalt von Dateien aus dem vorhandenen MVC 3-Projekt in das neue Projekt, und klicken Sie dann zum Aktualisieren der Assemblys verweist, in das neue Projekt entsprechend der einzelnen nicht-MVC-Vorlagen in Baugruppen inspirieren, die Sie verwenden. Wenn Sie Änderungen an der Datei "Web.config" in das MVC 3-Projekt vorgenommen haben, müssen Sie diese Änderungen auch in der Datei "Web.config" in der MVC 4-Projekts zusammenführen.

Um eine vorhandene ASP.NET MVC 3-Anwendung auf Version 4 manuell zu aktualisieren, führen Sie folgende Schritte aus:

1. Ersetzen Sie jede Instanz von den folgenden Text in alle "Web.config"-Dateien im Projekt (es ist eine im Stammverzeichnis des Projekts im Ordner "Views" und im Ordner "Views" für die einzelnen Bereiche in Ihrem Projekt vorhanden), (Hinweis: System.Web.WebPages, Version = 1.0.0.0 in nicht gefunden Projekte mit Visual Studio 2012 erstellt wurden): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    mit dem folgenden entsprechenden Text:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. Aktualisieren Sie in der Stammdatei "Web.config" der *WebPages:Version* Element in "2.0.0.0", und fügen Sie einen neuen *PreserveLoginUrl* Schlüssel, der den Wert "True" hat: 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Verweise, und wählen Sie die NuGet-Pakete verwalten. Wählen Sie im linken Bereich **Online\NuGet offizielle Paketquelle**, aktualisieren Sie dann Folgendes:

    - ASP.NET MVC 4
    - (Optional) jQuery, jQuery-Validierung und jQuery-Benutzeroberfläche
    - (Optional) Entitätsframework
    - (Optonal) Modernizr
4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Namens des Projekts, und wählen Sie dann auf Projekt entfernen. Klicken Sie dann mit der rechten Maustaste erneut auf des Namens, und wählen Sie bearbeiten *ProjectName*csproj.
5. Suchen Sie die *ProjectTypeGuids* Element, und Ersetzen Sie {E53F8FEA-EAE0-44A6-8774-FFD645390401} mit {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Die Änderungen zu speichern, schließen Sie die Projektdatei (.csproj), die Sie bearbeitet haben, mit der rechten Maustaste in des Projekts und wählen Sie dann das Projekt erneut laden.
7. Wenn das Projekt Drittanbieter Bibliotheken, die mit früheren Versionen von ASP.NET MVC kompiliert werden verweist, öffnen Sie die Stammdatei "Web.config", und fügen Sie die folgenden drei *BindingRedirect* Elemente unter dem  *Konfiguration* Abschnitt: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Änderungen von ASP.NET MVC 4-Release Candidate

Die Versionshinweise für ASP.NET MVC 4-Release Candidate finden Sie hier:

Die wichtigsten Änderungen in ASP.NET MVC 4-Release Candidate in dieser Version werden unten zusammengefasst:

- **Pro Controller-Konfiguration:** ASP.NET Web API Controller können ein benutzerdefiniertes Attribut, das implementiert zugeschrieben werden *IControllerConfiguration* eigene Formatierer, aktionsselektor und Parameter-Binder einrichten . Die *HttpControllerConfigurationAttribute* wurde entfernt.
- **Pro Route-Meldungshandler:** können Sie jetzt den Handler für die endgültige Nachricht angeben, in der Anforderungskette einer bestimmten Route. Dies ermöglicht die Unterstützung für die Fahrt auf Frameworks zu routing verwenden, um mit ihrer eigenen zu senden (nicht -*IHttpController*) Endpunkte.
- **Status von Benachrichtigungen:** der *ProgressMessageHandler* generiert eine statusbenachrichtigung für anforderungsentitäten, die hochgeladen und heruntergeladen werden. Mithilfe dieser Handler ist es möglich, behalten Sie verfolgen, wie weit Sie einen Anforderungstext hochladen oder einen Antworttext downloaden.
- **Pushübertragung von Inhalten:** der *PushStreamContent* Klasse ermöglicht Szenarien, in denen ein datenproduzent direkt in die Anforderung oder Antwort (entweder synchron oder asynchron) mithilfe eines Datenstroms schreiben möchte. Wenn die *PushStreamContent* bereit, Daten zu akzeptieren, um einen Action-Delegat, mit dem Ausgabedatenstrom heraus aufgerufen, wird. Der Entwickler kann dann in den Stream schreiben, für je nach Bedarf, und schließen so lange der Stream beim Schreiben von abgeschlossen wurde. Die *PushStreamContent* erkennt das Schließen des Streams und schließt den zugrunde liegenden asynchronen *Aufgabe* für das Schreiben des Inhalts.
- **Erstellen von Fehlerantworten:** verwenden die *HttpError* Typ, Fehlerinformationen aus wie Fehler und Ausnahmen während der dennoch konsistent darstellen der *IncludeErrorDetailPolicy*. Verwenden Sie die neue *CreateErrorResponse* Erweiterungsmethoden, die einfache Erstellung von Fehlerantworten mit *HttpError* als Inhalt. Die *HttpError* Inhalt ist vollständig Content ausgehandelt.
- **MediaRangeMapping entfernt:** Media Type Bereiche werden jetzt von den standardmäßigen Negotiator behandelt.
- **Parameter der standardbindung für Parameter vom einfachen Typ ist jetzt [FromUri]:** In früheren Versionen von ASP.NET Web API, die die Parameter standardbindung für Parameter einfache Typen wurden die modellbindung verwendet wird. Die standardbindung für die Parameter für den Parameter einfache Typen ist jetzt *[FromUri]*.
- **Aktionsauswahl berücksichtigt die erforderlichen Parameter:** Aktionsauswahl in der ASP.NET Web-API wird jetzt nur wählen Sie eine Aktion, wenn alle erforderlichen Parameter, die aus dem URI enthalten sind, bereitgestellt werden. Bereitstellung eines Standardwerts für das Argument in der Signatur der Aktion-Methode kann ein Parameter als optional angegeben werden.
- **Anpassen des HTTP-parameterbindungen:** verwenden die *ParameterBindingAttribute* zum Anpassen von der parameterbindung für eine bestimmte Aktion-Parameter, oder verwenden Sie die *ParameterBindingRules* auf der *HttpConfiguration* parameterbindungen anpassen breiter.
- **Verbesserungen der MediaTypeFormatter:** Formatierer haben jetzt Zugriff auf die vollständige *HttpContent* Instanz.
- **Host-Richtlinienauswahl Pufferung:** implementieren und konfigurieren Sie die *IHostBufferPolicySelector* -Dienst in ASP.NET Web-API können Hosts, um zu bestimmen, die Richtlinie für den Fall, Pufferung verwendet werden kann.
- **Zugriff auf Clientzertifikate in hostagnostische Weise:** verwenden die *GetClientCertificate* Erweiterungsmethode zum Abrufen des bereitgestellten Clientzertifikats aus der Anforderungsnachricht.
- **Content Negotiation-Erweiterbarkeit:** Anpassen der Aushandlung von Inhalten durch Ableiten von der *DefaultContentNegotiator* und überschreiben alle Aspekte der Aushandlung von Inhalten, die Sie möchten.
- **Unterstützung für die Rückgabe von Antworten für 406 nicht akzeptabel:** Sie können nun 406 nicht akzeptabel Antworten in ASP.NET Web-API zurückgeben, wenn ein geeignetes Formatierers nicht, durch das Erstellen gefunden wird einer *DefaultContentNegotiator* mit der *ExcludeMatchOnTypeOnly* Parametersatz zu *"true"*.
- **Lesen von Formulardaten als NameValueCollection oder JToken:** Formulardaten in der URI-Abfragezeichenfolge oder im Anforderungstext als gelesen werden können eine *NameValueCollection* mithilfe der *ParseQueryString* und  *ReadAsFormDataAsync* Erweiterungsmethoden bzw. Sie können auf ähnliche Weise Formulardaten in der URI-Abfragezeichenfolge oder im Anforderungstext als lesen eine *JToken* mithilfe der *TryReadQueryAsJson* und *ReadAsAsync*&lt;T&gt; Erweiterungsmethoden bzw.
- **Verbesserungen der mehrteiligen:** jetzt ist es möglich, Schreiben einer *MultipartStreamProvider* , die auf den Typ der mehrteiligen MIME--Daten, die sie lesen und zeigt das Ergebnis in der optimale Weg, die der Benutzer kann vollständig zugeschnitten sind. Sie können auch einen Post Verarbeitungsschritt verknüpfen, auf die *MultipartStreamProvider* , mit die Implementierung alle verarbeiten will, auf die MIME-Textteilen Post tun können. Z. B. die *MultipartFormDataStreamProvider* Implementierung liest das HTML-Formular Datenbestandteile und fügt sie einer *NameValueCollection* sodass sie problemlos auf vom Aufrufer erhalten werden.
- **Generation Verbesserungen zu verknüpfen:** der *UrlHelper* mehr hängt *HttpControllerContext*. Sie können jetzt zugreifen der *UrlHelper* aus einem beliebigen Kontext, in denen die *HttpRequestMessage* verfügbar ist.
- **Message-Handler Ausführung Reihenfolge ändern:** Message-Handler jetzt ausgeführt werden, in der Reihenfolge, die sie anstelle von in umgekehrter Reihenfolge konfiguriert sind.
- **Hilfsprogramm für die-Meldungshandler: verknüpfen:** der neuen *HttpClientFactory* , die verknüpfen können *DelegatingHandlers* , und erstellen Sie eine *"HttpClient"* mit der gewünschte Pipeline bereit. Außerdem werden Funktionen für: Verknüpfen mit anderen internen Handler (der Standardwert ist *HttpClientHandler*) als auch der Verdrahtung führen Sie bei Verwendung *HttpMessageInvoker* oder einem anderen  *DelegatingHandler* anstelle von *"HttpClient"* als Top-Invoker.
- **Unterstützung für das CDN in ASP.NET Web Optimization:** ASP.NET Web Optimization bietet jetzt Unterstützung für CDN alternative Pfade, die Sie für jede festlegen eine zusätzliche URL bündeln, die auf dieser Ressource auf ein Content Delivery Network verweist. Unterstützung von CDNs können Sie Ihre Skripts und Stilelemente Pakete geografisch näher auf die Endbenutzer Ihrer Webanwendungen zu erhalten. Produktions-apps sollten als Fallback implementieren, wenn das CDN nicht verfügbar ist. Testen Sie das Fallback.
- **ASP.NET Web-API weiterleitet und -Konfiguration verschoben werden, um *WebApiConfig.Register* statische Methode, die Resused im Code sein können.** ASP.NET Web-API-Routen wurden zuvor in hinzugefügt *RouteConfig.RegisterRoutes* zusammen mit der standard-MVC weiterleitet. Die standardmäßige ASP.NET Web-API-Routen und die Konfiguration erfolgt jetzt in einer separaten *WebApiConfig.Register* Methode, um Tests zu erleichtern.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

- **Die RC und RTM-Version von ASP.NET MVC 4 zurückgegeben zwischengespeicherte desktop Sichten nicht ordnungsgemäß, wenn es sich bei mobile Ansichten zurückgegeben werden sollen.**

    - Finden Sie unter [ASP.NET MVC 4 Mobile Zwischenspeichern Fehler festen](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) Details für diese Korrektur. Die Lösung kann installiert werden, aus der [festen DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet-Paket.
- **Wichtige Änderungen in der Razor-Ansichtsengine**. Die folgenden Typen wurden aus entfernt *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Die folgenden Methoden wurden ebenfalls entfernt werden: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Wenn WebMatrix.WebData.dll in das Verzeichnis "/ bin" von einer ASP.NET MVC 4-apps enthalten ist, hat er die URL für die Formularauthentifizierung.** Hinzufügen der Assembly WebMatrix.WebData.dll zu Ihrer Anwendung (z. B. durch Verwendung des Dialogfelds bereitstellbare Abhängigkeiten hinzufügen, wählen Sie "ASP.NET Web Pages mit Razor-Syntax") überschreibt die Anmeldung Umleitung Authentifizierung/Konto/anmelden statt / Konto/anmelden, wie in der Standardeinstellung die ASP.NET MVC-Kontocontroller erwartet wird. Um zu verhindern, dass dieses Verhalten, und verwenden Sie die URL, die bereits im Abschnitt "Authentifizierung" von "Web.config" angegeben wird, können ein AppSetting namens PreserveLoginUrl hinzufügen und auf "true" festlegen: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Der NuGet-Paket-Manager kann beim Installieren von ASP.NET MVC 4 für parallele Installationen von Visual Studio 2010 und Visual Web Developer 2010 zu installieren.** Zum Ausführen von Visual Studio 2010 und Visual Web Developer 2010 parallel zu ASP.NET MVC 4 müssen Sie ASP.NET MVC 4 installieren, nachdem beide Versionen von Visual Studio bereits installiert wurden.
- **Deinstallieren von ASP.NET MVC 4 schlägt fehl, wenn die Voraussetzungen bereits deinstalliert wurden.** Um ordnungsgemäß zu ASP.NET MVC deinstallieren müssen 4you ASP.NET MVC 4 deinstallieren, vor dem Deinstallieren von Visual Studio.
- **Installation von ASP.NET MVC 4, wird die ASP.NET MVC 3 RTM-Anwendungen funktionsuntüchtig.** Release von ASP.NET MVC 3-Anwendungen, die mit der RTM-Version erstellt wurden (nicht mit der [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/download/details.aspx?id=1491) Version) müssen Sie die folgenden Änderungen für die Arbeit Seite-an-Seite mit ASP.NET MVC 4. Beim Erstellen des Projekts ohne diese Updates Ergebnisse Kompilierungsfehler vorzunehmen. 

    **Erforderliche updates**

  1. Fügen Sie in der Stammdatei "Web.config" einen neuen *&lt;"appSettings"&gt;* Eintrag mit dem Schlüssel *WebPages:Version* und den Wert *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Namens des Projekts, und wählen Sie dann auf Projekt entfernen. Klicken Sie dann mit der rechten Maustaste erneut auf des Namens, und wählen Sie bearbeiten *ProjectName*csproj.
  3. Suchen Sie die folgenden Assemblyverweise aus: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Ersetzen sie durch Folgendes:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Speichern Sie die Änderungen zu, schließen Sie die clientprojektdatei (csproj) Datei, die Sie zuvor bearbeitet werden, mit der rechten Maustaste in des Projekts und wählen Sie erneut laden.

- **Ändern ein ASP.NET MVC 4-Projekt in Ziel 4.0, 4.5 aktualisiert nicht den EntityFramework-Assembly-Verweis:** Wenn Sie ein ASP.NET MVC 4-Projekt in 4.0 als Ziel nach abzielt 4.5 Ändern der Verweis auf EntityFramework-Assembly wird immer noch auf Version 4.5. Zum Beheben dieses Problems deinstallieren und Installieren des EntityFramework NuGet-Pakets.
- **"403 Forbidden", wenn eine ASP.NET MVC 4-Anwendung in Azure ausgeführt wird, nach der Änderung soll 4.0, 4.5:** Wenn Sie ein ASP.NET MVC 4-Projekt nach abzielt 4.5 in 4.0 als Ziel zu ändern und in Azure bereitstellen möglicherweise Fehler 403 Verboten zur Laufzeit angezeigt. Zur Umgehung dieses Problems fügen Sie den folgenden "Web.config": `<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 stürzt ab, bei der Eingabe einer "\' in einem Zeichenfolgenliteral in einer Razor-Datei.** Funktioniert umgeben dieses Problems geben Sie zuerst ein schließendes Anführungszeichen des Zeichenfolgenliterals.
- <strong>Navigieren zu &quot;Konto/verwalten&quot; in den Ergebnissen des Internet-Vorlage in einen Laufzeitfehler, CHS, TRK und CHT-Sprachen.</strong> Ändern Sie die Seite Trennen Sie zum Beheben des Problems <em>@User.Identity.Name</em> Wenn man diese Anwendungen als ausschließlichen Inhalt innerhalb der <em>&lt;starken&gt;</em> Tag.
- **In der Azure-Websites werden von Google und LinkedIn-Anbieter nicht unterstützt.** Verwenden Sie alternativen Authentifizierungsanbieter, bei der Bereitstellung auf Azure-Websites.
- **Wenn UriPathExtensionMapping mit IIS 8 Express/IIS verwenden, würden Sie 404 nicht gefunden-Fehler erhalten, wenn Sie versuchen, die die Erweiterung zu verwenden.** Anforderungen an Web-APIs, mit denen der Handler statische Dateien beeinträchtigt *UriPathExtensionMappings*. Legen Sie *RunAllManagedModulesForAllRequests = True* in "Web.config", das Problem zu umgehen.
- **Controller.Execute-Methode wird nicht mehr aufgerufen.** Alle MVC-Controller werden jetzt immer asynchron ausgeführt.
