---
title: View-basierte Autorisierung
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 3b7fa6025d766da80ba92ee27af20bf9bfe0dcf4
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="view-based-authorization"></a>View-basierte Autorisierung

<a name=security-authorization-views></a>

Häufig wird ein Entwickler möchte einblenden, ausblenden oder ändern Sie andernfalls eine Benutzeroberfläche, die basierend auf der Identität des aktuellen Benutzers. Sie können dem Prüfdienst in MVC-Ansichten über zugreifen [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection). Dem Prüfdienst in einer Razor-Verwendung anzeigen Einfügen der `@inject` , z. B. die Richtlinie `@inject IAuthorizationService AuthorizationService` (erfordert `@using Microsoft.AspNetCore.Authorization`). Legen Sie ggf. dem Prüfdienst in jeder Ansicht der `@inject` -Direktive in der `_ViewImports.cshtml` in der Datei die `Views` Verzeichnis. Weitere Informationen zu Abhängigkeitsinjektion in Sichten finden Sie unter [Abhängigkeitsinjektion in Sichten](../../mvc/views/dependency-injection.md).

Nachdem Sie dem Prüfdienst eingefügt haben verwenden Sie ihn durch Aufrufen der `AuthorizeAsync` Methode in genau dieselbe Weise wie Sie während der überprüfen würde [ressourcenbasierten Autorisierung](resourcebased.md#security-authorization-resource-based-imperative).

```csharp
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
   {
       <p>This paragraph is displayed because you fulfilled PolicyName.</p>
   }
   ```

In einigen Fällen werden die Ressourcen Ihrer Ansichtsmodell, und Sie können Aufrufen `AuthorizeAsync` in genau dieselbe Weise wie Sie während der überprüfen würde [ressourcenbasierten Autorisierung](resourcebased.md#security-authorization-resource-based-imperative);

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
   @if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
   @if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```
---

Hier sehen Sie sich, dass das Modell übergeben wird, wie die Ressource Autorisierung berücksichtigt werden sollten.

>[!WARNING]
>Verlassen Sie sich nicht auf den ein- oder Ausblenden der Teile der Benutzeroberfläche als einzige Autorisierung. Ausblenden einer Benutzeroberflächenautomatisierungs bedeutet Element nicht, dass ein Benutzer zugreifen kann. Sie müssen die Benutzer auch innerhalb des Codes Controller autorisieren.
