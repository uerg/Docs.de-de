---
title: "Konfigurieren Sie die Primärschlüsseldaten Identitätstyp"
author: AdrienTorris
description: "In diesem Artikel werden die Schritte zum Konfigurieren des gewünschten Datentyps für den Primärschlüssel ASP.NET Core Identity verwendet."
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: ff1c3aff3ea833081a25ea5fc4f2c2b65823f536
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a>Konfigurieren der ASP.NET Core Primärschlüsseldaten Identitätstyp

ASP.NET Core Identität können Sie so konfigurieren Sie den Datentyp verwendet, um einen Primärschlüssel darstellen. Identität verwendet die `string` -Datentyp in der Standardeinstellung. Sie können dieses Verhalten überschreiben.

## <a name="customize-the-primary-key-data-type"></a>Anpassen des Primärschlüsseldaten-Typs

1. Erstellen Sie eine benutzerdefinierte Implementierung von der [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) Klasse. Es stellt den Typ zum Erstellen von Benutzerobjekten verwendet werden soll. Im folgenden Beispiel, das standardmäßige `string` Typ wird mit ersetzt `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. Erstellen Sie eine benutzerdefinierte Implementierung von der [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) Klasse. Er den Typ darstellt, für das Role-Objekte erstellen. Im folgenden Beispiel, das standardmäßige `string` Typ wird mit ersetzt `Guid`.
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. Erstellen Sie eine benutzerdefinierte Datenbank Context-Klasse. Es erbt von der Entity Framework Database Context-Klasse für die Identität verwendet. Die `TUser` und `TRole` Argumente verweisen, die benutzerdefinierte Benutzer und die Rolle Klassen, die jeweils im vorherigen Schritt erstellt. Die `Guid` -Datentyp für den Primärschlüssel definiert ist.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. Registrieren Sie benutzerdefinierte Datenbank Context-Klasse, wenn die Identity-Dienst in der app-Start-Klasse hinzufügen.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    Die `AddEntityFrameworkStores` Methode nicht akzeptieren ein `TKey` Argument als es wurde in ASP.NET Core 1.x. Der Primärschlüssel-Datentyp wird abgeleitet, durch die Analyse der `DbContext` Objekt.
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    Die `AddEntityFrameworkStores` Methode akzeptiert eine `TKey` Arguments, den primären Schlüssel den Datentyp angibt.
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a>Testen Sie die Änderungen

Nach Abschluss der konfigurationsänderungen gibt die Eigenschaft, die den Primärschlüssel darstellt. der neue Datentyp wieder. Das folgende Beispiel veranschaulicht den Zugriff auf die Eigenschaft in einer MVC-Controller.

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
