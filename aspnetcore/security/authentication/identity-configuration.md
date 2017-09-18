---
title: "Konfigurieren von ASP.NET Core Identität"
author: AdrienTorris
description: "Verstehen Sie die ASP.NET Core Identity Standardwerte, und konfigurieren Sie die verschiedenen Identitätseigenschaften, um benutzerdefinierte Werte verwenden."
keywords: "ASP.NET Core, Identität und Authentifizierung, Sicherheit"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 629fcc2941b2d2fda9604a3eac04be3d0f5294b2
ms.sourcegitcommit: ddefc78270bd9b5ae0b1bd8de6c45f6977e7dceb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2017
---
# <a name="configure-identity"></a><span data-ttu-id="83b88-104">Konfigurieren von Identität</span><span class="sxs-lookup"><span data-stu-id="83b88-104">Configure Identity</span></span>

<span data-ttu-id="83b88-105">ASP.NET Core Identität verfügt über einige Standardverhaltensweisen, die Sie in Ihrer Anwendung Startklasse leicht überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="83b88-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's startup class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="83b88-106">Kennwörter-Richtlinie</span><span class="sxs-lookup"><span data-stu-id="83b88-106">Passwords policy</span></span>

<span data-ttu-id="83b88-107">Standardmäßig erfordert Identität an, dass Kennwörter einen Großbuchstaben, Kleinbuchstaben und Ziffern enthalten.</span><span class="sxs-lookup"><span data-stu-id="83b88-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, and digits.</span></span> <span data-ttu-id="83b88-108">Es gibt auch einige andere Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="83b88-108">There are also some other restrictions.</span></span> <span data-ttu-id="83b88-109">Wenn Sie kennworteinschränkungen vereinfachen möchten, können Sie dies tun, in der Anwendung die Startklasse.</span><span class="sxs-lookup"><span data-stu-id="83b88-109">If you want to simplify password restrictions, you can do that in the startup class of your application.</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a><span data-ttu-id="83b88-110">Cookie-Einstellungen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="83b88-110">Application's cookie settings</span></span>

<span data-ttu-id="83b88-111">Wie die Richtlinie Kennwörter können alle Einstellungen des Cookies für die Anwendung in die Startklasse geändert werden.</span><span class="sxs-lookup"><span data-stu-id="83b88-111">Like the passwords policy, all the settings of the application's cookie can be changed in the startup class.</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a><span data-ttu-id="83b88-112">Benutzersperre</span><span class="sxs-lookup"><span data-stu-id="83b88-112">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
