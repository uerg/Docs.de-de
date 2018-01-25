---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: Verwenden von asynchronen Methoden in ASP.NET 4.5 | Microsoft Docs
author: Rick-Anderson
description: "In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer asynchronen ASP.NET Web Forms-Anwendung mithilfe von Visual Studio Express 2012 für das Web, also eine kostenlose..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 73e46134cfafb9edc4c1888211eab44b8f2bf828
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>Verwenden von asynchronen Methoden in ASP.NET 4.5
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm lernen Sie die Grundlagen der Erstellung einer asynchronen ASP.NET Web Forms-Anwendung, mit [Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/11), dies ist eine kostenlose Version von Microsoft Visual Studio. Sie können auch [Visual Studio 2012](https://www.microsoft.com/visualstudio/11). In diesem Lernprogramm sind die folgenden Abschnitte enthalten.
> 
> - [Wie Anforderungen durch den Threadpool verarbeitet werden.](#HowRequestsProcessedByTP)
> - [Auswählen von synchronen oder asynchronen Methoden](#ChoosingSyncVasync)
> - [Die Beispielanwendung](#SampleApp)
> - [Die synchrone Zukunftsvisionen-Seite](#GizmosSynch)
> - [Erstellen einer asynchronen Zukunftsvisionen-Seite](#CreatingAsynchGizmos)
> - [Ausführen mehrerer Vorgänge parallel](#Parallel)
> - [Verwenden eines Abbruchtokens](#CancelToken)
> - [Serverkonfiguration für hoher Parallelität oder hohe Latenz Webdienstaufrufe](#ServerConfig)
> 
> Ein vollständiges Beispiel wird für dieses Lernprogramm zur bereitgestellt.  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) on the [GitHub](https://github.com/) site.


ASP.NET 4.5 Web Pages in Kombination [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) ermöglicht es Ihnen, die asynchronen Methoden zu registrieren, die ein Objekt des Typs zurückgeben [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). .NET Framework 4 eingeführt, eine asynchrone Programmierkonzept als bezeichnet eine [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) und ASP.NET 4.5 unterstützt [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Aufgaben werden durch dargestellt die **Aufgabe** Typ und verwandte Typen in der [System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) Namespace. .NET Framework 4.5 erstellt diese asynchrone Unterstützung mit der ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) und [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwörter, die Arbeiten mit [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) wesentlich komplexer ist als der vorherige Objekte asynchrone Methoden. Die ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) -Schlüsselwort ist syntaktische Kurzform für, der angibt, die ein Stück Code asynchron auf einen anderen Codeabschnitt warten soll. Die [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwort stellt einen Hinweis an, die Sie verwenden können, um Methoden als aufgabenbasierten asynchronen Methoden zu kennzeichnen. Die Kombination von **"await"**, **Async**, und die **Aufgabe** Objekt kann es viel einfacher Schreiben von asynchronem Code in .NET 4.5. Das neue Modell für asynchrone Methoden wird aufgerufen, die *aufgabenbasierte asynchrone Muster* (**Tippen Sie auf**). In diesem Lernprogramm wird vorausgesetzt, dass einige Kenntnisse im Umgang mit asynchronen Programing ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) und [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwörter und die [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Namespace.

Weitere Informationen zu den mit ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) und [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwörter und die [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Namespace finden Sie unter den folgenden Ressourcen.

- [Whitepaper: Asynchronie in .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Async/Await-häufig gestellte Fragen](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Asynchrone Programmierung für Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Wie Anforderungen durch den Threadpool verarbeitet werden.

Auf dem Webserver, .NET Framework verwaltet einen Pool von Threads, die auf die Verarbeitung von ASP.NET-ANFORDERUNGEN verwendet werden. Wenn eine Anforderung eingeht, wird ein Thread aus dem Pool verteilt, um diese Anforderung zu verarbeiten. Wenn die Anforderung synchron verarbeitet wird, wird der Thread, der die Anforderung verarbeitet ausgelastet ist, während die Anforderung verarbeitet wird und dass der Thread einer anderen Anforderung nicht verarbeiten kann.   
  
Dies möglicherweise kein Problem, da der Threadpool groß genug für viele ausgelasteten Threads vorgenommen werden kann. Die Anzahl der Threads im Threadpool ist jedoch beschränkt (5.000 ist das Standardmaximum, die für .NET 4.5). In großen Anwendungen mit hoher Parallelität für lange Anforderungen möglicherweise alle verfügbaren Threads ausgelastet. Diese Bedingung wird als Threadmangel (Starvation) bezeichnet. Wenn diese Bedingung erreicht wird, stellt der Webserver Anforderungen Warteschlange. Wenn die Anforderungswarteschlange voll wird, lehnt der Webserver Anforderungen mit einem HTTP 503-Status (Server ausgelastet). CLR-Threadpools weist Einschränkungen auf den neuen Thread einfügungen. Wenn die Parallelität bursty ist (d. h. Ihrer Website plötzlich erhalte eine große Anzahl von Anforderungen) und alle verfügbaren Anforderung Threads ausgelastet sind aufgrund von Back-End-Aufrufen mit häufiger Latenz, beschränkt Thread-Injection-Rate kann Ihre Anwendung sehr schlecht reagieren vornehmen. Darüber hinaus weist jede neuer Thread hinzugefügt, die an den Threadpool Mehraufwand (z. B. 1 MB Stapelspeicher zu benötigen). Eine Web-Anwendung mithilfe von synchronen Methoden auf hoher Latenz Dienstaufrufe, in denen überschreitet des Threadpools .NET 4.5 Standard maximal 5, 000 Threads würde erfordert etwa 5 GB mehr Arbeitsspeicher als eine Anwendung kann der Dienst die gleiche webanforderungen mit asynchrone Methoden und nur 50 Threads. Wenn Sie asynchrone Aufgaben durchführen, verwenden Sie nicht immer einen Thread. Beispielsweise, wenn Sie eine asynchrone Dienst webanforderung vornehmen, ASP.NET nicht verwenden keine Threads zwischen den **Async** Methodenaufruf und die **"await"**. Den Threadpool Serviceanfragen kann mit häufiger Latenz eine große Menge Arbeitsspeicher belegen und unzureichender Nutzung von Hardware des Servers führen.

## <a name="processing-asynchronous-requests"></a>Verarbeiten von asynchronen Anforderungen

In Webanwendungen, die noch eine bursty Last (wobei plötzlich nimmt die Parallelität) finden Sie eine große Anzahl gleichzeitiger Anforderungen beim Start, erhöht Webdienstaufrufe asynchrone die Reaktionsfähigkeit der Anwendung. Eine asynchrone Anforderung hat die gleiche Menge an Zeit als eine asynchrone Anforderung verarbeitet. Beispielsweise wenn eine Anforderung über einen Webdienst aufrufen sendet erfordert zwei Sekunden auf den Abschluss der Anforderung akzeptiert zwei Sekunden, ob sie synchron oder asynchron ausgeführt werden. Allerdings wird während eines asynchronen Aufrufs ein Thread nicht blockiert für andere Anforderungen reagieren, während er darauf wartet, dass die erste Anforderung abgeschlossen. Deshalb verhindern asynchrone Anforderungen Anforderung queuing und Thread-Pool-Wachstum, wenn es gibt viele gleichzeitige Anforderungen, die lang ausgeführter Vorgänge aufrufen.

## <a id="ChoosingSyncVasync"></a>Auswählen von synchronen oder asynchronen Methoden

Dieser Abschnitt enthält Richtlinien zum synchrone oder asynchrone Methoden zu verwenden. Diese sind nur Richtlinien. Überprüfen Sie jede Anwendung einzeln, um zu bestimmen, ob die asynchronen Methoden mit der Leistung helfen.

Im Allgemeinen verwenden Sie synchrone Methoden, für die folgenden Bedingungen:

- Die Vorgänge sind einfach oder kurze Ausführungsdauer.
- Einfachheit ist wichtiger als die Effizienz.
- Die Vorgänge sind in erster Linie CPU anstelle der Vorgänge, die eine umfangreiche Datenträger- oder Netzwerkauslastung betreffen. Verwenden Sie asynchrone Methoden für CPU-gebundene Vorgänge bietet keine Vorteile und führt zu einem Mehraufwand.

 Im Allgemeinen verwenden Sie asynchrone Methoden für die folgenden Bedingungen:

- Rufen Sie Dienste, die über asynchrone Methoden verwendet werden können, und Sie .NET 4.5 oder höher verwenden.
- Die Vorgänge sind netzwerkgebunden oder e/A-gebundene nicht CPU-gebunden.
- Parallelverarbeitung ist wichtiger als die Einfachheit des Codes.
- Sie möchten einen Mechanismus bereit, mit dem Benutzer eine lang ausgeführte Anforderung abbrechen können.
- Wenn der Vorteil der Wechsel von Threads, die Kosten der Kontextwechsel gewichtet. Im Allgemeinen sollten Sie eine Methode asynchron, wenn die synchrone Methode der ASP.NET-Anforderungsthreads blockiert, während keine Aktionen ausführt. Durch Aufruf der asynchronen, wird die ASP.NET-Anforderungsthreads nicht blockiert keine Aktionen ausführt, während er darauf wartet, dass der Dienst webanforderung abgeschlossen.
- Testen der zeigt, dass die blockierenden Vorgänge einen Engpass bei der websiteleistung sind und dass IIS mehr Anforderungen mithilfe von asynchronen Methoden für diese blockierenden Aufrufe bedienen kann.

 Im herunterladbare Beispiel wird gezeigt, wie asynchrone Methoden effektiv verwendet werden. Das bereitgestellte Beispiel wurde entworfen, um eine einfache Veranschaulichung der asynchronen Programmierung in ASP.NET 4.5 bereitzustellen. Das Beispiel dient nicht als bezugsarchitektur für die asynchrone Programmierung in ASP.NET. Das Beispielprogramm ruft [ASP.NET-Web-API](../../../web-api/index.md) Methoden, die wiederum rufen [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) langer Webdienstaufrufe zu simulieren. Die meisten Produktionsanwendungen werden offensichtliche Vorteile Verwenden von asynchronen Methoden nicht angezeigt werden.   
  
Einige Anwendungen erfordern alle Methoden asynchron ist. Konvertieren einige synchrone Methoden für asynchrone Methoden stellt häufig die beste effizienzsteigerung erforderlichen Arbeitsaufwand.

## <a id="SampleApp"></a>Die Beispielanwendung

Sie können die beispielanwendung aus herunterladen [https://github.com/RickAndMSFT/Async-ASP.NET](https://github.com/RickAndMSFT/Async-ASP.NET) auf die [GitHub](https://github.com/) Standort. Das Repository umfasst drei Projekte:

- *WebAppAsync*: der ASP.NET Web Forms-Projekt, das die Web-API nutzt **WebAPIpwg** Dienst. Hauptteil des Codes für dieses Lernprogramm, aus der dieser ist Projekt.
- *WebAPIpgw*: die Web-API ASP.NET MVC 4-Projekt implementiert die `Products, Gizmos and Widgets` Controller. Es enthält die Daten für die *WebAppAsync* Projekt und die *Mvc4Async* Projekt.
- *Mvc4Async*: der ASP.NET MVC 4-Projekt mit dem Code in ein weiteres Lernprogramm verwendet. Nimmt Web-API-Aufrufe an die **WebAPIpwg** Dienst.

## <a id="GizmosSynch"></a>Die synchrone Zukunftsvisionen-Seite

 Der folgende code zeigt die `Page_Load` synchrone Methode, die verwendet wird, um eine Liste der Zukunftsvisionen anzuzeigen. (Für diesen Artikel ist eine VoIP (Gizmo) eine fiktive mechanische Gerät.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

Der folgende code zeigt die `GetGizmos` -Methode des Diensts VoIP (Gizmo).

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

Die `GizmoService GetGizmos` Methode übergibt einen URI mit einem ASP.NET Web API-HTTP-Dienst die eine Liste der Zukunftsvisionen Daten zurückgibt. Die *WebAPIpgw* Projekt enthält die Implementierung des Web-API des `gizmos, widget` und `product` Controller.  
Die folgende Abbildung zeigt die Seite Zukunftsvisionen des Beispielprojekts.

![Zukunftsvisionen](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Erstellen einer asynchronen Zukunftsvisionen-Seite

Das Beispiel verwendet die neue [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) und ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) Schlüsselwörter (verfügbar in .NET 4.5 und Visual Studio 2012), damit der Compiler zuständig für die Verwaltung der für der komplexen Transformations werden asynchrone Programmierung. Der Compiler ermöglicht das Schreiben von Code mithilfe der # des synchronen ablaufsteuerung erstellt, und der Compiler wendet automatisch die Transformationen für die Rückrufe zu verwenden, um die Blockierung von Threads zu vermeiden.

Asynchrone ASP.NET-Seiten enthalten müssen die [Seite](https://msdn.microsoft.com/library/ydy4x04a.aspx) -Direktive zusammen mit den `Async` -Attribut auf "True" festgelegt ist. Der folgende code zeigt die [Seite](https://msdn.microsoft.com/library/ydy4x04a.aspx) -Direktive zusammen mit den `Async` -Attribut festgelegt auf "True" ist für die *GizmosAsync.aspx* Seite.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

Der folgende code zeigt die `Gizmos` synchrone `Page_Load` Methode und die `GizmosAsync` asynchrone Seite. Wenn Ihr Browser unterstützt die [HTML 5 &lt;markieren&gt; Element](http://www.w3.org/wiki/HTML/Elements/mark), sehen Sie die Änderungen in `GizmosAsync` in gelb hervorheben.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

Die asynchrone Version:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Die folgenden Änderungen wurden angewendet, um ermöglichen die `GizmosAsync` Seite asynchron sein.

- Die [Seite](https://msdn.microsoft.com/library/ydy4x04a.aspx) Richtlinie benötigen die `Async` -Attribut auf "True" festgelegt ist.
- Die `RegisterAsyncTask` Methode wird zum Registrieren einer asynchronen Aufgabe, die den Code, der asynchron ausgeführt wird.
- Die neue `GetGizmosSvcAsync` Methode mit gekennzeichnet ist der [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwort, das der Compiler, um Rückrufe für Teile des Texts zu generieren und zum automatischen Erstellen von gibt eine `Task` zurückgegeben wird.
- &quot;Asynchrone&quot; auf den Namen der asynchronen Methode angefügt wurde. Anfügen von "Async" ist nicht erforderlich, jedoch wird die Konvention beim asynchronen Methoden zu schreiben.
- Der Rückgabetyp des neuen neue `GetGizmosSvcAsync` Methode ist `Task`. Der Rückgabetyp der `Task` stellt derzeit ausgeführte Arbeit dar und stellt Aufrufern der Methode mit einem Handle über den Abschluss des asynchronen Vorgangs gewartet werden soll.
- Die ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) -Schlüsselwort auf den Aufruf des Webdiensts angewendet wurde.
- Die asynchrone Webdienst-API aufgerufen wurde (`GetGizmosAsync`).

In der die `GetGizmosSvcAsync` Methode Textkörper einer anderen asynchronen Methode `GetGizmosAsync` aufgerufen wird. `GetGizmosAsync`wird sofort zurückgegeben, eine `Task<List<Gizmo>>` , die letztendlich abgeschlossen, wenn die Daten verfügbar sind. Da Sie nichts weiter tun, bis Sie die Daten VoIP (Gizmo) verfügen möchten, wartet der Code auf die Aufgabe (mithilfe der **"await"** Schlüsselwort). Können Sie die **"await"** Schlüsselwort nur in Methoden mit Anmerkungen versehen, mit der **Async** Schlüsselwort.

Die **"await"** Schlüsselwort blockiert nicht den Thread, bis die Aufgabe abgeschlossen ist. Es registriert den Rest der Methode als Rückruf für den Task, und sofort zurückgegeben. Wenn schließlich die erwartete Aufgabe abgeschlossen hat, wird dieser Rückruf aufzurufen und somit fortsetzen die Ausführung der Methode nach rechts, in dem sie wurde unterbrochen. Für Weitere Informationen finden Sie unter der ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) und [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) Schlüsselwörter und die [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) Namespace finden Sie unter der [Async Verweise](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Der folgende code zeigt die `GetGizmos` und `GetGizmosAsync` Methoden.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Die asynchrone Änderungen ähneln denen versucht, die **GizmosAsync** oben. 

- Die Signatur der Methode wurde mit Anmerkung versehen, mit der [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) -Schlüsselwort, der Rückgabetyp wurde geändert in `Task<List<Gizmo>>`, und *Async* dem Methodennamen angefügt wurde.
- Der asynchrone ["HttpClient"](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) Klasse verwendet, anstatt Sie zu synchronen [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) Klasse.
- Die ["await"](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) Schlüsselwort wurde angewendet, um die ["HttpClient"](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) asynchrone Methode.

Die folgende Abbildung zeigt das asynchrone VoIP (Gizmo) anzeigen.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

Die Browser-Präsentation der Daten Zukunftsvisionen ist identisch mit der Ansicht, die durch den synchronen Aufruf erstellt. Der einzige Unterschied ist, dass die asynchrone Version bieten eine bessere Leistung bei hoher Auslastung möglicherweise.

## <a name="registerasynctask-notes"></a>RegisterAsyncTask Hinweise

Methoden mit eingebunden `RegisterAsyncTask` wird unmittelbar nach Ausführen [PreRender](https://msdn.microsoft.com/library/ms178472.aspx). Sie können asynchrone "void" Page-Ereignisse auch direkt verwenden, wie im folgenden Code gezeigt:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Der Nachteil "void" Async-Ereignisse ist, dass Entwickler nicht mehr Vollzugriff auf die beim Ausführen der Ereignisse. Beispielsweise wenn sowohl eine ASPX- und ein. Definieren Sie Master `Page_Load` Ereignisse und eine oder beide sind asynchron, die Reihenfolge der Ausführung nicht garantiert werden kann. Die Reihenfolge der Indeterminiate für nicht-Ereignishandler (z. B. `async void Button_Click` ) angewendet wird. Für die meisten Entwickler sollte dies akzeptabel sein, aber die APIs wie diejenigen, die vollständige Kontrolle über die Reihenfolge der Ausführung erfordern sollten nur verwenden, `RegisterAsyncTask` als beansprucht werden Methoden, die ein Task-Objekt zurück.

## <a id="Parallel"></a>Ausführen mehrerer Vorgänge parallel

Asynchrone Methoden haben einen erheblichen Vorteil gegenüber der synchronen Methoden auf, wenn eine Aktion mehrere unabhängige Vorgänge ausführen muss. Im Beispiel bereitgestellt, die synchrone Seite *PWG.aspx*(für Produkte, Widgets und Zukunftsvisionen) zeigt die Ergebnisse der drei Aufrufe des Webdiensts eine Liste der Produkte, Widgets und Zukunftsvisionen abgerufen. Die [ASP.NET Web API](../../../web-api/index.md) services-Projekt, das diese ermöglicht verwendet [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) Aufrufe an die Latenz oder langsames Netzwerk zu simulieren. Wenn die Verzögerung festgelegt ist, auf 500 Millisekunden, die den asynchronen *PWGasync.aspx* dauert es etwas mehr als 500 Millisekunden abgeschlossen beim synchronen Seite `PWG` Version übernimmt 1.500 Millisekunden. Die synchrone *PWG.aspx* Seite wird im folgenden Code gezeigt.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Der asynchrone `PWGasync` CodeBehind wird unten gezeigt.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

Die folgende Abbildung zeigt die Sicht zurückgegeben, die den asynchronen *PWGasync.aspx* Seite.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>Verwenden eines Abbruchtokens

Asynchrone Methoden zurückgeben `Task`abgebrochen werden kann, sind, die sie in Anspruch nehmen eine [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) -Parameter, wenn eine zur Verfügung steht die `AsyncTimeout` Attribut von der [Seite](https://msdn.microsoft.com/library/ydy4x04a.aspx) Richtlinie. Der folgende code zeigt die *GizmosCancelAsync.aspx* mit einem Timeout der zweiten Seite.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

Der folgende code zeigt die *GizmosCancelAsync.aspx.cs* Datei.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

Auswählen in der beispielanwendung zur Verfügung gestellt, die *GizmosCancelAsync* verknüpfen Aufrufe der *GizmosCancelAsync.aspx* Seite und zeigt die Kündigung (durch ein Timeout auftritt), des asynchronen Aufrufs. Da die Verzögerung in einem zufälligen Bereich ist, müssen Sie die Seite mehrmals aktualisieren, um die Timeout-Fehlermeldung abzurufen.

## <a id="ServerConfig"></a>Serverkonfiguration für hoher Parallelität oder hohe Latenz Webdienstaufrufe

Um die Vorteile einer asynchronen Web-Anwendung nutzen zu können, müssen Sie Änderungen an der Standardkonfiguration Server vornehmen. Beachten Sie beim Konfigurieren und Belastungstests in der asynchronen Webanwendung Bedenken Sie Folgendes.

- Windows 7, Windows Vista, Windows 8 und alle Windows-Clientbetriebssysteme höchstens 10 gleichzeitige Anforderungen. Sie benötigen ein Betriebssystem Windows Server, um die Vorteile der asynchronen Methoden verursacht bei hoher Auslastung anzuzeigen.
- Registrieren von .NET 4.5 mit IIS von einer Eingabeaufforderung mit dem folgenden Befehl:  
 %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i  
 Finden Sie unter [ASP.NET IIS Registration-Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Sie müssen u. u. erhöhen die [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) Limit für die Warteschlange von der Standardwert von 1.000 bis 5.000. Wenn die Einstellung zu niedrig ist, werden möglicherweise [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) ablehnen von Anforderungen mit einem HTTP 503-Status. So ändern Sie die Begrenzung der HTTP.sys-Warteschlange:

    - Öffnen Sie den IIS-Manager, und navigieren Sie in den Bereich des Anwendungspools.
    - Klicken Sie mit der rechten Maustaste auf den Ziel-Anwendungspool, und wählen Sie **Erweiterte Einstellungen**.  
        ![advanced](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - In der **Erweiterte Einstellungen** ändern Sie im Dialogfeld *Warteschlangenlänge* von 1.000 5.000.  
        ![Warteschlangenlänge](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
 Beachten Sie in den oben genannten Images als v4. 0, .NET Framework aufgeführt ist, auch wenn für der Anwendungspool .NET 4.5 verwendet wird. Um diese Abweichung zu verstehen, finden Sie in der folgenden:

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Wenn Ihre Anwendung von Webdiensten mithilfe wird oder System.NET Kommunikation mit einem Back-End-über-HTTP-Sie möglicherweise erhöhen müssen die [ConnectionManagement/Maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) Element. Für ASP.NET-Anwendungen wird dies durch die Funktion für die automatische Konfiguration 12-Mal der Anzahl der CPUs beschränkt. Das bedeutet, dass auf einen Quad-Proc höchstens 12 haben können \* 4 = 48 gleichzeitige Verbindungen zu einem IP-Endpunkt. Da dies gebunden ist [AutoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), die einfachste Möglichkeit zum Vergrößern `maxconnection` in einer ASP.NET-Anwendung Anwendung besteht darin, [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) programmgesteuert in die von `Application_Start` Methode in der *"Global.asax"* Datei. Finden Sie im Beispiel für ein Beispiel herunterladen.
- In .NET 4.5, den Standardwert von 5000 ["maxConcurrentRequestsPerCPU"](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) Ordnung.

## <a name="contributors"></a>Contributors

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
