---
title: Konfiguration in ASP.NET Core
author: guardrex
description: In diesem Artikel erfahren Sie, wie Sie mit der Konfigurations-API eine ASP.NET Core-App konfigurieren.
ms.author: riande
ms.custom: mvc
ms.date: 11/15/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 766ac77a2af01509f8e4bc646a18f7dfbc923511
ms.sourcegitcommit: d3392f688cfebc1f25616da7489664d69c6ee330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2018
ms.locfileid: "51818394"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="92617-103">Konfiguration in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="92617-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="92617-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="92617-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="92617-105">Die App-Konfiguration in ASP.NET Core basiert auf Schlüssel-Wert-Paaren, die von *Konfigurationsanbietern* erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="92617-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="92617-106">Konfigurationsanbieter lesen Konfigurationsdaten in Schlüssel-Wert-Paare aus verschiedenen Konfigurationsquellen:</span><span class="sxs-lookup"><span data-stu-id="92617-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="92617-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="92617-107">Azure Key Vault</span></span>
* <span data-ttu-id="92617-108">Befehlszeilenargumente</span><span class="sxs-lookup"><span data-stu-id="92617-108">Command-line arguments</span></span>
* <span data-ttu-id="92617-109">Benutzerdefinierte Anbieter (installiert oder erstellt)</span><span class="sxs-lookup"><span data-stu-id="92617-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="92617-110">Verzeichnisdateien</span><span class="sxs-lookup"><span data-stu-id="92617-110">Directory files</span></span>
* <span data-ttu-id="92617-111">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="92617-111">Environment variables</span></span>
* <span data-ttu-id="92617-112">Speicherinterne .NET Objekte</span><span class="sxs-lookup"><span data-stu-id="92617-112">In-memory .NET objects</span></span>
* <span data-ttu-id="92617-113">Einstellungsdateien</span><span class="sxs-lookup"><span data-stu-id="92617-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="92617-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="92617-114">Azure Key Vault</span></span>
* <span data-ttu-id="92617-115">Befehlszeilenargumente</span><span class="sxs-lookup"><span data-stu-id="92617-115">Command-line arguments</span></span>
* <span data-ttu-id="92617-116">Benutzerdefinierte Anbieter (installiert oder erstellt)</span><span class="sxs-lookup"><span data-stu-id="92617-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="92617-117">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="92617-117">Environment variables</span></span>
* <span data-ttu-id="92617-118">Speicherinterne .NET Objekte</span><span class="sxs-lookup"><span data-stu-id="92617-118">In-memory .NET objects</span></span>
* <span data-ttu-id="92617-119">Einstellungsdateien</span><span class="sxs-lookup"><span data-stu-id="92617-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="92617-120">Befehlszeilenargumente</span><span class="sxs-lookup"><span data-stu-id="92617-120">Command-line arguments</span></span>
* <span data-ttu-id="92617-121">Benutzerdefinierte Anbieter (installiert oder erstellt)</span><span class="sxs-lookup"><span data-stu-id="92617-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="92617-122">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="92617-122">Environment variables</span></span>
* <span data-ttu-id="92617-123">Speicherinterne .NET Objekte</span><span class="sxs-lookup"><span data-stu-id="92617-123">In-memory .NET objects</span></span>
* <span data-ttu-id="92617-124">Einstellungsdateien</span><span class="sxs-lookup"><span data-stu-id="92617-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="92617-125">Das *Optionsmuster* ist eine Erweiterung der in diesem Thema beschriebenen Konfigurationskonzepte.</span><span class="sxs-lookup"><span data-stu-id="92617-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="92617-126">Optionsmuster verwenden Klassen, um Gruppen von zusammengehörigen Einstellungen darzustellen.</span><span class="sxs-lookup"><span data-stu-id="92617-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="92617-127">Weitere Informationen zum Verwenden von Optionsmustern finden Sie unter <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="92617-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="92617-128">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="92617-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="92617-129">Für die in diesem Artikel genannten Beispiele müssen folgende Bedingungen erfüllt sein:</span><span class="sxs-lookup"><span data-stu-id="92617-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="92617-130">Der Basispfad der App wurde mit <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> festgelegt.</span><span class="sxs-lookup"><span data-stu-id="92617-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="92617-131">Eine App kann durch den Verweis auf das Paket [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) auf `SetBasePath` zugreifen.</span><span class="sxs-lookup"><span data-stu-id="92617-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="92617-132">Abschnitte von Konfigurationsdateien werden mit <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="92617-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="92617-133">Eine App kann durch den Verweis auf das Paket [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) auf `GetSection` zugreifen.</span><span class="sxs-lookup"><span data-stu-id="92617-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="92617-134">Die Konfiguration wird mit <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> und [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) an .NET-Klassen gebunden.</span><span class="sxs-lookup"><span data-stu-id="92617-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="92617-135">Eine App kann durch den Verweis auf das Paket [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) auf `Bind` und `Get<T>` zugreifen.</span><span class="sxs-lookup"><span data-stu-id="92617-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="92617-136">`Get<T>` ist in ASP.NET Core 1.1 und höher verfügbar.</span><span class="sxs-lookup"><span data-stu-id="92617-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="92617-137">Diese drei Pakete sind im Metapaket [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) enthalten.</span><span class="sxs-lookup"><span data-stu-id="92617-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="92617-138">Diese drei Pakete sind im Metapaket [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) enthalten.</span><span class="sxs-lookup"><span data-stu-id="92617-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="92617-139">Hostkonfiguration vs. App-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="92617-139">Host vs. app configuration</span></span>

<span data-ttu-id="92617-140">Bevor die App konfiguriert und gestartet wird, wird ein *Host* konfiguriert und gestartet.</span><span class="sxs-lookup"><span data-stu-id="92617-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="92617-141">Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="92617-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="92617-142">Die App und der Host werden mit den in diesem Thema beschriebenen Konfigurationsanbietern konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="92617-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="92617-143">Schlüssel-Wert-Paare der Hostkonfiguration werden Teil der globalen App-Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="92617-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="92617-144">Weitere Informationen dazu, wie Konfigurationsanbieter beim Erstellen des Hosts verwendet werden, und wie sich Konfigurationsquellen auf die Hostkonfiguration auswirken, finden Sie unter <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="92617-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="92617-145">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="92617-145">Security</span></span>

<span data-ttu-id="92617-146">Verwenden Sie die folgenden Best Practices:</span><span class="sxs-lookup"><span data-stu-id="92617-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="92617-147">Speichern Sie nie Kennwörter oder andere vertrauliche Daten im Konfigurationsanbietercode oder in Nur-Text-Konfigurationsdateien.</span><span class="sxs-lookup"><span data-stu-id="92617-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="92617-148">Verwenden Sie keine Produktionsgeheimnisse in Entwicklungs- oder Testumgebungen.</span><span class="sxs-lookup"><span data-stu-id="92617-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="92617-149">Geben Sie Geheimnisse außerhalb des Projekts an, damit sie nicht versehentlich in ein Quellcoderepository übernommen werden können.</span><span class="sxs-lookup"><span data-stu-id="92617-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="92617-150">Lesen Sie mehr über das [Verwenden von mehreren Umgebungen](xref:fundamentals/environments) und das Verwalten der [sicheren Speicherung von App-Geheimnissen bei der Entwicklung mit dem Geheimnis-Manager](xref:security/app-secrets) (mit Informationen zum Speichern vertraulicher Daten mit Umgebungsvariablen).</span><span class="sxs-lookup"><span data-stu-id="92617-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="92617-151">Der Geheimnis-Manager verwendet den Dateikonfigurationsanbieter, um Benutzergeheimnisse in einer JSON-Datei im lokalen System zu speichern.</span><span class="sxs-lookup"><span data-stu-id="92617-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="92617-152">Der Dateikonfigurationsanbieter wird später in diesem Thema beschrieben.</span><span class="sxs-lookup"><span data-stu-id="92617-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="92617-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stellt eine Option für die sichere Speicherung von App-Geheimnissen dar.</span><span class="sxs-lookup"><span data-stu-id="92617-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="92617-154">Weitere Informationen finden Sie unter <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="92617-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="92617-155">Hierarchische Konfigurationsdaten</span><span class="sxs-lookup"><span data-stu-id="92617-155">Hierarchical configuration data</span></span>

<span data-ttu-id="92617-156">Die Konfigurations-API kann hierarchische Konfigurationsdaten erhalten, indem sie die hierarchischen Daten mit einem Trennzeichen in den Konfigurationsschlüsseln vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="92617-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="92617-157">Die folgende JSON-Datei enthält vier Schlüssel in einer strukturierten Hierarchie von zwei Abschnitten:</span><span class="sxs-lookup"><span data-stu-id="92617-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="92617-158">Wenn die Datei in die Konfiguration gelesen wird, werden eindeutige Schlüssel erstellt, um die ursprüngliche hierarchische Datenstruktur der Konfigurationsquelle zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="92617-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="92617-159">Die Abschnitte und Schlüssel werden mithilfe eines Doppelpunkts (`:`) vereinfacht, um die Originalstruktur zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="92617-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="92617-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="92617-160">section0:key0</span></span>
* <span data-ttu-id="92617-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="92617-161">section0:key1</span></span>
* <span data-ttu-id="92617-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="92617-162">section1:key0</span></span>
* <span data-ttu-id="92617-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="92617-163">section1:key1</span></span>

<span data-ttu-id="92617-164">Mit den Methoden <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> und <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> können Abschnitte und untergeordnete Abschnittelemente in den Konfigurationsdaten isoliert werden.</span><span class="sxs-lookup"><span data-stu-id="92617-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="92617-165">Diese Methoden werden später im Abschnitt [GetSection, GetChildren und Exists](#getsection-getchildren-and-exists) beschrieben.</span><span class="sxs-lookup"><span data-stu-id="92617-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="92617-166">Konventionen</span><span class="sxs-lookup"><span data-stu-id="92617-166">Conventions</span></span>

<span data-ttu-id="92617-167">Beim Starten der Anwendung werden Konfigurationsquellen in der Reihenfolge gelesen, in der ihre Konfigurationsanbieter angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="92617-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="92617-168">Dateikonfigurationsanbieter können Konfigurationen erneut laden, wenn eine zugrunde liegende Einstellungsdatei nach dem App-Start geändert wird.</span><span class="sxs-lookup"><span data-stu-id="92617-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="92617-169">Der Dateikonfigurationsanbieter wird später in diesem Thema beschrieben.</span><span class="sxs-lookup"><span data-stu-id="92617-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="92617-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> steht im App-Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="92617-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="92617-171">Konfigurationsanbieter können die Abhängigkeitsinjektion nicht verwenden, weil sie nicht verfügbar ist, wenn sie vom Host eingerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="92617-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="92617-172">Konfigurationsschlüssel entsprechen den folgenden Konventionen:</span><span class="sxs-lookup"><span data-stu-id="92617-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="92617-173">Bei Schlüsseln wird die Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="92617-173">Keys are case-insensitive.</span></span> <span data-ttu-id="92617-174">Beispielsweise verweisen `ConnectionString` und `connectionstring` auf denselben Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="92617-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="92617-175">Wenn ein Wert für denselben Schlüssel von denselben oder unterschiedlichen Konfigurationsanbietern festgelegt wird, wird der zuletzt für den Schlüssel bestimmte Wert verwendet.</span><span class="sxs-lookup"><span data-stu-id="92617-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="92617-176">Hierarchische Schlüssel</span><span class="sxs-lookup"><span data-stu-id="92617-176">Hierarchical keys</span></span>
  * <span data-ttu-id="92617-177">Innerhalb der Konfigurations-API funktioniert ein Doppelpunkt (`:`) als Trennzeichen auf allen Plattformen.</span><span class="sxs-lookup"><span data-stu-id="92617-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="92617-178">In Umgebungsvariablen funktioniert ein Doppelpunkt als Trennzeichen ggf. nicht auf allen Plattformen.</span><span class="sxs-lookup"><span data-stu-id="92617-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="92617-179">Ein doppelter Unterstrich (`__`) wird auf allen Plattformen unterstützt und in einen Doppelpunkt konvertiert.</span><span class="sxs-lookup"><span data-stu-id="92617-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="92617-180">In Azure Key Vault verwenden hierarchische Schlüssel zwei Bindestriche (`--`) als Trennzeichen.</span><span class="sxs-lookup"><span data-stu-id="92617-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="92617-181">Sie müssen einen Code bereitstellen, um die Bindestriche durch einen Doppelpunkt zu ersetzen, wenn die Geheimnisse in die App-Konfiguration geladen werden.</span><span class="sxs-lookup"><span data-stu-id="92617-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="92617-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> unterstützt das Binden von Arrays an Objekte mit Arrayindizes in Konfigurationsschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="92617-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="92617-183">Die Arraybindung wird im Abschnitt [Binden eines Arrays an eine Klasse](#bind-an-array-to-a-class) beschrieben.</span><span class="sxs-lookup"><span data-stu-id="92617-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="92617-184">Konfigurationswerte entsprechen den folgenden Konventionen:</span><span class="sxs-lookup"><span data-stu-id="92617-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="92617-185">Die Werte sind Zeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="92617-185">Values are strings.</span></span>
* <span data-ttu-id="92617-186">NULL-Werte können nicht in einer Konfiguration gespeichert oder an Objekte gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="92617-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="92617-187">Anbieter</span><span class="sxs-lookup"><span data-stu-id="92617-187">Providers</span></span>

<span data-ttu-id="92617-188">Die folgende Tabelle zeigt die für ASP.NET Core-Apps verfügbaren Konfigurationsanbieter.</span><span class="sxs-lookup"><span data-stu-id="92617-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="92617-189">Anbieter</span><span class="sxs-lookup"><span data-stu-id="92617-189">Provider</span></span> | <span data-ttu-id="92617-190">Bereitstellung der Konfiguration über&hellip;</span><span class="sxs-lookup"><span data-stu-id="92617-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="92617-191">[Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration) (Thema *Sicherheit*)</span><span class="sxs-lookup"><span data-stu-id="92617-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="92617-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="92617-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="92617-193">Befehlszeilen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="92617-194">Befehlszeilenparameter</span><span class="sxs-lookup"><span data-stu-id="92617-194">Command-line parameters</span></span> |
| [<span data-ttu-id="92617-195">Benutzerdefinierter Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="92617-196">Benutzerdefinierte Quelle</span><span class="sxs-lookup"><span data-stu-id="92617-196">Custom source</span></span> |
| [<span data-ttu-id="92617-197">Umgebungsvariablen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-197">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="92617-198">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="92617-198">Environment variables</span></span> |
| [<span data-ttu-id="92617-199">Dateikonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="92617-200">Dateien (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="92617-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="92617-201">Schlüssel-pro-Datei-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="92617-202">Verzeichnisdateien</span><span class="sxs-lookup"><span data-stu-id="92617-202">Directory files</span></span> |
| [<span data-ttu-id="92617-203">Speicherkonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="92617-204">In-Memory-Sammlungen</span><span class="sxs-lookup"><span data-stu-id="92617-204">In-memory collections</span></span> |
| <span data-ttu-id="92617-205">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (Thema *Sicherheit*)</span><span class="sxs-lookup"><span data-stu-id="92617-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="92617-206">Datei im Benutzerprofilverzeichnis</span><span class="sxs-lookup"><span data-stu-id="92617-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="92617-207">Anbieter</span><span class="sxs-lookup"><span data-stu-id="92617-207">Provider</span></span> | <span data-ttu-id="92617-208">Bereitstellung der Konfiguration über&hellip;</span><span class="sxs-lookup"><span data-stu-id="92617-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="92617-209">[Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration) (Thema *Sicherheit*)</span><span class="sxs-lookup"><span data-stu-id="92617-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="92617-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="92617-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="92617-211">Befehlszeilen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="92617-212">Befehlszeilenparameter</span><span class="sxs-lookup"><span data-stu-id="92617-212">Command-line parameters</span></span> |
| [<span data-ttu-id="92617-213">Benutzerdefinierter Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="92617-214">Benutzerdefinierte Quelle</span><span class="sxs-lookup"><span data-stu-id="92617-214">Custom source</span></span> |
| [<span data-ttu-id="92617-215">Umgebungsvariablen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="92617-216">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="92617-216">Environment variables</span></span> |
| [<span data-ttu-id="92617-217">Dateikonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="92617-218">Dateien (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="92617-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="92617-219">Speicherkonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="92617-220">In-Memory-Sammlungen</span><span class="sxs-lookup"><span data-stu-id="92617-220">In-memory collections</span></span> |
| <span data-ttu-id="92617-221">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (Thema *Sicherheit*)</span><span class="sxs-lookup"><span data-stu-id="92617-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="92617-222">Datei im Benutzerprofilverzeichnis</span><span class="sxs-lookup"><span data-stu-id="92617-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="92617-223">Anbieter</span><span class="sxs-lookup"><span data-stu-id="92617-223">Provider</span></span> | <span data-ttu-id="92617-224">Bereitstellung der Konfiguration über&hellip;</span><span class="sxs-lookup"><span data-stu-id="92617-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="92617-225">Befehlszeilen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="92617-226">Befehlszeilenparameter</span><span class="sxs-lookup"><span data-stu-id="92617-226">Command-line parameters</span></span> |
| [<span data-ttu-id="92617-227">Benutzerdefinierter Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="92617-228">Benutzerdefinierte Quelle</span><span class="sxs-lookup"><span data-stu-id="92617-228">Custom source</span></span> |
| [<span data-ttu-id="92617-229">Umgebungsvariablen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-229">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="92617-230">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="92617-230">Environment variables</span></span> |
| [<span data-ttu-id="92617-231">Dateikonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="92617-232">Dateien (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="92617-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="92617-233">Speicherkonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="92617-234">In-Memory-Sammlungen</span><span class="sxs-lookup"><span data-stu-id="92617-234">In-memory collections</span></span> |
| <span data-ttu-id="92617-235">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (Thema *Sicherheit*)</span><span class="sxs-lookup"><span data-stu-id="92617-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="92617-236">Datei im Benutzerprofilverzeichnis</span><span class="sxs-lookup"><span data-stu-id="92617-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="92617-237">Konfigurationsquellen werden beim Start in der Reihenfolge gelesen, in der ihre Konfigurationsanbieter angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="92617-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="92617-238">Die in diesem Thema beschriebenen Konfigurationsanbieter werden in alphabetischer Reihenfolge beschrieben, nicht in der Reihenfolge, in der Ihr Code sie anordnet.</span><span class="sxs-lookup"><span data-stu-id="92617-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="92617-239">Ordnen Sie die Konfigurationsanbieter in Ihrem Code so an, dass sie den Prioritäten für die zugrunde liegenden Konfigurationsquellen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="92617-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="92617-240">Eine typische Konfigurationsanbietersequenz ist:</span><span class="sxs-lookup"><span data-stu-id="92617-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="92617-241">Dateien (*appsettings.json*, *appsettings.{Environment}.json*, wobei `{Environment}` die aktuelle Hostingumgebung der App ist)</span><span class="sxs-lookup"><span data-stu-id="92617-241">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="92617-242">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="92617-242">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="92617-243">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (nur in der Entwicklungsumgebung)</span><span class="sxs-lookup"><span data-stu-id="92617-243">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="92617-244">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="92617-244">Environment variables</span></span>
1. <span data-ttu-id="92617-245">Befehlszeilenargumente</span><span class="sxs-lookup"><span data-stu-id="92617-245">Command-line arguments</span></span>

<span data-ttu-id="92617-246">In der Regel werden Befehlszeilen-Konfigurationsanbieter zuletzt in einer Reihe von Anbietern positioniert, damit Befehlszeilenargumente die von anderen Anbietern festgelegte Konfiguration überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="92617-246">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="92617-247">Diese Anbieterreihenfolge wird beim Initialisieren eines neuen <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>-Elements mit <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>festgelegt.</span><span class="sxs-lookup"><span data-stu-id="92617-247">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="92617-248">Weitere Informationen finden Sie unter [Webhost: Einrichten eines Hosts](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="92617-248">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="92617-249">Diese Sequenz von Anbietern kann für die App (nicht für den Host) mit <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> und einem Aufruf der <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*>-Methode in `Startup` erstellt werden:</span><span class="sxs-lookup"><span data-stu-id="92617-249">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

<span data-ttu-id="92617-250">Im vorhergehenden Beispiel werden der Umgebungsname (`env.EnvironmentName`) und der Name der App-Assembly (`env.ApplicationName`) von <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="92617-250">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="92617-251">Weitere Informationen finden Sie unter <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="92617-251">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="92617-252">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="92617-252">ConfigureAppConfiguration</span></span>

<span data-ttu-id="92617-253">Rufen Sie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> beim Erstellen des Webhosts auf, um die Konfigurationsanbieter der App zusätzlich zu denen anzugeben, die automatisch von <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="92617-253">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="92617-254">Befehlszeilen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-254">Command-line Configuration Provider</span></span>

<span data-ttu-id="92617-255"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> lädt während der Laufzeit die Konfiguration aus den Schlüssel-Wert-Paaren des Befehlszeilenarguments.</span><span class="sxs-lookup"><span data-stu-id="92617-255">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="92617-256">Um die Befehlszeilenkonfiguration zu aktivieren, wird die <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>-Erweiterungsmethode für eine Instanz von <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="92617-256">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="92617-257">`AddCommandLine` wird automatisch aufgerufen, wenn Sie ein neues <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>-Element mit <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> initialisieren.</span><span class="sxs-lookup"><span data-stu-id="92617-257">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="92617-258">Weitere Informationen finden Sie unter [Webhost: Einrichten eines Hosts](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="92617-258">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="92617-259">`CreateDefaultBuilder` lädt außerdem:</span><span class="sxs-lookup"><span data-stu-id="92617-259">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="92617-260">Optionale Konfiguration aus *appsettings.json* und *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="92617-260">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="92617-261">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (in der Entwicklungsumgebung)</span><span class="sxs-lookup"><span data-stu-id="92617-261">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="92617-262">Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="92617-262">Environment variables.</span></span>

<span data-ttu-id="92617-263">`CreateDefaultBuilder` fügt den Befehlszeilen-Konfigurationsanbieter zuletzt hinzu.</span><span class="sxs-lookup"><span data-stu-id="92617-263">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="92617-264">Während der Laufzeit übergebene Befehlszeilenargumente überschreiben die von anderen Anbietern festgelegte Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="92617-264">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="92617-265">`CreateDefaultBuilder` wird aktiv, wenn der Host erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="92617-265">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="92617-266">Deswegen kann die durch `CreateDefaultBuilder` aktivierte Befehlszeilenkonfiguration die Konfiguration des Hosts beeinflussen.</span><span class="sxs-lookup"><span data-stu-id="92617-266">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="92617-267">Rufen Sie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> beim Erstellen des Hosts auf, um die Konfiguration der App anzugeben.</span><span class="sxs-lookup"><span data-stu-id="92617-267">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="92617-268">`AddCommandLine` wurde bereits von `CreateDefaultBuilder` aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="92617-268">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="92617-269">Wenn Sie die App-Konfiguration bereitstellen müssen und weiterhin in der Lage sein möchten, diese Konfiguration mit Befehlszeilenargumenten außer Kraft zu setzen, rufen Sie die zusätzlichen Anbieter der App in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> auf und rufen `AddCommandLine` zuletzt auf.</span><span class="sxs-lookup"><span data-stu-id="92617-269">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="92617-270">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-270">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="92617-271">Wenden Sie die Konfiguration mit der <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>-Methode auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an.</span><span class="sxs-lookup"><span data-stu-id="92617-271">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="92617-272">`AddCommandLine` wurde bereits von `CreateDefaultBuilder` aufgerufen, als `UseConfiguration` aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="92617-272">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="92617-273">Wenn Sie die App-Konfiguration bereitstellen müssen und weiterhin in der Lage sein möchten, diese Konfiguration mit Befehlszeilenargumenten außer Kraft zu setzen, rufen Sie die zusätzlichen Anbieter der App für einen `ConfigurationBuilder` auf und rufen `AddCommandLine` zuletzt auf.</span><span class="sxs-lookup"><span data-stu-id="92617-273">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="92617-274">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-274">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="92617-275">Um die Befehlszeilenkonfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="92617-275">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="92617-276">Rufen Sie den Anbieter zuletzt auf, damit die während der Laufzeit übergebenen Befehlszeilenargumente die von anderen Konfigurationsanbietern bestimmte Konfiguration überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="92617-276">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="92617-277">Wenden Sie die Konfiguration mit der <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>-Methode auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an:</span><span class="sxs-lookup"><span data-stu-id="92617-277">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="92617-278">**Beispiel**</span><span class="sxs-lookup"><span data-stu-id="92617-278">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="92617-279">Die 2.x-Beispiel-App nutzt die statische Hilfsmethode `CreateDefaultBuilder`, um den Host zu erstellen, die einen Aufruf von <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> enthält.</span><span class="sxs-lookup"><span data-stu-id="92617-279">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="92617-280">Die 1.x-Beispiel-App ruft <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> auf <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> auf.</span><span class="sxs-lookup"><span data-stu-id="92617-280">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="92617-281">Öffnen Sie im Projektverzeichnis eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="92617-281">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="92617-282">Geben Sie ein Befehlszeilenargument für den `dotnet run`-Befehl an, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="92617-282">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="92617-283">Wenn die App ausgeführt wird, öffnen Sie einen Browser für die App unter `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="92617-283">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="92617-284">Beachten Sie, dass die Ausgabe das Schlüssel-Wert-Paar für das an `dotnet run` übergebene Konfigurations-Befehlszeilenargument enthält.</span><span class="sxs-lookup"><span data-stu-id="92617-284">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="92617-285">Argumente</span><span class="sxs-lookup"><span data-stu-id="92617-285">Arguments</span></span>

<span data-ttu-id="92617-286">Der Wert muss einem Gleichheitszeichen (`=`) folgen, oder der Schlüssel muss ein Präfix (`--` oder `/`) haben, wenn der Wert einem Leerzeichen folgt.</span><span class="sxs-lookup"><span data-stu-id="92617-286">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="92617-287">Der Wert kann NULL sein, wenn ein Gleichheitszeichen verwendet wird (z.B. `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="92617-287">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="92617-288">Schlüsselpräfix</span><span class="sxs-lookup"><span data-stu-id="92617-288">Key prefix</span></span>               | <span data-ttu-id="92617-289">Beispiel</span><span class="sxs-lookup"><span data-stu-id="92617-289">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="92617-290">Ohne Präfix</span><span class="sxs-lookup"><span data-stu-id="92617-290">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="92617-291">Zwei Gedankenstriche (`--`)</span><span class="sxs-lookup"><span data-stu-id="92617-291">Two dashes (`--`)</span></span>        | <span data-ttu-id="92617-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="92617-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="92617-293">Schrägstrich (`/`)</span><span class="sxs-lookup"><span data-stu-id="92617-293">Forward slash (`/`)</span></span>      | <span data-ttu-id="92617-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="92617-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="92617-295">Kombinieren Sie in einem Befehl nicht Schlüssel-Wert-Paare, die ein Gleichheitszeichen verwenden, mit Schlüssel-Wert-Paaren, die ein Leerzeichen verwenden.</span><span class="sxs-lookup"><span data-stu-id="92617-295">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="92617-296">Beispielbefehle:</span><span class="sxs-lookup"><span data-stu-id="92617-296">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="92617-297">Switchmappings</span><span class="sxs-lookup"><span data-stu-id="92617-297">Switch mappings</span></span>

<span data-ttu-id="92617-298">Switchmappings erlauben das Angeben einer Logik zum Ersetzen von Schlüsselnamen.</span><span class="sxs-lookup"><span data-stu-id="92617-298">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="92617-299">Wenn Sie die Konfiguration manuell mit <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> erstellen, können Sie ein Wörterbuch der Switchersetzungen für die <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>-Methode bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="92617-299">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="92617-300">Wenn das Switchmappingwörterbuch verwendet wird, wird das Wörterbuch für Schlüssel, die dem von einem Befehlszeilenargument angegebenen Schlüssel entsprechen, überprüft.</span><span class="sxs-lookup"><span data-stu-id="92617-300">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="92617-301">Wenn der Befehlszeilenschlüssel im Wörterbuch gefunden wird, wird der Wörterbuchwert (der Schlüsselersatz) zum Festlegen des Schlüssel-Wert-Paares in der App-Konfiguration zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="92617-301">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="92617-302">Ein Switchmapping ist für jeden Befehlszeilenschlüssel erforderlich, dem ein einzelner Gedankenstrich (`-`) vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="92617-302">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="92617-303">Regeln für Schlüssel von Switchmappingwörterbüchern:</span><span class="sxs-lookup"><span data-stu-id="92617-303">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="92617-304">Switchmappings müssen mit einem Gedankenstrich (`-`) oder zwei Gedankenstrichen (`--`) beginnen.</span><span class="sxs-lookup"><span data-stu-id="92617-304">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="92617-305">Das Switchmappingwörterbuch darf keine doppelten Schlüssel enthalten.</span><span class="sxs-lookup"><span data-stu-id="92617-305">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="92617-306">Rufen Sie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> beim Erstellen des Hosts auf, um die Konfiguration der App anzugeben:</span><span class="sxs-lookup"><span data-stu-id="92617-306">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="92617-307">Wie im vorherigen Beispiel gezeigt, darf der Aufruf von `CreateDefaultBuilder` keine Argumente übergeben, wenn Switchmappings verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="92617-307">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="92617-308">Der Aufruf `AddCommandLine` der `CreateDefaultBuilder`-Methode umfasst keine zugeordneten Switches, und das Switchmappingwörterbuch kann nicht an `CreateDefaultBuilder` übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="92617-308">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="92617-309">Wenn die Argumente einen zugeordneten Switch enthalten und an `CreateDefaultBuilder` übergeben werden, kann der `AddCommandLine`-Anbieter nicht mit <xref:System.FormatException> initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="92617-309">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="92617-310">Die Lösung besteht nicht darin, die Argumente an `CreateDefaultBuilder` zu übergeben, sondern der Methode `AddCommandLine` der `ConfigurationBuilder`-Methode zu erlauben, sowohl die Argumente als auch das Switchmappingwörterbuch zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="92617-310">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="92617-311">Wie im vorherigen Beispiel gezeigt, darf der Aufruf von `CreateDefaultBuilder` keine Argumente übergeben, wenn Switchmappings verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="92617-311">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="92617-312">Der Aufruf `AddCommandLine` der `CreateDefaultBuilder`-Methode umfasst keine zugeordneten Switches, und das Switchmappingwörterbuch kann nicht an `CreateDefaultBuilder` übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="92617-312">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="92617-313">Wenn die Argumente einen zugeordneten Switch enthalten und an `CreateDefaultBuilder` übergeben werden, kann der `AddCommandLine`-Anbieter nicht mit <xref:System.FormatException> initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="92617-313">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="92617-314">Die Lösung besteht nicht darin, die Argumente an `CreateDefaultBuilder` zu übergeben, sondern der Methode `AddCommandLine` der `ConfigurationBuilder`-Methode zu erlauben, sowohl die Argumente als auch das Switchmappingwörterbuch zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="92617-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

<span data-ttu-id="92617-315">Das erstellte Switchmappingwörterbuch enthält die in der folgenden Tabelle gezeigten Daten.</span><span class="sxs-lookup"><span data-stu-id="92617-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="92617-316">Key</span><span class="sxs-lookup"><span data-stu-id="92617-316">Key</span></span>       | <span data-ttu-id="92617-317">Wert</span><span class="sxs-lookup"><span data-stu-id="92617-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="92617-318">Wenn Switches zugeordnete Schlüssel beim Start der App verwendet werden, erhält die Konfiguration den Konfigurationswert auf dem vom Wörterbuch bereitgestellten Schlüssel:</span><span class="sxs-lookup"><span data-stu-id="92617-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="92617-319">Nach dem Ausführen des obigen Befehls enthält die Konfiguration die in der folgenden Tabelle angegebenen Werte.</span><span class="sxs-lookup"><span data-stu-id="92617-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="92617-320">Key</span><span class="sxs-lookup"><span data-stu-id="92617-320">Key</span></span>               | <span data-ttu-id="92617-321">Wert</span><span class="sxs-lookup"><span data-stu-id="92617-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="92617-322">Umgebungsvariablen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="92617-323"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> lädt während der Laufzeit die Konfiguration aus den Schlüssel-Wert-Paaren der Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="92617-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="92617-324">Um die Umgebungsvariablenkonfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="92617-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="92617-325">Beim Verwenden hierarchischer Schlüssel in Umgebungsvariablen funktioniert ein Doppelpunkt (`:`) als Trennzeichen ggf. nicht auf allen Plattformen.</span><span class="sxs-lookup"><span data-stu-id="92617-325">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="92617-326">Ein doppelter Unterstrich (`__`) wird auf allen Plattformen unterstützt und durch einen Doppelpunkt ersetzt.</span><span class="sxs-lookup"><span data-stu-id="92617-326">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="92617-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) ermöglicht Ihnen Umgebungsvariablen im Azure-Portal festzulegen, die die App-Konfiguration mit dem Umgebungsvariablen-Konfigurationsanbieter überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="92617-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="92617-328">Weitere Informationen finden Sie unter [Azure Apps: Überschreiben der App-Konfiguration mit dem Azure-Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="92617-328">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="92617-329">`AddEnvironmentVariables` wird für Umgebungsvariablen mit dem Präfix `ASPNETCORE_` automatisch aufgerufen, wenn Sie einen neuen <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> initialisieren.</span><span class="sxs-lookup"><span data-stu-id="92617-329">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="92617-330">Weitere Informationen finden Sie unter [Webhost: Einrichten eines Hosts](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="92617-330">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="92617-331">`CreateDefaultBuilder` lädt außerdem:</span><span class="sxs-lookup"><span data-stu-id="92617-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="92617-332">App-Konfiguration über Umgebungsvariablen ohne Präfix durch Aufruf von `AddEnvironmentVariables` ohne Präfix.</span><span class="sxs-lookup"><span data-stu-id="92617-332">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="92617-333">Optionale Konfiguration aus *appsettings.json* und *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="92617-333">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="92617-334">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (in der Entwicklungsumgebung)</span><span class="sxs-lookup"><span data-stu-id="92617-334">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="92617-335">Befehlszeilenargumenten</span><span class="sxs-lookup"><span data-stu-id="92617-335">Command-line arguments.</span></span>

<span data-ttu-id="92617-336">Der Umgebungsvariablen-Konfigurationsanbieter wird aufgerufen, nachdem die Konfiguration aus Benutzergeheimnissen und *appsettings*-Dateien erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="92617-336">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="92617-337">Das Aufrufen des Anbieters an dieser Stelle ermöglicht den während der Laufzeit gelesenen Umgebungsvariablen, die von Benutzergeheimnissen und *appsettings*-Dateien festgelegte Konfiguration zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="92617-337">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="92617-338">Rufen Sie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> beim Erstellen des Hosts auf, um die Konfiguration der App anzugeben.</span><span class="sxs-lookup"><span data-stu-id="92617-338">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="92617-339">Wenn Sie App-Konfigurationen aus zusätzlichen Umgebungsvariablen bereitstellen müssen, rufen Sie die zusätzlichen Anbieter der App in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> auf, und rufen Sie `AddEnvironmentVariables` mit dem Präfix auf.</span><span class="sxs-lookup"><span data-stu-id="92617-339">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="92617-340">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-340">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="92617-341">Rufen Sie die Erweiterungsmethode `AddEnvironmentVariables` für eine <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="92617-341">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="92617-342">Wenden Sie die Konfiguration mit der <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>-Methode auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an.</span><span class="sxs-lookup"><span data-stu-id="92617-342">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="92617-343">Wenn Sie App-Konfigurationen aus zusätzlichen Umgebungsvariablen bereitstellen müssen, rufen Sie die zusätzlichen Anbieter der App in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> auf, und rufen Sie `AddEnvironmentVariables` mit dem Präfix auf.</span><span class="sxs-lookup"><span data-stu-id="92617-343">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="92617-344">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-344">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="92617-345">Wenden Sie die Konfiguration mit der <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>-Methode auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an:</span><span class="sxs-lookup"><span data-stu-id="92617-345">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="92617-346">**Beispiel**</span><span class="sxs-lookup"><span data-stu-id="92617-346">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="92617-347">Die 2.x-Beispiel-App nutzt die statische Hilfsmethode `CreateDefaultBuilder`, um den Host zu erstellen, die einen Aufruf von `AddEnvironmentVariables` enthält.</span><span class="sxs-lookup"><span data-stu-id="92617-347">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="92617-348">Die 1.x-Beispiel-App ruft `AddEnvironmentVariables` auf `ConfigurationBuilder` auf.</span><span class="sxs-lookup"><span data-stu-id="92617-348">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="92617-349">Führen Sie die Beispiel-App aus.</span><span class="sxs-lookup"><span data-stu-id="92617-349">Run the sample app.</span></span> <span data-ttu-id="92617-350">Öffnen Sie einen Browser für die App unter `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="92617-350">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="92617-351">Beachten Sie, dass die Ausgabe das Schlüssel-Wert-Paar für die Umgebungsvariable `ENVIRONMENT` enthält.</span><span class="sxs-lookup"><span data-stu-id="92617-351">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="92617-352">Der Wert entspricht der Umgebung, in der die App ausgeführt wird. In der Regel ist das `Development` bei lokaler Ausführung.</span><span class="sxs-lookup"><span data-stu-id="92617-352">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="92617-353">Um die Liste der von der App gerenderten Umgebungsvariablen kurz zu halten, filtert die App Umgebungsvariablen, die folgendermaßen beginnen:</span><span class="sxs-lookup"><span data-stu-id="92617-353">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="92617-354">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="92617-354">ASPNETCORE_</span></span>
* <span data-ttu-id="92617-355">urls</span><span class="sxs-lookup"><span data-stu-id="92617-355">urls</span></span>
* <span data-ttu-id="92617-356">Protokollierung</span><span class="sxs-lookup"><span data-stu-id="92617-356">Logging</span></span>
* <span data-ttu-id="92617-357">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="92617-357">ENVIRONMENT</span></span>
* <span data-ttu-id="92617-358">contentRoot</span><span class="sxs-lookup"><span data-stu-id="92617-358">contentRoot</span></span>
* <span data-ttu-id="92617-359">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="92617-359">AllowedHosts</span></span>
* <span data-ttu-id="92617-360">applicationName</span><span class="sxs-lookup"><span data-stu-id="92617-360">applicationName</span></span>
* <span data-ttu-id="92617-361">COMMANDLINE</span><span class="sxs-lookup"><span data-stu-id="92617-361">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="92617-362">Wenn Sie alle für die App verfügbaren Umgebungsvariablen verfügbar machen möchten, ändern Sie `FilteredConfiguration` in *Pages/Index.cshtml.cs* wie folgt:</span><span class="sxs-lookup"><span data-stu-id="92617-362">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="92617-363">Wenn Sie alle für die App verfügbaren Umgebungsvariablen verfügbar machen möchten, ändern Sie `FilteredConfiguration` in *Controllers/HomeController.cs* wie folgt:</span><span class="sxs-lookup"><span data-stu-id="92617-363">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="92617-364">Präfixe</span><span class="sxs-lookup"><span data-stu-id="92617-364">Prefixes</span></span>

<span data-ttu-id="92617-365">Umgebungsvariablen, die in die Konfiguration der App geladen werden, werden gefiltert, wenn Sie ein Präfix für die Methode `AddEnvironmentVariables` angeben.</span><span class="sxs-lookup"><span data-stu-id="92617-365">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="92617-366">Um beispielsweise Umgebungsvariablen nach dem Präfix `CUSTOM_` zu filtern, geben Sie das Präfix für den Konfigurationsanbieter an:</span><span class="sxs-lookup"><span data-stu-id="92617-366">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="92617-367">Das Präfix wird beim Erstellen der Schlüssel-Wert-Paare der Konfiguration entfernt.</span><span class="sxs-lookup"><span data-stu-id="92617-367">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="92617-368">Die statische Hilfsmethode `CreateDefaultBuilder` erstellt ein <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>-Element, um den Host der App einzurichten.</span><span class="sxs-lookup"><span data-stu-id="92617-368">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="92617-369">Wenn `WebHostBuilder` erstellt wird, befindet sich die Hostkonfiguration in Umgebungsvariablen mit dem Präfix `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="92617-369">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="92617-370">**Präfixe für Verbindungszeichenfolgen**</span><span class="sxs-lookup"><span data-stu-id="92617-370">**Connection string prefixes**</span></span>

<span data-ttu-id="92617-371">Die Konfigurations-API verfügt über spezielle Verarbeitungsregeln für vier Umgebungsvariablen für Verbindungszeichenfolgen, die beim Konfigurieren von Azure-Verbindungszeichenfolgen für die App-Umgebung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="92617-371">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="92617-372">Umgebungsvariablen mit Präfixen, die in der Tabelle aufgeführt werden, werden in die App geladen, wenn für `AddEnvironmentVariables` kein Präfix angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="92617-372">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="92617-373">Präfix für Verbindungszeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="92617-373">Connection string prefix</span></span> | <span data-ttu-id="92617-374">Anbieter</span><span class="sxs-lookup"><span data-stu-id="92617-374">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="92617-375">Benutzerdefinierter Anbieter</span><span class="sxs-lookup"><span data-stu-id="92617-375">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="92617-376">MySQL</span><span class="sxs-lookup"><span data-stu-id="92617-376">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="92617-377">Azure SQL-Datenbank</span><span class="sxs-lookup"><span data-stu-id="92617-377">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="92617-378">SQL Server</span><span class="sxs-lookup"><span data-stu-id="92617-378">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="92617-379">Wenn eine Umgebungsvariable entdeckt und mit einem der vier Präfixe aus der Tabelle in die Konfiguration geladen wird, tritt Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="92617-379">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="92617-380">Der Konfigurationsschlüssel wird durch Entfernen des Umgebungsvariablenpräfixes und Hinzufügen eines Konfigurationsschlüsselabschnitts (`ConnectionStrings`) erstellt.</span><span class="sxs-lookup"><span data-stu-id="92617-380">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="92617-381">Ein neues Konfigurations-Schlüssel-Wert-Paar wird erstellt, das den Datenbankverbindungsanbieter repräsentiert (mit Ausnahme des Präfixes `CUSTOMCONNSTR_`, für das kein Anbieter angegeben ist).</span><span class="sxs-lookup"><span data-stu-id="92617-381">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="92617-382">Umgebungsvariablenschlüssel</span><span class="sxs-lookup"><span data-stu-id="92617-382">Environment variable key</span></span> | <span data-ttu-id="92617-383">Konvertierter Konfigurationsschlüssel</span><span class="sxs-lookup"><span data-stu-id="92617-383">Converted configuration key</span></span> | <span data-ttu-id="92617-384">Anbieterkonfigurationseintrag</span><span class="sxs-lookup"><span data-stu-id="92617-384">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="92617-385">Konfigurationseintrag wurde nicht erstellt.</span><span class="sxs-lookup"><span data-stu-id="92617-385">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="92617-386">Schlüssel: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="92617-386">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="92617-387">Wert: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="92617-387">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="92617-388">Schlüssel: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="92617-388">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="92617-389">Wert: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="92617-389">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="92617-390">Schlüssel: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="92617-390">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="92617-391">Wert: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="92617-391">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="92617-392">Dateikonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-392">File Configuration Provider</span></span>

<span data-ttu-id="92617-393"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> ist die Basisklasse für das Laden einer Konfiguration aus dem Dateisystem.</span><span class="sxs-lookup"><span data-stu-id="92617-393"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="92617-394">Die folgenden Konfigurationsanbieter sind für bestimmte Dateitypen vorgesehen:</span><span class="sxs-lookup"><span data-stu-id="92617-394">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="92617-395">INI-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-395">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="92617-396">JSON-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-396">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="92617-397">XML-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-397">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="92617-398">INI-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-398">INI Configuration Provider</span></span>

<span data-ttu-id="92617-399"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> lädt während der Laufzeit die Konfiguration aus den Schlüssel-Wert-Paaren der INI-Datei.</span><span class="sxs-lookup"><span data-stu-id="92617-399">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="92617-400">Um die INI-Dateikonfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="92617-400">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="92617-401">Der Doppelpunkt kann in einer INI-Dateikonfiguration als Abschnittstrennzeichen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="92617-401">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="92617-402">Überladungen geben Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="92617-402">Overloads permit specifying:</span></span>

* <span data-ttu-id="92617-403">Ob die Datei optional ist</span><span class="sxs-lookup"><span data-stu-id="92617-403">Whether the file is optional.</span></span>
* <span data-ttu-id="92617-404">Ob die Konfiguration bei Dateiänderungen neu geladen wird</span><span class="sxs-lookup"><span data-stu-id="92617-404">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="92617-405">Dass mit <xref:Microsoft.Extensions.FileProviders.IFileProvider> auf die Datei zugegriffen wird</span><span class="sxs-lookup"><span data-stu-id="92617-405">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="92617-406">Rufen Sie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> beim Erstellen des Hosts auf, um die Konfiguration der App anzugeben:</span><span class="sxs-lookup"><span data-stu-id="92617-406">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="92617-407">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-407">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="92617-408">Rufen Sie beim Aufrufen von `CreateDefaultBuilder` <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-408">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="92617-409">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-409">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="92617-410">Wenden Sie die Konfiguration mit der <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>-Methode auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an:</span><span class="sxs-lookup"><span data-stu-id="92617-410">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="92617-411">Ein allgemeines Beispiel einer INI-Konfigurationsdatei:</span><span class="sxs-lookup"><span data-stu-id="92617-411">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="92617-412">Die vorherige Konfigurationsdatei lädt die folgenden Schlüssel mit `value`:</span><span class="sxs-lookup"><span data-stu-id="92617-412">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="92617-413">section0:key0</span><span class="sxs-lookup"><span data-stu-id="92617-413">section0:key0</span></span>
* <span data-ttu-id="92617-414">section0:key1</span><span class="sxs-lookup"><span data-stu-id="92617-414">section0:key1</span></span>
* <span data-ttu-id="92617-415">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="92617-415">section1:subsection:key</span></span>
* <span data-ttu-id="92617-416">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="92617-416">section2:subsection0:key</span></span>
* <span data-ttu-id="92617-417">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="92617-417">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="92617-418">JSON-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-418">JSON Configuration Provider</span></span>

<span data-ttu-id="92617-419"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> lädt während der Laufzeit die Konfiguration aus den Schlüssel-Wert-Paaren der JSON-Datei.</span><span class="sxs-lookup"><span data-stu-id="92617-419">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="92617-420">Um die JSON-Dateikonfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="92617-420">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="92617-421">Überladungen geben Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="92617-421">Overloads permit specifying:</span></span>

* <span data-ttu-id="92617-422">Ob die Datei optional ist</span><span class="sxs-lookup"><span data-stu-id="92617-422">Whether the file is optional.</span></span>
* <span data-ttu-id="92617-423">Ob die Konfiguration bei Dateiänderungen neu geladen wird</span><span class="sxs-lookup"><span data-stu-id="92617-423">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="92617-424">Dass mit <xref:Microsoft.Extensions.FileProviders.IFileProvider> auf die Datei zugegriffen wird</span><span class="sxs-lookup"><span data-stu-id="92617-424">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="92617-425">`AddJsonFile` wird automatisch zweimal aufgerufen, wenn Sie ein neues <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>-Element mit <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> initialisieren.</span><span class="sxs-lookup"><span data-stu-id="92617-425">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="92617-426">Die Methode wird aufgerufen, um die Konfiguration aus folgenden Dateien zu laden:</span><span class="sxs-lookup"><span data-stu-id="92617-426">The method is called to load configuration from:</span></span>

* <span data-ttu-id="92617-427">*appsettings.json* &ndash; Diese Datei wird zuerst gelesen.</span><span class="sxs-lookup"><span data-stu-id="92617-427">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="92617-428">Die Umgebungsversion der Datei kann die Werte der Datei *appsettings.json* überschreiben.</span><span class="sxs-lookup"><span data-stu-id="92617-428">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="92617-429">*appsettings.{Environment}.json* &ndash; Die Umgebungsversion der Datei wird basierend auf [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*) geladen.</span><span class="sxs-lookup"><span data-stu-id="92617-429">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="92617-430">Weitere Informationen finden Sie unter [Webhost: Einrichten eines Hosts](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="92617-430">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="92617-431">`CreateDefaultBuilder` lädt außerdem:</span><span class="sxs-lookup"><span data-stu-id="92617-431">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="92617-432">Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="92617-432">Environment variables.</span></span>
* <span data-ttu-id="92617-433">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (in der Entwicklungsumgebung)</span><span class="sxs-lookup"><span data-stu-id="92617-433">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="92617-434">Befehlszeilenargumenten</span><span class="sxs-lookup"><span data-stu-id="92617-434">Command-line arguments.</span></span>

<span data-ttu-id="92617-435">Der JSON-Konfigurationsanbieter wird zuerst eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="92617-435">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="92617-436">Daher überschreiben Benutzergeheimnisse, Umgebungsvariablen und Befehlszeilenargumente die von *appsettings*-Dateien festgelegte Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="92617-436">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="92617-437">Rufen Sie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> beim Erstellen des Hosts auf, um Konfiguration der App für andere Dateien als *appsettings.json* und *appsettings.{Environment}.json* anzugeben:</span><span class="sxs-lookup"><span data-stu-id="92617-437">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="92617-438">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-438">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="92617-439">Sie können die Erweiterungsmethode `AddJsonFile` auch direkt auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz aufrufen.</span><span class="sxs-lookup"><span data-stu-id="92617-439">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="92617-440">Rufen Sie beim Aufrufen von `CreateDefaultBuilder` <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-440">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="92617-441">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-441">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="92617-442">Wenden Sie die Konfiguration mit der <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>-Methode auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an:</span><span class="sxs-lookup"><span data-stu-id="92617-442">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="92617-443">**Beispiel**</span><span class="sxs-lookup"><span data-stu-id="92617-443">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="92617-444">Die 2.x-Beispiel-App nutzt die statische Hilfsmethode `CreateDefaultBuilder`, um den Host zu erstellen, die zwei Aufrufe von `AddJsonFile` enthält.</span><span class="sxs-lookup"><span data-stu-id="92617-444">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="92617-445">Die Konfiguration wird aus *appsettings.json* und *appsettings.{Environment}.json* geladen.</span><span class="sxs-lookup"><span data-stu-id="92617-445">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="92617-446">Die 1.x-Beispiel-App ruft `AddJsonFile` zweimal auf `ConfigurationBuilder` auf.</span><span class="sxs-lookup"><span data-stu-id="92617-446">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="92617-447">Die Konfiguration wird aus *appsettings.json* und *appsettings.{Environment}.json* geladen.</span><span class="sxs-lookup"><span data-stu-id="92617-447">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="92617-448">Führen Sie die Beispiel-App aus.</span><span class="sxs-lookup"><span data-stu-id="92617-448">Run the sample app.</span></span> <span data-ttu-id="92617-449">Öffnen Sie einen Browser für die App unter `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="92617-449">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="92617-450">Die Ausgabe enthält je nach Umgebung Schlüssel-Wert-Paare für die in der Tabelle dargestellte Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="92617-450">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="92617-451">Protokollierungskonfigurationsschlüssel verwenden den Doppelpunkt (`:`) als hierarchisches Trennzeichen.</span><span class="sxs-lookup"><span data-stu-id="92617-451">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="92617-452">Key</span><span class="sxs-lookup"><span data-stu-id="92617-452">Key</span></span>                        | <span data-ttu-id="92617-453">Entwicklungswert</span><span class="sxs-lookup"><span data-stu-id="92617-453">Development Value</span></span> | <span data-ttu-id="92617-454">Produktionswert</span><span class="sxs-lookup"><span data-stu-id="92617-454">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="92617-455">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="92617-455">Logging:LogLevel:System</span></span>    | <span data-ttu-id="92617-456">Information</span><span class="sxs-lookup"><span data-stu-id="92617-456">Information</span></span>       | <span data-ttu-id="92617-457">Information</span><span class="sxs-lookup"><span data-stu-id="92617-457">Information</span></span>      |
| <span data-ttu-id="92617-458">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="92617-458">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="92617-459">Information</span><span class="sxs-lookup"><span data-stu-id="92617-459">Information</span></span>       | <span data-ttu-id="92617-460">Information</span><span class="sxs-lookup"><span data-stu-id="92617-460">Information</span></span>      |
| <span data-ttu-id="92617-461">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="92617-461">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="92617-462">Debug</span><span class="sxs-lookup"><span data-stu-id="92617-462">Debug</span></span>             | <span data-ttu-id="92617-463">Fehler</span><span class="sxs-lookup"><span data-stu-id="92617-463">Error</span></span>            |
| <span data-ttu-id="92617-464">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="92617-464">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="92617-465">XML-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-465">XML Configuration Provider</span></span>

<span data-ttu-id="92617-466"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> lädt während der Laufzeit die Konfiguration aus den Schlüssel-Wert-Paaren der XML-Datei.</span><span class="sxs-lookup"><span data-stu-id="92617-466">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="92617-467">Um die XML-Dateikonfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="92617-467">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="92617-468">Überladungen geben Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="92617-468">Overloads permit specifying:</span></span>

* <span data-ttu-id="92617-469">Ob die Datei optional ist</span><span class="sxs-lookup"><span data-stu-id="92617-469">Whether the file is optional.</span></span>
* <span data-ttu-id="92617-470">Ob die Konfiguration bei Dateiänderungen neu geladen wird</span><span class="sxs-lookup"><span data-stu-id="92617-470">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="92617-471">Dass mit <xref:Microsoft.Extensions.FileProviders.IFileProvider> auf die Datei zugegriffen wird</span><span class="sxs-lookup"><span data-stu-id="92617-471">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="92617-472">Der Stammknoten der Konfigurationsdatei wird beim Erstellen der Konfigurations-Schlüssel-Wert-Paare ignoriert.</span><span class="sxs-lookup"><span data-stu-id="92617-472">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="92617-473">Geben Sie keine Dokumenttypdefinition (DTD) und keinen Namespace in der Datei an.</span><span class="sxs-lookup"><span data-stu-id="92617-473">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="92617-474">Rufen Sie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> beim Erstellen des Hosts auf, um die Konfiguration der App anzugeben:</span><span class="sxs-lookup"><span data-stu-id="92617-474">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="92617-475">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-475">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="92617-476">Rufen Sie beim Aufrufen von `CreateDefaultBuilder` <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-476">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="92617-477">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-477">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="92617-478">Wenden Sie die Konfiguration mit der <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>-Methode auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an:</span><span class="sxs-lookup"><span data-stu-id="92617-478">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="92617-479">XML-Konfigurationsdateien können unterschiedliche Elementnamen für wiederholte Abschnitte verwenden:</span><span class="sxs-lookup"><span data-stu-id="92617-479">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="92617-480">Die vorherige Konfigurationsdatei lädt die folgenden Schlüssel mit `value`:</span><span class="sxs-lookup"><span data-stu-id="92617-480">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="92617-481">section0:key0</span><span class="sxs-lookup"><span data-stu-id="92617-481">section0:key0</span></span>
* <span data-ttu-id="92617-482">section0:key1</span><span class="sxs-lookup"><span data-stu-id="92617-482">section0:key1</span></span>
* <span data-ttu-id="92617-483">section1:key0</span><span class="sxs-lookup"><span data-stu-id="92617-483">section1:key0</span></span>
* <span data-ttu-id="92617-484">section1:key1</span><span class="sxs-lookup"><span data-stu-id="92617-484">section1:key1</span></span>

<span data-ttu-id="92617-485">Wiederholte Elemente mit den gleichen Elementnamen funktionieren, wenn das `name`-Attribut zur Unterscheidung der Elemente verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="92617-485">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="92617-486">Die vorherige Konfigurationsdatei lädt die folgenden Schlüssel mit `value`:</span><span class="sxs-lookup"><span data-stu-id="92617-486">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="92617-487">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="92617-487">section:section0:key:key0</span></span>
* <span data-ttu-id="92617-488">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="92617-488">section:section0:key:key1</span></span>
* <span data-ttu-id="92617-489">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="92617-489">section:section1:key:key0</span></span>
* <span data-ttu-id="92617-490">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="92617-490">section:section1:key:key1</span></span>

<span data-ttu-id="92617-491">Mit Attributen können Werte bereitgestellt werden:</span><span class="sxs-lookup"><span data-stu-id="92617-491">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="92617-492">Die vorherige Konfigurationsdatei lädt die folgenden Schlüssel mit `value`:</span><span class="sxs-lookup"><span data-stu-id="92617-492">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="92617-493">key:attribute</span><span class="sxs-lookup"><span data-stu-id="92617-493">key:attribute</span></span>
* <span data-ttu-id="92617-494">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="92617-494">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="92617-495">Schlüssel-pro-Datei-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-495">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="92617-496"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> verwendet Verzeichnisdateien als Konfigurations-Schlüssel-Wert-Paare.</span><span class="sxs-lookup"><span data-stu-id="92617-496">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="92617-497">Der Schlüssel ist der Dateiname.</span><span class="sxs-lookup"><span data-stu-id="92617-497">The key is the file name.</span></span> <span data-ttu-id="92617-498">Der Wert enthält den Inhalt der Datei.</span><span class="sxs-lookup"><span data-stu-id="92617-498">The value contains the file's contents.</span></span> <span data-ttu-id="92617-499">Der Schlüssel-pro-Datei-Konfigurationsanbieter wird in Docker-Hostingszenarien verwendet.</span><span class="sxs-lookup"><span data-stu-id="92617-499">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="92617-500">Um die Schlüssel-pro-Datei-Konfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="92617-500">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="92617-501">Der `directoryPath` zu den Dateien muss ein absoluter Pfad sein.</span><span class="sxs-lookup"><span data-stu-id="92617-501">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="92617-502">Überladungen geben Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="92617-502">Overloads permit specifying:</span></span>

* <span data-ttu-id="92617-503">Einen `Action<KeyPerFileConfigurationSource>`-Delegat, der die Quelle konfiguriert</span><span class="sxs-lookup"><span data-stu-id="92617-503">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="92617-504">Ob das Verzeichnis optional ist; und den Pfad zum Verzeichnis</span><span class="sxs-lookup"><span data-stu-id="92617-504">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="92617-505">Rufen Sie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> beim Erstellen des Hosts auf, um die Konfiguration der App anzugeben:</span><span class="sxs-lookup"><span data-stu-id="92617-505">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="92617-506">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-506">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

::: moniker-end

## <a name="memory-configuration-provider"></a><span data-ttu-id="92617-507">Speicherkonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-507">Memory Configuration Provider</span></span>

<span data-ttu-id="92617-508"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> verwendet eine In-Memory-Sammlung für Konfigurations-Schlüssel-Wert-Paare.</span><span class="sxs-lookup"><span data-stu-id="92617-508">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="92617-509">Um die In-Memory-Sammlungskonfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="92617-509">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="92617-510">Der Konfigurationsanbieter kann mit `IEnumerable<KeyValuePair<String,String>>` initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="92617-510">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="92617-511">Rufen Sie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> beim Erstellen des Hosts auf, um die Konfiguration der App anzugeben:</span><span class="sxs-lookup"><span data-stu-id="92617-511">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="92617-512">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-512">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="92617-513">Rufen Sie beim Aufrufen von `CreateDefaultBuilder` <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-513">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

        var config = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="92617-514">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="92617-514">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="92617-515">Wenden Sie die Konfiguration mit der <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>-Methode auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an:</span><span class="sxs-lookup"><span data-stu-id="92617-515">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a><span data-ttu-id="92617-516">GetValue</span><span class="sxs-lookup"><span data-stu-id="92617-516">GetValue</span></span>

<span data-ttu-id="92617-517">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrahiert einen Wert aus der Konfiguration mit einem angegebenen Schlüssel, und konvertiert ihn in den angegebenen Typ.</span><span class="sxs-lookup"><span data-stu-id="92617-517">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="92617-518">Eine Überladung erlaubt es Ihnen, einen Standardwert anzugeben, wenn der Schlüssel nicht gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="92617-518">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="92617-519">Das folgende Beispiel extrahiert den Zeichenfolgenwert aus der Konfiguration mit dem Schlüssel `NumberKey`, gibt den Wert als `int` an und speichert den Wert in der Variablen `intValue`.</span><span class="sxs-lookup"><span data-stu-id="92617-519">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="92617-520">Wenn `NumberKey` nicht im Konfigurationsschlüssel gefunden wird, erhält `intValue` den Standardwert `99`:</span><span class="sxs-lookup"><span data-stu-id="92617-520">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="92617-521">GetSection, GetChildren und Exists</span><span class="sxs-lookup"><span data-stu-id="92617-521">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="92617-522">Betrachten Sie für die folgenden Beispiele die JSON-Datei unten.</span><span class="sxs-lookup"><span data-stu-id="92617-522">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="92617-523">Vier Schlüssel befinden sich in zwei Abschnitten, von denen einer ein Paar Unterabschnitte enthält:</span><span class="sxs-lookup"><span data-stu-id="92617-523">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="92617-524">Wenn die Datei in die Konfiguration gelesen wird, werden die folgenden eindeutigen hierarchischen Schlüssel erstellt, um die Konfigurationswerte zu enthalten:</span><span class="sxs-lookup"><span data-stu-id="92617-524">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="92617-525">section0:key0</span><span class="sxs-lookup"><span data-stu-id="92617-525">section0:key0</span></span>
* <span data-ttu-id="92617-526">section0:key1</span><span class="sxs-lookup"><span data-stu-id="92617-526">section0:key1</span></span>
* <span data-ttu-id="92617-527">section1:key0</span><span class="sxs-lookup"><span data-stu-id="92617-527">section1:key0</span></span>
* <span data-ttu-id="92617-528">section1:key1</span><span class="sxs-lookup"><span data-stu-id="92617-528">section1:key1</span></span>
* <span data-ttu-id="92617-529">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="92617-529">section2:subsection0:key0</span></span>
* <span data-ttu-id="92617-530">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="92617-530">section2:subsection0:key1</span></span>
* <span data-ttu-id="92617-531">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="92617-531">section2:subsection1:key0</span></span>
* <span data-ttu-id="92617-532">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="92617-532">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="92617-533">GetSection</span><span class="sxs-lookup"><span data-stu-id="92617-533">GetSection</span></span>

<span data-ttu-id="92617-534">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrahiert einen Konfigurationsunterabschnitt mit dem angegebenen Unterabschnittsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="92617-534">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="92617-535">Um ein <xref:Microsoft.Extensions.Configuration.IConfigurationSection>-Element zurückzugeben, das nur die Schlüssel-Wert-Paare in `section1` enthält, rufen Sie `GetSection` auf, und geben Sie den Abschnittsnamen an:</span><span class="sxs-lookup"><span data-stu-id="92617-535">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="92617-536">Um die Werte für Schlüssel in `section2:subsection0` zu erhalten, rufen Sie `GetSection` auf, und geben Sie den Abschnittspfad an:</span><span class="sxs-lookup"><span data-stu-id="92617-536">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="92617-537">`GetSection` gibt nie `null` zurück.</span><span class="sxs-lookup"><span data-stu-id="92617-537">`GetSection` never returns `null`.</span></span> <span data-ttu-id="92617-538">Wenn kein entsprechender Abschnitt gefunden wird, wird ein leeres `IConfigurationSection`-Element zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="92617-538">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="92617-539">GetChildren</span><span class="sxs-lookup"><span data-stu-id="92617-539">GetChildren</span></span>

<span data-ttu-id="92617-540">Ein Aufruf von [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) auf `section2` erhält `IEnumerable<IConfigurationSection>` mit folgenden Elementen:</span><span class="sxs-lookup"><span data-stu-id="92617-540">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="92617-541">Vorhanden</span><span class="sxs-lookup"><span data-stu-id="92617-541">Exists</span></span>

<span data-ttu-id="92617-542">Verwenden Sie [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) um zu bestimmen, ob ein Konfigurationsabschnitt vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="92617-542">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="92617-543">Im Falle der Beispieldaten ist `sectionExists` `false`, weil in den Konfigurationsdaten kein Abschnitt `section2:subsection2` vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="92617-543">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="92617-544">Binden an eine Klasse</span><span class="sxs-lookup"><span data-stu-id="92617-544">Bind to a class</span></span>

<span data-ttu-id="92617-545">Die Konfiguration kann mit *Optionsmustern* an Klassen gebunden werden, die Gruppen von verwandten Einstellungen darstellen.</span><span class="sxs-lookup"><span data-stu-id="92617-545">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="92617-546">Weitere Informationen finden Sie unter <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="92617-546">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="92617-547">Konfigurationswerte werden als Zeichenfolgen zurückgegeben, doch das Aufrufen von <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> ermöglicht die Erstellung von [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)-Objekten.</span><span class="sxs-lookup"><span data-stu-id="92617-547">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="92617-548">Die Beispiel-App enthält ein `Starship`-Modell (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="92617-548">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="92617-549">Der Abschnitt `starship` der Datei *starship.json* erstellt die Konfiguration, wenn die Beispiel-App den JSON-Konfigurationsanbieter zum Laden der Konfiguration verwendet:</span><span class="sxs-lookup"><span data-stu-id="92617-549">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="92617-550">Die folgenden Schlüssel-Wert-Paare werden für die Konfiguration erstellt:</span><span class="sxs-lookup"><span data-stu-id="92617-550">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="92617-551">Key</span><span class="sxs-lookup"><span data-stu-id="92617-551">Key</span></span>                   | <span data-ttu-id="92617-552">Wert</span><span class="sxs-lookup"><span data-stu-id="92617-552">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="92617-553">starship:name</span><span class="sxs-lookup"><span data-stu-id="92617-553">starship:name</span></span>         | <span data-ttu-id="92617-554">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="92617-554">USS Enterprise</span></span>                                    |
| <span data-ttu-id="92617-555">starship:registry</span><span class="sxs-lookup"><span data-stu-id="92617-555">starship:registry</span></span>     | <span data-ttu-id="92617-556">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="92617-556">NCC-1701</span></span>                                          |
| <span data-ttu-id="92617-557">starship:class</span><span class="sxs-lookup"><span data-stu-id="92617-557">starship:class</span></span>        | <span data-ttu-id="92617-558">Constitution</span><span class="sxs-lookup"><span data-stu-id="92617-558">Constitution</span></span>                                      |
| <span data-ttu-id="92617-559">starship:length</span><span class="sxs-lookup"><span data-stu-id="92617-559">starship:length</span></span>       | <span data-ttu-id="92617-560">304,8</span><span class="sxs-lookup"><span data-stu-id="92617-560">304.8</span></span>                                             |
| <span data-ttu-id="92617-561">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="92617-561">starship:commissioned</span></span> | <span data-ttu-id="92617-562">False</span><span class="sxs-lookup"><span data-stu-id="92617-562">False</span></span>                                             |
| <span data-ttu-id="92617-563">trademark</span><span class="sxs-lookup"><span data-stu-id="92617-563">trademark</span></span>             | <span data-ttu-id="92617-564">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="92617-564">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="92617-565">Die Beispiel-App ruft `GetSection` mit dem `starship`-Schlüssel auf.</span><span class="sxs-lookup"><span data-stu-id="92617-565">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="92617-566">Die `starship`-Schlüssel-Wert-Paaren sind isoliert.</span><span class="sxs-lookup"><span data-stu-id="92617-566">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="92617-567">Die `Bind`-Methode wird auf dem Unterabschnitt aufgerufen, der in einer Instanz der `Starship`-Klasse übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="92617-567">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="92617-568">Nach dem Binden der Instanzwerte wird die Instanz einer Eigenschaft zum Rendern zugewiesen:</span><span class="sxs-lookup"><span data-stu-id="92617-568">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="92617-569">Binden an ein Objektdiagramm</span><span class="sxs-lookup"><span data-stu-id="92617-569">Bind to an object graph</span></span>

<span data-ttu-id="92617-570"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> kann ein ganzes POCO-Objektdiagramm binden.</span><span class="sxs-lookup"><span data-stu-id="92617-570"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="92617-571">Das Beispiel enthält ein `TvShow`-Modell, dessen Objektdiagramm die Klassen `Metadata` und `Actors` enthält (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="92617-571">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="92617-572">Die Beispiel-App verfügt über eine *tvshow.xml*-Datei mit den Konfigurationsdaten:</span><span class="sxs-lookup"><span data-stu-id="92617-572">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="92617-573">Die Konfiguration wird mit der `Bind`-Methode an das gesamte `TvShow`-Objektdiagramm gebunden.</span><span class="sxs-lookup"><span data-stu-id="92617-573">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="92617-574">Die gebundene Instanz ist einer Eigenschaft zum Rendern zugewiesen:</span><span class="sxs-lookup"><span data-stu-id="92617-574">The bound instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="92617-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bindet den angegebenen Typ und gibt ihn zurück.</span><span class="sxs-lookup"><span data-stu-id="92617-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="92617-576">`Get<T>` ist praktischer als `Bind`.</span><span class="sxs-lookup"><span data-stu-id="92617-576">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="92617-577">Der folgende Code zeigt die Verwendung von `Get<T>` mit dem vorherigen Beispiel, sodass die gebundene Instanz direkt der Eigenschaft zum Rendern zugewiesen werden kann:</span><span class="sxs-lookup"><span data-stu-id="92617-577">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="92617-578">Binden eines Arrays an eine Klasse</span><span class="sxs-lookup"><span data-stu-id="92617-578">Bind an array to a class</span></span>

<span data-ttu-id="92617-579">*Die Beispiel-App veranschaulicht die in diesem Abschnitt erläuterten Konzepte.*</span><span class="sxs-lookup"><span data-stu-id="92617-579">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="92617-580"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> unterstützt das Binden von Arrays an Objekte mit Arrayindizes in Konfigurationsschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="92617-580">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="92617-581">Jedes Arrayformat, das ein numerisches Schlüsselsegment (`:0:`, `:1:`, &hellip; `:{n}:`) verfügbar macht, kann ein Array an ein POCO-Klassenarray binden.</span><span class="sxs-lookup"><span data-stu-id="92617-581">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="92617-582">Das Binden ist standardmäßig möglich.</span><span class="sxs-lookup"><span data-stu-id="92617-582">Binding is provided by convention.</span></span> <span data-ttu-id="92617-583">Benutzerdefinierte Konfigurationsanbieter sind nicht erforderlich, um Arraybindung zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="92617-583">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="92617-584">**In-Memory-Arrayverarbeitung**</span><span class="sxs-lookup"><span data-stu-id="92617-584">**In-memory array processing**</span></span>

<span data-ttu-id="92617-585">Betrachten Sie die Konfigurationsschlüssel und -werte in der folgenden Tabelle.</span><span class="sxs-lookup"><span data-stu-id="92617-585">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="92617-586">Key</span><span class="sxs-lookup"><span data-stu-id="92617-586">Key</span></span>             | <span data-ttu-id="92617-587">Wert</span><span class="sxs-lookup"><span data-stu-id="92617-587">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="92617-588">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="92617-588">array:entries:0</span></span> | <span data-ttu-id="92617-589">value0</span><span class="sxs-lookup"><span data-stu-id="92617-589">value0</span></span> |
| <span data-ttu-id="92617-590">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="92617-590">array:entries:1</span></span> | <span data-ttu-id="92617-591">value1</span><span class="sxs-lookup"><span data-stu-id="92617-591">value1</span></span> |
| <span data-ttu-id="92617-592">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="92617-592">array:entries:2</span></span> | <span data-ttu-id="92617-593">value2</span><span class="sxs-lookup"><span data-stu-id="92617-593">value2</span></span> |
| <span data-ttu-id="92617-594">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="92617-594">array:entries:4</span></span> | <span data-ttu-id="92617-595">value4</span><span class="sxs-lookup"><span data-stu-id="92617-595">value4</span></span> |
| <span data-ttu-id="92617-596">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="92617-596">array:entries:5</span></span> | <span data-ttu-id="92617-597">value5</span><span class="sxs-lookup"><span data-stu-id="92617-597">value5</span></span> |

<span data-ttu-id="92617-598">Diese Schlüssel und Werte werden mit dem Speicherkonfigurationsanbieter in die Beispiel-App geladen:</span><span class="sxs-lookup"><span data-stu-id="92617-598">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="92617-599">Das Array überspringt einen Wert für Index &num;3.</span><span class="sxs-lookup"><span data-stu-id="92617-599">The array skips a value for index &num;3.</span></span> <span data-ttu-id="92617-600">Der Konfigurationsbinder kann weder NULL-Werte binden noch NULL-Einträge in gebundenen Objekten erstellen. Dies wird deutlich, wenn das Ergebnis der Bindung dieses Arrays an ein Objekt demonstriert wird.</span><span class="sxs-lookup"><span data-stu-id="92617-600">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="92617-601">In der Beispiel-App ist eine POCO-Klasse verfügbar, um die gebundenen Konfigurationsdaten zu enthalten:</span><span class="sxs-lookup"><span data-stu-id="92617-601">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="92617-602">Die Konfigurationsdaten werden an das Objekt gebunden:</span><span class="sxs-lookup"><span data-stu-id="92617-602">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="92617-603">Die [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)-Syntax kann auch verwendet werden, wodurch ein kompakterer Code entsteht:</span><span class="sxs-lookup"><span data-stu-id="92617-603">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="92617-604">Das gebundene Objekt, eine Instanz von `ArrayExample`, empfängt die Arraydaten aus der Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="92617-604">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="92617-605">`ArrayExample.Entries`-Index</span><span class="sxs-lookup"><span data-stu-id="92617-605">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="92617-606">`ArrayExample.Entries`-Wert</span><span class="sxs-lookup"><span data-stu-id="92617-606">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="92617-607">0</span><span class="sxs-lookup"><span data-stu-id="92617-607">0</span></span>                            | <span data-ttu-id="92617-608">value0</span><span class="sxs-lookup"><span data-stu-id="92617-608">value0</span></span>                       |
| <span data-ttu-id="92617-609">1</span><span class="sxs-lookup"><span data-stu-id="92617-609">1</span></span>                            | <span data-ttu-id="92617-610">value1</span><span class="sxs-lookup"><span data-stu-id="92617-610">value1</span></span>                       |
| <span data-ttu-id="92617-611">2</span><span class="sxs-lookup"><span data-stu-id="92617-611">2</span></span>                            | <span data-ttu-id="92617-612">value2</span><span class="sxs-lookup"><span data-stu-id="92617-612">value2</span></span>                       |
| <span data-ttu-id="92617-613">3</span><span class="sxs-lookup"><span data-stu-id="92617-613">3</span></span>                            | <span data-ttu-id="92617-614">value4</span><span class="sxs-lookup"><span data-stu-id="92617-614">value4</span></span>                       |
| <span data-ttu-id="92617-615">4</span><span class="sxs-lookup"><span data-stu-id="92617-615">4</span></span>                            | <span data-ttu-id="92617-616">value5</span><span class="sxs-lookup"><span data-stu-id="92617-616">value5</span></span>                       |

<span data-ttu-id="92617-617">Index &num;3 im gebundenen Objekt enthält die Konfigurationsdaten für den `array:4`-Konfigurationsschlüssel und die Wert für `value4`.</span><span class="sxs-lookup"><span data-stu-id="92617-617">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="92617-618">Beim Binden von Konfigurationsdaten, die ein Array enthalten, werden die Arrayindizes in den Konfigurationsschlüsseln lediglich zum Durchlaufen der Konfigurationsdaten beim Erstellen des Objekts verwendet.</span><span class="sxs-lookup"><span data-stu-id="92617-618">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="92617-619">Ein NULL-Wert kann in den Konfigurationsdaten nicht beibehalten werden, und ein NULL-Eintrag wird nicht in einem gebundenen Objekt erstellt, wenn ein Array in Konfigurationsschlüsseln mindestens einen Index überspringt.</span><span class="sxs-lookup"><span data-stu-id="92617-619">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="92617-620">Das fehlende Konfigurationselement für Index &num;3 kann vor dem Binden an die `ArrayExample`Instanz von jedem Konfigurationsanbieter bereitgestellt werden, der das richtige Schlüssel-Wert-Paar in der Konfiguration erzeugt.</span><span class="sxs-lookup"><span data-stu-id="92617-620">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="92617-621">Wenn das Beispiel einen zusätzlichen JSON-Konfigurationsanbieter mit dem fehlenden Schlüssel-Wert-Paar enthält, entspricht `ArrayExample.Entries` dem kompletten Konfigurationsarray:</span><span class="sxs-lookup"><span data-stu-id="92617-621">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="92617-622">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="92617-622">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="92617-623">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="92617-623">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="92617-624">Im `Startup`-Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="92617-624">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="92617-625">Das Schlüssel-Wert-Paar in der Tabelle wird in die Konfiguration geladen.</span><span class="sxs-lookup"><span data-stu-id="92617-625">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="92617-626">Key</span><span class="sxs-lookup"><span data-stu-id="92617-626">Key</span></span>             | <span data-ttu-id="92617-627">Wert</span><span class="sxs-lookup"><span data-stu-id="92617-627">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="92617-628">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="92617-628">array:entries:3</span></span> | <span data-ttu-id="92617-629">value3</span><span class="sxs-lookup"><span data-stu-id="92617-629">value3</span></span> |

<span data-ttu-id="92617-630">Wenn die `ArrayExample`-Klasseninstanz gebunden wird, nachdem der JSON-Konfigurationsanbieter den Eintrag für Index &num;3 enthält, enthält das `ArrayExample.Entries`-Array den Wert.</span><span class="sxs-lookup"><span data-stu-id="92617-630">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="92617-631">`ArrayExample.Entries`-Index</span><span class="sxs-lookup"><span data-stu-id="92617-631">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="92617-632">`ArrayExample.Entries`-Wert</span><span class="sxs-lookup"><span data-stu-id="92617-632">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="92617-633">0</span><span class="sxs-lookup"><span data-stu-id="92617-633">0</span></span>                            | <span data-ttu-id="92617-634">value0</span><span class="sxs-lookup"><span data-stu-id="92617-634">value0</span></span>                       |
| <span data-ttu-id="92617-635">1</span><span class="sxs-lookup"><span data-stu-id="92617-635">1</span></span>                            | <span data-ttu-id="92617-636">value1</span><span class="sxs-lookup"><span data-stu-id="92617-636">value1</span></span>                       |
| <span data-ttu-id="92617-637">2</span><span class="sxs-lookup"><span data-stu-id="92617-637">2</span></span>                            | <span data-ttu-id="92617-638">value2</span><span class="sxs-lookup"><span data-stu-id="92617-638">value2</span></span>                       |
| <span data-ttu-id="92617-639">3</span><span class="sxs-lookup"><span data-stu-id="92617-639">3</span></span>                            | <span data-ttu-id="92617-640">value3</span><span class="sxs-lookup"><span data-stu-id="92617-640">value3</span></span>                       |
| <span data-ttu-id="92617-641">4</span><span class="sxs-lookup"><span data-stu-id="92617-641">4</span></span>                            | <span data-ttu-id="92617-642">value4</span><span class="sxs-lookup"><span data-stu-id="92617-642">value4</span></span>                       |
| <span data-ttu-id="92617-643">5</span><span class="sxs-lookup"><span data-stu-id="92617-643">5</span></span>                            | <span data-ttu-id="92617-644">value5</span><span class="sxs-lookup"><span data-stu-id="92617-644">value5</span></span>                       |

<span data-ttu-id="92617-645">**JSON-Arrayverarbeitung**</span><span class="sxs-lookup"><span data-stu-id="92617-645">**JSON array processing**</span></span>

<span data-ttu-id="92617-646">Wenn eine JSON-Datei ein Array enthält, werden Konfigurationsschlüssel für die Arrayelemente mit einem nullbasierten Abschnittsindex erstellt.</span><span class="sxs-lookup"><span data-stu-id="92617-646">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="92617-647">In der folgenden Konfigurationsdatei ist `subsection` ein Array:</span><span class="sxs-lookup"><span data-stu-id="92617-647">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="92617-648">Der JSON-Konfigurationsanbieter liest die Konfigurationsdaten in die folgenden Schlüssel-Wert-Paare:</span><span class="sxs-lookup"><span data-stu-id="92617-648">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="92617-649">Key</span><span class="sxs-lookup"><span data-stu-id="92617-649">Key</span></span>                     | <span data-ttu-id="92617-650">Wert</span><span class="sxs-lookup"><span data-stu-id="92617-650">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="92617-651">json_array:key</span><span class="sxs-lookup"><span data-stu-id="92617-651">json_array:key</span></span>          | <span data-ttu-id="92617-652">valueA</span><span class="sxs-lookup"><span data-stu-id="92617-652">valueA</span></span> |
| <span data-ttu-id="92617-653">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="92617-653">json_array:subsection:0</span></span> | <span data-ttu-id="92617-654">valueB</span><span class="sxs-lookup"><span data-stu-id="92617-654">valueB</span></span> |
| <span data-ttu-id="92617-655">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="92617-655">json_array:subsection:1</span></span> | <span data-ttu-id="92617-656">valueC</span><span class="sxs-lookup"><span data-stu-id="92617-656">valueC</span></span> |
| <span data-ttu-id="92617-657">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="92617-657">json_array:subsection:2</span></span> | <span data-ttu-id="92617-658">valueD</span><span class="sxs-lookup"><span data-stu-id="92617-658">valueD</span></span> |

<span data-ttu-id="92617-659">In der Beispiel-App können die Konfigurations-Schlüssel-Wert-Paare mit dieser POCO-Klasse gebunden werden:</span><span class="sxs-lookup"><span data-stu-id="92617-659">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="92617-660">Nach dem Binden enthält `JsonArrayExample.Key` den Wert `valueA`.</span><span class="sxs-lookup"><span data-stu-id="92617-660">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="92617-661">Die Unterabschnittwerte werden in der POCO-Arrayeigenschaft `Subsection` gespeichert.</span><span class="sxs-lookup"><span data-stu-id="92617-661">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="92617-662">`JsonArrayExample.Subsection`-Index</span><span class="sxs-lookup"><span data-stu-id="92617-662">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="92617-663">`JsonArrayExample.Subsection`-Wert</span><span class="sxs-lookup"><span data-stu-id="92617-663">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="92617-664">0</span><span class="sxs-lookup"><span data-stu-id="92617-664">0</span></span>                                   | <span data-ttu-id="92617-665">valueB</span><span class="sxs-lookup"><span data-stu-id="92617-665">valueB</span></span>                              |
| <span data-ttu-id="92617-666">1</span><span class="sxs-lookup"><span data-stu-id="92617-666">1</span></span>                                   | <span data-ttu-id="92617-667">valueC</span><span class="sxs-lookup"><span data-stu-id="92617-667">valueC</span></span>                              |
| <span data-ttu-id="92617-668">2</span><span class="sxs-lookup"><span data-stu-id="92617-668">2</span></span>                                   | <span data-ttu-id="92617-669">valueD</span><span class="sxs-lookup"><span data-stu-id="92617-669">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="92617-670">Benutzerdefinierter Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="92617-670">Custom configuration provider</span></span>

<span data-ttu-id="92617-671">Die Beispiel-App veranschaulicht, wie ein Standardkonfigurationsanbieter erstellt wird, der Konfigurations-Schlüssel-Wert-Paare aus einer Datenbank mit [Entity Framework (EF)](/ef/core/) liest.</span><span class="sxs-lookup"><span data-stu-id="92617-671">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="92617-672">Der Anbieter weist die folgenden Merkmale auf:</span><span class="sxs-lookup"><span data-stu-id="92617-672">The provider has the following characteristics:</span></span>

* <span data-ttu-id="92617-673">Die EF-In-Memory-Datenbank wird zu Demonstrationszwecken verwendet.</span><span class="sxs-lookup"><span data-stu-id="92617-673">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="92617-674">Um eine Datenbank zu verwenden, die eine Verbindungszeichenfolge benötigt, implementieren Sie einen sekundären `ConfigurationBuilder`, um die Verbindungszeichenfolge aus einem anderen Konfigurationsanbieter anzugeben.</span><span class="sxs-lookup"><span data-stu-id="92617-674">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="92617-675">Der Anbieter liest eine Datenbanktabelle beim Start in die Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="92617-675">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="92617-676">Der Anbieter fragt die Datenbank nicht pro Schlüssel ab.</span><span class="sxs-lookup"><span data-stu-id="92617-676">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="92617-677">Das erneute Laden bei Änderung ist nicht implementiert. Das heißt, das Aktualisieren der Datenbank nach App-Start hat keine Auswirkungen auf die App-Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="92617-677">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="92617-678">Definieren Sie eine `EFConfigurationValue`-Entität zum Speichern von Konfigurationswerten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="92617-678">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="92617-679">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="92617-679">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="92617-680">Fügen Sie `EFConfigurationContext` hinzu, um die konfigurierten Werte zu speichern und auf diese zugreifen.</span><span class="sxs-lookup"><span data-stu-id="92617-680">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="92617-681">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="92617-681">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="92617-682">Erstellen Sie eine Klasse, die das <xref:Microsoft.Extensions.Configuration.IConfigurationSource> implementiert.</span><span class="sxs-lookup"><span data-stu-id="92617-682">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="92617-683">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="92617-683">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="92617-684">Erstellen Sie den benutzerdefinierten Konfigurationsanbieter durch Vererbung von <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="92617-684">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="92617-685">Der Konfigurationsanbieter initialisiert die Datenbank, wenn diese leer ist.</span><span class="sxs-lookup"><span data-stu-id="92617-685">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="92617-686">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="92617-686">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="92617-687">Mit einer `AddEFConfiguration`-Erweiterungsmethode kann die Konfigurationsquelle `ConfigurationBuilder` hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="92617-687">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="92617-688">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="92617-688">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="92617-689">Der folgende Code veranschaulicht die Verwendung des benutzerdefinierten Anbieters `EFConfigurationProvider` in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="92617-689">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="92617-690">Zugreifen auf die Konfiguration während des Starts</span><span class="sxs-lookup"><span data-stu-id="92617-690">Access configuration during startup</span></span>

<span data-ttu-id="92617-691">Fügen Sie `IConfiguration` in den `Startup`-Konstruktor ein, um auf Konfigurationswerte in `Startup.ConfigureServices` zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="92617-691">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="92617-692">Um auf die Konfiguration in `Startup.Configure` zuzugreifen, fügen Sie entweder `IConfiguration` direkt in die Methode ein, oder verwenden Sie die Instanz aus dem Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="92617-692">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="92617-693">Ein Beispiel für den Zugriff auf die Konfiguration mit den Starthilfsmethoden finden Sie unter [Anwendungsstart: Hilfsmethoden](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="92617-693">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="92617-694">Zugreifen auf die Konfiguration auf einer Razor Pages-Seite oder in einer MVC-Ansicht</span><span class="sxs-lookup"><span data-stu-id="92617-694">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="92617-695">Um auf die Konfigurationseinstellungen auf einer Razor Pages-Seite oder in einer MVC-Ansicht zuzugreifen, fügen Sie eine [using-Anweisung](xref:mvc/views/razor#using) ([C#-Referenz: using-Anweisung](/dotnet/csharp/language-reference/keywords/using-directive)) für den [Microsoft.Extensions.Configuration-Namespace ](xref:Microsoft.Extensions.Configuration) hinzu, und fügen Sie <xref:Microsoft.Extensions.Configuration.IConfiguration> auf der Seite bzw. in der Ansicht ein.</span><span class="sxs-lookup"><span data-stu-id="92617-695">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="92617-696">Auf einer Razor Pages-Seite:</span><span class="sxs-lookup"><span data-stu-id="92617-696">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="92617-697">In einer MVC-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="92617-697">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="92617-698">Hinzufügen von Konfigurationen aus einer externen Assembly</span><span class="sxs-lookup"><span data-stu-id="92617-698">Add configuration from an external assembly</span></span>

<span data-ttu-id="92617-699">Eine <xref:Microsoft.AspNetCore.Hosting.IHostingStartup>-Implementierung ermöglicht das Hinzufügen von Erweiterungen zu einer App beim Start von einer externen Assembly außerhalb der `Startup`-Klasse der App aus.</span><span class="sxs-lookup"><span data-stu-id="92617-699">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="92617-700">Weitere Informationen finden Sie unter <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="92617-700">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="92617-701">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="92617-701">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="92617-702">[Fundierte Einblicke in die Microsoft-Konfiguration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (in englischer Sprache)</span><span class="sxs-lookup"><span data-stu-id="92617-702">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
