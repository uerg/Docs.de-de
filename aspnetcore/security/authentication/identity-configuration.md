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
