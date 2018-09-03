---
title: DevOps mit ASP.NET Core und Azure | Tools und downloads
author: CamSoper
description: Ein Leitfaden, der End-to-End-Anleitungen zum Erstellen einer DevOps-Pipeline für eine in Azure gehostete ASP.NET Core-App bereitstellt.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: a63e97d9ab9eb0ed2fbd30e8c2e033f0c048d33e
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312300"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="cc286-103">Tools und downloads</span><span class="sxs-lookup"><span data-stu-id="cc286-103">Tools and downloads</span></span>

<span data-ttu-id="cc286-104">Azure verfügt über mehrere Schnittstellen für die Bereitstellung und Verwaltung von Ressourcen, z. B. die [Azure-Portal](https://portal.azure.com), [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [Azure-Cloud Shell](https://shell.azure.com/bash), und Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cc286-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="cc286-105">Dieses Handbuch verwendet einen minimalistischen Ansatz und die Azure Cloud Shell nach Möglichkeit, um die erforderlichen Schritte zu reduzieren.</span><span class="sxs-lookup"><span data-stu-id="cc286-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="cc286-106">Allerdings muss das Azure-Portal für einige Teile verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cc286-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc286-107">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="cc286-107">Prerequisites</span></span>

<span data-ttu-id="cc286-108">Die folgenden Abonnements sind erforderlich:</span><span class="sxs-lookup"><span data-stu-id="cc286-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="cc286-109">Azure &mdash; , wenn Sie nicht über ein Konto verfügen [erhalten eine kostenlose Testversion](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="cc286-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="cc286-110">Visual Studio Team Services (VSTS) &mdash; dieses Konto wird erstellt, in Kapitel 4.</span><span class="sxs-lookup"><span data-stu-id="cc286-110">Visual Studio Team Services (VSTS) &mdash; This account is created in Chapter 4.</span></span>
* <span data-ttu-id="cc286-111">GitHub &mdash; , wenn Sie nicht über ein Konto verfügen [melden Sie sich jetzt](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="cc286-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="cc286-112">Die folgenden Tools sind erforderlich:</span><span class="sxs-lookup"><span data-stu-id="cc286-112">The following tools are required:</span></span>

* <span data-ttu-id="cc286-113">[Git](https://git-scm.com/downloads) &mdash; ein grundlegender Überblick über Git wird in diesem Handbuch empfohlen.</span><span class="sxs-lookup"><span data-stu-id="cc286-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="cc286-114">Überprüfen Sie die [Git-Dokumentation](https://git-scm.com/doc), insbesondere [Git-Remoterepository](https://git-scm.com/docs/git-remote) und [Git-Push](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="cc286-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="cc286-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; 2.1.300-Version oder höher erstellen und Ausführen der Beispiel-app erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="cc286-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="cc286-116">Wenn die Installation von Visual Studio mit der **plattformübergreifende Entwicklung mit .NET Core** Workload, die .NET Core SDK ist bereits installiert.</span><span class="sxs-lookup"><span data-stu-id="cc286-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="cc286-117">Überprüfen Sie die .NET Core SDK-Installation.</span><span class="sxs-lookup"><span data-stu-id="cc286-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="cc286-118">Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="cc286-118">Open a command shell, and run the following command:</span></span>

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="cc286-119">Empfohlene Tools (nur Windows)</span><span class="sxs-lookup"><span data-stu-id="cc286-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="cc286-120">[Visual Studio](https://www.visualstudio.com/)des robuste Azure-Tools bieten einer grafische Benutzeroberfläche für den Großteil der Funktionalität, die in diesem Handbuch beschrieben.</span><span class="sxs-lookup"><span data-stu-id="cc286-120">[Visual Studio](https://www.visualstudio.com/)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="cc286-121">Eine beliebige Edition von Visual Studio funktionieren, einschließlich der kostenlosen Visual Studio Community Edition.</span><span class="sxs-lookup"><span data-stu-id="cc286-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="cc286-122">In den Tutorials werden geschrieben, um Entwicklung und Bereitstellung von DevOps mit und ohne Visual Studio veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="cc286-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="cc286-123">Vergewissern Sie sich, dass Visual Studio die folgenden verfügt [Workloads](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) installiert:</span><span class="sxs-lookup"><span data-stu-id="cc286-123">Confirm that Visual Studio has the following [workloads](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="cc286-124">ASP.NET und Webentwicklung</span><span class="sxs-lookup"><span data-stu-id="cc286-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="cc286-125">Azure-Entwicklung</span><span class="sxs-lookup"><span data-stu-id="cc286-125">Azure development</span></span>
  * <span data-ttu-id="cc286-126">Plattformübergreifende .NET Core-Entwicklung</span><span class="sxs-lookup"><span data-stu-id="cc286-126">.NET Core cross-platform development</span></span>