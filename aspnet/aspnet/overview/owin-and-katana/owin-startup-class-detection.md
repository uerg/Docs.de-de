---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Owin-Start Klasse Erkennung | Microsoft Docs
author: Praburaj
description: "Dieses Lernprogramm zeigt, wie zum Konfigurieren der OWIN-Start-Klasse geladen wird. Weitere Informationen zu OWIN finden Sie eine Übersicht über Project Katana. Dieses Lernprogramm wurde..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: a6ac34307b7558ad13684448f339ca74ade9e997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="5ac4e-105">Erkennung von owin-Start-Klasse</span><span class="sxs-lookup"><span data-stu-id="5ac4e-105">OWIN Startup Class Detection</span></span>
====================
<span data-ttu-id="5ac4e-106">durch [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="5ac4e-106">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="5ac4e-107">Dieses Lernprogramm zeigt, wie zum Konfigurieren der OWIN-Start-Klasse geladen wird.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-107">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="5ac4e-108">Weitere Informationen zu OWIN, finden Sie unter [eine Übersicht über Project Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="5ac4e-108">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="5ac4e-109">Dieses Lernprogramm wurde von Rick Anderson geschrieben ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan und Howard Dierking ( [ @howard \_Dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="5ac4e-109">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
> 
> ## <a name="prerequisites"></a><span data-ttu-id="5ac4e-110">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="5ac4e-110">Prerequisites</span></span>
> 
> [<span data-ttu-id="5ac4e-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5ac4e-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="5ac4e-112">Erkennung von owin-Start-Klasse</span><span class="sxs-lookup"><span data-stu-id="5ac4e-112">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="5ac4e-113">Jede OWIN-Anwendung verfügt über eine Autostart-Klasse, in dem Sie Komponenten für die Pipeline der Anwendung angeben.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-113">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="5ac4e-114">Es gibt verschiedene Möglichkeiten, die Sie mit der Common Language Runtime die Startklasse verbinden können, abhängig von der Host-Modell (OwinHost, IIS und IIS Express) auswählen.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-114">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="5ac4e-115">Die Startklasse. in diesem Lernprogramm gezeigt, kann in jeder hostanwendung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-115">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="5ac4e-116">Sie verbinden die Startklasse. mit der hosting Common Language Runtime verwenden, die einen dieser Ansätze:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-116">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>  

1. <span data-ttu-id="5ac4e-117">**Namenskonvention**: Katana sucht eine Klasse namens `Startup` im Namespace der Name der Assembly oder den globalen Namespace entsprechen.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-117">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="5ac4e-118">**OwinStartup Attribut**: Dies ist der Ansatz, der meisten Entwickler dauert die Startklasse angeben.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-118">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="5ac4e-119">Das folgende Attribut wird die Startklasse festgelegt, um die `TestStartup` -Klasse in der `StartupDemo` Namespace.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-119">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span> 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

 <span data-ttu-id="5ac4e-120">Die `OwinStartup` Attribut überschreibt die Benennungskonvention verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-120">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="5ac4e-121">Sie können auch einen Anzeigenamen mit diesem Attribut angegeben ist, jedoch über einen Anzeigenamen erfordert, dass Sie auch verwenden, die `appSetting` Element in der Konfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-121">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="5ac4e-122">**AppSetting-Element in der Konfigurationsdatei**: die `appSetting` Element überschreibt die `OwinStartup` Attribut und Benennungskonvention.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-122">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="5ac4e-123">Kann auch über mehrere Autostart-Klassen (jede mit einer `OwinStartup` Attribut) und konfigurieren, welche Startklasse in einer Konfigurationsdatei mit dem Markup ähnlich der folgenden geladen werden:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-123">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

 <span data-ttu-id="5ac4e-124">Die folgende Schlüssel, der explizit die Startklasse und die Assembly angibt, kann auch verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-124">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

 <span data-ttu-id="5ac4e-125">Die folgenden XML-Code in der Konfigurationsdatei gibt einen benutzerfreundlichen Start Klassennamen des `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-125">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

 <span data-ttu-id="5ac4e-126">Die oben genannten Markup muss verwendet werden, mit den folgenden `OwinStartup` -Typattribut gibt einen Anzeigenamen und bewirkt, dass die `ProductionStartup2` Klasse ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-126">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="5ac4e-127">So deaktivieren Sie die OWIN-startermittlung Hinzufügen der `appSetting owin:AutomaticAppStartup` mit einem Wert von `"false"` in der Datei "Web.config".</span><span class="sxs-lookup"><span data-stu-id="5ac4e-127">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="5ac4e-128">Erstellen einer ASP.NET Web-App mithilfe von OWIN-Start</span><span class="sxs-lookup"><span data-stu-id="5ac4e-128">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="5ac4e-129">Erstellen Sie eine leere ASP.NET-Webanwendung und nennen sie **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-129">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="5ac4e-130">-Installieren Sie `Microsoft.Owin.Host.SystemWeb` mithilfe des NuGet-Paket-Managers.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-130">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="5ac4e-131">Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und klicken Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-131">From the **Tools** menu, select **Library Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="5ac4e-132">Geben Sie folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-132">Enter the following command:</span></span>  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
- <span data-ttu-id="5ac4e-133">Fügen Sie eine OWIN-Start-Klasse.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-133">Add an OWIN startup class.</span></span> <span data-ttu-id="5ac4e-134">In Visual Studio 2013 mit der rechten Maustaste auf das Projekt, und wählen Sie **Klasse hinzufügen**. – In der **neues Element hinzufügen** Dialogfeld Geben Sie *OWIN* suchen (Feld), und ändern Sie den Namen in Startup.cs, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-134">In Visual Studio 2013 right click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then click **Add**.</span></span>  
  
    ![](owin-startup-class-detection/_static/image1.png)   
  
 <span data-ttu-id="5ac4e-135">Das nächste Mal hinzugefügt werden soll ein *Owin Startklasse*, werden in von der **hinzufügen** im Menü.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-135">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>  
   
    ![](owin-startup-class-detection/_static/image2.png)  
  
 <span data-ttu-id="5ac4e-136">Alternativ können Sie mit der rechten Maustaste, klicken Sie auf das Projekt und wählen Sie **hinzufügen**, und wählen Sie dann **neues Element**, und wählen Sie dann die **Owin Startklasse**.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-136">Alternatively, you can right click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>  
  
    ![](owin-startup-class-detection/_static/image3.png)  
  
- <span data-ttu-id="5ac4e-137">Ersetzen Sie den generierten Code in der *Startup.cs* Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-137">Replace the generated code in the *Startup.cs* file with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
 <span data-ttu-id="5ac4e-138">Die `app.Use` Lambda-Ausdruck wird verwendet, um die angegebenen middlewarekomponente, für die OWIN-Pipeline zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-138">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="5ac4e-139">In diesem Fall werden wir die Protokollierung von eingehenden Anforderungen vor dem Antworten auf die eingehende Anforderung einrichten.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-139">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="5ac4e-140">Die `next` Parameter wird der Delegat ( [Func](https://msdn.microsoft.com/en-us/library/bb534960(v=vs.100).aspx) &lt; [Aufgabe](https://msdn.microsoft.com/en-us/library/dd321424(v=vs.100).aspx) &gt; ) an die nächste Komponente in der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-140">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/en-us/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/en-us/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="5ac4e-141">Die `app.Run` Lambda-Ausdruck, der Pipeline auf eingehende Anforderungen verknüpft, und stellt den Mechanismus für die Antwort.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-141">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="5ac4e-142">Im obigen Code haben wir auskommentiert ist die `OwinStartup` -Attribut, und wir sind der Konvention aus der Klasse mit dem Namen der vertrauenden Seite `Startup` .-drücken ***F5*** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-142">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="5ac4e-143">Drücken Sie die aktualisierungstaste mehrmals.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-143">Hit refresh a few times.</span></span>  
  
    ![](owin-startup-class-detection/_static/image4.png)  
<span data-ttu-id="5ac4e-144">Hinweis: Die Anzahl der Bilder in diesem Lernprogramm gezeigt entspricht nicht die Zahl angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-144">Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="5ac4e-145">Die Millisekunde Zeichenfolge wird verwendet, um eine neue Antwort anzuzeigen, wenn Sie die Seite aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-145">The millisecond string is used to show a new response when you refresh the page.</span></span>  
 <span data-ttu-id="5ac4e-146">Sehen Sie die Ablaufverfolgungsinformationen in die **Ausgabe** Fenster.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-146">You can see the trace information in the **Output** window.</span></span>  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="5ac4e-147">Fügen Sie weitere Startup-Klassen</span><span class="sxs-lookup"><span data-stu-id="5ac4e-147">Add More Startup Classes</span></span>

<span data-ttu-id="5ac4e-148">In diesem Abschnitt fügen wir einen anderen Startklasse.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-148">In this section we'll add another Startup class.</span></span> <span data-ttu-id="5ac4e-149">Sie können mehrere OWIN-Startklasse für Ihre Anwendung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-149">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="5ac4e-150">Sie möchten z. B. Start Klassen für Entwicklungs-, Test- und Produktionszwecke erstellen.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-150">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="5ac4e-151">Erstellen Sie eine neue OWIN-Start-Klasse, und nennen Sie sie `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-151">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="5ac4e-152">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-152">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="5ac4e-153">Drücken Sie F5-Steuerelement, um die app auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-153">Press Control F5 to run the app.</span></span> <span data-ttu-id="5ac4e-154">Die `OwinStartup` Attribut gibt an, die Produktion Startklasse ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-154">The `OwinStartup` attribute specifies the production startup class is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="5ac4e-155">Erstellen Sie eine andere OWIN-Start-Klasse, und nennen Sie sie `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-155">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="5ac4e-156">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-156">Replace the generated code with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

 <span data-ttu-id="5ac4e-157">Die `OwinStartup` Attribut-Überladung, die oben genannten gibt `TestingConfiguration` als die *angezeigten* -Namen der Start-Klasse.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-157">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="5ac4e-158">Öffnen der *"Web.config"* Datei, und fügen Sie den OWIN-App-Systemstartschlüssel gibt den Anzeigenamen der Startklasse an:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-158">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="5ac4e-159">Drücken Sie F5-Steuerelement, um die app auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-159">Press Control F5 to run the app.</span></span> <span data-ttu-id="5ac4e-160">Das app-Einstellungen-Element akzeptiert Vorgänger und den Test Konfiguration ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-160">The app settings element takes precedent, and the test configuration is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="5ac4e-161">Entfernen der *angezeigten* Namen aus der `OwinStartup` Attribut in der `TestStartup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-161">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="5ac4e-162">Ersetzen Sie den Schlüssel zum Systemstart OWIN-App in der *"Web.config"* Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-162">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="5ac4e-163">Wiederherstellen der `OwinStartup` Attribut in jeder Klasse, um den Standardcode-Attribut von Visual Studio generiert:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-163">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

 <span data-ttu-id="5ac4e-164">Jede der OWIN-App-Systemstartschlüssel unten führt dazu, dass die Produktions-Klasse zum Ausführen.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-164">Each of the OWIN App startup keys below will cause the production class to run.</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

 <span data-ttu-id="5ac4e-165">Die letzte Schlüssel zum Systemstart gibt die Startmethode der Konfiguration an.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-165">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="5ac4e-166">Die folgenden Schlüssel zum Systemstart OWIN-App können Sie den Namen der Konfigurationsklasse, die ändern `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="5ac4e-166">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="5ac4e-167">Verwenden von Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="5ac4e-167">Using Owinhost.exe</span></span>

1. <span data-ttu-id="5ac4e-168">Ersetzen Sie die Datei "Web.config" durch das folgende Markup:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-168">Replace the Web.config file with the following markup:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

 <span data-ttu-id="5ac4e-169">Der Schlüssel für die zuletzt wins, dies in diesem Fall `TestStartup` angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-169">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="5ac4e-170">Installieren Sie Owinhost aus der PMC:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-170">Install Owinhost from the PMC:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="5ac4e-171">Navigieren Sie zum Ordner Anwendung (Ordner mit der *"Web.config"* Datei) und in einem Eingabeaufforderungsfenster und Typ:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-171">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

 <span data-ttu-id="5ac4e-172">Es wird im Befehlsfenster angezeigt:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-172">The command window will show:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="5ac4e-173">Starten Sie einen Browser mit der URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-173">Launch a browser with the URL `http://localhost:5000/`.</span></span>  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
 <span data-ttu-id="5ac4e-174">OwinHost berücksichtigt die Start-Konventionen, die oben aufgeführten.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-174">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="5ac4e-175">In das Befehlsfenster die EINGABETASTE, um OwinHost zu beenden.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-175">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="5ac4e-176">In der `ProductionStartup` -Klasse verwenden, fügen Sie das folgende OwinStartup-Attribut gibt einen Anzeigenamen an *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-176">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="5ac4e-177">In der Eingabeaufforderung und geben:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-177">In the command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

 <span data-ttu-id="5ac4e-178">Die Startklasse Produktion wird geladen.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-178">The Production startup class is loaded.</span></span>  
    ![](owin-startup-class-detection/_static/image9.png)  
 <span data-ttu-id="5ac4e-179">Anwendung hat mehrere Autostart-Klassen, und in diesem Beispiel haben wir die Startklasse. bis zur Laufzeit laden verzögert.</span><span class="sxs-lookup"><span data-stu-id="5ac4e-179">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="5ac4e-180">Testen Sie die folgenden Startoptionen für die Common Language Runtime:</span><span class="sxs-lookup"><span data-stu-id="5ac4e-180">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
