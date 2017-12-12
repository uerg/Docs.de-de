---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
title: "Fehlerdetails protokollieren ASP.NET-Systemüberwachung (c#) | Microsoft Docs"
author: rick-anderson
description: "Microsoft Health-Überwachungssystem bietet eine einfache und anpassbare Möglichkeit, verschiedene Webereignisse, einschließlich der nicht behandelte Ausnahmen zu protokollieren. Dieses Lernprogramm führt wei..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: b1abb452-642a-4ff3-8504-37b85590ff79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
msc.type: authoredcontent
ms.openlocfilehash: 06f8b57c8973fff5c07e82100cd43f6757d454f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="logging-error-details-with-aspnet-health-monitoring-c"></a>Fehlerdetails protokollieren ASP.NET-Systemüberwachung (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_CS.zip) oder [PDF herunterladen](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_cs.pdf)

> Microsoft Health-Überwachungssystem bietet eine einfache und anpassbare Möglichkeit, verschiedene Webereignisse, einschließlich der nicht behandelte Ausnahmen zu protokollieren. Dieses Lernprogramm führt durch das System für die Systemüberwachung einrichten, um nicht behandelte Ausnahmen in einer Datenbank protokollieren und Entwickler über eine e-Mail-Nachricht zu benachrichtigen.


## <a name="introduction"></a>Einführung

Protokollierung ist ein nützliches Werkzeug für die Überwachung der Integrität einer bereitgestellten Anwendung und für die Diagnose von Problemen, die auftreten können. Es ist besonders wichtig, um Fehler zu protokollieren, die in einer bereitgestellten Anwendung auftreten, sodass sie behoben werden können. Die `Error` Ereignis wird ausgelöst, wenn eine nicht behandelte Ausnahme in einer ASP.NET-Anwendung; tritt auf, die [vorherigen Lernprogramm](processing-unhandled-exceptions-cs.md) wurde gezeigt, wie ein Entwickler einen Fehler zu benachrichtigen und melden Sie sich Details erstellen einen Ereignishandler für das `Error` Ereignis. Erstellen Sie jedoch eine `Error` Ereignishandler zum Protokollieren der Fehler-Details, und benachrichtigen einen Entwickler ist unnötig, wie diese Aufgabe von ASP ausgeführt werden kann. NET *Überwachungssystem*.

Das Überwachungssystem wurde in ASP.NET 2.0 eingeführt und dient zum Überwachen der Integrität einer bereitgestellten Anwendung von ASP.NET durch Protokollierung von Ereignissen, die während der Lebensdauer der Anforderung oder der Anwendung auftreten. Die vom System für die Systemüberwachung protokollierten Ereignisse werden als bezeichnet *Systemüberwachungsereignissen* oder *Webereignisse*, und enthalten:

- Lebensdauer Anwendungsereignisse bereit, z. B. wenn eine Anwendung startet oder beendet
- Sicherheitsereignisse auf, einschließlich fehlgeschlagener Anmeldeversuche und Fehler bei Anforderungen für URL-Autorisierung
- Anwendungsfehler, einschließlich nicht behandelte Ausnahmen, Analysieren von Ausnahmen, fordern Sie Ausnahmen Überprüfung und Kompilierungsfehler, u. a. Fehler Ansichtszustand

Wenn eine Überwachung Ereignis ausgelöst wird, Integrität können sie auf eine beliebige Anzahl von protokolliert angegebenen *protokollieren Quellen*. Das Überwachungssystem Lieferumfang Protokollquellen, die mit einer Microsoft SQL Server-Datenbank, in das Windows-Ereignisprotokoll oder über eine e-Mail-Nachricht, u. a. Web-Ereignisse zu protokollieren. Sie können auch eigene Protokollquellen erstellen.

Die Ereignisse, die das Überwachungssystem protokolliert werden, zusammen mit den Protokollquellen verwendet, sind in definierten `Web.config`. Mit wenigen Skriptzeilen Markup Konfiguration können Sie für die Systemüberwachung überwachen, um alle nicht behandelten Ausnahmen in einer Datenbank protokollieren und der Ausnahme per E-mail benachrichtigt.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Untersuchen die Konfiguration des Systems für die Systemüberwachung

Das Verhalten des Systems für die Systemüberwachung wird durch seine Konfigurationsinformationen, befindet sich im definiert die [ `<healthMonitoring>` Element](https://msdn.microsoft.com/en-us/library/2fwh2ss9.aspx) in `Web.config`. Dieser Konfigurationsabschnitt definiert, unter anderem die folgenden drei wichtigen Arten von Informationen:

1. Die Integrität Überwachen von Ereignissen, die bei der immer dann ausgelöst, die protokolliert werden,
2. Die Protokollquellen und
3. Wie jedes Ereignis definiert (1) für die Systemüberwachung der Protokollquellen zugeordnet ist, definiert in (2).

Diese Informationen über drei untergeordnete Konfigurationselemente angegeben wird: [ `<eventMappings>` ](https://msdn.microsoft.com/en-us/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/en-us/library/zaa41kz1.aspx), und [ `<rules>` ](https://msdn.microsoft.com/en-us/library/fe5wyxa0.aspx)zugeordnet.

Die Standardeinstellung für die Integritätsüberwachung Systemkonfigurationsinformationen finden Sie in der `Web.config` Datei `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` Ordner. Diese Standard-Konfigurationsinformationen ist mit Markup entfernt aus Platzgründen wird unten gezeigt:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample1.xml)]

Die Integrität Überwachungsereignisse relevant, in definiert werden der `<eventMappings>` -Element, das einem von Menschen-Namen auf eine Klasse von Ereignissen für die Integritätsüberwachung bietet. Im oben genannten Markup der `<eventMappings>` Element weist den Personal-Anzeigenamen "Alle Fehler" mit der Integrität der Überwachung von Ereignissen vom Typ `WebBaseErrorEvent` und dem Namen "Fehlerüberwachungen" Systemüberwachung Ereignisse des Typs `WebFailureAuditEvent`.

Die `<providers>` -Element definiert die Protokollquellen befindlichen Human-Namen und alle Protokollinformationen für datenquellenspezifische Konfiguration angeben. Die erste `<add>` -Element definiert die "EventLogProvider"-Anbieter, der der angegebene Systemüberwachungsereignissen mit protokolliert die `EventLogWebEventProvider` Klasse. Die `EventLogWebEventProvider` Klasse protokolliert das Ereignis in das Windows-Ereignisprotokoll. Die zweite `<add>` -Element definiert den Anbieter "SqlWebEventProvider", welche Ereignisse in einer Microsoft SQL Server-Datenbank über Protokolle der `SqlWebEventProvider` Klasse. Die Konfiguration "SqlWebEventProvider" gibt an, die Datenbank-Verbindungszeichenfolge (`connectionStringName`) zwischen anderen Konfigurationsoptionen.

Die `<rules>` -Element ordnet die im angegebenen Ereignisse der `<eventMappings>` Element Quellen Anmelden bei der `<providers>` Element. Standardmäßig ASP.NET-Webanwendungen melden Sie alle nicht behandelte Ausnahmen und Fehlern in der Windows-Ereignisprotokoll überwacht.

## <a name="logging-events-to-a-database"></a>Protokollieren von Ereignissen in einer Datenbank

Standardkonfiguration des Systems für die Systemüberwachung für eine Web-Anwendung von Web-Anwendung angepasst werden kann, durch Hinzufügen einer `<healthMonitoring>` Abschnitt aus, um der Anwendungsverzeichnis `Web.config` Datei. Sie können zusätzliche Elemente einschließen der `<eventMappings>`, `<providers>`, und `<rules>` Abschnitte mit den `<add>` Element. So entfernen Sie eine Einstellung aus der Verwendung der Standard-Konfiguration der `<remove>` -Element, oder verwenden Sie `<clear />` So entfernen alle Standardwerte aus einem der folgenden Abschnitte. Konfigurieren die Webanwendung Buch Reviews auf nicht behandelte Ausnahmen zu protokollieren, mit einer Microsoft SQL Server-Datenbank mithilfe der `SqlWebEventProvider` Klasse.

Die `SqlWebEventProvider` Klasse ist Teil des Systems für die Systemüberwachung und protokolliert ein Ereignis, um eine angegebene SQL Server-Datenbank für die Integritätsüberwachung. Die `SqlWebEventProvider` Klasse erwartet, dass die angegebene Datenbank eine gespeicherte Prozedur namens enthält `aspnet_WebEvent_LogEvent`. Diese gespeicherte Prozedur die Details des Ereignisses übergeben und speichern die Ereignisdetails beauftragt wird. Die gute Nachricht ist, dass Sie nicht benötigen zum Erstellen dieser gespeicherten Prozedur noch die Tabelle, um die Ereignisdetails zu speichern. Sie können diese Objekte hinzufügen, mit der Datenbank mithilfe der `aspnet_regsql.exe` Tool.

> [!NOTE]
> Die `aspnet_regsql.exe` Tool erläutert wurde, wieder in die [ *Konfigurieren einer Website, dass verwendet Anwendungsdienste* Lernprogramm](configuring-a-website-that-uses-application-services-cs.md) Wenn wir die ASP-Unterstützung hinzugefügt. NET Application-Dienste. Folglich Buch Reviews Website Datenbank enthält bereits die `aspnet_WebEvent_LogEvent` gespeicherte Prozedur, die speichert die Ereignisinformationen in eine Tabelle namens `aspnet_WebEvent_Events`.


Nachdem Sie die erforderliche gespeicherte Prozedur und die Tabelle, die mit Ihrer Datenbank hinzugefügt haben, übrig bleibt angewiesen Health überwacht werden, damit um alle nicht behandelte Ausnahmen in der Datenbank zu protokollieren. Dies erreichen, indem Sie das folgende Markup zu Ihrer Website hinzufügen `Web.config` Datei:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample2.xml)]

Konfiguration Markup oben verwendet für die Systemüberwachung `<clear />` Elemente zurücksetzen, die vordefinierte Systemüberwachung-Konfigurationsinformationen aus den `<eventMappings>`, `<providers>`, und `<rules>` Abschnitte. Anschließend wird einen Eintrag zu den folgenden Abschnitten hinzugefügt.

- Die `<eventMappings>` -Element definiert eine einzelne Integritätsüberwachung-Ereignisses von Interesse, die mit dem Namen "Alle Fehler", das ausgelöst wird, wenn eine nicht behandelte Ausnahme auftritt.
- Die `<providers>` -Element definiert eine Protokoll für einzelnes Ereignis Quelle mit dem Namen "SqlWebEventProvider", die verwendet die `SqlWebEventProvider` Klasse. Die `connectionStringName` Attribut auf "ReviewsConnectionString", der Name der unsere Verbindung ist festgelegt wurde definierte Zeichenfolge für die `<connectionStrings>` Abschnitt.
- Schließlich die &lt;Regeln&gt; Element gibt an, dass wenn ein Ereignis "Alle Errors", die herausstellt es protokolliert werden sollten mithilfe des Anbieters "SqlWebEventProvider".

Diese Konfigurationsinformationen weist das System, um alle nicht behandelten Ausnahmen in der Datenbank Buch Reviews melden Sie sich für die Systemüberwachung.

> [!NOTE]
> Die `WebBaseErrorEvent` Ereignis wird nur für Server-Fehler ausgelöst; es wird nicht ausgelöst, für HTTP-Fehler, z. B. eine Anforderung für eine ASP.NET-Ressource, die nicht gefunden wird. Dies unterscheidet sich vom Verhalten der `HttpApplication` Klasse `Error` Ereignis, das für den Server und HTTP-Fehler ausgelöst wird.


Um die Integrität Überwachungssystem in Aktion sehen zu können, besuchen Sie die Website, und generieren einen Laufzeitfehler auf `Genre.aspx?ID=foo`. Daraufhin sollte die entsprechende Fehlerseite - Ausnahme Details gelben Bildschirm des Tod (wenn es sich um eine lokal besuchen) oder die benutzerdefinierte Fehlerseite (beim Besuchen die Website in der Produktion). Im Hintergrund protokolliert das System für die Systemüberwachung die Fehlerinformationen an die Datenbank an. Es muss ein Datensatz in der `aspnet_WebEvent_Events` Tabelle (finden Sie unter **Abbildung 1**); dieser Datensatz enthält Informationen über den Runtime-Fehler, die nur aufgetreten sind.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image1.png)

**Abbildung 1**: wurden die Fehlerdetails protokolliert der `aspnet_WebEvent_Events` Tabelle  
([Klicken Sie hier, um das Bild in voller Größe angezeigt](logging-error-details-with-asp-net-health-monitoring-cs/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Das Fehlerprotokoll anzeigen auf einer Webseite

Mit der aktuellen Konfiguration der Website protokolliert das Überwachungssystem alle nicht behandelte Ausnahmen in der Datenbank an. Überwachen der Integrität stellt jedoch keine Mechanismus zum Anzeigen des Fehlerprotokolls über eine Webseite bereit. Jedoch könnten Sie eine ASP.NET-Seite erstellen, in dem diese Informationen aus der Datenbank angezeigt. (Wir vorübergehend sehen, können Sie ggf. die Fehlerdetails, die in einer e-Mail-Nachricht an Sie gesendet haben.)

Wenn Sie eine solche Seite erstellen, stellen Sie sicher, dass Sie die Schritte, um nur autorisierte Benutzer die Fehlerdetails anzeigen können. Wenn Ihre Website bereits Benutzerkonten einsetzt, können Sie URL-Autorisierungsregeln verwenden, um den Zugriff auf die Seite, um bestimmte Benutzer oder Rollen einzuschränken. Weitere Informationen zum gewähren oder Beschränken des Zugriffs auf Webseiten, die basierend auf dem angemeldeten Benutzer finden Sie in meinem [Lernprogramme für Website-Sicherheit](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> Das nachfolgende Lernprogramm wird eine alternative Fehler protokollieren und ein Benachrichtigungssystem mit dem Namen ELMAH erklärt. ELMAH enthält einen integrierten Mechanismus zum Anzeigen des Fehlerprotokolls von beiden einer Webseite und als RSS-feed.


## <a name="logging-events-to-e-mail"></a>Protokollierung von Ereignissen, E-Mail

Das Überwachungssystem enthält, ein Protokollanbieters für die Quelle, die von einer e-Mail-Nachricht "ein Ereignis protokolliert". Die Protokollquelle enthält dieselbe Informationen, die an die Datenbank in den Nachrichtentext der e-Mail-Nachricht protokolliert wird. Diese Protokollquelle können Sie um einen Entwickler zu benachrichtigen, wenn die Integrität überwachen Auftreten bestimmter Ereignisse.

Wir aktualisieren das Buch Reviews Websitekonfiguration tritt auf, sodass wir eine E-mail, wenn eine Ausnahme empfangen. Um dies zu erreichen, müssen wir drei Aufgaben ausführen:

1. Konfigurieren Sie die ASP.NET-Webanwendung zum Senden von E-mail. Dies wird erreicht, indem Sie angeben, wie e-Mail-Nachrichten gesendet werden, über die `<system.net>` Konfigurationselement. Nachrichten in einer ASP.NET-Anwendung finden Sie weitere Informationen zum Senden von e-Mails in [Senden von E-Mails in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) und [System.Net.Mail FAQ](http://systemnetmail.com/).
2. Registrieren Sie die e-Mail-Quelle Protokollanbieter in der `<providers>` -Element, und
3. Fügen Sie einen Eintrag, um die `<rules>` -Element, das Ereignis "Alle Fehler" in der Quelle Protokollanbieter hinzugefügt, die in Schritt (2) zugeordnet.

Das Überwachungssystem umfasst zwei e-Mail-Protokoll-Quelle-Anbieterklassen: `SimpleMailWebEventProvider` und `TemplatedMailWebEventProvider`. Die [ `SimpleMailWebEventProvider` Klasse](https://msdn.microsoft.com/en-us/library/system.web.management.simplemailwebeventprovider.aspx) sendet eine nur-Text-e-Mail-Nachricht, die das Ereignis enthält details und wenig Anpassung von e-Mail-Nachrichtentext enthält. Mit der [ `TemplatedMailWebEventProvider` Klasse](https://msdn.microsoft.com/en-us/library/system.web.management.templatedmailwebeventprovider.aspx) Angabe eine ASP.NET-Seite, deren gerenderten Markups als Text der e-Mail-Nachricht verwendet wird. Die [ `TemplatedMailWebEventProvider` Klasse](https://msdn.microsoft.com/en-us/library/system.web.management.templatedmailwebeventprovider.aspx) bietet viel größere Kontrolle über den Inhalt und Format der e-Mail-Nachricht jedoch erfordert mehr Vorarbeiten, da Sie sich auf die ASP.NET-Seite zu erstellen, der Text der e-Mail-Nachricht generiert. Dieses Lernprogramm konzentriert sich auf die Verwendung der `SimpleMailWebEventProvider` Klasse.

Aktualisieren des Systems für die Systemüberwachung `<providers>` Element in der `Web.config` Datei einzufügenden eine Protokollquelle für die `SimpleMailWebEventProvider` Klasse:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample3.xml)]

Die oben genannten Markup verwendet die `SimpleMailWebEventProvider` Klasse als der Protokollanbieter für Quelle und weist sie den Anzeigenamen "EmailWebEventProvider". Darüber hinaus die `<add>` Attribut enthält zusätzliche Konfigurationsoptionen, z. B. die und aus der e-Mail-Adressen.

Mit der Quelle von e-Mail-Protokoll definiert übrig bleibt das System, um zu dieser Quelle verwenden, um nicht behandelte Ausnahmen "Anmelden" für die Systemüberwachung angewiesen. Dies erfolgt durch Hinzufügen einer neuen Regel in der `<rules>` Abschnitt:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample4.xml)]

Die `<rules>` Abschnitt enthält jetzt zwei Regeln. Der ersten Abfrage, die mit dem Namen "Alle Fehler, e-Mail-Nachrichten", werden alle nicht behandelte Ausnahmen an die Protokollquelle "EmailWebEventProvider" gesendet. Mit dieser Regel wirkt sich die Details zu Fehlern auf der Website zu senden, in der angegebenen Adresse. Die Regel "Alle Fehler für Datenbank" protokolliert die Fehlerdetails in die Datenbank des Standorts an. Folglich eintreten eine nicht behandelte Ausnahme auf dem Standort die Details sind sowohl in der Datenbank protokolliert und an die angegebene e-Mail-Adresse gesendet.

**Abbildung 2** wird die E-mail von generiert die `SimpleMailWebEventProvider` -Klasse beim Zugriff auf `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image4.png)

**Abbildung 2**: der Fehlerdetails in einer e-Mail-Nachricht gesendet werden  
([Klicken Sie hier, um das Bild in voller Größe angezeigt](logging-error-details-with-asp-net-health-monitoring-cs/_static/image6.png))

## <a name="summary"></a>Zusammenfassung

Die ASP.NET-Systemüberwachung dient ermöglicht Administratoren bei der Überwachung einer Webanwendung bereitgestellt. Integrität Überwachungsereignisse werden ausgelöst, wenn bestimmte Aktionen zu erweitern, z. B. wenn die Anwendung beendet wird, wenn ein Benutzer erfolgreich an die Site anmeldet oder eine nicht behandelte Ausnahme auftritt. Diese Ereignisse können auf eine beliebige Anzahl von Protokollquellen protokolliert werden. Dieses Lernprogramm wurde gezeigt, wie die Details der nicht behandelten Ausnahmen in einer Datenbank und über eine e-Mail-Nachricht protokolliert wird.

In diesem Lernprogramm zur Verwendung für die Integritätsüberwachung zum Melden Sie sich nicht behandelte Ausnahmen, jedoch sollten Sie bedenken, dass die Integritätsüberwachung wurde entwickelt, um die Gesamtintegrität einer bereitgestellten Anwendung für ASP.NET zu messen und enthält eine Vielzahl von Systemüberwachungsereignissen und Quellen nicht mit Fokus Hier untersucht. Was mehr ist, können Sie Ihre eigenen Ereignisse und Protokollquellen, die Integritätsüberwachung erstellen ggf. auftreten. Wenn Sie weitere Informationen zu Systemüberwachung interessiert, ist ein guter erster Schritt, zu lesen, [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)des [– häufig gestellte Fragen für die Integritätsüberwachung](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). Danach finden Sie in [How To: Verwendung Systemüberwachung in ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/ms998306.aspx).

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Übersicht über die ASP.NET-Systemüberwachung](https://msdn.microsoft.com/en-us/library/bb398933.aspx)
- [Konfigurieren und Anpassen von System von ASP.NET für die Systemüberwachung](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Häufig gestellte Fragen – Integritätsüberwachung von ASP.NET 2.0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Gewusst wie: Senden von e-Mail-Einstellungen für Benachrichtigungen für die Integritätsüberwachung](https://msdn.microsoft.com/en-us/library/ms227553.aspx)
- [Gewusst wie: Verwenden Sie Systemüberwachung in ASP.NET](https://msdn.microsoft.com/en-us/library/ms998306.aspx)
- [Integritätsüberwachung von ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

>[!div class="step-by-step"]
[Zurück](processing-unhandled-exceptions-cs.md)
[Weiter](logging-error-details-with-elmah-cs.md)
