---
title: "Konfigurieren von Identität Primärschlüssel-Datentyp"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="6c363-102">Konfigurieren von Identität Primärschlüssel-Datentyp</span><span class="sxs-lookup"><span data-stu-id="6c363-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="6c363-103">ASP.NET Core Identität können Sie einfach den Datentyp zu konfigurieren, den Sie für den Primärschlüssel verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="6c363-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="6c363-104">Standardmäßig verwendet Identität Zeichenfolgendatentyp jedoch sehr schnell können Sie dieses Verhalten überschreiben.</span><span class="sxs-lookup"><span data-stu-id="6c363-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="6c363-105">Gewusst wie</span><span class="sxs-lookup"><span data-stu-id="6c363-105">How to</span></span>

1.  <span data-ttu-id="6c363-106">Der erste Schritt ist zum Implementieren der Identitäts-Modell und den String-Datentyp mit dem gewünschten Datentyp zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="6c363-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  <span data-ttu-id="6c363-107">Implementieren Sie den Datenbankkontext der Identität mit Modellen und den Datentyp, den Sie für Primärschlüssel verwendet werden soll</span><span class="sxs-lookup"><span data-stu-id="6c363-107">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  <span data-ttu-id="6c363-108">Verwenden Sie die Modelle und neuen Datentyps an für Primärschlüssel verwendet, sollen Wenn Sie die Identity-Dienst in Ihrer Anwendung Startklasse deklarieren</span><span class="sxs-lookup"><span data-stu-id="6c363-108">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
