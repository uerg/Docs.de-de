---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Im Kapitel über das Erstellen von Downloads für die EF 5 MVC 4-Tutorials | Microsoft-Dokumentation
author: Rick-Anderson
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 6b5d10ba9e878908953e999bd1fd44970acf4ca5
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911967"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="acdeb-103">Im Kapitel über das Erstellen von Downloads für die EF 5 MVC 4-Lernprogramme</span><span class="sxs-lookup"><span data-stu-id="acdeb-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="acdeb-104">durch [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="acdeb-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[<span data-ttu-id="acdeb-105">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="acdeb-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="acdeb-106">Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="acdeb-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="acdeb-107">Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="acdeb-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="acdeb-108">Downloads der einzelnen Kapitel</span><span class="sxs-lookup"><span data-stu-id="acdeb-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="acdeb-109">Herunterladen Sie und Entpacken Sie die Projekt-Beispiel-Zip-Datei.</span><span class="sxs-lookup"><span data-stu-id="acdeb-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="acdeb-110">In den entzippten Downloadpaket finden Sie zusätzliche Zip-Dateien, eine für den Abschluss der einzelnen Kapitel.</span><span class="sxs-lookup"><span data-stu-id="acdeb-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="acdeb-111">Klicken Sie mit der rechten Maustaste auf die gewünschte Zip-Datei, klicken Sie auf **Eigenschaften**, und klicken Sie auf die **Unblock** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="acdeb-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="acdeb-112">Entzippen Sie die Datei aus.</span><span class="sxs-lookup"><span data-stu-id="acdeb-112">Unzip the file.</span></span>
4. <span data-ttu-id="acdeb-113">Doppelklicken Sie auf die *CUx.sln* Datei zum Starten von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="acdeb-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="acdeb-114">Aus der **Tools** Menü klicken Sie auf **NuGet Paket-Manager**, dann **Paket-Manager Konsole**.</span><span class="sxs-lookup"><span data-stu-id="acdeb-114">From the **Tools** menu, click **NuGet Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="acdeb-115">In der Paket-Manager-Konsole (PMC), klicken Sie auf **wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="acdeb-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="acdeb-116">Beenden Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="acdeb-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="acdeb-117">Visual Studio neu starten, die Projektmappe öffnen Sie im vorherigen Schritt geschlossen.</span><span class="sxs-lookup"><span data-stu-id="acdeb-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="acdeb-118">In der Paket-Manager-Konsole (PMC), geben Sie die `Update-Database` Befehl:</span><span class="sxs-lookup"><span data-stu-id="acdeb-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="acdeb-119">Wenn Sie den folgenden Fehler erhalten:</span><span class="sxs-lookup"><span data-stu-id="acdeb-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="acdeb-120">*Der Begriff "Update-Database" wird nicht als Namen für ein Cmdlet, Funktion, Skriptdatei oder eines ausführbaren Programms erkannt. Überprüfen Sie die Schreibweise des Namens, oder wenn ein Pfad enthalten ist, stellen Sie sicher, dass der Pfad richtig ist, und versuchen Sie es erneut.*</span><span class="sxs-lookup"><span data-stu-id="acdeb-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="acdeb-121">Beenden und starten Sie Visual Studio neu.</span><span class="sxs-lookup"><span data-stu-id="acdeb-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="acdeb-122">Jede Migration wird ausgeführt, und klicken Sie dann die Seed-Methode wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="acdeb-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="acdeb-123">Sie können nun die app ausführen.</span><span class="sxs-lookup"><span data-stu-id="acdeb-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="acdeb-124">Vorherige</span><span class="sxs-lookup"><span data-stu-id="acdeb-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
