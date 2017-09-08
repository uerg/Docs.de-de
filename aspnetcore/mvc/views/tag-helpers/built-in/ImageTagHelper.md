---
title: Bild-Tag-Hilfsprogramm | Microsoft Docs
author: pkellner
description: Veranschaulicht das Arbeiten mit Images Tag Helper
keywords: ASP.NET Core, Tag-Hilfsprogramm
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a013
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/ImageTagHelper
ms.openlocfilehash: 67537674154d885fc6f69accd2cc7f01c9104d71
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="imagetaghelper"></a><span data-ttu-id="ff04b-104">ImageTagHelper</span><span class="sxs-lookup"><span data-stu-id="ff04b-104">ImageTagHelper</span></span>

<span data-ttu-id="ff04b-105">Durch [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="ff04b-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="ff04b-106">Das Image-Tag-Hilfsobjekt verbessert die `img` (`<img>`) Tag.</span><span class="sxs-lookup"><span data-stu-id="ff04b-106">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="ff04b-107">Erfordert eine `src` Tag als auch die `boolean` Attribut `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="ff04b-107">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="ff04b-108">Wenn die Bildquelle (`src`) ist eine statische Datei auf dem Host-Webserver, ein eindeutiger Cache Abwehrprogramm Zeichenfolge als Abfrageparameter an die Bildquelle angefügt ist.</span><span class="sxs-lookup"><span data-stu-id="ff04b-108">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="ff04b-109">Dadurch wird sichergestellt, dass wenn die Datei auf dem Webserver des Hosts geändert wird, eine eindeutige Anforderungs-URL generiert wird, die die aktualisierte Anforderungsparameter enthält.</span><span class="sxs-lookup"><span data-stu-id="ff04b-109">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="ff04b-110">Der Cache Abwehrprogramm Zeichenfolge ist ein eindeutiger Wert, der den Hash der Datei statisches Bild darstellt.</span><span class="sxs-lookup"><span data-stu-id="ff04b-110">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="ff04b-111">Wenn die Bildquelle (`src`) ist, eine statische Datei (z. B. eine remote-URL oder die Datei existiert nicht auf dem Server), die `<img>` des Tags `src` Attribut ohne Cache Abwehrprogramm Abfragezeichenfolgen-Parameters generiert wird.</span><span class="sxs-lookup"><span data-stu-id="ff04b-111">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="ff04b-112">Bildattributen Tag-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="ff04b-112">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="ff04b-113">ASP-Anhängen-version</span><span class="sxs-lookup"><span data-stu-id="ff04b-113">asp-append-version</span></span>

<span data-ttu-id="ff04b-114">Wenn zusammen mit angegebenen ein `src` -Attribut, das Image-Tag-Hilfsprogramm wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="ff04b-114">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="ff04b-115">Ein Beispiel einer gültigen `img` Tag Helper ist:</span><span class="sxs-lookup"><span data-stu-id="ff04b-115">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="ff04b-116">Eine statische Datei im Verzeichnis vorhanden *... wwwroot/Images/asplogo.PNG* die generierte HTML-Code ist ähnlich der folgenden (der Hash wird abweichen):</span><span class="sxs-lookup"><span data-stu-id="ff04b-116">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="ff04b-117">Der Wert, der dem Parameter zugewiesen `v` wird der Hashwert der Datei auf dem Datenträger.</span><span class="sxs-lookup"><span data-stu-id="ff04b-117">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="ff04b-118">Ist der Webserver konnte nicht abgerufen werden Lesezugriff auf die statische Datei auf die verwiesen wird, keine `v` Parameter hinzugefügt wird die `src` Attribut.</span><span class="sxs-lookup"><span data-stu-id="ff04b-118">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="ff04b-119">src</span><span class="sxs-lookup"><span data-stu-id="ff04b-119">src</span></span>

<span data-ttu-id="ff04b-120">Um das Image-Tag-Hilfsprogramm zu aktivieren, muss das Src-Attribut auf die `<img>` Element.</span><span class="sxs-lookup"><span data-stu-id="ff04b-120">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="ff04b-121">Das Image-Tag-Hilfsprogramm verwendet die `Cache` Anbieter auf dem lokalen Webserver zum Speichern der berechneten `Sha512` einer angegebenen Datei.</span><span class="sxs-lookup"><span data-stu-id="ff04b-121">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="ff04b-122">Wenn die Datei erneut angefordert wird die `Sha512` muss nicht neu berechnet werden.</span><span class="sxs-lookup"><span data-stu-id="ff04b-122">If the file is requested again the `Sha512` does not need to be recalculated.</span></span> <span data-ttu-id="ff04b-123">Der Cache wird durch einen Dateimonitor für die, die an die Datei angehängt ist ungültig Wenn der Datei `Sha512` berechnet wird.</span><span class="sxs-lookup"><span data-stu-id="ff04b-123">The Cache is invalidated by a file watcher that is attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff04b-124">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ff04b-124">Additional resources</span></span>

* <xref:performance/caching/memory>