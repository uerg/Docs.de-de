---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Veröffentlichen Sie die App in Azure-Azure App Service | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867813"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="b191f-102">Veröffentlichen Sie die App in Azure-Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b191f-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="b191f-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b191f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b191f-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="b191f-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="b191f-105">Als letzten Schritt veröffentlichen Sie die Anwendung in Azure.</span><span class="sxs-lookup"><span data-stu-id="b191f-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="b191f-106">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="b191f-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="b191f-107">Auf **veröffentlichen** Ruft die **Web veröffentlichen** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="b191f-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="b191f-108">Wenn Sie diese Option aktiviert **Host in der Cloud** bei Erstellung des Projekts, und klicken Sie dann auf die Verbindung und Einstellungen bereits konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="b191f-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="b191f-109">In diesem Fall klicken Sie einfach auf die **Einstellungen** und prüfen Sie &quot;führen Sie Code First-Migrationen&quot;.</span><span class="sxs-lookup"><span data-stu-id="b191f-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="b191f-110">(Wenn Sie eine Überprüfung auf nicht **Host in der Cloud** am Anfang, und führen Sie dann die Schritte in der [nächsten Abschnitt](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="b191f-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="b191f-111">Um die app bereitstellen möchten, klicken Sie auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="b191f-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="b191f-112">Sie können die publishing Status in Anzeigen der **Webaktivität veröffentlichen** Fenster.</span><span class="sxs-lookup"><span data-stu-id="b191f-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="b191f-113">(Aus der **Ansicht** klicken Sie im Menü **Weitere Fenster**, und wählen Sie dann **Webaktivität veröffentlichen**.)</span><span class="sxs-lookup"><span data-stu-id="b191f-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="b191f-114">Beendigung Visual Studio Bereitstellen der app der Standardbrowser automatisch geöffnet wird, um die URL der Website bereitgestellt, und die Anwendung, die Sie erstellt haben, wird nun in der Cloud ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="b191f-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="b191f-115">Die URL in die Adressleiste des Browsers zeigt, dass die Website aus dem Internet geladen wird.</span><span class="sxs-lookup"><span data-stu-id="b191f-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="b191f-116">Bereitstellung in einer neuen Website</span><span class="sxs-lookup"><span data-stu-id="b191f-116">Deploying to a New Website</span></span>

<span data-ttu-id="b191f-117">Wenn Sie nicht überprüfen **Host in der Cloud** beim Erstellen des Projekts können Sie jetzt eine neue Web-app konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="b191f-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="b191f-118">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="b191f-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="b191f-119">Wählen Sie die **Profil** Registerkarte, und klicken Sie auf **Microsoft Azure-Websites**.</span><span class="sxs-lookup"><span data-stu-id="b191f-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="b191f-120">Wenn Sie derzeit in Azure angemeldet sind, werden Sie aufgefordert, sich anzumelden.</span><span class="sxs-lookup"><span data-stu-id="b191f-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="b191f-121">In der **vorhandene Websites** Dialogfeld klicken Sie auf **neu**.</span><span class="sxs-lookup"><span data-stu-id="b191f-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="b191f-122">Geben Sie einen Standortnamen an.</span><span class="sxs-lookup"><span data-stu-id="b191f-122">Enter a site name.</span></span> <span data-ttu-id="b191f-123">Wählen Sie Ihr Azure-Abonnement und die Region an.</span><span class="sxs-lookup"><span data-stu-id="b191f-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="b191f-124">Unter **Datenbankserver**Option **Erstellen neuer Server**, oder wählen Sie einen vorhandenen Server.</span><span class="sxs-lookup"><span data-stu-id="b191f-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="b191f-125">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="b191f-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="b191f-126">Klicken Sie auf die **Einstellungen** und prüfen Sie &quot;führen Sie Code First-Migrationen&quot;.</span><span class="sxs-lookup"><span data-stu-id="b191f-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="b191f-127">Klicken Sie dann auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="b191f-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b191f-128">Vorherige</span><span class="sxs-lookup"><span data-stu-id="b191f-128">Previous</span></span>](part-9.md)
