---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: ASP.NET-Gerüstbau in Visual Studio 2013 | Microsoft-Dokumentation
author: tfitzmac
description: ASP.NET-Gerüstbau ist eine neue Funktion, die in Visual Studio 2013 enthalten ist.
ms.author: aspnetcontent
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 2fcfc31069e8fee79eb217ef6bad746f85a085e0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839567"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="bcc7f-103">ASP.NET-Gerüstbau in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bcc7f-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="bcc7f-104">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bcc7f-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="bcc7f-105">ASP.NET-Gerüstbau ist eine neue Funktion, die in Visual Studio 2013 enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="bcc7f-106">Übersicht</span><span class="sxs-lookup"><span data-stu-id="bcc7f-106">Overview</span></span>

<span data-ttu-id="bcc7f-107">ASP.NET-Gerüstbau ist ein Code-Generierung-Framework für Webanwendungen mit ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="bcc7f-108">Visual Studio 2013 umfasst die vorinstallierte Code-Generatoren für MVC und Web-API-Projekte.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="bcc7f-109">Sie hinzufügen Gerüstbau zu Ihrem Projekt, wenn Sie schnell Code hinzufügen, die mit Datenmodellen interagieren möchten.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="bcc7f-110">Mithilfe von Gerüstbau kann die Zeit für die Entwicklung von standard-Datenvorgänge in Ihrem Projekt verringern.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="bcc7f-111">Standardmäßig Visual Studio 2013 unterstützt nicht das Generieren von Code für eine Web Forms-Projekt, aber Sie mit Web Forms Gerüstbau durch Hinzufügen von MVC-Abhängigkeiten auf das Projekt, oder Installieren einer Erweiterung verwenden können.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="bcc7f-112">Beide Ansätze werden unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-112">Both approaches are shown below.</span></span>

<span data-ttu-id="bcc7f-113">Visual Studio 2013 Update 2 (derzeit RC) bietet die Möglichkeit, ASP.NET-Gerüstbau für die Anforderungen Ihres Szenarios zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="bcc7f-114">Mit dieser Funktionalität können Sie eine benutzerdefinierte gerüstvorlage erstellen und Dialogfeld "Neues Gerüst hinzufügen" hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="bcc7f-115">In der angepassten Vorlage Geben Sie den Code, der generiert wird, wenn ein Elements mit Gerüst hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="bcc7f-116">Weitere Informationen finden Sie unter [erstellen eine benutzerdefinierte Gerüstbauer für Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="bcc7f-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bcc7f-117">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="bcc7f-117">Prerequisites</span></span>

<span data-ttu-id="bcc7f-118">Um ASP.NET-Gerüstbau verwenden zu können, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="bcc7f-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="bcc7f-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bcc7f-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="bcc7f-120">Web-Entwicklertools (Teil der Standardinstallation der Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="bcc7f-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="bcc7f-121">ASP.NET Web Frameworks und Tools 2013 (Teil der Standardinstallation der Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="bcc7f-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="bcc7f-122">Hinzufügen eines Elements mit Gerüst zum MVC oder Web-API</span><span class="sxs-lookup"><span data-stu-id="bcc7f-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="bcc7f-123">Um ein Gerüst hinzuzufügen, mit der rechten Maustaste entweder auf das Projekt oder auf einen Ordner innerhalb des Projekts, und wählen **hinzufügen** – **neues Gerüstelement**, wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Element mit Gerüst hinzufügen](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="bcc7f-125">Von der **Gerüst hinzufügen** Fenster, wählen Sie das Gerüst hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Wählen Sie Gerüst](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="bcc7f-127">Die **Controller hinzufügen** Fenster bietet Ihnen die Möglichkeit, wählen Sie Optionen für die Generierung des Controllers, z. B., ob Sie die neuen asynchronen Features von Entity Framework 6 verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Controller hinzufügen](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="bcc7f-129">Die entsprechenden Klassen und die Seiten werden für Ihr Szenario erstellt.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="bcc7f-130">Die folgende Abbildung zeigt beispielsweise die MVC-Controller und Ansichten, die durch Gerüstbau für eine Modellklasse namens Filme erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Die erstellten Dateien](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="bcc7f-132">Hinzufügen eines Elements mit Gerüst zum Web Forms</span><span class="sxs-lookup"><span data-stu-id="bcc7f-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="bcc7f-133">Um Gerüst hinzuzufügen, das Web Forms-Code generiert, müssen Sie eine Erweiterung zu Visual Studio installieren oder MVC-Abhängigkeiten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="bcc7f-134">Beide Ansätze werden unten angezeigt, jedoch müssen Sie nur eine der folgenden Ansätze ausführen.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="bcc7f-135">Web Forms-Gerüst-Erweiterung</span><span class="sxs-lookup"><span data-stu-id="bcc7f-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="bcc7f-136">Sie können Visual Studio-Erweiterung, mit denen Sie Gerüstbau für Web Forms-Projekten verwenden können, installieren.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="bcc7f-137">Wählen Sie in Visual Studio **Tools** und dann **Erweiterungen und Updates**.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="bcc7f-138">Suchen Sie in diesem Dialogfeld der Visual Studio Gallery für **Web Forms Scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![Installieren von Web Forms-Gerüst](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="bcc7f-140">Weitere Informationen finden Sie unter [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="bcc7f-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="bcc7f-141">MVC-Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="bcc7f-141">MVC Dependencies</span></span>

<span data-ttu-id="bcc7f-142">Wählen Sie zum Hinzufügen der MVC-Abhängigkeiten **hinzufügen** - **neues Gerüstelement**.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="bcc7f-143">Wählen Sie im Fenster "Gerüst hinzufügen" **MVC-Abhängigkeiten**, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![MVC-Abhängigkeiten hinzufügen](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="bcc7f-145">Es gibt zwei Optionen für den Gerüstbau für MVC. Minimaler und vollständiger.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="bcc7f-146">Wenn Sie die minimale auswählen, werden nur die NuGet-Pakete und Verweise für ASP.NET MVC zu Ihrem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="bcc7f-147">Bei Auswahl der Option "vollständig", die mindestens erforderliche Abhängigkeiten hinzugefügt werden, sowie die erforderlichen Inhaltsdateien für ein MVC-Projekt.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="bcc7f-148">Um Gerüstbau problemlos verwenden zu können, wählen Sie die vollständige Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-148">To easily use scaffolding, select Full dependencies.</span></span>

![Wählen Sie die vollständige Abhängigkeiten](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="bcc7f-150">Nach dem Hinzufügen von Abhängigkeiten, wird Ihnen eine **"Readme.txt"** Datei.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="bcc7f-151">Sorgfältig befolgen Sie die Anweisungen in dieser Datei, um sicherzustellen, dass das Projekt ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="bcc7f-152">Wenn Sie die Schritte in der Datei "Readme.txt" abgeschlossen haben, können Sie ein neues gerüstelement hinzufügen, wie im vorherigen Abschnitt über MVC und Web-API dargestellt.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="bcc7f-153">Die automatisch generierte Ansichten und Controller funktionieren ordnungsgemäß in Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="bcc7f-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="bcc7f-154">Tutorials</span><span class="sxs-lookup"><span data-stu-id="bcc7f-154">Tutorials</span></span>

<span data-ttu-id="bcc7f-155">Um eine benutzerdefinierte gerüstbauer erstellen zu können, finden Sie unter [erstellen eine benutzerdefinierte Gerüstbauer für Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="bcc7f-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="bcc7f-156">Informationen zum Anpassen der generierten Dateien finden Sie unter [Gewusst wie: Anpassen der generierten Dateien über das Dialogfeld "Neues Gerüstelement"](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="bcc7f-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="bcc7f-157">Ein Beispiel für die mithilfe von Gerüstbau mit **Database First-Entwicklung**, finden Sie unter [EF Database First mit ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="bcc7f-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="bcc7f-158">Ein Beispiel für die mithilfe von Gerüstbau in einer **MVC** Projekt, finden Sie unter [erste Schritte mit ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="bcc7f-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="bcc7f-159">Ein Beispiel für die mithilfe von Gerüstbau in einer **Web-API-** Projekt, finden Sie unter [erstellen Sie eine REST-API mit Attributrouting in der Web-API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="bcc7f-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
