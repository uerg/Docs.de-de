---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: In der ASP.NET Web API 2 Tracing | Microsoft Docs
author: MikeWasson
description: Zeigt, wie in ASP.NET Web-API-Ablaufverfolgung aktivieren.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7392ae5d9bc4c3aab45a9373099a0ee18e873a4f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="f5f67-103">In der ASP.NET Web API 2 Tracing</span><span class="sxs-lookup"><span data-stu-id="f5f67-103">Tracing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="f5f67-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f5f67-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="f5f67-105">Wenn Sie eine webbasierte Anwendung debuggen möchten, wird kein Ersatz für einen geeigneten Satz von Ablaufverfolgungsprotokollen.</span><span class="sxs-lookup"><span data-stu-id="f5f67-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="f5f67-106">Dieses Lernprogramm zeigt, wie in ASP.NET Web-API-Ablaufverfolgung aktivieren.</span><span class="sxs-lookup"><span data-stu-id="f5f67-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="f5f67-107">Sie können diese Funktion verwenden, für die Ablaufverfolgung der Wirkungsweise der Web-API-Framework, bevor und nachdem sie Ihre Controller aufruft.</span><span class="sxs-lookup"><span data-stu-id="f5f67-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="f5f67-108">Sie können es auch verwenden, um Ihren eigenen Code zu verfolgen.</span><span class="sxs-lookup"><span data-stu-id="f5f67-108">You can also use it to trace your own code.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f5f67-109">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="f5f67-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f5f67-110">[Visual Studio-2017](https://www.visualstudio.com/downloads/) (funktioniert auch mit Visual Studio 2015)</span><span class="sxs-lookup"><span data-stu-id="f5f67-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="f5f67-111">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="f5f67-111">Web API 2</span></span>
> - [<span data-ttu-id="f5f67-112">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="f5f67-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="f5f67-113">Aktivieren Sie im Web API Tracing System.Diagnostics</span><span class="sxs-lookup"><span data-stu-id="f5f67-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="f5f67-114">Erstellen Sie zunächst ein neues ASP.NET Webanwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="f5f67-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="f5f67-115">In Visual Studio aus der **Datei** klicken Sie im Menü **neu**, klicken Sie dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="f5f67-115">In Visual Studio, from the **File** menu, select **New**, then **Project**.</span></span> <span data-ttu-id="f5f67-116">Klicken Sie unter **Vorlagen**, **Web**Option **ASP.NET-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="f5f67-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="f5f67-117">Wählen Sie die Web-API-Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="f5f67-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="f5f67-118">Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, klicken Sie dann **Paket Verwaltungskonsole**.</span><span class="sxs-lookup"><span data-stu-id="f5f67-118">From the **Tools** menu, select **Library Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="f5f67-119">Geben Sie die folgenden Befehle aus, klicken Sie im Paket-Manager-Konsole.</span><span class="sxs-lookup"><span data-stu-id="f5f67-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="f5f67-120">Mit dem erste Befehl wird das aktuellste Web API Tracing-Paket installiert.</span><span class="sxs-lookup"><span data-stu-id="f5f67-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="f5f67-121">Außerdem wird die Core-Web-API-Pakete aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="f5f67-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="f5f67-122">Mit dem zweite Befehl werden das WebApi.WebHost-Paket auf die neueste Version aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="f5f67-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="f5f67-123">Wenn eine bestimmte Version von Web-API ausgerichtet werden sollen, verwenden Sie Kennzeichen "- Version" bei der Installation des Pakets für die Ablaufverfolgung.</span><span class="sxs-lookup"><span data-stu-id="f5f67-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>


<span data-ttu-id="f5f67-124">Öffnen Sie die Datei WebApiConfig.cs in der App\_Startordner.</span><span class="sxs-lookup"><span data-stu-id="f5f67-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="f5f67-125">Fügen Sie folgenden Code, der **registrieren** Methode.</span><span class="sxs-lookup"><span data-stu-id="f5f67-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="f5f67-126">Dieser Code fügt der [systemdiagnosticstracewriter-Objekt](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) Klasse für die Web-API-Pipeline.</span><span class="sxs-lookup"><span data-stu-id="f5f67-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="f5f67-127">Die **systemdiagnosticstracewriter-Objekt** Klasse schreibt ablaufverfolgungen [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="f5f67-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="f5f67-128">Um die ablaufverfolgungen anzuzeigen, führen Sie die Anwendung im Debugger.</span><span class="sxs-lookup"><span data-stu-id="f5f67-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="f5f67-129">Wechseln Sie in den Browser zu `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="f5f67-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="f5f67-130">Die ablaufverfolgungsanweisungen werden in das Ausgabefenster in Visual Studio geschrieben.</span><span class="sxs-lookup"><span data-stu-id="f5f67-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="f5f67-131">(Aus der **Ansicht** klicken Sie im Menü **Ausgabe**).</span><span class="sxs-lookup"><span data-stu-id="f5f67-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="f5f67-132">Da **systemdiagnosticstracewriter-Objekt** schreibt ablaufverfolgungen **System.Diagnostics.Trace**, können Sie zusätzliche Ablaufverfolgungslistener registrieren; z. B. zum Schreiben von ablaufverfolgungen in einer Protokolldatei.</span><span class="sxs-lookup"><span data-stu-id="f5f67-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="f5f67-133">Weitere Informationen zur Ablaufverfolgung Writer finden Sie unter der [Ablaufverfolgungslistener](https://msdn.microsoft.com/library/4y5y10s7.aspx) Thema auf MSDN.</span><span class="sxs-lookup"><span data-stu-id="f5f67-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="f5f67-134">Konfigurieren von systemdiagnosticstracewriter-Objekt</span><span class="sxs-lookup"><span data-stu-id="f5f67-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="f5f67-135">Der folgende Code zeigt, wie den ablaufverfolgungswriter konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="f5f67-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="f5f67-136">Es gibt zwei Einstellungen, die Sie steuern können:</span><span class="sxs-lookup"><span data-stu-id="f5f67-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="f5f67-137">IsVerbose: Wenn "false", enthält jede Ablaufverfolgung wenige Informationen.</span><span class="sxs-lookup"><span data-stu-id="f5f67-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="f5f67-138">Bei "true", enthalten die ablaufverfolgungen mehr Informationen.</span><span class="sxs-lookup"><span data-stu-id="f5f67-138">If true, traces include more information.</span></span>
- <span data-ttu-id="f5f67-139">MinimumLevel: Legt die minimale Ablaufverfolgungsebene fest.</span><span class="sxs-lookup"><span data-stu-id="f5f67-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="f5f67-140">Ablaufverfolgungsebenen nacheinander, werden Debug-, Info, Warn, Fehler und schwerwiegend.</span><span class="sxs-lookup"><span data-stu-id="f5f67-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="f5f67-141">Hinzufügen von Ablaufverfolgungen für Ihre Web-API-Anwendung</span><span class="sxs-lookup"><span data-stu-id="f5f67-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="f5f67-142">Hinzufügen einer ablaufverfolgungswriter ermöglicht Ihnen unmittelbaren Zugriff auf die ablaufverfolgungen, die von der Web-API-Pipeline erstellt.</span><span class="sxs-lookup"><span data-stu-id="f5f67-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="f5f67-143">Sie können auch den ablaufverfolgungswriter verwenden, um Ihren eigenen Code zu verfolgen:</span><span class="sxs-lookup"><span data-stu-id="f5f67-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="f5f67-144">Rufen Sie zum Abrufen des ablaufverfolgungswriter **HttpConfiguration.Services.GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="f5f67-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="f5f67-145">Diese Methode ist von einem Controller über die **ApiController.Configuration** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f5f67-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="f5f67-146">Um eine Ablaufverfolgung zu schreiben, rufen Sie die **ITraceWriter.Trace** -Methode direkt, aber die [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) Klasse definiert einige Erweiterungsmethoden, die benutzerfreundlichere sind.</span><span class="sxs-lookup"><span data-stu-id="f5f67-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="f5f67-147">Z. B. die **Info** oben gezeigten Methode erstellt eine Ablaufverfolgung mit Ablaufverfolgungsebene **Info**.</span><span class="sxs-lookup"><span data-stu-id="f5f67-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="f5f67-148">Web-API-Infrastruktur für Ereignisablaufverfolgung</span><span class="sxs-lookup"><span data-stu-id="f5f67-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="f5f67-149">Dieser Abschnitt beschreibt, wie eine benutzerdefinierte ablaufverfolgungswriter für die Web-API schreiben.</span><span class="sxs-lookup"><span data-stu-id="f5f67-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="f5f67-150">Eine allgemeine Infrastruktur für ereignisablaufverfolgung in Web-API baut auf das Microsoft.AspNet.WebApi.Tracing-Paket.</span><span class="sxs-lookup"><span data-stu-id="f5f67-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="f5f67-151">Anstatt Microsoft.AspNet.WebApi.Tracing zu verwenden, können Sie auch anschließen in eine andere Bibliothek Ablaufverfolgung/Protokollierung, z. B. [NLog](http://nlog-project.org/) oder [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="f5f67-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/loggin library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="f5f67-152">Zum Sammeln von ablaufverfolgungen implementieren die **ITraceWriter** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="f5f67-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="f5f67-153">Hier ist ein einfaches Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f5f67-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="f5f67-154">Die **ITraceWriter.Trace** Methode erstellt eine Ablaufverfolgung.</span><span class="sxs-lookup"><span data-stu-id="f5f67-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="f5f67-155">Der Aufrufer gibt eine Kategorie und Trace-Ebene.</span><span class="sxs-lookup"><span data-stu-id="f5f67-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="f5f67-156">Die Kategorie kann es sich um eine beliebige benutzerdefinierte Zeichenfolge sein.</span><span class="sxs-lookup"><span data-stu-id="f5f67-156">The category can be any user-defined string.</span></span> <span data-ttu-id="f5f67-157">Ihre Implementierung von **Trace** Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="f5f67-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="f5f67-158">Erstellen Sie ein neues **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="f5f67-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="f5f67-159">Initialisieren Sie sie mit der Anforderung, Kategorie und der Ablaufverfolgungsebene, wie dargestellt.</span><span class="sxs-lookup"><span data-stu-id="f5f67-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="f5f67-160">Diese Werte werden vom Aufrufer bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="f5f67-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="f5f67-161">Aufrufen der *TraceAction* delegieren.</span><span class="sxs-lookup"><span data-stu-id="f5f67-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="f5f67-162">Der Aufrufer muss innerhalb dieser Delegat füllen Sie das restliche der **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="f5f67-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="f5f67-163">Schreiben der **TraceRecord**, verwenden Protokollierung-Technik, die Ihnen gefällt.</span><span class="sxs-lookup"><span data-stu-id="f5f67-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="f5f67-164">Die hier gezeigte Beispiel einfach Aufrufe **System.Diagnostics.Trace**.</span><span class="sxs-lookup"><span data-stu-id="f5f67-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="f5f67-165">Festlegen den Ablaufverfolgungswriter</span><span class="sxs-lookup"><span data-stu-id="f5f67-165">Setting the Trace Writer</span></span>

<span data-ttu-id="f5f67-166">Zum Aktivieren der Ablaufverfolgung müssen Sie konfigurieren, Web-API zur Verwendung Ihrer **ITraceWriter** Implementierung.</span><span class="sxs-lookup"><span data-stu-id="f5f67-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="f5f67-167">Hierzu Sie über die **HttpConfiguration** -Objekts, wie im folgenden Code gezeigt:</span><span class="sxs-lookup"><span data-stu-id="f5f67-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="f5f67-168">Nur ein ablaufverfolgungswriter kann aktiv sein.</span><span class="sxs-lookup"><span data-stu-id="f5f67-168">Only one trace writer can be active.</span></span> <span data-ttu-id="f5f67-169">Standardmäßig legt Web-API eine &quot;ohne-Op&quot; Überwachungstoken, die keine Aktionen ausführt.</span><span class="sxs-lookup"><span data-stu-id="f5f67-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="f5f67-170">(Die &quot;ohne-Op-&quot; Überwachungstoken vorhanden ist, sodass Ablaufverfolgungscode nicht überprüfen, ob der ablaufverfolgungswriter **null** vor dem Schreiben einer Ablaufverfolgungs.)</span><span class="sxs-lookup"><span data-stu-id="f5f67-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="f5f67-171">Wie Web API Tracing Works</span><span class="sxs-lookup"><span data-stu-id="f5f67-171">How Web API Tracing Works</span></span>

<span data-ttu-id="f5f67-172">Ablaufverfolgung in Web-API verwendet, die eine im Web-API verwendet einen *Fassade* Muster: Wenn Ablaufverfolgung aktiviert ist, bricht Web-API verschiedenen Bestandteile der Anforderungspipeline mit Klassen, die Trace-Aufrufe ausführen.</span><span class="sxs-lookup"><span data-stu-id="f5f67-172">Tracing in Web API uses a in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="f5f67-173">Zum Beispiel wenn Sie einen Controller auswählen, die Pipeline verwendet die **IHttpControllerSelector** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="f5f67-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="f5f67-174">Ohne die Ablaufverfolgung aktiviert, fügt der Pipleline eine Klasse, die implementiert **IHttpControllerSelector** jedoch Eigenschaftenaufrufe an die tatsächliche Implementierung:</span><span class="sxs-lookup"><span data-stu-id="f5f67-174">With tracing enabled, the pipleline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Web-API-Ablaufverfolgung verwendet das Fassade-Muster.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="f5f67-176">Dieser Entwurf bietet folgende Vorteile:</span><span class="sxs-lookup"><span data-stu-id="f5f67-176">The benefits of this design include:</span></span>

- <span data-ttu-id="f5f67-177">Wenn Sie keine ablaufverfolgungswriter hinzufügen, wird der ablaufverfolgungskomponenten nicht instanziiert und haben keine Auswirkungen auf die Leistung.</span><span class="sxs-lookup"><span data-stu-id="f5f67-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="f5f67-178">Wenn Sie z. B. Standarddienste ersetzen **IHttpControllerSelector** durch eine eigene benutzerdefinierte Implementierung Ablaufverfolgung ist nicht betroffen, da die Ablaufverfolgung durch die Wrapperobjekt erfolgt.</span><span class="sxs-lookup"><span data-stu-id="f5f67-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="f5f67-179">Sie können auch das gesamte Framework des Web-API-Ablaufverfolgung mit Ihren eigenen benutzerdefinierten Framework ersetzen, indem Sie durch das Ersetzen der standardmäßigen **ITraceManager** Dienst:</span><span class="sxs-lookup"><span data-stu-id="f5f67-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="f5f67-180">Implementieren **ITraceManager.Initialize** Ihre Ablaufverfolgungssystem initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="f5f67-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="f5f67-181">Beachten Sie, dass dies ersetzt die *gesamte* Trace-Framework, einschließlich aller dem Rückverfolgen von Code, der in der Web-API integriert ist.</span><span class="sxs-lookup"><span data-stu-id="f5f67-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
