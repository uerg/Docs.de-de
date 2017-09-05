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
# <a name="view-based-authorization"></a><span data-ttu-id="0747d-103">View-basierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="0747d-103">View Based Authorization</span></span>

<a name=security-authorization-views></a>

<span data-ttu-id="0747d-104">Häufig wird ein Entwickler möchte einblenden, ausblenden oder ändern Sie andernfalls eine Benutzeroberfläche, die basierend auf der Identität des aktuellen Benutzers.</span><span class="sxs-lookup"><span data-stu-id="0747d-104">Often a developer will want to show, hide or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="0747d-105">Sie können dem Prüfdienst in MVC-Ansichten über zugreifen [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0747d-105">You can access the authorization service within MVC views via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection).</span></span> <span data-ttu-id="0747d-106">Dem Prüfdienst in einer Razor-Verwendung anzeigen Einfügen der `@inject` , z. B. die Richtlinie `@inject IAuthorizationService AuthorizationService` (erfordert `@using Microsoft.AspNetCore.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="0747d-106">To inject the authorization service into a Razor view use the `@inject` directive, for example `@inject IAuthorizationService AuthorizationService` (requires `@using Microsoft.AspNetCore.Authorization`).</span></span> <span data-ttu-id="0747d-107">Legen Sie ggf. dem Prüfdienst in jeder Ansicht der `@inject` -Direktive in der `_ViewImports.cshtml` in der Datei die `Views` Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="0747d-107">If you want the authorization service in every view then place the `@inject` directive into the `_ViewImports.cshtml` file in the `Views` directory.</span></span> <span data-ttu-id="0747d-108">Weitere Informationen zu Abhängigkeitsinjektion in Sichten finden Sie unter [Abhängigkeitsinjektion in Sichten](../../mvc/views/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="0747d-108">For more information on dependency injection into views see [Dependency injection into views](../../mvc/views/dependency-injection.md).</span></span>

<span data-ttu-id="0747d-109">Nachdem Sie dem Prüfdienst eingefügt haben verwenden Sie ihn durch Aufrufen der `AuthorizeAsync` Methode in genau dieselbe Weise wie Sie während der überprüfen würde [ressourcenbasierten Autorisierung](resourcebased.md#security-authorization-resource-based-imperative).</span><span class="sxs-lookup"><span data-stu-id="0747d-109">Once you have injected the authorization service you use it by calling the `AuthorizeAsync` method in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative).</span></span>

```csharp
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
   {
       <p>This paragraph is displayed because you fulfilled PolicyName.</p>
   }
   ```

<span data-ttu-id="0747d-110">In einigen Fällen werden die Ressourcen Ihrer Ansichtsmodell, und Sie können Aufrufen `AuthorizeAsync` in genau dieselbe Weise wie Sie während der überprüfen würde [ressourcenbasierten Autorisierung](resourcebased.md#security-authorization-resource-based-imperative);</span><span class="sxs-lookup"><span data-stu-id="0747d-110">In some cases the resource will be your view model, and you can call `AuthorizeAsync` in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative);</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0747d-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0747d-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
   @if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0747d-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0747d-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
   @if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```
---

<span data-ttu-id="0747d-113">Hier sehen Sie sich, dass das Modell übergeben wird, wie die Ressource Autorisierung berücksichtigt werden sollten.</span><span class="sxs-lookup"><span data-stu-id="0747d-113">Here you can see the model is passed as the resource authorization should take into consideration.</span></span>

>[!WARNING]
><span data-ttu-id="0747d-114">Verlassen Sie sich nicht auf den ein- oder Ausblenden der Teile der Benutzeroberfläche als einzige Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="0747d-114">Do not rely on showing or hiding parts of your UI as your only authorization method.</span></span> <span data-ttu-id="0747d-115">Ausblenden einer Benutzeroberflächenautomatisierungs bedeutet Element nicht, dass ein Benutzer zugreifen kann.</span><span class="sxs-lookup"><span data-stu-id="0747d-115">Hiding a UI element does not mean a user cannot access it.</span></span> <span data-ttu-id="0747d-116">Sie müssen die Benutzer auch innerhalb des Codes Controller autorisieren.</span><span class="sxs-lookup"><span data-stu-id="0747d-116">You must also authorize the user within your controller code.</span></span>
