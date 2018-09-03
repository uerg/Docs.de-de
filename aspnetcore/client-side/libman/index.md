---
title: Clientseitiger Bibliothekserwerb in ASP.NET Core mit LibMan
author: scottaddie
description: Erfahren Sie, wie Sie clientseitige Bibliotheksobjekte in einem ASP.NET Core-Projekt über den Bibliotheks-Manager (LibMan) installieren.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: a6ff0cc3342cfac74739387aa17046ed5050232f
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312358"
---
# <a name="client-side-library-acquisition-in-aspnet-core-with-libman"></a><span data-ttu-id="807d2-103">Clientseitiger Bibliothekserwerb in ASP.NET Core mit LibMan</span><span class="sxs-lookup"><span data-stu-id="807d2-103">Client-side library acquisition in ASP.NET Core with LibMan</span></span>

<span data-ttu-id="807d2-104">Von [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="807d2-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="807d2-105">Der Bibliotheks-Manager (LibMan) ist ein einfaches, clientseitiges Tool zum Bibliothekserwerb.</span><span class="sxs-lookup"><span data-stu-id="807d2-105">Library Manager (LibMan) is a lightweight, client-side library acquisition tool.</span></span> <span data-ttu-id="807d2-106">LibMan lädt beliebte Bibliotheken und Frameworks vom Dateisystem oder von einem [Content Delivery Network (CDN)](https://wikipedia.org/wiki/Content_delivery_network) herunter.</span><span class="sxs-lookup"><span data-stu-id="807d2-106">LibMan downloads popular libraries and frameworks from the file system or from a [content delivery network (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span></span> <span data-ttu-id="807d2-107">Die unterstützten CDNs sind z.B. [CDNJS](https://cdnjs.com/) und [unpkg](https://unpkg.com/#/).</span><span class="sxs-lookup"><span data-stu-id="807d2-107">The supported CDNs include [CDNJS](https://cdnjs.com/) and [unpkg](https://unpkg.com/#/).</span></span> <span data-ttu-id="807d2-108">Die ausgewählten Bibliotheksdateien werden abgerufen und an der entsprechenden Position innerhalb des ASP.NET Core-Projekts platziert.</span><span class="sxs-lookup"><span data-stu-id="807d2-108">The selected library files are fetched and placed in the appropriate location within the ASP.NET Core project.</span></span>

## <a name="libman-use-cases"></a><span data-ttu-id="807d2-109">LibMan-Einsatzbeispiele</span><span class="sxs-lookup"><span data-stu-id="807d2-109">LibMan use cases</span></span>

<span data-ttu-id="807d2-110">LibMan bietet folgende Vorteile:</span><span class="sxs-lookup"><span data-stu-id="807d2-110">LibMan offers the following benefits:</span></span>

* <span data-ttu-id="807d2-111">Es werden nur die Bibliotheksdateien herunterladen, die Sie benötigten.</span><span class="sxs-lookup"><span data-stu-id="807d2-111">Only the library files you need are downloaded.</span></span>
* <span data-ttu-id="807d2-112">Zusätzliche Tools, wie etwa [Node.js](https://nodejs.org), [npm](https://www.npmjs.com) und [WebPack](https://webpack.js.org), sind nicht nötig, um eine Teilmenge von Dateien in einer Bibliothek abzurufen.</span><span class="sxs-lookup"><span data-stu-id="807d2-112">Additional tooling, such as [Node.js](https://nodejs.org), [npm](https://www.npmjs.com), and [WebPack](https://webpack.js.org), isn't necessary to acquire a subset of files in a library.</span></span>
* <span data-ttu-id="807d2-113">Dateien können an einer bestimmten Position platziert werden, ohne dass auf Buildaufgaben oder auf das manuelle Kopieren von Dateien zurückgegriffen werden muss.</span><span class="sxs-lookup"><span data-stu-id="807d2-113">Files can be placed in a specific location without resorting to build tasks or manual file copying.</span></span>

<span data-ttu-id="807d2-114">Weitere Informationen zu den Vorteilen von LibMan finden Sie im Video [Modern front-end web development in Visual Studio 2017: LibMan segment (Moderne Front-End-Webentwicklung in Visual Studio 2017: LibMan-Segment)](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).</span><span class="sxs-lookup"><span data-stu-id="807d2-114">For more information about LibMan's benefits, watch [Modern front-end web development in Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).</span></span>

<span data-ttu-id="807d2-115">LibMan ist kein Paketverwaltungssystem.</span><span class="sxs-lookup"><span data-stu-id="807d2-115">LibMan isn't a package management system.</span></span> <span data-ttu-id="807d2-116">Wenn Sie bereits einen Paket-Manager wie npm oder [yarn](https://yarnpkg.com) verwenden, tun Sie das auch weiterhin.</span><span class="sxs-lookup"><span data-stu-id="807d2-116">If you're already using a package manager, such as npm or [yarn](https://yarnpkg.com), continue doing so.</span></span> <span data-ttu-id="807d2-117">LibMan wurde nicht dazu entwickelt, diese Tools zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="807d2-117">LibMan wasn't developed to replace those tools.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="807d2-118">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="807d2-118">Additional resources</span></span>

* <xref:client-side/libman/libman-vs>
* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="807d2-119">GitHub-Repository für LibMan</span><span class="sxs-lookup"><span data-stu-id="807d2-119">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)