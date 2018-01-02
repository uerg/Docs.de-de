---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Erstellen einen benutzerdefinierte AJAX steuern Toolkit Extendersteuerelement (VB) | Microsoft Docs
author: microsoft
description: "Benutzerdefinierte Extender ermöglichen es Ihnen, anpassen und erweitern die Funktionen der ASP.NET-Steuerelementen, ohne neue Klassen erstellen zu müssen."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3e8fceb3c7570aa1bf085c8e1037736254e74ef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a><span data-ttu-id="70cb2-103">Erstellen eine benutzerdefinierte AJAX-Steuerelement-Toolkit Extendersteuerelement (VB)</span><span class="sxs-lookup"><span data-stu-id="70cb2-103">Creating a Custom AJAX Control Toolkit Control Extender (VB)</span></span>
====================
<span data-ttu-id="70cb2-104">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="70cb2-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="70cb2-105">Benutzerdefinierte Extender ermöglichen es Ihnen, anpassen und erweitern die Funktionen der ASP.NET-Steuerelementen, ohne neue Klassen erstellen zu müssen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="70cb2-106">In diesem Lernprogramm erfahren Sie, wie eine benutzerdefinierte AJAX Control Toolkit Extendersteuerelement erstellen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="70cb2-107">Erstellen wir eine einfache, aber hilfreich, neue Extender, die deaktiviert bzw. aktiviert, wenn Sie Text in ein Textfeld eingeben, wird der Status einer Schaltfläche geändert.</span><span class="sxs-lookup"><span data-stu-id="70cb2-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="70cb2-108">Nach dem Lesen dieses Lernprogramms, werden Sie können das ASP.NET AJAX-Toolkit mit Ihren eigenen-Extender erweitert.</span><span class="sxs-lookup"><span data-stu-id="70cb2-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="70cb2-109">Sie können benutzerdefinierte Steuerelement Extender mit Visual Studio oder Visual Web Developer erstellen (Stellen Sie sicher, dass Sie die neueste Version von Visual Web Developer).</span><span class="sxs-lookup"><span data-stu-id="70cb2-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="70cb2-110">Übersicht über den DisabledButton Extender</span><span class="sxs-lookup"><span data-stu-id="70cb2-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="70cb2-111">Unsere neue Extendersteuerelement heißt den DisabledButton Extender.</span><span class="sxs-lookup"><span data-stu-id="70cb2-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="70cb2-112">Diesen Extender müssen drei Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="70cb2-112">This extender will have three properties:</span></span>

- <span data-ttu-id="70cb2-113">TargetControlID - Textfeld, das das Steuerelement erweitert.</span><span class="sxs-lookup"><span data-stu-id="70cb2-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="70cb2-114">TargetButtonIID - Schaltfläche, mit der aktiviert oder deaktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="70cb2-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="70cb2-115">DisabledText - der Text, der ursprünglich in der Schaltfläche angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="70cb2-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="70cb2-116">Wenn Sie mit der Eingabe beginnen, zeigt die Schaltfläche den Wert der Eigenschaft Schaltflächentext.</span><span class="sxs-lookup"><span data-stu-id="70cb2-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="70cb2-117">Sie verknüpfen die DisabledButton Extender an ein Textfeld und Schaltfläche-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="70cb2-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="70cb2-118">Bevor Sie einen beschreibenden Text eingeben, die Schaltfläche ist deaktiviert, und das Textfeld und einer Schaltfläche wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="70cb2-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

<span data-ttu-id="70cb2-119">([Klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="70cb2-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span></span>


<span data-ttu-id="70cb2-120">Nach dem Text eingeben starten, wird die Schaltfläche aktiviert ist, und das Textfeld und einer Schaltfläche wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="70cb2-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

<span data-ttu-id="70cb2-121">([Klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="70cb2-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span></span>


<span data-ttu-id="70cb2-122">Um unseren Extendersteuerelement erstellen zu können, müssen wir die folgenden drei Dateien zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="70cb2-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="70cb2-123">DisabledButtonExtender.vb – diese Datei ist der serverseitigen Control-Klasse, die Erstellen des Extenders verwalten und ermöglichen es Ihnen, die Eigenschaften zur Entwurfszeit festgelegt.</span><span class="sxs-lookup"><span data-stu-id="70cb2-123">DisabledButtonExtender.vb - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="70cb2-124">Außerdem definiert die Eigenschaften, die für Ihre Extender festgelegt werden können.</span><span class="sxs-lookup"><span data-stu-id="70cb2-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="70cb2-125">Diese Eigenschaften über den Code auch zur Entwurfszeit zugegriffen werden kann und in der Datei DisableButtonBehavior.js definierte Eigenschaften übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="70cb2-126">DisabledButtonBehavior.js – Diese Datei befindet, in dem Sie alle Ihre Client-Skript-Logik hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="70cb2-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="70cb2-127">DisabledButtonDesigner.vb - kann diese Klasse Entwurfszeitfunktionen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-127">DisabledButtonDesigner.vb - This class enables design-time functionality.</span></span> <span data-ttu-id="70cb2-128">Benötigen Sie diese Klasse, wenn das Extendersteuerelement ordnungsgemäß mit dem Visual Studio/Visual Web Developer-Designer ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="70cb2-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="70cb2-129">Daher besteht ein Extendersteuerelement ein serverseitiges Steuerelement, ein Client-Side-Verhalten und einer serverseitigen Designerklasse.</span><span class="sxs-lookup"><span data-stu-id="70cb2-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="70cb2-130">Erfahren Sie, wie Sie alle drei Dateien in den folgenden Abschnitten zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="70cb2-131">Erstellen der benutzerdefinierten Extender Website und das Projekt</span><span class="sxs-lookup"><span data-stu-id="70cb2-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="70cb2-132">Der erste Schritt besteht im Visual Studio/Visual Web Developer eine Klassenbibliotheksprojekt und die Website erstellen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="70cb2-133">Wir ll benutzerdefinierte Extender in das Klassenbibliotheksprojekt erstellen und testen den benutzerdefinierten Extender auf der Website.</span><span class="sxs-lookup"><span data-stu-id="70cb2-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="70cb2-134">Lassen Sie s mit der Website zu starten.</span><span class="sxs-lookup"><span data-stu-id="70cb2-134">Let�s start with the website.</span></span> <span data-ttu-id="70cb2-135">Führen Sie diese Schritte aus, um die Website zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="70cb2-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="70cb2-136">Wählen Sie die Menüoption **Datei, die neue Website**.</span><span class="sxs-lookup"><span data-stu-id="70cb2-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="70cb2-137">Wählen Sie die **ASP.NET-Website** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="70cb2-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="70cb2-138">Benennen Sie die neue Website *Website1*.</span><span class="sxs-lookup"><span data-stu-id="70cb2-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="70cb2-139">Klicken Sie auf die **OK** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="70cb2-139">Click the **OK** button.</span></span>

<span data-ttu-id="70cb2-140">Als Nächstes müssen wir das Klassenbibliotheksprojekt zu erstellen, das den Code für das Extendersteuerelement enthält:</span><span class="sxs-lookup"><span data-stu-id="70cb2-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="70cb2-141">Wählen Sie die Menüoption **Datei hinzufügen, neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="70cb2-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="70cb2-142">Wählen Sie die **-Klassenbibliothek** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="70cb2-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="70cb2-143">Benennen Sie die neue Klassenbibliothek mit dem Namen **CustomExtenders**.</span><span class="sxs-lookup"><span data-stu-id="70cb2-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="70cb2-144">Klicken Sie auf die **OK** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="70cb2-144">Click the **OK** button.</span></span>

<span data-ttu-id="70cb2-145">Nachdem Sie diese Schritte abgeschlossen haben, sieht das Fenster Projektmappen-Explorer wie in Abbildung 1.</span><span class="sxs-lookup"><span data-stu-id="70cb2-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="70cb2-146">[![Lösung mit der Website und die Klasse Library-Projekts](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="70cb2-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span></span>

<span data-ttu-id="70cb2-147">**Abbildung 01**: Projektmappe mit der Website und die Klasse Bibliotheksprojekt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="70cb2-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span></span>


<span data-ttu-id="70cb2-148">Als Nächstes müssen Sie alle auf das Klassenbibliotheksprojekt die notwendigen Assemblyverweise hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="70cb2-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="70cb2-149">Mit der rechten Maustaste des CustomExtenders-Projekts, und wählen Sie die Menüoption **Verweis hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="70cb2-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="70cb2-150">Wählen Sie die Registerkarte ".NET".</span><span class="sxs-lookup"><span data-stu-id="70cb2-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="70cb2-151">Fügen Sie Verweise auf die folgenden Assemblys hinzu:</span><span class="sxs-lookup"><span data-stu-id="70cb2-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="70cb2-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="70cb2-152">System.Web.dll</span></span>
    2. <span data-ttu-id="70cb2-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="70cb2-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="70cb2-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="70cb2-154">System.Design.dll</span></span>
    4. <span data-ttu-id="70cb2-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="70cb2-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="70cb2-156">Wählen Sie die Registerkarte "Durchsuchen".</span><span class="sxs-lookup"><span data-stu-id="70cb2-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="70cb2-157">Fügen Sie einen Verweis auf die AjaxControlToolkit.dll-Assembly.</span><span class="sxs-lookup"><span data-stu-id="70cb2-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="70cb2-158">Diese Assembly befindet sich im Ordner, in dem Sie das AJAX-Steuerelement-Toolkit heruntergeladen haben.</span><span class="sxs-lookup"><span data-stu-id="70cb2-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="70cb2-159">Sie können überprüfen, ob Sie alle richtigen Verweise hinzugefügt haben, indem mit der rechten Maustaste des Projekts, Eigenschaften, und klicken Sie auf der Registerkarte "Verweise" (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="70cb2-159">You can verify that you have added all of the right references by right-clicking your project, selecting Properties, and clicking the References tab (see Figure 2).</span></span>


<span data-ttu-id="70cb2-160">[![Ordner "Verweise" mit der erforderlichen Verweise](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="70cb2-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span></span>

<span data-ttu-id="70cb2-161">**Abbildung 02**: Ordner "Verweise" mit der erforderlichen Verweise ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="70cb2-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="70cb2-162">Erstellen die benutzerdefinierte Extendersteuerelement</span><span class="sxs-lookup"><span data-stu-id="70cb2-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="70cb2-163">Nun, da wir unsere Klassenbibliothek haben, beginnen wir unsere Extendersteuerelement erstellen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="70cb2-164">Lassen Sie s mit der Texten ein benutzerdefiniertes Extender-Steuerelementklasse (Siehe Programmbeispiel 1) beginnen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="70cb2-165">**1 – MyCustomExtender.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="70cb2-165">**Listing 1 - MyCustomExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

<span data-ttu-id="70cb2-166">Es gibt mehrere Dinge, die Sie zu der Extender Steuerelementklasse in Codebeispiel 1 fest.</span><span class="sxs-lookup"><span data-stu-id="70cb2-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="70cb2-167">Erstens ist zu beachten, dass die Klasse von der Basisklasse ExtenderControlBase erbt.</span><span class="sxs-lookup"><span data-stu-id="70cb2-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="70cb2-168">Alle AJAX Control Toolkit Extendersteuerelementen werden von dieser Basisklasse abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="70cb2-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="70cb2-169">Beispielsweise enthält die Basisklasse der TargetID-Eigenschaft, die eine erforderliche Eigenschaft des Extenders wird jedes Steuerelement ist.</span><span class="sxs-lookup"><span data-stu-id="70cb2-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="70cb2-170">Als Nächstes Beachten Sie, dass die Klasse die folgenden zwei Attribute, die im Zusammenhang mit Clientskripts umfasst:</span><span class="sxs-lookup"><span data-stu-id="70cb2-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="70cb2-171">WebResource - bewirkt, dass eine Datei als eingebettete Ressource in einer Assembly enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="70cb2-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="70cb2-172">ClientScriptResource - bewirkt, dass eine Ressource "Script" aus einer Assembly abgerufen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="70cb2-173">Das WebResource-Attribut wird verwendet, um die MyControlBehavior.js JavaScript-Datei in die Assembly eingebettet, wenn der benutzerdefinierte Extender kompiliert wird.</span><span class="sxs-lookup"><span data-stu-id="70cb2-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="70cb2-174">Das ClientScriptResource-Attribut wird verwendet, um das MyControlBehavior.js-Skript aus der Assembly abgerufen werden, wenn der benutzerdefinierte Extender in einer Webseite verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="70cb2-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="70cb2-175">In der Reihenfolge für die Attribute WebResource und ClientScriptResource arbeiten müssen Sie die JavaScript-Datei als eingebettete Ressource kompilieren.</span><span class="sxs-lookup"><span data-stu-id="70cb2-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="70cb2-176">Wählen Sie im Fenster Projektmappen-Explorer die Datei, öffnen Sie die Eigenschaftenseite, und weisen Sie den Wert *eingebettete Ressource* auf die **Buildvorgang** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="70cb2-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="70cb2-177">Beachten Sie, dass der Steuerelement-Extender auch ein TargetControlType-Attribut enthält.</span><span class="sxs-lookup"><span data-stu-id="70cb2-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="70cb2-178">Dieses Attribut wird verwendet, um den Typ des Steuerelements anzugeben, die durch das Extendersteuerelement erweitert wird.</span><span class="sxs-lookup"><span data-stu-id="70cb2-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="70cb2-179">Bei einem Codebeispiel 1 ist das Extendersteuerelement verwendet, um ein Textfeld zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="70cb2-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="70cb2-180">Beachten Sie, dass der benutzerdefinierte Extender eine Eigenschaft "MyProperty" enthält.</span><span class="sxs-lookup"><span data-stu-id="70cb2-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="70cb2-181">Die Eigenschaft ist mit dem ExtenderControlProperty-Attribut markiert.</span><span class="sxs-lookup"><span data-stu-id="70cb2-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="70cb2-182">Die Methoden GetPropertyValue() und SetPropertyValue() werden verwendet, den Wert der Eigenschaft aus der Extender serverseitiges Steuerelement an das Verhalten des clientseitigen übergeben.</span><span class="sxs-lookup"><span data-stu-id="70cb2-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="70cb2-183">Lassen Sie s, fahren Sie fort, und implementieren Sie den Code für unsere DisabledButton Extender.</span><span class="sxs-lookup"><span data-stu-id="70cb2-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="70cb2-184">Der Code für diesen Extender finden Sie im Codebeispiel 2.</span><span class="sxs-lookup"><span data-stu-id="70cb2-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="70cb2-185">**Auflisten von 2 – DisabledButtonExtender.vb**</span><span class="sxs-lookup"><span data-stu-id="70cb2-185">**Listing 2 - DisabledButtonExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

<span data-ttu-id="70cb2-186">Der Extender DisabledButton auflisten 2 verfügt über zwei Eigenschaften, die mit dem Namen TargetButtonID und DisabledText.</span><span class="sxs-lookup"><span data-stu-id="70cb2-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="70cb2-187">Die IDReferenceProperty auf die Eigenschaft TargetButtonID angewendet verhindert, dass Sie etwas anderes als die ID des einem Schaltflächen-Steuerelement diese Eigenschaft zuweisen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="70cb2-188">Die Attribute WebResource und ClientScriptResource zuordnen ein clientseitigen Verhaltens befindet sich in einer Datei namens DisabledButtonBehavior.js mit diesen Extender.</span><span class="sxs-lookup"><span data-stu-id="70cb2-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="70cb2-189">Dieser JavaScript-Datei im nächsten Abschnitt erörtert.</span><span class="sxs-lookup"><span data-stu-id="70cb2-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="70cb2-190">Erstellen das benutzerdefinierte Extender-Verhalten</span><span class="sxs-lookup"><span data-stu-id="70cb2-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="70cb2-191">Die Client-Side-Komponente des ein Extendersteuerelement ist ein Verhalten wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="70cb2-192">Die eigentliche Logik zum Deaktivieren und aktivieren die Schaltfläche befindet sich im DisabledButton Verhalten.</span><span class="sxs-lookup"><span data-stu-id="70cb2-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="70cb2-193">Der JavaScript-Code für das Verhalten ist im Codebeispiel 3 enthalten.</span><span class="sxs-lookup"><span data-stu-id="70cb2-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="70cb2-194">**3 – DisabledButton.js auflisten**</span><span class="sxs-lookup"><span data-stu-id="70cb2-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

<span data-ttu-id="70cb2-195">Die JavaScript-Datei Auflisten von 3 enthält eine Client-Side-Klasse, die mit dem Namen DisabledButtonBehavior.</span><span class="sxs-lookup"><span data-stu-id="70cb2-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="70cb2-196">Diese Klasse wird wie die serverseitige und enthält zwei Eigenschaften, die mit dem Namen TargetButtonID und DisabledText, die Sie zugreifen können, mithilfe von get\_TargetButtonID/Set\_TargetButtonID und\_DisabledText/Set\_ DisabledText.</span><span class="sxs-lookup"><span data-stu-id="70cb2-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="70cb2-197">Die Initialize()-Methode ordnet einen Keyup-Ereignis-Handler für das Verhalten der Target-Element.</span><span class="sxs-lookup"><span data-stu-id="70cb2-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="70cb2-198">Jedes Mal, Sie geben Sie einen Buchstaben in das Textfeld ein, die mit diesem Verhalten verbundenen, führt das Keyup-Handler.</span><span class="sxs-lookup"><span data-stu-id="70cb2-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="70cb2-199">Der Handler Keyup entweder aktiviert oder deaktiviert die Schaltfläche abhängig davon, ob das Textfeld mit dem Verhalten verknüpft einen beschreibenden Text enthält.</span><span class="sxs-lookup"><span data-stu-id="70cb2-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="70cb2-200">Denken Sie daran, dass Sie die JavaScript-Datei auflisten 3 als eingebettete Ressource kompilieren müssen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="70cb2-201">Wählen Sie im Fenster Projektmappen-Explorer die Datei, öffnen Sie die Eigenschaftenseite, und weisen Sie den Wert *eingebettete Ressource* auf die **Buildvorgang** -Eigenschaft (siehe Abbildung 3).</span><span class="sxs-lookup"><span data-stu-id="70cb2-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="70cb2-202">Diese Option ist in Visual Studio und Visual Web Developer verfügbar.</span><span class="sxs-lookup"><span data-stu-id="70cb2-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="70cb2-203">[![Hinzufügen einer JavaScript-Datei als eingebettete Ressource](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="70cb2-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span></span>

<span data-ttu-id="70cb2-204">**Abbildung 03**: Hinzufügen einer JavaScript-Datei als eingebettete Ressource ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="70cb2-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="70cb2-205">Erstellen des benutzerdefinierten Extender-Designers</span><span class="sxs-lookup"><span data-stu-id="70cb2-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="70cb2-206">Es wird eine letzte-Klasse, die wir erstellen, um unsere Extender abschließen müssen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="70cb2-207">Wir müssen die Designerklasse auflisten 4 erstellen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="70cb2-208">Diese Klasse ist erforderlich, um den Extender verhält sich ordnungsgemäß mit dem Visual Studio/Visual Web Developer-Designer.</span><span class="sxs-lookup"><span data-stu-id="70cb2-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="70cb2-209">**4 – DisabledButtonDesigner.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="70cb2-209">**Listing 4 - DisabledButtonDesigner.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

<span data-ttu-id="70cb2-210">Ordnen Sie den Designer in 4 Auflisten der DisabledButton Extender mit dem Designer-Attribut. Sie müssen das Designer-Attribut auf die Klasse DisabledButtonExtender wie folgt anwenden:</span><span class="sxs-lookup"><span data-stu-id="70cb2-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="70cb2-211">Mithilfe des benutzerdefinierten Extenders</span><span class="sxs-lookup"><span data-stu-id="70cb2-211">Using the Custom Extender</span></span>

<span data-ttu-id="70cb2-212">Nun, dass wir das Extendersteuerelement DisabledButton erstellt haben, ist es Zeit für die Verwendung in unserer ASP.NET-Website.</span><span class="sxs-lookup"><span data-stu-id="70cb2-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="70cb2-213">Zunächst müssen wir die benutzerdefinierte Extender zur Toolbox hinzu.</span><span class="sxs-lookup"><span data-stu-id="70cb2-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="70cb2-214">Führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="70cb2-214">Follow these steps:</span></span>

1. <span data-ttu-id="70cb2-215">Öffnen Sie eine ASP.NET-Seite durch Doppelklicken auf die Seite im Projektmappen-Explorer-Fenster.</span><span class="sxs-lookup"><span data-stu-id="70cb2-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="70cb2-216">Mit der rechten Maustaste in der Toolbox, und wählen Sie die Menüoption **Elemente auswählen**.</span><span class="sxs-lookup"><span data-stu-id="70cb2-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="70cb2-217">Navigieren Sie zu der Assembly CustomExtenders.dll, klicken Sie im Dialogfeld "Toolboxelemente auswählen".</span><span class="sxs-lookup"><span data-stu-id="70cb2-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="70cb2-218">Klicken Sie auf die **OK** Schaltfläche, um das Dialogfeld zu schließen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="70cb2-219">Nachdem Sie diese Schritte abgeschlossen haben, sollte die DisabledButton Extendersteuerelement in der Toolbox angezeigt (siehe Abbildung 4).</span><span class="sxs-lookup"><span data-stu-id="70cb2-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="70cb2-220">[![DisabledButton in der toolbox](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="70cb2-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span></span>

<span data-ttu-id="70cb2-221">**Abbildung 04**: DisabledButton in der Toolbox ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="70cb2-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span></span>


<span data-ttu-id="70cb2-222">Als Nächstes müssen wir eine neue ASP.NET-Seite zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="70cb2-223">Führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="70cb2-223">Follow these steps:</span></span>

1. <span data-ttu-id="70cb2-224">Erstellen Sie eine neue, mit dem Namen ShowDisabledButton.aspx ASP.NET-Seite.</span><span class="sxs-lookup"><span data-stu-id="70cb2-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="70cb2-225">Ziehen Sie ein ScriptManager auf der Seite "ein.</span><span class="sxs-lookup"><span data-stu-id="70cb2-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="70cb2-226">Ziehen Sie ein TextBox-Steuerelement auf der Seite "ein.</span><span class="sxs-lookup"><span data-stu-id="70cb2-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="70cb2-227">Ziehen Sie ein Button-Steuerelement auf der Seite "ein.</span><span class="sxs-lookup"><span data-stu-id="70cb2-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="70cb2-228">Ändern Sie im Fenster Eigenschaften die Schaltfläche ID-Eigenschaft auf den Wert *hinzu* und der Text-Eigenschaft auf den Wert *speichern\**.</span><span class="sxs-lookup"><span data-stu-id="70cb2-228">In the Properties window, change the Button ID property to the value *btnSave* and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="70cb2-229">Wir haben eine Seite mit einem Standardsteuerelement ASP.NET Textfeld und Schaltfläche erstellt.</span><span class="sxs-lookup"><span data-stu-id="70cb2-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="70cb2-230">Als Nächstes müssen wir das TextBox-Steuerelement mit dem DisabledButton Extender erweitern:</span><span class="sxs-lookup"><span data-stu-id="70cb2-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="70cb2-231">Wählen Sie die **Extender hinzufügen** Aufgabe Option, um das Dialogfeld Extender-Assistenten zu öffnen (siehe Abbildung 5).</span><span class="sxs-lookup"><span data-stu-id="70cb2-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="70cb2-232">Beachten Sie, dass das Dialogfeld "unsere benutzerdefinierte DisabledButton Extender enthält.</span><span class="sxs-lookup"><span data-stu-id="70cb2-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="70cb2-233">Wählen Sie den DisabledButton Extender, und klicken Sie auf die **OK** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="70cb2-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="70cb2-234">[![Das Dialogfeld Extender-Assistenten](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="70cb2-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span></span>

<span data-ttu-id="70cb2-235">**Abbildung 05**: der Extender-Assistent-Dialogfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="70cb2-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span></span>


<span data-ttu-id="70cb2-236">Schließlich können wir die Eigenschaften des Extenders DisabledButton festlegen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="70cb2-237">Sie können die Eigenschaften des Extenders DisabledButton durch Ändern der Eigenschaften des Textfeld-Steuerelements ändern:</span><span class="sxs-lookup"><span data-stu-id="70cb2-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="70cb2-238">Wählen Sie im Designer das Textfeld ein.</span><span class="sxs-lookup"><span data-stu-id="70cb2-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="70cb2-239">Erweitern Sie im Fenster Eigenschaften die Extender-Knoten (siehe Abbildung 6).</span><span class="sxs-lookup"><span data-stu-id="70cb2-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="70cb2-240">Weisen Sie den Wert *speichern* DisabledText-Eigenschaft und der Wert *hinzu* der TargetButtonID-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="70cb2-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="70cb2-241">[![Einstellungseigenschaften für extender](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="70cb2-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span></span>

<span data-ttu-id="70cb2-242">**Abbildung 06**: Extendereigenschaften festlegen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="70cb2-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span></span>


<span data-ttu-id="70cb2-243">Wenn Sie die Seite "(durch Drücken von F5) ausführen, ist anfangs das Schaltflächen-Steuerelement deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="70cb2-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="70cb2-244">Sobald Sie Text in das Textfeld eingeben starten, aktiviert die Schaltfläche mit dem Steuerelement befindet (siehe Abbildung 7).</span><span class="sxs-lookup"><span data-stu-id="70cb2-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="70cb2-245">[![Der Extender DisabledButton in Aktion](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="70cb2-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span></span>

<span data-ttu-id="70cb2-246">**Abbildung 07**: die DisabledButton Extender in Aktion ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="70cb2-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="70cb2-247">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="70cb2-247">Summary</span></span>

<span data-ttu-id="70cb2-248">Das Ziel dieses Lernprogramms wurde zur Erläuterung, wie Sie das AJAX-Steuerelement-Toolkit mit benutzerdefinierten Extendersteuerelementen erweitern können.</span><span class="sxs-lookup"><span data-stu-id="70cb2-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="70cb2-249">In diesem Lernprogramm haben wir eine einfache DisabledButton Extendersteuerelement erstellt.</span><span class="sxs-lookup"><span data-stu-id="70cb2-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="70cb2-250">Wir implementiert diesen Extender durch Erstellen einer DisabledButtonExtender-Klasse, ein DisabledButtonBehavior JavaScript-Verhalten und einer DisabledButtonDesigner-Klasse.</span><span class="sxs-lookup"><span data-stu-id="70cb2-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="70cb2-251">Sie führen Sie eine ähnliche Reihe von Schritten, wenn Sie einen benutzerdefiniertes Steuerelement Extender erstellen.</span><span class="sxs-lookup"><span data-stu-id="70cb2-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="70cb2-252">Zurück</span><span class="sxs-lookup"><span data-stu-id="70cb2-252">Previous</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
