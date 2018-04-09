---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Überwachung und Telemetrie (Building Real-World Cloud Apps with Azure) | Microsoft Docs
author: MikeWasson
description: Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2015
ms.topic: article
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: d58c495b3888c146a2a9bc831865cf7cc0d94c7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Überwachung und Telemetrie (Building Real-World Cloud Apps with Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Weitere Informationen zu e-Book herunterladen, finden Sie unter [im Kapitel über das erste](introduction.md).


Viele Leute basieren auf Kunden zu informieren, wenn ihre Anwendung ausgefallen ist. Das ist eigentlich keine bewährte Methode eine beliebige Stelle, und zwar insbesondere nicht in der Cloud. Besteht keine Garantie der kurze Benachrichtigung, und wenn Sie benachrichtigt werden, führen Sie häufig erhalten minimale oder eine irreführende Daten über was passiert ist. Mit guten Telemetrie und protokollierungssysteme, die Sie was passiert berücksichtigen können mit der app, und wenn etwas geht falschen sofort festzustellen und nützliche Problembehandlungsinformationen zur Bearbeitung haben.

## <a name="buy-or-rent-a-telemetry-solution"></a>Erwerben Sie oder zu vermieten Sie Telemetrie-Lösung

> [!NOTE]
> In diesem Artikel wurde geschrieben, bevor Sie [Application Insights](https://azure.microsoft.com/services/application-insights/) freigegeben wurde. Application Insights ist die bevorzugte Methode für Telemetrie-Lösungen in Azure. Finden Sie unter [Einrichten von Application Insights für Ihre ASP.NET-Website](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) für Weitere Informationen.


Eine der Aufgaben, die über die Cloudumgebung hervorragende des ist die für den Kauf oder den besten Weg zum Sieg vermieten wirklich sehr einfach. Telemetrie ist ein Beispiel. Ohne großen Aufwand erhalten Sie eine gute Telemetriesystem bis und ausführen, sehr kostengünstig. Es gibt eine Reihe von hervorragende Partnern, die in Azure zu integrieren, und einige von ihnen freie Ebenen – haben, sodass Sie grundlegende Telemetrie Nothing abrufen können. Hier sind nur einige der Einträge in Azure derzeit verfügbar:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

Ab März 2015 [Microsoft Application Insights für Visual Studio Online](https://azure.microsoft.com/documentation/articles/app-insights-get-started/) wurde noch nicht freigegeben, doch sind in Vorschau auszuprobieren. [Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) umfasst auch die Überwachungsfunktionen.

Wir müssen schnell durchlaufen New Relic einrichten, um anzuzeigen, wie einfach es ist ein Telemetriesystem verwenden.

Registrieren Sie sich für den Dienst an, in der Azure-Verwaltungsportal. Klicken Sie auf **neu**, und klicken Sie dann auf **Store**. Die **wählen Sie ein Add-on** Dialogfeld wird angezeigt. Führen Sie einen Bildlauf nach unten, und klicken Sie auf **New Relic**.

![Wählen Sie ein Add-on](monitoring-and-telemetry/_static/image1.png)

Klicken Sie auf den Pfeil nach rechts, und wählen Sie die gewünschte Ebene. Für diese Demo verwenden wir die free-Tarif.

![Personalisieren-Add-On](monitoring-and-telemetry/_static/image2.png)

Klicken Sie auf den Pfeil nach rechts, bestätigen die "Bestellung" und New Relic jetzt wird als ein Add-on im Portal.

![Überprüfen Sie die Bestellung](monitoring-and-telemetry/_static/image3.png)

![Neue Relic Add-on im Verwaltungsportal](monitoring-and-telemetry/_static/image4.png)

Klicken Sie auf **Verbindungsinformationen**, und kopieren Sie den Lizenzschlüssel.

![Verbindungsinformationen](monitoring-and-telemetry/_static/image5.png)

Wechseln Sie zu der **konfigurieren** Registerkarte für Ihre Web-app im Portal festgelegt **Leistungsüberwachung** auf **Add-On**, und legen Sie die **Add-On auswählen** Dropdown-Liste aus, um **New Relic**. Klicken Sie dann auf **speichern**.

![New Relic in der Registerkarte "konfigurieren"](monitoring-and-telemetry/_static/image6.png)

Installieren Sie das neue Relic NuGet-Paket in Ihrer app in Visual Studio.

![Entwickleranalysen in der Registerkarte "konfigurieren"](monitoring-and-telemetry/_static/image7.png)

Die app in Azure bereitstellen und verwenden. Erstellen Sie einige korrigieren Aufgaben aus, um eine Aktivität für New Relic überwachen bereitzustellen.

Kehren Sie zurück an die **New Relic** auf der Seite der **Add-ons** das Portal und klicken Sie auf der Registerkarte **verwalten**. Das Portal leitet Sie zu New Relic-Verwaltungsportal, einmaliges Anmelden für die Authentifizierung verwenden, daher ist es nicht Ihre Anmeldeinformationen erneut einzugeben. Die Seite "Übersicht" stellt eine Vielzahl von Leistungsstatistiken. (Klicken Sie auf das Bild, um die vollständige Übersicht über die Seitengröße anzuzeigen.)

[![Neue Relic Registerkarte "Überwachung"](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Hier sind nur einige der Statistiken, die Sie sehen können:

- Durchschnittliche Antwortzeit zu unterschiedlichen Zeiten des Tages.

    ![Antwortzeit](monitoring-and-telemetry/_static/image10.png)
- Durchsatzraten (bei den Anforderungen pro Minute) zu unterschiedlichen Zeiten des Tages.

    ![Durchsatz](monitoring-and-telemetry/_static/image11.png)
- Server CPU-Zeitaufwand für die Behandlung von anderen HTTP-Anforderungen.

    ![Web-Transaktionszeiten](monitoring-and-telemetry/_static/image12.png)
- CPU-Zeit, die für verschiedene Teile des Anwendungscodes:

    ![Ablaufverfolgungsdetails](monitoring-and-telemetry/_static/image13.png)
- Leistungsverlauf Statistiken.

    ![Leistungsverlauf](monitoring-and-telemetry/_static/image14.png)
- Aufrufe externer Dienste wie z. B. die Blob-Dienst und die Statistik zur zuverlässigen und reaktionsfähig wurde der Dienst.

    ![Externe Dienste](monitoring-and-telemetry/_static/image15.png)

    ![Externe Dienste](monitoring-and-telemetry/_static/image16.png)

    ![Externen Dienst](monitoring-and-telemetry/_static/image17.png)
- Informationen dazu, wo in der ganzen Welt oder, in dem in der US-Web-app Datenverkehr stammt.

    ![Geography](monitoring-and-telemetry/_static/image18.png)

Sie können auch Berichte und Ereignisse einrichten. Sie können z. B. sagen Sie Fehler bandbreitenvorlagen jederzeit, senden Sie eine e-Mail an alert Supportmitarbeiter, um das Problem.

![Berichte](monitoring-and-telemetry/_static/image19.png)

New Relic ist nur ein Beispiel eines Systems Telemetrie. Sie können all diese von anderen Diensten sowie abrufen. Der Vorteil der Cloud ist, ohne Code schreiben zu müssen und für die minimale oder keine Ausgaben, plötzlich können Sie viel mehr Informationen wie Ihre Anwendung verwendet wird und was Ihre Kunden tatsächlich auftreten.

<a id="log"></a>
## <a name="log-for-insight"></a>Protokoll Einblicke

Ein Telemetrie-Paket ist ein guter erster Schritt allerdings weiterhin bestehen, Ihren eigenen Code zu instrumentieren. Der telemetriedienst Aufschluss darüber, wenn ein Problem vorliegt und Aufschluss darüber, welche Kunden treten, aber es nicht erzielen möglicherweise viele Einblick in was im Code.

Sie möchten nicht müssen remote in einem Produktionsserver, um festzustellen, welche Vorgänge die app ausführt. Die möglicherweise nicht praktikabel, wenn Sie einen Server haben, aber was, wenn Sie haben die Skalierung auf Hunderte von Servern und Sie wissen nicht, welche in remote erforderlich? Die Protokollierung sollte aussagekräftig sind, die Sie niemals in Produktionsservern zum Analysieren und Debuggen von remote Probleme. Sie sollten genügend Informationen anmelden, damit Sie Probleme, die ausschließlich über die Protokolle isolieren können.

### <a name="log-in-production"></a>Melden Sie sich in der Produktion

Viele Leute Aktivieren der Ablaufverfolgung in der Produktion nur, wenn ein Problem vorliegt und sie debuggen möchten. Dies kann zu eine beträchtliche Verzögerung zwischen dem Zeitpunkt, den Sie ein Problem bekannt sind und die Zeit, die Sie hilfreiche Informationen zur Problembehandlung zu erhalten führen. Und die Informationen erhalten Sie möglicherweise nicht für vorübergehende Fehler hilfreich sein.

Was wir in der Cloudumgebung empfiehlt sich in dem Speicher billig ist ist, dass Sie immer in der Produktion anmelden. Auf diese Weise, wenn Fehler auftreten, Ihnen bereits werden protokolliert, und Sie Verlaufsdaten, mit deren Hilfe können analysieren Sie Probleme, die regelmäßig zu unterschiedlichen Zeiten auftreten oder Entwickeln mit der Zeit. Sie können einen Bereinigungsprozess zum Löschen der alten Protokolle automatisieren, aber Sie möglicherweise feststellen, dass es in solchen Prozess einrichten, statt die Protokolle beibehalten werden sollen.

Zusätzliche Kosten für die Protokollierung ist trivial, im Vergleich zu den bei der Problembehandlung Beseitigung Zeit- und kostenintensiv, wenn Sie, dass alle Informationen, die Sie bereits müssen, wenn etwas schief geht speichern können. Klicken Sie dann, wenn jemand besagt diesem Unternehmen arbeitete ein zufälliger Fehler einen Augenblick ca. 8:00 Uhr gestern, aber sie können den Fehler nicht speichern, können Sie leicht feststellen, was das Problem auftrat.

Für weniger als 4 $ ist einen Monat, 50 GB von Protokollen auf Seite beibehalten werden kann, und die Leistungseinbußen bei der Protokollierung trivial, solange Sie daran denken, dass – um Leistungsengpässe vermeiden, stellen Sie sicher, dass die Protokollierung Bibliothek asynchrone wird beibehalten.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Unterscheiden Sie Protokolle, die Kenntnis von Protokollen, die eine Aktion erfordern

Protokolle werden INFORM (Ich möchte Sie etwas wissen) oder ACT (Ich möchte Sie eine Aktion) voneinander. Achten Sie darauf, dass Sie nur ACT-Protokolle, um Probleme zu schreiben, die tatsächlich eine Person oder ein automatisierter Prozess Maßnahmen erfordern. Zu viele ACT-Protokolle erstellt Rauschen, dass zu viel Arbeit gesichtet alles Original Probleme gefunden. Und wenn Ihre ACT-Protokolle automatisch eine Aktion wie etwa das Versenden von e-Mail für die Supportmitarbeiter auszulösen, vermeiden Sie Tausende von Aktionen, die durch ein einzelnes Problem ausgelöst werden können.

In .NET System.Diagnostics-Ablaufverfolgung können Protokolle Fehler-, Warn-, Info und Debug/Verbose-Ebene zugewiesen werden. Sie können ACT aus INFORM-Protokollen durch reservieren Fehlerstufe für ACT-Protokolle, und verwenden die unteren Ebenen für INFORM Protokolle unterscheiden.

![Protokolliergrade](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Konfigurieren von Protokolliergraden zur Laufzeit

Während der Anmeldung immer in der Produktion haben ist, wird eine weitere best Practice protokollierungsframework implementiert, mit dem Sie zur Laufzeit Anpassen der Detailebene, die Sie sich anmelden können, ohne erneute Bereitstellung oder Neustarten Ihrer Anwendung. Beispielsweise wird bei Verwendung der Verfolgungsfunktion in `System.Diagnostics` können Fehler-, Warn-, Info und Debug/ausführlich protokolliert. Es wird empfohlen Sie immer Error, Warning, melden und Informationen protokolliert werden, in der Produktion kann dynamisch Debug/Verbose-Protokollierung für die Problembehandlung von Fall zu Fall hinzufügen möchten.

Web-Apps in Azure App Service bieten eine integrierte Unterstützung für das Schreiben von `System.Diagnostics` Protokolle in das Dateisystem, Table Storage oder Blob-Speicher. Sie können verschiedene Protokolliergrade für die einzelnen Ziele Speicher auswählen, und Sie können den Protokolliergrad auf einfache Weise ändern, ohne Ihre Anwendung neu zu starten. Die Blob-Speicher-Unterstützung erleichtert das auszuführende [HDInsight](https://docs.microsoft.com/azure/hdinsight/) Analysis-Aufträge für die Anwendungsprotokolle, da HDInsight weiß, wie direkt arbeiten mit Blob-Speicher.

### <a name="log-exceptions"></a>Log-Ausnahmen

Nicht einfach *Ausnahme. ToString()* in Ihrem Protokollierungscode. Die auslässt Kontextinformationen. Im Fall von SQL-Fehler auslässt es der SQL-Fehlernummer. Umfassen Sie für alle Ausnahmen Kontextinformationen, die Ausnahme selbst und inneren Ausnahmen um sicherzustellen, dass Sie alles bereitstellen, die für die Problembehandlung benötigt werden. Informationen zum Sitzungskontext kann z. B. den Servernamen, eine Transaktions-ID und einen Benutzernamen (jedoch nicht das Kennwort oder vertrauliche Daten!) enthalten.

Wenn Sie für jeden Entwickler darin, die richtigen Schritte mit der Ausnahme, die Protokollierung benötigen, wird nicht von denen einige aus. Um sicherzustellen, dass es das Recht jeder Anforderung abgeschlossen wird, erstellen Sie direkt in Ihre protokollierungsschnittstelle Ausnahmebehandlung: übergeben Sie das Ausnahmeobjekt selbst die Protokollierungsklasse und melden Sie sich die Ausnahmedaten ordnungsgemäß die Protokollierungsklasse.

### <a name="log-calls-to-services"></a>Log-Aufrufe an Dienste

Es wird dringend empfohlen, dass Sie ein Protokoll schreiben jedes Mal, wenn Ihre app an einen Dienst aufruft, ob er an eine Datenbank, eine REST-API oder keine externen Dienste ist. Schließen Sie in Ihre Protokolle nicht nur ein Hinweis auf Erfolg oder Fehler, aber wie lange jeder Anforderung gedauert hat. In der Cloudumgebung wird häufig Probleme im Zusammenhang mit Leistungseinbußen statt umfassende Ausfälle angezeigt werden. Etwas, das 10 Millisekunden normalerweise dauert plötzlich start möglicherweise ein zweites dauert. Wenn jemand Ihnen, dass Ihre app langsam ist mitteilt, wird New Relic ansehen können soll, oder den telemetriedienst haben und überprüfen Hauptbenutzern vorliegt und Sie möchten suchen können eigene Protokolle tauchen Sie in die Details, warum es langsam ist sind.

### <a name="use-an-ilogger-interface"></a>Verwenden Sie eine ILogger-Schnittstelle

Erstellen eine einfachen werden empfohlen, auf diese Weise bei der Erstellung einer produktionsanwendung *ILogger* Schnittstelle, und einige Methoden darin bleiben. Dies vereinfacht die protokollierungsimplementierung bereitgestellt werden später ändern und nicht in Ihren gesamten Code zu diesem Zweck zu. Es konnte unter Verwendung der `System.Diagnostics.Trace` Klasse in der app zu beheben, aber stattdessen verwenden wir es im Hintergrund in einer Protokollierung-Klasse, die implementiert *ILogger*, und stellen wir *ILogger* Methodenaufrufe in der gesamten die app.

Auf diese Weise, wenn Sie Ihre Meinung Ihrer Protokollierung umfangreichere, stellen können Sie ersetzen [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) mit den Protokollierungsmechanismus werden sollen. Beispielsweise wenn Ihre app wächst Sie können festlegen, dass Sie so verwenden Sie ein umfassenderes Protokollierung Paket wie z. B. [NLog](http://nlog-project.org/) oder [Enterprise Library-Anwendungsblock Protokollierung](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) eine andere beliebte protokollierungsframework ist jedoch nicht asynchronen protokollierungs.)

Ein möglicher Grund für die Verwendung von ein Framework wie NLog ist zur Erleichterung der Protokollausgabe in separaten hohem Volumen und hoher / Wert-Datenspeicher unterteilen. Mit dem Sie große Datenmengen INFORM effizienter zu speichern, die nicht schnelle Abfragen auf und schnellen Zugriff auf Daten von ACT gleichzeitig ausgeführt werden müssen.

### <a name="semantic-logging"></a>Semantische Protokollierung

Eine relativ neue Möglichkeit Protokollierung ausführen können, die weitere nützliche Diagnoseinformationen erstellt werden kann, finden Sie unter [Enterprise Library semantische Protokollierung Application Block (Bereich)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Bereich verwendet [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) und [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) -Unterstützung in .NET 4.5, um weitere strukturierte und des queryable-Protokolle erstellen können. Sie definieren eine andere Methode für jede Art von Ereignis, das Sie sich anmelden, dem Sie die Anpassung der Informationen, die Sie schreiben kann. Um beispielsweise einen SQL-Datenbank-Fehler zu protokollieren, rufen Sie möglicherweise, eine `LogSQLDatabaseError` Methode. Art der Ausnahme bedeutet dies, dass eine wichtige Information die Fehlernummer ist, sodass Sie einen Fehler-Number-Parameter in der Methodensignatur enthalten und zeichnen Sie die Nummer des Fehlers als ein separates Feld in der Systemprotokoll-Datensatz, den Sie schreiben konnte. Da die Zahl in einem separaten Feld erhalten mehr einfach und zuverlässig Sie Berichte auf Grundlage der SQL-Fehlernummern als könnten Sie, wenn Sie nur die Nummer des Fehlers in eine Meldungszeichenfolge verkettet wurden.

## <a name="logging-in-the-fix-it-app"></a>Die Lösung Fehleranzahl app

### <a name="the-ilogger-interface"></a>ILogger-Schnittstelle

So sieht die *ILogger* Schnittstelle in der app zu beheben.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Diese Methoden ermöglichen es Ihnen, auf den gleichen vier Ebenen von unterstützten Protokolle schreiben *System.Diagnostics*. Die TraceApi-Methoden sind für die Protokollierung von externen Dienstaufrufe mit Informationen zur Latenz. Sie können auch einen Satz von Methoden zum Debuggen/Verbose-Ebene hinzufügen.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>Die Implementierung der Protokollierung der ILogger-Schnittstelle

Die Implementierung der Schnittstelle ist sehr einfach. Er im Grunde ruft nur in den Standard *System.Diagnostics* Methoden. Der folgende Codeausschnitt zeigt alle drei Methoden Informationen und jeweils von den anderen.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Aufrufen der ILogger-Methoden

Jedes Mal, wenn Code in der app zu beheben, eine Ausnahme abfängt, ruft eine *ILogger* Methode, um die Details der Ausnahme protokollieren. Und jedes Mal, wenn sie die Datenbank, Blob-Dienst oder eine REST-API aufruft, startet eine Stoppuhr vor dem Aufruf, wann die Stoppuhr beendet, wenn der Dienst zurückgibt, und die verstrichene Zeit zusammen mit Informationen zum Erfolg oder Fehler protokolliert.

Beachten Sie, dass die protokollmeldung den Klassennamen und den Namen der Methode enthält. In diesem Zusammenhang empfiehlt es sich, sicherzustellen, dass protokollmeldungen identifizieren, welcher Teil des Anwendungscodes, die sie geschrieben wurde.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Nun für jedes Mal die app zu beheben einen Aufruf zur SQL-Datenbank erfolgt ist, sehen Sie den Aufruf der Methode, die davon aufgerufen, und es ermittelt, genau wie zu lange gedauert hat.

![SQL-Datenbankabfrage in Protokollen](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Wenn Sie über die Protokolle durchsuchen wechseln, sehen Sie sich, dass die Zeit Datenbankaufrufe nehmen Variable ist. Informationen hilfreich sein könnte: Da die app alle Protokolle können Sie historische Trends in der Datenbank Leistung des Diensts über die Zeit wird analysieren. Z. B. ein Dienst möglicherweise schnell in den meisten Fällen jedoch fehlschlagen, Anforderungen oder Antworten zu bestimmten Zeiten des Tages verlangsamt möglicherweise.

Sie können das gleiche gilt für den Blob-Dienst – vornehmen, für jedes Mal, wenn die app wird eine neue Datei hochgeladen, es ist ein Protokoll und sehen Sie, wie lange gedauert hat jede Datei hochladen.

![BLOB-Upload-Protokoll](monitoring-and-telemetry/_static/image23.png)

Es ist ein paar zusätzliche Codezeilen, jedes Mal, wenn Sie Aufrufen eines Dienstes, und wenn jemand, dass die Ausführung zu einem Problem besagt, Sie jetzt genau wie das Problem wurde wissen, ob es sich um ein Fehler aufgetreten, oder wenn sie nur langsam ausgeführt wurde, zu schreiben. Sie können die Ursache des Problems zu ermitteln, ohne auf einem Server remote oder Aktivieren der Protokollierung, nachdem der Fehler geschieht und hoffen, neu erstellt.

## <a name="dependency-injection-in-the-fix-it-app"></a>Abhängigkeitsinjektion in die Lösung diese app

Sie Fragen sich vielleicht, wie der Repository-Konstruktor im oben gezeigten Beispiel wird die Protokollierung Implementierung abruft:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Zum Einrichten der Schnittstelle für die Implementierung die app von Netzwerkdaten verwendet [Abhängigkeitsinjektion](http://en.wikipedia.org/wiki/Dependency_injection)(DI) mit [AutoFac](http://autofac.org/). DI können Sie mithilfe eines Objekts basierend auf einer Schnittstelle an vielen Stellen im gesamten Code und nur die Implementierung an einem Ort angeben, die verwendet wird, wenn die Schnittstelle instanziiert wird. Dies vereinfacht die Implementierung zu ändern: Möglicherweise möchten z. B. die System.Diagnostics-Protokollierung durch eine Protokollierung für NLog zu ersetzen. Oder für automatisierte Tests möchten Sie möglicherweise eine Pseudoversion des Protokollierungsmoduls ersetzen.

Die Anwendung beheben verwendet DI in allen Repositorys und aller Controller. Die Konstruktoren des der Controllerklassen Abrufen einer *ITaskRepository* Schnittstelle die gleiche Weise wie das Repository eine protokollierungsschnittstelle ruft:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

Die app mithilfe die Bibliothek AutoFac DI automatisch bereitstellen *TaskRepository* und *Protokollierung* Instanzen für diese Konstruktoren.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Dieser Code im Wesentlichen sagt, dass an einer beliebigen Stelle ein Konstruktor muss ein *ILogger* Schnittstelle, übergeben Sie Sie in einer Instanz von der *Protokollierung* -Klasse, und dann auf, wenn ein *IFixItTaskRepository*-Schnittstelle, übergeben Sie Sie in einer Instanz von der *FixItTaskRepository* Klasse.

[AutoFac](http://autofac.org/) ist einer der vielen Abhängigkeit-Injection-Frameworks, die Sie verwenden können. Ist eine beliebte [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), d. h. empfohlen und vom Microsoft Patterns and Practices unterstützt.

## <a name="built-in-logging-support-in-azure"></a>Unterstützung für integrierte Protokollierung in Azure

Azure unterstützt die folgenden Arten von [Protokollierung für Web-Apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- System.Diagnostics-Ablaufverfolgung (Sie können ein-und ausschalten und zielsatzebenen im Handumdrehen ohne Neustart der Website).
- Windows-Ereignisse.
- IIS Logs (HTTP/FREB).

Azure unterstützt die folgenden Arten von [Clouddiensten anmelden](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- System.Diagnostics-Ablaufverfolgung.
- Leistungsindikatoren.
- Windows-Ereignisse.
- IIS Logs (HTTP/FREB).
- Überwachen von benutzerdefinierten Verzeichnis.

Die korrigieren-app verwendet die System.Diagnostics-Ablaufverfolgung. Alles, was Sie zum Aktivieren der Protokollierung in einer Web-app System.Diagnostics tun müssen ist einen Switch in das Portal wechseln oder die REST-API aufrufen. Klicken Sie im Portal auf der **Konfiguration** Ihrer Website auf der Registerkarte, und führen Sie einen Bildlauf nach unten zu finden Sie unter der **Application Diagnostics** Abschnitt. Sie können aktivieren oder deaktivieren Sie die Protokollierung, und wählen den gewünschten Protokolliergrad. Sie können Azure-Protokolle im Dateisystem oder in ein Speicherkonto geschrieben haben.

![App-Diagnose und Website-Diagnose in der Registerkarte "konfigurieren"](monitoring-and-telemetry/_static/image24.png)

Nach der Aktivierung der Protokollierung in Azure können Sie die Protokolle im Ausgabefenster von Visual Studio sehen, wie sie erstellt werden.

![Menü "Logs" Streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menü "Logs" Streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Außerdem können Sie Protokolle in Ihrem Speicherkonto geschrieben und anzeigen, die sie mit jedem tool, kann Azure-Speichertabelle auf den Dienst zugreifen, wie z. B. **Server-Explorer** in Visual Studio oder [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

![Protokolle in Server-Explorer](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Zusammenfassung

Es ist wirklich einfach, eine Out-of-Box Telemetriesystem zu implementieren, Protokollierung in Ihrem eigenen Code zu instrumentieren und Konfigurieren der Protokollierung in Azure. Und wenn Produktion Probleme auftreten, die Kombination von einem Telemetriesystem und benutzerdefinierte Protokolle hilft Probleme schnell zu beheben, bevor sie für Ihre Kunden erhebliche Probleme aufgetreten sind.

In der [nächsten Kapitels](transient-fault-handling.md) betrachten wir vorübergehende Fehler zu behandeln, damit sie Produktion Probleme, die Sie untersuchen, werden nicht.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen.

Die Dokumentation hauptsächlich über Telemetrie:

- [Microsoft Patterns and Practices - Azure-Leitfaden](https://msdn.microsoft.com/library/dn568099.aspx). Instrumentation und Telemetrie Anleitungen, Dienst Messung Anleitungen, Muster Integrität Webendpunkt-Überwachung und Runtime-Neukonfiguration Muster angezeigt.
- [In der Cloud Pinch Cent: Aktivieren der Überwachung auf Azure-Websites New Relic-Leistungsüberwachung](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Bewährte Methoden für den Entwurf umfangreicher Dienste auf Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Whitepaper von Mark Simms und Michael Thomassy. Finden Sie im Abschnitt Telemetrie und Diagnose.
- [Entwicklung der nächsten Generation mit Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). MSDN Magazine-Artikel.

Dokumentation in erster Linie zur Protokollierung:

- [Semantische Protokollierung Anwendungsblock (Bereich)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie stellt den Fall für die semantische Protokollierung mit Bereich.
- [Erstellen von strukturierten und sinnvolle Protokolle mit semantische Protokollierung](https://channel9.msdn.com/Events/Build/2013/3-336). (Video) Julianischen Dominguez stellt den Fall für die semantische Protokollierung mit Bereich.
- [EF6 SQL-Protokollierung – Teil 1: einfache Protokollierung](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers wird gezeigt, wie zum Protokollieren von Abfragen, die von Entity Framework in EF 6 ausgeführt wird.
- [Verbindungsresilienz und Abfangen der Befehl mit dem Entity Framework in einer ASP.NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Vierte veranschaulicht in einer Reihe Tutorial neun-Teil der EF-6-Befehl-Abfangfunktion-Funktion verwenden, um SQL-Befehle an die Datenbank gesendet wird, Entity Framework protokolliert.
- [Verbessern der Protokollierung mithilfe von C#-5.0 Aufrufer-Informationsattribute](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Wie der Name der Aufrufmethode problemlos zu protokollieren, ohne eine feste Programmierung es in Literalen oder mithilfe von Reflektion Bezugsquelle manuell.

Dokumentation in erster Linie zur Problembehandlung:

- [Azure Problembehandlung &amp; Debuggen Blog](https://blogs.msdn.com/b/kwill/).
- [AzureTools – das Diagnosedienstprogramm verwendet, die für das Azure Developer Support-Team](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Stellt ein, und bietet einen Downloadlink für ein Tool, das auf einer Azure-VM herunterladen und Ausführen einer Vielzahl von Tools für Diagnose und Überwachung verwendet werden kann. Der Wert ist nützlich, wenn Sie auf einen bestimmten virtuellen Computer ein Problem diagnostizieren müssen.
- [Problembehandlung bei einer Web-app in Azure App Service mithilfe von Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Ein schrittweises Lernprogramm für erste Schritte mit System.Diagnostics-Protokollierung und Remotedebuggen.

Videos:

- [FailSafe: Erstellen von skalierbaren, robusten Cloud-Dienste](https://channel9.msdn.com/Series/FailSafe). Neun zweiteilige Reihe Marc Mercuri, Ulrich Homann und Mark Simms. Bietet allgemeine Konzepte und Architekturprinzipien auf eine Weise zugegriffen werden kann, und interessante Storys, die von Microsoft Customer Advisory Team (CAT) anstelle von Erfahrungen mit Kunden gezeichnet. Folgen, 4 und 9 sind zur Überwachung und Telemetrie. 9-Episode enthält einen Überblick über die Überwachung von Diensten MetricsHub, AppDynamics New Relic und PagerDuty.
- [Erstellen von Big: Erkenntnisse aus Azure-Kunden – Teil II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms dreht Entwerfen für Fehler und Instrumentieren alles ab. Vergleichbar mit dem Failsafe-Serie, aber wechselt in den Gewusst-wie-Informationen.

Codebeispiel:

- [Grundlagen von Clouddiensten in Azure Cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Beispielanwendung, die von der Microsoft Azure-Kundenberatungsteam erstellt werden. Telemetrie und Protokollierung-Methoden veranschaulicht, wie in den folgenden Artikeln beschrieben. Das Beispiel implementiert die anwendungsprotokollierung mit [NLog](http://nlog-project.org/). Verwandte Dokumentation finden Sie in der [Reihe von vier TechNet Wiki-Artikel zur Protokollierung und Telemetrie](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Zurück](design-to-survive-failures.md)
> [Weiter](transient-fault-handling.md)
