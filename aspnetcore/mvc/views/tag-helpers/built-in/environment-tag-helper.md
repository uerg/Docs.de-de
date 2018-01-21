---
title: Umgebung Tag Helper in ASP.NET Core
author: pkellner
description: "ASP.NET Core Umgebung Tag Helper definiert, einschließlich aller Eigenschaften"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 32646f1fdaf840f796da1ec573459157a41a86d1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="cc005-103">Umgebung Tag Helper in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc005-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="cc005-104">Durch [Peter Kellner](http://peterkellner.net) und [Hisham "bin" Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="cc005-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="cc005-105">Die Umgebung Tags Hilfsprogramm rendert bedingt den eingeschlossenen Inhalt basierend auf der aktuellen hostumgebung.</span><span class="sxs-lookup"><span data-stu-id="cc005-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="cc005-106">Die einzelnen Attribut `names` ist eine durch Trennzeichen getrennte Liste der Umgebung Namen, wenn eine Übereinstimmung mit der aktuellen Umgebung, löst den eingeschlossenen Inhalt gerendert werden soll.</span><span class="sxs-lookup"><span data-stu-id="cc005-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="cc005-107">Umgebung Tagattribute-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="cc005-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="cc005-108">Namen</span><span class="sxs-lookup"><span data-stu-id="cc005-108">names</span></span>

<span data-ttu-id="cc005-109">Akzeptiert ein einzelnes hosting Umgebungsname oder eine durch Trennzeichen getrennte Liste hosten Umgebungsnamen, mit die das Rendering des eingeschlossenen Inhalts ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="cc005-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="cc005-110">Diese Werte werden auf den aktuellen Wert von der statischen ASP.NET Core-Eigenschaft zurückgegebenen verglichen `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="cc005-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="cc005-111">Dieser Wert ist eine der folgenden: **Staging**; **Entwicklung** oder **Produktion**.</span><span class="sxs-lookup"><span data-stu-id="cc005-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="cc005-112">Der Vergleich die Groß-/Kleinschreibung ignoriert.</span><span class="sxs-lookup"><span data-stu-id="cc005-112">The comparison ignores case.</span></span>

<span data-ttu-id="cc005-113">Ein Beispiel einer gültigen `environment` Tag Helper ist:</span><span class="sxs-lookup"><span data-stu-id="cc005-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="cc005-114">einschließen und Ausschließen von Attributen</span><span class="sxs-lookup"><span data-stu-id="cc005-114">include and exclude attributes</span></span>

<span data-ttu-id="cc005-115">ASP.NET Core 2.x fügt die `include`  &  `exclude` Attribute.</span><span class="sxs-lookup"><span data-stu-id="cc005-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="cc005-116">Diese Attribute-Steuerelement, das Rendern des eingeschlossenen Inhalts basierend auf den eingeschlossene oder ausgeschlossene hosting Umgebungsnamen.</span><span class="sxs-lookup"><span data-stu-id="cc005-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="cc005-117">ASP.NET Core 2.0 und höher enthalten</span><span class="sxs-lookup"><span data-stu-id="cc005-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="cc005-118">Die `include` Eigenschaft weist ein ähnliches Verhalten von der `names` -Attribut in ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="cc005-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="cc005-119">Ausschließen von ASP.NET Core 2.0 und höher</span><span class="sxs-lookup"><span data-stu-id="cc005-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="cc005-120">Im Gegensatz dazu die `exclude` Eigenschaft ermöglicht die `EnvironmentTagHelper` Rendern den eingeschlossenen Inhalt für alle hosting Umgebungsnamen mit Ausnahme der Datei, die Sie angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="cc005-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="cc005-121">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="cc005-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
