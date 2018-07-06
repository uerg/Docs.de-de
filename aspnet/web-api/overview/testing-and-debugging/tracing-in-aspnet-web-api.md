---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Ablaufverfolgung in ASP.NET-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Zeigt, wie die Ablaufverfolgung in ASP.NET Web-API aktiviert.
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0fabb9bcd0293ba88a41ad9d070958dbbb0c4749
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838108"
---
<a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="355b4-103">Ablaufverfolgung in ASP.NET Web-API 2</span><span class="sxs-lookup"><span data-stu-id="355b4-103">Tracing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="355b4-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="355b4-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="355b4-105">Beim Debuggen eine webbasierten Anwendung, ist es keinen Ersatz für einen guten Satz von Ablaufverfolgungsprotokollen an.</span><span class="sxs-lookup"><span data-stu-id="355b4-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="355b4-106">Dieses Tutorial zeigt, wie die Ablaufverfolgung in ASP.NET Web-API aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="355b4-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="355b4-107">Sie können diese Funktion verwenden, für die Ablaufverfolgung, was das Web-API-Framework ermöglicht, vor und nach dem sie Ihre Controller aufruft.</span><span class="sxs-lookup"><span data-stu-id="355b4-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="355b4-108">Sie können es auch verwenden, um Ihren eigenen Code zu verfolgen.</span><span class="sxs-lookup"><span data-stu-id="355b4-108">You can also use it to trace your own code.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="355b4-109">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="355b4-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="355b4-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (funktioniert auch mit Visual Studio 2015)</span><span class="sxs-lookup"><span data-stu-id="355b4-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="355b4-111">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="355b4-111">Web API 2</span></span>
> - [<span data-ttu-id="355b4-112">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="355b4-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="355b4-113">Aktivieren Sie System.Diagnostics, die in der Web-API-Ablaufverfolgung</span><span class="sxs-lookup"><span data-stu-id="355b4-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="355b4-114">Zunächst erstellen wir ein neues ASP.NET-Webanwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="355b4-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="355b4-115">In Visual Studio aus der **Datei** , wählen Sie im Menü **neu**, klicken Sie dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="355b4-115">In Visual Studio, from the **File** menu, select **New**, then **Project**.</span></span> <span data-ttu-id="355b4-116">Klicken Sie unter **Vorlagen**, **Web**Option **ASP.NET-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="355b4-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="355b4-117">Wählen Sie die Web-API-Projektvorlage aus.</span><span class="sxs-lookup"><span data-stu-id="355b4-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="355b4-118">Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**, klicken Sie dann **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="355b4-118">From the **Tools** menu, select **Library Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="355b4-119">Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle ein.</span><span class="sxs-lookup"><span data-stu-id="355b4-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="355b4-120">Mit dem erste Befehl wird das neueste Paket der Web-API-Ablaufverfolgung installiert.</span><span class="sxs-lookup"><span data-stu-id="355b4-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="355b4-121">Außerdem aktualisiert es die Core-Web-API-Pakete.</span><span class="sxs-lookup"><span data-stu-id="355b4-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="355b4-122">Der zweite Befehl aktualisiert das WebApi.WebHost-Paket auf die neueste Version.</span><span class="sxs-lookup"><span data-stu-id="355b4-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="355b4-123">Wenn Sie eine bestimmte Version von Web-API abzielen möchten, verwenden Sie das - Version-Flag, bei der Installation des Pakets für die Ablaufverfolgung.</span><span class="sxs-lookup"><span data-stu-id="355b4-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>


<span data-ttu-id="355b4-124">Öffnen Sie in der App die Datei WebApiConfig.cs\_Startordner.</span><span class="sxs-lookup"><span data-stu-id="355b4-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="355b4-125">Fügen Sie den folgenden Code der **registrieren** Methode.</span><span class="sxs-lookup"><span data-stu-id="355b4-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="355b4-126">Dieser Code fügt die [systemdiagnosticstracewriter-Objekt](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) Klasse, um die Web-API-Pipeline.</span><span class="sxs-lookup"><span data-stu-id="355b4-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="355b4-127">Die **systemdiagnosticstracewriter-Objekt** Klasse schreibt ablaufverfolgungen ["System.Diagnostics.Trace"](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="355b4-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="355b4-128">Um die ablaufverfolgungen anzuzeigen, führen Sie die Anwendung im Debugger aus.</span><span class="sxs-lookup"><span data-stu-id="355b4-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="355b4-129">Navigieren Sie im Browser zu `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="355b4-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="355b4-130">Die Ablaufverfolgungs-Anweisungen werden im Ausgabefenster in Visual Studio geschrieben.</span><span class="sxs-lookup"><span data-stu-id="355b4-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="355b4-131">(Aus der **Ansicht** , wählen Sie im Menü **Ausgabe**).</span><span class="sxs-lookup"><span data-stu-id="355b4-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="355b4-132">Da **systemdiagnosticstracewriter-Objekt** schreibt ablaufverfolgungen **"System.Diagnostics.Trace"**, können Sie zusätzliche Ablaufverfolgungslistener registrieren; z. B. zum Schreiben von ablaufverfolgungen in einer Protokolldatei.</span><span class="sxs-lookup"><span data-stu-id="355b4-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="355b4-133">Weitere Informationen zu Ablaufverfolgung Writern finden Sie unter den [Ablaufverfolgungslistener](https://msdn.microsoft.com/library/4y5y10s7.aspx) Thema auf MSDN.</span><span class="sxs-lookup"><span data-stu-id="355b4-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="355b4-134">Konfigurieren von systemdiagnosticstracewriter-Objekt</span><span class="sxs-lookup"><span data-stu-id="355b4-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="355b4-135">Der folgende Code zeigt, wie den ablaufverfolgungswriter konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="355b4-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="355b4-136">Es gibt zwei Einstellungen, die Sie steuern können:</span><span class="sxs-lookup"><span data-stu-id="355b4-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="355b4-137">IsVerbose: Wenn "false", enthält jede Ablaufverfolgung wenige Informationen.</span><span class="sxs-lookup"><span data-stu-id="355b4-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="355b4-138">Bei "true", enthält die ablaufverfolgungen mehr Informationen.</span><span class="sxs-lookup"><span data-stu-id="355b4-138">If true, traces include more information.</span></span>
- <span data-ttu-id="355b4-139">MinimumLevel: Legt die minimale Ablaufverfolgungsebene fest.</span><span class="sxs-lookup"><span data-stu-id="355b4-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="355b4-140">Ablaufverfolgungsebenen, in der Reihenfolge, werden Debug-, Info, Warnung, Fehler und schwerwiegend.</span><span class="sxs-lookup"><span data-stu-id="355b4-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="355b4-141">Hinzufügen von Ablaufverfolgungen zu Ihrer Web-API-Anwendung</span><span class="sxs-lookup"><span data-stu-id="355b4-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="355b4-142">Hinzufügen eines Ablaufverfolgungs-Writers bietet Ihnen unmittelbaren Zugriff auf die ablaufverfolgungen, die von der Web-API-Pipeline erstellt.</span><span class="sxs-lookup"><span data-stu-id="355b4-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="355b4-143">Sie können auch den ablaufverfolgungswriter verwenden, um Ihren eigenen Code zu verfolgen:</span><span class="sxs-lookup"><span data-stu-id="355b4-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="355b4-144">Rufen Sie zum Abrufen des ablaufverfolgungswriter **HttpConfiguration.Services.GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="355b4-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="355b4-145">Diese Methode ist von einem Controller, über die **ApiController.Configuration** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="355b4-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="355b4-146">Um eine Ablaufverfolgung zu schreiben, können Sie rufen die **ITraceWriter.Trace** -Methode direkt, aber die [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) -Klasse definiert einige Erweiterungsmethoden, die einfacher sind.</span><span class="sxs-lookup"><span data-stu-id="355b4-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="355b4-147">Z. B. die **Informationen** oben gezeigten Methode erstellt eine Ablaufverfolgung mit Ablaufverfolgungsebene **Informationen**.</span><span class="sxs-lookup"><span data-stu-id="355b4-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="355b4-148">Web-API-Infrastruktur für Ereignisablaufverfolgung</span><span class="sxs-lookup"><span data-stu-id="355b4-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="355b4-149">Dieser Abschnitt beschreibt, wie Sie einen benutzerdefinierten Ablaufverfolgungs-Writer für Web-API schreiben.</span><span class="sxs-lookup"><span data-stu-id="355b4-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="355b4-150">Das Paket Microsoft.AspNet.WebApi.Tracing basiert auf einer allgemeineren Ablaufverfolgungsinfrastruktur in Web-API.</span><span class="sxs-lookup"><span data-stu-id="355b4-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="355b4-151">Anstatt Microsoft.AspNet.WebApi.Tracing, Sie können auch einbinden in eine andere Ablaufverfolgung/Protokollierung-Bibliothek, z. B. [NLog](http://nlog-project.org/) oder [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="355b4-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/loggin library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="355b4-152">Um ablaufverfolgungen zu erfassen, implementieren die **ITraceWriter** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="355b4-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="355b4-153">Hier ist ein einfaches Beispiel:</span><span class="sxs-lookup"><span data-stu-id="355b4-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="355b4-154">Die **ITraceWriter.Trace** Methode erstellt eine Ablaufverfolgung.</span><span class="sxs-lookup"><span data-stu-id="355b4-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="355b4-155">Der Aufrufer gibt es sich um eine Kategorie und Trace-Ebene.</span><span class="sxs-lookup"><span data-stu-id="355b4-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="355b4-156">Die Kategorie kann eine beliebige benutzerdefinierte Zeichenfolge sein.</span><span class="sxs-lookup"><span data-stu-id="355b4-156">The category can be any user-defined string.</span></span> <span data-ttu-id="355b4-157">Die Implementierung von **Ablaufverfolgung** sollten gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="355b4-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="355b4-158">Erstellen Sie ein neues **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="355b4-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="355b4-159">Initialisieren Sie es mit der Anforderung, Kategorie und -Ablaufverfolgungsebene, wie gezeigt.</span><span class="sxs-lookup"><span data-stu-id="355b4-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="355b4-160">Diese Werte werden vom Aufrufer bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="355b4-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="355b4-161">Rufen Sie die *TraceAction* delegieren.</span><span class="sxs-lookup"><span data-stu-id="355b4-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="355b4-162">Innerhalb dieses Delegaten ein: der Aufrufer soll, geben Sie im weiteren Verlauf der **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="355b4-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="355b4-163">Schreiben der **TraceRecord**, über jede Protokollierung-Technik, die Ihnen gefällt.</span><span class="sxs-lookup"><span data-stu-id="355b4-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="355b4-164">Die hier gezeigte Beispiel ruft einfach in **"System.Diagnostics.Trace"**.</span><span class="sxs-lookup"><span data-stu-id="355b4-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="355b4-165">Festlegen den Ablaufverfolgungswriter</span><span class="sxs-lookup"><span data-stu-id="355b4-165">Setting the Trace Writer</span></span>

<span data-ttu-id="355b4-166">Zum Aktivieren der Ablaufverfolgung müssen Sie die Web-API verwenden, konfigurieren Ihre **ITraceWriter** Implementierung.</span><span class="sxs-lookup"><span data-stu-id="355b4-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="355b4-167">Sie ist dies über die **HttpConfiguration** Objekt, wie im folgenden Code gezeigt:</span><span class="sxs-lookup"><span data-stu-id="355b4-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="355b4-168">Nur eine Ablaufverfolgungs-Writer kann aktiv sein.</span><span class="sxs-lookup"><span data-stu-id="355b4-168">Only one trace writer can be active.</span></span> <span data-ttu-id="355b4-169">Web-API legt standardmäßig eine &quot;ohne-Op-&quot; Überwachungstoken, die keine Aktionen ausführt.</span><span class="sxs-lookup"><span data-stu-id="355b4-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="355b4-170">(Die &quot;ohne-Op-&quot; Überwachung vorhanden, sodass Rückverfolgen von Code nicht überprüfen unbedingt, ob der ablaufverfolgungswriter **null** vor dem Schreiben einer Ablaufverfolgungs.)</span><span class="sxs-lookup"><span data-stu-id="355b4-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="355b4-171">Wie Web-API-Ablaufverfolgung funktioniert</span><span class="sxs-lookup"><span data-stu-id="355b4-171">How Web API Tracing Works</span></span>

<span data-ttu-id="355b4-172">Ablaufverfolgung in Web-API verwendet, die in Web-API verwendet einen *Fassade* Muster: Wenn die Ablaufverfolgung aktiviert ist, Web-API dient als Wrapper für verschiedene Teile der Pipeline mit Klassen, die Trace-Aufrufe ausführen.</span><span class="sxs-lookup"><span data-stu-id="355b4-172">Tracing in Web API uses a in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="355b4-173">Z. B. Wenn Sie einen Controller auswählen, die die Pipeline verwendet die **IHttpControllerSelector** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="355b4-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="355b4-174">Mit aktivierter Ablaufverfolgung, fügt die Pipleline eine Klasse, die implementiert **IHttpControllerSelector** aber Eigenschaftenaufrufe an die tatsächliche Implementierung:</span><span class="sxs-lookup"><span data-stu-id="355b4-174">With tracing enabled, the pipleline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Web-API-Ablaufverfolgung verwendet die Fassade-Muster.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="355b4-176">Die von dieser Entwurf bietet folgende Vorteile:</span><span class="sxs-lookup"><span data-stu-id="355b4-176">The benefits of this design include:</span></span>

- <span data-ttu-id="355b4-177">Wenn Sie einen Ablaufverfolgungs-Writer nicht hinzufügen, werden die ablaufverfolgungskomponenten nicht instanziiert und haben keine Auswirkungen auf die Leistung.</span><span class="sxs-lookup"><span data-stu-id="355b4-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="355b4-178">Wenn Sie z. B. Standarddienste ersetzen **IHttpControllerSelector** durch Ihre eigene benutzerdefinierte Implementierung, Ablaufverfolgung ist nicht betroffen, da die Ablaufverfolgung durch das Wrapperobjekt erfolgt.</span><span class="sxs-lookup"><span data-stu-id="355b4-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="355b4-179">Sie können auch das gesamte Web-API-Ablaufverfolgung-Framework mit Ihren eigenen benutzerdefinierten Framework verwenden, ersetzen, von den standardmäßigen ersetzen **ITraceManager** Dienst:</span><span class="sxs-lookup"><span data-stu-id="355b4-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="355b4-180">Implementieren **ITraceManager.Initialize** zum Initialisieren von Ihrem Ablaufverfolgungssystem.</span><span class="sxs-lookup"><span data-stu-id="355b4-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="355b4-181">Beachten Sie, dass dies ersetzt die *gesamte* Ablaufverfolgung Frameworks einschließlich sämtlicher den Ablaufverfolgungscode, die in Web-API integriert ist.</span><span class="sxs-lookup"><span data-stu-id="355b4-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
