---
title: Umgebung Tag Helper in ASP.NET Core
author: pkellner
description: "ASP.Net Core Umgebung Tag Helper definiert, einschließlich aller Eigenschaften"
keywords: ASP.NET Core, Taghilfsprogramm
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 6da7840b84ae453483519e9610d9bb59c0e8fdb5
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Umgebung Tag Helper in ASP.NET Core

Durch [Peter Kellner](http://peterkellner.net) und [Hisham "bin" Ateya](https://twitter.com/hishambinateya)

Die Umgebung Tags Hilfsprogramm rendert bedingt den eingeschlossenen Inhalt basierend auf der aktuellen hostumgebung. Die einzelnen Attribut `names` ist eine durch Trennzeichen getrennte Liste der Umgebung Namen, wenn eine Übereinstimmung mit der aktuellen Umgebung, löst den eingeschlossenen Inhalt gerendert werden soll.

## <a name="environment-tag-helper-attributes"></a>Umgebung Tagattribute-Hilfsprogramm

### <a name="names"></a>Namen

Akzeptiert ein einzelnes hosting Umgebungsname oder eine durch Trennzeichen getrennte Liste hosten Umgebungsnamen, mit die das Rendering des eingeschlossenen Inhalts ausgelöst.

Diese Werte werden auf den aktuellen Wert von der statischen ASP.NET Core-Eigenschaft zurückgegebenen verglichen `HostingEnvironment.EnvironmentName`.  Dieser Wert ist eine der folgenden: **Staging**; **Entwicklung** oder **Produktion**. Der Vergleich die Groß-/Kleinschreibung ignoriert.

Ein Beispiel einer gültigen `environment` Tag Helper ist:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>einschließen und Ausschließen von Attributen

ASP.NET Core 2.x fügt die `include`  &  `exclude` Attribute. Diese Attribute-Steuerelement, das Rendern des eingeschlossenen Inhalts basierend auf den eingeschlossene oder ausgeschlossene hosting Umgebungsnamen.

### <a name="include-aspnet-core-20-and-later"></a>ASP.NET Core 2.0 und höher enthalten

Die `include` Eigenschaft weist ein ähnliches Verhalten von der `names` -Attribut in ASP.NET Core 1.0.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>Ausschließen von ASP.NET Core 2.0 und höher

Im Gegensatz dazu die `exclude` Eigenschaft ermöglicht die `EnvironmentTagHelper` Rendern den eingeschlossenen Inhalt für alle hosting Umgebungsnamen mit Ausnahme der Datei, die Sie angegeben haben.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
