---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: Protokollieren von Fehlerdetails mit der ASP.NET-Systemüberwachung (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Überwachung von Microsoft Health-System bietet eine einfache und anpassbare Möglichkeit, verschiedene Webereignisse, einschließlich der nicht behandelte Ausnahmen zu protokollieren. In diesem Tutorial werden wei...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: 1ed7b63989dc6ea7e46210a45612e2672a662177
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382471"
---
<a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>Protokollieren von Fehlerdetails mit der ASP.NET-Systemüberwachung (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> Überwachung von Microsoft Health-System bietet eine einfache und anpassbare Möglichkeit, verschiedene Webereignisse, einschließlich der nicht behandelte Ausnahmen zu protokollieren. Dieses Tutorial führt durch das System für die Systemüberwachung einrichten, um nicht behandelte Ausnahmen in einer Datenbank zu protokollieren und Entwickler über eine e-Mail zu benachrichtigen.


## <a name="introduction"></a>Einführung

Die Protokollierung ist ein nützliches Tool für die Überwachung der Integrität einer bereitgestellten Anwendung und für die Diagnose von Problemen, die auftreten können. Es ist besonders wichtig, um Fehler zu protokollieren, die in einer bereitgestellten Anwendung auftreten, sodass sie korrigiert werden, können. Die `Error` Ereignis wird ausgelöst, wenn eine unbehandelte Ausnahme, in einer ASP.NET-Anwendung auftritt; die [vorherigen Lernprogramm](processing-unhandled-exceptions-vb.md) wurde gezeigt, wie ein Entwickler einen Fehler und melden Sie sich die Details erstellen einen Ereignishandler für die `Error` Ereignis. Erstellen Sie jedoch eine `Error` -Ereignishandler zum Melden des Fehlers Details, und benachrichtigen einen Entwickler ist nicht erforderlich, wie diese Aufgabe von ASP durchgeführt werden kann. NET *System für die Systemüberwachung*.

Das System für die Integritätsüberwachung in ASP.NET 2.0 eingeführt wurde und dient zur Überwachung der Integrität einer bereitgestellten Anwendung mit ASP.NET durch Protokollierung von Ereignissen, die während der Lebensdauer der Anforderung oder der Anwendung auftreten. Die vom System für die Systemüberwachung protokollierten Ereignisse werden als bezeichnet *Systemüberwachungsereignisse* oder *Web Ereignisse*, und enthalten:

- Anwendungslebensdauer-Ereignisse, z. B. wenn eine Anwendung startet oder beendet
- Sicherheitsereignisse, einschließlich fehlgeschlagener Anmeldeversuche und URL-autorisierungsanforderungen mit Fehlern
- Anwendungsfehler, einschließlich der Ausnahmefehler, Analysieren von Ausnahmen, Überprüfung fordern Sie Ausnahmen und Kompilierungsfehler, u. a. Fehler Ansichtszustand.

Wenn ein Health monitoring Ereignis ausgelöst wird, kann er auf eine beliebige Anzahl von protokolliert werden angegebene *melden Quellen*. Das System für die Systemüberwachung im Lieferumfang von Protokollquellen, die mit einer Microsoft SQL Server-Datenbank, in das Windows-Ereignisprotokoll oder über eine e-Mail-Nachricht, u. a. Web-Ereignisse zu protokollieren. Sie können auch Ihre eigenen Protokollquellen erstellen.

Die Ereignisse, die System für die Systemüberwachung protokolliert werden, sowie die Protokollquellen, die verwendet wird, werden in definiert `Web.config`. Mit wenigen Codezeilen konfigurationsmarkup können Sie alle nicht behandelte Ausnahmen in einer Datenbank zu protokollieren und Sie für die Ausnahme per e-Mail benachrichtigt werden, die Systemüberwachung.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Untersuchen die Konfiguration des Systems für die Systemüberwachung

Das Verhalten des Systems für die Integritätsüberwachung wird durch seine Konfigurationsinformationen, die sich im befindet definiert die [ `<healthMonitoring>` Element](https://msdn.microsoft.com/library/2fwh2ss9.aspx) in `Web.config`. Dieser Konfigurationsabschnitt definiert, unter anderem die folgenden drei wichtigen Arten von Informationen:

1. Die Integrität Überwachen von Ereignissen, die protokolliert werden sollen, wenn ausgelöst
2. Die Protokollquellen und
3. Wie jede Systemüberwachungsereignisses in (1) definiert die Protokollquellen zugeordnet wird, die in (2) definiert wurden.

Diese Informationen werden mithilfe von Konfigurationselementen für die drei untergeordneten Elemente angegeben: [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx), und [ `<rules>` ](https://msdn.microsoft.com/library/fe5wyxa0.aspx)bzw.

Informationen zur Betriebssystemkonfiguration für die standardmäßige Integritätsüberwachung finden Sie in der `Web.config` Datei `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` Ordner. Diese Daten standardmäßig mit Markup entfernt aus Gründen der Übersichtlichkeit unten:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

Die Integrität, die Ereignisse bei der Überwachung von Interesse, in definiert sind der `<eventMappings>` -Element, das ein Benutzerfreundlicher Name für eine Klasse von Systemüberwachungsereignissen zu erhalten. Im obigen Markup der `<eventMappings>` -Element weist den Benutzerfreundlicher Namen "Alle Fehler" mit der Integrität der Überwachung von Ereignissen vom Typ `WebBaseErrorEvent` und dem Namen "Fehlerüberwachungen" zum Überwachen von Ereignissen des Typs des Zustands `WebFailureAuditEvent`.

Die `<providers>` -Element definiert die Protokollquellen, Ihnen einen Benutzerfreundlicher Namen, und alle Protokollinformationen für die Source-spezifische Konfiguration angeben. Die erste `<add>` -Element definiert den Anbieter "EventLogProvider" protokolliert die angegebene Integrität Überwachen von Ereignissen mit der `EventLogWebEventProvider` Klasse. Die `EventLogWebEventProvider` Klasse das Ereignis protokolliert in das Windows-Ereignisprotokoll. Die zweite `<add>` -Element definiert den Anbieter "SqlWebEventProvider" protokolliert Ereignisse in einer Microsoft SQL Server-Datenbank über die `SqlWebEventProvider` Klasse. Die Konfiguration "SqlWebEventProvider" gibt an, der Datenbank-Verbindungszeichenfolge (`connectionStringName`) zwischen anderen Optionen.

Die `<rules>` -Element ordnet die im angegebenen Ereignisse der `<eventMappings>` Element Quellen melden Sie sich die `<providers>` Element. In der Standardeinstellung ASP.NET-Webanwendungen melden Sie sich alle nicht behandelte Ausnahmen und Fehlern in das Windows-Ereignisprotokoll überwachen.

## <a name="logging-events-to-a-database"></a>Protokollieren von Ereignissen in einer Datenbank

Standardkonfiguration des Systems für die Systemüberwachung kann pro Web-Anwendung-von-Web-Anwendung angepasst werden, durch das Hinzufügen einer `<healthMonitoring>` Abschnitt aus, um der Anwendung `Web.config` Datei. Sie können zusätzliche Elemente einschließen der `<eventMappings>`, `<providers>`, und `<rules>` Abschnitte mit der `<add>` Element. So entfernen Sie eine Einstellung aus der Verwendung der standardmäßigen Konfiguration der `<remove>` -Element, oder verwenden Sie `<clear />` So entfernen Sie alle Standardwerte aus einem dieser Abschnitte. Konfigurieren wir die Book Reviews-Webanwendung, um nicht behandelte Ausnahmen zu protokollieren, einer Microsoft SQL Server-Datenbank mithilfe der `SqlWebEventProvider` Klasse.

Die `SqlWebEventProvider` -Klasse ist Teil des Systems für die Integritätsüberwachung und protokolliert ein Ereignis, um eine angegebene SQL Server-Datenbank für die Systemüberwachung. Die `SqlWebEventProvider` Klasse erwartet, dass die angegebene Datenbank eine gespeicherte Prozedur namens enthält `aspnet_WebEvent_LogEvent`. Diese gespeicherte Prozedur die Details des Ereignisses übergeben und ist damit beauftragt, Details zu speichern. Die gute Nachricht ist, dass Sie nicht benötigen, erstellen Sie diese gespeicherte Prozedur noch in der Tabelle, um die Ereignisdetails zu speichern. Sie können diese Objekte hinzufügen, um Ihre Datenbank mit der `aspnet_regsql.exe` Tool.

> [!NOTE]
> Die `aspnet_regsql.exe` Tool wurde erläutert, in der [ *konfigurieren eine Website, dass verwendet Anwendungsdienste* Tutorial](configuring-a-website-that-uses-application-services-vb.md) bei der Addition ASP-Unterstützung. NET Application-Dienste. Daher der Book Reviews-Website Datenbank enthält bereits die `aspnet_WebEvent_LogEvent` gespeicherte Prozedur, die speichert die Ereignisinformationen in eine Tabelle namens `aspnet_WebEvent_Events`.


Nachdem Sie die erforderliche gespeicherte Prozedur und die Tabelle, die mit Ihrer Datenbank hinzugefügt haben, übrig bleibt anweisen Health überwacht werden, um alle nicht behandelten Ausnahmen in der Datenbank zu protokollieren. Dies erreichen, indem Sie das folgende Markup Ihrer Website hinzufügen `Web.config` Datei:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

Die Systemüberwachung konfigurationsmarkup oben verwendet `<clear />` Elementen, die Konfigurationsinformationen aus für die vordefinierte Integritätsüberwachung Zurücksetzen der `<eventMappings>`, `<providers>`, und `<rules>` Abschnitte. Anschließend wird einen einziger Eintrag auf den folgenden Abschnitten hinzugefügt.

- Die `<eventMappings>` -Element definiert eine einzelne Systemüberwachung-Ereignis von Interesse, die mit dem Namen "Alle Fehler", die ausgelöst wird, wenn eine unbehandelte Ausnahme auftritt.
- Die `<providers>` -Element definiert eine Protokoll für einzelnes Ereignis-Quelle, die mit dem Namen "SqlWebEventProvider", die verwendet die `SqlWebEventProvider` Klasse. Die `connectionStringName` Attribut auf "ReviewsConnectionString", der Name der unsere Verbindung ist festgelegt wurde in definierten Zeichenfolge die `<connectionStrings>` Abschnitt.
- Zum Schluss die &lt;Regeln&gt; Element gibt an, dass wenn ein Ereignis "Alle Fehler", die herausstellt es protokolliert werden sollten mit dem Anbieter "SqlWebEventProvider".

Diese Konfigurationsinformationen weist das System, um alle nicht behandelten Ausnahmen in der Datenbank Book Reviews melden Sie sich für die Systemüberwachung.

> [!NOTE]
> Die `WebBaseErrorEvent` Ereignis wird nur für Server-Fehler ausgelöst; es wird nicht ausgelöst, für die HTTP-Fehler, z. B. eine Anforderung für eine ASP.NET-Ressource, die nicht gefunden wird. Dies unterscheidet sich vom Verhalten der `HttpApplication` Klasse `Error` -Ereignis, das für sowohl Server-als auch HTTP-Fehler ausgelöst wird.


Um das Überwachungssystem in Aktion sehen zu können, finden Sie auf der Website aus, und verursachen einen Laufzeitfehler finden Sie unter `Genre.aspx?ID=foo`. Daraufhin sollte die entsprechende Fehlerseite – entweder die Ausnahme Details gelben Bildschirm of Death (wenn lokal auf) oder die benutzerdefinierte Fehlerseite (wenn besuchen die Website in der Produktion). Hinter den Kulissen System für die Systemüberwachung werden die Fehlerinformationen in der Datenbank protokolliert. Es muss ein Datensatz in die `aspnet_WebEvent_Events` Tabelle (finden Sie unter **Abbildung 1**); dieser Datensatz enthält Informationen über den Common Language Runtime-Fehler, die nur aufgetreten sind.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**Abbildung 1**: die Fehlerdetails wurden protokolliert, um die `aspnet_WebEvent_Events` Tabelle  
([Klicken Sie, um das Bild in voller Größe anzeigen](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Das Fehlerprotokoll anzeigen auf einer Webseite

Mit der aktuellen Konfiguration der Website protokolliert das System für die Systemüberwachung alle nicht behandelte Ausnahmen in der Datenbank. Überwachung der Integrität bietet sich jedoch nicht auf alle Mechanismus zum Anzeigen des Fehlerprotokolls über eine Webseite aus. Allerdings könnten Sie eine ASP.NET-Seite erstellen, die diese Informationen aus der Datenbank angezeigt. (Wir sofort sehen, können Sie optional die Fehlerdetails, die in einer e-Mail-Nachricht an Sie gesendet haben.)

Wenn Sie eine solche Seite erstellen, stellen Sie sicher, dass Sie Maßnahmen können nur autorisierte Benutzer auf die Fehlerdetails anzuzeigen. Wenn Ihre Website bereits Benutzerkonten verwendet, Sie URL-Autorisierungsregeln verwenden, um den Zugriff auf der Seite auf bestimmte Benutzer oder Rollen einschränken. Weitere Informationen zum gewähren oder Einschränken des Zugriffs auf Webseiten, die basierend auf dem angemeldeten Benutzer finden Sie in meinem [Website-Lernprogramme zur ASP.NET-Sicherheit](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> Die nachfolgenden Tutorial wird beschrieben, ein alternativer Fehler protokollieren und Benachrichtigung System mit dem Namen ELMAH. ELMAH enthält einen integrierten Mechanismus zum Anzeigen des Fehlerprotokolls von sowohl einer Webseite und als RSS-feed.


## <a name="logging-events-to-email"></a>Protokollierung von Ereignissen an e-Mail-Adresse

Das System für die Systemüberwachung enthält einen Protokollanbieter in der Datenquelle ein, das "in einer e-Mail-Nachricht ein Ereignis protokolliert". Die Protokollquelle enthält dieselbe Informationen, die mit der Datenbank, in dem Textkörper der e-Mail-Adresse angemeldet ist. Diese Protokollquelle können Sie um einen Entwickler zu benachrichtigen, wenn ein bestimmtes Überwachung Health-Ereignis auftritt.

Aktualisieren wir die Book Reviews Websitekonfiguration, damit erhalten wir eine e-Mail, wenn eine Ausnahme tritt auf, ein. Zu diesem Zweck müssen wir drei Aufgaben ausführen:

1. Konfigurieren der ASP.NET-Webanwendung zum Senden von e-Mails an. Dies wird erreicht, indem Sie angeben, wie e-Mail-Nachrichten gesendet werden, über die `<system.net>` Konfigurationselement. Weitere Informationen zum Senden von e-Mails von Nachrichten in einer ASP.NET-Anwendung finden Sie in [Senden von e-Mail-Adresse in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) und [System.Net.Mail – häufig gestellte Fragen](http://systemnetmail.com/).
2. Registrieren Sie den e-Mail-Quelle Protokollanbieter in der `<providers>` -Element, und
3. Fügen Sie einen Eintrag, um die `<rules>` -Element, das Ereignis "Alle Fehler" in der Quelle Protokollanbieter hinzugefügt, die in Schritt (2) zugeordnet.

Das System für die Systemüberwachung enthält zwei e-Mail-Log-Datenquellen-Anbieterklassen: `SimpleMailWebEventProvider` und `TemplatedMailWebEventProvider`. Die [ `SimpleMailWebEventProvider` Klasse](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) sendet eine nur-Text-e-Mail-Nachricht, die das Ereignis enthält details und bietet aber kleine Anpassungen des e-Mail-Texts. Mit der [ `TemplatedMailWebEventProvider` Klasse](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) Angabe eine ASP.NET-Seite, deren gerendertes Markup wird als Text der e-Mail-Nachricht verwendet. Die [ `TemplatedMailWebEventProvider` Klasse](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) bietet Ihnen viel bessere Kontrolle über den Inhalt und Format der e-Mail-Nachricht, aber erfordert etwas mehr Vorarbeiten, wie Sie die ASP.NET-Seite zu erstellen, die e-Mail-Text generiert. Dieses Tutorial konzentriert sich auf die Verwendung der `SimpleMailWebEventProvider` Klasse.

Aktualisieren des Systems für die Systemüberwachung `<providers>` Element in der `Web.config` eingeschlossen ein Protokollquelle für die `SimpleMailWebEventProvider` Klasse:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

Das obenstehende Markup verwendet den `SimpleMailWebEventProvider` Klasse als den Protokollanbieter für die Datenquelle, und weist sie den Anzeigenamen "EmailWebEventProvider". Darüber hinaus die `<add>` Attribut enthält zusätzliche Konfigurationsoptionen, z. B. den und aus der e-Mail-Adressen.

Mit der Quelle des e-Mail-Protokoll, die definiert übrig bleibt, weisen Sie das System, um diese Quelle verwendet wird, nicht behandelte Ausnahmen "Anmelden" für die Systemüberwachung. Dies geschieht durch Hinzufügen einer neuen Regel in der `<rules>` Abschnitt:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

Die `<rules>` Abschnitt enthält jetzt zwei Regeln. Erstens, mit dem Namen "Alle Fehler, Email", sendet alle nicht behandelte Ausnahmen an die Protokollquelle "EmailWebEventProvider". Diese Regel wirkt sich das Senden von Informationen zu Fehlern auf der Website mit dem angegebenen Adresse. Die Regel "Alle Fehler für Datenbank" protokolliert die Fehlerdetails in der Datenbank des Standorts an. Folglich immer eine unbehandelte Ausnahme, auf der Website die Details auftritt werden beide in der Datenbank protokolliert und an die angegebene e-Mail-Adresse gesendet.

**Abbildung 2** zeigt die vom e-Mail-Adresse der `SimpleMailWebEventProvider` Klasse, wenn der Zugriff auf `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**Abbildung 2**: die Fehlerdetails werden in einer e-Mail-Nachricht gesendet  
([Klicken Sie, um das Bild in voller Größe anzeigen](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>Zusammenfassung

Der ASP.NET-Systemüberwachung soll Administratoren die Integrität einer bereitgestellten Webanwendung überwachen können. Überwachung Health-Ereignisse werden ausgelöst, wenn bestimmte Aktionen wie z. B. wenn die Anwendung beendet wird, wenn ein Benutzer erfolgreich bei der Site protokolliert umgewandelt werden soll, oder wenn eine nicht behandelte Ausnahme auftritt. Diese Ereignisse können auf eine beliebige Anzahl von Protokollquellen protokolliert werden. In diesem Tutorial wurde gezeigt, wie die Details der nicht behandelten Ausnahmen in einer Datenbank und über eine e-Mail-Nachricht protokolliert wird.

Dieses Tutorial konzentriert sich auf die Systemüberwachung zum Melden Sie sich nicht behandelte Ausnahmen, aber denken Sie daran, dass die Systemüberwachung dient zum Messen Sie der allgemeinen Integritäts einer bereitgestellten Anwendung mit ASP.NET und bietet eine Fülle von Systemüberwachungsereignissen und Quellen nicht verwenden Hier untersucht. Was ist, können Sie Ihre eigenen Ereignisse und Protokollquellen, die Systemüberwachung erstellen Bedarf auftreten. Wenn Sie möchten mehr über die Überwachung der Integrität, ist ein guter erster Schritt, zu lesen, [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)des [– häufig gestellte Fragen für die Systemüberwachung](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). Danach finden Sie in [so wird's gemacht: verwenden Überwachen der Sicherheitsintegrität in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998306.aspx).

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Übersicht über die ASP.NET-Systemüberwachung](https://msdn.microsoft.com/library/bb398933.aspx)
- [Konfigurieren und Anpassen von System von ASP.NET für die Systemüberwachung](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Häufig gestellte Fragen – ASP.NET 2.0 für die Systemüberwachung](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Gewusst wie: Senden von e-Mail-Einstellungen für Benachrichtigungen für die Systemüberwachung](https://msdn.microsoft.com/library/ms227553.aspx)
- [Gewusst wie: Verwenden Sie Überwachen der Sicherheitsintegrität in ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Systemüberwachung in ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Zurück](processing-unhandled-exceptions-vb.md)
> [Weiter](logging-error-details-with-elmah-vb.md)
