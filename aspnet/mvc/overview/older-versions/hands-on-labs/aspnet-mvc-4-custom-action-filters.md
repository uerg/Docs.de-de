---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: ASP.NET MVC 4 benutzerdefinierte Aktionsfilter | Microsoft Docs
author: rick-anderson
description: "ASP.NET MVC bietet Aktionsfilter für das Ausführen von Filterlogik entweder vor oder nach eine Aktionsmethode aufgerufen wird. Aktionsfilter sind benutzerdefinierte Attribute Tha..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 6362f0506934ca3b3cc86e1a927af75e7bc4e1d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="c1c5c-104">ASP.NET MVC 4 benutzerdefinierten Aktionsfiltern</span><span class="sxs-lookup"><span data-stu-id="c1c5c-104">ASP.NET MVC 4 Custom Action Filters</span></span>
====================
<span data-ttu-id="c1c5c-105">durch [Web Lager Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c1c5c-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="c1c5c-106">ASP.NET MVC bietet Aktionsfilter für das Ausführen von Filterlogik entweder vor oder nach eine Aktionsmethode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-106">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="c1c5c-107">Aktionsfilter sind benutzerdefinierte Attribute, die bereitstellen, deklaratives Mittel für den Controller Aktionsmethoden einfügen vor und nach Abschluss der Aktion Verhalten hinzu.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-107">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>
> 
> <span data-ttu-id="c1c5c-108">In dieser praktischen Übungseinheit erstellen Sie eine benutzerdefinierte Aktionsfilterattribut in MvcMusicStore Lösung zum Abfangen von Anforderungen des Controllers und melden Sie sich die Aktivität von einem Standort in einer Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-108">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="c1c5c-109">Sie werden können den Protokollierungsfilter von Injection Controller bzw. die Aktionsmethode hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-109">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="c1c5c-110">Schließlich wird die Protokollansicht angezeigt, die die Liste der Besucher anzeigt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-110">Finally, you will see the log view that shows the list of visitors.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="c1c5c-111">Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse im **ASP.NET-MVC**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-111">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="c1c5c-112">Wenn Sie nicht verwendet haben **ASP.NET-MVC** vorher, empfehlen wir Ihnen, durchlaufen **ASP.NET MVC 4-Grundlagen** praktische Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-112">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="c1c5c-113">Ziele</span><span class="sxs-lookup"><span data-stu-id="c1c5c-113">Objectives</span></span>

<span data-ttu-id="c1c5c-114">In dieser praktischen Übungseinheit erfahren Sie, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-114">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="c1c5c-115">Erstellen Sie eine benutzerdefinierte Aktionsfilterattribut zum Erweitern von Filterfunktionen</span><span class="sxs-lookup"><span data-stu-id="c1c5c-115">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="c1c5c-116">Wenden Sie ein benutzerdefiniertes Filterattribut an, indem Sie Injection auf einen bestimmten Grad an</span><span class="sxs-lookup"><span data-stu-id="c1c5c-116">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="c1c5c-117">Registrieren Sie einen benutzerdefinierten Aktionsfiltern Global</span><span class="sxs-lookup"><span data-stu-id="c1c5c-117">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c1c5c-118">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="c1c5c-118">Prerequisites</span></span>

<span data-ttu-id="c1c5c-119">Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-119">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="c1c5c-120">[Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).</span><span class="sxs-lookup"><span data-stu-id="c1c5c-120">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c1c5c-121">Setup</span><span class="sxs-lookup"><span data-stu-id="c1c5c-121">Setup</span></span>

<span data-ttu-id="c1c5c-122">**Installieren von Codeausschnitten**</span><span class="sxs-lookup"><span data-stu-id="c1c5c-122">**Installing Code Snippets**</span></span>

<span data-ttu-id="c1c5c-123">Der Einfachheit halber gilt Großteil des Codes, den Sie entlang dieser Übung verwalten als Visual Studio-Codeausschnitte verfügbar.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-123">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="c1c5c-124">So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-124">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="c1c5c-125">Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang "c:" mithilfe von Code Snippets](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-125">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c1c5c-126">Übungen</span><span class="sxs-lookup"><span data-stu-id="c1c5c-126">Exercises</span></span>

<span data-ttu-id="c1c5c-127">Diese praktische Übungseinheit wird durch den folgenden Übungen umfasst:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-127">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="c1c5c-128">Übung 1: Protokollieren von Aktionen</span><span class="sxs-lookup"><span data-stu-id="c1c5c-128">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="c1c5c-129">Übung 2: Verwalten mehrerer Aktionsfilter</span><span class="sxs-lookup"><span data-stu-id="c1c5c-129">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="c1c5c-130">Geschätzte Zeit zum Abschließen dieser testumgebung: **30 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-130">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="c1c5c-131">Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-131">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="c1c5c-132">Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-132">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="c1c5c-133">Übung 1: Protokollieren von Aktionen</span><span class="sxs-lookup"><span data-stu-id="c1c5c-133">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="c1c5c-134">In dieser Übung erfahren Sie, wie eine benutzerdefinierte Aktion Protokollfilter erstellt mithilfe von ASP.NET MVC 4-Filteranbieter.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-134">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="c1c5c-135">Zu diesem Zweck werden Sie einen Protokollierungsfilter für die MusicStore Site gelten, die alle Aktivitäten in der ausgewählten Controller aufgezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-135">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="c1c5c-136">Erweitern Sie der Filter **ActionFilterAttributeClass** und überschreiben **OnActionExecuting** Methode, um jede Anforderung abfangen und führen Sie dann das Protokollieren von Aktionen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-136">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="c1c5c-137">Die Kontextinformationen über HTTP-Anforderungen, Ausführen von Methoden, Ergebnisse und Parameter bereitgestellt werden von ASP.NET MVC **ActionExecutingContext** Klasse **.**</span><span class="sxs-lookup"><span data-stu-id="c1c5c-137">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="c1c5c-138">ASP.NET MVC 4 hat auch die Filter-Standardanbieter, die Sie verwenden können, ohne einen benutzerdefinierten Filter erstellen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-138">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="c1c5c-139">ASP.NET MVC 4 stellt die folgenden Arten von Filtern:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-139">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="c1c5c-140">**Autorisierung** zu filtern, wodurch sicherheitsrelevanten Aspekten, ob eine Aktionsmethode, z. B. zur Authentifizierung oder zum Überprüfen der Eigenschaften der Anforderung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-140">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="c1c5c-141">**Aktion** Filter, der die aktionsmethodenausführung umschließt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-141">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="c1c5c-142">Dieser Filter kann zusätzliche Verarbeitungsschritte, z. B. Bereitstellen zusätzlicher Daten für die Aktionsmethode, das Überprüfen des Rückgabewerts oder das Abbrechen der Ausführung der Aktionsmethode ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-142">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="c1c5c-143">**Ergebnis** Filter, der Ausführung der ActionResult-Objekt umschließt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-143">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="c1c5c-144">Dieser Filter kann zusätzliche Verarbeitung des Ergebnisses, z. B. Ändern der HTTP-Antwort ausführen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-144">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="c1c5c-145">**Ausnahme** Filter, der ausgeführt wird, wenn es eine nicht behandelte Ausnahme, die an einer beliebigen Stelle in der Aktionsmethode, die mit den Autorisierungsfilter beginnt und endet mit der Ausführung des Ergebnisses ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-145">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="c1c5c-146">Ausnahmefilter können für Aufgaben wie die Protokollierung oder Anzeigen einer Fehlerseite verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-146">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="c1c5c-147">Weitere Informationen zu Filter-Anbietern finden Sie auf diesen Link MSDN: ([https://msdn.microsoft.com/en-us/library/dd410209.aspx](https://msdn.microsoft.com/en-us/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="c1c5c-147">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/en-us/library/dd410209.aspx](https://msdn.microsoft.com/en-us/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="c1c5c-148">Informationen zu MVC Music Store-Anwendung Protokollierung (Feature)</span><span class="sxs-lookup"><span data-stu-id="c1c5c-148">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="c1c5c-149">Diese Lösung Music Store hat eine neue Datentabelle Modell für die Website Protokollierung **ActionLog**, mit den folgenden Feldern: Name des Controllers, die eine Anforderung, die aufgerufene Aktion, die Client-IP und die Zeitstempel erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-149">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="c1c5c-150">![Entity Data Model. ActionLog-Tabelle. ] (aspnet-mvc-4-custom-action-filters/_static/image1.png "Datenmodell. ActionLog-Tabelle.")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-150">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="c1c5c-151">*Datenmodell - ActionLog-Tabelle*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-151">*Data model - ActionLog table*</span></span>

<span data-ttu-id="c1c5c-152">Die Lösung bietet eine ASP.NET MVC-Ansicht für das Aktionsprotokoll, die am befinden **MvcMusicStores/Ansichten/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-152">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="c1c5c-153">![Aktion protokollsicht](aspnet-mvc-4-custom-action-filters/_static/image2.png "Aktionsprotokoll anzeigen")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-153">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="c1c5c-154">*Aktion Protokollansicht*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-154">*Action Log view*</span></span>

<span data-ttu-id="c1c5c-155">Mit dieser Struktur erhält wird die gesamte Arbeit konzentriert sich werden, auf Anforderung des Controllers unterbrechen und die Protokollierung mit benutzerdefinierten Filtern ausführen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-155">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="c1c5c-156">Aufgabe 1: Erstellen eines benutzerdefinierten Filters zum Abfangen von einem Domänencontroller-Anforderung</span><span class="sxs-lookup"><span data-stu-id="c1c5c-156">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="c1c5c-157">In dieser Aufgabe erstellen Sie eine benutzerdefinierten Filter Attributklasse, die die protokollierungslogik enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-157">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="c1c5c-158">Erweitern Sie zu diesem Zweck ASP.NET-MVC **ActionFilterAttribute** Klasse und Implementieren der Schnittstelle **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-158">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="c1c5c-159">Die **ActionFilterAttribute** ist die Basisklasse für die Attribut-Filter.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-159">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="c1c5c-160">Es bietet die folgenden Methoden, um eine bestimmte Logik nach und vor der Ausführung des Controlleraktion auszuführen:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-160">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="c1c5c-161">**OnActionExecuting**(ActionExecutingContext FilterContext): nur vor der Aktion wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-161">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="c1c5c-162">**OnActionExecuted**(ActionExecutedContext FilterContext): Nachdem die Aktionsmethode aufgerufen wurde und bevor das Ergebnis (vor dem Rendern der Ansicht) ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-162">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="c1c5c-163">**OnResultExecuting**(ResultExecutingContext FilterContext): unmittelbar vor dem Ausführen des Ergebnisses (vor dem Rendern der Ansicht).</span><span class="sxs-lookup"><span data-stu-id="c1c5c-163">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="c1c5c-164">**OnResultExecuted**(ResultExecutedContext FilterContext): nach dem Ausführen des Ergebnisses (nach dem Rendern der Ansicht).</span><span class="sxs-lookup"><span data-stu-id="c1c5c-164">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="c1c5c-165">Durch das Überschreiben einer dieser Methoden in einer abgeleiteten Klasse, können Sie Ihren eigenen Filter Code ausführen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-165">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="c1c5c-166">Öffnen der **beginnen** Lösung finden Sie unter **\Source\Ex01-LoggingActions\Begin** Ordner.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-166">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

    1. <span data-ttu-id="c1c5c-167">Sie müssen einige fehlende NuGet-Pakete herunterladen, bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-167">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="c1c5c-168">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-168">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="c1c5c-169">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-169">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="c1c5c-170">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-170">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c1c5c-171">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-171">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c1c5c-172">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-172">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c1c5c-173">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-173">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="c1c5c-174">Weitere Informationen finden Sie im Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="c1c5c-174">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="c1c5c-175">Fügen Sie eine neue C#-Klasse in der **Filter** Ordner und nennen Sie sie *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-175">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="c1c5c-176">In diesem Ordner werden die benutzerdefinierten Filter gespeichert.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-176">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="c1c5c-177">Open **CustomActionFilter.cs** und Hinzufügen eines Verweises auf **System.Web.Mvc** und **MvcMusicStore.Models** Namespaces:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-177">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="c1c5c-178">(Codeausschnitt - *benutzerdefinierte Aktionsfilter ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="c1c5c-178">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="c1c5c-179">Erben der **CustomActionFilter** -Klasse aus **ActionFilterAttribute** und stellen Sie dann **CustomActionFilter** Klasse implementieren **IActionFilter** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-179">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="c1c5c-180">Stellen Sie **CustomActionFilter** Klasse überschreiben Sie die Methode **OnActionExecuting** und fügen Sie die erforderliche Logik zum Protokollieren der Ausführung der Filter hinzu.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-180">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="c1c5c-181">Zu diesem Zweck fügen Sie den folgenden hervorgehobenen Code innerhalb **CustomActionFilter** Klasse.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-181">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="c1c5c-182">(Codeausschnitt - *benutzerdefinierte Aktionsfilter ASP.NET MVC 4 - Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="c1c5c-182">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="c1c5c-183">**OnActionExecuting** Methode ist die Verwendung **Entity Framework** zum Hinzufügen eines neuen ActionLog-Registers.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-183">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="c1c5c-184">Erstellt und füllt Sie mit die Kontextinformationen über eine neue Entitätsinstanz **FilterContext**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-184">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="c1c5c-185">Sie können erfahren Sie mehr über **ControllerContext** am-Klasse [Msdn](https://msdn.microsoft.com/en-us/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1c5c-185">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/en-us/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="c1c5c-186">Aufgabe 2: Räumen einen Code-Interceptor in die Speicher-Controller-Klasse</span><span class="sxs-lookup"><span data-stu-id="c1c5c-186">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="c1c5c-187">In dieser Aufgabe fügen Sie den benutzerdefinierten Filter von Räumen es auf alle Controllerklassen und Controlleraktionen, die protokolliert werden.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-187">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="c1c5c-188">Im Rahmen dieser Übung wird die Store-Controllerklasse ein Protokoll haben.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-188">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="c1c5c-189">Die Methode **OnActionExecuting** aus **ActionLogFilterAttribute** benutzerdefinierter Filter ausgeführt wird, wenn ein eingefügten Element aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-189">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="c1c5c-190">Es ist auch möglich, eine bestimmte Controllermethode abzufangen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-190">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="c1c5c-191">Öffnen der **StoreController** am **MvcMusicStore\Controllers** und fügen einen Verweis auf die **Filter** Namespace:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-191">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="c1c5c-192">Einfügen von benutzerdefinierten Filter **CustomActionFilter** in **StoreController** Klasse durch Hinzufügen von **[CustomActionFilter]** Attribut vor der Klassendeklaration.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-192">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

    > [!NOTE]
    > <span data-ttu-id="c1c5c-193">Wenn Sie ein Filter in einer Controllerklasse eingefügt wird, werden auch alle zugehörigen Aktionen eingefügt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-193">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="c1c5c-194">Wenn Sie den Filter nur für eine Reihe von Aktionen anwenden möchten, müssten Sie einfügen **[CustomActionFilter]** jeweils davon:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-194">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="c1c5c-195">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="c1c5c-195">Task 3 - Running the Application</span></span>

<span data-ttu-id="c1c5c-196">In dieser Aufgabe testen Sie, dass die Protokollierungsfilter arbeitet.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-196">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="c1c5c-197">Sie startet die Anwendung und den Store besuchen, und klicken Sie dann prüft protokollierte Aktivitäten.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-197">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="c1c5c-198">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-198">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="c1c5c-199">Navigieren Sie zu **/ActionLog** zum ersten Protokoll Ansichtszustand finden Sie unter:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-199">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="c1c5c-200">![Protokollstatus Tracker vor der Aktivität "Seite"](aspnet-mvc-4-custom-action-filters/_static/image3.png "Tracker Protokollstatus vor der Aktivität "Seite"")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-200">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="c1c5c-201">*Protokoll Tracker Status vor der Aktivität "Seite"*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-201">*Log tracker status before page activity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c1c5c-202">Standardmäßig wird immer ein Element angezeigt, die generiert wird, wenn Sie die vorhandenen Genres für das Menü abrufen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-202">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
    > 
    > <span data-ttu-id="c1c5c-203">Aus Gründen der Einfachheit halber haben wir Sie Bereinigen der **ActionLog** Tabelle jedes Mal die Anwendung ausgeführt wird, damit sie nur die Protokolle der Überprüfung für jede bestimmte Aufgabe angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-203">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
    > 
    > <span data-ttu-id="c1c5c-204">Sie müssen unter Umständen entfernen den folgenden Code aus der **Sitzung\_starten** Methode (in der **"Global.asax"** Klasse), um ein Verlaufsprotokoll für alle Aktionen, die ausgeführt werden, in den Speicher zu speichern Controller.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-204">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="c1c5c-205">Klicken Sie auf eines der **Genres** aus dem Menü und einige Aktionen vorhanden ist, wie z. B. Browsen verfügbaren Album ausführen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-205">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="c1c5c-206">Navigieren Sie zu **/ActionLog** und wenn das Protokoll leer drücken ist **F5** auf der Seite zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-206">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="c1c5c-207">Überprüfen Sie, dass Ihre Besuche nachverfolgt wurden:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-207">Check that your visits were tracked:</span></span>

    <span data-ttu-id="c1c5c-208">![Aktionsprotokoll mit Aktivitäten protokolliert](aspnet-mvc-4-custom-action-filters/_static/image4.png "Aktionsprotokoll mit Aktivitäten protokolliert")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-208">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c1c5c-209">*Aktionsprotokoll mit Aktivitäten protokolliert*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-209">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="c1c5c-210">Übung 2: Verwalten mehrerer Aktionsfilter</span><span class="sxs-lookup"><span data-stu-id="c1c5c-210">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="c1c5c-211">In dieser Übung werden fügen Sie eine zweite Custom Action-Filter auf die Klasse StoreController und definieren die Reihenfolge, in der beide Filter ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-211">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="c1c5c-212">Klicken Sie dann aktualisieren Sie den Code, um den Filter global zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-212">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="c1c5c-213">Sie haben verschiedene Optionen, die beim Definieren der Filterliste Ausführungsreihenfolge berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-213">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="c1c5c-214">Z. B. die Order-Eigenschaft und die Filter Bereich:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-214">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="c1c5c-215">Sie können definieren, eine **Bereich** für jeden dieser Filter, z. B. Sie konnte den Bereich der Aktionsfilter für die Ausführung innerhalb der **Controller Bereich**, und alle Autorisierungsfilter für die Ausführung im **globalen Gültigkeitsbereich** .</span><span class="sxs-lookup"><span data-stu-id="c1c5c-215">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="c1c5c-216">Die Bereiche haben eine definierte Ausführungsreihenfolge.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-216">The scopes have a defined execution order.</span></span>

<span data-ttu-id="c1c5c-217">Jeder Aktionsfilter verfügt darüber hinaus eine Order-Eigenschaft, die verwendet wird, um zu bestimmen, die Ausführungsreihenfolge im Bereich des Filters.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-217">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="c1c5c-218">Weitere Informationen zu Ausführungsreihenfolge benutzerdefinierte Aktionsfilter verwendet werden, finden Sie auf diese MSDN-Artikel: ([https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="c1c5c-218">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/en-us/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="c1c5c-219">Aufgabe 1: Erstellen eines neuen benutzerdefinierten Aktionsfilters</span><span class="sxs-lookup"><span data-stu-id="c1c5c-219">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="c1c5c-220">In dieser Aufgabe erstellen Sie eine neue benutzerdefinierte Aktionsfilter in der Klasse StoreController einfügen lernen, wie die Ausführungsreihenfolge der Filter zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-220">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="c1c5c-221">Öffnen der **beginnen** Lösung finden Sie unter **\Source\Ex02-ManagingMultipleActionFilters\Begin** Ordner.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-221">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="c1c5c-222">Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="c1c5c-223">Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c1c5c-224">Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="c1c5c-225">In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="c1c5c-226">Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="c1c5c-227">Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c1c5c-228">Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c1c5c-229">Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="c1c5c-230">Weitere Informationen finden Sie im Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="c1c5c-230">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="c1c5c-231">Fügen Sie eine neue C#-Klasse in der **Filter** Ordner und nennen Sie sie *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-231">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="c1c5c-232">Open **MyNewCustomActionFilter.cs** und Hinzufügen eines Verweises auf **System.Web.Mvc** und **MvcMusicStore.Models** Namespace:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-232">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="c1c5c-233">(Codeausschnitt - *benutzerdefinierte Aktionsfilter ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="c1c5c-233">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="c1c5c-234">Ersetzen Sie die Standard-Klassendeklaration durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-234">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="c1c5c-235">(Codeausschnitt - *benutzerdefinierte Aktionsfilter ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="c1c5c-235">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="c1c5c-236">Diese benutzerdefinierte Aktionsfilter entspricht fast dem als die Version, die Sie in der vorherigen Übung erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-236">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="c1c5c-237">Der Hauptunterschied besteht darin, dass sie hat die  *&quot;protokolliert von&quot;*  Attribut aktualisiert, die durch diese neue Art Namen zum Identifizieren des Filters eingetragen werden sollen, registriert das Protokoll.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-237">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="c1c5c-238">Aufgabe 2: Räumen einen neuen Code Interceptor in die StoreController-Klasse</span><span class="sxs-lookup"><span data-stu-id="c1c5c-238">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="c1c5c-239">In dieser Aufgabe erstellen Sie fügen einen neuen benutzerdefinierten Filter in die Klasse StoreController und führen Sie die Projektmappe, um zu überprüfen, wie beide Filter zusammenarbeiten.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-239">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="c1c5c-240">Öffnen der **StoreController** Klasse finden Sie unter **MvcMusicStore\Controllers** und Einfügen der neuen benutzerdefinierten Filter **MyNewCustomActionFilter** in  **StoreController** -Klasse wie im folgenden Code gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-240">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="c1c5c-241">Führen Sie nun die Anwendung aus, um zu sehen, wie diese zwei benutzerdefinierte Aktionsfilter funktionieren.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-241">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="c1c5c-242">Drücken Sie die zu diesem Zweck **F5** und warten Sie, bis die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-242">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="c1c5c-243">Navigieren Sie zu **/ActionLog** zum ersten Protokoll Ansichtszustand finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-243">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="c1c5c-244">![Protokollstatus Tracker vor der Aktivität "Seite"](aspnet-mvc-4-custom-action-filters/_static/image5.png "Tracker Protokollstatus vor der Aktivität "Seite"")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-244">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="c1c5c-245">*Protokoll Tracker Status vor der Aktivität "Seite"*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-245">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="c1c5c-246">Klicken Sie auf eines der **Genres** aus dem Menü und einige Aktionen vorhanden ist, wie z. B. Browsen verfügbaren Album ausführen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-246">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="c1c5c-247">Überprüfen Sie, ob dieses Mal; Ihre Besuche zweimal nachverfolgt wurden: für jede der benutzerdefinierten Aktionsfilter im hinzugefügten der **StorageController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-247">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="c1c5c-248">![Aktionsprotokoll mit Aktivitäten protokolliert](aspnet-mvc-4-custom-action-filters/_static/image6.png "Aktionsprotokoll mit Aktivitäten protokolliert")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-248">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c1c5c-249">*Aktionsprotokoll mit Aktivitäten protokolliert*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-249">*Action log with activity logged*</span></span>
6. <span data-ttu-id="c1c5c-250">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-250">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="c1c5c-251">Aufgabe 3: Verwalten von Filterreihenfolge</span><span class="sxs-lookup"><span data-stu-id="c1c5c-251">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="c1c5c-252">In dieser Aufgabe erfahren Sie, wie die Filter Ausführungsreihenfolge zu verwalten, indem Sie die Order-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-252">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="c1c5c-253">Öffnen der **StoreController** Klasse finden Sie unter **MvcMusicStore\Controllers** , und geben Sie die **Reihenfolge** Eigenschaft in beide Filter wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-253">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="c1c5c-254">Überprüfen Sie nun, wie die Filter ausgeführt werden, je nach Wert für die Order-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-254">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="c1c5c-255">Sie finden, die den Filter mit dem kleinsten Wert der Bestellung (**CustomActionFilter**) ist das erste Schema an, die ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-255">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="c1c5c-256">Drücken Sie **F5** und warten Sie, bis die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-256">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="c1c5c-257">Navigieren Sie zu **/ActionLog** zum ersten Protokoll Ansichtszustand finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-257">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="c1c5c-258">![Protokollstatus Tracker vor der Aktivität "Seite"](aspnet-mvc-4-custom-action-filters/_static/image7.png "Tracker Protokollstatus vor der Aktivität "Seite"")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-258">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="c1c5c-259">*Protokoll Tracker Status vor der Aktivität "Seite"*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-259">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="c1c5c-260">Klicken Sie auf eines der **Genres** aus dem Menü und einige Aktionen vorhanden ist, wie z. B. Browsen verfügbaren Album ausführen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-260">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="c1c5c-261">Überprüfen Sie, dass dieses Mal Ihre Besuche nachverfolgt wurden nach der Filterliste Reihenfolgenwert sortiert: **CustomActionFilter** Protokolle erste.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-261">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="c1c5c-262">![Aktionsprotokoll mit Aktivitäten protokolliert](aspnet-mvc-4-custom-action-filters/_static/image8.png "Aktionsprotokoll mit Aktivitäten protokolliert")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-262">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c1c5c-263">*Aktionsprotokoll mit Aktivitäten protokolliert*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-263">*Action log with activity logged*</span></span>
6. <span data-ttu-id="c1c5c-264">Jetzt, Sie Reihenfolgenwert der Filterliste aktualisieren und überprüfen Sie, wie sich die Reihenfolge der Protokollierung ändert.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-264">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="c1c5c-265">In der **StoreController** Klasse, zum Aktualisieren der Filterliste Reihenfolgenwert wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-265">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="c1c5c-266">Führen Sie die Anwendung erneut durch Drücken von **F5**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-266">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="c1c5c-267">Klicken Sie auf eines der **Genres** aus dem Menü und einige Aktionen vorhanden ist, wie z. B. Browsen verfügbaren Album ausführen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-267">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="c1c5c-268">Überprüfen Sie, dass dieses Mal erstellt die Protokolle mit **MyNewCustomActionFilter** Filter wird zuerst angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-268">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="c1c5c-269">![Aktionsprotokoll mit Aktivitäten protokolliert](aspnet-mvc-4-custom-action-filters/_static/image9.png "Aktionsprotokoll mit Aktivitäten protokolliert")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-269">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c1c5c-270">*Aktionsprotokoll mit Aktivitäten protokolliert*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-270">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="c1c5c-271">Aufgabe 4: Registrieren von Global filtert</span><span class="sxs-lookup"><span data-stu-id="c1c5c-271">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="c1c5c-272">In dieser Aufgabe aktualisieren Sie die Lösung, um den neuen Filter zu registrieren (**MyNewCustomActionFilter**) als globaler Filter.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-272">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="c1c5c-273">Dadurch wird es durch alle die Aktionen ausgeführt, in der Anwendung und nicht nur auf die StoreController diejenigen wie in der vorherigen Aufgabe ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-273">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="c1c5c-274">In **StoreController** -Klasse, entfernen Sie **[MyNewCustomActionFilter]** Attribut und der Order-Eigenschaft von **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-274">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="c1c5c-275">Es sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-275">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="c1c5c-276">Open **"Global.asax"** Datei, und suchen Sie die **Anwendung\_starten** Methode.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-276">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="c1c5c-277">Beachten Sie, dass jede Thime die Anwendung starten wird der globalen Filter registrieren, durch den Aufruf **RegisterGlobalFilters** Methode innerhalb **FilterConfig** Klasse.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-277">Notice that each thime the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="c1c5c-278">![Globale Filter in Global.asax registriert](aspnet-mvc-4-custom-action-filters/_static/image10.png "globale Filter in Global.asax registriert")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-278">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="c1c5c-279">*Globale Filter registriert in Global.asax*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-279">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="c1c5c-280">Open **FilterConfig.cs** innerhalb **App\_starten** Ordner.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-280">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="c1c5c-281">Fügen Sie einen Verweis auf System.Web.Mvc verwenden; Verwenden MvcMusicStore.Filters; Namespace.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-281">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="c1c5c-282">Update **RegisterGlobalFilters** Methode des benutzerdefinierten Filters hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-282">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="c1c5c-283">Zu diesem Zweck fügen Sie den hervorgehobenen Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-283">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="c1c5c-284">Führen Sie die Anwendung durch Drücken von **F5**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-284">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="c1c5c-285">Klicken Sie auf eines der **Genres** aus dem Menü und einige Aktionen vorhanden ist, wie z. B. Browsen verfügbaren Album ausführen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-285">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="c1c5c-286">Überprüfen Sie, die nun **[MyNewCustomActionFilter]** wird in HomeController und ActionLogController zu eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-286">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="c1c5c-287">![Aktionsprotokoll mit Aktivitäten protokolliert](aspnet-mvc-4-custom-action-filters/_static/image11.png "Aktionsprotokoll mit Aktivitäten protokolliert")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-287">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="c1c5c-288">*Aktionsprotokoll mit globalen Aktivitäten protokolliert*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-288">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="c1c5c-289">Darüber hinaus können Sie die Bereitstellung dieser Anwendung, die Windows Azure-Websites folgenden [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="c1c5c-289">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c1c5c-290">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="c1c5c-290">Summary</span></span>

<span data-ttu-id="c1c5c-291">Durch diese praktische Übungseinheit haben Sie gelernt erweitert einen Aktionsfilter, um benutzerdefinierte Aktionen auszuführen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-291">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="c1c5c-292">Sie haben auch gelernt, wie ein beliebiger anzuwendender Filter auf Ihren Domänencontrollern Seite einfügen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-292">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="c1c5c-293">Es wurden die folgenden Konzepte verwendet:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-293">The following concepts were used:</span></span>

- <span data-ttu-id="c1c5c-294">Vorgehensweise: Erstellen von benutzerdefinierten Aktionsfiltern mit der ASP.NET MVC ActionFilterAttribute-Klasse</span><span class="sxs-lookup"><span data-stu-id="c1c5c-294">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="c1c5c-295">Gewusst wie: Einfügen von Filtern in ASP.NET MVC-Controller.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-295">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="c1c5c-296">Verwalten von Filtern Sortierung mit der Order-Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="c1c5c-296">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="c1c5c-297">Vorgehensweise beim Registrieren von Global filtern</span><span class="sxs-lookup"><span data-stu-id="c1c5c-297">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c1c5c-298">Anhang A: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="c1c5c-298">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c1c5c-299">Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="c1c5c-299">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c1c5c-300">Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-300">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c1c5c-301">Wechseln Sie zu [ [Https://go.microsoft.com/? Linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="c1c5c-301">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c1c5c-302">Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; *Visual Studio Express 2012 für das Web mit Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-302">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="c1c5c-303">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-303">Click on **Install Now**.</span></span> <span data-ttu-id="c1c5c-304">Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-304">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c1c5c-305">Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-305">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c1c5c-306">![Visual Studio Express installieren](aspnet-mvc-4-custom-action-filters/_static/image12.png "Visual Studio Express installieren")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-306">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c1c5c-307">*Installieren Sie Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-307">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c1c5c-308">Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-308">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="c1c5c-310">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-310">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c1c5c-311">Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-311">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="c1c5c-313">*Installationsstatus*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-313">*Installation progress*</span></span>
6. <span data-ttu-id="c1c5c-314">Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-314">When the installation completes, click **Finish**.</span></span>

    ![Installation wurde abgeschlossen](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="c1c5c-316">*Installation wurde abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-316">*Installation completed*</span></span>
7. <span data-ttu-id="c1c5c-317">Klicken Sie auf **beenden** Webplattform-Installer zu schließen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-317">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c1c5c-318">Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-318">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="c1c5c-320">*Visual Studio Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-320">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c1c5c-321">Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy</span><span class="sxs-lookup"><span data-stu-id="c1c5c-321">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="c1c5c-322">In diesem Anhang wird gezeigt, wie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und veröffentlichen Sie die Anwendung, die Sie erhalten haben anhand der testumgebung profitieren von der Web Deploy Veröffentlichungsfunktion von Windows Azure bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-322">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="c1c5c-323">Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="c1c5c-323">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="c1c5c-324">Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-324">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c1c5c-325">Sie können mit Windows Azure hosten 10 ASP.NET-Websites kostenlos und skalieren Sie, wenn Ihr Datenverkehr zunimmt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-325">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="c1c5c-326">Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="c1c5c-326">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="c1c5c-327">![Melden Sie sich bei Windows Azure-Portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "melden Sie sich bei Windows Azure-Portal")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-327">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="c1c5c-328">*Melden Sie sich bei Windows Azure-Verwaltungsportal*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-328">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="c1c5c-329">Klicken Sie auf **neu** in der Befehlszeile.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-329">Click **New** on the command bar.</span></span>

    <span data-ttu-id="c1c5c-330">![Erstellen einer neuen Website](aspnet-mvc-4-custom-action-filters/_static/image18.png "Erstellen einer neuen Website")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-330">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="c1c5c-331">*Erstellen einer neuen Website*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-331">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="c1c5c-332">Klicken Sie auf **berechnen** | **Website**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-332">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="c1c5c-333">Wählen Sie dann **Schnellerfassung** Option.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-333">Then select **Quick Create** option.</span></span> <span data-ttu-id="c1c5c-334">Verfügbare URL für die neue Website bereitstellen, und klicken Sie auf **Website erstellen**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-334">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c1c5c-335">Eine Windows Azure-Website ist der Host für eine Webanwendung, die ausgeführt wird, in der Cloud, die Sie steuern und verwalten können.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-335">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="c1c5c-336">Die Option Schnellerfassung können Sie eine vollständige Webanwendung, die Windows Azure-Website von außerhalb des Portals bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-336">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="c1c5c-337">Es umfasst nicht die Schritte zum Einrichten einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-337">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="c1c5c-338">![Erstellen eine neue Website, die mit der Schnellerfassung](aspnet-mvc-4-custom-action-filters/_static/image19.png "erstellen eine neue Website, die mit der Schnellerfassung")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-338">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="c1c5c-339">*Erstellen eine neue Website, die mit der Schnellerfassung*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-339">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="c1c5c-340">Warten Sie, bis die neue **Website** wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-340">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="c1c5c-341">Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-341">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="c1c5c-342">Überprüfen Sie, dass die neue Website funktioniert.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-342">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="c1c5c-343">![Um die neue Website durchsuchen](aspnet-mvc-4-custom-action-filters/_static/image20.png "durchsuchen, um die neue Website")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-343">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="c1c5c-344">*Um die neue Website durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-344">*Browsing to the new web site*</span></span>

    <span data-ttu-id="c1c5c-345">![Website](aspnet-mvc-4-custom-action-filters/_static/image21.png "Website ausgeführt wird")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-345">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="c1c5c-346">*Die Website ausgeführt wird*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-346">*Web site running*</span></span>
6. <span data-ttu-id="c1c5c-347">Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-347">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="c1c5c-348">![Öffnen die Website-Verwaltungsseiten](aspnet-mvc-4-custom-action-filters/_static/image22.png "öffnen die Website-Verwaltungsseiten")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-348">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="c1c5c-349">*Öffnen die Website-Verwaltungsseiten*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-349">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="c1c5c-350">In der **Dashboard** Seite der **kurzer Blick** auf die **Herunterladen eines Veröffentlichungsprofils** Link.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-350">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c1c5c-351">Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-351">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="c1c5c-352">Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die erforderlich sind, eine Verbindung herstellen und die Authentifizierung für alle Endpunkte für die eine Veröffentlichungsmethode aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-352">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="c1c5c-353">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung beim Lesen von veröffentlichungsprofilen zum Automatisieren der Konfiguration dieser Programme für Veröffentlichung von Webanwendungen auf Windows Azure-Websites.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-353">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="c1c5c-354">![Herunterladen der Website-Veröffentlichungsprofil](aspnet-mvc-4-custom-action-filters/_static/image23.png "der Website herunterladen eines Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-354">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="c1c5c-355">*Die Website herunterladen eines Veröffentlichungsprofils*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-355">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="c1c5c-356">Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort ein.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-356">Download the publish profile file to a known location.</span></span> <span data-ttu-id="c1c5c-357">In dieser Übung wird weiter Gewusst wie: Verwenden Sie diese Datei zum Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-357">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="c1c5c-358">![Speichern das Veröffentlichungsprofil](aspnet-mvc-4-custom-action-filters/_static/image24.png "speichern das Veröffentlichungsprofil")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-358">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="c1c5c-359">*Speichern das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-359">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="c1c5c-360">Aufgabe 2: konfigurieren den Datenbankserver</span><span class="sxs-lookup"><span data-stu-id="c1c5c-360">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="c1c5c-361">Wenn die Anwendung durchführt, verwenden Sie SQL Server Datenbanken Sie einen SQL-Datenbankserver erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-361">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="c1c5c-362">Wenn eine einfache Anwendung bereitstellen, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-362">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="c1c5c-363">Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-363">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="c1c5c-364">Sehen Sie die SQL-Datenbankservern über Ihr Abonnement in Windows Azure-Verwaltungsportal am **Sql-Datenbanken** | **Server** | **des Servers Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-364">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="c1c5c-365">Wenn Sie keinen Server erstellt haben, können Sie erstellen, mit der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-365">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="c1c5c-366">Notieren Sie die **Servername und -URL, Administrator-Benutzername und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-366">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="c1c5c-367">Erstellen Sie die Datenbank nicht noch, wie er in einer späteren Phase erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-367">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="c1c5c-368">![SQL-Datenbank-Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL-Datenbank-Dashboard")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-368">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="c1c5c-369">*SQL-Datenbank-Dashboard*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-369">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="c1c5c-370">In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, aus Visual Studio aus diesem Grund Sie die lokale IP-Adresse in der Serverliste des müssen **zulässigen IP-Adressen**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-370">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="c1c5c-371">Klicken Sie hierzu auf **konfigurieren**, wählen Sie die IP-Adresse von **aktuelle Client-IP-Adresse** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-371">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Client-IP-Adresse hinzufügen](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="c1c5c-373">*Client-IP-Adresse hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-373">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="c1c5c-374">Einmal die **IP-Clientadresse** wird zu den zulässigen IP-Adressen hinzugefügt Liste, klicken Sie auf **speichern** um die Änderungen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-374">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Bestätigen von Änderungen](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="c1c5c-376">*Bestätigen von Änderungen*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-376">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c1c5c-377">Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy</span><span class="sxs-lookup"><span data-stu-id="c1c5c-377">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="c1c5c-378">Wechseln Sie zurück, die ASP.NET MVC 4-Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-378">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="c1c5c-379">In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-379">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="c1c5c-380">![Veröffentlichen der Anwendung](aspnet-mvc-4-custom-action-filters/_static/image29.png "Veröffentlichen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-380">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="c1c5c-381">*Veröffentlichen der Website*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-381">*Publishing the web site*</span></span>
2. <span data-ttu-id="c1c5c-382">Importieren Sie das Veröffentlichungsprofil, das Sie in der ersten Aufgabe gespeichert.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-382">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="c1c5c-383">![Importieren des Veröffentlichungsprofils](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importieren des Veröffentlichungsprofils")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-383">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="c1c5c-384">*Importieren Sie das Veröffentlichungsprofil*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-384">*Importing publish profile*</span></span>
3. <span data-ttu-id="c1c5c-385">Klicken Sie auf **überprüft, ob Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-385">Click **Validate Connection**.</span></span> <span data-ttu-id="c1c5c-386">Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-386">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c1c5c-387">Überprüfung ist beendet, sobald Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Validate Connection" angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-387">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="c1c5c-388">![Überprüfen der Verbindung](aspnet-mvc-4-custom-action-filters/_static/image31.png "Überprüfen der Verbindung")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-388">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="c1c5c-389">*Überprüfen der Verbindung*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-389">*Validating connection*</span></span>
4. <span data-ttu-id="c1c5c-390">In der **Einstellungen** Seite der **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="c1c5c-390">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="c1c5c-391">![Web deploy-Konfiguration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy-Konfiguration")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-391">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="c1c5c-392">*Web deploy-Konfiguration*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-392">*Web deploy configuration*</span></span>
5. <span data-ttu-id="c1c5c-393">Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:</span><span class="sxs-lookup"><span data-stu-id="c1c5c-393">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="c1c5c-394">In der **Servernamen** Geben Sie Ihre SQL-Datenbank Server-URL unter Verwendung der *Tcp:* Präfix.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-394">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="c1c5c-395">In **Benutzername** Geben Sie Ihre Administrator Serveranmeldenamen an.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-395">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="c1c5c-396">In **Kennwort** Geben Sie Ihre Server-Administratorkennwort.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-396">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="c1c5c-397">Geben Sie einen neuen Datenbanknamen ein.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-397">Type a new database name.</span></span>

    <span data-ttu-id="c1c5c-398">![Konfigurieren von Zielverbindungszeichenfolge](aspnet-mvc-4-custom-action-filters/_static/image33.png "Zielverbindungszeichenfolge konfigurieren")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-398">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="c1c5c-399">*Konfigurieren von Ziel-Verbindungszeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-399">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="c1c5c-400">Klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-400">Then click **OK**.</span></span> <span data-ttu-id="c1c5c-401">Bei der Aufforderung zum Erstellen des Datenbank auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-401">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="c1c5c-402">![Erstellen der Datenbank](aspnet-mvc-4-custom-action-filters/_static/image34.png "erstellen die Datenbank-Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-402">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="c1c5c-403">*Erstellen der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-403">*Creating the database*</span></span>
7. <span data-ttu-id="c1c5c-404">Die Verbindungszeichenfolge, die Sie verwenden für die Verbindung mit SQL-Datenbank in Windows Azure ist innerhalb der Verbindung standardmäßig Textfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-404">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="c1c5c-405">Klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-405">Then click **Next**.</span></span>

    <span data-ttu-id="c1c5c-406">![Verbindungszeichenfolge zur SQL-Datenbank zeigt](aspnet-mvc-4-custom-action-filters/_static/image35.png "Verbindungszeichenfolge zur SQL-Datenbank zeigt")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-406">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="c1c5c-407">*Verbindungszeichenfolge, die auf der SQL-Datenbank*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-407">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="c1c5c-408">In der **Vorschau** auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-408">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="c1c5c-409">![Veröffentlichen der Webanwendung](aspnet-mvc-4-custom-action-filters/_static/image36.png "Veröffentlichen der Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-409">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="c1c5c-410">*Veröffentlichen der Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-410">*Publishing the web application*</span></span>
9. <span data-ttu-id="c1c5c-411">Sobald der Veröffentlichungsprozess abgeschlossen ist, wird der Standardbrowser veröffentlichten Website geöffnet.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-411">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="c1c5c-412">Anhang C: Verwendung von Codeausschnitten</span><span class="sxs-lookup"><span data-stu-id="c1c5c-412">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="c1c5c-413">Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-413">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="c1c5c-414">Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-414">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="c1c5c-415">![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-custom-action-filters/_static/image37.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-415">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="c1c5c-416">*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-416">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="c1c5c-417">***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***</span><span class="sxs-lookup"><span data-stu-id="c1c5c-417">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="c1c5c-418">Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-418">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="c1c5c-419">Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-419">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="c1c5c-420">Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-420">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="c1c5c-421">Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="c1c5c-421">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="c1c5c-422">Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-422">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="c1c5c-423">![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-custom-action-filters/_static/image38.png "starten Sie den Namen des Ausschnitts eingeben")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-423">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="c1c5c-424">*Starten Sie den Namen des Ausschnitts eingeben*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-424">*Start typing the snippet name*</span></span>

<span data-ttu-id="c1c5c-425">![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](aspnet-mvc-4-custom-action-filters/_static/image39.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-425">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="c1c5c-426">*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-426">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="c1c5c-427">![Drücken Sie erneut die Tab und den Ausschnitt erweitert](aspnet-mvc-4-custom-action-filters/_static/image40.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-427">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="c1c5c-428">*Drücken Sie erneut die Tab und den Ausschnitt erweitert*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-428">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="c1c5c-429">***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-429">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="c1c5c-430">Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-430">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="c1c5c-431">Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-431">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="c1c5c-432">Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="c1c5c-432">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="c1c5c-433">![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-custom-action-filters/_static/image41.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-433">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="c1c5c-434">*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-434">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="c1c5c-435">![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](aspnet-mvc-4-custom-action-filters/_static/image42.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")</span><span class="sxs-lookup"><span data-stu-id="c1c5c-435">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="c1c5c-436">*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*</span><span class="sxs-lookup"><span data-stu-id="c1c5c-436">*Pick the relevant snippet from the list, by clicking on it*</span></span>
