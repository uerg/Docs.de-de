---
title: Artikel basierend auf Projekte mit Konten einzelner Benutzer erstellt
author: rick-anderson
description: Dieses Dokument listet Artikel basierend auf Projekte mit Konten einzelner Benutzer erstellt.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: aee18fa08fbc5c8452ca2b401d32858edaf55e7c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="b2b24-103">Artikel basierend auf Projekte mit Konten einzelner Benutzer erstellt</span><span class="sxs-lookup"><span data-stu-id="b2b24-103">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="b2b24-104">ASP.NET Core Identität ist in-Projektvorlagen in Visual Studio mit der Option "Einzelne Benutzerkonten" enthalten.</span><span class="sxs-lookup"><span data-stu-id="b2b24-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="b2b24-105">Die Authentifizierung Vorlagen stehen in .NET Core CLI mit `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="b2b24-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="b2b24-106">In den folgenden Artikeln veranschaulichen, wie den Code in ASP.NET Core Vorlagen, mit denen einzelne Benutzerkonten generiert:</span><span class="sxs-lookup"><span data-stu-id="b2b24-106">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="b2b24-107">Zweistufige Authentifizierung mit SMS</span><span class="sxs-lookup"><span data-stu-id="b2b24-107">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="b2b24-108">Account confirmation and password recovery in ASP.NET Core (Kontobestätigung und Kennwortwiederherstellung in ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="b2b24-108">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="b2b24-109">Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt</span><span class="sxs-lookup"><span data-stu-id="b2b24-109">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
