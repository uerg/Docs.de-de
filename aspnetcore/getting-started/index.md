---
title: Erste Schritte mit ASP.NET Core
author: rick-anderson
description: Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core erstellt und ausgeführt wird.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: a6a5023594aec01370143e7d1f35fb45c109122a
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860939"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="a3814-103">Erste Schritte mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a3814-103">Get started with ASP.NET Core</span></span>

<span data-ttu-id="a3814-104">Dieses Dokument beschreibt die Schritte zum Erstellen und Ausführen einer ASP.NET Core-App.</span><span class="sxs-lookup"><span data-stu-id="a3814-104">This document provides steps for creating and running an ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="a3814-105">Installieren Sie [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="a3814-105">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="a3814-106">Erstellen Sie ein ASP.NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="a3814-106">Create an ASP.NET Core project.</span></span> <span data-ttu-id="a3814-107">Öffnen Sie eine Befehlsshell, und geben Sie den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="a3814-107">Open a command shell and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

3. <span data-ttu-id="a3814-108">Vertrauen Sie dem HTTPS-Entwicklungszertifikat:</span><span class="sxs-lookup"><span data-stu-id="a3814-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="a3814-109">Windows</span><span class="sxs-lookup"><span data-stu-id="a3814-109">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="a3814-110">Über den vorherigen Befehl wird der folgende Dialog angezeigt:</span><span class="sxs-lookup"><span data-stu-id="a3814-110">The preceding command displays the following dialog:</span></span>

  ![Dialogfeld „Sicherheitswarnung“](_static/cert.png)

  <span data-ttu-id="a3814-112">Klicken Sie auf **Ja**, wenn Sie zustimmen möchten, dass das Entwicklungszertifikat vertrauenswürdig ist.</span><span class="sxs-lookup"><span data-stu-id="a3814-112">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="a3814-113">macOS</span><span class="sxs-lookup"><span data-stu-id="a3814-113">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="a3814-114">Über den vorherigen Befehl wird die folgende Meldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="a3814-114">The preceding command displays the following message:</span></span>

  <span data-ttu-id="a3814-115">*Trusting the HTTPS development certificate was requested. Wenn das Zertifikat nicht bereits vertrauenswürdig, ist führen wir den folgenden Befehl aus:*  `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="a3814-115">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="a3814-116">\*Dieser Befehl fordert Sie möglicherweise zur Eingabe Ihres Kennworts auf, um das Zertifikat für die Systemkeychain zu installieren.</span><span class="sxs-lookup"><span data-stu-id="a3814-116">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="a3814-117">Kennwort:\*</span><span class="sxs-lookup"><span data-stu-id="a3814-117">Password:\*</span></span>

  <span data-ttu-id="a3814-118">Geben Sie Ihr Kennwort ein, wenn Sie die Vertrauenswürdigkeit des Entwicklungszertifikats bestätigen möchten.</span><span class="sxs-lookup"><span data-stu-id="a3814-118">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="a3814-119">Linux</span><span class="sxs-lookup"><span data-stu-id="a3814-119">Linux</span></span>](#tab/linux)

  <span data-ttu-id="a3814-120">Weitere Informationen zum Bestätigen der Vertrauenswürdigkeit eines HTTPS-Entwicklungszertifikats finden Sie in der Dokumentation zu Ihrer Linux-Distribution.</span><span class="sxs-lookup"><span data-stu-id="a3814-120">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

4. <span data-ttu-id="a3814-121">Führen Sie die App aus:</span><span class="sxs-lookup"><span data-stu-id="a3814-121">Run the app:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

5. <span data-ttu-id="a3814-122">Wechseln Sie zu [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="a3814-122">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="a3814-123">Klicken Sie auf **Accept** (Akzeptieren), um die Datenschutz- und Cookierichtlinie zu akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="a3814-123">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="a3814-124">Diese App bewahrt keine personenbezogenen Informationen auf.</span><span class="sxs-lookup"><span data-stu-id="a3814-124">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="a3814-125">Öffnen Sie *Pages/About.cshtml*, und modifizieren Sie die Seite mit dem folgenden hervorgehobenen Markup:</span><span class="sxs-lookup"><span data-stu-id="a3814-125">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="a3814-126">Navigieren Sie zu [http://localhost:5001/About](http://localhost:5001/About), und überprüfen Sie, ob die Änderungen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="a3814-126">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="a3814-127">Installieren Sie [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="a3814-127">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="a3814-128">Erstellen Sie ein neues ASP.NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="a3814-128">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="a3814-129">Öffnen Sie eine Befehlsshell.</span><span class="sxs-lookup"><span data-stu-id="a3814-129">Open a command shell.</span></span> <span data-ttu-id="a3814-130">Geben Sie folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="a3814-130">Enter the following command:</span></span>

   ```console
   dotnet new razor -o aspnetcoreapp
   ```

3. <span data-ttu-id="a3814-131">Führen Sie die App mit den folgenden Befehlen aus:</span><span class="sxs-lookup"><span data-stu-id="a3814-131">Run the app with the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

4. <span data-ttu-id="a3814-132">Wechseln Sie zu [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="a3814-132">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="a3814-133">Öffnen Sie *Pages/About.cshtml*, und verändern Sie die Seite so, dass sie die Meldung „Hallo Welt!“ anzeigt.</span><span class="sxs-lookup"><span data-stu-id="a3814-133">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="a3814-134">Die Zeit auf dem Server ist @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="a3814-134">The time on the server is @DateTime.Now":</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="a3814-135">Wechseln Sie zu [http://localhost:5000/About](http://localhost:5000/About), und bestätigen Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="a3814-135">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="a3814-136">Installieren Sie den .NET Core **SDK Installer** für SDK 1.0.4 von der .NET Core-Seite [Alle Downloads](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="a3814-136">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="a3814-137">Erstellen Sie einen Ordner für das neue ASP.NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="a3814-137">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="a3814-138">Öffnen Sie eine Befehlsshell.</span><span class="sxs-lookup"><span data-stu-id="a3814-138">Open a command shell.</span></span> <span data-ttu-id="a3814-139">Geben Sie die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="a3814-139">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="a3814-140">Wenn Sie eine höhere Version des SDK auf Ihrem Computer installiert haben, erstellen Sie die Datei *global.json*, und wählen Sie das SDK 1.0.4 aus.</span><span class="sxs-lookup"><span data-stu-id="a3814-140">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="a3814-141">Erstellen Sie ein neues ASP.NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="a3814-141">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="a3814-142">Stellen Sie die Pakete wieder her.</span><span class="sxs-lookup"><span data-stu-id="a3814-142">Restore the packages.</span></span>

   ```console
   dotnet restore
   ```

6. <span data-ttu-id="a3814-143">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="a3814-143">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="a3814-144">Mit dem Befehl [dotnet run](/dotnet/core/tools/dotnet-run) wird die App bei Bedarf zunächst erstellt.</span><span class="sxs-lookup"><span data-stu-id="a3814-144">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="a3814-145">Wechseln Sie zu `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a3814-145">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end
