---
title: Artikel basierend auf ASP.NET Core-Projekte mit Konten einzelner Benutzer erstellt
author: rick-anderson
description: Ermitteln Sie Artikel basierend auf ASP.NET Core-Projekte mit Konten einzelner Benutzer erstellt.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 6d05cd8c0ee6c9029eb9bbafc15d9b19ee7633de
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252021"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="77d00-103">Artikel basierend auf ASP.NET Core-Projekte mit Konten einzelner Benutzer erstellt</span><span class="sxs-lookup"><span data-stu-id="77d00-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="77d00-104">ASP.NET Core Identität ist in-Projektvorlagen in Visual Studio mit der Option "Einzelne Benutzerkonten" enthalten.</span><span class="sxs-lookup"><span data-stu-id="77d00-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="77d00-105">Die Authentifizierung Vorlagen stehen in .NET Core CLI mit `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="77d00-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="77d00-107">In den folgenden Artikeln veranschaulichen, wie den Code in ASP.NET Core Vorlagen, mit denen einzelne Benutzerkonten generiert:</span><span class="sxs-lookup"><span data-stu-id="77d00-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="77d00-108">Zweistufige Authentifizierung mit SMS</span><span class="sxs-lookup"><span data-stu-id="77d00-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="77d00-109">Account confirmation and password recovery in ASP.NET Core (Kontobestätigung und Kennwortwiederherstellung in ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="77d00-109">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="77d00-110">Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt</span><span class="sxs-lookup"><span data-stu-id="77d00-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
