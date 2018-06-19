---
title: View-basierte Autorisierung in ASP.NET Core MVC
author: rick-anderson
description: Dieses Dokument veranschaulicht, wie zum Einfügen und Nutzen von dem Prüfdienst innerhalb einer ASP.NET Core Razor-Ansicht.
manager: wpickett
ms.author: riande
ms.date: 10/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/views
ms.openlocfilehash: dad59a297efb4648755436fbd07742f95af97fb2
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
ms.locfileid: "30076632"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>View-basierte Autorisierung in ASP.NET Core MVC

Ein Entwickler möchte häufig einblenden, ausblenden oder ändern Sie andernfalls eine Benutzeroberfläche, die basierend auf der Identität des aktuellen Benutzers. Sie können dem Prüfdienst in MVC-Ansichten über zugreifen [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection#fundamentals-dependency-injection). Um den Autorisierungsdienst in einer Razor-Ansicht einfügen möchten, verwenden Sie die `@inject` Richtlinie:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Wenn Sie dem Prüfdienst in jeder Ansicht möchten, fügen die `@inject` -Direktive in der *_ViewImports.cshtml* Datei von der *Ansichten* Verzeichnis. Weitere Informationen finden Sie unter [Dependency Injection in Ansichten](xref:mvc/views/dependency-injection).

Verwenden Sie den eingefügten Autorisierungsdienst aufzurufenden `AuthorizeAsync` auf genau die gleiche Weise Sie überprüfen, während der würden [ressourcenbasierte Autorisierung](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

In einigen Fällen wird die Ressource die Ansichtsmodell sein. Aufrufen `AuthorizeAsync` auf genau die gleiche Weise Sie überprüfen, während der würden [ressourcenbasierte Autorisierung](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

Im vorangehenden Code wird das Modell als eine Ressource, die die richtlinienauswertung ausführen sollten übergeben, zu berücksichtigen.

> [!WARNING]
> Verlassen Sie sich nicht auf die Umschaltfläche Sichtbarkeit Ihrer app von Elementen der Benutzeroberfläche, wie die einzige autorisierungsprüfung. Ausblenden eines Benutzeroberflächenelements möglicherweise nicht vollständig Zugriff auf seine zugeordneten Controlleraktion verhindern. Betrachten Sie beispielsweise die Schaltfläche im vorherigen Codeausschnitt. Benutzer kann Aufrufen der `Edit` Aktionsmethode, wenn der relative Ressource ist die URL ist */Document/Edit/1*. Aus diesem Grund die `Edit` Aktionsmethode sollte eine eigene autorisierungsprüfung ausführen.
