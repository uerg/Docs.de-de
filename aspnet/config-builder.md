---
uid: config-builder
title: Konfigurations-Generatoren für ASP.NET
author: rick-anderson
description: Erfahren Sie, wie Sie Konfigurationsdaten aus anderen Quellen als Datei "Web.config"-Werte aus externen Quellen abrufen.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 4dcc62573fad13ec8b37b2c59e884eec7ca80b92
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148836"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="282d1-103">Konfigurations-Generatoren für ASP.NET</span><span class="sxs-lookup"><span data-stu-id="282d1-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="282d1-104">Durch [Stephen Molloy](https://github.com/StephenMolloy) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="282d1-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="282d1-105">Konfigurations-Generatoren bieten einen Mechanismus für moderne und flexible für ASP.NET-Apps Konfigurationswerte aus externen Quellen abgerufen.</span><span class="sxs-lookup"><span data-stu-id="282d1-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="282d1-106">Konfigurationsgeneratoren:</span><span class="sxs-lookup"><span data-stu-id="282d1-106">Configuration builders:</span></span>

* <span data-ttu-id="282d1-107">Sind in .NET Framework 4.7.1 und höher verfügbar.</span><span class="sxs-lookup"><span data-stu-id="282d1-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="282d1-108">Geben Sie einen flexiblen Mechanismus zum Lesen von Werten.</span><span class="sxs-lookup"><span data-stu-id="282d1-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="282d1-109">Beheben Sie einige grundlegende Anforderungen von apps in einem Container zu verschieben und fokussierte Umgebung der cloud.</span><span class="sxs-lookup"><span data-stu-id="282d1-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="282d1-110">Dienen zur Verbesserung des Schutzes von Konfigurationsdaten durch Zeichnen von Quellen, die zuvor nicht verfügbar (z. B. Azure Key Vault und Umgebungsvariablen) im Konfigurationssystem .NET.</span><span class="sxs-lookup"><span data-stu-id="282d1-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="282d1-111">Schlüssel/Wert-Konfigurations-Generatoren</span><span class="sxs-lookup"><span data-stu-id="282d1-111">Key/value configuration builders</span></span>

<span data-ttu-id="282d1-112">Ein häufiges Szenario, das vom Konfigurations-Generatoren verarbeitet werden kann, ist, einen einfachen Schlüssel-Wert-Ersatz-Mechanismus für Konfigurationsabschnitte bereitzustellen, die ein Schlüssel/Wert-Muster folgen.</span><span class="sxs-lookup"><span data-stu-id="282d1-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="282d1-113">Das .NET Framework-Konzept der ConfigurationBuilders ist nicht beschränkt auf bestimmte Konfigurationsabschnitte oder Muster.</span><span class="sxs-lookup"><span data-stu-id="282d1-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="282d1-114">Allerdings viele der in der Konfigurations-Generatoren `Microsoft.Configuration.ConfigurationBuilders` ([Github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) innerhalb des Schlüssel-Wert-Musters arbeiten.</span><span class="sxs-lookup"><span data-stu-id="282d1-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="282d1-115">Schlüssel/Wert-Generatoren Konfigurationseinstellungen</span><span class="sxs-lookup"><span data-stu-id="282d1-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="282d1-116">Die folgenden Einstellungen gelten für alle Schlüssel/Wert-konfigurationsbuilder in `Microsoft.Configuration.ConfigurationBuilders`.</span><span class="sxs-lookup"><span data-stu-id="282d1-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="282d1-117">Modus</span><span class="sxs-lookup"><span data-stu-id="282d1-117">Mode</span></span>

<span data-ttu-id="282d1-118">Der Konfigurations-Generatoren verwenden eine externe Quelle von Schlüssel-/Wertinformationen um ausgewählten Schlüssel/Wert-Elementen des Konfigurationssystems zu füllen.</span><span class="sxs-lookup"><span data-stu-id="282d1-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="282d1-119">Insbesondere die `<appSettings/>` und `<connectionStrings/>` Abschnitten erhalten Sie spezielle Behandlung von Konfigurations-Generatoren.</span><span class="sxs-lookup"><span data-stu-id="282d1-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="282d1-120">Die Generatoren arbeiten in drei Modi:</span><span class="sxs-lookup"><span data-stu-id="282d1-120">The builders work in three modes:</span></span>

* <span data-ttu-id="282d1-121">`Strict` -Der Standardmodus.</span><span class="sxs-lookup"><span data-stu-id="282d1-121">`Strict` - The default mode.</span></span> <span data-ttu-id="282d1-122">In diesem Modus kann der Konfigurations-Generator nur auf bekannte Konfigurationsabschnitte für Schlüssel/Wert-orientierte eingesetzt.</span><span class="sxs-lookup"><span data-stu-id="282d1-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="282d1-123">`Strict` Jeder Schlüssel im Abschnitt listet die Modus.</span><span class="sxs-lookup"><span data-stu-id="282d1-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="282d1-124">Wenn ein übereinstimmenden Schlüssel in der externen Quelle gefunden wird:</span><span class="sxs-lookup"><span data-stu-id="282d1-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="282d1-125">Der Konfigurations-Generatoren ersetzen Sie den Wert im Abschnitt mit resultierenden mit dem Wert aus der externen Quelle.</span><span class="sxs-lookup"><span data-stu-id="282d1-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="282d1-126">`Greedy` : Dieser Modus ist eng verwandt mit `Strict` Modus.</span><span class="sxs-lookup"><span data-stu-id="282d1-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="282d1-127">Anstatt die Begrenzung auf Schlüssel, die bereits in der ursprünglichen Konfiguration vorhanden sein:</span><span class="sxs-lookup"><span data-stu-id="282d1-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="282d1-128">Der Konfigurations-Generatoren fügt alle Schlüssel/Wert-Paare aus der externen Quelle in der resultierenden Konfigurationsabschnitt hinzu.</span><span class="sxs-lookup"><span data-stu-id="282d1-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="282d1-129">`Expand` -Arbeitet mit den unformatierten XML-Daten, bevor er in ein Konfigurationsobjekt für den Abschnitt aufgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="282d1-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="282d1-130">Sie kann als Erweiterung von Token in einer Zeichenfolge betrachtet werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="282d1-131">Einen beliebigen Teil der unformatierten XML-Zeichenfolge mit dem Muster übereinstimmt `${token}` ist ein Kandidat für tokenerweiterung.</span><span class="sxs-lookup"><span data-stu-id="282d1-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="282d1-132">Wenn keine entsprechenden Wert in der externen Quelle gefunden wird, wird das Token nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="282d1-133">Generatoren, die in diesem Modus sind nicht darauf beschränkt die `<appSettings/>` und `<connectionStrings/>` Abschnitte.</span><span class="sxs-lookup"><span data-stu-id="282d1-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="282d1-134">Das folgende Markup aus *"Web.config"* ermöglicht die [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` Modus:</span><span class="sxs-lookup"><span data-stu-id="282d1-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="282d1-135">Der folgende code liest die `<appSettings/>` und `<connectionStrings/>` angezeigt, in der vorherigen *"Web.config"* Datei:</span><span class="sxs-lookup"><span data-stu-id="282d1-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="282d1-136">Der vorangehende Code wird die Eigenschaftswerte festgelegt, um:</span><span class="sxs-lookup"><span data-stu-id="282d1-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="282d1-137">Die Werte in der *"Web.config"* Datei, wenn die Schlüssel in Umgebungsvariablen nicht festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="282d1-138">Die Werte der Umgebung Variable If festgelegt.</span><span class="sxs-lookup"><span data-stu-id="282d1-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="282d1-139">Z. B. `ServiceID` enthält:</span><span class="sxs-lookup"><span data-stu-id="282d1-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="282d1-140">"ServiceID Wert aus" Web.config "", wenn die Umgebungsvariable `ServiceID` ist nicht festgelegt.</span><span class="sxs-lookup"><span data-stu-id="282d1-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="282d1-141">Der Wert des der `ServiceID` Umgebungsvariable, falls festgelegt.</span><span class="sxs-lookup"><span data-stu-id="282d1-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="282d1-142">Die folgende Abbildung zeigt die `<appSettings/>` Schlüssel/Werte aus dem vorherigen *"Web.config"* Dateisatz in der Umgebung-Editor:</span><span class="sxs-lookup"><span data-stu-id="282d1-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Umgebung-editor](config-builder/static/env.png)

<span data-ttu-id="282d1-144">Hinweis: Sie sollten zu beenden und starten Sie Visual Studio, um Änderungen in Umgebungsvariablen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="282d1-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="282d1-145">Behandlung von Präfix</span><span class="sxs-lookup"><span data-stu-id="282d1-145">Prefix handling</span></span>

<span data-ttu-id="282d1-146">Wichtige Präfixe können Einstellungsschlüssel vereinfachen, da:</span><span class="sxs-lookup"><span data-stu-id="282d1-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="282d1-147">Die .NET Framework-Konfiguration ist komplex und geschachtelte.</span><span class="sxs-lookup"><span data-stu-id="282d1-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="282d1-148">Externen Schlüssel/Wert-Quellen sind häufig basic "und" naturgemäß Flatfile.</span><span class="sxs-lookup"><span data-stu-id="282d1-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="282d1-149">Beispielsweise werden Umgebungsvariablen nicht geschachtelt werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="282d1-150">Eine der folgenden Methoden zum Einfügen von sowohl verwenden `<appSettings/>` und `<connectionStrings/>` in die Konfiguration über Umgebungsvariablen:</span><span class="sxs-lookup"><span data-stu-id="282d1-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="282d1-151">Mit der `EnvironmentConfigBuilder` in der standardmäßigen `Strict` Modus und den entsprechenden Schlüsselnamen in der Konfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="282d1-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="282d1-152">Der vorherige Code und Markup verwendet diesen Ansatz.</span><span class="sxs-lookup"><span data-stu-id="282d1-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="282d1-153">Mit diesem Ansatz Sie können **nicht** haben identisch benannten Schlüssel in beiden `<appSettings/>` und `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="282d1-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="282d1-154">Verwenden Sie zwei `EnvironmentConfigBuilder`s in `Greedy` Modus mit unterschiedlichen Präfixen und `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="282d1-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="282d1-155">Bei diesem Ansatz kann die app lesen `<appSettings/>` und `<connectionStrings/>` ohne die Konfigurationsdatei aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="282d1-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="282d1-156">Im nächsten Abschnitt [StripPrefix](#stripprefix), veranschaulicht dies.</span><span class="sxs-lookup"><span data-stu-id="282d1-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="282d1-157">Verwenden Sie zwei `EnvironmentConfigBuilder`s in `Greedy` Modus mit unterschiedlichen Präfixen.</span><span class="sxs-lookup"><span data-stu-id="282d1-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="282d1-158">Bei diesem Ansatz können nicht Sie doppelte Schlüssel aufweisen wie Schlüsselnamen nach Präfix unterscheiden müssen.</span><span class="sxs-lookup"><span data-stu-id="282d1-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="282d1-159">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="282d1-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="282d1-160">Mit der im obigen Markup kann die gleiche einfache Schlüssel/Wert-Quelle zum Auffüllen der Konfiguration für zwei verschiedene Abschnitte verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="282d1-161">Die folgende Abbildung zeigt die `<appSettings/>` und `<connectionStrings/>` Schlüssel/Werte aus dem vorherigen *"Web.config"* Dateisatz in der Umgebung-Editor:</span><span class="sxs-lookup"><span data-stu-id="282d1-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Umgebung-editor](config-builder/static/prefix.png)

<span data-ttu-id="282d1-163">Der folgende code liest die `<appSettings/>` und `<connectionStrings/>` Schlüssel/Werte enthalten, die in der vorherigen *"Web.config"* Datei:</span><span class="sxs-lookup"><span data-stu-id="282d1-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="282d1-164">Der vorangehende Code wird die Eigenschaftswerte festgelegt, um:</span><span class="sxs-lookup"><span data-stu-id="282d1-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="282d1-165">Die Werte in der *"Web.config"* Datei, wenn die Schlüssel in Umgebungsvariablen nicht festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="282d1-166">Die Werte der Umgebung Variable If festgelegt.</span><span class="sxs-lookup"><span data-stu-id="282d1-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="282d1-167">Verwenden Sie beispielsweise die vorherige *"Web.config"* -Datei, die die Schlüssel/Werte in der vorherigen Abbildung Umgebung-Editor und dem vorherigen Code, die folgenden Werte festgelegt sind:</span><span class="sxs-lookup"><span data-stu-id="282d1-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="282d1-168">Key</span><span class="sxs-lookup"><span data-stu-id="282d1-168">Key</span></span>              | <span data-ttu-id="282d1-169">Wert</span><span class="sxs-lookup"><span data-stu-id="282d1-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="282d1-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="282d1-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="282d1-171">AppSetting_ServiceID von Env-Variablen</span><span class="sxs-lookup"><span data-stu-id="282d1-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="282d1-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="282d1-172">AppSetting_default</span></span>            | <span data-ttu-id="282d1-173">AppSetting_default Wert env</span><span class="sxs-lookup"><span data-stu-id="282d1-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="282d1-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="282d1-174">ConnStr_default</span></span>         | <span data-ttu-id="282d1-175">ConnStr_default Val von env</span><span class="sxs-lookup"><span data-stu-id="282d1-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="282d1-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="282d1-176">stripPrefix</span></span>

<span data-ttu-id="282d1-177">`stripPrefix`: Boolesch, standardmäßig `false`.</span><span class="sxs-lookup"><span data-stu-id="282d1-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="282d1-178">Das vorhergehende XML-Markup trennt von app-Einstellungen von Verbindungszeichenfolgen, jedoch müssen Sie alle Schlüssel in der *"Web.config"* Datei, die das angegebene Präfix verwendet.</span><span class="sxs-lookup"><span data-stu-id="282d1-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="282d1-179">Z. B. das Präfix `AppSetting` muss hinzugefügt werden, um die `ServiceID` Schlüssel ("AppSetting_ServiceID").</span><span class="sxs-lookup"><span data-stu-id="282d1-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="282d1-180">Mit `stripPrefix`, das Präfix wird nicht verwendet, der *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="282d1-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="282d1-181">Das Präfix muss in der Konfigurations-Generator-Quelle (z. B. in der Umgebung.) Wir erwarten die meisten Entwickler verwenden `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="282d1-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="282d1-182">Anwendungen werden in der Regel aus dem Präfix entfernen.</span><span class="sxs-lookup"><span data-stu-id="282d1-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="282d1-183">Die folgenden *"Web.config"* entfernt das Präfix:</span><span class="sxs-lookup"><span data-stu-id="282d1-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="282d1-184">In der vorherigen *"Web.config"* -Datei, die `default` Schlüssel wird sowohl die `<appSettings/>` und `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="282d1-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="282d1-185">Die folgende Abbildung zeigt die `<appSettings/>` und `<connectionStrings/>` Schlüssel/Werte aus dem vorherigen *"Web.config"* Dateisatz in der Umgebung-Editor:</span><span class="sxs-lookup"><span data-stu-id="282d1-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Umgebung-editor](config-builder/static/prefix.png)

<span data-ttu-id="282d1-187">Der folgende code liest die `<appSettings/>` und `<connectionStrings/>` Schlüssel/Werte enthalten, die in der vorherigen *"Web.config"* Datei:</span><span class="sxs-lookup"><span data-stu-id="282d1-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="282d1-188">Der vorangehende Code wird die Eigenschaftswerte festgelegt, um:</span><span class="sxs-lookup"><span data-stu-id="282d1-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="282d1-189">Die Werte in der *"Web.config"* Datei, wenn die Schlüssel in Umgebungsvariablen nicht festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="282d1-190">Die Werte der Umgebung Variable If festgelegt.</span><span class="sxs-lookup"><span data-stu-id="282d1-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="282d1-191">Verwenden Sie beispielsweise die vorherige *"Web.config"* -Datei, die die Schlüssel/Werte in der vorherigen Abbildung Umgebung-Editor und dem vorherigen Code, die folgenden Werte festgelegt sind:</span><span class="sxs-lookup"><span data-stu-id="282d1-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="282d1-192">Key</span><span class="sxs-lookup"><span data-stu-id="282d1-192">Key</span></span>              | <span data-ttu-id="282d1-193">Wert</span><span class="sxs-lookup"><span data-stu-id="282d1-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="282d1-194">ServiceID</span><span class="sxs-lookup"><span data-stu-id="282d1-194">ServiceID</span></span>           | <span data-ttu-id="282d1-195">AppSetting_ServiceID von Env-Variablen</span><span class="sxs-lookup"><span data-stu-id="282d1-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="282d1-196">default</span><span class="sxs-lookup"><span data-stu-id="282d1-196">default</span></span>            | <span data-ttu-id="282d1-197">AppSetting_default Wert env</span><span class="sxs-lookup"><span data-stu-id="282d1-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="282d1-198">default</span><span class="sxs-lookup"><span data-stu-id="282d1-198">default</span></span>         | <span data-ttu-id="282d1-199">ConnStr_default Val von env</span><span class="sxs-lookup"><span data-stu-id="282d1-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="282d1-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="282d1-200">tokenPattern</span></span>

<span data-ttu-id="282d1-201">`tokenPattern`: String, Standardwert `@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="282d1-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="282d1-202">Die `Expand` Verhalten der Generatoren sucht das unformatierte XML für Token, die wie folgt aussehen `${token}`.</span><span class="sxs-lookup"><span data-stu-id="282d1-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="282d1-203">Suche erfolgt mit dem regulären Standardausdruck `@"\$\{(\w+)\}"`.</span><span class="sxs-lookup"><span data-stu-id="282d1-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="282d1-204">Der Satz von Zeichen, die entspricht `\w` ist strenger als XML und viele Konfigurationsquellen zulassen.</span><span class="sxs-lookup"><span data-stu-id="282d1-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="282d1-205">Verwendung `tokenPattern` Wenn mehr Zeichen als `@"\$\{(\w+)\}"` in den token-Namen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="282d1-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="282d1-206">`tokenPattern`: Zeichenfolge:</span><span class="sxs-lookup"><span data-stu-id="282d1-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="282d1-207">Ermöglicht Entwicklern, die dem regulären Ausdruck zu ändern, für den Abgleich von token verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="282d1-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="282d1-208">Keine Validierung wird vorgenommen, um sicherzustellen, dass es sich um ein Regex wohlgeformt und nicht riskant ist.</span><span class="sxs-lookup"><span data-stu-id="282d1-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="282d1-209">Es muss eine Erfassungsgruppe enthalten.</span><span class="sxs-lookup"><span data-stu-id="282d1-209">It must contain a capture group.</span></span> <span data-ttu-id="282d1-210">Das gesamte Token muss mit dem gesamten regulären Ausdruck übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="282d1-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="282d1-211">Die erste Erfassung muss den token-Namen, die in der Konfigurationsquelle gesucht werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="282d1-212">Konfigurations-Generatoren in Microsoft.Configuration.ConfigurationBuilders</span><span class="sxs-lookup"><span data-stu-id="282d1-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="282d1-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="282d1-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="282d1-214">Die [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="282d1-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="282d1-215">Ist die einfachste der konfigurationsgeneratoren.</span><span class="sxs-lookup"><span data-stu-id="282d1-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="282d1-216">Liest die Werte aus der Umgebung.</span><span class="sxs-lookup"><span data-stu-id="282d1-216">Reads values from the environment.</span></span>
* <span data-ttu-id="282d1-217">Es muss keine zusätzlichen Konfigurationsoptionen.</span><span class="sxs-lookup"><span data-stu-id="282d1-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="282d1-218">Die `name` Attributwert ist willkürlich.</span><span class="sxs-lookup"><span data-stu-id="282d1-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="282d1-219">**Hinweis:** In einer Umgebung mit Windows-Container werden nur zur Laufzeit festgelegten Variablen in der Umgebung der EntryPoint-Prozess eingefügt.</span><span class="sxs-lookup"><span data-stu-id="282d1-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="282d1-220">Apps, die als Dienst ausführen, oder ein Prozess nicht EntryPoint wählen Sie diese Variablen, wenn sie andernfalls über einen Mechanismus in den Container eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="282d1-221">Für [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-basierte Container, die aktuelle Version des [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) verarbeitet dies in der *DefaultAppPool* nur.</span><span class="sxs-lookup"><span data-stu-id="282d1-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="282d1-222">Andere Windows-basierten Container Varianten müssen möglicherweise ihre eigenen Injection-Methode für nicht-EntryPoint-Prozesse zu entwickeln.</span><span class="sxs-lookup"><span data-stu-id="282d1-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="282d1-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="282d1-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="282d1-224">Nie Speichern von Kennwörtern, vertrauliche Verbindungszeichenfolgen und andere sensiblen Daten im Quellcode.</span><span class="sxs-lookup"><span data-stu-id="282d1-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="282d1-225">Produktionsgeheimnisse sollte nicht verwendet werden zu Entwicklungs- oder Testzwecken.</span><span class="sxs-lookup"><span data-stu-id="282d1-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="282d1-226">Diese Konfigurations-Generator bietet eine Funktion, die ähnlich wie [Geheimnis-Manager für ASP.NET Core](/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="282d1-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="282d1-227">Die [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) in .NET Framework-Projekten verwendet werden kann, aber eine Datei mit Geheimnissen muss angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="282d1-228">Alternativ können Sie definieren die `UserSecretsId` Eigenschaft im Projekt Datei und erstellt die unformatierten geheimnisdatei am richtigen Speicherort für das Lesen.</span><span class="sxs-lookup"><span data-stu-id="282d1-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="282d1-229">Um externe Abhängigkeiten aus Ihrem Projekt zu halten, ist die geheimnisdatei XML-Format.</span><span class="sxs-lookup"><span data-stu-id="282d1-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="282d1-230">Die XML-Formatierung ist ein Implementierungsdetail, und das Format sollte nicht als zuverlässig betrachtet werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="282d1-231">Wenn Sie zum Freigeben benötigen eine *secrets.json* mit .NET Core-Projekte, erwägen Sie die Verwendung der [SimpleJsonConfigBuilder](#simplejsonconfig).</span><span class="sxs-lookup"><span data-stu-id="282d1-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfig).</span></span> <span data-ttu-id="282d1-232">Die `SimpleJsonConfigBuilder` format für .NET Core ein Implementierungsdetail unterliegen ebenfalls berücksichtigt werden sollten.</span><span class="sxs-lookup"><span data-stu-id="282d1-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="282d1-233">Konfiguration für Attribute `UserSecretsConfigBuilder`:</span><span class="sxs-lookup"><span data-stu-id="282d1-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="282d1-234">`userSecretsId` -Dies ist die bevorzugte Methode zum Identifizieren einer XML-Datei mit Geheimnissen aus.</span><span class="sxs-lookup"><span data-stu-id="282d1-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="282d1-235">Es funktioniert ähnlich wie .NET Core, verwendet eine `UserSecretsId` Projekteigenschaft zum Speichern von diesen Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="282d1-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="282d1-236">Die Zeichenfolge muss eindeutig sein, sie muss keine GUID sein.</span><span class="sxs-lookup"><span data-stu-id="282d1-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="282d1-237">Mit diesem Attribut den `UserSecretsConfigBuilder` Suchen in einem bekannten Speicherort (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) für eine Datei mit Geheimnissen, die zu dieser Bezeichner gehören.</span><span class="sxs-lookup"><span data-stu-id="282d1-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="282d1-238">`userSecretsFile` – Ein optionales Attribut, das die Datei mit die geheimen Schlüssel angeben.</span><span class="sxs-lookup"><span data-stu-id="282d1-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="282d1-239">Die `~` Zeichen am Anfang auf den Stammordner der Anwendung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="282d1-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="282d1-240">Dieses Attribut oder das `userSecretsId` Attribut ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="282d1-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="282d1-241">Wenn beide angegeben werden, `userSecretsFile` hat Vorrang vor.</span><span class="sxs-lookup"><span data-stu-id="282d1-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="282d1-242">`optional`: Boolesch, Standardwert `true` -wird keine Ausnahme aus, wenn die Datei mit Geheimnissen nicht gefunden werden kann.</span><span class="sxs-lookup"><span data-stu-id="282d1-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="282d1-243">Die `name` Attributwert ist willkürlich.</span><span class="sxs-lookup"><span data-stu-id="282d1-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="282d1-244">Die Datei mit Geheimnissen weist das folgende Format:</span><span class="sxs-lookup"><span data-stu-id="282d1-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="282d1-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="282d1-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="282d1-246">Die [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) gespeicherten Werte liest die [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="282d1-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="282d1-247">`vaultName` ist erforderlich, entweder (der Name des Tresors) oder ein URI in den Tresor.</span><span class="sxs-lookup"><span data-stu-id="282d1-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="282d1-248">Die anderen Attribute ermöglichen die Steuerung zu welchem Tresor für die Verbindung, jedoch nur dann erforderlich sind, wenn die Anwendung nicht in einer Umgebung ausgeführt wird, die mit `Microsoft.Azure.Services.AppAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="282d1-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="282d1-249">Die Authentifizierungsbibliothek von Azure-Dienste werden automatisch um Verbindungsinformationen aus die ausführungsumgebung möglichst zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="282d1-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="282d1-250">Sie können automatisch überschreiben durch eine Verbindungszeichenfolge Bereitstellen von Verbindungsinformationen übernehmen.</span><span class="sxs-lookup"><span data-stu-id="282d1-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="282d1-251">`vaultName` -Erforderlich, wenn `uri` in nicht angegeben.</span><span class="sxs-lookup"><span data-stu-id="282d1-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="282d1-252">Gibt den Namen des Tresors in Ihrem Azure-Abonnement aus dem Schlüssel/Wert-Paare gelesen.</span><span class="sxs-lookup"><span data-stu-id="282d1-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="282d1-253">`connectionString` -Eine Verbindungszeichenfolge, die verwendet werden, indem [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="282d1-253">`connectionString` - A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* <span data-ttu-id="282d1-254">`uri` -Eine Verbindung mit anderen Key Vault-Anbieter mit dem angegebenen `uri` Wert.</span><span class="sxs-lookup"><span data-stu-id="282d1-254">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="282d1-255">Wenn nicht angegeben ist, Azure (`vaultName`) ist die schlüsseltresor-Anbieters.</span><span class="sxs-lookup"><span data-stu-id="282d1-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="282d1-256">`version` – Azure Key Vault bietet eine versionsverwaltung für geheime Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="282d1-256">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="282d1-257">Wenn `version` angegeben ist, wird der Generator ruft nur Geheimnisse, die diese Version entspricht.</span><span class="sxs-lookup"><span data-stu-id="282d1-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="282d1-258">`preloadSecretNames` -Wird standardmäßig dieser Generator Abfragen **alle** Schlüssel Namen in den schlüsseltresor aus, wenn es initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="282d1-258">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="282d1-259">Legen Sie dieses Attribut auf, um zu verhindern, lesen alle Schlüsselwerte, `false`.</span><span class="sxs-lookup"><span data-stu-id="282d1-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="282d1-260">Wenn dies auf `false` geheime Schlüssel 1 zu einem Zeitpunkt liest.</span><span class="sxs-lookup"><span data-stu-id="282d1-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="282d1-261">Lesen von Geheimnissen, die jeweils einzeln kann sinnvoll sein, wenn im Tresor ermöglicht den Zugriff "Abrufen", aber nicht "List" zugreifen.</span><span class="sxs-lookup"><span data-stu-id="282d1-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="282d1-262">**Hinweis:** Verwendung `Greedy` Modus `preloadSecretNames` muss `true` (die Standardeinstellung.)</span><span class="sxs-lookup"><span data-stu-id="282d1-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="282d1-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="282d1-263">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="282d1-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) ein Basiskonfiguration-Generator, der einem Verzeichnis enthaltenen Dateien als Quelle für Werte verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="282d1-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="282d1-265">Der Name einer Datei ist der Schlüssel, und der Inhalt ist der Wert.</span><span class="sxs-lookup"><span data-stu-id="282d1-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="282d1-266">Diese Konfigurations-Generator kann nützlich sein, wenn in einem orchestrierten containerumgebung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="282d1-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="282d1-267">Systeme, z. B. das Bereitstellen von Docker Swarm und Kubernetes `secrets` für ihre orchestrierten Windows-Container auf diese Weise Schlüssel pro Datei.</span><span class="sxs-lookup"><span data-stu-id="282d1-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="282d1-268">Attributdetails:</span><span class="sxs-lookup"><span data-stu-id="282d1-268">Attribute details:</span></span>

* <span data-ttu-id="282d1-269">`directoryPath` – Erforderlich.</span><span class="sxs-lookup"><span data-stu-id="282d1-269">`directoryPath` - Required.</span></span> <span data-ttu-id="282d1-270">Gibt einen Pfad nach Werten gesucht werden soll.</span><span class="sxs-lookup"><span data-stu-id="282d1-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="282d1-271">Docker für Windows, die vertraulichen Informationen sich in befinden der *C:\ProgramData\Docker\secrets* Verzeichnis standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="282d1-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="282d1-272">`ignorePrefix` -Dateien, die mit diesem Präfix beginnen, werden ausgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="282d1-272">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="282d1-273">Der Standardwert ist "ignorieren".</span><span class="sxs-lookup"><span data-stu-id="282d1-273">Defaults to "ignore.".</span></span>
* <span data-ttu-id="282d1-274">`keyDelimiter` – Der Standardwert ist `null`.</span><span class="sxs-lookup"><span data-stu-id="282d1-274">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="282d1-275">Wenn angegeben, durchsucht der Konfigurations-Generator mehrere Ebenen des Verzeichnisses, Schlüsselnamen mit diesem Trennzeichen erstellen.</span><span class="sxs-lookup"><span data-stu-id="282d1-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="282d1-276">Wenn dieser Wert ist `null`, sieht der Konfigurations-Generator nur auf der obersten Ebene des Verzeichnisses.</span><span class="sxs-lookup"><span data-stu-id="282d1-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="282d1-277">`optional` – Der Standardwert ist `false`.</span><span class="sxs-lookup"><span data-stu-id="282d1-277">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="282d1-278">Gibt an, ob der Konfigurations-Generator zu Fehlern führen sollten, wenn das Quellverzeichnis nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="282d1-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="282d1-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="282d1-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="282d1-280">Nie Speichern von Kennwörtern, vertrauliche Verbindungszeichenfolgen und andere sensiblen Daten im Quellcode.</span><span class="sxs-lookup"><span data-stu-id="282d1-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="282d1-281">Produktionsgeheimnisse sollte nicht verwendet werden zu Entwicklungs- oder Testzwecken.</span><span class="sxs-lookup"><span data-stu-id="282d1-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="282d1-282">.NET Core-Projekte verwenden häufig die JSON-Dateien für die Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="282d1-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="282d1-283">Die [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) -Generator können Sie .NET Core-JSON-Dateien, die in .NET Framework verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="282d1-284">Diese Konfigurations-Generator ist, stellt eine einfache Zuordnung zwischen einer einfache Schlüssel/Wert-Quelle in bestimmten Schlüssel/Wert-Bereiche von .NET Framework-Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="282d1-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="282d1-285">Diese Konfigurations-Generator ist **nicht** für hierarchische Konfigurationen bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="282d1-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="282d1-286">Die zugrunde liegende JSON-Datei ähnelt einem Wörterbuch vorhanden ist, keine komplexe hierarchische-Objekt.</span><span class="sxs-lookup"><span data-stu-id="282d1-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="282d1-287">Eine hierarchische-Datei mit mehreren Ebene kann verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="282d1-288">Dieser Anbieter `flatten`s die Tiefe durch den Namen der Eigenschaft auf jeder Ebene mit anhängen `:` als Trennzeichen.</span><span class="sxs-lookup"><span data-stu-id="282d1-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="282d1-289">Attributdetails:</span><span class="sxs-lookup"><span data-stu-id="282d1-289">Attribute details:</span></span>

* <span data-ttu-id="282d1-290">`jsonFile` – Erforderlich.</span><span class="sxs-lookup"><span data-stu-id="282d1-290">`jsonFile` - Required.</span></span> <span data-ttu-id="282d1-291">Gibt an, die JSON-Datei gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="282d1-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="282d1-292">Die `~` Zeichen kann zu Beginn verwendet werden, auf das um Stammverzeichnis der app zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="282d1-292">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="282d1-293">`optional` – Boolesch, Standardwert ist `true`.</span><span class="sxs-lookup"><span data-stu-id="282d1-293">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="282d1-294">Verhindert, dass Ausnahmen auslösen, wenn die JSON-Datei nicht gefunden werden kann.</span><span class="sxs-lookup"><span data-stu-id="282d1-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="282d1-295">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="282d1-295">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="282d1-296">Standardmäßig ist `Flat` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="282d1-296">`Flat` is the default.</span></span> <span data-ttu-id="282d1-297">Wenn `jsonMode` ist `Flat`, die JSON-Datei ist eine einzelne einfache Schlüssel/Wert-Quelle.</span><span class="sxs-lookup"><span data-stu-id="282d1-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="282d1-298">Die `EnvironmentConfigBuilder` und `AzureKeyVaultConfigBuilder` sind auch einzelne einfache Schlüssel/Wert-Quellen.</span><span class="sxs-lookup"><span data-stu-id="282d1-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="282d1-299">Wenn die `SimpleJsonConfigBuilder` erfolgt `Sectional` Modus:</span><span class="sxs-lookup"><span data-stu-id="282d1-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="282d1-300">Die JSON-Datei ist im Prinzip nur auf der obersten Ebene in mehreren Wörterbüchern unterteilt.</span><span class="sxs-lookup"><span data-stu-id="282d1-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="282d1-301">Jede der Wörterbücher wird nur auf den Konfigurationsabschnitt angewendet, die die Eigenschaft der obersten Ebene Namens auf diese entspricht.</span><span class="sxs-lookup"><span data-stu-id="282d1-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="282d1-302">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="282d1-302">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="282d1-303">Implementieren einen benutzerdefinierte Schlüssel-Wert-Konfigurations-Generator</span><span class="sxs-lookup"><span data-stu-id="282d1-303">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="282d1-304">Wenn der Konfigurations-Generatoren nicht Ihren Anforderungen entsprechen, können Sie eine benutzerdefinierte Aktivität schreiben.</span><span class="sxs-lookup"><span data-stu-id="282d1-304">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="282d1-305">Die `KeyValueConfigBuilder` Basisklasse behandelt das Ersatz-Modi und die meisten Präfix Bedenken.</span><span class="sxs-lookup"><span data-stu-id="282d1-305">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="282d1-306">Es müssen nur ein Projekt implementieren:</span><span class="sxs-lookup"><span data-stu-id="282d1-306">An implementing project need only:</span></span>

* <span data-ttu-id="282d1-307">Von der Basisklasse erben, und implementieren Sie eine grundlegende Quelle von Schlüssel/Wert-Paaren, durch die `GetValue` und `GetAllValues`:</span><span class="sxs-lookup"><span data-stu-id="282d1-307">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="282d1-308">Hinzufügen der [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) zum Projekt.</span><span class="sxs-lookup"><span data-stu-id="282d1-308">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="282d1-309">Die `KeyValueConfigBuilder` Basisklasse bietet im Wesentlichen das Geschäfts-, Schul- oder konsistent Verhalten für Schlüssel/Wert-Konfigurations-Generatoren.</span><span class="sxs-lookup"><span data-stu-id="282d1-309">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="282d1-310">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="282d1-310">Additional resources</span></span>

* [<span data-ttu-id="282d1-311">Konfigurations-Generatoren-GitHub-repository</span><span class="sxs-lookup"><span data-stu-id="282d1-311">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="282d1-312">Dienst-zu-Dienst-Authentifizierung in Azure Key Vault mithilfe von .NET</span><span class="sxs-lookup"><span data-stu-id="282d1-312">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
