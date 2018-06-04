---
title: Allgemeine Daten Schutz vor (GDPR)-Unterstützung in ASP.NET Core
author: rick-anderson
description: Zeigt, wie die Erweiterungspunkte GDPR in einer ASP.NET Core Zugriff auf Web-app.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: 92a7000f4f8e4c2097065cb530fe106ef0e98545
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688626"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="64e1c-103">Europa allgemeine Daten Schutz vor (GDPR)-Unterstützung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64e1c-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="64e1c-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="64e1c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="64e1c-105">ASP.NET Core bietet APIs und Vorlagen, um einige der erfüllen die [Europa allgemeine Daten Schutz vor (GDPR)](https://www.eugdpr.org/) Anforderungen:</span><span class="sxs-lookup"><span data-stu-id="64e1c-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="64e1c-106">Die Projektvorlagen enthalten Erweiterungspunkte und gekürzte Markup, das Sie mit Ihren Datenschutz und Cookies verwenden Gruppenrichtlinien ersetzen können.</span><span class="sxs-lookup"><span data-stu-id="64e1c-106">The project templates include extension points and stubbed markup you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="64e1c-107">Ein Cookie Zustimmung-Feature können Sie seine Zustimmung bitten (und Nachverfolgen) Ihre Benutzer zum Speichern von persönlichen Informationen.</span><span class="sxs-lookup"><span data-stu-id="64e1c-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="64e1c-108">Wenn ein Benutzer nicht, um die Datensammlung zugestimmt hat und die app mit festgelegt ist [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) zu `true`, unbedeutende Cookies werden nicht an den Browser gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="64e1c-108">If a user has not consented to data collection and the app is set with [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) to `true`, non-essential cookies will not be sent to the browser.</span></span>
* <span data-ttu-id="64e1c-109">Cookies können als wesentlich markiert werden.</span><span class="sxs-lookup"><span data-stu-id="64e1c-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="64e1c-110">Wichtige Cookies werden an den Browser gesendet, selbst wenn der Benutzer nicht zugestimmt hat und die Überwachung ist deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="64e1c-110">Essential cookies are sent to the browser even when the user has not consented and tracking is disabled.</span></span>
* <span data-ttu-id="64e1c-111">[TempData und Sitzungscookies](#tempdata) sind nicht funktionsfähig, wenn die Überwachung deaktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="64e1c-111">[TempData and Session cookies](#tempdata) are not functional when tracking is disabled.</span></span>
* <span data-ttu-id="64e1c-112">Die [Verwaltung der Identität](#pd) Seite enthält einen Link zum Herunterladen und Löschen von Benutzerdaten.</span><span class="sxs-lookup"><span data-stu-id="64e1c-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="64e1c-113">Die [Beispiel-app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) können Sie die meisten GDPR Erweiterungspunkte und APIs, die an den Vorlagen für ASP.NET Core 2.1 hinzugefügt testen.</span><span class="sxs-lookup"><span data-stu-id="64e1c-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="64e1c-114">Finden Sie unter der [Infodatei](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) Datei zum Testen von Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="64e1c-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="64e1c-115">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="64e1c-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="64e1c-116">ASP.NET Core GDPR-Unterstützung in der Vorlage generierter code</span><span class="sxs-lookup"><span data-stu-id="64e1c-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="64e1c-117">Razor-Seiten und MVC-Projekte mit den Projektvorlagen erstellt umfassen die folgenden GDPR-Unterstützung:</span><span class="sxs-lookup"><span data-stu-id="64e1c-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="64e1c-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) und [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in festgelegt sind `Startup`.</span><span class="sxs-lookup"><span data-stu-id="64e1c-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) are set in `Startup`.</span></span>
* <span data-ttu-id="64e1c-119">Die *_CookieConsentPartial.cshtml* [Teilansicht](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="64e1c-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="64e1c-120">Die *Pages/Privacy.cshtml* oder *Home/Privacy.cshtml* Ansicht bietet eine Seite, um die Datenschutzrichtlinie des Standorts ausführlich beschrieben.</span><span class="sxs-lookup"><span data-stu-id="64e1c-120">The *Pages/Privacy.cshtml* or *Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="64e1c-121">Die *_CookieConsentPartial.cshtml* Datei wird einen Link zur Seite "Datenschutz" generiert.</span><span class="sxs-lookup"><span data-stu-id="64e1c-121">The *_CookieConsentPartial.cshtml* file generates a link to the privacy page.</span></span>
* <span data-ttu-id="64e1c-122">Für Anwendungen mit Konten einzelner Benutzer erstellt wurde, enthält der Seite "verwalten" Links zum Herunterladen und Löschen von [persönlicher Benutzerdaten](#pd).</span><span class="sxs-lookup"><span data-stu-id="64e1c-122">For applications created with individual user accounts, the manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="64e1c-123">CookiePolicyOptions und UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="64e1c-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="64e1c-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) initialisiert werden die `Startup` Klasse `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="64e1c-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) are initialized in the `Startup` class `ConfigureServices` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="64e1c-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) wird aufgerufen, der `Startup` Klasse `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="64e1c-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) is called in the `Startup` class `Configure` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="64e1c-126">Teilansicht _CookieConsentPartial.cshtml</span><span class="sxs-lookup"><span data-stu-id="64e1c-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="64e1c-127">Die *_CookieConsentPartial.cshtml* partielle Ansicht:</span><span class="sxs-lookup"><span data-stu-id="64e1c-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="64e1c-128">Diese partielle:</span><span class="sxs-lookup"><span data-stu-id="64e1c-128">This partial:</span></span>

* <span data-ttu-id="64e1c-129">Ruft den Status der Überwachung für den Benutzer ab.</span><span class="sxs-lookup"><span data-stu-id="64e1c-129">Gets the state of tracking for the user.</span></span> <span data-ttu-id="64e1c-130">Wenn die Anwendung konfiguriert ist, um seine Zustimmung erforderlich muss der Benutzer zustimmen, damit Cookies nachverfolgt werden können.</span><span class="sxs-lookup"><span data-stu-id="64e1c-130">If the application is configured to require consent the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="64e1c-131">Wenn Zustimmung erforderlich ist, wird das Cookie Zustimmung Chrome behoben, oben auf der Navigationsleiste in erstellt die *Pages/Shared/_Layout.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="64e1c-131">If consent is required, the cookie consent chrome is fixed on top of the navigation bar created in the *Pages/Shared/_Layout.cshtml* file.</span></span>
* <span data-ttu-id="64e1c-132">Stellt eine HTML `<p>` Element zusammengefasst Datenschutz und Cookies verwenden Gruppenrichtlinien.</span><span class="sxs-lookup"><span data-stu-id="64e1c-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="64e1c-133">Enthält einen Link zu *Pages/Privacy.cshtml* , in dem können Sie Ihrer Website-Datenschutzrichtlinie ausführlich beschrieben.</span><span class="sxs-lookup"><span data-stu-id="64e1c-133">Provides a link to *Pages/Privacy.cshtml* where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="64e1c-134">Wichtige cookies</span><span class="sxs-lookup"><span data-stu-id="64e1c-134">Essential cookies</span></span>

<span data-ttu-id="64e1c-135">Wenn Zustimmung nicht angegeben wurde, werden nur Cookies gekennzeichnet wesentliche an den Browser gesendet.</span><span class="sxs-lookup"><span data-stu-id="64e1c-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="64e1c-136">Im folgenden Code werden einen Cookie wichtig:</span><span class="sxs-lookup"><span data-stu-id="64e1c-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="64e1c-137">TempData-Anbieter und Status Sitzungscookies sind nicht wichtig</span><span class="sxs-lookup"><span data-stu-id="64e1c-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="64e1c-138">Die [Tempdata Anbieter](xref:fundamentals/app-state#tempdata) Cookie ist nicht unbedingt erforderlich.</span><span class="sxs-lookup"><span data-stu-id="64e1c-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie is not essential.</span></span> <span data-ttu-id="64e1c-139">Wenn die Überwachung deaktiviert ist, ist der Anbieter Tempdata nicht funktionsfähig.</span><span class="sxs-lookup"><span data-stu-id="64e1c-139">If tracking is disabled, the Tempdata provider is not functional.</span></span> <span data-ttu-id="64e1c-140">Damit der Tempdata-Anbieter kann bei der Überwachung deaktiviert ist, markieren Sie das Cookie TempData als im Wesentlichen `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="64e1c-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="64e1c-141">[Sitzungsstatus](xref:fundamentals/app-state) Cookies sind nicht wichtig.</span><span class="sxs-lookup"><span data-stu-id="64e1c-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="64e1c-142">Status der Sitzung ist nicht funktionsfähig, wenn die Überwachung deaktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="64e1c-142">Session state is not functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="64e1c-143">Persönlichen Daten</span><span class="sxs-lookup"><span data-stu-id="64e1c-143">Personal data</span></span>

<span data-ttu-id="64e1c-144">ASP.NET Core-Anwendungen, die mit den einzelnen Benutzerkonten erstellt einschließen von Code zum Herunterladen und Löschen von persönlichen Daten.</span><span class="sxs-lookup"><span data-stu-id="64e1c-144">ASP.NET Core applications created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="64e1c-145">Wählen Sie den Benutzernamen ein, und wählen Sie dann **persönliche Daten**:</span><span class="sxs-lookup"><span data-stu-id="64e1c-145">Select the user name and then select **Personal data**:</span></span>

![Verwalten persönlicher Daten (Seite)](gdpr/_static/pd.png)

<span data-ttu-id="64e1c-147">Notizen:</span><span class="sxs-lookup"><span data-stu-id="64e1c-147">Notes:</span></span>

* <span data-ttu-id="64e1c-148">Zum Generieren der `Account/Manage` code, finden Sie unter [Gerüst Identität](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="64e1c-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="64e1c-149">Löschen und nur Auswirkungen auf die Standard-Identitätsdaten herunterladen.</span><span class="sxs-lookup"><span data-stu-id="64e1c-149">Delete and download only impact the default identity data.</span></span> <span data-ttu-id="64e1c-150">Apps, die benutzerdefinierten Benutzerdaten erstellen müssen Delete / die benutzerdefinierten Benutzerdaten herunterladen erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="64e1c-150">Apps the create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="64e1c-151">GitHub-Problem [hinzufügen und Löschen von benutzerdefinierten Benutzerdaten Identität](https://github.com/aspnet/Docs/issues/6226) einen vorgeschlagenen Artikel zum Erstellen von benutzerdefinierten/löschen/Herunterladen von benutzerdefinierten Benutzerdaten verfolgt.</span><span class="sxs-lookup"><span data-stu-id="64e1c-151">GitHub issue [How to add/delete custom user data to Identity](https://github.com/aspnet/Docs/issues/6226) tracks a proposed article on creating custom/deleting/downloading custom user data.</span></span> <span data-ttu-id="64e1c-152">Wenn Sie, finden in diesem Thema priorisiert möchten, lassen Sie eine Daumen hoch Reaktion in der Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="64e1c-152">If you'd like to see that topic prioritized, leave a thumbs up reaction in the issue.</span></span>
* <span data-ttu-id="64e1c-153">Gespeicherte Token für den Benutzer, die in der Datenbanktabelle Identität gespeichert sind `AspNetUserTokens` werden gelöscht, wenn der Benutzer, über das cascading Delete Verhalten aufgrund gelöscht wird der [Fremdschlüssel](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="64e1c-153">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="64e1c-154">Verschlüsselung ruhender</span><span class="sxs-lookup"><span data-stu-id="64e1c-154">Encryption at rest</span></span>

<span data-ttu-id="64e1c-155">Einige Datenbanken und andere Speichermechanismen ermöglichen Verschlüsselung ruhender.</span><span class="sxs-lookup"><span data-stu-id="64e1c-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="64e1c-156">Verschlüsselung ruhender:</span><span class="sxs-lookup"><span data-stu-id="64e1c-156">Encryption at rest:</span></span>

* <span data-ttu-id="64e1c-157">Gespeicherte Daten verschlüsselt automatisch.</span><span class="sxs-lookup"><span data-stu-id="64e1c-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="64e1c-158">Verschlüsselt ohne Konfiguration, Programmierung und andere arbeiten, die für die Software, die auf die Daten zugreift.</span><span class="sxs-lookup"><span data-stu-id="64e1c-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="64e1c-159">Ist die einfachste und sicherste Option.</span><span class="sxs-lookup"><span data-stu-id="64e1c-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="64e1c-160">So kann die Datenbank Verwalten von Schlüsseln und Verschlüsselung.</span><span class="sxs-lookup"><span data-stu-id="64e1c-160">Lets the database manage keys and encryption.</span></span>

<span data-ttu-id="64e1c-161">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="64e1c-161">For example:</span></span>

* <span data-ttu-id="64e1c-162">Bereitstellen von Microsoft SQL- und Azure SQL [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span><span class="sxs-lookup"><span data-stu-id="64e1c-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span></span>
* [<span data-ttu-id="64e1c-163">SQL Azure werden die Datenbank standardmäßig verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="64e1c-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/en-us/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="64e1c-164">[Azure-Blobs, Dateien, Tabelle und Warteschlangenspeicher standardmäßig verschlüsselt](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="64e1c-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="64e1c-165">Für Datenbanken, die integrierte Verschlüsselung ruhender nicht bieten möglicherweise Sie datenträgerverschlüsselung verwenden, um den gleichen Schutz bereitstellen können.</span><span class="sxs-lookup"><span data-stu-id="64e1c-165">For databases that don't provide built-in encryption at rest you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="64e1c-166">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="64e1c-166">For example:</span></span>

* [<span data-ttu-id="64e1c-167">BitLocker für WindowsServer</span><span class="sxs-lookup"><span data-stu-id="64e1c-167">Bitlocker for windows server</span></span>](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="64e1c-168">Linux:</span><span class="sxs-lookup"><span data-stu-id="64e1c-168">Linux:</span></span>
  * [<span data-ttu-id="64e1c-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="64e1c-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="64e1c-170">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="64e1c-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64e1c-171">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="64e1c-171">Additional Resources</span></span>

* [<span data-ttu-id="64e1c-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="64e1c-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
