---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Dieses Dokument beschreibt die Version von ASP.NET MVC 4.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: fb9d2eaa83fe7486279815c21aec204bdfdf122d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Dieses Dokument beschreibt die Version von ASP.NET MVC 4.


- [Hinweise zur installationsnachbereitung](#_Toc303253802)
- [Dokumentation](#_Toc303253803)
- [Unterstützung](#_Toc303253804)
- [Softwareanforderungen](#_Toc303253805)
- [Neue Funktionen in ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET-Web-API](#_Toc317096197)
    - [Verbesserungen an Standardeinstellung-Projektvorlagen](#_Toc303253808)
    - [Mobile-Projektvorlage](#_Toc303253809)
    - [Anzeigemodi](#_Toc303253810)
    - [jQuery Mobile, Ansichtumschaltung und Browser überschreiben](#_Toc303253811)
    - [Task-Unterstützung für asynchrone Controller](#_Toc303253813)
    - [Azure SDK](#_Toc303253814)
    - [Datenbank-Migrationen](#_Toc303253818)
    - [Leere Projektvorlage](#_Toc303253819)
    - [Hinzufügen eines Controllers, einem Projektordner](#_Toc303253820)
    - [Bundling und Minimierung](#_Toc303253821)
    - [Anmeldungen von Facebook und andere Standorte mithilfe von OAuth und OpenID aktivieren](#_Toc303253822)
- [Aktualisieren eines ASP.NET MVC 3-Projekts zu ASP.NET MVC 4](#_Toc303253806)
- [Änderungen von ASP.NET MVC 4 Release Candidate](#_Toc303253817)
- [Bekannte Probleme und aktueller Änderungen](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Hinweise zur installationsnachbereitung

ASP.NET MVC 4 für Visual Studio 2010 installiert werden können, aus der [ASP.NET MVC 4-Startseite](../mvc/mvc4.md) mithilfe von Web Platform Installer.

Wir empfehlen die Deinstallation alle zuvor installierten Previews von ASP.NET MVC 4 vor der Installation von ASP.NET MVC 4. Sie können ASP.NET MVC 4 ohne eine Deinstallation der ASP.NET MVC 4 Beta und Release Candidate aktualisieren.

Diese Version ist nicht kompatibel mit jeder Preview-Versionen von .NET Framework 4.5. Sie müssen die alle installierten Preview-Versionen von .NET Framework 4.5 getrennt aktualisieren, auf die endgültige Version vor der Installation von ASP.NET MVC 4.

ASP.NET MVC 4 können installiert und zur Seite-an-Seite mit ASP.NET MVC 3 sein.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentation

Dokumentation für ASP.NET MVC ist auf der MSDN-Website unter folgender URL verfügbar:

[https://go.Microsoft.com/fwlink/?LinkId=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Lernprogramme und Weitere Informationen zu ASP.NET MVC sind auf die MVC 4-Seite der ASP.NET-Website verfügbar ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Unterstützung

ASP.NET MVC 4 wird vollständig unterstützt. Wenn Sie Fragen zum Arbeiten mit dieser Version haben auch bereitstellen können, das ASP.NET MVC-Forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), in denen Mitglieder der ASP.NET-Community informelle Unterstützung leisten häufig können sind.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Softwareanforderungen

Die ASP.NET MVC 4-Komponenten für Visual Studio erfordern PowerShell 2.0 und Visual Studio 2010 mit Service Pack 1 oder Visual Web Developer Express 2010 mit Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Neue Funktionen in ASP.NET MVC 4

Dieser Abschnitt beschreibt die Funktionen, die eingeführt wurden in der ASP.NET MVC 4-Version.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET-Web-API

ASP.NET MVC 4 enthält ASP.NET Web-API, ein neues Framework zum Erstellen von HTTP-Diensten, die eine Breite Palette von Clients einschließlich Browsern und mobilen Geräten erreichen können. ASP.NET-Web-API ist auch eine ideale Plattform zum Erstellen von RESTful-Dienste.

ASP.NET Web-API unterstützt die folgenden Funktionen:

- **Moderne HTTP-Programmiermodell:** direkt zugreifen und diese bearbeiten, die HTTP-Anforderungen und Antworten in Ihrer Web-APIs, die mit einer neuen, stark typisierte HTTP-Objektmodell. Dasselbe Programmiermodell und HTTP-Pipeline ist symmetrisch verfügbar ist, auf dem Client über die neue *"HttpClient"* Typ.
- **Vollständige Unterstützung für Routen:** ASP.NET Web-API unterstützt den vollständigen Satz von Route-Funktionen von ASP.NET-Routing, einschließlich Routenparameter und Einschränkungen. Verwenden Sie darüber hinaus einfache Benennungskonventionen, Zuordnen von Aktionen zu HTTP-Methoden.
- **Inhalts-Aushandlung:** Client und Server zusammenarbeiten, um zu bestimmen, das richtige Format für Daten, die von einer Web-API zurückgegeben wird. ASP.NET Web-API bietet standardmäßig Unterstützung für XML, JSON und Formate Formular URL-codiert und Sie können diese Unterstützung erweitern, indem Sie eine eigene Formatierungsprogramme hinzufügen oder sogar bei der Standardstrategie inhaltsaushandlung ersetzt.
- **Modell Bindung und Validierung:** Modellbinder bieten eine einfache Möglichkeit zum Extrahieren von Daten aus verschiedenen Teilen einer HTTP-Anforderung, und konvertieren die Nachrichtenteile in .NET-Objekte der durch die Web-API-Aktionen verwendet werden kann. Action-Parameter, die basierend auf von datenanmerkungen wird auch Validierung durchgeführt.
- **Filter:** ASP.NET Web-API unterstützt die Filter, einschließlich bekannten Filter wie z. B. die *[Authorize]* Attribut. Sie können erstellen und schließen Sie Ihren eigenen Filter für Aktionen, Autorisierung und Behandlung von Ausnahmen.
- **Der abfragekomposition:** verwenden die *[Queryable]* Standardsicherheitsfilter-Attribut auf eine Aktion, die zurückgibt *IQueryable* zum Aktivieren der Unterstützung für die Abfrage Ihre Web-API über die OData-Konventionen für die Abfrage.
- **Prüfbarkeit verbessert:** statt HTTP-Details im statischen Kontextobjekte festlegen, web-API-Aktionen arbeiten mit Instanzen von *HttpRequestMessage* und *HttpResponseMessage*. Erstellen Sie ein Komponententestprojekt zusammen mit Ihrer Web-API-Projekt für den Einstieg schnell Schreiben von Komponententests für Ihre Web-API-Funktionen.
- **Code-basierte Konfiguration:** ASP.NET Web-API-Konfiguration erfolgt ausschließlich über Code, verlassen der Config Dateien zu bereinigen. Verwenden Sie die bereitgestellten Service Locator-Muster, um Erweiterungspunkte konfigurieren.
- **Verbesserte Unterstützung für Inversion of Control (IoC) Container:** ASP.NET Web-API bietet hervorragende Unterstützung für IoC-Container über eine verbesserte Abhängigkeit Konfliktlöser Abstraktion
- **Selbsthosting:** Web-APIs können in Ihren eigenen Prozess zusätzlich zu den IIS gehostet werden, wobei dennoch die volle Leistung von Routen und anderen Features der Web-API.
- **Benutzerdefinierte Hilfe erstellen und Testen von Seiten:** Sie jetzt einfach benutzerdefinierte Hilfe zu erstellen und testen können Seiten für Ihre Web-APIs mit dem neuen *IApiExplorer* Dienst um eine vollständige Common Language Runtime-Beschreibung der Web-APIs abzurufen.
- **Überwachung und Diagnose:** ASP.NET Web-API bietet jetzt eine Lightweight-Tracing-Infrastruktur, die Integration in vorhandene Protokollierung Lösungen wie System.Diagnostics, ETW und Drittanbieter-Protokollierungsframeworks erleichtert. Sie können die Ablaufverfolgung aktivieren, durch die Bereitstellung einer *ITraceWriter* Implementierung und Ihre Web-API-Konfiguration hinzugefügt.
- **Verknüpfen von Generation:** verwenden Sie die ASP.NET Web API *UrlHelper* zum Generieren von Links zu verwandten Ressourcen in derselben Anwendung.
- **Web-API-Projektvorlage:** wählen Sie das neue Formular für Web-API-Projekt der neue MVC 4-Projekt-Assistent, schnell einsatzbereit mit ASP.NET Web-API.
- **Gerüstbau:** verwenden die **Controller hinzufügen** Dialogfeld, um schnell einen Web-API-Controller basierend auf einem Entity Framework Gerüst basierend Modelltyp.

Weitere Informationen zu ASP.NET Web-API finden Sie hier [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Verbesserungen an Standardeinstellung-Projektvorlagen

Die Vorlage, die zum Erstellen von neuen ASP.NET MVC 4-Projekte wurde aktualisiert, um eine Website mehr Modern-Suche zu erstellen:

![](mvc4-release-notes/_static/image1.png)

Zusätzlich zu den kosmetische Verbesserungen wurde es Funktionalität in der neuen Vorlage verbessert. Die Vorlage verwendet ein Verfahren namens adaptives Rendering in Desktopbrowsern und mobilen Browsern ohne Anpassung gut aussehen.

![](mvc4-release-notes/_static/image2.png)

Um adaptive Rendering in Aktion zu sehen, können Sie mobile-Emulator verwenden, oder versuchen Sie es nur mit dem Ändern der Größe der desktop Browserfenster, um kleiner sein. Das Browserfenster klein genug abruft, ändert das Layout der Seite.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Mobile-Projektvorlage

Wenn Sie ein neues Projekt beginnen und einen Standort speziell für mobile Geräte und Browser Tablet erstellen möchten, können Sie die neue Projektvorlage von Mobile-Anwendung. Dieser Wert basiert auf jQuery Mobile, ein Open Source-Bibliothek für touchbedienung optimierten Benutzeroberfläche zu erstellen:

![](mvc4-release-notes/_static/image3.png)

Diese Vorlage enthält die gleiche Anwendungsstruktur wie der Internet Application-Vorlage (und der controllercode ist im Grunde identisch), es ist jedoch formatiert, jQuery Mobile gut aussehen und Verhalten sich auch auf Touch-basierte mobile Geräte verwenden. Weitere Informationen zur Struktur hinzu und Formatieren von mobile-Benutzeroberfläche finden Sie unter der [jQuery Mobile Projekt Website](http://jquerymobile.com/).

Falls Sie eine Desktop-orientierten Site noch Ansichten für Mobilgeräte optimierte hinzugefügt werden soll, oder einen einzelnen Standort erstellt werden soll, die unterschiedlich formatierte Ansichten für desktop und mobile Browser dient, können Sie die neue Anzeigemodi-Funktion verwenden. (Siehe nächsten Abschnitt.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Anzeigemodi

Das neue Feature zur Anzeigemodi ermöglicht eine Anwendung, die je nach Browser Ansichten auswählen, die die Anforderung stammt. Z. B. kann ein Desktopbrowser auf der Startseite anfordert, die Anwendung die Views\Home\Index.cshtml-Vorlage verwenden. Ein mobilen Browser auf der Startseite "angefordert, kann die Anwendung die Vorlage Views\Home\Index.mobile.cshtml zurückgeben.

Layouts und Replikatsgruppenelemente können auch für bestimmte Browsertypen überschrieben werden. Zum Beispiel:

- Wenn Ordner Views\Shared sowohl enthält die \_Layout.cshtml und \_Layout.mobile.cshtml Vorlagen, standardmäßig die Anwendung \_Layout.mobile.cshtml während der Anfragen von mobilen Browsern und \_Layout.cshtml, während andere Anforderungen.
- Enthält sowohl ein Ordner \_MyPartial.cshtml und \_MyPartial.mobile.cshtml, die Anweisung @Html.Partial("\_MyPartial") gibt die \_MyPartial.mobile.cshtml während Anforderungen von Mobile Browser, und \_MyPartial.cshtml, während andere Anforderungen.

Wenn Sie spezifischere Ansichten, Layouts oder Teilansichten für andere Geräte erstellen möchten, können Sie ein neues registrieren *DefaultDisplayMode* Instanz angeben, den Namen, wenn bestimmte Bedingungen erfüllt, die eine Anforderung für gesucht werden soll. Sie können z. B. den folgenden Code hinzu Hinzufügen der *Anwendung\_starten* Methode in der Datei "Global.asax" die Zeichenfolge "iPhone" als einen Anzeigemodus zu registrieren, die gilt, wenn es sich bei der Apple iPhone-Browser eine Anforderung sendet:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Nachdem dieser Code ausgeführt, wenn ein Apple iPhone-Browser eine Anforderung sendet, verwendet die Anwendung die Views\Shared\\_Layout.iPhone.cshtml-Layout (falls vorhanden). Weitere Informationen zu den Anzeigemodus, finden Sie unter [Features von ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Mithilfe von DisplayModeProvider Clientanwendungen sollten installieren, die [festen DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet-Paket. Die [ASP.NET Herbst 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322) enthält die [festen DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet-Paket in neue Projektvorlagen. Finden Sie unter [ASP.NET MVC 4 Mobile Zwischenspeichern Fehler Fixedd](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) Einzelheiten über die Korrektur.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile und Mobile-Funktionen

Informationen zum Erstellen von mobilen Anwendungen mit ASP.NET MVC 4 mit jQuery Mobile, finden Sie im Lernprogramm [Features von ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Task-Unterstützung für asynchrone Controller

Sie können jetzt schreiben asynchrone Aktionsmethoden als einzelne Methoden, die ein Objekt des Typs zurückgeben *Aufgabe* oder *Aufgabe&lt;ActionResult&gt;*.

 Weitere Informationen finden Sie unter [Verwenden von asynchronen Methoden in ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 unterstützt die 1.6 und neueren Versionen von Windows Azure SDK.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Datenbank-Migrationen

ASP.NET MVC 4-Projekte umfassen jetzt die Entity Framework 5. Eine großartige Funktion in Entity Framework 5 ist die Unterstützung für die Migration der Datenbank. Diese Funktion können Sie problemlos Datenbankschema mit einer Migration codeorientierten Beibehaltung der Daten in der Datenbank zu entwickeln. Weitere Informationen zu Datenbank-Migrationen finden Sie unter [die Film-Modell und die Tabelle ein neues Feld hinzugefügt](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) in der [Einführung in ASP.NET MVC 4-Lernprogramm](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Leere Projektvorlage

Die leere MVC-Projektvorlage ist jetzt tatsächlich leer, damit Sie vollständig vorn beginnen können. Die frühere Version der leere Projektvorlage wurde auf Basic umbenannt.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Hinzufügen eines Controllers, einem Projektordner

Sie können jetzt mit der rechten Maustaste, und wählen Sie **Controller hinzufügen** aus einem beliebigen Ordner in das MVC-Projekt. Dies bietet Ihnen mehr Flexibilität beim Controller jedoch einschließlich MVC und Web-API-Controller in separaten Ordnern aufbewahrt werden sollen, zu organisieren.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Bundling und Minimierung

Die Bündelung und Minimierung-Framework können Sie zum Reduzieren der Anzahl der HTTP-Anforderungen, die eine Webseite zu machen, indem Sie einzelne Dateien in einer einzelnen, gebündelte Datei für Skripts und CSS kombiniert. Sie reduzieren dann die Gesamtgröße des diese Anforderungen, indem weboptimierungsframework den Inhalt des Pakets. Verkleinern von zählen Aktivitäten wie das Eliminieren von Leerzeichen auf die Variablennamen an, sogar Reduzieren von CSS-Selektoren, die auf Grundlage ihrer Semantik verkürzen. Pakete sind deklariert und im konfiguriert und einfach referenziert in Ansichten über Hilfsmethoden, die entweder generieren kann, die dem Paket einen einzelnen Link oder beim Debuggen von mehreren links, auf den einzelnen Inhalt des Pakets. Weitere Informationen finden Sie unter [Bündelung und Minimierung](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Anmeldungen von Facebook und andere Standorte mithilfe von OAuth und OpenID aktivieren

Die Standardvorlagen in ASP.NET MVC 4-Internet-Projektvorlage und bietet nun Unterstützung für OAuth- und OpenID-Anmeldung, die mithilfe der DotNetOpenAuth-Bibliothek. Informationen zur Konfiguration einer OAuth- oder OpenID-Anbieters finden Sie unter [OAuth-/OpenID-Unterstützung für Web Forms, MVC und Webseiten](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) und [OAuth- und OpenID-feature in ASP.NET Web Pages-Dokumentation](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Aktualisieren eines ASP.NET MVC 3-Projekts zu ASP.NET MVC 4

ASP.NET MVC 4 kann parallel zu ASP.NET MVC 3 auf dem gleichen Computer installiert werden die Flexibilität bei der Wahl der Zeitpunkt des Upgrades von einer ASP.NET MVC 3-Anwendung zu ASP.NET MVC 4.

Ist die einfachste Methode zum Aktualisieren, erstellen Sie ein neues ASP.NET MVC 4-Projekt und eine Kopie aller Ansichten, Controller, Code und Inhalt Dateien aus dem vorhandenen MVC 3-Projekt in das neue Projekt, und klicken Sie dann auf Aktualisieren Sie die Assembly verweist, in das neue Projekt entsprechend der einzelnen nicht-MVC-Vorlagen in Baugruppen beinhaltete, die Sie verwenden. Wenn Sie Änderungen an der Datei "Web.config" in der MVC 3-Projekts vorgenommen haben, müssen Sie diese Änderungen auch in der Datei "Web.config" in der MVC 4-Projekt zusammenführen.

Um eine vorhandene ASP.NET MVC 3-Anwendung auf Version 4 manuell zu aktualisieren, führen Sie folgende Schritte aus:

1. Ersetzen Sie jede Instanz von den folgenden Text in alle "Web.config"-Dateien im Projekt (es ist eine im Stammverzeichnis des Projekts, in den Ordner Views, und im Ordner "Ansichten" für die einzelnen Bereiche im Projekt vorhanden), (Hinweis: System.Web.WebPages, Version = 1.0.0.0 befindet sich nicht Projekte mit Visual Studio 2012 erstellt): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    mit dem folgenden entsprechenden Text:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. In der Stammdatei "Web.config" Aktualisieren der *WebPages:Version* Element "2.0.0.0" und fügen Sie einen neuen *PreserveLoginUrl* Schlüssel, der den Wert "True" aufweist: 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Verweise, und wählen Sie die NuGet-Pakete verwalten. Wählen Sie im linken Bereich **Online\NuGet offizielle Paketquelle**, aktualisieren Sie dann Folgendes:

    - ASP.NET MVC 4
    - (Optional) jQuery, jQuery-Validierung und jQuery UI
    - (Optional) Entity Framework
    - (Optonal) Modernizr
4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projektnamens, und wählen Sie dann auf Projekt entfernen. Klicken Sie dann mit der rechten Maustaste erneut auf des Namens, und wählen Sie bearbeiten *Projektname*csproj.
5. Suchen Sie die *ProjectTypeGuids* -Element, und Ersetzen Sie {E53F8FEA-EAE0-44A6-8774-FFD645390401} durch {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Die Änderungen zu speichern, schließen Sie die Projektdatei (csproj), die Sie in Bearbeitung hatten, mit der rechten Maustaste des Projekts und wählen Sie dann Projekt erneut laden.
7. Wenn das Projekt alle Bibliotheken von Drittanbietern, die mit früheren Versionen von ASP.NET MVC kompiliert werden verweist, öffnen Sie die Stammdatei "Web.config", und fügen Sie die folgenden drei *BindingRedirect* Elemente unter dem  *Konfiguration* Abschnitt: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Änderungen von ASP.NET MVC 4 Release Candidate

Die Versionshinweise für ASP.NET MVC 4 Release Candidate finden Sie hier:

Die wichtigsten Änderungen von ASP.NET MVC 4 Release Candidate in dieser Version sind im folgenden zusammengefasst:

- **Pro Controller-Konfiguration:** ASP.NET Web API-Controller können ein benutzerdefiniertes Attribut, das implementiert zugeschrieben werden *IControllerConfiguration* eigene Formatierungsprogramme, aktionsselektor und Parameter Bindern einrichten . Die *HttpControllerConfigurationAttribute* wurde entfernt.
- **Pro Route-Meldungshandler:** können Sie jetzt die endgültige Nachrichtenhandler in der Anforderungskette für eine bestimmte Route angeben. Dies ermöglicht die Unterstützung für fuhr entlang-Frameworks zum routing verwenden, um mit ihrer eigenen zuzuteilen (nicht -*IHttpController*) Endpunkte.
- **Status der Benachrichtigungen:** der *ProgressMessageHandler* generiert eine statusbenachrichtigung für anforderungsentitäten, die hochgeladen und heruntergeladen werden. Mit dieser Handler ist es möglich, Nachverfolgen von wie weit Sie einen Anforderungstext hochladen oder Herunterladen einen Antworttext.
- **Push Inhalte:** der *PushStreamContent* Klasse unterstützt Szenarien, bei denen ein datenproduzent direkt in der Anforderung oder Antwort (synchron oder asynchron) mithilfe eines Datenstroms schreiben möchte. Wenn die *PushStreamContent* kann Daten akzeptieren, es zu einer Action-Delegat, mit dem Ausgabestream aufruft. Der Entwickler kann dann in den Stream geschrieben, für lange je nach Bedarf, und Schließen der Stream beim Schreiben von abgeschlossen wurde. Die *PushStreamContent* erkennt das Schließen des Streams und schließt den zugrunde liegenden asynchronen *Aufgabe* für das Schreiben des Inhalts.
- **Erstellen von Fehlerantworten:** verwenden die *HttpError* Typ, Fehlerinformationen von z. B. Fehler und Ausnahmen während weiterhin berücksichtigt einheitlich darstellen der *IncludeErrorDetailPolicy* . Verwenden Sie die neue *CreateErrorResponse* Erweiterungsmethoden einfache Erstellung von Fehlerantworten mit *HttpError* als Inhalt. Die *HttpError* Inhalt ist vollständig Content ausgehandelt.
- **MediaRangeMapping entfernt:** Medien Typ Bereiche jetzt von der Standardeinstellung inhaltsaushandlungsmodul behandelt.
- **Standard-parameterbindung für einfache Typparameter ist jetzt [FromUri]:** In früheren Versionen von ASP.NET Web-API die standardmäßige Parameter, die Bindung für einfache Typparameter wurden die modellbindung verwendet. Die Standard-parameterbindung für Parameter einfache Typen ist jetzt *[FromUri]*.
- **Aktionsauswahl berücksichtigt erforderlichen Parameter:** Aktionsauswahl in ASP.NET Web-API wird jetzt nur wählen Sie eine Aktion, wenn alle erforderlichen Parameter, die stammen aus dem URI bereitgestellt werden. Ein Parameter kann einen Standardwert für das Argument in der Methodensignatur Aktion bereitstellen als optional angegeben werden.
- **Anpassen von HTTP-parameterbindungen:** verwenden die *ParameterBindingAttribute* verwenden oder Anpassen von die parameterbindung für einen bestimmten Action-Parameter der *ParameterBindingRules* auf die *HttpConfiguration* parameterbindungen anpassen breiterer.
- **MediaTypeFormatter-Verbesserungen:** Formatierungsprogramme haben nun Zugriff auf die vollständige *HttpContent* Instanz.
- **Host Pufferung Richtlinienauswahl:** implementieren und Konfigurieren der *IHostBufferPolicySelector* -Dienst in ASP.NET Web-API zum Aktivieren der Hosts, um zu bestimmen, die Richtlinie für den Fall, Pufferung verwendet werden kann.
- **Clientzertifikate auf hostagnostische Weise darauf zugreifen:** verwenden die *GetClientCertificate* Erweiterungsmethode zum Abrufen des bereitgestellten Clientzertifikats aus der Anforderungsnachricht.
- **Inhalts-Aushandlung-Erweiterbarkeit:** anpassen inhaltsaushandlung durch Ableiten von der *DefaultContentNegotiator* und überschreiben beliebig inhaltsaushandlung, die Sie möchten.
- **Unterstützung für die Rückgabe 406 nicht akzeptabel Antworten:** Sie können jetzt 406 nicht akzeptabel Antworten in ASP.NET Web-API zurückgeben, wenn ein geeigneter Formatierer durch das Erstellen nicht gefunden wird eine *DefaultContentNegotiator* mit der *ExcludeMatchOnTypeOnly* Parametersatz auf *"true"*.
- **Lesen von Formulardaten als NameValueCollection oder JToken:** erfahren Sie Formulardaten in der URI-Abfragezeichenfolge oder im Anforderungstext als ein *NameValueCollection* mithilfe der *ParseQueryString* und  *ReadAsFormDataAsync* Erweiterungsmethoden bzw. Sie können auf ähnliche Weise Formulardaten, die in der URI-Abfragezeichenfolge oder im Anforderungstext als lesen eine *JToken* mithilfe der *TryReadQueryAsJson* und *ReadAsAsync*&lt;T&gt; Erweiterungsmethoden bzw.
- **Mehrteilige Verbesserungen:** ist es jetzt möglich, Schreiben einer *MultipartStreamProvider* , die vollständig in den Typ der mehrteiligen MIME-Daten, lesen und das Ergebnis in die optimale Möglichkeit, die Benutzer zeigt können zugeschnitten ist. Sie können auch einen Post Verarbeitungsschritt verknüpfen, auf die *MultipartStreamProvider* , mit dessen Hilfe der Implementierung, beliebige Post möchte, auf den MIME-Textteilen verarbeiten. Z. B. die *MultipartFormDataStreamProvider* Implementierung liest das HTML-Formular Datenbestandteile und fügt diese einer *NameValueCollection* , sodass sie leicht vom Aufrufer an abzurufen sind.
- **Verknüpfen von Generation Verbesserungen:** der *UrlHelper* mehr richtet sich nach *HttpControllerContext*. Sie können jetzt zugreifen der *UrlHelper* aus einem beliebigen Kontext, in dem der *HttpRequestMessage* ist verfügbar.
- **Message-Handler Ausführung Reihenfolge ändern:** Meldungshandler werden jetzt die Ausführung in der Reihenfolge, die sie in umgekehrter Reihenfolge statt konfiguriert sind.
- **Hilfsprogramm für Meldungshandler verknüpft:** neuen *HttpClientFactory* , die verknüpfen können *DelegatingHandlers* , und erstellen Sie eine *"HttpClient"* mit der gewünschte Pipeline jetzt kann es losgehen. Sie bietet auch Funktionen für verknüpft mit alternativen internen Handler (die Standardeinstellung ist *HttpClientHandler*) sowie die Verkabelung führen Sie bei Verwendung *HttpMessageInvoker* oder ein anderes  *DelegatingHandler* anstelle von *"HttpClient"* als Top-Instanz zum Aufrufen.
- **Unterstützung für CDNs in ASP.NET Web-Optimierung:** ASP.NET Weboptimierung bietet jetzt Unterstützung für alternative aktivieren möchten, geben Sie für jeden CDN-Pfaden eine zusätzliche URL bündeln, die auf die gleiche Ressource in einem Netzwerk für Inhaltsübermittlung verweist. Unterstützende CDNs ermöglicht es Ihnen, Ihre Skripts und des Stils Bündel geografisch näher an den Consumer Ende Ihrer Webanwendungen abgerufen.
- **ASP.NET Web API weiterleitet und Konfiguration verschoben werden, um *WebApiConfig.Register* statische Methode, die im Testcode Resused sein können.** ASP.NET Web API Routen zuvor hinzugefügt wurden *RouteConfig.RegisterRoutes* zusammen mit den standard-MVC weiterleitet. Die Standardeinstellung leitet der ASP.NET Web-API und die Konfiguration in einer separaten jetzt behandelt *WebApiConfig.Register* Methode, um Tests zu vereinfachen.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Bekannte Probleme und aktueller Änderungen

- **Die RC und die RTM-Version von ASP.NET MVC 4 zurückgegeben falsch zwischengespeicherte desktop Ansichten beim mobile Ansichten zurückgegeben werden sollen.**

    - Finden Sie unter [ASP.NET MVC 4 Mobile Zwischenspeichern Fehler festen](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) Einzelheiten über die Korrektur. Die Lösung kann installiert werden, aus der [festen DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet-Paket.
- **Wichtige Änderungen in das Razor-Ansichtsmodul**. Die folgenden Typen von entfernt wurden *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 Die folgenden Methoden wurden ebenfalls entfernt: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Wenn WebMatrix.WebData.dll im Verzeichnis "/ bin" von einer ASP.NET MVC 4-apps in enthalten ist, hat er die URL für die Formularauthentifizierung.** Der Anwendung (z. B. durch "ASP.NET Web Pages mit Razor-Syntax" auswählen, wenn Sie das Dialogfeld "bereitstellbare Abhängigkeiten hinzufügen" verwenden) die WebMatrix.WebData.dll-Assembly hinzufügen, wird die Authentifizierung Anmeldung Umleitung für die Anmeldung/Account/überschrieben statt / Konto/Anmeldenamens, wie erwartet das ASP.NET MVC-Controllers ein Konto in der Standardeinstellung verwendet wird. Um zu verhindern, dass dieses Verhalten, und verwenden Sie die URL, die bereits im Abschnitt "Authentifizierung" der Datei "Web.config" angegeben, können Sie Hinzufügen einer AppSetting PreserveLoginUrl aufgerufen und auf "true" festgelegt haben: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Die NuGet-Paket-Manager nicht installiert werden, beim Versuch, die ASP.NET MVC 4 für parallele Installationen von Visual Studio 2010 und Visual Web Developer 2010 installieren.** Zum Ausführen von Visual Studio 2010 und Visual Web Developer 2010 parallel zu ASP.NET MVC 4 müssen Sie ASP.NET MVC 4 installieren, nachdem beide Versionen von Visual Studio bereits installiert wurden.
- **Deinstallieren von ASP.NET MVC 4 fehlschlägt, wenn die erforderlichen Komponenten bereits deinstalliert wurden.** Um ASP.NET MVC ordnungsgemäß deinstalliert muss 4you ASP.NET MVC 4 Deinstallieren Sie vor dem Deinstallieren von Visual Studio.
- **ASP.NET MVC 4 installieren, wird ASP.NET MVC 3 RTM Anwendungen unterbrochen.** Freigeben von ASP.NET MVC 3-Anwendungen, die mit der RTM-Version erstellt wurden (nicht mit der [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/en-us/download/details.aspx?id=1491) freigeben) benötigen Sie die folgenden Änderungen, um die Seite-an-Seite mit ASP.NET MVC 4 arbeiten. Beim Erstellen des Projekts ohne diese Updates Ergebnisse in Kompilierungsfehler auftreten. 

    **Erforderliche updates**

    1. In der Stammdatei "Web.config", fügen Sie einen neuen  *&lt;"appSettings"&gt;*  Eintrag mit dem Schlüssel *WebPages:Version* und dem Wert *1.0.0.0*. 

        [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
    2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projektnamens, und wählen Sie dann auf Projekt entfernen. Klicken Sie dann mit der rechten Maustaste erneut auf des Namens, und wählen Sie bearbeiten *Projektname*csproj.
    3. Suchen Sie die folgenden Assemblyverweise aus: 

        [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

        Ersetzen Sie sie wie folgt:

        [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
    4. Speichern Sie die Änderungen zu, schließen Sie die Datei clientprojektdatei (csproj), die Sie wurden bearbeiten, und klicken Sie dann mit der rechten Maustaste des Projekts und wählen Sie erneut laden.
- **Ändern ein ASP.NET MVC 4-Projekt in Ziel 4.0, 4.5 aktualisiert sich nicht auf den Assemblyverweis EntityFramework:** Wenn Sie ein ASP.NET MVC 4-Projekt zum Ziel 4.0 nach Anwendung 4.5 der Verweis auf die Assembly EntityFramework zeigen weiterhin ändern auf die Version 4.5. Zum Beheben dieses Problems deinstallieren und das EntityFramework NuGet-Paket neu installieren.
- **"403 Forbidden", wenn eine ASP.NET MVC 4-Anwendung in Azure ausgeführt wird, nach der Änderung Ziel 4.0, 4.5:** Wenn Sie ein ASP.NET MVC 4-Projekt zu Ziel 4.0 nach Anwendung 4.5 ändern, und klicken Sie dann in Azure bereitstellen möglicherweise zur Laufzeit eine 403 Forbidden Fehler angezeigt. Fügen zur Umgehung dieses Problems Folgendes verwenden, um die Datei "Web.config":`<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 stürzt ab, bei der Eingabe einer "\' in einem Zeichenfolgenliteral in einer Razor-Datei.** Funktioniert das Problem geben Sie das schließende Anführungszeichen des Zeichenfolgenliterals zuerst.
- **Navigieren zum &quot;Konto/verwalten&quot; in den Ergebnissen des Internet-Vorlage zu einem Laufzeitfehler, CHS, TRK und CHT Sprachen.** Zum Beheben des Problems ändern Sie die Seite zum Aussondern  *@User.Identity.Name*  verlegen als nur der Inhalt innerhalb der  *&lt;starken&gt;*  Tag.
- **Google und LinkedIn-Anbietern werden im Azure-Websites nicht unterstützt.** Verwenden Sie alternativen Authentifizierungsanbieter, bei der Bereitstellung auf Azure-Websites.
- **Bei Verwendung von UriPathExtensionMapping mit IIS 8 Express/IIS, würden Sie 404 Nichtgefunden-Fehler empfangen, wenn Sie versuchen, die die Erweiterung verwenden.** Der Handler für statische Dateien wirkt sich bei den Anforderungen an Web-APIs, mit denen *UriPathExtensionMappings*. Legen Sie *RunAllManagedModulesForAllRequests = "true"* in "Web.config", um das Problem zu umgehen.
- **Controller.Execute-Methode wird nicht mehr aufgerufen.** Alle MVC-Controller werden jetzt immer asynchron ausgeführt.
