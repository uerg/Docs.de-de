---
title: "Übersicht über die ASP.NET Core-Sicherheit"
author: rachelappel
description: "Erfahren Sie mehr über die Grundlagen der Authentifizierung, Autorisierung und Sicherheit in ASP.NET Core."
manager: wpickett
ms.author: rachelap
ms.date: 11/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/index
ms.openlocfilehash: e03256d7b8b442569b0b0126983732c10817e20f
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="aspnet-core-security-overview"></a>ASP.NET Core-Sicherheit (Übersicht)

ASP.NET Core ermöglicht es Entwicklern, die Sicherheit ihrer Apps einfach zu konfigurieren und zu verwalten. ASP.NET Core enthält Features für die Verwaltung von Authentifizierung, Autorisierung, Datenschutz, SSL-Erzwingung, App-Geheimnissen, Schutz vor Anti Request Forgery und CORS-Verwaltung. Diese Sicherheitsfunktionen ermöglichen es Ihnen, robuste und dennoch sichere ASP.NET Core-Apps zu erstellen.

## <a name="aspnet-core-security-features"></a>ASP.NET Core-Sicherheitsfeatures

ASP.NET Core stellt zahlreiche Tools und Bibliotheken zur Verfügung, um Ihre Apps zu sichern (einschließlich integrierter Identitätsanbieter). Sie können jedoch auch Identitätsdienste von Drittanbietern wie Facebook, Twitter oder LinkedIn nutzen. Mit ASP.NET Core können Sie ganz einfach App-Geheimnisse verwalten, die eine Möglichkeit darstellen, vertrauliche Informationen zu speichern und zu verwenden, ohne sie im Code offenlegen zu müssen.

## <a name="authentication-vs-authorization"></a>Authentifizierung im Vergleich zu Autorisierung

Authentifizierung ist ein Vorgang, bei dem ein Benutzer Anmeldeinformationen bereitstellt, die dann mit den Angaben verglichen werden, die in einem Betriebssystem, einer Datenbank, einer App oder einer Ressource gespeichert sind. Wenn diese übereinstimmen, authentifizieren sich die Benutzer erfolgreich und können dann während eines Autorisierungsprozesses Aktionen ausführen, für die sie berechtigt sind. Die Autorisierung bezieht sich auf den Prozess, der festlegt, welche Aktionen ein Benutzer ausführen darf.

Eine weitere Möglichkeit, Authentifizierung zu definieren, besteht darin, sie als eine Möglichkeit zu betrachten, einen „Raum“ (z.B. einen Server, eine Datenbank, eine App oder eine Ressource) zu betreten, während die Autorisierung darin besteht, welche Aktionen der Benutzer mit welchen Objekten innerhalb dieses „Raums“ (Server, Datenbank oder App) ausführen kann.

## <a name="common-vulnerabilities-in-software"></a>Häufige Sicherheitsrisiken in Software

ASP.NET Core und EF enthalten Features, die Ihnen helfen, Ihre Anwendungen zu schützen und Sicherheitsverletzungen zu verhindern. Die folgende Liste von Links verweist auf die Dokumentation, die Techniken zur Vermeidung der häufigsten Sicherheitsrisiken in Web-Apps beschreibt:

* [XSS-Angriffe (Cross-Site Scripting)](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [Angriffe durch Einschleusung von SQL-Befehlen](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Websiteübergreifende Anforderungsfälschung (CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [Offene Weiterleitungsangriffe](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

Es gibt weitere Sicherheitsrisiken, die Sie kennen sollten. Weitere Informationen finden Sie im Abschnitt *ASP.NET Sicherheitsdokumentation* dieses Dokuments.

## <a name="aspnet-security-documentation"></a>ASP.NET-Sicherheitsdokumentation

*   [Authentifizierung](authentication/index.md)
    *   [Einführung in Identity](authentication/identity.md)
    *   [Aktivieren der Authentifizierung mithilfe von Facebook, Google und anderen externen Anbietern](authentication/social/index.md)
    *   [Aktivieren der Authentifizierung mit dem WS-Verbund](authentication/ws-federation.md)
    * [Konfigurieren der Windows-Authentifizierung](authentication/windowsauth.md)
    *   [Kontobestätigung und Kennwortwiederherstellung](authentication/accconfirm.md)
    *   [Zweistufige Authentifizierung mit SMS](authentication/2fa.md)
    *   [Verwenden der Cookieauthentifizierung ohne Identity](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Integrieren von Azure AD in eine ASP.NET Core-Web-App](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Aufrufen einer ASP.NET Core-Web-API aus einer WPF-App mithilfe von Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Aufrufen einer Web-API in einer ASP.NET Core-Web-App mithilfe von Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Eine ASP.NET Core-Web-App in Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Schützen von ASP.NET Core-Apps mit IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorisierung](authorization/index.md)
    *   [Introduction (Einführung)](authorization/introduction.md)
    *   [Erstellen einer App mit Benutzerdaten, die durch Autorisierung geschützt sind](xref:security/authorization/secure-data)
    *   [Einfache Autorisierung](authorization/simple.md)
    *   [Rollenbasierte Autorisierung](authorization/roles.md)
    *   [Anspruchsbasierte Autorisierung](authorization/claims.md)
    *   [Richtlinienbasierte Autorisierung](authorization/policies.md)
    *   [Abhängigkeitsinjektion in Anforderungshandlern](authorization/dependencyinjection.md)
    *   [Ressourcenbasierte Autorisierung](authorization/resourcebased.md)
    *   [Ansichtsbasierte Autorisierung](authorization/views.md)
    *   [Einschränken der Identität nach Schema](authorization/limitingidentitybyscheme.md)
*   [Schutz von Daten](data-protection/index.md)
    *   [Einführung in den Schutz von Daten](data-protection/introduction.md)
    *   [Erste Schritte mit Datenschutz-APIs](data-protection/using-data-protection.md)
    *   [Consumer-APIs](data-protection/consumer-apis/index.md)
        *   [Übersicht über Consumer-APIs](data-protection/consumer-apis/overview.md)
        *   [purpose-Zeichenfolgen](data-protection/consumer-apis/purpose-strings.md)
        *   [Zweckhierarchie und Mehrinstanzenfähigkeit](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [Kennworthashing](data-protection/consumer-apis/password-hashing.md)
        *   [Beschränken der Lebensdauer von geschützten Payloads](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [Aufheben des Schutzes von Payloads, deren Schlüssel gesperrt wurden](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [Konfiguration](data-protection/configuration/index.md)
        *   [Konfigurieren des Schutzes von Daten](data-protection/configuration/overview.md)
        *   [Standardeinstellungen](data-protection/configuration/default-settings.md)
        *   [Computerweite Richtlinie](data-protection/configuration/machine-wide-policy.md)
        *   [Szenarios ohne Möglichkeiten zur Abhängigkeitsinjektion](data-protection/configuration/non-di-scenarios.md)
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
        *   [Verschlüsselung ruhender Daten mit Schlüsseln](data-protection/implementation/key-encryption-at-rest.md)
        *   [Unveränderlichkeit von Schlüsseln und Ändern von Einstellungen](data-protection/implementation/key-immutability.md)
        *   [Schlüsselspeicherformat](data-protection/implementation/key-storage-format.md)
        *   [Kurzlebige Datenschutzanbieter](data-protection/implementation/key-storage-ephemeral.md)
    *   [Kompatibilität](data-protection/compatibility/index.md)
        *   [Ersetzen von <machineKey> in ASP.NET](data-protection/compatibility/replacing-machinekey.md)
*   [Erstellen einer App mit Benutzerdaten, die durch Autorisierung geschützt sind](xref:security/authorization/secure-data)
*   [Sicheres Speichern geheimer App-Schlüssel während der Entwicklung](app-secrets.md)
*   [Azure Key Vault-Konfigurationsanbieter](key-vault-configuration.md)
*   [Erzwingen von SSL](enforcing-ssl.md)
*   [Antianforderungsfälschung](anti-request-forgery.md)
*   [Verhindern von Angriffen durch offene Umleitungen](preventing-open-redirects.md)
*   [Verhindern von siteübergreifendem Scripting](cross-site-scripting.md)
*   [Aktivieren ursprungsübergreifender Anforderungen (CORS)](cors.md)
*   [Freigeben von Cookies für mehrere Apps](cookie-sharing.md)
