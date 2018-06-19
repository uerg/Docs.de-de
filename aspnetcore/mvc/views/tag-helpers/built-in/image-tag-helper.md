---
title: Image-Taghilfsprogramm in ASP.NET Core
author: pkellner
description: Veranschaulicht die Arbeit mit dem Image-Taghilfsprogramm
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 6aa9175f873c4ea62e0319c812e5312cd3331141
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072261"
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="686d3-103">Image-Taghilfsprogramm in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="686d3-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="686d3-104">Von [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="686d3-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="686d3-105">Das Image-Taghilfsprogramm verbessert den Tag `img` (`<img>`).</span><span class="sxs-lookup"><span data-stu-id="686d3-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="686d3-106">Es benötigt den Tag `src` und das `boolean`-Attribut `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="686d3-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="686d3-107">Wenn die Bildquelle (`src`) eine statische Datei auf dem Hostwebserver ist, wird der Bildquelle eine eindeutige Cache-Busting-Zeichenfolge als Abfrageparameter angefügt.</span><span class="sxs-lookup"><span data-stu-id="686d3-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="686d3-108">Dadurch wird eine eindeutige Anforderungs-URL erstellt, die den aktualisierten Anforderungsparameter enthält, wenn die Datei auf dem Hostwebserver geändert wird.</span><span class="sxs-lookup"><span data-stu-id="686d3-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="686d3-109">Die Cache-Busting-Zeichenfolge ist ein eindeutiger Wert, der den Hash der statischen Bilddatei darstellt.</span><span class="sxs-lookup"><span data-stu-id="686d3-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="686d3-110">Wenn die Bildquelle (`src`) keine statische Datei ist (z.B. eine Remote-URL oder die Datei ist auf dem Server nicht vorhanden), wird das Attribut `src` des Tags `<img>` ohne einen Cache-Busting-Abfragezeichenfolgenparameter erstellt.</span><span class="sxs-lookup"><span data-stu-id="686d3-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="686d3-111">Attribute von Image-Taghilfsprogrammen</span><span class="sxs-lookup"><span data-stu-id="686d3-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="686d3-112">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="686d3-112">asp-append-version</span></span>

<span data-ttu-id="686d3-113">Wenn das Attribut `src` zusätzlich angegeben wird, wird das Image-Taghilfsprogramm aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="686d3-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="686d3-114">Das folgende Beispiel zeigt ein gültiges `img`-Taghilfsprogramm:</span><span class="sxs-lookup"><span data-stu-id="686d3-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="686d3-115">Wenn die statische Datei im Verzeichnis *..wwwroot/images/asplogo.png* vorhanden ist, ähnelt der erstellte HTML-Code folgendem (der Hash wird abweichen):</span><span class="sxs-lookup"><span data-stu-id="686d3-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="686d3-116">Der Wert, der dem Parameter `v` zugewiesen ist, ist der Hashwert der Datei auf dem Datenträger.</span><span class="sxs-lookup"><span data-stu-id="686d3-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="686d3-117">Wenn der Webserver den Lesezugriff auf die statische Datei, auf die verwiesen wird, nicht erhalten kann, werden keine `v`-Parameter dem Attribut `src` zugefügt.</span><span class="sxs-lookup"><span data-stu-id="686d3-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="686d3-118">src</span><span class="sxs-lookup"><span data-stu-id="686d3-118">src</span></span>

<span data-ttu-id="686d3-119">Das src-Attribut wird auf dem Element `<img>` benötigt, um das Image-Taghilfsprogramm zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="686d3-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="686d3-120">Das Image-Taghilfsprogramm verwendet den `Cache`-Anbieter auf dem lokalen Webserver zum Speichern der berechneten `Sha512` einer angegebenen Datei.</span><span class="sxs-lookup"><span data-stu-id="686d3-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="686d3-121">Wenn die Datei erneut angefordert wird, muss `Sha512` nicht neu berechnet werden.</span><span class="sxs-lookup"><span data-stu-id="686d3-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="686d3-122">Der Cache wird durch einen Datei-Watcher ungültig gemacht, der an die Datei angefügt wird, wenn `Sha512` der Datei berechnet wird.</span><span class="sxs-lookup"><span data-stu-id="686d3-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="686d3-123">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="686d3-123">Additional resources</span></span>

* <xref:performance/caching/memory>
