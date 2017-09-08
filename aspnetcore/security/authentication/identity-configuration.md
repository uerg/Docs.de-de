---
title: "Konfigurieren von Identität"
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity"></a><span data-ttu-id="d35e9-102">Konfigurieren von Identität</span><span class="sxs-lookup"><span data-stu-id="d35e9-102">Configure Identity</span></span>

<span data-ttu-id="d35e9-103">ASP.NET Core Identität verfügt über einige Standardverhaltensweisen, die Sie in Ihrer Anwendung Startklasse leicht überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="d35e9-103">ASP.NET Core Identity has some default behaviors that you can override easily in your application's startup class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="d35e9-104">Kennwörter-Richtlinie</span><span class="sxs-lookup"><span data-stu-id="d35e9-104">Passwords policy</span></span>

<span data-ttu-id="d35e9-105">Standardmäßig erfordert Identität an, dass Kennwörter einen Großbuchstaben, Kleinbuchstaben und Ziffern enthalten.</span><span class="sxs-lookup"><span data-stu-id="d35e9-105">By default, Identity requires that passwords contain an uppercase character, lowercase character, and digits.</span></span> <span data-ttu-id="d35e9-106">Es gibt auch einige andere Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="d35e9-106">There are also some other restrictions.</span></span> <span data-ttu-id="d35e9-107">Wenn Sie kennworteinschränkungen vereinfachen möchten, können Sie dies tun, in der Anwendung die Startklasse.</span><span class="sxs-lookup"><span data-stu-id="d35e9-107">If you want to simplify password restrictions, you can do that in the startup class of your application.</span></span>

<span data-ttu-id="d35e9-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span><span class="sxs-lookup"><span data-stu-id="d35e9-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="d35e9-109">Cookie-Einstellungen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="d35e9-109">Application's cookie settings</span></span>

<span data-ttu-id="d35e9-110">Wie die Richtlinie Kennwörter können alle Einstellungen des Cookies für die Anwendung in die Startklasse geändert werden.</span><span class="sxs-lookup"><span data-stu-id="d35e9-110">Like the passwords policy, all the settings of the application's cookie can be changed in the startup class.</span></span>

<span data-ttu-id="d35e9-111">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span><span class="sxs-lookup"><span data-stu-id="d35e9-111">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span></span>

## <a name="users-lockout"></a><span data-ttu-id="d35e9-112">Benutzersperre</span><span class="sxs-lookup"><span data-stu-id="d35e9-112">User's lockout</span></span>

<span data-ttu-id="d35e9-113">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span><span class="sxs-lookup"><span data-stu-id="d35e9-113">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span></span>
