---
title: Übersicht über die ASP.NET Core-Sicherheit
author: rachelappel
description: Erfahren Sie mehr über die Grundlagen der Authentifizierung, Autorisierung und Sicherheit in ASP.NET Core.
manager: wpickett
ms.author: rachelap
ms.date: 11/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/index
ms.openlocfilehash: da3829b2d5ae5db1861c7423da5afc7acbee6697
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="overview-of-aspnet-core-security"></a>Übersicht über die ASP.NET Core-Sicherheit

ASP.NET Core ermöglicht es Entwicklern, die Sicherheit ihrer Apps einfach zu konfigurieren und zu verwalten. ASP.NET Core enthält Features für die Verwaltung von Authentifizierung, Autorisierung, Datenschutz, SSL-Erzwingung, App-Geheimnissen, Schutz vor Anti Request Forgery und CORS-Verwaltung. Diese Sicherheitsfunktionen ermöglichen es Ihnen, robuste und dennoch sichere ASP.NET Core-Apps zu erstellen.

## <a name="aspnet-core-security-features"></a>ASP.NET Core-Sicherheitsfeatures

ASP.NET Core stellt zahlreiche Tools und Bibliotheken zur Verfügung, um Ihre Apps zu sichern (einschließlich integrierter Identitätsanbieter). Sie können jedoch auch Identitätsdienste von Drittanbietern wie Facebook, Twitter oder LinkedIn nutzen. Mit ASP.NET Core können Sie ganz einfach App-Geheimnisse verwalten, die eine Möglichkeit darstellen, vertrauliche Informationen zu speichern und zu verwenden, ohne sie im Code offenlegen zu müssen.

## <a name="authentication-vs-authorization"></a>Authentifizierung im Vergleich zu Autorisierung

Authentifizierung ist ein Vorgang, bei dem ein Benutzer Anmeldeinformationen bereitstellt, die dann mit den Angaben verglichen werden, die in einem Betriebssystem, einer Datenbank, einer App oder einer Ressource gespeichert sind. Wenn diese übereinstimmen, authentifizieren sich die Benutzer erfolgreich und können dann während eines Autorisierungsprozesses Aktionen ausführen, für die sie berechtigt sind. Die Autorisierung bezieht sich auf den Prozess, der festlegt, welche Aktionen ein Benutzer ausführen darf.

Eine weitere Möglichkeit, Authentifizierung zu definieren, besteht darin, sie als eine Möglichkeit zu betrachten, einen „Raum“ (z.B. einen Server, eine Datenbank, eine App oder eine Ressource) zu betreten, während die Autorisierung darin besteht, welche Aktionen der Benutzer mit welchen Objekten innerhalb dieses „Raums“ (Server, Datenbank oder App) ausführen kann.

## <a name="common-vulnerabilities-in-software"></a>Häufige Sicherheitsrisiken in Software

ASP.NET Core und EF enthalten Features, die Ihnen helfen, Ihre Anwendungen zu schützen und Sicherheitsverletzungen zu verhindern. Die folgende Liste von Links verweist auf die Dokumentation, die Techniken zur Vermeidung der häufigsten Sicherheitsrisiken in Web-Apps beschreibt:

* [XSS-Angriffe (Cross-Site Scripting)](xref:security/cross-site-scripting)
* [Angriffe durch Einschleusung von SQL-Befehlen](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Websiteübergreifende Anforderungsfälschung (CSRF)](xref:security/anti-request-forgery)
* [Offene Weiterleitungsangriffe](xref:security/preventing-open-redirects)

Es gibt weitere Sicherheitsrisiken, die Sie kennen sollten. Weitere Informationen finden Sie im Abschnitt *ASP.NET Sicherheitsdokumentation* dieses Dokuments.

## <a name="aspnet-security-documentation"></a>ASP.NET-Sicherheitsdokumentation

*   [Authentifizierung](xref:security/authentication/index)
    *   [Einführung in Identity](xref:security/authentication/identity)
    *   [Aktivieren der Authentifizierung mithilfe von Facebook, Google und anderen externen Anbietern](xref:security/authentication/social/index)
    *   [Aktivieren der Authentifizierung mit dem WS-Verbund](xref:security/authentication/ws-federation)
    * [Konfigurieren der Windows-Authentifizierung](xref:security/authentication/windowsauth)
    *   [Kontobestätigung und Kennwortwiederherstellung](xref:security/authentication/accconfirm)
    *   [Zweistufige Authentifizierung mit SMS](xref:security/authentication/2fa)
    *   [Verwenden der Cookieauthentifizierung ohne Identity](xref:security/authentication/cookie)
    *   [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
        *   [Integrieren von Azure AD in eine ASP.NET Core-Web-App](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Aufrufen einer ASP.NET Core-Web-API aus einer WPF-App mithilfe von Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Aufrufen einer Web-API in einer ASP.NET Core-Web-App mithilfe von Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Eine ASP.NET Core-Web-App in Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Schützen von ASP.NET Core-Apps mit IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorisierung](xref:security/authorization/index)
    *   [Introduction (Einführung)](xref:security/authorization/introduction)
    *   [Erstellen einer App mit Benutzerdaten, die durch Autorisierung geschützt sind](xref:security/authorization/secure-data)
    *   [Einfache Autorisierung](xref:security/authorization/simple)
    *   [Rollenbasierte Autorisierung](xref:security/authorization/roles)
    *   [Anspruchsbasierte Autorisierung](xref:security/authorization/claims)
    *   [Richtlinienbasierte Autorisierung](xref:security/authorization/policies)
    *   [Abhängigkeitsinjektion in Anforderungshandlern](xref:security/authorization/dependencyinjection)
    *   [Ressourcenbasierte Autorisierung](xref:security/authorization/resourcebased)
    *   [Ansichtsbasierte Autorisierung](xref:security/authorization/views)
    *   [Einschränken der Identität nach Schema](xref:security/authorization/limitingidentitybyscheme)
*   [Schutz von Daten](xref:security/data-protection/index)
    *   [Einführung in den Schutz von Daten](xref:security/data-protection/introduction)
    *   [Erste Schritte mit Datenschutz-APIs](xref:security/data-protection/using-data-protection)
    *   [Consumer-APIs](xref:security/data-protection/consumer-apis/index)
        *   [Übersicht über Consumer-APIs](xref:security/data-protection/consumer-apis/overview)
        *   [purpose-Zeichenfolgen](xref:security/data-protection/consumer-apis/purpose-strings)
        *   [Zweckhierarchie und Mehrinstanzenfähigkeit](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
        *   [Hasherstellung für Kennwörter](xref:security/data-protection/consumer-apis/password-hashing)
        *   [Beschränken der Lebensdauer von geschützten Payloads](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
        *   [Aufheben des Schutzes von Payloads, deren Schlüssel gesperrt wurden](xref:security/data-protection/consumer-apis/dangerous-unprotect)
    *   [Konfiguration](xref:security/data-protection/configuration/index)
        *   [Konfigurieren des Schutzes von Daten](xref:security/data-protection/configuration/overview)
        *   [Standardeinstellungen](xref:security/data-protection/configuration/default-settings)
        *   [Computerweite Richtlinie](xref:security/data-protection/configuration/machine-wide-policy)
        *   [Szenarios ohne Möglichkeiten zur Abhängigkeitsinjektion](xref:security/data-protection/configuration/non-di-scenarios)
    *   [Erweiterbarkeits-APIs](xref:security/data-protection/extensibility/index)
        *   [Kryptografieerweiterbarkeit in Core](xref:security/data-protection/extensibility/core-crypto)
        *   [Schlüsselverwaltungserweiterbarkeit](xref:security/data-protection/extensibility/key-management)
        *   [Verschiedene APIs](xref:security/data-protection/extensibility/misc-apis)
    *   [Implementation (Implementierung)](xref:security/data-protection/implementation/index)
        *   [Authentifizierte Verschlüsselungsdetails](xref:security/data-protection/implementation/authenticated-encryption-details)
        *   [Unterschlüsselableitung und authentifizierte Verschlüsselung](xref:security/data-protection/implementation/subkeyderivation)
        *   [Kontextheader](xref:security/data-protection/implementation/context-headers)
        *   [Schlüsselverwaltung](xref:security/data-protection/implementation/key-management)
        *   [Schlüsselspeicheranbieter](xref:security/data-protection/implementation/key-storage-providers)
        *   [Verschlüsselung ruhender Daten mit Schlüsseln](xref:security/data-protection/implementation/key-encryption-at-rest)
        *   [Schlüsselunveränderlichkeit und -einstellungen](xref:security/data-protection/implementation/key-immutability)
        *   [Schlüsselspeicherformat](xref:security/data-protection/implementation/key-storage-format)
        *   [Kurzlebige Datenschutzanbieter](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [Kompatibilität](xref:security/data-protection/compatibility/index)
        *   [Ersetzen von <machineKey> in ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
*   [Erstellen einer App mit Benutzerdaten, die durch Autorisierung geschützt sind](xref:security/authorization/secure-data)
*   [Sicheres Speichern geheimer App-Schlüssel in der Entwicklung](xref:security/app-secrets)
*   [Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration)
*   [Erzwingen von SSL](xref:security/enforcing-ssl)
*   [Antianforderungsfälschung](xref:security/anti-request-forgery)
*   [Verhindern von Angriffen durch offene Umleitungen](xref:security/preventing-open-redirects)
*   [Verhindern von siteübergreifendem Scripting](xref:security/cross-site-scripting)
*   [Aktivieren ursprungsübergreifender Anforderungen (CORS)](xref:security/cors)
*   [Freigeben von Cookies für mehrere Apps](xref:security/cookie-sharing)
