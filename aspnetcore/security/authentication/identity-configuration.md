---
title: "Konfigurieren von Identität"
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity"></a>Konfigurieren von Identität

ASP.NET Core Identität verfügt über einige Standardverhaltensweisen, die Sie in Ihrer Anwendung Startklasse leicht überschreiben können.

## <a name="passwords-policy"></a>Kennwörter-Richtlinie

Standardmäßig erfordert Identität an, dass Kennwörter einen Großbuchstaben, Kleinbuchstaben und Ziffern enthalten. Es gibt auch einige andere Einschränkungen. Wenn Sie kennworteinschränkungen vereinfachen möchten, können Sie dies tun, in der Anwendung die Startklasse.

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a>Cookie-Einstellungen der Anwendung

Wie die Richtlinie Kennwörter können alle Einstellungen des Cookies für die Anwendung in die Startklasse geändert werden.

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a>Benutzersperre

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
