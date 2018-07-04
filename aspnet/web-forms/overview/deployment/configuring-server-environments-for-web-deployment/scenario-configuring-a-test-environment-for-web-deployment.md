---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Szenario: Konfigurieren einer Testumgebung für die Webbereitstellung | Microsoft-Dokumentation'
author: jrjlee
description: In diesem Thema wird beschrieben, ein typisches Web-Bereitstellungsszenario für Entwickler oder testumgebungen und erläutert, die Aufgaben, die Sie zum Einrichten eines servicevorfalls durchführen müssen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f8636fab82b63edab50fc13ae32f4dd536f133ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382493"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="ebd1f-103">Szenario: Konfigurieren einer Testumgebung für die Webbereitstellung</span><span class="sxs-lookup"><span data-stu-id="ebd1f-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="ebd1f-104">durch [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="ebd1f-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="ebd1f-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="ebd1f-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="ebd1f-106">In diesem Thema wird beschrieben, ein typisches Web-Bereitstellungsszenario für Entwickler oder testumgebungen und erläutert, die Aufgaben, die Sie ausführen, um eine ähnliche Umgebung einrichten müssen.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="ebd1f-107">Wenn Entwickler auf Webanwendungen arbeiten zu können, sind sie häufig Zugriff auf eine Server-Umgebung gewährt, die sie verwenden können, um Änderungen für ihre Anwendungen in einer realistischen Umgebung zu testen.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="ebd1f-108">Diese Art von Entwicklungs-oder testumgebung hat in der Regel folgende Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="ebd1f-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="ebd1f-109">Die Umgebung besteht aus einem einzelnen Web-Server und einem einzelnen Datenbankserver.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="ebd1f-110">Die Entwickler werden in der Regel Administratorrechte auf den Servern, auf die Umgebung die Anforderungen ihrer Anwendungen konfigurieren zu können.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="ebd1f-111">Änderungen an Anwendungen in regelmäßigen Abständen, bereitgestellt werden, damit die Umgebung zur Unterstützung von einem Schritt muss oder die automatisierte Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="ebd1f-112">Z. B. in unserer [lernprogrammszenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink ist Entwickler bei Fabrikam, Inc. Matt arbeitet an der Contact Manager-Lösung und muss regelmäßig Änderungen in einer testumgebung bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="ebd1f-113">Matt ist ein Administrator auf dem Test-Webserver und dem Testserver für die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="ebd1f-114">Anfangs muss Matt, um die Lösung direkt in die testumgebung bereitstellen zu können.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="ebd1f-115">Funktionsweise von verknüpfen verläuft und mehr Entwickler im Team, das Contact Manager-Lösung für continuous Integration (CI) in Team Foundation Server (TFS) konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="ebd1f-116">Wenn Entwickler Inhalte eincheckt, sollte Team Build erstellen Sie die Projektmappe, alle Komponententests ausführen und automatisch die Lösung bereitstellen, in die testumgebung.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="ebd1f-117">Übersicht über die Lösung</span><span class="sxs-lookup"><span data-stu-id="ebd1f-117">Solution Overview</span></span>

<span data-ttu-id="ebd1f-118">Die testumgebung muss Schritt für Schritt zu unterstützen oder die automatisierte Bereitstellung von einem Remotecomputer befindet, müssen Sie eine Auswahl von zwei Hauptansätze.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="ebd1f-119">Sie haben folgende Möglichkeiten:</span><span class="sxs-lookup"><span data-stu-id="ebd1f-119">You can:</span></span>

- <span data-ttu-id="ebd1f-120">Konfigurieren des Test-Servers zur Unterstützung der Bereitstellung mithilfe der Webbereitstellungs-Agent-Dienst ("remote-Agent").</span><span class="sxs-lookup"><span data-stu-id="ebd1f-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="ebd1f-121">Konfigurieren Sie den Test-Webserver für die Bereitstellung der Web Deploy-Handler zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="ebd1f-122">Außerdem können Sie [Web Deploy bei Bedarf](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("temporäre Agent" bezeichnet).</span><span class="sxs-lookup"><span data-stu-id="ebd1f-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="ebd1f-123">Dies ist vergleichbar mit dem remote-Agent-Ansatz in Bezug auf die Anforderungen und Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="ebd1f-124">In diesem Fall die Entwickler über Administratorrechte verfügen, auf dem Zielserver, und die testumgebung unterliegt nicht der strenge sicherheitseinschränkungen, daher ist die logische Wahl Testwebserver zur Unterstützung einer Bereitstellung mit dem remote-Agent zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="ebd1f-125">Dies ist weniger komplex und erfordert weniger anfängliche Konfiguration als der Ansatz WebHandler bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="ebd1f-126">Sie müssen auch so konfigurieren Sie Ihren Datenbankserver zur Unterstützung von Remotezugriff und Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="ebd1f-127">In diesen Themen enthalten alle Informationen, die Sie benötigen, um diese Aufgaben ausführen:</span><span class="sxs-lookup"><span data-stu-id="ebd1f-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="ebd1f-128">[Konfigurieren Sie einen Webserver für Web Deploy-Veröffentlichung (Remote-Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span><span class="sxs-lookup"><span data-stu-id="ebd1f-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="ebd1f-129">In diesem Thema wird beschrieben, wie Sie einen Webserver zu erstellen, der von Web Deploy veröffentlichen, verwenden den remote-Agent-Ansatz, beginnend mit einen bereinigten Build von Windows Server 2008 R2 unterstützt.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="ebd1f-130">[Konfigurieren eines Datenbankservers aus, für Web Deploy-Veröffentlichung](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="ebd1f-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="ebd1f-131">Dieses Thema beschreibt, wie Sie einen Datenbankserver zur Unterstützung von Remotezugriff und -Bereitstellung, beginnend bei einer Standardinstallation von SQL Server 2008 R2 zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="ebd1f-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="ebd1f-132">Weiterführende Themen</span><span class="sxs-lookup"><span data-stu-id="ebd1f-132">Further Reading</span></span>

<span data-ttu-id="ebd1f-133">Anleitungen zum Konfigurieren einer typischen staging-Umgebung finden Sie unter [Szenario: Konfigurieren von einer Staging-Umgebung für die Webbereitstellung](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="ebd1f-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="ebd1f-134">Anleitungen zum Konfigurieren einer typischen produktionsumgebung finden Sie unter [Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="ebd1f-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ebd1f-135">[Zurück](choosing-the-right-approach-to-web-deployment.md)
> [Weiter](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="ebd1f-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>
