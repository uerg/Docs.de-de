---
title: Allgemeine Daten Schutz vor (GDPR)-Unterstützung in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie die Erweiterungspunkte GDPR in einer ASP.NET Core-Web-app zugreifen.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: eb9173bfe554b8b00ef8deb255e8347a534e7ba3
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725795"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Europa allgemeine Daten Schutz vor (GDPR)-Unterstützung in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core bietet APIs und Vorlagen, um einige der erfüllen die [Europa allgemeine Daten Schutz vor (GDPR)](https://www.eugdpr.org/) Anforderungen:

* Die Projektvorlagen enthalten Erweiterungspunkte und gekürzte Markup, das Sie mit Ihren Datenschutz und Cookies verwenden Gruppenrichtlinien ersetzen können.
* Ein Cookie Zustimmung-Feature können Sie seine Zustimmung bitten (und Nachverfolgen) Ihre Benutzer zum Speichern von persönlichen Informationen. Wenn ein Benutzer nicht, um die Datensammlung zugestimmt hat und die app mit festgelegt ist [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) zu `true`, unbedeutende Cookies werden nicht an den Browser gesendet werden.
* Cookies können als wesentlich markiert werden. Wichtige Cookies werden an den Browser gesendet, selbst wenn der Benutzer nicht zugestimmt hat und die Überwachung ist deaktiviert.
* [TempData und Sitzungscookies](#tempdata) sind nicht funktionsfähig, wenn die Überwachung deaktiviert ist.
* Die [Verwaltung der Identität](#pd) Seite enthält einen Link zum Herunterladen und Löschen von Benutzerdaten.

Die [Beispiel-app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) können Sie die meisten GDPR Erweiterungspunkte und APIs, die an den Vorlagen für ASP.NET Core 2.1 hinzugefügt testen. Finden Sie unter der [Infodatei](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) Datei zum Testen von Anweisungen.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>ASP.NET Core GDPR-Unterstützung in der Vorlage generierter code

Razor-Seiten und MVC-Projekte mit den Projektvorlagen erstellt umfassen die folgenden GDPR-Unterstützung:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) und [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) in festgelegt sind `Startup`.
* Die *_CookieConsentPartial.cshtml* [Teilansicht](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).
* Die *Pages/Privacy.cshtml* oder *Home/Privacy.cshtml* Ansicht bietet eine Seite, um die Datenschutzrichtlinie des Standorts ausführlich beschrieben. Die *_CookieConsentPartial.cshtml* Datei wird einen Link zur Seite "Datenschutz" generiert.
* Für Anwendungen mit Konten einzelner Benutzer erstellt wurde, enthält der Seite "verwalten" Links zum Herunterladen und Löschen von [persönlicher Benutzerdaten](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions und UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) initialisiert werden die `Startup` Klasse `ConfigureServices` Methode:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) wird aufgerufen, der `Startup` Klasse `Configure` Methode:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>Teilansicht _CookieConsentPartial.cshtml

Die *_CookieConsentPartial.cshtml* partielle Ansicht:

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

Diese partielle:

* Ruft den Status der Überwachung für den Benutzer ab. Wenn die Anwendung konfiguriert ist, um seine Zustimmung erforderlich muss der Benutzer zustimmen, damit Cookies nachverfolgt werden können. Wenn Zustimmung erforderlich ist, wird das Cookie Zustimmung Chrome behoben, oben auf der Navigationsleiste in erstellt die *Pages/Shared/_Layout.cshtml* Datei.
* Stellt eine HTML `<p>` Element zusammengefasst Datenschutz und Cookies verwenden Gruppenrichtlinien.
* Enthält einen Link zu *Pages/Privacy.cshtml* , in dem können Sie Ihrer Website-Datenschutzrichtlinie ausführlich beschrieben.

## <a name="essential-cookies"></a>Wichtige cookies

Wenn Zustimmung nicht angegeben wurde, werden nur Cookies gekennzeichnet wesentliche an den Browser gesendet. Im folgenden Code werden einen Cookie wichtig:

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a>TempData-Anbieter und Status Sitzungscookies sind nicht wichtig

Die [Tempdata Anbieter](xref:fundamentals/app-state#tempdata) Cookie ist nicht unbedingt erforderlich. Wenn die Überwachung deaktiviert ist, ist der Anbieter Tempdata nicht funktionsfähig. Damit der Tempdata-Anbieter kann bei der Überwachung deaktiviert ist, markieren Sie das Cookie TempData als im Wesentlichen `ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[Sitzungsstatus](xref:fundamentals/app-state) Cookies sind nicht wichtig. Status der Sitzung ist nicht funktionsfähig, wenn die Überwachung deaktiviert ist.

<a name="pd"></a>

## <a name="personal-data"></a>Persönlichen Daten

ASP.NET Core-Anwendungen, die mit den einzelnen Benutzerkonten erstellt einschließen von Code zum Herunterladen und Löschen von persönlichen Daten.

Wählen Sie den Benutzernamen ein, und wählen Sie dann **persönliche Daten**:

![Verwalten persönlicher Daten (Seite)](gdpr/_static/pd.png)

Notizen:

* Zum Generieren der `Account/Manage` code, finden Sie unter [Gerüst Identität](xref:security/authentication/scaffold-identity).
* Löschen und nur Auswirkungen auf die Standard-Identitätsdaten herunterladen. Apps, die benutzerdefinierten Benutzerdaten erstellen müssen Delete / die benutzerdefinierten Benutzerdaten herunterladen erweitert werden. GitHub-Problem [hinzufügen und Löschen von benutzerdefinierten Benutzerdaten Identität](https://github.com/aspnet/Docs/issues/6226) einen vorgeschlagenen Artikel zum Erstellen von benutzerdefinierten/löschen/Herunterladen von benutzerdefinierten Benutzerdaten verfolgt. Wenn Sie, finden in diesem Thema priorisiert möchten, lassen Sie eine Daumen hoch Reaktion in der Ausgabe.
* Gespeicherte Token für den Benutzer, die in der Datenbanktabelle Identität gespeichert sind `AspNetUserTokens` werden gelöscht, wenn der Benutzer, über das cascading Delete Verhalten aufgrund gelöscht wird der [Fremdschlüssel](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).

## <a name="encryption-at-rest"></a>Verschlüsselung ruhender

Einige Datenbanken und andere Speichermechanismen ermöglichen Verschlüsselung ruhender. Verschlüsselung ruhender:

* Gespeicherte Daten verschlüsselt automatisch.
* Verschlüsselt ohne Konfiguration, Programmierung und andere arbeiten, die für die Software, die auf die Daten zugreift.
* Ist die einfachste und sicherste Option.
* So kann die Datenbank Verwalten von Schlüsseln und Verschlüsselung.

Zum Beispiel:

* Bereitstellen von Microsoft SQL- und Azure SQL [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).
* [SQL Azure werden die Datenbank standardmäßig verschlüsselt.](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Azure-Blobs, Dateien, Tabelle und Warteschlangenspeicher standardmäßig verschlüsselt](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Für Datenbanken, die integrierte Verschlüsselung ruhender nicht bieten möglicherweise Sie datenträgerverschlüsselung verwenden, um den gleichen Schutz bereitstellen können. Zum Beispiel:

* [BitLocker für WindowsServer](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Microsoft.com/GDPR](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
