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
ms.openlocfilehash: e8045b8804d1e7915cd81d697d43a173e33a7119
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="security"></a><span data-ttu-id="f74bb-103">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="f74bb-103">Security</span></span>

*   [<span data-ttu-id="f74bb-104">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="f74bb-104">Authentication</span></span>](authentication/index.md)
    *   [<span data-ttu-id="f74bb-105">Einführung in Identity</span><span class="sxs-lookup"><span data-stu-id="f74bb-105">Introduction to Identity</span></span>](authentication/identity.md)
    *   [<span data-ttu-id="f74bb-106">Aktivieren der Authentifizierung mithilfe von Facebook, Google und anderen externen Anbietern</span><span class="sxs-lookup"><span data-stu-id="f74bb-106">Enabling authentication using Facebook, Google and other external providers</span></span>](authentication/social/index.md)
    * [<span data-ttu-id="f74bb-107">Konfigurieren der Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="f74bb-107">Configure Windows Authentication</span></span>](authentication/windowsauth.md)
    *   [<span data-ttu-id="f74bb-108">Kontobestätigung und Kennwortwiederherstellung</span><span class="sxs-lookup"><span data-stu-id="f74bb-108">Account Confirmation and Password Recovery</span></span>](authentication/accconfirm.md)
    *   [<span data-ttu-id="f74bb-109">Zweistufige Authentifizierung mit SMS</span><span class="sxs-lookup"><span data-stu-id="f74bb-109">Two-factor authentication with SMS</span></span>](authentication/2fa.md) 
    *   [<span data-ttu-id="f74bb-110">Verwenden der Cookieauthentifizierung ohne ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="f74bb-110">Using Cookie Authentication without ASP.NET Core Identity</span></span>](authentication/cookie.md)
    *   [<span data-ttu-id="f74bb-111">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f74bb-111">Azure Active Directory</span></span>](authentication/azure-active-directory/index.md)
        *   [<span data-ttu-id="f74bb-112">Integrieren von Azure AD in eine ASP.NET Core-Web-App</span><span class="sxs-lookup"><span data-stu-id="f74bb-112">Integrating Azure AD Into an ASP.NET Core Web App</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="f74bb-113">Aufrufen einer ASP.NET Core-Web-API aus einer WPF-Anwendung mithilfe von Azure AD</span><span class="sxs-lookup"><span data-stu-id="f74bb-113">Calling a ASP.NET Core Web API From a WPF Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [<span data-ttu-id="f74bb-114">Aufrufen einer Web-API in einer ASP.NET Core-Webanwendung mithilfe von Azure AD</span><span class="sxs-lookup"><span data-stu-id="f74bb-114">Calling a Web API in an ASP.NET Core Web Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="f74bb-115">Eine ASP.NET Core-Web-App in Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="f74bb-115">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="f74bb-116">Sichern von ASP.NET Core-Apps mit IdentityServer4</span><span class="sxs-lookup"><span data-stu-id="f74bb-116">Securing ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="f74bb-117">Autorisierung</span><span class="sxs-lookup"><span data-stu-id="f74bb-117">Authorization</span></span>](authorization/index.md)
    *   [<span data-ttu-id="f74bb-118">Introduction (Einführung)</span><span class="sxs-lookup"><span data-stu-id="f74bb-118">Introduction</span></span>](authorization/introduction.md)
    *   [<span data-ttu-id="f74bb-119">Erstellen einer App mit Benutzerdaten, die durch Autorisierung geschützt sind</span><span class="sxs-lookup"><span data-stu-id="f74bb-119">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="f74bb-120">Einfache Autorisierung</span><span class="sxs-lookup"><span data-stu-id="f74bb-120">Simple Authorization</span></span>](authorization/simple.md)
    *   [<span data-ttu-id="f74bb-121">Rollenbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="f74bb-121">Role based Authorization</span></span>](authorization/roles.md)
    *   [<span data-ttu-id="f74bb-122">Anspruchsbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="f74bb-122">Claims-Based Authorization</span></span>](authorization/claims.md)
    *   [<span data-ttu-id="f74bb-123">Benutzerdefinierte, richtlinienbasierende Autorisierung</span><span class="sxs-lookup"><span data-stu-id="f74bb-123">Custom Policy-Based Authorization</span></span>](authorization/policies.md)
    *   [<span data-ttu-id="f74bb-124">Abhängigkeitsinjektion in Anforderungshandlern</span><span class="sxs-lookup"><span data-stu-id="f74bb-124">Dependency Injection in requirement handlers</span></span>](authorization/dependencyinjection.md)
    *   [<span data-ttu-id="f74bb-125">Ressourcenbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="f74bb-125">Resource Based Authorization</span></span>](authorization/resourcebased.md)
    *   [<span data-ttu-id="f74bb-126">Ansehen der rollenbasierten Autorisierung</span><span class="sxs-lookup"><span data-stu-id="f74bb-126">View Based Authorization</span></span>](authorization/views.md)
    *   [<span data-ttu-id="f74bb-127">Einschränken der Identität nach Schema</span><span class="sxs-lookup"><span data-stu-id="f74bb-127">Limiting identity by scheme</span></span>](authorization/limitingidentitybyscheme.md)
*   [<span data-ttu-id="f74bb-128">Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="f74bb-128">Data Protection</span></span>](data-protection/index.md)
    *   [<span data-ttu-id="f74bb-129">Einführung in den Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="f74bb-129">Introduction to Data Protection</span></span>](data-protection/introduction.md)
    *   [<span data-ttu-id="f74bb-130">Erste Schritte mit Datenschutz-APIs</span><span class="sxs-lookup"><span data-stu-id="f74bb-130">Getting Started with the Data Protection APIs</span></span>](data-protection/using-data-protection.md)
    *   [<span data-ttu-id="f74bb-131">Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="f74bb-131">Consumer APIs</span></span>](data-protection/consumer-apis/index.md)
        *   [<span data-ttu-id="f74bb-132">Übersicht über Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="f74bb-132">Consumer APIs Overview</span></span>](data-protection/consumer-apis/overview.md)
        *   [<span data-ttu-id="f74bb-133">Zweckzeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="f74bb-133">Purpose Strings</span></span>](data-protection/consumer-apis/purpose-strings.md)
        *   [<span data-ttu-id="f74bb-134">Zweckhierarchie und Mehrinstanzenfähigkeit</span><span class="sxs-lookup"><span data-stu-id="f74bb-134">Purpose hierarchy and multi-tenancy</span></span>](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [<span data-ttu-id="f74bb-135">Kennwort-Hashing</span><span class="sxs-lookup"><span data-stu-id="f74bb-135">Password Hashing</span></span>](data-protection/consumer-apis/password-hashing.md)
        *   [<span data-ttu-id="f74bb-136">Beschränken der Lebensdauer von geschützten Nutzlasten</span><span class="sxs-lookup"><span data-stu-id="f74bb-136">Limiting the lifetime of protected payloads</span></span>](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [<span data-ttu-id="f74bb-137">Aufheben des Schutzes von Nutzlasten, deren Schlüssel gesperrt wurden</span><span class="sxs-lookup"><span data-stu-id="f74bb-137">Unprotecting payloads whose keys have been revoked</span></span>](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [<span data-ttu-id="f74bb-138">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="f74bb-138">Configuration</span></span>](data-protection/configuration/index.md)
        *   [<span data-ttu-id="f74bb-139">Konfigurieren des Schutzes von Daten</span><span class="sxs-lookup"><span data-stu-id="f74bb-139">Configuring Data Protection</span></span>](data-protection/configuration/overview.md)
        *   [<span data-ttu-id="f74bb-140">Standardeinstellungen</span><span class="sxs-lookup"><span data-stu-id="f74bb-140">Default Settings</span></span>](data-protection/configuration/default-settings.md)
        *   [<span data-ttu-id="f74bb-141">Computerübergreifende Richtlinie</span><span class="sxs-lookup"><span data-stu-id="f74bb-141">Machine Wide Policy</span></span>](data-protection/configuration/machine-wide-policy.md)
        *   [<span data-ttu-id="f74bb-142">Szenarios, die die Abhängigkeitsinjektion nicht beachten</span><span class="sxs-lookup"><span data-stu-id="f74bb-142">Non DI Aware Scenarios</span></span>](data-protection/configuration/non-di-scenarios.md)
    *   [<span data-ttu-id="f74bb-143">Erweiterbarkeits-APIs</span><span class="sxs-lookup"><span data-stu-id="f74bb-143">Extensibility APIs</span></span>](data-protection/extensibility/index.md)
        *   [<span data-ttu-id="f74bb-144">Kryptografieerweiterbarkeit in Core</span><span class="sxs-lookup"><span data-stu-id="f74bb-144">Core cryptography extensibility</span></span>](data-protection/extensibility/core-crypto.md)
        *   [<span data-ttu-id="f74bb-145">Schlüsselverwaltungserweiterbarkeit</span><span class="sxs-lookup"><span data-stu-id="f74bb-145">Key management extensibility</span></span>](data-protection/extensibility/key-management.md)
        *   [<span data-ttu-id="f74bb-146">Verschiedene APIs</span><span class="sxs-lookup"><span data-stu-id="f74bb-146">Miscellaneous APIs</span></span>](data-protection/extensibility/misc-apis.md)
    *   [<span data-ttu-id="f74bb-147">Implementation (Implementierung)</span><span class="sxs-lookup"><span data-stu-id="f74bb-147">Implementation</span></span>](data-protection/implementation/index.md)
        *   [<span data-ttu-id="f74bb-148">Authentifizierte Verschlüsselungsdetails</span><span class="sxs-lookup"><span data-stu-id="f74bb-148">Authenticated encryption details.</span></span>](data-protection/implementation/authenticated-encryption-details.md)
        *   [<span data-ttu-id="f74bb-149">Unterschlüsselableitung und authentifizierte Verschlüsselung</span><span class="sxs-lookup"><span data-stu-id="f74bb-149">Subkey Derivation and Authenticated Encryption</span></span>](data-protection/implementation/subkeyderivation.md)
        *   [<span data-ttu-id="f74bb-150">Kontextheader</span><span class="sxs-lookup"><span data-stu-id="f74bb-150">Context headers</span></span>](data-protection/implementation/context-headers.md)
        *   [<span data-ttu-id="f74bb-151">Schlüsselverwaltung</span><span class="sxs-lookup"><span data-stu-id="f74bb-151">Key Management</span></span>](data-protection/implementation/key-management.md)
        *   [<span data-ttu-id="f74bb-152">Schlüsselspeicheranbieter</span><span class="sxs-lookup"><span data-stu-id="f74bb-152">Key Storage Providers</span></span>](data-protection/implementation/key-storage-providers.md)
        *   [<span data-ttu-id="f74bb-153">Ruhende Verschlüsselung von Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="f74bb-153">Key Encryption At Rest</span></span>](data-protection/implementation/key-encryption-at-rest.md)
        *   [<span data-ttu-id="f74bb-154">Unveränderlichkeit von Schlüsseln und Ändern von Einstellungen</span><span class="sxs-lookup"><span data-stu-id="f74bb-154">Key Immutability and Changing Settings</span></span>](data-protection/implementation/key-immutability.md)
        *   [<span data-ttu-id="f74bb-155">Schlüsselspeicherformat</span><span class="sxs-lookup"><span data-stu-id="f74bb-155">Key Storage Format</span></span>](data-protection/implementation/key-storage-format.md)
        *   [<span data-ttu-id="f74bb-156">Kurzlebige Datenschutzanbieter</span><span class="sxs-lookup"><span data-stu-id="f74bb-156">Ephemeral data protection providers</span></span>](data-protection/implementation/key-storage-ephemeral.md)
    *   [<span data-ttu-id="f74bb-157">Kompatibilität</span><span class="sxs-lookup"><span data-stu-id="f74bb-157">Compatibility</span></span>](data-protection/compatibility/index.md)
        *   [<span data-ttu-id="f74bb-158">Freigeben von Cookies zwischen Anwendungen</span><span class="sxs-lookup"><span data-stu-id="f74bb-158">Sharing cookies between applications</span></span>](data-protection/compatibility/cookie-sharing.md)
        *   [<span data-ttu-id="f74bb-159">Ersetzen von <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f74bb-159">Replacing <machineKey> in ASP.NET</span></span>](data-protection/compatibility/replacing-machinekey.md)
*   [<span data-ttu-id="f74bb-160">Erstellen einer App mit Benutzerdaten, die durch Autorisierung geschützt sind</span><span class="sxs-lookup"><span data-stu-id="f74bb-160">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="f74bb-161">Sicheres Speichern geheimer App-Schlüssel während der Entwicklung</span><span class="sxs-lookup"><span data-stu-id="f74bb-161">Safe storage of app secrets during development</span></span>](app-secrets.md)
*   [<span data-ttu-id="f74bb-162">Azure Key Vault-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="f74bb-162">Azure Key Vault configuration provider</span></span>](key-vault-configuration.md)
*   [<span data-ttu-id="f74bb-163">Erzwingen von SSL</span><span class="sxs-lookup"><span data-stu-id="f74bb-163">Enforcing SSL</span></span>](enforcing-ssl.md)
*   [<span data-ttu-id="f74bb-164">Einrichten von HTTPS für die Entwicklung</span><span class="sxs-lookup"><span data-stu-id="f74bb-164">Setting up HTTPS for development</span></span>](https.md)
*   [<span data-ttu-id="f74bb-165">Antianforderungsfälschung</span><span class="sxs-lookup"><span data-stu-id="f74bb-165">Anti-Request Forgery</span></span>](anti-request-forgery.md)
*   [<span data-ttu-id="f74bb-166">Verhindern von offenen Weiterleitungsangriffen</span><span class="sxs-lookup"><span data-stu-id="f74bb-166">Preventing Open Redirect Attacks</span></span>](preventing-open-redirects.md)
*   [<span data-ttu-id="f74bb-167">Verhindern von siteübergreifendem Skripting</span><span class="sxs-lookup"><span data-stu-id="f74bb-167">Preventing Cross-Site Scripting</span></span>](cross-site-scripting.md)
*   [<span data-ttu-id="f74bb-168">Aktivieren ursprungsübergreifender Anforderungen (CORS)</span><span class="sxs-lookup"><span data-stu-id="f74bb-168">Enabling Cross-Origin Requests (CORS)</span></span>](cors.md)
