---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: "Auszuführende Aktion nicht in ASP.NET und wie stattdessen vorzugehen ist | Microsoft Docs"
author: tfitzmac
description: "Dieses Thema beschreibt einige häufige Fehler, die Personen innerhalb von ASP.NET-Webprojekten vornehmen. Empfehlungen für was Sie tun sollten, um diese Commo vermeiden..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 6790cd0deb36c9fb297ccd4df371f763dba17844
ms.sourcegitcommit: 17b025bd33f4474f0deaafc6d0447a4e72bcad87
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2017
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Auszuführende Aktion nicht in ASP.NET und wie stattdessen vorzugehen ist
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieses Thema beschreibt einige häufige Fehler, die Personen innerhalb von ASP.NET-Webprojekten vornehmen. Empfehlungen für was Sie tun müssen, um diese häufige Fehler zu vermeiden. Basiert auf einem [Präsentation](http://vimeo.com/68390507) von **Damian Edwards** Conference Norwegisch Entwickler.


## <a name="disclaimer"></a>Haftungsausschluss

In diesem Thema ist nicht als eine vollständige Anleitung vorgesehen, um sicherzustellen, dass Ihre Anwendung sicheren und effizienten ist. Sie müssen noch best Practices für Sicherheit und Leistung folgen, die nicht in diesem Thema beschrieben werden. Es wird nur das Vermeiden von häufigen Fehlern im Zusammenhang mit .NET-Klassen und Prozesse vorgeschlagen.

## <a name="overview"></a>Übersicht

Dieses Thema enthält folgende Abschnitte:

- [Einhaltung von Standards](#standards)

    - [Steuerelementadapter](#adapters)
    - [Stileigenschaften für Steuerelemente](#styleprop)
    - [Seiten- und Steuerelement Rückrufe](#callback)
    - [Erkennung von Browser-Funktion](#browsercap)
- [Sicherheit](#security)

    - [Anforderungsüberprüfung](#validation)
    - [Formularauthentifizierung und Sitzung](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Mittlere Vertrauenswürdigkeit](#medium)
    - [&lt;"appSettings"&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Zuverlässigkeit und Leistung](#performance)

    - [PreSendRequestHeaders und PreSendRequestContent](#presend)
    - [Asynchrone Page-Ereignisse mit WebForms](#asyncevents)
    - [Auslösen und vergessen Arbeit](#fire)
    - [Anforderungs-Einheitstextkörper](#requestentity)
    - [Response.Redirect und Response.End](#redirect)
    - [EnableViewState und ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Anforderungen für den lang ausgeführt (> 110 Sekunden)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Einhaltung von Standards

<a id="adapters"></a>

### <a name="control-adapters"></a>Steuerelementadapter

Empfehlung: Beenden Sie die Verwendung von Steuerelementadapter für adaptives Rendering und stattdessen Sie CSS-Medienabfragen und Standards kompatiblen HTML.

Steuerelemente-Adapter wurden in .NET 2.0 Präsentation Code gerendert, die für verschiedene Geräte und Umgebungen angepasst wurde eingeführt. Nun kann dieser adaptive Rendern HTML und CSS-erreicht werden. Sie sollten nicht mehr verwenden Steuerelementadapter und konvertieren alle vorhandenen Adapter in CSS- und HTML.

Weitere Informationen finden Sie unter [Medienabfragen](http://www.w3.org/TR/css3-mediaqueries/) und [Vorgehensweise: Hinzufügen mobiler Seiten zu Ihren ASP.NET Web Forms / MVC-Anwendung](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Stileigenschaften für Steuerelemente

Empfehlung: Beenden Sie Einstellungswerte in Markup des Steuerelements, und legen Sie stattdessen Formatierung Werte im CSS-Stylesheets.

Webserversteuerelemente enthalten Dutzende von Eigenschaften, die zum Festlegen von Stileigenschaften für in-Line-verwendet werden kann. Beispielsweise legt die ForeColor-Eigenschaft, die Farbe des Texts für ein Steuerelement. Sie können diesen Effekt effizienter durch CSS-Stylesheets erreichen. Stylesheets können Sie zum Zentralisieren der Style-Werte, und vermeiden Sie diese Werte in der gesamten Anwendung festzulegen.

Das folgende Beispiel zeigt einer CSS-Klasse, die Mengen Text in Rot.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

Im nächste Beispiel veranschaulicht die dynamisch anwenden, die CSS-Klasse.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Seiten- und Steuerelement Rückrufe

Empfehlung: Beenden Sie die Verwendung der Seiten- und Rückrufe und stattdessen eine der folgenden: AJAX, UpdatePanel, MVC-Aktionsmethoden, Web-API oder SignalR.

In früheren Versionen von ASP.NET ermöglichte Seiten- und Rückrufmethoden Teil der Webseite zu aktualisieren, ohne eine gesamte Seite zu aktualisieren. Erreichen Sie Aktualisierungen von Teilseiten bis jetzt [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/en-US/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web-API](../../../web-api/index.md) oder [SignalR](../../../signalr/index.md). Beenden Sie Rückrufmethoden verwenden, da Probleme mit friendly URLs verursacht werden können und routing. Standardmäßig Steuerelemente Rückrufmethoden nicht aktivieren, aber wenn Sie diese Funktion in einem Steuerelement aktiviert haben, sollten Sie es deaktivieren.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Erkennung von Browser-Funktion

Empfehlung: Beenden Sie die Verwendung von statischen Browser-Funktion zur Erkennung und stattdessen verwenden Sie dynamische Funktion Erkennung zu.

Die unterstützten Funktionen für jeden Browser wurden in früheren Versionen von ASP.NET in eine XML-Datei gespeichert. Erkennen von Feature-Unterstützung über die statische Suche ist nicht der beste Ansatz. Jetzt, Sie können dynamisch erkennen, ein Browser Features wie z. B. mithilfe einer Funktion zur Erkennung Framework unterstützten des [Modernizr](http://modernizr.com/). Funktion zur Erkennung bestimmt die Unterstützung von versuchen, eine Methode oder Eigenschaft zu verwenden, und klicken Sie dann überprüfen, um festzustellen, ob der Browser das gewünschte Ergebnis hervorgerufen hat. Modernizr ist standardmäßig in der Anwendungsvorlagen enthalten.

<a id="security"></a>

## <a name="security"></a>Sicherheit

<a id="validation"></a>

### <a name="request-validation"></a>Anforderungsüberprüfung

Empfehlung: Validieren von Benutzereingaben und Ausgabe von Benutzern zu codieren.

Anforderungsüberprüfung ist ein Feature von ASP.NET, die untersucht jede Anforderung und die Anforderung beendet, wenn eine erkannten Bedrohung gefunden wird. Hängen Sie nicht davon anforderungsüberprüfung für Ihre Anwendung vor Angriffen durch Cross-Site scripting sichern. Stattdessen überprüfen Sie aller Eingaben von Benutzern und codieren Sie die Ausgabe zu. In manchen Fällen verwenden von regulären Ausdrücken zur Überprüfung der Eingabe, aber in etwas komplizierteren Fällen, die Sie überprüfen sollten Benutzereingaben mithilfe von Klassen in .NET, die bestimmen, ob der Wert übereinstimmt. zulässige Werte.

Das folgende Beispiel zeigt, wie eine statische Methode in der Uri-Klasse verwenden, um zu bestimmen, ob der Uri von einem Benutzer gültig ist.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Allerdings um den Uri ausreichend zu überprüfen, Sie sollten auch überprüfen um sicherzustellen, dass er gibt `http` oder `https`. Im folgenden Beispiel wird Instanzmethoden, um sicherzustellen, dass der Uri gültig ist.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Codieren Sie Benutzereingaben als HTML-rendern oder einschließlich Benutzereingaben in einer SQL-Abfrage, die Werte, um sicherzustellen, dass bösartiger Code nicht enthalten ist.

Sie können HTML codieren Sie den Wert im Markup, mit der &lt;%: %&gt; Syntax, wie unten dargestellt.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

In Razor-Syntax können Sie HTML codieren mit @, wie unten dargestellt.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

Im folgenden Beispiel gezeigt Codieren der HTML wie einen Wert im Code-Behind.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Um einen Wert für die SQL-Befehle sicher zu codieren, verwenden Sie Parameter des Befehls wie z. B. die ["SqlParameter"](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Formularauthentifizierung und Sitzung

Empfehlung: Erfordern Sie Cookies.

Übergeben Authentifizierungsinformationen in der Abfragezeichenfolge ist nicht sicher. Erfordern Sie daher Cookies auf, wenn Ihre Anwendung Authentifizierung enthält. Wenn Ihr Cookie vertraulichen Informationen gespeichert werden, erwägen Sie, erfordern von SSL für das Cookie.

Im folgende Beispiel wird gezeigt, wie in der Datei "Web.config" angeben, dass die Formularauthentifizierung ist einen Cookie erforderlich, der über SSL übermittelt wird.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Empfehlung: Nie festlegen auf "false".

Standardmäßig EnbableViewStateMac festgelegt ist auf "true". Auch wenn die Anwendung kein Ansichtszustand verwendet wird, legen Sie EnableViewStateMac nicht auf "false". Wenn dieser Wert auf "false" wird die Anwendung anfällig für siteübergreifende machen.

ASP.NET 4.5.2 ab, die die Laufzeit erzwingt **EnableViewStateMac = "true"**. Auch wenn Sie auf "false" festlegen, wird die Common Language Runtime ignoriert diesen Wert und mit der festgelegte Wert auf "true" wird fortgesetzt. Weitere Informationen finden Sie unter [ASP.NET 4.5.2 und EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

Im folgende Beispiel wird gezeigt, wie EnableViewStateMac auf "true" festgelegt wird. Sie müssen nicht tatsächlich legen Sie diesen Wert auf "true", da sie in der Standardeinstellung "true" ist. Wenn Sie es auf "false" auf einer beliebigen Seite in der Anwendung festgelegt haben, müssen Sie diesen Wert sofort beheben.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Mittlere Vertrauenswürdigkeit

Empfehlung: Hängen Sie nicht davon mittlere Vertrauensebene (oder einer anderen Ebene der Vertrauenswürdigkeit) als Sicherheitsgrenze.

Teilweiser Vertrauenswürdigkeit schützt Ihre Anwendung nicht angemessen und sollte nicht verwendet werden. Verwenden Sie stattdessen volle Vertrauenswürdigkeit, und nicht vertrauenswürdige Anwendungen in separaten Anwendungspools isolieren. Jeder Anwendungspool unter einer eindeutigen Identität, zudem ausführen. Weitere Informationen finden Sie unter [ASP.NET teilweise Vertrauenswürdigkeit garantiert nicht Anwendungsisolation](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;"appSettings"&gt;

Empfehlung: Deaktivieren Sie die Sicherheitseinstellungen in nicht &lt;"appSettings"&gt; Element.

Das Element "appSettings" enthält viele Werte, die für die Sicherheitsupdates erforderlich sind. Sie sollten nicht ändern oder deaktivieren diese Werte. Wenn Sie diese Werte bei der Bereitstellung eines Updates deaktivieren müssen, sofort aktivieren Sie wieder nach Abschluss der Bereitstellung.

Weitere Informationen finden Sie unter [ASP.NET AppSettings-Element](https://msdn.microsoft.com/en-us/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Empfehlung: Verwenden von [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx) stattdessen.

.NET Framework ein sehr spezifischen Browser Kompatibilitätsproblem behoben wurde die UrlPathEncode-Methode hinzugefügt. Er nicht ordnungsgemäß codiert eine URL und die Anwendung von siteübergreifendem Skripting bietet keinen Schutz. Sie sollten niemals in Ihrer Anwendung verwenden. Verwenden Sie stattdessen [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx).

Im folgende Beispiel wird gezeigt, wie eine codierte URL als ein Abfragezeichenfolgen-Parameter für ein Linksteuerelement übergeben wird.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Zuverlässigkeit und Leistung

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders und PreSendRequestContent

Empfehlung: Verwenden Sie diese Ereignisse nicht mit verwalteten Module an. Schreiben Sie stattdessen ein natives Modul für IIS zum Ausführen der gewünschten Aufgabe. Finden Sie unter [Erstellen von systemeigenem Code HTTP-Modulen](https://msdn.microsoft.com/en-us/library/ms693629.aspx).

Sie können die [PreSendRequestHeaders](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestheaders.aspx) und [PreSendRequestContent](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestcontent.aspx) Ereignisse mit dem systemeigenen IIS-Module.
> [!WARNING]
> Verwenden Sie keine `PreSendRequestHeaders` und `PreSendRequestContent` mit verwalteten Modulen, die implementieren `IHttpModule`. Das Festlegen dieser Eigenschaften kann dazu führen, dass Probleme bei asynchronen Anforderungen. Die Kombination der Anwendung angeforderte Routing (ARR) und Websockets kann zu Ausnahmen für den Verstoß führen, die zum Absturz w3wp verursachen kann. Z. B. Iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 in iiscore.dll verursacht eine Verletzung Zugriffsausnahme (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Asynchrone Page-Ereignisse mit WebForms

Empfehlung: In Web Forms, zu vermeiden, Schreiben asynchrone "void" Methoden für die Seite-Lebenszyklusereignisse und stattdessen [Page.RegisterAsyncTask](https://msdn.microsoft.com/en-us/library/system.web.ui.page.registerasynctask.aspx) für asynchronen Code.

Wenn Sie ein Page-Ereignisklasse mit markieren **Async** und **"void"**, Sie können nicht bestimmen, wann der asynchrone Code beendet wurde. Verwenden Sie stattdessen Page.RegisterAsyncTask, um den asynchronen Code auf eine Weise auszuführen, die Ihnen ermöglicht, dessen Abschluss nachzuverfolgen.

Das folgende Beispiel zeigt eine Schaltfläche klicken Sie auf Handler, der asynchronen Code enthält. Dieses Beispiel enthält einen Zeichenfolgenwert asynchron ausgeführt wird, lesen die dient nur als ein vereinfachtes Beispiel einer asynchronen Aufgabe und nicht als empfohlene Vorgehensweise.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Bei Verwendung von asynchronen Aufgaben können Sie das Zielframework für HTTP-Laufzeit auf 4.5 festgelegt, in der Datei "Web.config". Durch Festlegen des Zielframeworks auf 4.5 im neuen Synchronisierungskontext, die wurde in .NET 4.5 hinzugefügt. Dieser Wert wird standardmäßig in neuen Projekten in Visual Studio 2012 festgelegt, aber es ist nicht festgelegt werden, wenn Sie ein vorhandenes Projekt arbeiten.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Auslösen und vergessen Arbeit

Empfehlung: Bei der Behandlung von einer Anforderung innerhalb von ASP.NET vermeiden Sie starten auslösen und vergessen Arbeit (solche Aufrufen der Methode ThreadPool.QueueUserWorkItem oder erstellen einen Zeitgeber, der wiederholt einen Delegaten aufruft).

Verfügt Ihre Anwendung Arbeit auslösen und vergessen, die innerhalb von ASP.NET ausgeführt wird, erhalten Ihre Anwendung nicht mehr synchron. Zu jedem Zeitpunkt kann die Anwendungsdomäne zerstört werden dies bedeutet, dass Ihre laufende Prozess den aktuellen Status der Anwendung möglicherweise nicht mehr übereinstimmen.

Sie sollten diese Art von Arbeit außerhalb von ASP.NET verschieben. Sie können ein Web Jobs, Windows-Dienst oder eine workerrolle in Azure verwenden, um derzeit ausgeführte Arbeit auszuführen, und führen Sie diesen Code aus einem anderen Prozess.

Wenn Sie diese Arbeit innerhalb von ASP.NET durchführen müssen, können Sie das NuGet-Paket aufgerufen hinzufügen [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) Ausführen des Codes.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Anforderungs-Einheitstextkörper

Empfehlung: Vermeiden Sie Request.Form oder Request.InputStream lesen, bevor des Handlers des Ereignisses ausgeführt.

Frühestens Request.Form oder Request.InputStream gelesen sollte ausführen während der Ereignishandler Ereignis. In MVC wird der Controller ist der Handler und das Execute-Ereignis ist, wenn die Aktionsmethode ausgeführt wird. In Web Forms die Seite wird der Handler und das Execute-Ereignis ist, wenn das Page.Init-Ereignis auslöst. Wenn Sie die Anforderungs-einheitstextkörper vor der Execute-Ereignis lesen, beeinträchtigen Sie bei der Verarbeitung der Anforderung.

Wenn Sie die Anforderungs-einheitstextkörper vor der Execute-Ereignis zu lesen, verwenden Sie entweder [Request.GetBufferlessInputStream](https://msdn.microsoft.com/en-us/library/ff406798.aspx) oder [Request.GetBufferedInputStream](https://msdn.microsoft.com/en-us/library/system.web.httprequest.getbufferedinputstream.aspx). Bei Verwendung von GetBufferlessInputStream den unformatierten Stream aus der Anforderung abrufen und für die Verarbeitung der gesamten Anforderungs verantwortlich. Nach dem Aufruf von GetBufferlessInputStream sind Request.Form und Request.InputStream nicht verfügbar, da sie nicht von ASP.NET aufgefüllt wurden. Wenn Sie GetBufferedInputStream verwenden, erhalten Sie eine Kopie des Datenstroms aus der Anforderung. Request.Form und Request.InputStream sind weiter unten in der Anforderung weiterhin verfügbar, da ASP.NET die andere Kopie füllt.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response.Redirect und Response.End

Empfehlung: Achten Sie Unterschiede in der Behandlung der Thread nach dem Aufruf [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx).

Die [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx) Methode ruft die Response.End-Methode. In einem synchronen Prozess durch den Aufruf von Request.Redirect der aktuelle Thread sofort abgebrochen. Allerdings in einem asynchronen Prozess aufrufen Response.Redirect nicht den aktuellen Thread abgebrochen wird, damit die Ausführung von Code für die Anforderung ausgeführt wird. In einem asynchronen Prozess müssen Sie die Methode zum Beenden der Ausführung des Codes die Aufgabe zurückgeben.

In einer MVC-Projekt sollten Sie nicht Response.Redirect aufrufen. Geben Sie stattdessen ein RedirectResult zurück.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState und ViewStateMode

Empfehlung: Verwendung ViewStateMode statt EnableViewState, ermöglichen eine präzise Kontrolle, über den Ansichtszustand von Steuerelemente verwenden.

Wenn Sie auf "false" in der Seitendirektive EnableViewState festlegen, wird Ansichtszustand für alle Steuerelemente auf der Seite ist deaktiviert und kann nicht aktiviert werden. Wenn Sie Anzeigen des Status für nur für bestimmte Steuerelemente auf der Seite aktivieren möchten, können Sie ViewStateMode auf deaktiviert festgelegt, für die Seite.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Klicken Sie dann auf nur die Steuerelemente, die tatsächlich Ansichtszustand benötigen ViewStateMode auf Enabled festgelegt.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Durch Aktivieren der Ansichtszustand für Steuerelemente, die sie benötigen, können Sie die Größe des Ansichtszustands für Ihre Web-Seiten verkleinern.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Empfehlung: Verwenden von Universal Providers.

In der aktuellen Projektvorlagen SqlMembershipProvider wurde ersetzt durch [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), der als ein NuGet-Paket verfügbar ist. Wenn Sie SqlMembershipProvider in einem Projekt arbeiten, die mit einer früheren Version der Vorlagen erstellt wurde, sollten Sie auf Universal Providers wechseln. Universal Providers funktioniert mit allen Datenbanken, die vom Entity Framework unterstützt werden.

Weitere Informationen finden Sie unter [Einführung in ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Lang ausgeführte Anforderungen (> 110 Sekunden)

Empfehlung: Verwenden von [WebSockets](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket.aspx) oder [SignalR](../../../signalr/index.md) für verbundene Clients und die Verwendung asynchroner e/a-Vorgänge.

Lang ausgeführte Anforderungen können in der Webanwendung zu unvorhersehbaren Ergebnissen führt und eine schlechte Leistung führen. Die standardmäßige Timeouteinstellung für eine Anforderung beträgt 110 Sekunden. Bei Verwendung von Sitzungsstatus mit einer lang andauernden-Anforderung gibt ASP.NET die Sperre für das Sitzungsobjekt nach 110 Sekunden frei. Allerdings kann die Anwendung gerade ein Vorgang für das Sitzungsobjekt sein, wenn die Sperre wird aufgehoben, und der Vorgang kann nicht erfolgreich abgeschlossen. Wenn eine zweite Anforderung vom Benutzer blockiert wird, während die erste Anforderung ausgeführt wird, kann die zweite Anforderung das Sitzungsobjekt in einem inkonsistenten Zustand zugreifen.

Wenn Ihre Anwendung blockiert (oder synchrone) e/a-Vorgänge enthält, wird die Anwendung reagiert jedoch nicht.

Um die Leistung zu verbessern, verwenden Sie die asynchrone e/a-Vorgänge in .NET Framework. Verwenden Sie außerdem WebSockets oder SignalR, für die Verbindung von Clients mit dem Server. Diese Funktionen dienen, lang ausgeführte Anforderungen effizient zu verarbeiten.
