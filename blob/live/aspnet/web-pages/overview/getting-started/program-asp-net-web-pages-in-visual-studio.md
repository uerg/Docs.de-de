---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programmierung ASP.NET Web Pages (Razor) mit Visual Studio | Microsoft Docs
author: tfitzmac
description: "In diesem Anhang wird erläutert, wie Sie Visual Studio 2010 noch Visual Web Developer 2010 Express Programm ASP.NET Web Pages mit Razor-Syntax verwenden können."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5cfeda206eda8fb3fd769d34fb40bae2c3b65093
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="6380f-103">Programmieren von ASP.NET Web Pages (Razor) mithilfe von Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6380f-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="6380f-104">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6380f-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6380f-105">In diesem Artikel wird erläutert, wie Sie Visual Studio oder Visual Web Developer Express Programm Websites mit ASP.NET Web Pages (Razor) verwenden können.</span><span class="sxs-lookup"><span data-stu-id="6380f-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
> 
> <span data-ttu-id="6380f-106">Lernen Sie</span><span class="sxs-lookup"><span data-stu-id="6380f-106">What you'll learn</span></span>
> 
> - <span data-ttu-id="6380f-107">Was müssen Sie (falls zutreffend) zum Arbeiten mit ASP.NET Web Pages in Ihrer Version von Visual Studio installieren.</span><span class="sxs-lookup"><span data-stu-id="6380f-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="6380f-108">Wie Sie die Unterstützung für ASP.NET Web Pages Visual Web Developer 2010 Express hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6380f-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="6380f-109">Wie Sie Funktionen zum Arbeiten mit ASP.NET Razor-Seiten, einschließlich IntelliSense und der Debugger in Visual Studio verwenden.</span><span class="sxs-lookup"><span data-stu-id="6380f-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6380f-110">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="6380f-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6380f-111">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="6380f-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="6380f-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6380f-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="6380f-113">WebMatrix-3</span><span class="sxs-lookup"><span data-stu-id="6380f-113">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="6380f-114">Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 und WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="6380f-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="6380f-115">Sie können ASP.NET Web Pages mit Razor-Syntax, die mithilfe von WebMatrix oder viele andere Code-Editoren programmieren.</span><span class="sxs-lookup"><span data-stu-id="6380f-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="6380f-116">Sie können auch Microsoft Visual Studio, eine voll ausgestattete integrierte Entwicklungsumgebung (IDE), die einen Satz leistungsstarken Tools bietet für viele Arten von Anwendungen (nicht nur Websites) erstellen.</span><span class="sxs-lookup"><span data-stu-id="6380f-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="6380f-117">Um mit ASP.NET Razor-Seiten arbeiten, können Sie entweder eine der vollständigen Editionen von Visual Studio oder das kostenlose [Visual Studio Express für Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) Edition.</span><span class="sxs-lookup"><span data-stu-id="6380f-117">To work with ASP.NET Razor pages, you can either use one of the full editions of Visual Studio or the free [Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span></span>

<span data-ttu-id="6380f-118">Zwei besonders nützliche, Funktionen, die Visual Studio für die Programmierung mit Webseiten ASP.NET Razor bereitstellt sind:</span><span class="sxs-lookup"><span data-stu-id="6380f-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="6380f-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="6380f-119">*IntelliSense*.</span></span> <span data-ttu-id="6380f-120">Die IntelliSense-Funktion in Visual Studio integriert ist umfassender als IntelliSense in WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6380f-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="6380f-121">*Debugger*.</span><span class="sxs-lookup"><span data-stu-id="6380f-121">*Debugger*.</span></span> <span data-ttu-id="6380f-122">Mit dem Debugger können Sie Ihren Code zu lösen, indem Sie ein Programm beenden, während er ausgeführt wird, untersuchen von Variablen und den Code zeilenweise schrittweise durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="6380f-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="6380f-123">Mithilfe von Visual Studio mit verschiedenen Versionen von ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="6380f-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="6380f-124">Visual Studio 2012 und Visual Studio 2013 enthalten die Unterstützung für ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="6380f-124">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="6380f-125">(Die Pakete, die erforderlich sind, um die Unterstützung von ASP.NET Web Pages sind installiert, bei der Installation von Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="6380f-125">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="6380f-126">Visual Studio 2010 umfasst Unterstützung standardmäßig für ASP.NET Web Pages nicht.</span><span class="sxs-lookup"><span data-stu-id="6380f-126">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="6380f-127">Um ASP.NET Web Pages mit Visual Studio 2010 verwenden zu können, müssen Sie das ASP.NET MVC-Paket installieren.</span><span class="sxs-lookup"><span data-stu-id="6380f-127">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="6380f-128">Um ASP.NET Web Pages 2 zu erhalten, installieren Sie ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="6380f-128">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="6380f-129">In der folgenden Tabelle werden die Unterstützung für ASP.NET Web Pages in verschiedenen Versionen von Visual Studio zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="6380f-129">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="6380f-130">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="6380f-130">Visual Studio 2010</span></span> | <span data-ttu-id="6380f-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="6380f-131">Visual Studio 2012</span></span> | <span data-ttu-id="6380f-132">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6380f-132">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6380f-133">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="6380f-133">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="6380f-134">Installieren Sie ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="6380f-134">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="6380f-135">(Enthalten)</span><span class="sxs-lookup"><span data-stu-id="6380f-135">(Included)</span></span> | <span data-ttu-id="6380f-136">(Enthalten)</span><span class="sxs-lookup"><span data-stu-id="6380f-136">(Included)</span></span> |
| <span data-ttu-id="6380f-137">**ASP.NET Web Pages 3**</span><span class="sxs-lookup"><span data-stu-id="6380f-137">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="6380f-138">Update für ASP.NET Web Pages 3 über NuGet</span><span class="sxs-lookup"><span data-stu-id="6380f-138">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="6380f-139">(Enthalten)</span><span class="sxs-lookup"><span data-stu-id="6380f-139">(Included)</span></span> |

<span data-ttu-id="6380f-140">Um mit Visual Studio 2010 arbeiten, finden Sie unter [Installieren von Unterstützung für ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="6380f-140">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="6380f-141">Starten Visual Studio aus WebMatrix</span><span class="sxs-lookup"><span data-stu-id="6380f-141">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="6380f-142">Wenn Sie ein Projekt, klicken Sie in WebMatrix gestartet haben und Visual Studio wechseln möchten, bietet WebMatrix eine Schaltfläche, um das Projekt in Visual Studio problemlos zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="6380f-142">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="6380f-143">Sie benötigen Visual Studio installiert, die auf Ihren Computer auf diese Schaltfläche aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="6380f-143">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="6380f-144">Die folgende Abbildung zeigt die Schaltfläche in WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6380f-144">The following image shows the button in WebMatrix.</span></span>

![Starten Sie Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="6380f-146">Wenn Sie die Schaltfläche klicken, wird das Projekt in Visual Studio geöffnet.</span><span class="sxs-lookup"><span data-stu-id="6380f-146">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="6380f-147">Sie können zwischen WebMatrix und Visual Studio hin-und ohne Probleme wechseln.</span><span class="sxs-lookup"><span data-stu-id="6380f-147">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="6380f-148">Sie werden benachrichtigt, wenn alle Dateien in der anderen Umgebung geändert haben und erneut geladen werden, damit die neuesten Änderungen abrufen müssen.</span><span class="sxs-lookup"><span data-stu-id="6380f-148">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="6380f-149">Erstellen von ASP.NET Razor-Website in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6380f-149">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="6380f-150">So erstellen eine ASP.NET Razor-Website in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="6380f-150">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="6380f-151">Starten Sie Visual Studio oder Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="6380f-151">Start Visual Studio or Visual Web Developer.</span></span>
2. <span data-ttu-id="6380f-152">In der **Datei** Menü klicken Sie auf **neue Website**.</span><span class="sxs-lookup"><span data-stu-id="6380f-152">In the **File** menu, click **New Web Site**.</span></span>

    ![neue Website erstellen](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="6380f-154">In der **neue Website** Dialogfeld wählen die Sprache an (Visual c# oder Visual Basic) verwenden.</span><span class="sxs-lookup"><span data-stu-id="6380f-154">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="6380f-155">Wählen Sie die **ASP.NET-Website (Razor)** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="6380f-155">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![Razor-Website](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="6380f-157">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="6380f-157">Click **OK**.</span></span>

<span data-ttu-id="6380f-158">Das neue Projekt vorhanden ist und mit einigen Webseiten Standard, um Ihnen beim Einstieg helfen gefüllt.</span><span class="sxs-lookup"><span data-stu-id="6380f-158">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="6380f-159">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="6380f-159">Using IntelliSense</span></span>

<span data-ttu-id="6380f-160">Nun, dass Sie eine Website erstellt haben, können Sie die Funktionsweise von IntelliSense in Visual Studio finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="6380f-160">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="6380f-161">Öffnen Sie auf der Website, die Sie gerade erstellt haben, die *Default.cshtml* Seite.</span><span class="sxs-lookup"><span data-stu-id="6380f-161">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="6380f-162">Nach der `<h3>` Tags auf der Seite eingeben `@ServerInfo.` (einschließlich des durch Punktes).</span><span class="sxs-lookup"><span data-stu-id="6380f-162">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="6380f-163">Beachten Sie, wie IntelliSense zeigt an, die verfügbaren Methoden für die `ServerInfo` Helper in einer Dropdownliste.</span><span class="sxs-lookup"><span data-stu-id="6380f-163">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span> 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="6380f-165">Wählen Sie die `GetHtml` Methode aus der Liste aus und drücken Sie dann die EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="6380f-165">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="6380f-166">IntelliSense ist in der Methode füllt.</span><span class="sxs-lookup"><span data-stu-id="6380f-166">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="6380f-167">(Wie jeder Methode in C# geschrieben, das Sie hinzufügen müssen `()` Zeichen nach der Methode.)</span><span class="sxs-lookup"><span data-stu-id="6380f-167">(As with any method in C#, you must add `()` characters after the method.)</span></span>  
 <span data-ttu-id="6380f-168">Der vollständige Code für die `GetHtml` -Methode sieht wie im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6380f-168">The completed code for the `GetHtml` method looks like the following example:</span></span>  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="6380f-169">Drücken Sie STRG + F5, um die Seite auszuführen.</span><span class="sxs-lookup"><span data-stu-id="6380f-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="6380f-170">Sieht die Seite wie wenn in einem Browser angezeigt:</span><span class="sxs-lookup"><span data-stu-id="6380f-170">This is what the page looks like when displayed in a browser:</span></span> 

    ![Standardseite im browser](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="6380f-172">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="6380f-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="6380f-173">Verwenden des Debuggers</span><span class="sxs-lookup"><span data-stu-id="6380f-173">Using the Debugger</span></span>

1. <span data-ttu-id="6380f-174">Am oberen Rand der *Default.cshtml* Seite nach der Zeile, die mit beginnt `Page.Title`, fügen Sie die folgende Codezeile hinzu:</span><span class="sxs-lookup"><span data-stu-id="6380f-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span> 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="6380f-175">Klicken Sie in den grauen Rand des Editors auf der linken Seite des Codes auf neben dieser neuen Zeile zum Hinzufügen einer *Haltepunkt*.</span><span class="sxs-lookup"><span data-stu-id="6380f-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="6380f-176">Ein Haltepunkt ist ein Marker, der weist den Debugger zu beenden, das Programm zu diesem Zeitpunkt ausgeführt werden, damit Sie sehen, was geschieht.</span><span class="sxs-lookup"><span data-stu-id="6380f-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![Haltepunkt festlegen](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="6380f-178">Entfernen Sie den Aufruf an die `ServerInfo.GetHtml` -Methode, und fügen Sie einen Aufruf an die `@myTime` an seiner Stelle Variable.</span><span class="sxs-lookup"><span data-stu-id="6380f-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="6380f-179">Dieser Aufruf zeigt die aktuelle Zeitwert, der von der neuen Zeile des Codes zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="6380f-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="6380f-180">Drücken Sie F5, um die Seite im Debugger auszuführen.</span><span class="sxs-lookup"><span data-stu-id="6380f-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="6380f-181">Die Seite hält am Haltepunkt, den Sie festlegen.</span><span class="sxs-lookup"><span data-stu-id="6380f-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="6380f-182">Die folgende Abbildung zeigt, wie die Seite im Editor mit dem Haltepunkt (in Gelb) aussieht.</span><span class="sxs-lookup"><span data-stu-id="6380f-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span> 

    ![Haltepunkt Debuggen](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="6380f-184">Klicken Sie in der Debug-Symbolleiste auf die **Einzelschritt** Schaltfläche (oder drücken Sie F11) auf die nächste Zeile des Codes ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6380f-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="6380f-185">Bei jedem Klicken auf diese Schaltfläche, wechseln Sie mit die Ausführung in die nächste Zeile des Codes.</span><span class="sxs-lookup"><span data-stu-id="6380f-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Schaltfläche Einzelschritt](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="6380f-187">Überprüfen Sie den Wert, der die `myTime` Variable, indem Sie den Mauszeiger darüber halten oder Überprüfen der Werte angezeigt, der **"lokal"** und **Aufrufliste** Windows.</span><span class="sxs-lookup"><span data-stu-id="6380f-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="6380f-188">Den Wert der Variablen in Visual Studio angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6380f-188">Visual Studio display the value of the variable.</span></span>

    ![Zeit-Wert](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="6380f-190">Wenn Sie fertig sind, untersuchen von Variablen und Code schrittweise durchlaufen, drücken Sie F5, um die Seite ausgeführt, ohne bei jeder Zeile fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="6380f-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="6380f-191">Wenn Sie alle den Code schrittweise durchlaufen haben, zeigt der Browser die Seite.</span><span class="sxs-lookup"><span data-stu-id="6380f-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="6380f-192">Weitere Informationen über den Debugger und Informationen zum Debuggen von Code in Visual Studio finden Sie unter [Exemplarische Vorgehensweise: Debuggen von Webseiten in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="6380f-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="6380f-193">Verwenden von Razor in ASP.NET MVC-Projekte mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6380f-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="6380f-194">Die Razor-Syntax ist auch sehr häufig in ASP.NET MVC-Projekte verwendet.</span><span class="sxs-lookup"><span data-stu-id="6380f-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="6380f-195">MVC ist eine leistungsfähige, auf Mustern basierende Möglichkeit zum Erstellen dynamischer Websites.</span><span class="sxs-lookup"><span data-stu-id="6380f-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="6380f-196">Wenn Ihre Website für ASP.NET Web Pages schwierig zu verwalten ist, empfiehlt es sich, sollten ihn in einer ASP.NET MVC-Anwendung zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="6380f-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="6380f-197">Ein Beispiel zum Erstellen einer MVC-Anwendung finden Sie unter [erste Schritte mit ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="6380f-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="6380f-198">Installieren von Unterstützung für ASP.NET Web Pages in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="6380f-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="6380f-199">In diesem Abschnitt wird gezeigt, wie Visual Web Developer Express 2010 und die ASP.NET Web Pages-Tools für Visual Studio installiert.</span><span class="sxs-lookup"><span data-stu-id="6380f-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="6380f-200">Wenn Sie den Webplattform-Installer noch nicht haben, laden Sie es unter folgender URL herunter:</span><span class="sxs-lookup"><span data-stu-id="6380f-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [<span data-ttu-id="6380f-201">https://www.Microsoft.com/Web/Downloads/Platform.aspx</span><span class="sxs-lookup"><span data-stu-id="6380f-201">https://www.microsoft.com/web/downloads/platform.aspx</span></span>](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="6380f-202">Führen Sie den Webplattform-Installer.</span><span class="sxs-lookup"><span data-stu-id="6380f-202">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="6380f-203">Klicken Sie auf die **Produkte** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="6380f-203">Click the **Products** tab.</span></span>

    ![Registerkarte "WebPI Produkte"](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="6380f-205">Suchen Sie nach **ASP.NET MVC 4** (für ASP.NET Web Pages 2), und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="6380f-205">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="6380f-206">Diese Produkte umfassen Visual Studio-Tools zum Erstellen von ASP.NET Razor-Websites.</span><span class="sxs-lookup"><span data-stu-id="6380f-206">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![WebPi-Installationsoptionen](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="6380f-208">Klicken Sie auf **installieren** um die Installation abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="6380f-208">Click **Install** to complete the installation.</span></span>
