---
title: "Konfigurieren von Identität Primärschlüssel-Datentyp"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: e6661708d003aa50204e7f79d3070442a3440391
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="19f17-102">Konfigurieren von Identität Primärschlüssel-Datentyp</span><span class="sxs-lookup"><span data-stu-id="19f17-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="19f17-103">ASP.NET Core Identität können Sie einfach den Datentyp zu konfigurieren, den Sie für den Primärschlüssel verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="19f17-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="19f17-104">Standardmäßig verwendet Identität Zeichenfolgendatentyp jedoch sehr schnell können Sie dieses Verhalten überschreiben.</span><span class="sxs-lookup"><span data-stu-id="19f17-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="19f17-105">Gewusst wie</span><span class="sxs-lookup"><span data-stu-id="19f17-105">How to</span></span>

1.  <span data-ttu-id="19f17-106">Der erste Schritt ist zum Implementieren der Identitäts-Modell und den String-Datentyp mit dem gewünschten Datentyp zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="19f17-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    <span data-ttu-id="19f17-107">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span><span class="sxs-lookup"><span data-stu-id="19f17-107">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span></span>

    <span data-ttu-id="19f17-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span><span class="sxs-lookup"><span data-stu-id="19f17-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span></span>
    
2.  <span data-ttu-id="19f17-109">Implementieren Sie den Datenbankkontext der Identität mit Modellen und den Datentyp, den Sie für Primärschlüssel verwendet werden soll</span><span class="sxs-lookup"><span data-stu-id="19f17-109">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    <span data-ttu-id="19f17-110">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span><span class="sxs-lookup"><span data-stu-id="19f17-110">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span></span>
    
3.  <span data-ttu-id="19f17-111">Verwenden Sie die Modelle und neuen Datentyps an für Primärschlüssel verwendet, sollen Wenn Sie die Identity-Dienst in Ihrer Anwendung Startklasse deklarieren</span><span class="sxs-lookup"><span data-stu-id="19f17-111">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    <span data-ttu-id="19f17-112">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span><span class="sxs-lookup"><span data-stu-id="19f17-112">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span></span>
