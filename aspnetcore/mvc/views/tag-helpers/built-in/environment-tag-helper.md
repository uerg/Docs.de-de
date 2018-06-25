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
# <a name="environment-tag-helper-in-aspnet-core"></a>Umgebungstaghilfsprogramm in ASP.NET Core

Von [Peter Kellner](http://peterkellner.net) und [Hisham Bin Ateya](https://twitter.com/hishambinateya)

Basierend auf der aktuellen Hostingumgebung rendert das Umgebungstaghilfsprogramm den von ihm eingeschlossenen Inhalt unter Vorbehalt. Das Programm besitzt nur das Attribut `names`, das aus einer durch Trennzeichen getrennten Liste mit Umgebungsnamen besteht. Wenn mindestens einer der Namen mit dem Namen der aktuellen Umgebung übereinstimmt, wird der eingeschlossene Inhalt gerendert.

## <a name="environment-tag-helper-attributes"></a>Attribute von Umgebungstaghilfsprogrammen

### <a name="names"></a>Namen

Akzeptiert den Namen einer einzelnen Hostingumgebung oder eine durch Trennzeichen getrennte Liste mit Namen von Hostingumgebungen, die das Rendering des eingeschlossenen Inhalts auslösen.

Dieser Wert bzw. diese Werte werden mit dem aktuellen Wert verglichen, der von der statischen ASP.NET Core-Eigenschaft `HostingEnvironment.EnvironmentName` zurückgegeben wird.  Folgende Werte können zurückgegeben werden: **Staging**; **Entwicklung** oder **Produktion**. Bei dem Vergleich wird die Groß-/Kleinschreibung ignoriert.

Das folgende Beispiel zeigt ein gültiges `environment`-Taghilfsprogramm:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>Die Attribute „include“ und „exclude“

ASP.NET Core 2.x fügt die Attribute `include` & `exclude` hinzu. Diese Attribute steuern das Rendern des eingeschlossenen Inhalts anhand der eingeschlossenen bzw. ausgeschlossenen Namen von Hostingumgebungen.

### <a name="include-aspnet-core-20-and-later"></a>Attribut „include“ in ASP.NET Core 2.0 und höher

Die Eigenschaft `include` weist ein ähnliches Verhalten wie das Attribut `names` in ASP.NET Core 1.0 auf.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>Attribut „exclude“ in ASP.NET Core 2.0 und höher

Im Gegensatz dazu ermöglicht die Eigenschaft `exclude` dem `EnvironmentTagHelper` das Rendern des eingeschlossenen Inhalts für alle Namen von Hostingumgebungen außer dem bzw. den von Ihnen angegebenen Namen.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
