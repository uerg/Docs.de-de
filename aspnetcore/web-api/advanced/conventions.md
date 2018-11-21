---
title: Verwenden von Web-API-Konventionen
author: pranavkm
description: Informationen zu Web-API-Konventionen in ASP.NET Core
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 023b8d09511aa42966e2a7d1c85e407bb6e79b0f
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635395"
---
# <a name="use-web-api-conventions"></a>Verwenden von Web-API-Konventionen

Mit ASP.NET Core 2.2 wird die Möglichkeit eingeführt, allgemeine [API-Dokumentation](xref:tutorials/web-api-help-pages-using-swagger) zu extrahieren und diese auf mehrere Aktionen, Controller sowie alle Controller in einer Assembly anzuwenden. Web-API-Konventionen können verwendet werden, damit keine einzelnen Aktionen mit [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) versehen werden müssen. Sie können sie nutzen, um die gängigsten Rückgabetypen und Statuscodes, die durch Ihre Aktionen zurückgegeben werden, mit einer Möglichkeit zu definieren, die Konvention auszuwählen, die zu der jeweiligen Aktion passt.

In der Regel sind verschiedene Standardkonventionen im Lieferumfang von ASP.NET Core MVC 2.2 enthalten (`Microsoft.AspNetCore.Mvc.DefaultApiConventions`). Diese Konventionen basieren auf dem Controller, dessen Gerüst ASP.NET Core erstellt. Wenn Ihre Aktionen dem Muster folgen, das aus dem Gerüstbau resultiert, sollten die Standardkonventionen ausreichen.

<xref:Microsoft.AspNetCore.Mvc.ApiExplorer> erkennt zur Laufzeit Konventionen. `ApiExplorer` ist die Abstraktion von MVC, über die mit Dokumentgeneratoren für Open API kommuniziert wird. Die Attribute der angewendeten Konvention werden einer Aktion zugeordnet und zu der Swagger-Dokumentation der Aktion hinzugefügt. Auch API-Analysetools erkennen Konventionen. Wenn Ihre Aktion keiner Konvention folgt (also z.B. einen Statuscode zurückgibt, der nicht von der angewendeten Konvention dokumentiert wird), wird eine Warnung ausgegeben, in der Sie zur Dokumentation aufgefordert werden.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>Anwenden von Web-API-Konventionen

Es gibt drei Möglichkeiten, eine Konvention anzuwenden. Konventionen werden nicht miteinander kombiniert. Jede Aktion kann genau einer Konvention zugeordnet werden. Genauere Konventionen (unten aufgeführt) haben Vorrang gegenüber ungenaueren. Die Auswahl ist nicht deterministisch, wenn mindestens zwei Konventionen mit gleicher Priorität für eine Aktion gelten. Sie können eine der folgenden Optionen auswählen, um angefangen bei der genausten Konvention bis hin zur ungenausten eine Konvention auf eine Aktion anzuwenden:

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Gilt für einzelne Aktionen und gibt den jeweiligen passenden Konventionstyp und die Konventionsmethode an. Im folgenden Beispiel wird die Konventionsmethode `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` auf die Aktion `Update` angewendet:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute`, auf einen Controller angewendet &mdash; wendet den Konventionstyp bei allen Controlleraktionen an. Konventionsmethoden werden mit Hinweisen dazu versehen, für welche Aktionen sie jeweils anzuwenden sind (werden als Teil von Konventionen zur Dokumenterstellung zugewiesen). Zum Beispiel:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute`, auf eine Assembly angewendet &mdash; wendet den Konventionstyp auf alle Controller in der aktuellen Assembly an. Zum Beispiel:

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a>Erstellen von Web-API-Konventionen

Bei einer Konvention handelt es sich um einen statischen Typ mit Methoden. Diese Methoden werden mit den Attributen `[ProducesResponseType]` oder `[ProducesDefaultResponseType]` versehen.

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

Wenn diese Konvention auf eine Assembly angewendet wird, wird die Konventionsmethode auf eine beliebige Aktion mit dem Namen `Find` und einem Parameter mit dem Namen `id` angewendet, wenn keine genaueren Metadatenattribute vorhanden sind.

Neben `[ProducesResponseType]` und `[ProducesDefaultResponseType]` können auch die Attribute `[ApiConventionNameMatch]` und `[ApiConventionTypeMatch]` auf die Konventionsmethode angewendet werden, die die Methoden bestimmt, auf die sie anzuwenden sind. Zum Beispiel:

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* Wenn die Option `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` auf die Methode angewendet wird, deutet dies darauf hin, dass die Konvention mit jeder beliebigen Aktion übereinstimmt, solange diese das Präfix „Find“ aufweist. Dies betrifft z.B. die Methoden `Find`, `FindPet` und `FindById`.
* Wenn die Option `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` auf den Parameter angewendet wird, deutet dies darauf hin, dass die Konvention Methoden zugeordnet werden kann, die genau einen Parameter aufweisen, der auf dem Suffixbezeichner endet. Dies betrifft z.B. die Parameter `id` und `petId`. Genauso kann `ApiConventionTypeMatch` auf Typen angewendet, um den Parametertyp einzuschränken. Ein `params[]`-Argument kann verwendet werden, um auf verbleibende Parameter zu verweisen, die nicht genau zugeordnet werden müssen.
