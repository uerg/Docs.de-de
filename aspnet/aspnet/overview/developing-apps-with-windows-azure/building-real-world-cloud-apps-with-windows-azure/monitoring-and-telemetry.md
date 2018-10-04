---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Überwachung und Telemetrie (erstellen realer Cloud-Apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: f4dae827627103e5cfb9981b6c3b9342cdc34c13
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578002"
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Überwachung und Telemetrie (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen zu e-Book, finden Sie unter [im ersten Kapitel](introduction.md).


Viele Leute, abhängig von Kunden zu informieren, wenn ihre Anwendung nicht verfügbar ist. Das ist nicht wirklich eine bewährte Methode eine beliebige Stelle, und zwar insbesondere nicht in der Cloud. Es gibt keine Garantie der schnellen Benachrichtigung, und bei der Sie benachrichtigt werden,, erhalten Sie oft minimale oder irreführende Daten, was passiert ist. Mit guten Telemetrie- und protokollierungssysteme, die Sie was passiert achten können mit der app, und wenn etwas geht ein auftreten, Sie unverzüglich erkennen und mithilfe nützlicher Problembehandlungsinformationen zum Arbeiten mit.

## <a name="buy-or-rent-a-telemetry-solution"></a>Erwerben Sie oder zu mieten Sie eine komplettlösung für Telemetrie

> [!NOTE]
> Dieser Artikel geschrieben wurde, bevor Sie [Application Insights](/azure/application-insights/app-insights-overview) wurde veröffentlicht. Application Insights ist die bevorzugte Methode für die telemetrielösungen in Azure. Finden Sie unter [richten Sie Application Insights für Ihre ASP.NET-Website](/azure/application-insights/app-insights-asp-net) für Weitere Informationen.


Eines der Dinge, die besondere an der Cloud-Umgebung ist, es wirklich einfach ist oder so Sieg Mieten. Telemetrie ist ein Beispiel. Ohne viel Aufwand erhalten Sie ein System wirklich guten Telemetrie- und ausgeführt wird, sehr kostengünstig. Es gibt einige hervorragende Partner, die in Azure integriert, und einige von ihnen haben free-Tarife –, damit Sie grundlegende Telemetriedaten auf ' Nothing ' nutzen können. Hier sind nur einige der in Azure derzeit verfügbar:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) enthält außerdem Überwachungsfunktionen.

Wir zeigen Ihnen schnell New Relic zu zeigen, wie einfach es sein kann, verwenden Sie ein Telemetriesystem einrichten.

Registrieren Sie in der Azure-Verwaltungsportal sich für den Dienst an. Klicken Sie auf **neu**, und klicken Sie dann auf **Store**. Die **Add-on auswählen** Dialogfeld wird angezeigt. Scrollen Sie nach unten, und klicken Sie auf **New Relic**.

![Add-on auswählen](monitoring-and-telemetry/_static/image1.png)

Klicken Sie auf den Pfeil nach rechts, und wählen Sie die gewünschten Dienstebene. Für diese Demo verwenden wir den freien-Tarif.

![Add-on personalisieren](monitoring-and-telemetry/_static/image2.png)

Klicken Sie auf den Pfeil nach rechts, bestätigen die "kaufen", und New Relic jetzt wird als ein Add-on im Portal.

![Kauf überprüfen](monitoring-and-telemetry/_static/image3.png)

![Neue Relic-Add-Ons im Verwaltungsportal](monitoring-and-telemetry/_static/image4.png)

Klicken Sie auf **Verbindungsinformationen**, und kopieren Sie den Lizenzschlüssel.

![Verbindungsinformationen](monitoring-and-telemetry/_static/image5.png)

Wechseln Sie zu der **konfigurieren** Registerkarte für Ihre Web-app im Portal festgelegt **Leistungsüberwachung** zu **-Add-On**, und legen Sie die **Add-On auswählen** Dropdown-Liste aus, um **New Relic**. Klicken Sie dann auf **speichern**.

![New Relic auf der Registerkarte "konfigurieren"](monitoring-and-telemetry/_static/image6.png)

Installieren Sie das New Relic NuGet-Paket in Ihrer app in Visual Studio.

![Developer Analytics in der Registerkarte "konfigurieren"](monitoring-and-telemetry/_static/image7.png)

Bereitstellen der app in Azure, und es verwenden. Erstellen Sie ein paar Fix It-Aufgaben zum Bereitstellen von Aktivitäten bei New Relic zur Überwachung.

Navigieren Sie zurück zu den **New Relic** auf der Seite die **-Add-Ons** das Portal und klicken Sie auf der Registerkarte **verwalten**. Das Portal sendet Sie an das New Relic-Verwaltungsportal, die einmaliges Anmelden für die Authentifizierung verwenden, müssen Sie nicht Ihre Anmeldeinformationen erneut einzugeben. Die Seite "Übersicht" bietet eine Vielzahl von Leistungsstatistiken. (Klicken Sie auf das Bild, um die vollständige Übersicht über die Seitengröße anzuzeigen.)

[![Neue Relic-Überwachung-Registerkarte](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Hier sind nur einige der Statistiken, die Sie sehen können:

- Durchschnittliche Antwortzeit zu unterschiedlichen Zeiten des Tages.

    ![Antwortzeit](monitoring-and-telemetry/_static/image10.png)
- Durchsatzraten erforderlich sind (in der Anforderungen pro Minute) zu unterschiedlichen Zeiten des Tages.

    ![Durchsatz](monitoring-and-telemetry/_static/image11.png)
- Server-CPU-Zeit aufgewendet, unterschiedliche HTTP-Anforderungen verarbeiten.

    ![Web-Transaktionszeiten](monitoring-and-telemetry/_static/image12.png)
- CPU-Zeit in verschiedenen Teilen des Anwendungscodes:

    ![Ablaufverfolgungsdetails](monitoring-and-telemetry/_static/image13.png)
- Leistungsverlauf Statistiken.

    ![Leistungsverlauf](monitoring-and-telemetry/_static/image14.png)
- Aufrufe von externen Diensten wie z. B. die Blob-Dienst und die Statistik zur zuverlässigen und reaktionsfähige wurde der Dienst.

    ![Externe Dienste](monitoring-and-telemetry/_static/image15.png)

    ![Externe Dienste](monitoring-and-telemetry/_static/image16.png)

    ![Externe Dienste](monitoring-and-telemetry/_static/image17.png)
- Informationen dazu, wo in der ganzen Welt oder, in dem in der US-Web-app Datenverkehr stammt.

    ![Geography](monitoring-and-telemetry/_static/image18.png)

Sie können auch Berichte und-Ereignisse einrichten. Sie können beispielsweise jederzeit angezeigt werden Fehler angenommen, Senden einer e-Mail an alert Supportpersonal auf das Problem.

![Berichte](monitoring-and-telemetry/_static/image19.png)

New Relic ist nur ein Beispiel für ein Telemetriesystem; Sie können all dies von anderen Diensten sowie abrufen. Das Schöne an der Cloud ist, ohne Code schreiben zu müssen, und klicken Sie für die minimale oder keine Ausgaben, plötzlich erhalten Sie noch viel mehr Informationen, wie Ihre Anwendung verwendet wird und was Ihre Kunden tatsächlich auftreten.

<a id="log"></a>
## <a name="log-for-insight"></a>Protokoll für Einblicke

Ein telemetriepaket ist ein guter erster Schritt, müssen jedoch Ihren eigenen Code zu instrumentieren. Der telemetriedienst Aufschluss darüber, wenn ein Problem vorliegt und nennt die Kunden treten, aber es kann keine bieten Ihnen viele der Einblick in die Abläufe in Ihrem Code.

Sie möchten nicht haben, um remote auf einem Produktionsserver, um festzustellen, was den Status Ihrer app. Die möglicherweise praktisch, wenn Sie einen Server haben, aber was, wenn Sie die auf Hunderten von Servern skaliert haben, und Sie nicht wissen, welche Sie Remotezugriff müssen? Ihre Protokollierung sollte aussagekräftig sind, die nie eine Remoteverbindung mit Produktionsservern zum Analysieren und beheben Probleme. Sie sollten genügend Informationen protokollieren, damit Sie Probleme, die ausschließlich über die Protokolle isolieren können.

### <a name="log-in-production"></a>Melden Sie sich in der Produktion

Viele Leute Aktivieren der Ablaufverfolgung in einer produktionsumgebung nur, wenn ein Problem vorliegt und sie debuggen möchten. Dies kann eine erhebliche Verzögerung zwischen dem Zeitpunkt, wenn Sie ein Problem bekannt sind und die Zeit erhalten Sie hilfreiche Informationen dazu, die zur Problembehandlung führen. Und die Informationen erhalten Sie möglicherweise nicht nach vorübergehenden Fehlern hilfreich sein.

Was wir in der Cloud-Umgebung wird empfohlen, in dem Speicher günstig ist, ist, dass Sie lassen Sie in der Produktion anmelden. Auf diese Weise, wenn Fehler auftreten, bereits diese protokolliert, und Sie haben es sich um historische Daten, die helfen können, analysieren Sie Probleme, die im Laufe der Zeit entwickeln oder regelmäßig zu unterschiedlichen Zeiten ausgeführt. Sie können einen Bereinigungsprozess zum Löschen der alten Protokolle automatisieren, aber unter Umständen ist dieser Prozess einrichten, als es ist, behalten Sie die Protokolle kostenintensiver.

Der zusätzliche Aufwand für die Protokollierung ist trivial, im Vergleich zur Menge der Problembehandlung von Zeit und Geld, die Sie speichern können, indem Sie alle Informationen, die Sie bereits verfügbar benötigen, falls etwas schief geht. Klicken Sie dann, wenn jemand besagt wiesen einen zufälligen Fehler einiger Zeit rund um 8:00 Uhr gestern Abend, aber sie nicht den Fehler nicht mehr wissen, können Sie leicht feststellen, Problem.

Für weniger als $4 ist einen Monat, die Sie 50 GB von Protokollen auf Seite aufbewahren können und die Auswirkungen auf die Leistung der Protokollierung trivial, sofern Sie halten daran denken, dass – um Leistungsengpässe vermeiden, stellen Sie sicher, dass Ihre protokollierungsbibliothek asynchron ist.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Unterscheiden Sie Protokolle, die informieren von Protokollen, die ein Eingreifen erfordern

Protokolle sind eigentlich INFORM (Ich möchte etwas bekannt sein) oder ACT (Ich möchte Sie etwas tun). Achten Sie darauf, dass Sie nur Schreiben von ACT-Protokollen für Probleme, die tatsächlich eine Person oder ein automatisierter Prozess, eine Aktion erfordern. Zu viele ACT-Protokolle erstellt Rauschen an, dass zu viel Arbeit, gesichtet, alles um echte Probleme zu finden. Und wenn die ACT-Protokolle automatisch eine Aktion wie z. B. das Senden von e-Mails für die Supportmitarbeiter auslösen, vermeiden, dass Tausende von Aktionen, die durch ein einzelnes Problem ausgelöst werden.

In .NET System.Diagnostics-Ablaufverfolgung, können Protokolle Fehler-, Warn-, Info und Debug/Verbose-Ebene zugewiesen werden. Sie können ACT aus Protokollen der INFORM unterscheiden, indem reservieren Fehlerstufe für ACT-Protokolle und die unteren Ebenen für INFORM Protokolle.

![Protokolliergrade](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Konfigurieren von Protokolliergraden zur Laufzeit

Obwohl es lohnt sich damit anmelden immer in der Produktion ist, ist ein anderes bewährtes Verfahren ein protokollierungsframework zu implementieren, die Sie anpassen, die Ebene an Informationen, die protokolliert wird, ohne erneute Bereitstellung oder Neustarten Ihrer Anwendung zur Laufzeit ermöglicht. Zum Beispiel bei Verwendung der Verfolgungsfunktion in `System.Diagnostics` können Sie Fehler, Warnung, Informationen erstellen und Debuggen/Verbose protokolliert. Es wird empfohlen, Sie sollten immer protokolliert werden Fehler, Warnung, und Informationen protokolliert werden, in der Produktion sollten Sie zum Debuggen/Verbose-Protokollierung, für die Problembehandlung auf von Fall zu Fall dynamisch hinzufügen können.

In Azure App Service-Web-Apps bieten integrierte Unterstützung für das Schreiben von `System.Diagnostics` Protokolle, um das Dateisystem, Table Storage oder BLOB-Speicher. Sie können verschiedene Protokolliergrade für die einzelnen Ziele Speicher auswählen, und Sie können den Protokolliergrad im laufenden Betrieb ohne Neustarten Ihrer Anwendung ändern. Der Blob-speichersupport erleichtert das auszuführende [HDInsight](https://docs.microsoft.com/azure/hdinsight/) Analysis-Aufträge für die Anwendungsprotokolle, da HDInsight direkt mit BLOB-Speicher arbeiten kann.

### <a name="log-exceptions"></a>Protokollieren von Ausnahmen

Nur fügen Sie keine *Ausnahme. ToString()* in Ihrem Protokollierungscode. Kontextinformationen bleiben. Bleiben sie im Fall von SQL-Fehlern die SQL-Fehlernummer. Enthalten Sie für alle Ausnahmen Kontextinformationen, die Ausnahme selbst und die inneren Ausnahmen um sicherzustellen, dass alles, was Sie bereitstellen, die für die Problembehandlung benötigt werden. Informationen zum Sitzungskontext kann z. B. den Namen des Servers, einer Transaktions-ID und einen Benutzernamen ein (aber nicht das Kennwort oder geheimen Schlüssel) enthalten.

Wenn Sie für jeden Entwickler, die das richtige tun mit der Ausnahme, die Protokollierung benötigen, nicht einige davon. Um sicherzustellen, dass es der rechten Seite Art jedes Mal abgeschlossen wird, direkt in Ihre protokollierungsschnittstelle für die Ausnahmebehandlung zu erstellen: übergeben Sie das Ausnahmeobjekt selbst die Protokollierungsklasse und die Ausnahmedaten ordnungsgemäß in der Protokollierungsklasse zu melden.

### <a name="log-calls-to-services"></a>Log-Aufrufe von Diensten

Es wird dringend empfohlen, dass Sie ein Protokoll schreiben jedes Mal, wenn Ihre app an einen Dienst aufruft, ob es sich um eine Datenbank oder eine REST-API oder einen externen Dienst handelt. Enthalten Sie in Ihren Protokollen nicht nur ein Hinweis auf Erfolg oder Fehler, aber wie lange jede Anforderung gedauert hat. In der Cloud-Umgebung sehen Sie häufig Probleme im Zusammenhang mit ggf. statt des vollständigen Ausfälle. Etwas, das normalerweise 10 Millisekunden dauert möglicherweise plötzlich gestartet, eine Sekunde dauert. Wenn jemand Ihnen, dass Ihre app langsam ist mitteilt, New Relic ansehen sollen, oder den telemetriedienst, Sie haben, und überprüfen Sie, ihre Erfahrungen, und klicken Sie dann Sie möchten, um sehen zu können, sind eigene Protokolle tiefer in die Details, warum es langsam ist.

### <a name="use-an-ilogger-interface"></a>Verwenden Sie eine ILogger-Schnittstelle

Zum Erstellen eines einfachen werden empfohlen, beim Erstellen der Anwendung in einer produktionsumgebung *"ILogger"* Schnittstelle, und halten Sie sich einige Methoden. Dies erleichtert die protokollierungsimplementierung später ändern und nicht durchlaufen Sie Ihren gesamten Code dafür. Können wir verwenden die `System.Diagnostics.Trace` Klasse in der gesamten app Fix It, aber stattdessen verwenden wir es im Hintergrund in einer Protokollierungsklasse, die implementiert *"ILogger"*, und wir *"ILogger"* Methodenaufrufe in der gesamten die app.

Auf diese Weise, wenn Sie Ihre Meinung Ihrer Protokollierung Auswahlelements, können Sie ersetzen [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) mit den verwendeten Protokollierungsmechanismus werden soll. Z. B. Wachstum Ihrer app Sie können festlegen, dass Sie ein umfassenderes Protokollierung-Paket verwenden, z. B. [NLog](http://nlog-project.org/) oder [Enterprise Library Logging Application Block](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) ist ein beliebter protokollierungsframework aber asynchrone Protokollierung ausgeführt werden.)

Ein möglicher Grund für die Verwendung eines Frameworks wie NLog ist, um zu ermöglichen, Aufteilen der Protokollausgabe in separate hohem Volumen und hoher-Wert-Datenspeicher. Mit dem Sie große Datenmengen INFORM effizient zu speichern, die Sie benötigen keine, schnelle Abfragen berücksichtigt werden sollen, und schnellen Zugriff auf Daten von ACT gleichzeitig ausgeführt werden.

### <a name="semantic-logging"></a>Semantische Protokollierung

Ein relativ neues Aussehen Protokollierung durchführen, die mehr nützlicher diagnostischen Informationen erzeugen können, finden Sie unter [Enterprise Bibliothek Semantic Logging Application Block (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). SLAB verwendet [Ereignisablaufverfolgung für Windows-Ereignis](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) und [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) -Unterstützung in .NET 4.5, um weitere strukturierter und abgefragt werden Protokolle erstellen können. Sie definieren eine andere Methode für jeden Typ von Ereignis, das Sie sich anmelden, können Sie die Informationen anpassen, die Sie schreiben. Um beispielsweise einen SQL-Datenbank-Fehler zu protokollieren, rufen Sie möglicherweise, eine `LogSQLDatabaseError` Methode. Für diese Art der Ausnahme wissen Sie, dass eine wichtige Information ist die Fehlernummer, damit Sie einen Fehler-Number-Parameter in der Methodensignatur enthalten könnten, und notieren Sie die Fehlernummer als eigenständiges Feld in der Protokolldatensatz, den Sie schreiben. Da die Zahl in einem separaten Feld ist erhalten einfacher und zuverlässig Sie Berichte auf Grundlage der SQL-Fehlernummern, als wenn Sie nur die Fehlernummer in eine Meldungszeichenfolge verkettet wurden, können Sie.

## <a name="logging-in-the-fix-it-app"></a>Die Lösung protokollieren app

### <a name="the-ilogger-interface"></a>Die ILogger-Schnittstelle

Hier ist die *"ILogger"* Schnittstelle in der app zu beheben.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Diese Methoden können Sie zum Schreiben von Protokollen auf den gleichen vier Ebenen von unterstützten *System.Diagnostics*. Die TraceApi-Methoden sind für die Protokollierung von externen Dienst-Aufrufe mit Informationen zur Latenz. Sie können auch einen Satz von Methoden für Debuggen/Verbose-Ebene hinzufügen.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>Die Implementierung der Protokollierung der ILogger-Schnittstelle

Die Implementierung der Schnittstelle ist sehr einfach. Es im Grunde genommen ruft nur in den Standard *System.Diagnostics* Methoden. Der folgende Codeausschnitt zeigt alle drei Methoden Informationen und jeweils eine der anderen.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Aufrufen der Methoden "ILogger"

Jedes Mal, wenn Code in der Fix It-app eine Ausnahme auffängt, ruft er einen *"ILogger"* Methode, um die Ausnahmedetails zu protokollieren. Und jedes Mal, wenn sie die Datenbank, Blob-Dienst oder eine REST-API aufruft, beginnt eine Stoppuhr vor dem Aufruf, beendet die Stoppuhr, wenn der Dienst zurückgibt, und die verstrichene Zeit sowie Informationen zu Erfolg oder Fehler protokolliert.

Beachten Sie, dass die Nachricht des Klassen- und Methodennamen enthält. Es wird empfohlen, sicherzustellen, dass protokollmeldungen identifizieren, welcher Teil der Anwendungscode geschrieben haben, werden.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Jetzt für jedes Mal, wenn die Fix It-app einen Aufruf in SQL-Datenbank erfolgt ist, sehen Sie den Aufruf der Methode, die sie aufgerufen, und genau wie viel Zeit gedauert hat.

![SQL-Datenbank-Abfrage in Protokollen](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Wenn Sie über die Protokolle durchsuchen wechseln, sehen Sie sich, dass die Zeit Datenbankaufrufe nehmen Variable. Dass Informationen hilfreich sein kann: Da die app diese Protokolle können Sie historische Trends in den Informationen zur Leistung des Datenbank-Diensts im Laufe der Zeit analysieren. Z. B. kann ein Dienst schnell in den meisten Fällen jedoch Anforderungen fehlschlagen, oder Antworten verlangsamt möglicherweise die zu bestimmten Zeiten des Tages.

Sie können das gleiche gilt für den Blob-Dienst – durchführen, für jedes Mal, wenn die app eine neue Datei hochlädt, es gibt ein Protokoll und sehen Sie, wie lange dauerte einzelnen Dateien hochzuladen.

![BLOB-Upload-Protokoll](monitoring-and-telemetry/_static/image23.png)

Es ist ein paar zusätzliche Codezeilen, jedes Mal, wenn Sie einen Dienst aufrufen, und jetzt immer jemand, dass sie ein Problem aufgetreten sagt, Sie genau wie das Problem war wissen, ob es sich um einen Fehler war, oder wenn sie nur langsam ausgeführt wurde, zu schreiben. Sie können die Ursache des Problems zu ermitteln, ohne dass eine Remoteverbindung mit einem Server oder Aktivieren der Protokollierung, nachdem der Fehler passiert und hoffen, dass es neu erstellen.

## <a name="dependency-injection-in-the-fix-it-app"></a>Abhängigkeitsinjektion in die Lösung diese app

Sie Fragen sich vielleicht, wie der Repository-Konstruktor im obigen Beispiel die Protokollierung schnittstellenimplementierung abruft:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Verwendet, um die Schnittstelle für die Implementierung die app zu verknüpfen [Abhängigkeitsinjektion](http://en.wikipedia.org/wiki/Dependency_injection)(DI) mit [AutoFac](http://autofac.org/). Mithilfe der Abhängigkeitseinfügung können Sie ein Objekt, das basierend auf einer Schnittstelle an vielen Stellen im gesamten Code verwenden und nur an einem zentralen Ort die Implementierung, die verwendet wird, wenn die Schnittstelle instanziiert wird. Dies erleichtert die Implementierung zu ändern: Möglicherweise möchten z. B. die System.Diagnostics-Protokollierung durch eine NLog-Protokollierung zu ersetzen. Oder für automatisierte Tests möchten Sie möglicherweise eine Pseudoversion des Protokollierungsmoduls zu ersetzen.

Die Fix It-Anwendung nutzt DI in allen Repositorys und alle Controller. Die Konstruktoren der Controllerklassen abzurufen ein *ITaskRepository* Schnittstelle die gleiche Weise wie das Repository, eine protokollierungsschnittstelle ruft:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

Die app verwendet die AutoFac-DI-Bibliothek, um automatisch bereitstellen *TaskRepository* und *Protokollierung* -Instanzen für diese Konstruktoren.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Dieser Code im Wesentlichen sagt: ein Konstruktor an einer beliebigen Stelle muss ein *"ILogger"* Schnittstelle, übergeben Sie Sie in einer Instanz von der *Protokollierung* -Klasse, und dann auf, wenn ein *IFixItTaskRepository*Schnittstelle, übergeben Sie Sie in einer Instanz von der *FixItTaskRepository* Klasse.

[AutoFac](http://autofac.org/) ist eines der vielen Dependency Injection-Frameworks, die Sie verwenden können. Ist syntactically [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), die empfohlen und von Microsoft Patterns and Practices unterstützt.

## <a name="built-in-logging-support-in-azure"></a>Integrierte Protokollierung-Unterstützung in Azure

Azure unterstützt die folgenden Arten von [Protokollierung für Web-Apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- System.Diagnostics-Ablaufverfolgung (können Sie ein-und ausschalten und Ebenen im laufenden Betrieb ohne Neustart der Website).
- Windows-Ereignisse.
- IIS-Protokolle (HTTP/FREB).

Azure unterstützt die folgenden Arten von [Protokollierung in Cloud Services](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- System.Diagnostics-Ablaufverfolgung.
- Leistungsindikatoren.
- Windows-Ereignisse.
- IIS-Protokolle (HTTP/FREB).
- Überwachen von benutzerdefinierten Verzeichnis.

Die Fix It-app verwendet die System.Diagnostics-Ablaufverfolgung. Sie müssen zum Aktivieren der Protokollierung in eine Web-app System.Diagnostics lediglich kippen einen Switch in das Portal oder die REST-API aufrufen. Im Portal klicken Sie auf die **Konfiguration** Registerkarte für die Website, und scrollen Sie nach unten, um finden Sie unter der **Application Diagnostics** Abschnitt. Sie können aktivieren oder Deaktivieren der Protokollierung, und wählen den gewünschten Protokolliergrad. Sie haben Azure die Protokolle im Dateisystem oder in ein Speicherkonto zu schreiben.

![App-Diagnose und Website-Diagnose in der Registerkarte "konfigurieren"](monitoring-and-telemetry/_static/image24.png)

Nach der Aktivierung der diagnoseprotokollierung in Azure sehen Sie Protokolle im Ausgabefenster von Visual Studio, wie sie erstellt werden.

![Menü "Streaming Logs"](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menü "Streaming Logs"](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Sie können auch veranlassen, Protokolle, die in Ihr Speicherkonto geschrieben und zeigen sie mit jedem tool, das kann Azure Storage-Tabelle auf den Dienst zugreifen, z. B. **Server-Explorer** in Visual Studio oder [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/).

![Protokolle im Server-Explorer](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Zusammenfassung

Es ist sehr einfach, eine Out-of-the-Box Telemetriesystem zu implementieren, Protokollierung, die in Ihrem eigenen Code zu instrumentieren und Konfigurieren der Protokollierung in Azure. Und bei der produktionsprobleme auftreten, die Kombination von ein Telemetriesystem und benutzerdefinierte Protokolle können Sie Probleme schnell zu lösen, bevor sie die wichtigsten Probleme für Ihre Kunden werden.

In der [im nächsten Kapitel](transient-fault-handling.md) betrachten wir Gewusst wie: Behandeln von vorübergehenden Fehlern, damit sie produktionsprobleme, die Sie untersuchen, werden nicht.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen.

Dokumentation in erster Linie zur Telemetrie:

- [Microsoft Patterns and Practices - Leitfaden zur Azure](https://msdn.microsoft.com/library/dn568099.aspx). Instrumentation und Telemetrie Anleitungen, Leitfaden zur Dienstmessung Service, Überwachung des Integritätsendpunkts Muster und Laufzeit-neukonfigurationsmuster angezeigt.
- [In der Cloud Berührpunkte Cent: Aktivieren der neuen Leistungsüberwachung Relic auf Azure-Websites](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Bewährte Methoden für den Entwurf umfangreicher Dienste auf Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Whitepaper von Mark Simms und Michael Thomassy. Finden Sie im Abschnitt Telemetrie und Diagnose.
- [Entwicklung der nächsten Generation mit Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). MSDN Magazine-Artikel.

Die Dokumentation hauptsächlich über die Protokollierung:

- [Semantic Logging Application Block (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie stellt den Fall für die semantische Protokollierung mit SLAB.
- [Erstellen von strukturierten und sinnvolle-Protokollen mit der semantischen Protokollierung](https://channel9.msdn.com/Events/Build/2013/3-336). (Video) Julianischen Dominguez stellt den Fall für die semantische Protokollierung mit SLAB.
- [EF6 SQL-Protokollierung – Teil 1: einfache Protokollierung](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers zeigt, wie zum Protokollieren von Abfragen, die von Entity Framework in EF 6 ausgeführt wird.
- [Verbindungsresilienz und Abfangen von Befehlen mit Entitätsframework in einer ASP.NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Viertens veranschaulicht in einer tutorialreihe neun-Teil der Abfangfunktion für EF 6-Befehl verwenden, um SQL-Befehle, die an die Datenbank gesendet werden, von Entity Framework zu protokollieren.
- [Verbessern Sie die Protokollierung mithilfe von c# 5.0 Aufrufer-Informationsattribute](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Wie Sie einfach den Namen der aufrufenden Methode protokollieren, ohne fest zu Programmieren in Literale oder mithilfe von Reflektion manuell anfordern.

Dokumentation in erster Linie zur Problembehandlung:

- [Azure-Problembehandlung &amp; Debuggen Blog](https://blogs.msdn.com/b/kwill/).
- [AzureTools – dem Diagnose-Hilfsprogramm verwendet werden, indem Sie das Azure Developer Support-Team](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Stellt und enthält einen Link zum Herunterladen für ein Tool, das auf einer Azure-VM zum Herunterladen und Ausführen einer Vielzahl von Tools für die Diagnose und Überwachung verwendet werden kann. Nützlich, wenn Sie ein Problem auf einem bestimmten virtuellen Computer zu diagnostizieren müssen.
- [Problembehandlung bei Web-Apps in Azure App Service mithilfe von Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Ein ausführliches Tutorial für erste Schritte mit System.Diagnostics-Ablaufverfolgung und Remotedebuggen.

Videos:

- [FailSafe: Erstellen von skalierbaren, robusten Clouddiensten](https://channel9.msdn.com/Series/FailSafe). Teil 9-Reihe von Marc Mercuri, Ulrich Homann und Mark Simms. Bietet allgemeine Konzepte und architektonischen Prinzipien auf eine Weise sehr zugegriffen werden kann und interessante Geschichten, die von Microsoft Customer Advisory Teams (CAT) die Erfahrung für tatsächliche Kunden gezeichnet werden. Episoden 4 und 9 sind zur Überwachung und Telemetrie. 9-Episode enthält einen Überblick über die Überwachung von Diensten MetricsHub, AppDynamics, New Relic und PagerDuty.
- [Erstellen von Big: Erfahrungen aus dem Azure-Kunden – Teil II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms spricht über Entwerfen für Fehler, und alles instrumentieren. Ähnlich wie die Failsafe-Serie, aber wird auf Weitere Gewusst-wie-Details.

Codebeispiel:

- [Clouddienstgrundlagen in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Die Microsoft Azure Customer Advisory Team erstellte beispielanwendung. Telemetriedaten und Protokollierung Methoden veranschaulicht, wie in den folgenden Artikeln beschrieben. Das Beispiel implementiert die anwendungsprotokollierung mit [NLog](http://nlog-project.org/). Verwandte Dokumentation finden Sie in der [Reihe von vier TechNet-Wiki-Artikel zur Protokollierung und Telemetrie](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Zurück](design-to-survive-failures.md)
> [Weiter](transient-fault-handling.md)
