---
title: Sicherheit
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: f173d03f55a1ce52222a75c023f9e8a20d5c60dc
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2017
---
# <a name="security"></a>Sicherheit

*   [Authentifizierung](authentication/index.md)
    *   [Einführung in Identity](authentication/identity.md)
    *   [Aktivieren der Authentifizierung mithilfe von Facebook, Google und anderen externen Anbietern](authentication/social/index.md)
    * [Konfigurieren der Windows-Authentifizierung](authentication/windowsauth.md)
    *   [Kontobestätigung und Kennwortwiederherstellung](authentication/accconfirm.md)
    *   [Zweistufige Authentifizierung mit SMS](authentication/2fa.md) 
    *   [Verwenden der Cookieauthentifizierung ohne ASP.NET Core Identity](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Integrieren von Azure AD in eine ASP.NET Core-Web-App](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Aufrufen einer ASP.NET Core-Web-API aus einer WPF-Anwendung mithilfe von Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Aufrufen einer Web-API in einer ASP.NET Core-Webanwendung mithilfe von Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Eine ASP.NET Core-Web-App in Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Sichern von ASP.NET Core-Apps mit IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorisierung](authorization/index.md)
    *   [Introduction (Einführung)](authorization/introduction.md)
    *   [Erstellen einer App mit Benutzerdaten, die durch Autorisierung geschützt sind](xref:security/authorization/secure-data)
    *   [Einfache Autorisierung](authorization/simple.md)
    *   [Rollenbasierte Autorisierung](authorization/roles.md)
    *   [Anspruchsbasierte Autorisierung](authorization/claims.md)
    *   [Benutzerdefinierte, richtlinienbasierende Autorisierung](authorization/policies.md)
    *   [Abhängigkeitsinjektion in Anforderungshandlern](authorization/dependencyinjection.md)
    *   [Ressourcenbasierte Autorisierung](authorization/resourcebased.md)
    *   [Ansehen der rollenbasierten Autorisierung](authorization/views.md)
    *   [Einschränken der Identität nach Schema](authorization/limitingidentitybyscheme.md)
*   [Schutz von Daten](data-protection/index.md)
    *   [Einführung in den Schutz von Daten](data-protection/introduction.md)
    *   [Erste Schritte mit Datenschutz-APIs](data-protection/using-data-protection.md)
    *   [Consumer-APIs](data-protection/consumer-apis/index.md)
        *   [Übersicht über Consumer-APIs](data-protection/consumer-apis/overview.md)
        *   [Zweckzeichenfolgen](data-protection/consumer-apis/purpose-strings.md)
        *   [Zweckhierarchie und Mehrinstanzenfähigkeit](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [Kennwort-Hashing](data-protection/consumer-apis/password-hashing.md)
        *   [Beschränken der Lebensdauer von geschützten Nutzlasten](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [Aufheben des Schutzes von Nutzlasten, deren Schlüssel gesperrt wurden](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [Konfiguration](data-protection/configuration/index.md)
        *   [Konfigurieren des Schutzes von Daten](data-protection/configuration/overview.md)
        *   [Standardeinstellungen](data-protection/configuration/default-settings.md)
        *   [Computerübergreifende Richtlinie](data-protection/configuration/machine-wide-policy.md)
        *   [Szenarios, die die Abhängigkeitsinjektion nicht beachten](data-protection/configuration/non-di-scenarios.md)
    *   [Erweiterbarkeits-APIs](data-protection/extensibility/index.md)
        *   [Kryptografieerweiterbarkeit in Core](data-protection/extensibility/core-crypto.md)
        *   [Schlüsselverwaltungserweiterbarkeit](data-protection/extensibility/key-management.md)
        *   [Verschiedene APIs](data-protection/extensibility/misc-apis.md)
    *   [Implementation (Implementierung)](data-protection/implementation/index.md)
        *   [Authentifizierte Verschlüsselungsdetails](data-protection/implementation/authenticated-encryption-details.md)
        *   [Unterschlüsselableitung und authentifizierte Verschlüsselung](data-protection/implementation/subkeyderivation.md)
        *   [Kontextheader](data-protection/implementation/context-headers.md)
        *   [Schlüsselverwaltung](data-protection/implementation/key-management.md)
        *   [Schlüsselspeicheranbieter](data-protection/implementation/key-storage-providers.md)
        *   [Ruhende Verschlüsselung von Schlüsseln](data-protection/implementation/key-encryption-at-rest.md)
        *   [Unveränderlichkeit von Schlüsseln und Ändern von Einstellungen](data-protection/implementation/key-immutability.md)
        *   [Schlüsselspeicherformat](data-protection/implementation/key-storage-format.md)
        *   [Kurzlebige Datenschutzanbieter](data-protection/implementation/key-storage-ephemeral.md)
    *   [Kompatibilität](data-protection/compatibility/index.md)
        *   [Freigeben von Cookies zwischen Anwendungen](data-protection/compatibility/cookie-sharing.md)
        *   [Ersetzen von <machineKey> in ASP.NET](data-protection/compatibility/replacing-machinekey.md)
*   [Erstellen einer App mit Benutzerdaten, die durch Autorisierung geschützt sind](xref:security/authorization/secure-data)
*   [Sicheres Speichern geheimer App-Schlüssel während der Entwicklung](app-secrets.md)
*   [Azure Key Vault-Konfigurationsanbieter](key-vault-configuration.md)
*   [Erzwingen von SSL](enforcing-ssl.md)
*   [Antianforderungsfälschung](anti-request-forgery.md)
*   [Verhindern von offenen Weiterleitungsangriffen](preventing-open-redirects.md)
*   [Verhindern von siteübergreifendem Skripting](cross-site-scripting.md)
*   [Aktivieren ursprungsübergreifender Anforderungen (CORS)](cors.md)
