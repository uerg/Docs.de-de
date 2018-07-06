---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: Verwenden asynchroner Methoden in ASP.NET MVC 4 | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen der Erstellung einer asynchronen ASP.NET MVC-Web-Anwendung mithilfe von Visual Studio Express 2012 für Web, das ist eine kostenlose speichern...
ms.author: aspnetcontent
ms.date: 06/06/2012
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 5eb1b6dc3e9166c1ecb8e0d1968e12e8fa07f985
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826399"
---
<a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>Verwenden asynchroner Methoden in ASP.NET MVC 4
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Erstellung einer asynchronen ASP.NET MVC-Web-Anwendung, mit [Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/11), dies ist eine kostenlose Version von Microsoft Visual Studio. Sie können auch [Visual Studio 2012](https://www.microsoft.com/visualstudio/11).
> 
> Es wird ein vollständiges Beispiel für dieses Tutorial auf Github bereitgestellt.  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)


Die ASP.NET MVC 4 [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller(VS.108).aspx) Klasse in Kombination [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) können Sie asynchrone Aktionsmethoden schreiben, die ein Objekt des Typs zurückgeben [Aufgabe&lt;ActionResult&gt; ](https://msdn.microsoft.com/library/dd321424(VS.110).aspx). .NET Framework 4 eingeführt wurde, eine asynchrone Programmierkonzept, das als ein [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) und unterstützt die ASP.NET MVC 4 [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Aufgaben werden durch dargestellt die **Aufgabe** Typ und verwandte Typen in der [System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) Namespace. .NET Framework 4.5 basiert diese asynchrone Unterstützung mit der ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) und [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwörter, die Arbeit mit stellen [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Objekte, die wesentlich komplexer als die vorherige Version asynchrone Methoden. Die ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) -Schlüsselwort ist syntaktische Kurzform zum angeben, die ein Stück Code asynchron auf den anderen Teil des Codes warten soll. Die [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwort stellt einen Hinweis, mit denen Sie kennzeichnen Methoden als aufgabenbasierte asynchrone Methoden dar. Die Kombination von **"await"**, **Async**, und die **Aufgabe** -Objekts macht es viel einfacher Schreiben von asynchronem Code in .NET 4.5. Das neue Modell für asynchrone Methoden wird aufgerufen, die *aufgabenbasierte asynchrone Muster* (**Tippen Sie auf**). In diesem Tutorial wird vorausgesetzt, dass einige Kenntnisse im Umgang mit asynchronen Programmieren ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) und [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwörter und [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Namespace.

Weitere Informationen zur Verwendung von der ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) und [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwörter und [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) -Namespace finden Sie die folgenden Themen.

- [Whitepaper: Asynchronie in .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Häufig gestellte Fragen zu Async/Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Asynchrone Programmierung in Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  Verarbeitung von Anforderungen vom Threadpool

Auf dem Webserver verwaltet das .NET Framework einen Pool von Threads, die auf die Verarbeitung von ASP.NET-ANFORDERUNGEN verwendet werden. Wenn eine Anforderung eingeht, wird ein Thread aus dem Pool verteilt, um diese Anforderung zu verarbeiten. Wenn die Anforderung synchron verarbeitet wird, ist der Thread, der die Anforderung verarbeitet, beschäftigt, während die Anforderung verarbeitet wird und dass der Thread eine andere Anforderung nicht verarbeiten kann.   
  
Dies kann ein Problem darstellen, nicht sein, da der Threadpool groß genug für viele ausgelastete Threads im vorgenommen werden kann. Die Anzahl der Threads im Threadpool ist jedoch beschränkt (Standardmäßig ist der Maximalwert für .NET 4.5 ist 5000). In großen Anwendungen mit hoher Parallelität von lang andauernder Anforderungen möglicherweise alle verfügbaren Threads ausgelastet. Diese Bedingung wird als Threadmangel (Starvation) bezeichnet. Wenn diese Bedingung erreicht wird, stellt der Webserver Anforderungen Warteschlange. Wenn die Anforderungswarteschlange voll wird, lehnt der Webserver Anforderungen mit einem HTTP 503-Status (Server ausgelastet). CLR-Threadpool weist Einschränkungen auf den neuen Thread-Einschleusung. Wenn Parallelität bursty ist (d. h. Ihrer Website plötzlich erhalten eine große Anzahl von Anforderungen) und alle verfügbaren Anforderungsthreads ausgelastet sind aufgrund von Back-End-Aufrufen mit hoher Latenz, die eingeschränkte Thread-Injection-Rate kann Ihre Anwendung sehr schlecht reagiert. Darüber hinaus verfügt jeder neuer Thread hinzugefügt, die an den Threadpool Mehraufwand (z. B. 1 MB Stapelspeicher zu benötigen). Eine Webanwendung mithilfe von synchronen Methoden von Dienstaufrufen für hohe Latenz, in denen überschreitet Threadpool der Warteschleife hinzu, der Standardwert .NET 4.5 Max. 5, 000 Threads würde etwa 5 GB mehr Arbeitsspeicher beansprucht als eine Anwendung kann der Dienst die gleichen Anforderungen mithilfe asynchrone Methoden und nur 50 Threads. Wenn Sie asynchronen Arbeit ausführen, verwenden Sie nicht immer einen Thread. Z. B. Wenn Sie eine asynchrone Webanfrage für den Dienst vornehmen, ASP.NET nicht verwenden alle Threads zwischen den **Async** Methodenaufruf und **"await"**. Verwenden den Threadpool Anforderungen mit hoher Latenz kann auf eine große Menge Arbeitsspeicher belegen und unzureichender Nutzung von Hardware des Servers führen.

## <a name="processing-asynchronous-requests"></a>Verarbeiten von asynchronen Anforderungen

In einer WebApp, die eine große Anzahl von gleichzeitigen Anforderungen, die beim Start angezeigt oder verfügt über bursty (wobei plötzlich nimmt die Parallelität) erhöht die Webdienstaufrufe asynchron die Reaktionsfähigkeit der app. Eine asynchrone Anforderung hat die gleiche Menge an Zeit, als eine synchrone Anforderung zu verarbeiten. Wenn eine Anforderung über einen Webdienst aufrufen, die stellt erfordert zwei Sekunden auf den Abschluss der Anforderung akzeptiert zwei Sekunden, ob sie synchron oder asynchron ausgeführt wird. Jedoch wird nicht während eines asynchronen Aufrufs wird ein Thread blockiert, aus auf andere Anforderungen reagiert, während er darauf wartet, dass die erste Anforderung abgeschlossen. Aus diesem Grund verhindern asynchrone Anforderungen das Wachstum der Anforderung queuing und Thread-Pool, wenn es gibt viele gleichzeitige Anforderungen, die lang andauernde Vorgänge aufrufen.

## <a id="ChoosingSyncVasync"></a>  Auswählen von synchronen oder asynchronen Aktionsmethoden

Dieser Abschnitt enthält Richtlinien für die synchrone oder asynchrone Aktionsmethoden zu verwenden. Diese sind nur Richtlinien. Überprüfen Sie jede Anwendung einzeln, um zu bestimmen, ob asynchrone Methoden können Sie mit der Leistung.

Im Allgemeinen verwenden Sie synchrone Methoden, die folgenden Bedingungen:

- Die Vorgänge sind einfache oder kurzer.
- Einfachheit ist wichtiger als Effizienz.
- Die Vorgänge sind in erster Linie CPU-Vorgänge, anstatt die Vorgänge, die umfangreiche Datenträger- oder Netzwerkauslastung betreffen. Verwenden asynchroner Aktionsmethoden für CPU-gebundene Vorgänge bietet keine Vorteile und führt zu einem Mehraufwand.

  Im Allgemeinen verwenden Sie asynchrone Methoden für die folgenden Bedingungen:

- .NET 4.5 oder höher verwenden, und rufen Sie Dienste, die über asynchrone Methoden genutzt werden können.
- Die Vorgänge sind netzwerkgebunden oder e/A-anstelle von CPU-gebunden.
- Parallelverarbeitung ist wichtiger als die Einfachheit des Codes.
- Möchten Sie einen Mechanismus bereitstellen, mit dem Benutzer eine lang ausgeführte Anforderung abbrechen können.
- Bei der Vorteil, dass das Wechseln von Threads die Kosten für den Kontextwechsel überwiegt. Im Allgemeinen sollten Sie eine Methode asynchron, wenn die synchrone Methode auf dem Anforderungsthread ASP.NET wartet, während keine Funktionen ausführt. Durch Aufruf der asynchronen, wird der Anforderungsthread ASP.NET nicht unterbrochen, keine Funktionen ausführt, während er darauf wartet, dass die webdienstanforderung abgeschlossen.
- Testen der zeigt, dass die blockierenden Vorgänge einen Engpass bei der websiteleistung sind und IIS mehr Anforderungen verarbeitet werden können, durch die Verwendung von asynchronen Methoden für diese blockierenden Aufrufe.

  Im herunterladbare Beispiel wird gezeigt, wie asynchrone Aktionsmethoden effektiv verwendet werden. Das bereitgestellte Beispiel wurde konzipiert, eine einfache Veranschaulichung der asynchronen Programmierung in ASP.NET MVC 4 mithilfe von .NET 4.5. Das Beispiel ist nicht vorgesehen, eine Referenzarchitektur für die asynchrone Programmierung in ASP.NET MVC zu sein. Das Beispielprogramm ruft [ASP.NET Web-API](../../../web-api/index.md) Methoden, die wiederum Aufrufen [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) , lang andauernde Webdienstaufrufe zu simulieren. Die meisten Produktionsanwendungen werden Verwenden asynchroner Aktionsmethoden offensichtliche Vorteile nicht angezeigt werden.   
  
Einige Anwendungen erfordern alle Aktionsmethoden asynchron sein müssen. Konvertieren ein einiger synchroner Aktionsmethoden in asynchrone Methoden bietet häufig die beste effizienzsteigerung für den erforderlichen Arbeitsaufwand auf.

## <a id="SampleApp"></a>  Die Beispielanwendung

Sie können die beispielanwendung aus [ https://github.com/RickAndMSFT/Async-ASP.NET/ ](https://github.com/RickAndMSFT/Async-ASP.NET) auf die [GitHub](https://github.com/) Standort. Das Repository besteht aus drei Projekten:

- *Mvc4Async*: die ASP.NET MVC 4-Projekt, das in diesem Tutorial verwendeten Code enthält. Führt Sie Web-API-Aufrufe für die **WebAPIpgw** Service.
- *WebAPIpgw*: die Web-API ASP.NET MVC 4-Projekt, das implementiert die `Products, Gizmos and Widgets` Controller. Es bietet es sich um die Daten für die *WebAppAsync* Projekt und die *Mvc4Async* Projekt.
- *WebAppAsync*: die ASP.NET Web Forms-Projekt in ein anderes Tutorial verwendet.

## <a id="GizmosSynch"></a>  Die synchrone Aktionsmethode Gizmos

 Der folgende code zeigt die `Gizmos` synchrone Aktionsmethode, die verwendet wird, um eine Liste der Gizmos anzuzeigen. (In diesem Artikel ist ein VoIP (Gizmo) ein fiktives mechanischen Gerät.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

Der folgende code zeigt die `GetGizmos` -Methode des Diensts VoIP (Gizmo).

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

Die `GizmoService GetGizmos` Methode übergibt einen URI zu einem ASP.NET Web-API-HTTP-Dienst die eine Liste der Gizmos Daten zurückgibt. Die *WebAPIpgw* Projekt enthält die Implementierung der Web-API `gizmos, widget` und `product` Controller.  
Die folgende Abbildung zeigt die Ansicht Gizmos aus dem Beispielprojekt.

![Gizmos](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  Erstellen eine asynchrone Gizmos Action-Methode

Das Beispiel verwendet die neue [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) und ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) Schlüsselwörter (verfügbar in .NET 4.5 und Visual Studio 2012) damit vom Compiler zuständig für die Verwaltung der für der komplexen Transformations werden asynchrone Programmierung. Der Compiler ermöglicht Ihnen das Schreiben von Code zu verwenden, die die # synchrone ablaufsteuerung erstellt, und der Compiler wendet automatisch die Transformationen, die erforderlichen Rückrufe verwendet, um zu vermeiden, sodass Threads blockiert.

Der folgende code zeigt die `Gizmos` synchrone Methode und die `GizmosAsync` asynchrone Methode. Wenn Ihr Browser unterstützt das [HTML 5 `<mark>` Element](http://www.w3.org/wiki/HTML/Elements/mark), sehen Sie die Änderungen im `GizmosAsync` in gelbe Markierung.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 Die folgenden Änderungen wurden angewendet, um zu ermöglichen die `GizmosAsync` asynchron sein müssen.

- Die Methode ist mit markiert die [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) -Schlüsselwort, das weist den Compiler an, um Rückrufe für Teile des Texts zu generieren und zum automatischen Erstellen von eine `Task<ActionResult>` , der zurückgegeben wird.
- &quot;Asynchrone&quot; dem Methodennamen angefügt wurde. Anfügen von "Async" ist nicht erforderlich, jedoch ist die Konvention, wenn Sie asynchrone Methoden zu schreiben.
- Der Rückgabetyp geändert wurde, aus `ActionResult` zu `Task<ActionResult>`. Der Rückgabetyp der `Task<ActionResult>` stellt derzeit ausgeführte Arbeit dar, und stellt die Aufrufer der Methode mit einem Handle über den Abschluss des asynchronen Vorgangs warten bereit. In diesem Fall ist der Aufrufer den Webdienst. `Task<ActionResult>` Stellt laufende Arbeiten mit dem Ergebnis `ActionResult.`
- Die ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) -Schlüsselwort auf den Aufruf des Webdiensts angewendet wurde.
- Die asynchrone Web-API aufgerufen wurde (`GetGizmosAsync`).

In der die `GetGizmosAsync` Methode Text eine weitere asynchrone Methode `GetGizmosAsync` aufgerufen wird. `GetGizmosAsync` wird sofort zurückgegeben, eine `Task<List<Gizmo>>` , die schließlich abgeschlossen, wenn die Daten verfügbar sind. Da Sie nicht möchten, dass für nichts weiter tun, bevor Sie die VoIP (Gizmo) Daten haben wird, wartet der Code auf die Aufgabe (mithilfe der **"await"** Schlüsselwort). Können Sie die **"await"** Schlüsselwort nur in Methoden, die mit Anmerkungen versehen, mit der **Async** Schlüsselwort.

Die **"await"** Schlüsselwort wird den Thread nicht blockiert, bis die Aufgabe abgeschlossen ist. Es registriert den Rest der Methode als Rückruf für den Task, und gibt sofort zurück. Wenn schließlich die erwartete Aufgabe abgeschlossen ist, wird dieser Rückruf aufgerufen und somit fortsetzen die Ausführung der Methode nach rechts, wo er unterbrochen. Weitere Informationen zur Verwendung von der ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) und [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwörter und die [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) -Namespace finden Sie unter der [Async-Verweise](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Der folgende code zeigt die `GetGizmos` und `GetGizmosAsync` Methoden.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 Die asynchrone Änderungen ähneln den versucht, den **GizmosAsync** oben. 

- Die Signatur der Methode wurde mit Anmerkung versehen, mit der [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) -Schlüsselwort, der Rückgabetyp wurde geändert in `Task<List<Gizmo>>`, und *Async* dem Methodennamen angefügt wurde.
- Die asynchrone ["HttpClient"](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) Klasse wird verwendet, statt die ["Webclient"](https://msdn.microsoft.com/library/system.net.webclient.aspx) Klasse.
- Die ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) Schlüsselwort wurde angewendet, um die ["HttpClient"](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) asynchrone Methoden.

Die folgende Abbildung zeigt die Ansicht für die asynchrone VoIP (Gizmo).

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

Die Browser-Präsentation der Daten Gizmos ist identisch mit der Ansicht, die durch den synchronen Aufruf erstellt. Der einzige besteht Unterschied darin, dass die asynchrone Version bieten eine bessere Leistung bei hoher Auslastung möglicherweise.

## <a id="Parallel"></a>  Mehrere Vorgänge ausführen parallel

Asynchrone Aktionsmethoden haben einen deutlicher Vorteil über synchrone Methoden auf, wenn eine Aktion mehrere unabhängige Vorgänge ausführen muss. In diesem Beispiel bereitgestellt, die synchrone Methode `PWG`(für Produkte, Widgets und Gizmos) zeigt die Ergebnisse der drei Aufrufe des Webdiensts eine Liste von Produkten, Widgets und Gizmos abrufen. Die [ASP.NET Web-API](../../../web-api/index.md) services-Projekt, das diese ermöglicht verwendet [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) aufruft, um Latenz oder ein langsames Netzwerk zu simulieren. Wenn die Verzögerung festgelegt ist, auf 500 Millisekunden, die den asynchronen `PWGasync` Methode nimmt ein wenig mehr als 500 Millisekunden abgeschlossen und die synchrone `PWG` Version übernimmt 1.500 Millisekunden. Die synchrone `PWG` Methode ist in den folgenden Code gezeigt.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

Die asynchrone `PWGasync` Methode ist in den folgenden Code gezeigt.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

Die folgende Abbildung zeigt die Ansicht zurückgegeben, die von der **PWGasync** Methode.

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>  Verwenden eines Abbruchtokens

Asynchrone Aktionsmethoden zurückgeben `Task<ActionResult>`abgebrochen werden kann, sind, die sie akzeptieren ein [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) Parameter an, wenn eine zur Verfügung steht die [AsyncTimeout](https://msdn.microsoft.com/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx) Attribut. Der folgende code zeigt die `GizmosCancelAsync` Methode mit einem Timeout von 150 Millisekunden.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

Der folgende Code zeigt die GetGizmosAsync-Überladung, die akzeptiert eine [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) Parameter.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

Wählen Sie in der beispielanwendung bereitgestellt, die *Abbruch-Token-Demo* verknüpfen Aufrufe der `GizmosCancelAsync` Methode und der Abbruch des asynchronen Aufrufs veranschaulicht.

## <a id="ServerConfig"></a>  Server-Konfiguration für hohe Parallelität/hohe Wartezeit für Aufrufe des Webdiensts

Um die Vorteile einer asynchronen Webanwendung nutzen zu können, müssen Sie einige Änderungen an der Standardkonfiguration Server vornehmen. Beachten Sie beim Konfigurieren und Belastungstests in Ihre Webanwendung für die asynchrone Bedenken Sie Folgendes.

- Windows 7, Windows Vista und alle Windows-Clientbetriebssysteme höchstens 10 gleichzeitige Anforderungen. Sie benötigen ein Windows Server-Betriebssystem zum die Vorteile der asynchronen Methoden unter hoher Last finden Sie unter.
- Registrieren Sie .NET 4.5 mit IIS über eine Eingabeaufforderung mit erhöhten Rechten aus:  
  %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_Regiis -i  
  Finden Sie unter [ASP.NET IIS Registration-Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Sie müssen möglicherweise erhöhen die [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) Begrenzung für Anforderungswarteschlange vom Standardwert 1.000 auf 5.000. Wenn die Einstellung zu niedrig ist, wird möglicherweise angezeigt [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) ablehnen von Anforderungen mit einem HTTP 503-Status. So ändern Sie die HTTP.sys-Grenze der Warteschlange:

    - Öffnen Sie IIS-Manager, und navigieren Sie in den Bereich des Anwendungspools.
    - Klicken Sie mit der rechten Maustaste auf den Zielanwendungspool, und wählen Sie **Erweiterte Einstellungen**.  
        ![Erweiterte](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png)
    - In der **Erweiterte Einstellungen** im Dialogfeld *Warteschlangenlänge* von 1.000 auf 5.000.  
        ![Warteschlangenlänge](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
  Beachten Sie in den oben genannten Images als v4. 0, .NET Framework aufgeführt ist, auch wenn der Anwendungspool .NET 4.5 verwendet. Um diese Abweichung zu verstehen, sehen Sie Folgendes ein:

    - [.NET-versionsverwaltung und Festlegung von Zielversionen – .NET 4.5 ist ein direktes Upgrade auf .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [Eine IIS-Anwendung oder AppPool, Verwendung von ASP.NET 3.5 und nicht als Version 2.0 einrichten](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [.NET Framework-Versionen und -Abhängigkeiten](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Wenn Ihre Anwendung von Webdiensten mithilfe wird oder System.NET Kommunikation mit einem Back-End-über-HTTP-Sie möglicherweise erhöhen müssen die [ConnectionManagement/Maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) Element. Bei ASP.NET-Anwendungen wird dies durch das Feature für die automatische Konfiguration 12 Mal die Anzahl der CPUs beschränkt. Dies bedeutet, dass für eine Quad-Prozedur, höchstens 12 stehen \* 4 = 48 gleichzeitige Verbindungen zu einem IP-Endpunkt. Da dies mit verbunden ist [AutoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), die einfachste Möglichkeit zum erhöhen `maxconnection` in einer ASP.NET-Anwendung-Anwendung besteht darin, festzulegen [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) programmgesteuert in die von `Application_Start` -Methode in der die *"Global.asax"* Datei. Siehe Beispiel für ein Beispiel herunterladen.
- In .NET 4.5, den Standardwert von 5000 für ["maxConcurrentRequestsPerCPU"](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) sollten ausreichend sein.
