---
title: Konfiguration in ASP.NET Core
author: guardrex
description: In diesem Artikel erfahren Sie, wie Sie mit der Konfigurations-API eine ASP.NET Core-App konfigurieren.
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: a0c57e75b28bc7c5590d20a8fa59b00b6bb9af4e
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927877"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="73bc5-103">Konfiguration in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73bc5-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="73bc5-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="73bc5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="73bc5-105">Die App-Konfiguration in ASP.NET Core basiert auf Schlüssel-Wert-Paaren, die von *Konfigurationsanbietern* erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="73bc5-106">Konfigurationsanbieter lesen Konfigurationsdaten in Schlüssel-Wert-Paare aus verschiedenen Konfigurationsquellen:</span><span class="sxs-lookup"><span data-stu-id="73bc5-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="73bc5-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="73bc5-107">Azure Key Vault</span></span>
* <span data-ttu-id="73bc5-108">Befehlszeilenargumente</span><span class="sxs-lookup"><span data-stu-id="73bc5-108">Command-line arguments</span></span>
* <span data-ttu-id="73bc5-109">Benutzerdefinierte Anbieter (installiert oder erstellt)</span><span class="sxs-lookup"><span data-stu-id="73bc5-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="73bc5-110">Verzeichnisdateien</span><span class="sxs-lookup"><span data-stu-id="73bc5-110">Directory files</span></span>
* <span data-ttu-id="73bc5-111">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="73bc5-111">Environment variables</span></span>
* <span data-ttu-id="73bc5-112">Speicherinterne .NET Objekte</span><span class="sxs-lookup"><span data-stu-id="73bc5-112">In-memory .NET objects</span></span>
* <span data-ttu-id="73bc5-113">Einstellungsdateien</span><span class="sxs-lookup"><span data-stu-id="73bc5-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="73bc5-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="73bc5-114">Azure Key Vault</span></span>
* <span data-ttu-id="73bc5-115">Befehlszeilenargumente</span><span class="sxs-lookup"><span data-stu-id="73bc5-115">Command-line arguments</span></span>
* <span data-ttu-id="73bc5-116">Benutzerdefinierte Anbieter (installiert oder erstellt)</span><span class="sxs-lookup"><span data-stu-id="73bc5-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="73bc5-117">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="73bc5-117">Environment variables</span></span>
* <span data-ttu-id="73bc5-118">Speicherinterne .NET Objekte</span><span class="sxs-lookup"><span data-stu-id="73bc5-118">In-memory .NET objects</span></span>
* <span data-ttu-id="73bc5-119">Einstellungsdateien</span><span class="sxs-lookup"><span data-stu-id="73bc5-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="73bc5-120">Befehlszeilenargumente</span><span class="sxs-lookup"><span data-stu-id="73bc5-120">Command-line arguments</span></span>
* <span data-ttu-id="73bc5-121">Benutzerdefinierte Anbieter (installiert oder erstellt)</span><span class="sxs-lookup"><span data-stu-id="73bc5-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="73bc5-122">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="73bc5-122">Environment variables</span></span>
* <span data-ttu-id="73bc5-123">Speicherinterne .NET Objekte</span><span class="sxs-lookup"><span data-stu-id="73bc5-123">In-memory .NET objects</span></span>
* <span data-ttu-id="73bc5-124">Einstellungsdateien</span><span class="sxs-lookup"><span data-stu-id="73bc5-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="73bc5-125">Das *Optionsmuster* ist eine Erweiterung der in diesem Thema beschriebenen Konfigurationskonzepte.</span><span class="sxs-lookup"><span data-stu-id="73bc5-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="73bc5-126">Optionsmuster verwenden Klassen, um Gruppen von zusammengehörigen Einstellungen darzustellen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="73bc5-127">Weitere Informationen zum Verwenden von Optionsmustern finden Sie unter <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="73bc5-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="73bc5-128">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="73bc5-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="73bc5-129">Für die in diesem Artikel genannten Beispiele müssen folgende Bedingungen erfüllt sein:</span><span class="sxs-lookup"><span data-stu-id="73bc5-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="73bc5-130">Der Basispfad der App wurde mit <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> festgelegt.</span><span class="sxs-lookup"><span data-stu-id="73bc5-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="73bc5-131">Eine App kann durch den Verweis auf das Paket [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) auf `SetBasePath` zugreifen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="73bc5-132">Abschnitte von Konfigurationsdateien werden mit <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="73bc5-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="73bc5-133">Eine App kann durch den Verweis auf das Paket [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) auf `GetSection` zugreifen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="73bc5-134">Die Konfiguration wird mit <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> und [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) an .NET-Klassen gebunden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="73bc5-135">Eine App kann durch den Verweis auf das Paket [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) auf `Bind` und `Get<T>` zugreifen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="73bc5-136">`Get<T>` ist in ASP.NET Core 1.1 und höher verfügbar.</span><span class="sxs-lookup"><span data-stu-id="73bc5-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="73bc5-137">Diese drei Pakete sind im Metapaket [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) enthalten.</span><span class="sxs-lookup"><span data-stu-id="73bc5-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="73bc5-138">Diese drei Pakete sind im Metapaket [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) enthalten.</span><span class="sxs-lookup"><span data-stu-id="73bc5-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="73bc5-139">Hostkonfiguration vs. App-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="73bc5-139">Host vs. app configuration</span></span>

<span data-ttu-id="73bc5-140">Bevor die App konfiguriert und gestartet wird, wird ein *Host* konfiguriert und gestartet.</span><span class="sxs-lookup"><span data-stu-id="73bc5-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="73bc5-141">Der Host ist verantwortlich für das Starten der App und das Verwalten der Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="73bc5-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="73bc5-142">Die App und der Host werden mit den in diesem Thema beschriebenen Konfigurationsanbietern konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="73bc5-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="73bc5-143">Schlüssel-Wert-Paare der Hostkonfiguration werden Teil der globalen App-Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="73bc5-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="73bc5-144">Weitere Informationen dazu, wie Konfigurationsanbieter beim Erstellen des Hosts verwendet werden, und wie sich Konfigurationsquellen auf die Hostkonfiguration auswirken, finden Sie unter <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="73bc5-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="73bc5-145">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="73bc5-145">Security</span></span>

<span data-ttu-id="73bc5-146">Verwenden Sie die folgenden Best Practices:</span><span class="sxs-lookup"><span data-stu-id="73bc5-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="73bc5-147">Speichern Sie nie Kennwörter oder andere vertrauliche Daten im Konfigurationsanbietercode oder in Nur-Text-Konfigurationsdateien.</span><span class="sxs-lookup"><span data-stu-id="73bc5-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="73bc5-148">Verwenden Sie keine Produktionsgeheimnisse in Entwicklungs- oder Testumgebungen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="73bc5-149">Geben Sie Geheimnisse außerhalb des Projekts an, damit sie nicht versehentlich in ein Quellcoderepository übernommen werden können.</span><span class="sxs-lookup"><span data-stu-id="73bc5-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="73bc5-150">Lesen Sie mehr über das [Verwenden von mehreren Umgebungen](xref:fundamentals/environments) und das Verwalten der [sicheren Speicherung von App-Geheimnissen bei der Entwicklung mit dem Geheimnis-Manager](xref:security/app-secrets) (mit Informationen zum Speichern vertraulicher Daten mit Umgebungsvariablen).</span><span class="sxs-lookup"><span data-stu-id="73bc5-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="73bc5-151">Der Geheimnis-Manager verwendet den Dateikonfigurationsanbieter, um Benutzergeheimnisse in einer JSON-Datei im lokalen System zu speichern.</span><span class="sxs-lookup"><span data-stu-id="73bc5-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="73bc5-152">Der Dateikonfigurationsanbieter wird später in diesem Thema beschrieben.</span><span class="sxs-lookup"><span data-stu-id="73bc5-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="73bc5-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stellt eine Option für die sichere Speicherung von App-Geheimnissen dar.</span><span class="sxs-lookup"><span data-stu-id="73bc5-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="73bc5-154">Weitere Informationen finden Sie unter <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="73bc5-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="73bc5-155">Hierarchische Konfigurationsdaten</span><span class="sxs-lookup"><span data-stu-id="73bc5-155">Hierarchical configuration data</span></span>

<span data-ttu-id="73bc5-156">Die Konfigurations-API kann hierarchische Konfigurationsdaten erhalten, indem sie die hierarchischen Daten mit einem Trennzeichen in den Konfigurationsschlüsseln vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="73bc5-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="73bc5-157">Die folgende JSON-Datei enthält vier Schlüssel in einer strukturierten Hierarchie von zwei Abschnitten:</span><span class="sxs-lookup"><span data-stu-id="73bc5-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="73bc5-158">Wenn die Datei in die Konfiguration gelesen wird, werden eindeutige Schlüssel erstellt, um die ursprüngliche hierarchische Datenstruktur der Konfigurationsquelle zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="73bc5-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="73bc5-159">Die Abschnitte und Schlüssel werden mithilfe eines Doppelpunkts (`:`) vereinfacht, um die Originalstruktur zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="73bc5-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="73bc5-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="73bc5-160">section0:key0</span></span>
* <span data-ttu-id="73bc5-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="73bc5-161">section0:key1</span></span>
* <span data-ttu-id="73bc5-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="73bc5-162">section1:key0</span></span>
* <span data-ttu-id="73bc5-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="73bc5-163">section1:key1</span></span>

<span data-ttu-id="73bc5-164">Mit den Methoden <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> und <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> können Abschnitte und untergeordnete Abschnittelemente in den Konfigurationsdaten isoliert werden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="73bc5-165">Diese Methoden werden später im Abschnitt [GetSection, GetChildren und Exists](#getsection-getchildren-and-exists) beschrieben.</span><span class="sxs-lookup"><span data-stu-id="73bc5-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="73bc5-166">Konventionen</span><span class="sxs-lookup"><span data-stu-id="73bc5-166">Conventions</span></span>

<span data-ttu-id="73bc5-167">Beim Starten der Anwendung werden Konfigurationsquellen in der Reihenfolge gelesen, in der ihre Konfigurationsanbieter angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="73bc5-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="73bc5-168">Dateikonfigurationsanbieter können Konfigurationen erneut laden, wenn eine zugrunde liegende Einstellungsdatei nach dem App-Start geändert wird.</span><span class="sxs-lookup"><span data-stu-id="73bc5-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="73bc5-169">Der Dateikonfigurationsanbieter wird später in diesem Thema beschrieben.</span><span class="sxs-lookup"><span data-stu-id="73bc5-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="73bc5-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> steht im App-Container [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="73bc5-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="73bc5-171">Konfigurationsanbieter können die Abhängigkeitsinjektion nicht verwenden, weil sie nicht verfügbar ist, wenn sie vom Host eingerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="73bc5-172">Konfigurationsschlüssel entsprechen den folgenden Konventionen:</span><span class="sxs-lookup"><span data-stu-id="73bc5-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="73bc5-173">Bei Schlüsseln wird die Groß-/Kleinschreibung nicht beachtet.</span><span class="sxs-lookup"><span data-stu-id="73bc5-173">Keys are case-insensitive.</span></span> <span data-ttu-id="73bc5-174">Beispielsweise verweisen `ConnectionString` und `connectionstring` auf denselben Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="73bc5-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="73bc5-175">Wenn ein Wert für denselben Schlüssel von denselben oder unterschiedlichen Konfigurationsanbietern festgelegt wird, wird der zuletzt für den Schlüssel bestimmte Wert verwendet.</span><span class="sxs-lookup"><span data-stu-id="73bc5-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="73bc5-176">Hierarchische Schlüssel</span><span class="sxs-lookup"><span data-stu-id="73bc5-176">Hierarchical keys</span></span>
  * <span data-ttu-id="73bc5-177">Innerhalb der Konfigurations-API funktioniert ein Doppelpunkt (`:`) als Trennzeichen auf allen Plattformen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="73bc5-178">In Umgebungsvariablen funktioniert ein Doppelpunkt als Trennzeichen ggf. nicht auf allen Plattformen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="73bc5-179">Ein doppelter Unterstrich (`__`) wird auf allen Plattformen unterstützt und in einen Doppelpunkt konvertiert.</span><span class="sxs-lookup"><span data-stu-id="73bc5-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="73bc5-180">In Azure Key Vault verwenden hierarchische Schlüssel zwei Bindestriche (`--`) als Trennzeichen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="73bc5-181">Sie müssen einen Code bereitstellen, um die Bindestriche durch einen Doppelpunkt zu ersetzen, wenn die Geheimnisse in die App-Konfiguration geladen werden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="73bc5-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> unterstützt das Binden von Arrays an Objekte mit Arrayindizes in Konfigurationsschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="73bc5-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="73bc5-183">Die Arraybindung wird im Abschnitt [Binden eines Arrays an eine Klasse](#bind-an-array-to-a-class) beschrieben.</span><span class="sxs-lookup"><span data-stu-id="73bc5-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="73bc5-184">Konfigurationswerte entsprechen den folgenden Konventionen:</span><span class="sxs-lookup"><span data-stu-id="73bc5-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="73bc5-185">Die Werte sind Zeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-185">Values are strings.</span></span>
* <span data-ttu-id="73bc5-186">NULL-Werte können nicht in einer Konfiguration gespeichert oder an Objekte gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="73bc5-187">Anbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-187">Providers</span></span>

<span data-ttu-id="73bc5-188">Die folgende Tabelle zeigt die für ASP.NET Core-Apps verfügbaren Konfigurationsanbieter.</span><span class="sxs-lookup"><span data-stu-id="73bc5-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="73bc5-189">Anbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-189">Provider</span></span> | <span data-ttu-id="73bc5-190">Bereitstellung der Konfiguration über&hellip;</span><span class="sxs-lookup"><span data-stu-id="73bc5-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="73bc5-191">[Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration) (Thema *Sicherheit*)</span><span class="sxs-lookup"><span data-stu-id="73bc5-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="73bc5-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="73bc5-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="73bc5-193">Befehlszeilen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="73bc5-194">Befehlszeilenparameter</span><span class="sxs-lookup"><span data-stu-id="73bc5-194">Command-line parameters</span></span> |
| [<span data-ttu-id="73bc5-195">Benutzerdefinierter Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="73bc5-196">Benutzerdefinierte Quelle</span><span class="sxs-lookup"><span data-stu-id="73bc5-196">Custom source</span></span> |
| [<span data-ttu-id="73bc5-197">Umgebungsvariablen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="73bc5-198">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="73bc5-198">Environment variables</span></span> |
| [<span data-ttu-id="73bc5-199">Dateikonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="73bc5-200">Dateien (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="73bc5-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="73bc5-201">Schlüssel-pro-Datei-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="73bc5-202">Verzeichnisdateien</span><span class="sxs-lookup"><span data-stu-id="73bc5-202">Directory files</span></span> |
| [<span data-ttu-id="73bc5-203">Speicherkonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="73bc5-204">In-Memory-Sammlungen</span><span class="sxs-lookup"><span data-stu-id="73bc5-204">In-memory collections</span></span> |
| <span data-ttu-id="73bc5-205">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (Thema *Sicherheit*)</span><span class="sxs-lookup"><span data-stu-id="73bc5-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="73bc5-206">Datei im Benutzerprofilverzeichnis</span><span class="sxs-lookup"><span data-stu-id="73bc5-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="73bc5-207">Anbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-207">Provider</span></span> | <span data-ttu-id="73bc5-208">Bereitstellung der Konfiguration über&hellip;</span><span class="sxs-lookup"><span data-stu-id="73bc5-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="73bc5-209">[Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration) (Thema *Sicherheit*)</span><span class="sxs-lookup"><span data-stu-id="73bc5-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="73bc5-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="73bc5-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="73bc5-211">Befehlszeilen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="73bc5-212">Befehlszeilenparameter</span><span class="sxs-lookup"><span data-stu-id="73bc5-212">Command-line parameters</span></span> |
| [<span data-ttu-id="73bc5-213">Benutzerdefinierter Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="73bc5-214">Benutzerdefinierte Quelle</span><span class="sxs-lookup"><span data-stu-id="73bc5-214">Custom source</span></span> |
| [<span data-ttu-id="73bc5-215">Umgebungsvariablen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="73bc5-216">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="73bc5-216">Environment variables</span></span> |
| [<span data-ttu-id="73bc5-217">Dateikonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="73bc5-218">Dateien (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="73bc5-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="73bc5-219">Speicherkonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="73bc5-220">In-Memory-Sammlungen</span><span class="sxs-lookup"><span data-stu-id="73bc5-220">In-memory collections</span></span> |
| <span data-ttu-id="73bc5-221">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (Thema *Sicherheit*)</span><span class="sxs-lookup"><span data-stu-id="73bc5-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="73bc5-222">Datei im Benutzerprofilverzeichnis</span><span class="sxs-lookup"><span data-stu-id="73bc5-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="73bc5-223">Anbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-223">Provider</span></span> | <span data-ttu-id="73bc5-224">Bereitstellung der Konfiguration über&hellip;</span><span class="sxs-lookup"><span data-stu-id="73bc5-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="73bc5-225">Befehlszeilen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="73bc5-226">Befehlszeilenparameter</span><span class="sxs-lookup"><span data-stu-id="73bc5-226">Command-line parameters</span></span> |
| [<span data-ttu-id="73bc5-227">Benutzerdefinierter Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="73bc5-228">Benutzerdefinierte Quelle</span><span class="sxs-lookup"><span data-stu-id="73bc5-228">Custom source</span></span> |
| [<span data-ttu-id="73bc5-229">Umgebungsvariablen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="73bc5-230">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="73bc5-230">Environment variables</span></span> |
| [<span data-ttu-id="73bc5-231">Dateikonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="73bc5-232">Dateien (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="73bc5-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="73bc5-233">Speicherkonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="73bc5-234">In-Memory-Sammlungen</span><span class="sxs-lookup"><span data-stu-id="73bc5-234">In-memory collections</span></span> |
| <span data-ttu-id="73bc5-235">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (Thema *Sicherheit*)</span><span class="sxs-lookup"><span data-stu-id="73bc5-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="73bc5-236">Datei im Benutzerprofilverzeichnis</span><span class="sxs-lookup"><span data-stu-id="73bc5-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="73bc5-237">Konfigurationsquellen werden beim Start in der Reihenfolge gelesen, in der ihre Konfigurationsanbieter angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="73bc5-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="73bc5-238">Die in diesem Thema beschriebenen Konfigurationsanbieter werden in alphabetischer Reihenfolge beschrieben, nicht in der Reihenfolge, in der Ihr Code sie anordnet.</span><span class="sxs-lookup"><span data-stu-id="73bc5-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="73bc5-239">Ordnen Sie die Konfigurationsanbieter in Ihrem Code so an, dass sie den Prioritäten für die zugrunde liegenden Konfigurationsquellen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="73bc5-240">Eine typische Konfigurationsanbietersequenz ist:</span><span class="sxs-lookup"><span data-stu-id="73bc5-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="73bc5-241">Dateien (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, wobei `<Environment>` die aktuelle APP-Hostingumgebung ist)</span><span class="sxs-lookup"><span data-stu-id="73bc5-241">Files (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, where `<Environment>` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="73bc5-242">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (nur in der Entwicklungsumgebung)</span><span class="sxs-lookup"><span data-stu-id="73bc5-242">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="73bc5-243">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="73bc5-243">Environment variables</span></span>
1. <span data-ttu-id="73bc5-244">Befehlszeilenargumente</span><span class="sxs-lookup"><span data-stu-id="73bc5-244">Command-line arguments</span></span>

<span data-ttu-id="73bc5-245">In der Regel werden Befehlszeilen-Konfigurationsanbieter zuletzt in einer Reihe von Anbietern positioniert, damit Befehlszeilenargumente die von anderen Anbietern festgelegte Konfiguration überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="73bc5-245">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73bc5-246">Diese Anbieterreihenfolge wird beim Initialisieren eines neuen <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>-Elements mit <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>festgelegt.</span><span class="sxs-lookup"><span data-stu-id="73bc5-246">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="73bc5-247">Weitere Informationen finden Sie unter [Webhost: Einrichten eines Hosts](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="73bc5-247">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="73bc5-248">Rufen Sie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> beim Erstellen des Webhost auf, um die Konfigurationsanbieter der App anzugeben:</span><span class="sxs-lookup"><span data-stu-id="73bc5-248">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="73bc5-249">`ConfigureAppConfiguration` *ist in ASP.NET Core 2.1 und höher verfügbar*.</span><span class="sxs-lookup"><span data-stu-id="73bc5-249">`ConfigureAppConfiguration` *is available in ASP.NET Core 2.1 or later.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73bc5-250">Diese Sequenz von Anbietern kann für die App (nicht für den Host) mit <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> und einem Aufruf der <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*>-Methode in `Startup` erstellt werden:</span><span class="sxs-lookup"><span data-stu-id="73bc5-250">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="73bc5-251">Im vorhergehenden Beispiel werden der Umgebungsname (`env.EnvironmentName`) und der Name der App-Assembly (`env.ApplicationName`) von <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="73bc5-251">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="73bc5-252">Weitere Informationen finden Sie unter <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="73bc5-252">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="73bc5-253">Befehlszeilen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-253">Command-line Configuration Provider</span></span>

<span data-ttu-id="73bc5-254"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> lädt während der Laufzeit die Konfiguration aus den Schlüssel-Wert-Paaren des Befehlszeilenarguments.</span><span class="sxs-lookup"><span data-stu-id="73bc5-254">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73bc5-255">Um die Befehlszeilenkonfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="73bc5-255">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="73bc5-256">`AddCommandLine` wird automatisch aufgerufen, wenn Sie ein neues <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>-Element mit <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> initialisieren.</span><span class="sxs-lookup"><span data-stu-id="73bc5-256">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="73bc5-257">Weitere Informationen finden Sie unter [Webhost: Einrichten eines Hosts](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="73bc5-257">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="73bc5-258">`CreateDefaultBuilder` lädt außerdem:</span><span class="sxs-lookup"><span data-stu-id="73bc5-258">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="73bc5-259">Optionale Konfiguration aus *appsettings.json* und *appsettings.&lt;Environment&gt;.json*</span><span class="sxs-lookup"><span data-stu-id="73bc5-259">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="73bc5-260">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (in der Entwicklungsumgebung)</span><span class="sxs-lookup"><span data-stu-id="73bc5-260">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="73bc5-261">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="73bc5-261">Environment variables.</span></span>

<span data-ttu-id="73bc5-262">`CreateDefaultBuilder` fügt den Befehlszeilen-Konfigurationsanbieter zuletzt hinzu.</span><span class="sxs-lookup"><span data-stu-id="73bc5-262">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="73bc5-263">Während der Laufzeit übergebene Befehlszeilenargumente überschreiben die von anderen Anbietern festgelegte Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="73bc5-263">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="73bc5-264">`CreateDefaultBuilder` wird aktiv, wenn der Host erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="73bc5-264">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="73bc5-265">Deswegen kann die durch `CreateDefaultBuilder` aktivierte Befehlszeilenkonfiguration die Konfiguration des Hosts beeinflussen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-265">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="73bc5-266">Wenn Sie den Host manuell erstellen und `CreateDefaultBuilder` nicht aufrufen, rufen Sie die Erweiterungsmethode `AddCommandLine` auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf:</span><span class="sxs-lookup"><span data-stu-id="73bc5-266">When building the host manually and not calling `CreateDefaultBuilder`, call the `AddCommandLine` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73bc5-267">Um die Befehlszeilenkonfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="73bc5-267">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="73bc5-268">Rufen Sie den Anbieter zuletzt auf, damit die während der Laufzeit übergebenen Befehlszeilenargumente die von anderen Konfigurationsanbietern bestimmte Konfiguration überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="73bc5-268">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="73bc5-269">Wenden Sie mit der <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>-Methode die Konfiguration auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an:</span><span class="sxs-lookup"><span data-stu-id="73bc5-269">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="73bc5-270">**Beispiel**</span><span class="sxs-lookup"><span data-stu-id="73bc5-270">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73bc5-271">Die 2.x-Beispiel-App nutzt die statische Hilfsmethode `CreateDefaultBuilder`, um den Host zu erstellen, die einen Aufruf von <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> enthält.</span><span class="sxs-lookup"><span data-stu-id="73bc5-271">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73bc5-272">Die 1.x-Beispiel-App ruft <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> auf <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> auf.</span><span class="sxs-lookup"><span data-stu-id="73bc5-272">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="73bc5-273">Öffnen Sie im Projektverzeichnis eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="73bc5-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="73bc5-274">Geben Sie ein Befehlszeilenargument für den `dotnet run`-Befehl an, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="73bc5-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="73bc5-275">Wenn die App ausgeführt wird, öffnen Sie einen Browser für die App unter `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="73bc5-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="73bc5-276">Beachten Sie, dass die Ausgabe das Schlüssel-Wert-Paar für das an `dotnet run` übergebene Konfigurations-Befehlszeilenargument enthält.</span><span class="sxs-lookup"><span data-stu-id="73bc5-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="73bc5-277">Argumente</span><span class="sxs-lookup"><span data-stu-id="73bc5-277">Arguments</span></span>

<span data-ttu-id="73bc5-278">Der Wert muss einem Gleichheitszeichen (`=`) folgen, oder der Schlüssel muss ein Präfix (`--` oder `/`) haben, wenn der Wert einem Leerzeichen folgt.</span><span class="sxs-lookup"><span data-stu-id="73bc5-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="73bc5-279">Der Wert kann NULL sein, wenn ein Gleichheitszeichen verwendet wird (z.B. `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="73bc5-279">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="73bc5-280">Schlüsselpräfix</span><span class="sxs-lookup"><span data-stu-id="73bc5-280">Key prefix</span></span>               | <span data-ttu-id="73bc5-281">Beispiel</span><span class="sxs-lookup"><span data-stu-id="73bc5-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="73bc5-282">Ohne Präfix</span><span class="sxs-lookup"><span data-stu-id="73bc5-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="73bc5-283">Zwei Gedankenstriche (`--`)</span><span class="sxs-lookup"><span data-stu-id="73bc5-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="73bc5-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="73bc5-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="73bc5-285">Schrägstrich (`/`)</span><span class="sxs-lookup"><span data-stu-id="73bc5-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="73bc5-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="73bc5-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="73bc5-287">Kombinieren Sie in einem Befehl nicht Schlüssel-Wert-Paare, die ein Gleichheitszeichen verwenden, mit Schlüssel-Wert-Paaren, die ein Leerzeichen verwenden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="73bc5-288">Beispielbefehle:</span><span class="sxs-lookup"><span data-stu-id="73bc5-288">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="73bc5-289">Switchmappings</span><span class="sxs-lookup"><span data-stu-id="73bc5-289">Switch mappings</span></span>

<span data-ttu-id="73bc5-290">Switchmappings erlauben das Angeben einer Logik zum Ersetzen von Schlüsselnamen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="73bc5-291">Wenn Sie die Konfiguration manuell mit <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> erstellen, können Sie ein Wörterbuch der Switchersetzungen für die <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>-Methode bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="73bc5-292">Wenn das Switchmappingwörterbuch verwendet wird, wird das Wörterbuch für Schlüssel, die dem von einem Befehlszeilenargument angegebenen Schlüssel entsprechen, überprüft.</span><span class="sxs-lookup"><span data-stu-id="73bc5-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="73bc5-293">Wenn der Befehlszeilenschlüssel im Wörterbuch gefunden wird, wird der Wörterbuchwert (der Schlüsselersatz) zum Festlegen des Schlüssel-Wert-Paares in der App-Konfiguration zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="73bc5-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="73bc5-294">Ein Switchmapping ist für jeden Befehlszeilenschlüssel erforderlich, dem ein einzelner Gedankenstrich (`-`) vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="73bc5-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="73bc5-295">Regeln für Schlüssel von Switchmappingwörterbüchern:</span><span class="sxs-lookup"><span data-stu-id="73bc5-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="73bc5-296">Switchmappings müssen mit einem Gedankenstrich (`-`) oder zwei Gedankenstrichen (`--`) beginnen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="73bc5-297">Das Switchmappingwörterbuch darf keine doppelten Schlüssel enthalten.</span><span class="sxs-lookup"><span data-stu-id="73bc5-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.0"

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

        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="73bc5-298">Wie im vorherigen Beispiel gezeigt, darf der Aufruf von `CreateDefaultBuilder` keine Argumente übergeben, wenn Switchmappings verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-298">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="73bc5-299">Der Aufruf `AddCommandLine` der `CreateDefaultBuilder`-Methode umfasst keine zugeordneten Switches, und das Switchmappingwörterbuch kann nicht an `CreateDefaultBuilder` übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-299">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="73bc5-300">Wenn die Argumente einen zugeordneten Switch enthalten und an `CreateDefaultBuilder` übergeben werden, kann der `AddCommandLine`-Anbieter nicht mit <xref:System.FormatException> initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-300">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="73bc5-301">Die Lösung besteht nicht darin, die Argumente an `CreateDefaultBuilder` zu übergeben, sondern der Methode `AddCommandLine` der `ConfigurationBuilder`-Methode zu erlauben, sowohl die Argumente als auch das Switchmappingwörterbuch zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="73bc5-301">The solution is not to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="73bc5-302">Das erstellte Switchmappingwörterbuch enthält die in der folgenden Tabelle gezeigten Daten.</span><span class="sxs-lookup"><span data-stu-id="73bc5-302">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="73bc5-303">Key</span><span class="sxs-lookup"><span data-stu-id="73bc5-303">Key</span></span>       | <span data-ttu-id="73bc5-304">Wert</span><span class="sxs-lookup"><span data-stu-id="73bc5-304">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="73bc5-305">Wenn Switches zugeordnete Schlüssel beim Start der App verwendet werden, erhält die Konfiguration den Konfigurationswert auf dem vom Wörterbuch bereitgestellten Schlüssel:</span><span class="sxs-lookup"><span data-stu-id="73bc5-305">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="73bc5-306">Nach dem Ausführen des obigen Befehls enthält die Konfiguration die in der folgenden Tabelle angegebenen Werte.</span><span class="sxs-lookup"><span data-stu-id="73bc5-306">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="73bc5-307">Key</span><span class="sxs-lookup"><span data-stu-id="73bc5-307">Key</span></span>               | <span data-ttu-id="73bc5-308">Wert</span><span class="sxs-lookup"><span data-stu-id="73bc5-308">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="73bc5-309">Umgebungsvariablen-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-309">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="73bc5-310"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> lädt während der Laufzeit die Konfiguration aus den Schlüssel-Wert-Paaren der Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-310">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="73bc5-311">Um die Umgebungsvariablenkonfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="73bc5-311">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="73bc5-312">Beim Verwenden hierarchischer Schlüssel in Umgebungsvariablen funktioniert ein Doppelpunkt (`:`) als Trennzeichen ggf. nicht auf allen Plattformen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-312">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="73bc5-313">Ein doppelter Unterstrich (`__`) wird auf allen Plattformen unterstützt und durch einen Doppelpunkt ersetzt.</span><span class="sxs-lookup"><span data-stu-id="73bc5-313">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="73bc5-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) ermöglicht Ihnen Umgebungsvariablen im Azure-Portal festzulegen, die die App-Konfiguration mit dem Umgebungsvariablen-Konfigurationsanbieter überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="73bc5-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="73bc5-315">Weitere Informationen finden Sie unter [Azure Apps: Überschreiben der App-Konfiguration mit dem Azure-Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="73bc5-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73bc5-316">`AddEnvironmentVariables` wird automatisch aufgerufen, wenn Sie ein neues <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>-Element mit <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> initialisieren.</span><span class="sxs-lookup"><span data-stu-id="73bc5-316">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="73bc5-317">Weitere Informationen finden Sie unter [Webhost: Einrichten eines Hosts](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="73bc5-317">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="73bc5-318">`CreateDefaultBuilder` lädt außerdem:</span><span class="sxs-lookup"><span data-stu-id="73bc5-318">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="73bc5-319">Optionale Konfiguration aus *appsettings.json* und *appsettings.&lt;Environment&gt;.json*</span><span class="sxs-lookup"><span data-stu-id="73bc5-319">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="73bc5-320">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (in der Entwicklungsumgebung)</span><span class="sxs-lookup"><span data-stu-id="73bc5-320">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="73bc5-321">Befehlszeilenargumenten</span><span class="sxs-lookup"><span data-stu-id="73bc5-321">Command-line arguments.</span></span>

<span data-ttu-id="73bc5-322">Der Umgebungsvariablen-Konfigurationsanbieter wird aufgerufen, nachdem die Konfiguration aus Benutzergeheimnissen und *appsettings*-Dateien erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="73bc5-322">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="73bc5-323">Das Aufrufen des Anbieters an dieser Stelle ermöglicht den während der Laufzeit gelesenen Umgebungsvariablen, die von Benutzergeheimnissen und *appsettings*-Dateien festgelegte Konfiguration zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="73bc5-323">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="73bc5-324">Sie können die Erweiterungsmethode `AddEnvironmentVariables` auch direkt auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz aufrufen:</span><span class="sxs-lookup"><span data-stu-id="73bc5-324">You can also directly call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73bc5-325">Wenden Sie die Konfiguration mit der `UseConfiguration`-Methode auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an:</span><span class="sxs-lookup"><span data-stu-id="73bc5-325">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="73bc5-326">**Beispiel**</span><span class="sxs-lookup"><span data-stu-id="73bc5-326">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73bc5-327">Die 2.x-Beispiel-App nutzt die statische Hilfsmethode `CreateDefaultBuilder`, um den Host zu erstellen, die einen Aufruf von `AddEnvironmentVariables` enthält.</span><span class="sxs-lookup"><span data-stu-id="73bc5-327">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73bc5-328">Die 1.x-Beispiel-App ruft `AddEnvironmentVariables` auf `ConfigurationBuilder` auf.</span><span class="sxs-lookup"><span data-stu-id="73bc5-328">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="73bc5-329">Führen Sie die Beispiel-App aus.</span><span class="sxs-lookup"><span data-stu-id="73bc5-329">Run the sample app.</span></span> <span data-ttu-id="73bc5-330">Öffnen Sie einen Browser für die App unter `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="73bc5-330">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="73bc5-331">Beachten Sie, dass die Ausgabe das Schlüssel-Wert-Paar für die Umgebungsvariable `ENVIRONMENT` enthält.</span><span class="sxs-lookup"><span data-stu-id="73bc5-331">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="73bc5-332">Der Wert entspricht der Umgebung, in der die App ausgeführt wird. In der Regel ist das `Development` bei lokaler Ausführung.</span><span class="sxs-lookup"><span data-stu-id="73bc5-332">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="73bc5-333">Um die Liste der von der App gerenderten Umgebungsvariablen kurz zu halten, filtert die App Umgebungsvariablen, die folgendermaßen beginnen:</span><span class="sxs-lookup"><span data-stu-id="73bc5-333">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="73bc5-334">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="73bc5-334">ASPNETCORE_</span></span>
* <span data-ttu-id="73bc5-335">urls</span><span class="sxs-lookup"><span data-stu-id="73bc5-335">urls</span></span>
* <span data-ttu-id="73bc5-336">Protokollierung</span><span class="sxs-lookup"><span data-stu-id="73bc5-336">Logging</span></span>
* <span data-ttu-id="73bc5-337">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="73bc5-337">ENVIRONMENT</span></span>
* <span data-ttu-id="73bc5-338">contentRoot</span><span class="sxs-lookup"><span data-stu-id="73bc5-338">contentRoot</span></span>
* <span data-ttu-id="73bc5-339">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="73bc5-339">AllowedHosts</span></span>
* <span data-ttu-id="73bc5-340">applicationName</span><span class="sxs-lookup"><span data-stu-id="73bc5-340">applicationName</span></span>
* <span data-ttu-id="73bc5-341">COMMANDLINE</span><span class="sxs-lookup"><span data-stu-id="73bc5-341">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73bc5-342">Wenn Sie alle für die App verfügbaren Umgebungsvariablen verfügbar machen möchten, ändern Sie `FilteredConfiguration` in *Pages/Index.cshtml.cs* wie folgt:</span><span class="sxs-lookup"><span data-stu-id="73bc5-342">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73bc5-343">Wenn Sie alle für die App verfügbaren Umgebungsvariablen verfügbar machen möchten, ändern Sie `FilteredConfiguration` in *Controllers/HomeController.cs* wie folgt:</span><span class="sxs-lookup"><span data-stu-id="73bc5-343">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="73bc5-344">Präfixe</span><span class="sxs-lookup"><span data-stu-id="73bc5-344">Prefixes</span></span>

<span data-ttu-id="73bc5-345">Umgebungsvariablen, die in die Konfiguration der App geladen werden, werden gefiltert, wenn Sie ein Präfix für die Methode `AddEnvironmentVariables` angeben.</span><span class="sxs-lookup"><span data-stu-id="73bc5-345">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="73bc5-346">Um beispielsweise Umgebungsvariablen nach dem Präfix `CUSTOM_` zu filtern, geben Sie das Präfix für den Konfigurationsanbieter an:</span><span class="sxs-lookup"><span data-stu-id="73bc5-346">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="73bc5-347">Das Präfix wird beim Erstellen der Schlüssel-Wert-Paare der Konfiguration entfernt.</span><span class="sxs-lookup"><span data-stu-id="73bc5-347">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73bc5-348">Die statische Hilfsmethode `CreateDefaultBuilder` erstellt ein <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>-Element, um den Host der App einzurichten.</span><span class="sxs-lookup"><span data-stu-id="73bc5-348">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="73bc5-349">Wenn `WebHostBuilder` erstellt wird, befindet sich die Hostkonfiguration in Umgebungsvariablen mit dem Präfix `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="73bc5-349">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="73bc5-350">**Präfixe für Verbindungszeichenfolgen**</span><span class="sxs-lookup"><span data-stu-id="73bc5-350">**Connection string prefixes**</span></span>

<span data-ttu-id="73bc5-351">Die Konfigurations-API verfügt über spezielle Verarbeitungsregeln für vier Umgebungsvariablen für Verbindungszeichenfolgen, die beim Konfigurieren von Azure-Verbindungszeichenfolgen für die App-Umgebung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-351">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="73bc5-352">Umgebungsvariablen mit Präfixen, die in der Tabelle aufgeführt werden, werden in die App geladen, wenn für `AddEnvironmentVariables` kein Präfix angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="73bc5-352">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="73bc5-353">Präfix für Verbindungszeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="73bc5-353">Connection string prefix</span></span> | <span data-ttu-id="73bc5-354">Anbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-354">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="73bc5-355">Benutzerdefinierter Anbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-355">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="73bc5-356">MySQL</span><span class="sxs-lookup"><span data-stu-id="73bc5-356">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="73bc5-357">Azure SQL-Datenbank</span><span class="sxs-lookup"><span data-stu-id="73bc5-357">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="73bc5-358">SQL Server</span><span class="sxs-lookup"><span data-stu-id="73bc5-358">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="73bc5-359">Wenn eine Umgebungsvariable entdeckt und mit einem der vier Präfixe aus der Tabelle in die Konfiguration geladen wird, tritt Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="73bc5-359">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="73bc5-360">Der Konfigurationsschlüssel wird durch Entfernen des Umgebungsvariablenpräfixes und Hinzufügen eines Konfigurationsschlüsselabschnitts (`ConnectionStrings`) erstellt.</span><span class="sxs-lookup"><span data-stu-id="73bc5-360">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="73bc5-361">Ein neues Konfigurations-Schlüssel-Wert-Paar wird erstellt, das den Datenbankverbindungsanbieter repräsentiert (mit Ausnahme des Präfixes `CUSTOMCONNSTR_`, für das kein Anbieter angegeben ist).</span><span class="sxs-lookup"><span data-stu-id="73bc5-361">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="73bc5-362">Umgebungsvariablenschlüssel</span><span class="sxs-lookup"><span data-stu-id="73bc5-362">Environment variable key</span></span> | <span data-ttu-id="73bc5-363">Konvertierter Konfigurationsschlüssel</span><span class="sxs-lookup"><span data-stu-id="73bc5-363">Converted configuration key</span></span> | <span data-ttu-id="73bc5-364">Anbieterkonfigurationseintrag</span><span class="sxs-lookup"><span data-stu-id="73bc5-364">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="73bc5-365">Konfigurationseintrag wurde nicht erstellt.</span><span class="sxs-lookup"><span data-stu-id="73bc5-365">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="73bc5-366">Schlüssel: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="73bc5-366">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="73bc5-367">Wert: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="73bc5-367">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="73bc5-368">Schlüssel: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="73bc5-368">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="73bc5-369">Wert: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="73bc5-369">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="73bc5-370">Schlüssel: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="73bc5-370">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="73bc5-371">Wert: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="73bc5-371">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="73bc5-372">Dateikonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-372">File Configuration Provider</span></span>

<span data-ttu-id="73bc5-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> ist die Basisklasse für das Laden einer Konfiguration aus dem Dateisystem.</span><span class="sxs-lookup"><span data-stu-id="73bc5-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="73bc5-374">Die folgenden Konfigurationsanbieter sind für bestimmte Dateitypen vorgesehen:</span><span class="sxs-lookup"><span data-stu-id="73bc5-374">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="73bc5-375">INI-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-375">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="73bc5-376">JSON-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-376">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="73bc5-377">XML-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-377">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="73bc5-378">INI-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-378">INI Configuration Provider</span></span>

<span data-ttu-id="73bc5-379"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> lädt während der Laufzeit die Konfiguration aus den Schlüssel-Wert-Paaren der INI-Datei.</span><span class="sxs-lookup"><span data-stu-id="73bc5-379">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="73bc5-380">Um die INI-Dateikonfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="73bc5-380">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="73bc5-381">Der Doppelpunkt kann in einer INI-Dateikonfiguration als Abschnittstrennzeichen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-381">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="73bc5-382">Überladungen geben Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="73bc5-382">Overloads permit specifying:</span></span>

* <span data-ttu-id="73bc5-383">Ob die Datei optional ist</span><span class="sxs-lookup"><span data-stu-id="73bc5-383">Whether the file is optional.</span></span>
* <span data-ttu-id="73bc5-384">Ob die Konfiguration bei Dateiänderungen neu geladen wird</span><span class="sxs-lookup"><span data-stu-id="73bc5-384">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="73bc5-385">Dass mit <xref:Microsoft.Extensions.FileProviders.IFileProvider> auf die Datei zugegriffen wird</span><span class="sxs-lookup"><span data-stu-id="73bc5-385">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73bc5-386">Rufen Sie beim Aufrufen von `CreateDefaultBuilder` `UseConfiguration` mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="73bc5-386">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="73bc5-387">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> `UseConfiguration` mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="73bc5-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73bc5-388">Wenden Sie die Konfiguration mit der `UseConfiguration`-Methode auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an:</span><span class="sxs-lookup"><span data-stu-id="73bc5-388">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="73bc5-389">Ein allgemeines Beispiel einer INI-Konfigurationsdatei:</span><span class="sxs-lookup"><span data-stu-id="73bc5-389">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="73bc5-390">Die vorherige Konfigurationsdatei lädt die folgenden Schlüssel mit `value`:</span><span class="sxs-lookup"><span data-stu-id="73bc5-390">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="73bc5-391">section0:key0</span><span class="sxs-lookup"><span data-stu-id="73bc5-391">section0:key0</span></span>
* <span data-ttu-id="73bc5-392">section0:key1</span><span class="sxs-lookup"><span data-stu-id="73bc5-392">section0:key1</span></span>
* <span data-ttu-id="73bc5-393">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="73bc5-393">section1:subsection:key</span></span>
* <span data-ttu-id="73bc5-394">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="73bc5-394">section2:subsection0:key</span></span>
* <span data-ttu-id="73bc5-395">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="73bc5-395">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="73bc5-396">JSON-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-396">JSON Configuration Provider</span></span>

<span data-ttu-id="73bc5-397"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> lädt während der Laufzeit die Konfiguration aus den Schlüssel-Wert-Paaren der JSON-Datei.</span><span class="sxs-lookup"><span data-stu-id="73bc5-397">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="73bc5-398">Um die JSON-Dateikonfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="73bc5-398">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="73bc5-399">Überladungen geben Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="73bc5-399">Overloads permit specifying:</span></span>

* <span data-ttu-id="73bc5-400">Ob die Datei optional ist</span><span class="sxs-lookup"><span data-stu-id="73bc5-400">Whether the file is optional.</span></span>
* <span data-ttu-id="73bc5-401">Ob die Konfiguration bei Dateiänderungen neu geladen wird</span><span class="sxs-lookup"><span data-stu-id="73bc5-401">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="73bc5-402">Dass mit <xref:Microsoft.Extensions.FileProviders.IFileProvider> auf die Datei zugegriffen wird</span><span class="sxs-lookup"><span data-stu-id="73bc5-402">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73bc5-403">`AddJsonFile` wird automatisch zweimal aufgerufen, wenn Sie ein neues <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>-Element mit <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> initialisieren.</span><span class="sxs-lookup"><span data-stu-id="73bc5-403">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="73bc5-404">Die Methode wird aufgerufen, um die Konfiguration aus folgenden Dateien zu laden:</span><span class="sxs-lookup"><span data-stu-id="73bc5-404">The method is called to load configuration from:</span></span>

* <span data-ttu-id="73bc5-405">*appsettings.json* &ndash; Diese Datei wird zuerst gelesen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-405">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="73bc5-406">Die Umgebungsversion der Datei kann die Werte der Datei *appsettings.json* überschreiben.</span><span class="sxs-lookup"><span data-stu-id="73bc5-406">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="73bc5-407">*appsettings.&lt;Environment&gt;.json* &ndash; Die Umgebungsversion der Datei wird basierend auf [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*) geladen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-407">*appsettings.&lt;Environment&gt;.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="73bc5-408">Weitere Informationen finden Sie unter [Webhost: Einrichten eines Hosts](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="73bc5-408">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="73bc5-409">`CreateDefaultBuilder` lädt außerdem:</span><span class="sxs-lookup"><span data-stu-id="73bc5-409">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="73bc5-410">Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-410">Environment variables.</span></span>
* <span data-ttu-id="73bc5-411">[Benutzergeheimnisse (Geheimnis-Manager)](xref:security/app-secrets) (in der Entwicklungsumgebung)</span><span class="sxs-lookup"><span data-stu-id="73bc5-411">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="73bc5-412">Befehlszeilenargumenten</span><span class="sxs-lookup"><span data-stu-id="73bc5-412">Command-line arguments.</span></span>

<span data-ttu-id="73bc5-413">Der JSON-Konfigurationsanbieter wird zuerst eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="73bc5-413">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="73bc5-414">Daher überschreiben Benutzergeheimnisse, Umgebungsvariablen und Befehlszeilenargumente die von *appsettings*-Dateien festgelegte Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="73bc5-414">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="73bc5-415">Sie können die Erweiterungsmethode `AddJsonFile` auch direkt auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz aufrufen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-415">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="73bc5-416">Rufen Sie beim Aufrufen von `CreateDefaultBuilder` `UseConfiguration` mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="73bc5-416">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="73bc5-417">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> `UseConfiguration` mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="73bc5-417">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73bc5-418">Wenden Sie die Konfiguration mit der `UseConfiguration`-Methode auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an:</span><span class="sxs-lookup"><span data-stu-id="73bc5-418">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="73bc5-419">**Beispiel**</span><span class="sxs-lookup"><span data-stu-id="73bc5-419">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73bc5-420">Die 2.x-Beispiel-App nutzt die statische Hilfsmethode `CreateDefaultBuilder`, um den Host zu erstellen, die zwei Aufrufe von `AddJsonFile` enthält.</span><span class="sxs-lookup"><span data-stu-id="73bc5-420">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="73bc5-421">Die Konfiguration wird aus *appsettings.json* und *appsettings.&lt;Environment&gt;.json* geladen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-421">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73bc5-422">Die 1.x-Beispiel-App ruft `AddJsonFile` zweimal auf `ConfigurationBuilder` auf.</span><span class="sxs-lookup"><span data-stu-id="73bc5-422">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="73bc5-423">Die Konfiguration wird aus *appsettings.json* und *appsettings.&lt;Environment&gt;.json* geladen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-423">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="73bc5-424">Führen Sie die Beispiel-App aus.</span><span class="sxs-lookup"><span data-stu-id="73bc5-424">Run the sample app.</span></span> <span data-ttu-id="73bc5-425">Öffnen Sie einen Browser für die App unter `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="73bc5-425">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="73bc5-426">Die Ausgabe enthält je nach Umgebung Schlüssel-Wert-Paare für die in der Tabelle dargestellte Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="73bc5-426">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="73bc5-427">Protokollierungskonfigurationsschlüssel verwenden den Doppelpunkt (`:`) als hierarchisches Trennzeichen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-427">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="73bc5-428">Key</span><span class="sxs-lookup"><span data-stu-id="73bc5-428">Key</span></span>                        | <span data-ttu-id="73bc5-429">Entwicklungswert</span><span class="sxs-lookup"><span data-stu-id="73bc5-429">Development Value</span></span> | <span data-ttu-id="73bc5-430">Produktionswert</span><span class="sxs-lookup"><span data-stu-id="73bc5-430">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="73bc5-431">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="73bc5-431">Logging:LogLevel:System</span></span>    | <span data-ttu-id="73bc5-432">Information</span><span class="sxs-lookup"><span data-stu-id="73bc5-432">Information</span></span>       | <span data-ttu-id="73bc5-433">Information</span><span class="sxs-lookup"><span data-stu-id="73bc5-433">Information</span></span>      |
| <span data-ttu-id="73bc5-434">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="73bc5-434">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="73bc5-435">Information</span><span class="sxs-lookup"><span data-stu-id="73bc5-435">Information</span></span>       | <span data-ttu-id="73bc5-436">Information</span><span class="sxs-lookup"><span data-stu-id="73bc5-436">Information</span></span>      |
| <span data-ttu-id="73bc5-437">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="73bc5-437">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="73bc5-438">Debug</span><span class="sxs-lookup"><span data-stu-id="73bc5-438">Debug</span></span>             | <span data-ttu-id="73bc5-439">Fehler</span><span class="sxs-lookup"><span data-stu-id="73bc5-439">Error</span></span>            |
| <span data-ttu-id="73bc5-440">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="73bc5-440">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="73bc5-441">XML-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-441">XML Configuration Provider</span></span>

<span data-ttu-id="73bc5-442"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> lädt während der Laufzeit die Konfiguration aus den Schlüssel-Wert-Paaren der XML-Datei.</span><span class="sxs-lookup"><span data-stu-id="73bc5-442">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="73bc5-443">Um die XML-Dateikonfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="73bc5-443">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="73bc5-444">Überladungen geben Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="73bc5-444">Overloads permit specifying:</span></span>

* <span data-ttu-id="73bc5-445">Ob die Datei optional ist</span><span class="sxs-lookup"><span data-stu-id="73bc5-445">Whether the file is optional.</span></span>
* <span data-ttu-id="73bc5-446">Ob die Konfiguration bei Dateiänderungen neu geladen wird</span><span class="sxs-lookup"><span data-stu-id="73bc5-446">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="73bc5-447">Dass mit <xref:Microsoft.Extensions.FileProviders.IFileProvider> auf die Datei zugegriffen wird</span><span class="sxs-lookup"><span data-stu-id="73bc5-447">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="73bc5-448">Der Stammknoten der Konfigurationsdatei wird beim Erstellen der Konfigurations-Schlüssel-Wert-Paare ignoriert.</span><span class="sxs-lookup"><span data-stu-id="73bc5-448">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="73bc5-449">Geben Sie keine Dokumenttypdefinition (DTD) und keinen Namespace in der Datei an.</span><span class="sxs-lookup"><span data-stu-id="73bc5-449">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73bc5-450">Rufen Sie beim Aufrufen von `CreateDefaultBuilder` `UseConfiguration` mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="73bc5-450">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="73bc5-451">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> `UseConfiguration` mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="73bc5-451">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73bc5-452">Wenden Sie die Konfiguration mit der `UseConfiguration`-Methode auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an:</span><span class="sxs-lookup"><span data-stu-id="73bc5-452">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

<span data-ttu-id="73bc5-453">XML-Konfigurationsdateien können unterschiedliche Elementnamen für wiederholte Abschnitte verwenden:</span><span class="sxs-lookup"><span data-stu-id="73bc5-453">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="73bc5-454">Die vorherige Konfigurationsdatei lädt die folgenden Schlüssel mit `value`:</span><span class="sxs-lookup"><span data-stu-id="73bc5-454">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="73bc5-455">section0:key0</span><span class="sxs-lookup"><span data-stu-id="73bc5-455">section0:key0</span></span>
* <span data-ttu-id="73bc5-456">section0:key1</span><span class="sxs-lookup"><span data-stu-id="73bc5-456">section0:key1</span></span>
* <span data-ttu-id="73bc5-457">section1:key0</span><span class="sxs-lookup"><span data-stu-id="73bc5-457">section1:key0</span></span>
* <span data-ttu-id="73bc5-458">section1:key1</span><span class="sxs-lookup"><span data-stu-id="73bc5-458">section1:key1</span></span>

<span data-ttu-id="73bc5-459">Wiederholte Elemente mit den gleichen Elementnamen funktionieren, wenn das `name`-Attribut zur Unterscheidung der Elemente verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="73bc5-459">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="73bc5-460">Die vorherige Konfigurationsdatei lädt die folgenden Schlüssel mit `value`:</span><span class="sxs-lookup"><span data-stu-id="73bc5-460">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="73bc5-461">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="73bc5-461">section:section0:key:key0</span></span>
* <span data-ttu-id="73bc5-462">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="73bc5-462">section:section0:key:key1</span></span>
* <span data-ttu-id="73bc5-463">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="73bc5-463">section:section1:key:key0</span></span>
* <span data-ttu-id="73bc5-464">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="73bc5-464">section:section1:key:key1</span></span>

<span data-ttu-id="73bc5-465">Mit Attributen können Werte bereitgestellt werden:</span><span class="sxs-lookup"><span data-stu-id="73bc5-465">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="73bc5-466">Die vorherige Konfigurationsdatei lädt die folgenden Schlüssel mit `value`:</span><span class="sxs-lookup"><span data-stu-id="73bc5-466">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="73bc5-467">key:attribute</span><span class="sxs-lookup"><span data-stu-id="73bc5-467">key:attribute</span></span>
* <span data-ttu-id="73bc5-468">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="73bc5-468">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="73bc5-469">Schlüssel-pro-Datei-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-469">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="73bc5-470"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> verwendet Verzeichnisdateien als Konfigurations-Schlüssel-Wert-Paare.</span><span class="sxs-lookup"><span data-stu-id="73bc5-470">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="73bc5-471">Der Schlüssel ist der Dateiname.</span><span class="sxs-lookup"><span data-stu-id="73bc5-471">The key is the file name.</span></span> <span data-ttu-id="73bc5-472">Der Wert enthält den Inhalt der Datei.</span><span class="sxs-lookup"><span data-stu-id="73bc5-472">The value contains the file's contents.</span></span> <span data-ttu-id="73bc5-473">Der Schlüssel-pro-Datei-Konfigurationsanbieter wird in Docker-Hostingszenarien verwendet.</span><span class="sxs-lookup"><span data-stu-id="73bc5-473">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="73bc5-474">Um die Schlüssel-pro-Datei-Konfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="73bc5-474">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="73bc5-475">Der `directoryPath` zu den Dateien muss ein absoluter Pfad sein.</span><span class="sxs-lookup"><span data-stu-id="73bc5-475">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="73bc5-476">Überladungen geben Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="73bc5-476">Overloads permit specifying:</span></span>

* <span data-ttu-id="73bc5-477">Einen `Action<KeyPerFileConfigurationSource>`-Delegat, der die Quelle konfiguriert</span><span class="sxs-lookup"><span data-stu-id="73bc5-477">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="73bc5-478">Ob das Verzeichnis optional ist; und den Pfad zum Verzeichnis</span><span class="sxs-lookup"><span data-stu-id="73bc5-478">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="73bc5-479">Rufen Sie beim Aufrufen von `CreateDefaultBuilder` `UseConfiguration` mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="73bc5-479">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
        var config = new ConfigurationBuilder()
            .AddKeyPerFile(directoryPath: path, optional: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="73bc5-480">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> `UseConfiguration` mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="73bc5-480">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="73bc5-481">Speicherkonfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-481">Memory Configuration Provider</span></span>

<span data-ttu-id="73bc5-482"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> verwendet eine In-Memory-Sammlung für Konfigurations-Schlüssel-Wert-Paare.</span><span class="sxs-lookup"><span data-stu-id="73bc5-482">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="73bc5-483">Um die In-Memory-Sammlungskonfiguration zu aktivieren, rufen Sie die <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*>-Erweiterungsmethode auf einer <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>-Instanz auf.</span><span class="sxs-lookup"><span data-stu-id="73bc5-483">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="73bc5-484">Der Konfigurationsanbieter kann mit `IEnumerable<KeyValuePair<String,String>>` initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-484">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73bc5-485">Rufen Sie beim Aufrufen von `CreateDefaultBuilder` `UseConfiguration` mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="73bc5-485">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

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

<span data-ttu-id="73bc5-486">Rufen Sie beim direkten Erstellen von <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> `UseConfiguration` mit der Konfiguration auf:</span><span class="sxs-lookup"><span data-stu-id="73bc5-486">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73bc5-487">Wenden Sie die Konfiguration mit der `UseConfiguration`-Methode auf <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> an:</span><span class="sxs-lookup"><span data-stu-id="73bc5-487">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="73bc5-488">GetValue</span><span class="sxs-lookup"><span data-stu-id="73bc5-488">GetValue</span></span>

<span data-ttu-id="73bc5-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrahiert einen Wert aus der Konfiguration mit einem angegebenen Schlüssel, und konvertiert ihn in den angegebenen Typ.</span><span class="sxs-lookup"><span data-stu-id="73bc5-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="73bc5-490">Eine Überladung erlaubt es Ihnen, einen Standardwert anzugeben, wenn der Schlüssel nicht gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="73bc5-490">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="73bc5-491">Das folgende Beispiel extrahiert den Zeichenfolgenwert aus der Konfiguration mit dem Schlüssel `NumberKey`, gibt den Wert als `int` an und speichert den Wert in der Variablen `intValue`.</span><span class="sxs-lookup"><span data-stu-id="73bc5-491">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="73bc5-492">Wenn `NumberKey` nicht im Konfigurationsschlüssel gefunden wird, erhält `intValue` den Standardwert `99`:</span><span class="sxs-lookup"><span data-stu-id="73bc5-492">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="73bc5-493">GetSection, GetChildren und Exists</span><span class="sxs-lookup"><span data-stu-id="73bc5-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="73bc5-494">Betrachten Sie für die folgenden Beispiele die JSON-Datei unten.</span><span class="sxs-lookup"><span data-stu-id="73bc5-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="73bc5-495">Vier Schlüssel befinden sich in zwei Abschnitten, von denen einer ein Paar Unterabschnitte enthält:</span><span class="sxs-lookup"><span data-stu-id="73bc5-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="73bc5-496">Wenn die Datei in die Konfiguration gelesen wird, werden die folgenden eindeutigen hierarchischen Schlüssel erstellt, um die Konfigurationswerte zu enthalten:</span><span class="sxs-lookup"><span data-stu-id="73bc5-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="73bc5-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="73bc5-497">section0:key0</span></span>
* <span data-ttu-id="73bc5-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="73bc5-498">section0:key1</span></span>
* <span data-ttu-id="73bc5-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="73bc5-499">section1:key0</span></span>
* <span data-ttu-id="73bc5-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="73bc5-500">section1:key1</span></span>
* <span data-ttu-id="73bc5-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="73bc5-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="73bc5-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="73bc5-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="73bc5-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="73bc5-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="73bc5-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="73bc5-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="73bc5-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="73bc5-505">GetSection</span></span>

<span data-ttu-id="73bc5-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrahiert einen Konfigurationsunterabschnitt mit dem angegebenen Unterabschnittsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="73bc5-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="73bc5-507">Um ein <xref:Microsoft.Extensions.Configuration.IConfigurationSection>-Element zurückzugeben, das nur die Schlüssel-Wert-Paare in `section1` enthält, rufen Sie `GetSection` auf, und geben Sie den Abschnittsnamen an:</span><span class="sxs-lookup"><span data-stu-id="73bc5-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="73bc5-508">Um die Werte für Schlüssel in `section2:subsection0` zu erhalten, rufen Sie `GetSection` auf, und geben Sie den Abschnittspfad an:</span><span class="sxs-lookup"><span data-stu-id="73bc5-508">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="73bc5-509">`GetSection` gibt nie `null` zurück.</span><span class="sxs-lookup"><span data-stu-id="73bc5-509">`GetSection` never returns `null`.</span></span> <span data-ttu-id="73bc5-510">Wenn kein entsprechender Abschnitt gefunden wird, wird ein leeres `IConfigurationSection`-Element zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="73bc5-510">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="73bc5-511">GetChildren</span><span class="sxs-lookup"><span data-stu-id="73bc5-511">GetChildren</span></span>

<span data-ttu-id="73bc5-512">Ein Aufruf von [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) auf `section2` erhält `IEnumerable<IConfigurationSection>` mit folgenden Elementen:</span><span class="sxs-lookup"><span data-stu-id="73bc5-512">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="73bc5-513">Vorhanden</span><span class="sxs-lookup"><span data-stu-id="73bc5-513">Exists</span></span>

<span data-ttu-id="73bc5-514">Verwenden Sie [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) um zu bestimmen, ob ein Konfigurationsabschnitt vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="73bc5-514">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="73bc5-515">Im Falle der Beispieldaten ist `sectionExists` `false`, weil in den Konfigurationsdaten kein Abschnitt `section2:subsection2` vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="73bc5-515">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="73bc5-516">Binden an eine Klasse</span><span class="sxs-lookup"><span data-stu-id="73bc5-516">Bind to a class</span></span>

<span data-ttu-id="73bc5-517">Die Konfiguration kann mit *Optionsmustern* an Klassen gebunden werden, die Gruppen von verwandten Einstellungen darstellen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-517">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="73bc5-518">Weitere Informationen finden Sie unter <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="73bc5-518">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="73bc5-519">Konfigurationswerte werden als Zeichenfolgen zurückgegeben, doch das Aufrufen von <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> ermöglicht die Erstellung von [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)-Objekten.</span><span class="sxs-lookup"><span data-stu-id="73bc5-519">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="73bc5-520">Die Beispiel-App enthält ein `Starship`-Modell (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="73bc5-520">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="73bc5-521">Der Abschnitt `starship` der Datei *starship.json* erstellt die Konfiguration, wenn die Beispiel-App den JSON-Konfigurationsanbieter zum Laden der Konfiguration verwendet:</span><span class="sxs-lookup"><span data-stu-id="73bc5-521">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="73bc5-522">Die folgenden Schlüssel-Wert-Paare werden für die Konfiguration erstellt:</span><span class="sxs-lookup"><span data-stu-id="73bc5-522">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="73bc5-523">Key</span><span class="sxs-lookup"><span data-stu-id="73bc5-523">Key</span></span>                   | <span data-ttu-id="73bc5-524">Wert</span><span class="sxs-lookup"><span data-stu-id="73bc5-524">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="73bc5-525">starship:name</span><span class="sxs-lookup"><span data-stu-id="73bc5-525">starship:name</span></span>         | <span data-ttu-id="73bc5-526">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="73bc5-526">USS Enterprise</span></span>                                    |
| <span data-ttu-id="73bc5-527">starship:registry</span><span class="sxs-lookup"><span data-stu-id="73bc5-527">starship:registry</span></span>     | <span data-ttu-id="73bc5-528">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="73bc5-528">NCC-1701</span></span>                                          |
| <span data-ttu-id="73bc5-529">starship:class</span><span class="sxs-lookup"><span data-stu-id="73bc5-529">starship:class</span></span>        | <span data-ttu-id="73bc5-530">Constitution</span><span class="sxs-lookup"><span data-stu-id="73bc5-530">Constitution</span></span>                                      |
| <span data-ttu-id="73bc5-531">starship:length</span><span class="sxs-lookup"><span data-stu-id="73bc5-531">starship:length</span></span>       | <span data-ttu-id="73bc5-532">304,8</span><span class="sxs-lookup"><span data-stu-id="73bc5-532">304.8</span></span>                                             |
| <span data-ttu-id="73bc5-533">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="73bc5-533">starship:commissioned</span></span> | <span data-ttu-id="73bc5-534">False</span><span class="sxs-lookup"><span data-stu-id="73bc5-534">False</span></span>                                             |
| <span data-ttu-id="73bc5-535">trademark</span><span class="sxs-lookup"><span data-stu-id="73bc5-535">trademark</span></span>             | <span data-ttu-id="73bc5-536">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="73bc5-536">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="73bc5-537">Die Beispiel-App ruft `GetSection` mit dem `starship`-Schlüssel auf.</span><span class="sxs-lookup"><span data-stu-id="73bc5-537">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="73bc5-538">Die `starship`-Schlüssel-Wert-Paaren sind isoliert.</span><span class="sxs-lookup"><span data-stu-id="73bc5-538">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="73bc5-539">Die `Bind`-Methode wird auf dem Unterabschnitt aufgerufen, der in einer Instanz der `Starship`-Klasse übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="73bc5-539">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="73bc5-540">Nach dem Binden der Instanzwerte wird die Instanz einer Eigenschaft zum Rendern zugewiesen:</span><span class="sxs-lookup"><span data-stu-id="73bc5-540">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="73bc5-541">Binden an ein Objektdiagramm</span><span class="sxs-lookup"><span data-stu-id="73bc5-541">Bind to an object graph</span></span>

<span data-ttu-id="73bc5-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> kann ein ganzes POCO-Objektdiagramm binden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="73bc5-543">Das Beispiel enthält ein `TvShow`-Modell, dessen Objektdiagramm die Klassen `Metadata` und `Actors` enthält (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="73bc5-543">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="73bc5-544">Die Beispiel-App verfügt über eine *tvshow.xml*-Datei mit den Konfigurationsdaten:</span><span class="sxs-lookup"><span data-stu-id="73bc5-544">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="73bc5-545">Die Konfiguration wird mit der `Bind`-Methode an das gesamte `TvShow`-Objektdiagramm gebunden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-545">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="73bc5-546">Die gebundene Instanz ist einer Eigenschaft zum Rendern zugewiesen:</span><span class="sxs-lookup"><span data-stu-id="73bc5-546">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="73bc5-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bindet den angegebenen Typ und gibt ihn zurück.</span><span class="sxs-lookup"><span data-stu-id="73bc5-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="73bc5-548">`Get<T>` ist praktischer als `Bind`.</span><span class="sxs-lookup"><span data-stu-id="73bc5-548">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="73bc5-549">Der folgende Code zeigt die Verwendung von `Get<T>` mit dem vorherigen Beispiel, sodass die gebundene Instanz direkt der Eigenschaft zum Rendern zugewiesen werden kann:</span><span class="sxs-lookup"><span data-stu-id="73bc5-549">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="73bc5-550">Binden eines Arrays an eine Klasse</span><span class="sxs-lookup"><span data-stu-id="73bc5-550">Bind an array to a class</span></span>

<span data-ttu-id="73bc5-551">*Die Beispiel-App veranschaulicht die in diesem Abschnitt erläuterten Konzepte.*</span><span class="sxs-lookup"><span data-stu-id="73bc5-551">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="73bc5-552"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> unterstützt das Binden von Arrays an Objekte mit Arrayindizes in Konfigurationsschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="73bc5-552">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="73bc5-553">Jedes Arrayformat, das ein numerisches Schlüsselsegment (`:0:`, `:1:`, &hellip; `:{n}:`) verfügbar macht, kann ein Array an ein POCO-Klassenarray binden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-553">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="73bc5-554">Das Binden ist standardmäßig möglich.</span><span class="sxs-lookup"><span data-stu-id="73bc5-554">Binding is provided by convention.</span></span> <span data-ttu-id="73bc5-555">Benutzerdefinierte Konfigurationsanbieter sind nicht erforderlich, um Arraybindung zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="73bc5-555">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="73bc5-556">**In-Memory-Arrayverarbeitung**</span><span class="sxs-lookup"><span data-stu-id="73bc5-556">**In-memory array processing**</span></span>

<span data-ttu-id="73bc5-557">Betrachten Sie die Konfigurationsschlüssel und -werte in der folgenden Tabelle.</span><span class="sxs-lookup"><span data-stu-id="73bc5-557">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="73bc5-558">Key</span><span class="sxs-lookup"><span data-stu-id="73bc5-558">Key</span></span>     | <span data-ttu-id="73bc5-559">Wert</span><span class="sxs-lookup"><span data-stu-id="73bc5-559">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="73bc5-560">array:0</span><span class="sxs-lookup"><span data-stu-id="73bc5-560">array:0</span></span> | <span data-ttu-id="73bc5-561">value0</span><span class="sxs-lookup"><span data-stu-id="73bc5-561">value0</span></span> |
| <span data-ttu-id="73bc5-562">array:1</span><span class="sxs-lookup"><span data-stu-id="73bc5-562">array:1</span></span> | <span data-ttu-id="73bc5-563">value1</span><span class="sxs-lookup"><span data-stu-id="73bc5-563">value1</span></span> |
| <span data-ttu-id="73bc5-564">array:2</span><span class="sxs-lookup"><span data-stu-id="73bc5-564">array:2</span></span> | <span data-ttu-id="73bc5-565">value2</span><span class="sxs-lookup"><span data-stu-id="73bc5-565">value2</span></span> |
| <span data-ttu-id="73bc5-566">array:4</span><span class="sxs-lookup"><span data-stu-id="73bc5-566">array:4</span></span> | <span data-ttu-id="73bc5-567">value4</span><span class="sxs-lookup"><span data-stu-id="73bc5-567">value4</span></span> |
| <span data-ttu-id="73bc5-568">array:5</span><span class="sxs-lookup"><span data-stu-id="73bc5-568">array:5</span></span> | <span data-ttu-id="73bc5-569">value5</span><span class="sxs-lookup"><span data-stu-id="73bc5-569">value5</span></span> |

<span data-ttu-id="73bc5-570">Diese Schlüssel und Werte werden mit dem Speicherkonfigurationsanbieter in die Beispiel-App geladen:</span><span class="sxs-lookup"><span data-stu-id="73bc5-570">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="73bc5-571">Das Array überspringt einen Wert für Index &num;3.</span><span class="sxs-lookup"><span data-stu-id="73bc5-571">The array skips a value for index &num;3.</span></span> <span data-ttu-id="73bc5-572">Der Konfigurationsbinder kann weder NULL-Werte binden noch NULL-Einträge in gebundenen Objekten erstellen. Dies wird deutlich, wenn das Ergebnis der Bindung dieses Arrays an ein Objekt demonstriert wird.</span><span class="sxs-lookup"><span data-stu-id="73bc5-572">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="73bc5-573">In der Beispiel-App ist eine POCO-Klasse verfügbar, um die gebundenen Konfigurationsdaten zu enthalten:</span><span class="sxs-lookup"><span data-stu-id="73bc5-573">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="73bc5-574">Die Konfigurationsdaten werden an das Objekt gebunden:</span><span class="sxs-lookup"><span data-stu-id="73bc5-574">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="73bc5-575">Die [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)-Syntax kann auch verwendet werden, wodurch ein kompakterer Code entsteht:</span><span class="sxs-lookup"><span data-stu-id="73bc5-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="73bc5-576">Das gebundene Objekt, eine Instanz von `ArrayExample`, empfängt die Arraydaten aus der Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="73bc5-576">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="73bc5-577">`ArrayExamples.Entries`-Index</span><span class="sxs-lookup"><span data-stu-id="73bc5-577">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="73bc5-578">`ArrayExamples.Entries`-Wert</span><span class="sxs-lookup"><span data-stu-id="73bc5-578">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="73bc5-579">0</span><span class="sxs-lookup"><span data-stu-id="73bc5-579">0</span></span>                             | <span data-ttu-id="73bc5-580">value0</span><span class="sxs-lookup"><span data-stu-id="73bc5-580">value0</span></span>                        |
| <span data-ttu-id="73bc5-581">1</span><span class="sxs-lookup"><span data-stu-id="73bc5-581">1</span></span>                             | <span data-ttu-id="73bc5-582">value1</span><span class="sxs-lookup"><span data-stu-id="73bc5-582">value1</span></span>                        |
| <span data-ttu-id="73bc5-583">2</span><span class="sxs-lookup"><span data-stu-id="73bc5-583">2</span></span>                             | <span data-ttu-id="73bc5-584">value2</span><span class="sxs-lookup"><span data-stu-id="73bc5-584">value2</span></span>                        |
| <span data-ttu-id="73bc5-585">3</span><span class="sxs-lookup"><span data-stu-id="73bc5-585">3</span></span>                             | <span data-ttu-id="73bc5-586">value4</span><span class="sxs-lookup"><span data-stu-id="73bc5-586">value4</span></span>                        |
| <span data-ttu-id="73bc5-587">4</span><span class="sxs-lookup"><span data-stu-id="73bc5-587">4</span></span>                             | <span data-ttu-id="73bc5-588">value5</span><span class="sxs-lookup"><span data-stu-id="73bc5-588">value5</span></span>                        |

<span data-ttu-id="73bc5-589">Index &num;3 im gebundenen Objekt enthält die Konfigurationsdaten für den `array:4`-Konfigurationsschlüssel und die Wert für `value4`.</span><span class="sxs-lookup"><span data-stu-id="73bc5-589">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="73bc5-590">Beim Binden von Konfigurationsdaten, die ein Array enthalten, werden die Arrayindizes in den Konfigurationsschlüsseln lediglich zum Durchlaufen der Konfigurationsdaten beim Erstellen des Objekts verwendet.</span><span class="sxs-lookup"><span data-stu-id="73bc5-590">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="73bc5-591">Ein NULL-Wert kann in den Konfigurationsdaten nicht beibehalten werden, und ein NULL-Eintrag wird nicht in einem gebundenen Objekt erstellt, wenn ein Array in Konfigurationsschlüsseln mindestens einen Index überspringt.</span><span class="sxs-lookup"><span data-stu-id="73bc5-591">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="73bc5-592">Das fehlende Konfigurationselement für Index &num;3 kann vor dem Binden an die `ArrayExamples`Instanz von jedem Konfigurationsanbieter bereitgestellt werden, der das richtige Schlüssel-Wert-Paar in der Konfiguration erzeugt.</span><span class="sxs-lookup"><span data-stu-id="73bc5-592">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="73bc5-593">Wenn das Beispiel einen zusätzlichen JSON-Konfigurationsanbieter mit dem fehlenden Schlüssel-Wert-Paar enthält, entspricht `ArrayExamples.Entries` dem kompletten Konfigurationsarray:</span><span class="sxs-lookup"><span data-stu-id="73bc5-593">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="73bc5-594">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="73bc5-594">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="73bc5-595">In `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="73bc5-595">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="73bc5-596">Im `Startup`-Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="73bc5-596">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="73bc5-597">Das Schlüssel-Wert-Paar in der Tabelle wird in die Konfiguration geladen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-597">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="73bc5-598">Key</span><span class="sxs-lookup"><span data-stu-id="73bc5-598">Key</span></span>             | <span data-ttu-id="73bc5-599">Wert</span><span class="sxs-lookup"><span data-stu-id="73bc5-599">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="73bc5-600">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="73bc5-600">array:entries:3</span></span> | <span data-ttu-id="73bc5-601">value3</span><span class="sxs-lookup"><span data-stu-id="73bc5-601">value3</span></span> |

<span data-ttu-id="73bc5-602">Wenn die `ArrayExamples`-Klasseninstanz gebunden wird, nachdem der JSON-Konfigurationsanbieter den Eintrag für Index &num;3 enthält, enthält das `ArrayExamples.Entries`-Array den Wert.</span><span class="sxs-lookup"><span data-stu-id="73bc5-602">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="73bc5-603">`ArrayExamples.Entries`-Index</span><span class="sxs-lookup"><span data-stu-id="73bc5-603">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="73bc5-604">`ArrayExamples.Entries`-Wert</span><span class="sxs-lookup"><span data-stu-id="73bc5-604">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="73bc5-605">0</span><span class="sxs-lookup"><span data-stu-id="73bc5-605">0</span></span>                             | <span data-ttu-id="73bc5-606">value0</span><span class="sxs-lookup"><span data-stu-id="73bc5-606">value0</span></span>                        |
| <span data-ttu-id="73bc5-607">1</span><span class="sxs-lookup"><span data-stu-id="73bc5-607">1</span></span>                             | <span data-ttu-id="73bc5-608">value1</span><span class="sxs-lookup"><span data-stu-id="73bc5-608">value1</span></span>                        |
| <span data-ttu-id="73bc5-609">2</span><span class="sxs-lookup"><span data-stu-id="73bc5-609">2</span></span>                             | <span data-ttu-id="73bc5-610">value2</span><span class="sxs-lookup"><span data-stu-id="73bc5-610">value2</span></span>                        |
| <span data-ttu-id="73bc5-611">3</span><span class="sxs-lookup"><span data-stu-id="73bc5-611">3</span></span>                             | <span data-ttu-id="73bc5-612">value3</span><span class="sxs-lookup"><span data-stu-id="73bc5-612">value3</span></span>                        |
| <span data-ttu-id="73bc5-613">4</span><span class="sxs-lookup"><span data-stu-id="73bc5-613">4</span></span>                             | <span data-ttu-id="73bc5-614">value4</span><span class="sxs-lookup"><span data-stu-id="73bc5-614">value4</span></span>                        |
| <span data-ttu-id="73bc5-615">5</span><span class="sxs-lookup"><span data-stu-id="73bc5-615">5</span></span>                             | <span data-ttu-id="73bc5-616">value5</span><span class="sxs-lookup"><span data-stu-id="73bc5-616">value5</span></span>                        |

<span data-ttu-id="73bc5-617">**JSON-Arrayverarbeitung**</span><span class="sxs-lookup"><span data-stu-id="73bc5-617">**JSON array processing**</span></span>

<span data-ttu-id="73bc5-618">Wenn eine JSON-Datei ein Array enthält, werden Konfigurationsschlüssel für die Arrayelemente mit einem nullbasierten Abschnittsindex erstellt.</span><span class="sxs-lookup"><span data-stu-id="73bc5-618">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="73bc5-619">In der folgenden Konfigurationsdatei ist `subsection` ein Array:</span><span class="sxs-lookup"><span data-stu-id="73bc5-619">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="73bc5-620">Der JSON-Konfigurationsanbieter liest die Konfigurationsdaten in die folgenden Schlüssel-Wert-Paare:</span><span class="sxs-lookup"><span data-stu-id="73bc5-620">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="73bc5-621">Key</span><span class="sxs-lookup"><span data-stu-id="73bc5-621">Key</span></span>                     | <span data-ttu-id="73bc5-622">Wert</span><span class="sxs-lookup"><span data-stu-id="73bc5-622">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="73bc5-623">json_array:key</span><span class="sxs-lookup"><span data-stu-id="73bc5-623">json_array:key</span></span>          | <span data-ttu-id="73bc5-624">valueA</span><span class="sxs-lookup"><span data-stu-id="73bc5-624">valueA</span></span> |
| <span data-ttu-id="73bc5-625">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="73bc5-625">json_array:subsection:0</span></span> | <span data-ttu-id="73bc5-626">valueB</span><span class="sxs-lookup"><span data-stu-id="73bc5-626">valueB</span></span> |
| <span data-ttu-id="73bc5-627">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="73bc5-627">json_array:subsection:1</span></span> | <span data-ttu-id="73bc5-628">valueC</span><span class="sxs-lookup"><span data-stu-id="73bc5-628">valueC</span></span> |
| <span data-ttu-id="73bc5-629">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="73bc5-629">json_array:subsection:2</span></span> | <span data-ttu-id="73bc5-630">valueD</span><span class="sxs-lookup"><span data-stu-id="73bc5-630">valueD</span></span> |

<span data-ttu-id="73bc5-631">In der Beispiel-App können die Konfigurations-Schlüssel-Wert-Paare mit dieser POCO-Klasse gebunden werden:</span><span class="sxs-lookup"><span data-stu-id="73bc5-631">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="73bc5-632">Nach dem Binden enthält `JsonArrayExample.Key` den Wert `valueA`.</span><span class="sxs-lookup"><span data-stu-id="73bc5-632">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="73bc5-633">Die Unterabschnittwerte werden in der POCO-Arrayeigenschaft `Subsection` gespeichert.</span><span class="sxs-lookup"><span data-stu-id="73bc5-633">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="73bc5-634">`JsonArrayExample.Subsection`-Index</span><span class="sxs-lookup"><span data-stu-id="73bc5-634">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="73bc5-635">`JsonArrayExample.Subsection`-Wert</span><span class="sxs-lookup"><span data-stu-id="73bc5-635">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="73bc5-636">0</span><span class="sxs-lookup"><span data-stu-id="73bc5-636">0</span></span>                                   | <span data-ttu-id="73bc5-637">valueB</span><span class="sxs-lookup"><span data-stu-id="73bc5-637">valueB</span></span>                              |
| <span data-ttu-id="73bc5-638">1</span><span class="sxs-lookup"><span data-stu-id="73bc5-638">1</span></span>                                   | <span data-ttu-id="73bc5-639">valueC</span><span class="sxs-lookup"><span data-stu-id="73bc5-639">valueC</span></span>                              |
| <span data-ttu-id="73bc5-640">2</span><span class="sxs-lookup"><span data-stu-id="73bc5-640">2</span></span>                                   | <span data-ttu-id="73bc5-641">valueD</span><span class="sxs-lookup"><span data-stu-id="73bc5-641">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="73bc5-642">Benutzerdefinierter Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="73bc5-642">Custom configuration provider</span></span>

<span data-ttu-id="73bc5-643">Die Beispiel-App veranschaulicht, wie ein Standardkonfigurationsanbieter erstellt wird, der Konfigurations-Schlüssel-Wert-Paare aus einer Datenbank mit [Entity Framework (EF)](/ef/core/) liest.</span><span class="sxs-lookup"><span data-stu-id="73bc5-643">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="73bc5-644">Der Anbieter weist die folgenden Merkmale auf:</span><span class="sxs-lookup"><span data-stu-id="73bc5-644">The provider has the following characteristics:</span></span>

* <span data-ttu-id="73bc5-645">Die EF-In-Memory-Datenbank wird zu Demonstrationszwecken verwendet.</span><span class="sxs-lookup"><span data-stu-id="73bc5-645">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="73bc5-646">Um eine Datenbank zu verwenden, die eine Verbindungszeichenfolge benötigt, implementieren Sie einen sekundären `ConfigurationBuilder`, um die Verbindungszeichenfolge aus einem anderen Konfigurationsanbieter anzugeben.</span><span class="sxs-lookup"><span data-stu-id="73bc5-646">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="73bc5-647">Der Anbieter liest eine Datenbanktabelle beim Start in die Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="73bc5-647">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="73bc5-648">Der Anbieter fragt die Datenbank nicht pro Schlüssel ab.</span><span class="sxs-lookup"><span data-stu-id="73bc5-648">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="73bc5-649">Das erneute Laden bei Änderung ist nicht implementiert. Das heißt, das Aktualisieren der Datenbank nach App-Start hat keine Auswirkungen auf die App-Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="73bc5-649">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="73bc5-650">Definieren Sie eine `EFConfigurationValue`-Entität zum Speichern von Konfigurationswerten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="73bc5-650">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="73bc5-651">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="73bc5-651">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="73bc5-652">Fügen Sie `EFConfigurationContext` hinzu, um die konfigurierten Werte zu speichern und auf diese zugreifen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-652">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="73bc5-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="73bc5-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="73bc5-654">Erstellen Sie eine Klasse, die das <xref:Microsoft.Extensions.Configuration.IConfigurationSource> implementiert.</span><span class="sxs-lookup"><span data-stu-id="73bc5-654">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="73bc5-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="73bc5-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="73bc5-656">Erstellen Sie den benutzerdefinierten Konfigurationsanbieter durch Vererbung von <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="73bc5-656">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="73bc5-657">Der Konfigurationsanbieter initialisiert die Datenbank, wenn diese leer ist.</span><span class="sxs-lookup"><span data-stu-id="73bc5-657">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="73bc5-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="73bc5-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="73bc5-659">Mit einer `AddEFConfiguration`-Erweiterungsmethode kann die Konfigurationsquelle `ConfigurationBuilder` hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="73bc5-659">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="73bc5-660">*EFConfigurationProvider/EFConfigurationExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="73bc5-660">*EFConfigurationProvider/EFConfigurationExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="73bc5-661">Der folgende Code veranschaulicht die Verwendung des benutzerdefinierten Anbieters `EFConfigurationProvider` in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="73bc5-661">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="73bc5-662">Zugreifen auf die Konfiguration während des Starts</span><span class="sxs-lookup"><span data-stu-id="73bc5-662">Access configuration during startup</span></span>

<span data-ttu-id="73bc5-663">Fügen Sie `IConfiguration` in den `Startup`-Konstruktor ein, um auf Konfigurationswerte in `Startup.ConfigureServices` zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="73bc5-663">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="73bc5-664">Um auf die Konfiguration in `Startup.Configure` zuzugreifen, fügen Sie entweder `IConfiguration` direkt in die Methode ein, oder verwenden Sie die Instanz aus dem Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="73bc5-664">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="73bc5-665">Ein Beispiel für den Zugriff auf die Konfiguration mit den Starthilfsmethoden finden Sie unter [Anwendungsstart: Hilfsmethoden](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="73bc5-665">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="73bc5-666">Zugreifen auf die Konfiguration auf einer Razor Pages-Seite oder in einer MVC-Ansicht</span><span class="sxs-lookup"><span data-stu-id="73bc5-666">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="73bc5-667">Um auf die Konfigurationseinstellungen auf einer Razor Pages-Seite oder in einer MVC-Ansicht zuzugreifen, fügen Sie eine [using-Anweisung](xref:mvc/views/razor#using) ([C#-Referenz: using-Anweisung](/dotnet/csharp/language-reference/keywords/using-directive)) für den [Microsoft.Extensions.Configuration-Namespace ](xref:Microsoft.Extensions.Configuration) hinzu, und fügen Sie <xref:Microsoft.Extensions.Configuration.IConfiguration> auf der Seite bzw. in der Ansicht ein.</span><span class="sxs-lookup"><span data-stu-id="73bc5-667">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="73bc5-668">Auf einer Razor Pages-Seite:</span><span class="sxs-lookup"><span data-stu-id="73bc5-668">In a Razor Pages page:</span></span>

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

<span data-ttu-id="73bc5-669">In einer MVC-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="73bc5-669">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="73bc5-670">Hinzufügen von Konfigurationen aus einer externen Assembly</span><span class="sxs-lookup"><span data-stu-id="73bc5-670">Add configuration from an external assembly</span></span>

<span data-ttu-id="73bc5-671">Eine <xref:Microsoft.AspNetCore.Hosting.IHostingStartup>-Implementierung ermöglicht das Hinzufügen von Erweiterungen zu einer App beim Start von einer externen Assembly außerhalb der `Startup`-Klasse der App aus.</span><span class="sxs-lookup"><span data-stu-id="73bc5-671">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="73bc5-672">Weitere Informationen finden Sie unter <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="73bc5-672">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73bc5-673">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="73bc5-673">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="73bc5-674">[Fundierte Einblicke in die Microsoft-Konfiguration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (in englischer Sprache)</span><span class="sxs-lookup"><span data-stu-id="73bc5-674">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
