---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Erstellen im Kapitel über das Downloads für die EF 5 MVC 4 Lernprogramme | Microsoft Docs
author: Rick-Anderson
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: bda7effabd715e4658d2472e1f0a66d7bba18cab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878515"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="89c9c-103">Erstellen im Kapitel über das Downloads für EF 5 MVC 4-Lernprogramme</span><span class="sxs-lookup"><span data-stu-id="89c9c-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="89c9c-104">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="89c9c-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[<span data-ttu-id="89c9c-105">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="89c9c-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="89c9c-106">Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="89c9c-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="89c9c-107">Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="89c9c-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="89c9c-108">Erstellen die Downloads Kapitel</span><span class="sxs-lookup"><span data-stu-id="89c9c-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="89c9c-109">Herunterladen Sie und Entzippen Sie die Project-Beispiel Zip-Datei.</span><span class="sxs-lookup"><span data-stu-id="89c9c-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="89c9c-110">In das Downloadpaket entpackt finden Sie zusätzliche Zip-Dateien, eine für den Abschluss der einzelnen Kapitel.</span><span class="sxs-lookup"><span data-stu-id="89c9c-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="89c9c-111">Klicken Sie mit der rechten Maustaste auf die gewünschte Zip-Datei, klicken Sie auf **Eigenschaften**, und klicken Sie auf die **zum Aufheben der Sperre** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="89c9c-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="89c9c-112">Entzippen Sie die Datei an.</span><span class="sxs-lookup"><span data-stu-id="89c9c-112">Unzip the file.</span></span>
4. <span data-ttu-id="89c9c-113">Doppelklicken Sie auf die *CUx.sln* Datei zum Starten von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="89c9c-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="89c9c-114">Aus der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager**, klicken Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="89c9c-114">From the **Tools** menu, click **Library Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="89c9c-115">Klicken Sie in der Paket-Manager-Konsole (PMC) auf **wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="89c9c-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="89c9c-116">Beenden Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="89c9c-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="89c9c-117">Visual Studio neu starten, öffnen die Projektmappendatei Sie im vorherigen Schritt geschlossen.</span><span class="sxs-lookup"><span data-stu-id="89c9c-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="89c9c-118">Geben Sie in der Paket-Manager-Konsole (PMC) die `Update-Database` Befehl:</span><span class="sxs-lookup"><span data-stu-id="89c9c-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="89c9c-119">Wenn Sie die folgende Fehlermeldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="89c9c-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="89c9c-120">*Der Begriff 'Update-Database' ist nicht als Namen für ein Cmdlet, Funktion, Skriptdatei oder eines ausführbaren Programms erkannt. Überprüfen Sie die Schreibweise des Namens, oder wenn ein Pfad enthalten ist, stellen Sie sicher, dass der Pfad korrekt ist, und versuchen Sie es erneut.*</span><span class="sxs-lookup"><span data-stu-id="89c9c-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="89c9c-121">Beenden Sie und starten Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="89c9c-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="89c9c-122">Jeder Migration wird ausgeführt, und der Seed-Methode wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="89c9c-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="89c9c-123">Sie können jetzt die app ausführen.</span><span class="sxs-lookup"><span data-stu-id="89c9c-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="89c9c-124">Vorherige</span><span class="sxs-lookup"><span data-stu-id="89c9c-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
