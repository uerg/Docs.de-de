---
title: Artikel basierend auf Projekte mit Konten einzelner Benutzer erstellt
author: rick-anderson
description: Dieses Dokument listet Artikel basierend auf Projekte mit Konten einzelner Benutzer erstellt.
keywords: ASP.NET Core, Autorisierung, IAuthorizationService
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 1864625e0ad6b4ec6fc2ada3fa7d93edec91b633
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2017
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="37cd2-104">Artikel basierend auf Projekte mit Konten einzelner Benutzer erstellt</span><span class="sxs-lookup"><span data-stu-id="37cd2-104">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="37cd2-105">ASP.NET Core Identität ist in-Projektvorlagen in Visual Studio mit der Option "Einzelne Benutzerkonten" enthalten.</span><span class="sxs-lookup"><span data-stu-id="37cd2-105">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="37cd2-106">Die Authentifizierung Vorlagen stehen in .NET Core CLI mit `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="37cd2-106">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="37cd2-107">In den folgenden Artikeln veranschaulichen, wie den Code in ASP.NET Core Vorlagen, mit denen einzelne Benutzerkonten generiert:</span><span class="sxs-lookup"><span data-stu-id="37cd2-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="37cd2-108">Zweistufige Authentifizierung mit SMS</span><span class="sxs-lookup"><span data-stu-id="37cd2-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="37cd2-109">Account confirmation and password recovery in ASP.NET Core (Kontobestätigung und Kennwortwiederherstellung in ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="37cd2-109">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="37cd2-110">Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt</span><span class="sxs-lookup"><span data-stu-id="37cd2-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)