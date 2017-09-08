---
title: "Konfigurieren von Identität Primärschlüssel-Datentyp"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: e6661708d003aa50204e7f79d3070442a3440391
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity-primary-keys-data-type"></a>Konfigurieren von Identität Primärschlüssel-Datentyp

ASP.NET Core Identität können Sie einfach den Datentyp zu konfigurieren, den Sie für den Primärschlüssel verwendet werden soll. Standardmäßig verwendet Identität Zeichenfolgendatentyp jedoch sehr schnell können Sie dieses Verhalten überschreiben.

## <a name="how-to"></a>Gewusst wie

1.  Der erste Schritt ist zum Implementieren der Identitäts-Modell und den String-Datentyp mit dem gewünschten Datentyp zu überschreiben.

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  Implementieren Sie den Datenbankkontext der Identität mit Modellen und den Datentyp, den Sie für Primärschlüssel verwendet werden soll

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  Verwenden Sie die Modelle und neuen Datentyps an für Primärschlüssel verwendet, sollen Wenn Sie die Identity-Dienst in Ihrer Anwendung Startklasse deklarieren

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
