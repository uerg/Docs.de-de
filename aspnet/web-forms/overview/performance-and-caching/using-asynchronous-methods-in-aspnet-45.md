---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: Verwenden asynchroner Methoden in ASP.NET 4.5 | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen der Erstellung einer asynchronen ASP.NET Web Forms-Anwendung mithilfe von Visual Studio Express 2012 für Web, das ein kostenloses...
ms.author: aspnetcontent
ms.date: 06/06/2012
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 94adc03eeba61310d60ca88a0495c5a2e5dc4cf6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819301"
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>Verwenden asynchroner Methoden in ASP.NET 4.5
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Erstellung einer asynchronen ASP.NET Web Forms-Anwendung, mit [Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/11), dies ist eine kostenlose Version von Microsoft Visual Studio. Sie können auch [Visual Studio 2012](https://www.microsoft.com/visualstudio/11). In den folgenden Abschnitten sind in diesem Tutorial enthalten.
> 
> - [Verarbeitung von Anforderungen vom Threadpool](#HowRequestsProcessedByTP)
> - [Auswählen von synchronen oder asynchronen Methoden](#ChoosingSyncVasync)
> - [Die Beispielanwendung](#SampleApp)
> - [Die synchrone Gizmos-Seite](#GizmosSynch)
> - [Erstellen einer asynchronen Gizmos-Seite](#CreatingAsynchGizmos)
> - [Mehrere Vorgänge ausführen parallel](#Parallel)
> - [Verwenden eines Abbruchtokens](#CancelToken)
> - [Server-Konfiguration für hohe Parallelität/hohe Wartezeit für Aufrufe des Webdiensts](#ServerConfig)
> 
> Ein vollständiges Beispiel wird für dieses Tutorial auf bereitgestellt.  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) auf der [GitHub](https://github.com/) Standort.


ASP.NET 4.5 Web Pages kombiniert [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) können Sie asynchrone Methoden zu registrieren, die ein Objekt des Typs zurückgeben [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). .NET Framework 4 eingeführt wurde, eine asynchrone Programmierkonzept, das als ein [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) und ASP.NET 4.5 unterstützt [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Aufgaben werden durch dargestellt die **Aufgabe** Typ und verwandte Typen in der [System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) Namespace. .NET Framework 4.5 basiert diese asynchrone Unterstützung mit der ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) und [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwörter, die Arbeit mit stellen [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Objekte, die wesentlich komplexer als die vorherige Version asynchrone Methoden. Die ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) -Schlüsselwort ist syntaktische Kurzform zum angeben, die ein Stück Code asynchron auf den anderen Teil des Codes warten soll. Die [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwort stellt einen Hinweis, mit denen Sie kennzeichnen Methoden als aufgabenbasierte asynchrone Methoden dar. Die Kombination von **"await"**, **Async**, und die **Aufgabe** -Objekts macht es viel einfacher Schreiben von asynchronem Code in .NET 4.5. Das neue Modell für asynchrone Methoden wird aufgerufen, die *aufgabenbasierte asynchrone Muster* (**Tippen Sie auf**). In diesem Tutorial wird vorausgesetzt, dass einige Kenntnisse im Umgang mit asynchronen Programmieren ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) und [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwörter und [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Namespace.

Weitere Informationen zur Verwendung von der ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) und [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwörter und [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) -Namespace finden Sie die folgenden Themen.

- [Whitepaper: Asynchronie in .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Häufig gestellte Fragen zu Async/Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Asynchrone Programmierung in Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  Verarbeitung von Anforderungen vom Threadpool

Auf dem Webserver verwaltet das .NET Framework einen Pool von Threads, die auf die Verarbeitung von ASP.NET-ANFORDERUNGEN verwendet werden. Wenn eine Anforderung eingeht, wird ein Thread aus dem Pool verteilt, um diese Anforderung zu verarbeiten. Wenn die Anforderung synchron verarbeitet wird, ist der Thread, der die Anforderung verarbeitet, beschäftigt, während die Anforderung verarbeitet wird und dass der Thread eine andere Anforderung nicht verarbeiten kann.   
  
Dies kann ein Problem darstellen, nicht sein, da der Threadpool groß genug für viele ausgelastete Threads im vorgenommen werden kann. Die Anzahl der Threads im Threadpool ist jedoch beschränkt (Standardmäßig ist der Maximalwert für .NET 4.5 ist 5000). In großen Anwendungen mit hoher Parallelität von lang andauernder Anforderungen möglicherweise alle verfügbaren Threads ausgelastet. Diese Bedingung wird als Threadmangel (Starvation) bezeichnet. Wenn diese Bedingung erreicht wird, stellt der Webserver Anforderungen Warteschlange. Wenn die Anforderungswarteschlange voll wird, lehnt der Webserver Anforderungen mit einem HTTP 503-Status (Server ausgelastet). CLR-Threadpool weist Einschränkungen auf den neuen Thread-Einschleusung. Wenn Parallelität bursty ist (d. h. Ihrer Website plötzlich erhalten eine große Anzahl von Anforderungen) und alle verfügbaren Anforderungsthreads ausgelastet sind aufgrund von Back-End-Aufrufen mit hoher Latenz, die eingeschränkte Thread-Injection-Rate kann Ihre Anwendung sehr schlecht reagiert. Darüber hinaus verfügt jeder neuer Thread hinzugefügt, die an den Threadpool Mehraufwand (z. B. 1 MB Stapelspeicher zu benötigen). Eine Webanwendung mithilfe von synchronen Methoden von Dienstaufrufen für hohe Latenz, in denen überschreitet Threadpool der Warteschleife hinzu, der Standardwert .NET 4.5 Max. 5, 000 Threads würde etwa 5 GB mehr Arbeitsspeicher beansprucht als eine Anwendung kann der Dienst die gleichen Anforderungen mithilfe asynchrone Methoden und nur 50 Threads. Wenn Sie asynchronen Arbeit ausführen, verwenden Sie nicht immer einen Thread. Z. B. Wenn Sie eine asynchrone Webanfrage für den Dienst vornehmen, ASP.NET nicht verwenden alle Threads zwischen den **Async** Methodenaufruf und **"await"**. Verwenden den Threadpool Anforderungen mit hoher Latenz kann auf eine große Menge Arbeitsspeicher belegen und unzureichender Nutzung von Hardware des Servers führen.

## <a name="processing-asynchronous-requests"></a>Verarbeiten von asynchronen Anforderungen

In Webanwendungen, die eine große Anzahl von gleichzeitigen Anforderungen, die beim Start angezeigt oder verfügt über bursty (wobei plötzlich nimmt die Parallelität) wird die Webdienstaufrufe asynchron die Reaktionsfähigkeit Ihrer Anwendung erhöhen. Eine asynchrone Anforderung hat die gleiche Menge an Zeit, als eine synchrone Anforderung zu verarbeiten. Wenn eine Anforderung über einen Webdienst aufrufen, die stellt erfordert beispielsweise zwei Sekunden auf den Abschluss der Anforderung akzeptiert zwei Sekunden, ob sie synchron oder asynchron ausgeführt wird. Jedoch wird während eines asynchronen Aufrufs wird ein Thread nicht blockiert aus auf andere Anforderungen reagiert, während er darauf wartet, dass die erste Anforderung abgeschlossen. Aus diesem Grund verhindern asynchrone Anforderungen das Wachstum der Anforderung queuing und Thread-Pool, wenn es gibt viele gleichzeitige Anforderungen, die lang andauernde Vorgänge aufrufen.

## <a id="ChoosingSyncVasync"></a>  Auswählen von synchronen oder asynchronen Methoden

Dieser Abschnitt enthält Richtlinien für die Verwendung von synchroner oder asynchroner Methoden. Diese sind nur Richtlinien. Überprüfen Sie jede Anwendung einzeln, um zu bestimmen, ob asynchrone Methoden können Sie mit der Leistung.

Im Allgemeinen verwenden Sie synchrone Methoden, die folgenden Bedingungen:

- Die Vorgänge sind einfache oder kurzer.
- Einfachheit ist wichtiger als Effizienz.
- Die Vorgänge sind in erster Linie CPU-Vorgänge, anstatt die Vorgänge, die umfangreiche Datenträger- oder Netzwerkauslastung betreffen. Verwenden Sie asynchrone Methoden für CPU-gebundene Vorgänge bietet keine Vorteile und führt zu einem Mehraufwand.

  Im Allgemeinen verwenden Sie asynchrone Methoden für die folgenden Bedingungen:

- .NET 4.5 oder höher verwenden, und rufen Sie Dienste, die über asynchrone Methoden genutzt werden können.
- Die Vorgänge sind netzwerkgebunden oder e/A-anstelle von CPU-gebunden.
- Parallelverarbeitung ist wichtiger als die Einfachheit des Codes.
- Möchten Sie einen Mechanismus bereitstellen, mit dem Benutzer eine lang ausgeführte Anforderung abbrechen können.
- Bei der Vorteil, dass das Wechseln von Threads, die Kosten für den Kontextwechsel gewichtet. Im Allgemeinen sollten Sie eine Methode asynchron, wenn es sich bei die synchrone Methode den ASP.NET-Anforderungsthread blockiert, während keine Funktionen ausführt. Indem der Aufruf asynchron, wird der Anforderungsthread ASP.NET nicht blockiert keine Funktionen ausführt, während er darauf wartet, dass die webdienstanforderung abgeschlossen.
- Testen der zeigt, dass die blockierenden Vorgänge einen Engpass bei der websiteleistung sind und IIS mehr Anforderungen verarbeitet werden können, durch die Verwendung von asynchronen Methoden für diese blockierenden Aufrufe.

  Im herunterladbare Beispiel zeigt, wie Sie asynchrone Methoden effektiv verwenden. Das bereitgestellte Beispiel wurde konzipiert, eine einfache Veranschaulichung der asynchronen Programmierung in ASP.NET 4.5. Das Beispiel ist nicht vorgesehen, eine Referenzarchitektur für die asynchrone Programmierung in ASP.NET zu sein. Das Beispielprogramm ruft [ASP.NET Web-API](../../../web-api/index.md) Methoden, die wiederum Aufrufen [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) , lang andauernde Webdienstaufrufe zu simulieren. Die meisten Produktionsanwendungen werden offensichtliche Vorteile verwenden asynchroner Methoden nicht angezeigt werden.   
  
Einige Anwendungen erfordern alle Methoden asynchron sein müssen. Einige synchrone Methoden in asynchronen Methoden konvertieren stellt häufig die beste effizienzsteigerung für den erforderlichen Arbeitsaufwand bereit.

## <a id="SampleApp"></a>  Die Beispielanwendung

Sie können die beispielanwendung aus [ https://github.com/RickAndMSFT/Async-ASP.NET ](https://github.com/RickAndMSFT/Async-ASP.NET) auf die [GitHub](https://github.com/) Standort. Das Repository besteht aus drei Projekten:

- *WebAppAsync*: die ASP.NET Web Forms-Projekt, das die Web-API nutzt **WebAPIpwg** Service. Hauptteil des Codes für dieses Tutorial ist der von diesem Projekt.
- *WebAPIpgw*: die Web-API ASP.NET MVC 4-Projekt, das implementiert die `Products, Gizmos and Widgets` Controller. Es bietet es sich um die Daten für die *WebAppAsync* Projekt und die *Mvc4Async* Projekt.
- *Mvc4Async*: die ASP.NET MVC 4-Projekt mit dem Code in ein anderes Tutorial verwendet. Führt Sie Web-API-Aufrufe für die **WebAPIpwg** Service.

## <a id="GizmosSynch"></a>  Die synchrone Gizmos-Seite

 Der folgende code zeigt die `Page_Load` synchrone Methode, die verwendet wird, um eine Liste der Gizmos anzuzeigen. (In diesem Artikel ist ein VoIP (Gizmo) ein fiktives mechanischen Gerät.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

Der folgende code zeigt die `GetGizmos` -Methode des Diensts VoIP (Gizmo).

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

Die `GizmoService GetGizmos` Methode übergibt einen URI zu einem ASP.NET Web-API-HTTP-Dienst die eine Liste der Gizmos Daten zurückgibt. Die *WebAPIpgw* Projekt enthält die Implementierung der Web-API `gizmos, widget` und `product` Controller.  
Die folgende Abbildung zeigt die Seite "Gizmos" aus dem Beispielprojekt.

![Gizmos](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  Erstellen einer asynchronen Gizmos-Seite

Das Beispiel verwendet die neue [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) und ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) Schlüsselwörter (verfügbar in .NET 4.5 und Visual Studio 2012) damit vom Compiler zuständig für die Verwaltung der für der komplexen Transformations werden asynchrone Programmierung. Der Compiler ermöglicht Ihnen das Schreiben von Code zu verwenden, die die # synchrone ablaufsteuerung erstellt, und der Compiler wendet automatisch die Transformationen, die erforderlichen Rückrufe verwendet, um zu vermeiden, sodass Threads blockiert.

Asynchrone ASP.NET-Seiten enthalten müssen die [Seite](https://msdn.microsoft.com/library/ydy4x04a.aspx) -Direktive zusammen mit den `Async` -Attribut auf "True" festgelegt. Der folgende code zeigt die [Seite](https://msdn.microsoft.com/library/ydy4x04a.aspx) -Direktive zusammen mit den `Async` -Attribut festgelegt ist, auf "True" für die *GizmosAsync.aspx* Seite.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

Der folgende code zeigt die `Gizmos` synchrone `Page_Load` Methode und die `GizmosAsync` asynchrone Seite. Wenn Ihr Browser unterstützt das [HTML 5 &lt;markieren&gt; Element](http://www.w3.org/wiki/HTML/Elements/mark), sehen Sie die Änderungen im `GizmosAsync` in gelbe Markierung.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

Die asynchrone Version:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Die folgenden Änderungen wurden angewendet, um zu ermöglichen die `GizmosAsync` Seite asynchron sein.

- Die [Seite](https://msdn.microsoft.com/library/ydy4x04a.aspx) Richtlinie benötigen die `Async` -Attribut auf "True" festgelegt.
- Die `RegisterAsyncTask` Methode dient zum Registrieren einer asynchronen Aufgabe, die den Code, der asynchron ausgeführt wird.
- Die neue `GetGizmosSvcAsync` Methode ist mit markiert die [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) -Schlüsselwort, das weist den Compiler an, um Rückrufe für Teile des Texts zu generieren und zum automatischen Erstellen von eine `Task` , der zurückgegeben wird.
- &quot;Asynchrone&quot; wurde hinzugefügt, dass der Name der asynchronen Methode. Anfügen von "Async" ist nicht erforderlich, jedoch ist die Konvention, wenn Sie asynchrone Methoden zu schreiben.
- Der Rückgabetyp des neuen `GetGizmosSvcAsync` Methode `Task`. Der Rückgabetyp der `Task` stellt derzeit ausgeführte Arbeit dar, und stellt die Aufrufer der Methode mit einem Handle über den Abschluss des asynchronen Vorgangs warten bereit.
- Die ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) -Schlüsselwort auf den Aufruf des Webdiensts angewendet wurde.
- Die asynchrone Web-API aufgerufen wurde (`GetGizmosAsync`).

In der die `GetGizmosSvcAsync` Methode Text eine weitere asynchrone Methode `GetGizmosAsync` aufgerufen wird. `GetGizmosAsync` wird sofort zurückgegeben, eine `Task<List<Gizmo>>` , die schließlich abgeschlossen, wenn die Daten verfügbar sind. Da Sie nicht möchten, dass für nichts weiter tun, bevor Sie die VoIP (Gizmo) Daten haben wird, wartet der Code auf die Aufgabe (mithilfe der **"await"** Schlüsselwort). Können Sie die **"await"** Schlüsselwort nur in Methoden, die mit Anmerkungen versehen, mit der **Async** Schlüsselwort.

Die **"await"** Schlüsselwort wird den Thread nicht blockiert, bis die Aufgabe abgeschlossen ist. Es registriert den Rest der Methode als Rückruf für den Task, und gibt sofort zurück. Wenn schließlich die erwartete Aufgabe abgeschlossen ist, wird dieser Rückruf aufgerufen und somit fortsetzen die Ausführung der Methode nach rechts, wo er unterbrochen. Weitere Informationen zur Verwendung von der ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) und [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwörter und die [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) -Namespace finden Sie unter der [Async-Verweise](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Der folgende code zeigt die `GetGizmos` und `GetGizmosAsync` Methoden.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Die asynchrone Änderungen ähneln den versucht, den **GizmosAsync** oben. 

- Die Signatur der Methode wurde mit Anmerkung versehen, mit der [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) -Schlüsselwort, der Rückgabetyp wurde geändert in `Task<List<Gizmo>>`, und *Async* dem Methodennamen angefügt wurde.
- Die asynchrone ["HttpClient"](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) Klasse wird verwendet, anstatt die synchrone ["Webclient"](https://msdn.microsoft.com/library/system.net.webclient.aspx) Klasse.
- Die ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) Schlüsselwort wurde angewendet, um die ["HttpClient"](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)["getasync"](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) asynchrone Methode.

Die folgende Abbildung zeigt die Ansicht für die asynchrone VoIP (Gizmo).

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

Die Browser-Präsentation der Daten Gizmos ist identisch mit der Ansicht, die durch den synchronen Aufruf erstellt. Der einzige besteht Unterschied darin, dass die asynchrone Version bieten eine bessere Leistung bei hoher Auslastung möglicherweise.

## <a name="registerasynctask-notes"></a>Anmerkungen zu dieser Version RegisterAsyncTask

Methoden mit eingebunden `RegisterAsyncTask` wird unmittelbar nach Ausführen [PreRender](https://msdn.microsoft.com/library/ms178472.aspx). Sie können asynchrone "void" Seitenereignisse auch direkt verwenden, wie im folgenden Code gezeigt:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Async-void-Ereignisse der Nachteil ist, dass Entwickler nicht mehr vollständig steuern, wann Ereignisse ausgeführt. Z. B. Wenn sowohl eine ASPX- und ein. Definieren Sie Master `Page_Load` Ereignisse und eine oder beide Angaben sind asynchrone, nicht die Reihenfolge der Ausführung sichergestellt werden. Die gleichen Indeterminiate-Reihenfolge für nicht-Ereignishandler (wie z. B. `async void Button_Click` ) angewendet wird. Für die meisten Entwickler sollten in akzeptabel sein, aber die APIs wie diejenigen, die vollständige Kontrolle über die Reihenfolge der Ausführung benötigen sollten nur verwenden, `RegisterAsyncTask` als beansprucht werden Methoden, die ein Task-Objekt zurück.

## <a id="Parallel"></a>  Mehrere Vorgänge ausführen parallel

Asynchrone Methoden haben einen erheblichen Vorteil gegenüber synchronen Methoden, wenn eine Aktion mehrere unabhängige Vorgänge ausführen muss. In diesem Beispiel bereitgestellt, die synchrone Seite *PWG.aspx*(für Produkte, Widgets und Gizmos) zeigt die Ergebnisse der drei Aufrufe des Webdiensts eine Liste von Produkten, Widgets und Gizmos abrufen. Die [ASP.NET Web-API](../../../web-api/index.md) services-Projekt, das diese ermöglicht verwendet [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) aufruft, um Latenz oder ein langsames Netzwerk zu simulieren. Wenn die Verzögerung festgelegt ist, auf 500 Millisekunden, die den asynchronen *PWGasync.aspx* dauert ein wenig mehr als 500 Millisekunden abgeschlossen und die synchrone Seite `PWG` Version übernimmt 1.500 Millisekunden. Die synchrone *PWG.aspx* Seite ist im folgenden Code gezeigt.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Die asynchrone `PWGasync` Code-behind wird unten angezeigt.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

Die folgende Abbildung zeigt die Ansicht zurückgegeben, von dem asynchronen *PWGasync.aspx* Seite.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>  Verwenden eines Abbruchtokens

Asynchrone Methoden zurückgeben `Task`abgebrochen werden kann, sind, die sie akzeptieren ein [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) Parameter an, wenn eine zur Verfügung steht die `AsyncTimeout` Attribut der [Seite](https://msdn.microsoft.com/library/ydy4x04a.aspx) Richtlinie. Der folgende code zeigt die *GizmosCancelAsync.aspx* Seite mit einem Timeout von unter einer Sekunde.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

Der folgende code zeigt die *GizmosCancelAsync.aspx.cs* Datei.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

Wählen Sie in der beispielanwendung bereitgestellt, die *GizmosCancelAsync* verknüpfen Aufrufe der *GizmosCancelAsync.aspx* Seite und zeigt, der Abbruch (durch ein Timeout) des asynchronen Aufrufs. Da die Verzögerungszeit innerhalb eines zufälligen Bereichs ist, müssen Sie die Seite mehrmals aktualisieren, um die Timeout-Fehlermeldung zu erhalten.

## <a id="ServerConfig"></a>  Server-Konfiguration für hohe Parallelität/hohe Wartezeit für Aufrufe des Webdiensts

Um die Vorteile einer asynchronen Webanwendung nutzen zu können, müssen Sie einige Änderungen an der Standardkonfiguration Server vornehmen. Beachten Sie beim Konfigurieren und Belastungstests in Ihre Webanwendung für die asynchrone Bedenken Sie Folgendes.

- Windows 7, Windows Vista, Windows 8 und alle Windows-Clientbetriebssysteme höchstens 10 gleichzeitige Anforderungen. Sie benötigen ein Windows Server-Betriebssystem zum die Vorteile der asynchronen Methoden unter hoher Last finden Sie unter.
- Registrieren Sie .NET 4.5 mit IIS eine Eingabeaufforderung mit erhöhten Rechten mit dem mit dem folgenden Befehl ein:  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_Regiis -i  
  Finden Sie unter [ASP.NET IIS Registration-Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Sie müssen möglicherweise erhöhen die [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) Begrenzung für Anforderungswarteschlange vom Standardwert 1.000 auf 5.000. Wenn die Einstellung zu niedrig ist, wird möglicherweise angezeigt [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) ablehnen von Anforderungen mit einem HTTP 503-Status. So ändern Sie die HTTP.sys-Grenze der Warteschlange:

    - Öffnen Sie IIS-Manager, und navigieren Sie in den Bereich des Anwendungspools.
    - Klicken Sie mit der rechten Maustaste auf den Zielanwendungspool, und wählen Sie **Erweiterte Einstellungen**.  
        ![Erweiterte](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - In der **Erweiterte Einstellungen** im Dialogfeld *Warteschlangenlänge* von 1.000 auf 5.000.  
        ![Warteschlangenlänge](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  Beachten Sie in den oben genannten Images als v4. 0, .NET Framework aufgeführt ist, auch wenn der Anwendungspool .NET 4.5 verwendet. Um diese Abweichung zu verstehen, sehen Sie Folgendes ein:

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Wenn Ihre Anwendung von Webdiensten mithilfe wird oder System.NET Kommunikation mit einem Back-End-über-HTTP-Sie möglicherweise erhöhen müssen die [ConnectionManagement/Maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) Element. Bei ASP.NET-Anwendungen wird dies durch das Feature für die automatische Konfiguration 12 Mal die Anzahl der CPUs beschränkt. Dies bedeutet, dass für eine Quad-Prozedur, höchstens 12 stehen \* 4 = 48 gleichzeitige Verbindungen zu einem IP-Endpunkt. Da dies mit verbunden ist [AutoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), die einfachste Möglichkeit zum erhöhen `maxconnection` in einer ASP.NET-Anwendung-Anwendung besteht darin, festzulegen [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) programmgesteuert in die von `Application_Start` -Methode in der die *"Global.asax"* Datei. Siehe Beispiel für ein Beispiel herunterladen.
- In .NET 4.5, den Standardwert von 5000 für ["maxConcurrentRequestsPerCPU"](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) sollten ausreichend sein.

## <a name="contributors"></a>Contributors

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
