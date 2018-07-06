---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Veröffentlichen Sie die App in Azure, Azure App Service | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 66dde7b54ce084eed873afae56fd686d0dc8795f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808226"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="5e53b-102">Veröffentlichen Sie die App in Azure, Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5e53b-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="5e53b-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5e53b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="5e53b-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="5e53b-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="5e53b-105">Als letzten Schritt veröffentlichen Sie die Anwendung in Azure.</span><span class="sxs-lookup"><span data-stu-id="5e53b-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="5e53b-106">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="5e53b-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="5e53b-107">Auf **veröffentlichen** Ruft die **Webveröffentlichung** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="5e53b-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="5e53b-108">Wenn Sie aktiviert **Host in der Cloud** Wenn Sie zuerst das Projekt, und klicken Sie dann auf die Verbindung erstellt haben und die Einstellungen bereits konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="5e53b-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="5e53b-109">In diesem Fall, klicken Sie einfach auf die **Einstellungen** Registerkarte, und überprüfen Sie &quot;Execute Code First Migrations&quot;.</span><span class="sxs-lookup"><span data-stu-id="5e53b-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="5e53b-110">(Wenn Sie eine Überprüfung auf nicht **Host in der Cloud** am Anfang, und befolgen Sie dann die Schritte in der [nächsten Abschnitt](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="5e53b-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="5e53b-111">Klicken Sie zum Bereitstellen der app auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="5e53b-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="5e53b-112">Sehen Sie den veröffentlichungsfortschritt in die **Webveröffentlichungsaktivität** Fenster.</span><span class="sxs-lookup"><span data-stu-id="5e53b-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="5e53b-113">(Aus der **Ansicht** , wählen Sie im Menü **Other Windows**, und wählen Sie dann **Webveröffentlichungsaktivität**.)</span><span class="sxs-lookup"><span data-stu-id="5e53b-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="5e53b-114">Wenn Visual Studio abgeschlossen ist, das Bereitstellen der app, an die URL der bereitgestellten Website automatisch im Standardbrowser geöffnet wird, und die Anwendung, die Sie erstellt haben, wird jetzt in der Cloud ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="5e53b-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="5e53b-115">Die URL in die Adressleiste des Browsers zeigt, dass die Website aus dem Internet geladen wird.</span><span class="sxs-lookup"><span data-stu-id="5e53b-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="5e53b-116">Bereitstellung in einer neuen Website</span><span class="sxs-lookup"><span data-stu-id="5e53b-116">Deploying to a New Website</span></span>

<span data-ttu-id="5e53b-117">Wenn Sie nicht aktiviert haben **Host in der Cloud** , wenn Sie zuerst das Projekt erstellt haben, können Sie eine neue Web-app jetzt konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="5e53b-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="5e53b-118">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="5e53b-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="5e53b-119">Wählen Sie die **Profil** Registerkarte, und klicken Sie auf **Microsoft Azure Websites**.</span><span class="sxs-lookup"><span data-stu-id="5e53b-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="5e53b-120">Wenn Sie sich derzeit in Azure angemeldet sind, werden Sie aufgefordert werden, für die Anmeldung.</span><span class="sxs-lookup"><span data-stu-id="5e53b-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="5e53b-121">In der **vorhandene Websites** Dialogfeld klicken Sie auf **neu**.</span><span class="sxs-lookup"><span data-stu-id="5e53b-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="5e53b-122">Geben Sie einen Standortnamen an.</span><span class="sxs-lookup"><span data-stu-id="5e53b-122">Enter a site name.</span></span> <span data-ttu-id="5e53b-123">Wählen Sie Ihr Azure-Abonnement und die Region an.</span><span class="sxs-lookup"><span data-stu-id="5e53b-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="5e53b-124">Klicken Sie unter **Datenbankserver**Option **neuen Server erstellen**, oder wählen Sie einen vorhandenen Server.</span><span class="sxs-lookup"><span data-stu-id="5e53b-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="5e53b-125">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="5e53b-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="5e53b-126">Klicken Sie auf die **Einstellungen** Registerkarte, und überprüfen Sie &quot;Execute Code First Migrations&quot;.</span><span class="sxs-lookup"><span data-stu-id="5e53b-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="5e53b-127">Klicken Sie dann auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="5e53b-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5e53b-128">Vorherige</span><span class="sxs-lookup"><span data-stu-id="5e53b-128">Previous</span></span>](part-9.md)
