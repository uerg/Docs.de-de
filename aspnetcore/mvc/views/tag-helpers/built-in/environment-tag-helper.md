---
title: Umgebungstaghilfsprogramm in ASP.NET Core
author: pkellner
description: Definition des Umgebungstaghilfsprogramms für ASP.NET Core, einschließlich aller Eigenschaften
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 05c07b06a4fedac0b0ff39d168807f5e2e6996cf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276915"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="5c3f7-103">Umgebungstaghilfsprogramm in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c3f7-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="5c3f7-104">Von [Peter Kellner](http://peterkellner.net) und [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="5c3f7-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="5c3f7-105">Basierend auf der aktuellen Hostingumgebung rendert das Umgebungstaghilfsprogramm den von ihm eingeschlossenen Inhalt unter Vorbehalt.</span><span class="sxs-lookup"><span data-stu-id="5c3f7-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="5c3f7-106">Das Programm besitzt nur das Attribut `names`, das aus einer durch Trennzeichen getrennten Liste mit Umgebungsnamen besteht. Wenn mindestens einer der Namen mit dem Namen der aktuellen Umgebung übereinstimmt, wird der eingeschlossene Inhalt gerendert.</span><span class="sxs-lookup"><span data-stu-id="5c3f7-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="5c3f7-107">Attribute von Umgebungstaghilfsprogrammen</span><span class="sxs-lookup"><span data-stu-id="5c3f7-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="5c3f7-108">Namen</span><span class="sxs-lookup"><span data-stu-id="5c3f7-108">names</span></span>

<span data-ttu-id="5c3f7-109">Akzeptiert den Namen einer einzelnen Hostingumgebung oder eine durch Trennzeichen getrennte Liste mit Namen von Hostingumgebungen, die das Rendering des eingeschlossenen Inhalts auslösen.</span><span class="sxs-lookup"><span data-stu-id="5c3f7-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="5c3f7-110">Dieser Wert bzw. diese Werte werden mit dem aktuellen Wert verglichen, der von der statischen ASP.NET Core-Eigenschaft `HostingEnvironment.EnvironmentName` zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="5c3f7-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="5c3f7-111">Folgende Werte können zurückgegeben werden: **Staging**; **Entwicklung** oder **Produktion**.</span><span class="sxs-lookup"><span data-stu-id="5c3f7-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="5c3f7-112">Bei dem Vergleich wird die Groß-/Kleinschreibung ignoriert.</span><span class="sxs-lookup"><span data-stu-id="5c3f7-112">The comparison ignores case.</span></span>

<span data-ttu-id="5c3f7-113">Das folgende Beispiel zeigt ein gültiges `environment`-Taghilfsprogramm:</span><span class="sxs-lookup"><span data-stu-id="5c3f7-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="5c3f7-114">Die Attribute „include“ und „exclude“</span><span class="sxs-lookup"><span data-stu-id="5c3f7-114">include and exclude attributes</span></span>

<span data-ttu-id="5c3f7-115">ASP.NET Core 2.x fügt die Attribute `include` & `exclude` hinzu.</span><span class="sxs-lookup"><span data-stu-id="5c3f7-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="5c3f7-116">Diese Attribute steuern das Rendern des eingeschlossenen Inhalts anhand der eingeschlossenen bzw. ausgeschlossenen Namen von Hostingumgebungen.</span><span class="sxs-lookup"><span data-stu-id="5c3f7-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="5c3f7-117">Attribut „include“ in ASP.NET Core 2.0 und höher</span><span class="sxs-lookup"><span data-stu-id="5c3f7-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="5c3f7-118">Die Eigenschaft `include` weist ein ähnliches Verhalten wie das Attribut `names` in ASP.NET Core 1.0 auf.</span><span class="sxs-lookup"><span data-stu-id="5c3f7-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="5c3f7-119">Attribut „exclude“ in ASP.NET Core 2.0 und höher</span><span class="sxs-lookup"><span data-stu-id="5c3f7-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="5c3f7-120">Im Gegensatz dazu ermöglicht die Eigenschaft `exclude` dem `EnvironmentTagHelper` das Rendern des eingeschlossenen Inhalts für alle Namen von Hostingumgebungen außer dem bzw. den von Ihnen angegebenen Namen.</span><span class="sxs-lookup"><span data-stu-id="5c3f7-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="5c3f7-121">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="5c3f7-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
