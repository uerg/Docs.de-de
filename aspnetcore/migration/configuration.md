---
title: Migrieren der Konfiguration zu ASP.NET Core
author: ardalis
description: Informationen Sie zum Migrieren der Konfiguration eines ASP.NET MVC-Projekts zu ASP.NET Core MVC-Projekt.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: 5bb89401ac54b54810fe5724b293ae8ed7e5afef
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="migrate-configuration-to-aspnet-core"></a>Migrieren der Konfiguration zu ASP.NET Core

Von [Steve Smith](https://ardalis.com/) und [Scott Addie](https://scottaddie.com)

In der vorherigen Artikel wir begann [migrieren ein ASP.NET MVC-Projekts zu ASP.NET Core MVC](mvc.md). In diesem Artikel wir die Konfiguration migrieren.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Setup-Konfiguration

ASP.NET Core wird nicht mehr verwendet, die *"Global.asax"* und *"Web.config"* Dateien, die frühere Versionen von ASP.NET verwendet. In früheren Versionen von ASP.NET Startlogik für die Anwendung im positioniert wurde ein `Application_StartUp` Methode innerhalb *"Global.asax"*. Später werden Sie in ASP.NET MVC eine *Startup.cs* Datei im Stammverzeichnis des Projekts; enthalten war, und es wurde aufgerufen, wenn die Anwendung gestartet. ASP.NET Core hat dieser Ansatz vollständig von übernommen platzieren Startlogik für alle in der *Startup.cs* Datei.

Die *"Web.config"* Datei auch in ASP.NET Core ersetzt wurde. Konfiguration selbst kann nun konfiguriert werden, und als Teil der Schritte beim Starten einer Anwendung in beschriebenen *Startup.cs*. Konfiguration kann XML-Dateien weiterhin nutzen, sondern in der Regel ASP.NET Core Projekte werden platzieren Konfigurationswerte in einer JSON-formatierten Datei wie z. B. *appsettings.json*. ASP.NET-Core-Konfigurationssystem erreichen Umgebungsvariablen, die bereitstellen, können auch einfach einen [mehr sicherer und robuster Speicherort](xref:security/app-secrets) für umgebungsspezifische Werte. Dies ist besonders für geheime Schlüssel, wie z. B. Verbindungszeichenfolgen und API-Schlüssel, die in die quellcodeverwaltung eingecheckt werden, darf nicht "true". Finden Sie unter [Konfiguration](xref:fundamentals/configuration/index) Weitere Informationen zur Konfiguration in ASP.NET Core.

Für diesen Artikel beginnen wir mit dem teilweise migriert ASP.NET Core-Projekt aus [vorherigen Artikel](mvc.md). Zum Einrichten des Konfiguration fügen die folgenden Konstruktor und die Eigenschaft, um die *Startup.cs* Datei befindet sich im Stammverzeichnis des Projekts:

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

Beachten Sie, dass an diesem Punkt der *Startup.cs* Datei nicht kompiliert werden, da wir benötigen Sie Folgendes hinzufügen `using` Anweisung:

```csharp
using Microsoft.Extensions.Configuration;
```

Hinzufügen einer *appsettings.json* Datei im Stammverzeichnis des Projekts mit der entsprechenden Elementvorlage:

![Hinzufügen von "appSettings" JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Migrieren von Konfigurationseinstellungen aus der Datei "Web.config"

Unsere ASP.NET MVC-Projekt enthielt, die erforderliche Datenbank-Verbindungszeichenfolge in *"Web.config"*in die `<connectionStrings>` Element. In unserem Projekt ASP.NET Core Kegel zum Speichern dieser Informationen in den *appsettings.json* Datei. Open *appsettings.json*, und beachten Sie, dass sie bereits die Folgendes umfasst:

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


In der markierten Zeile in der oben dargestellten, ändern Sie den Namen der Datenbank von **_CHANGE_ME** auf den Namen der Datenbank.

## <a name="summary"></a>Zusammenfassung

ASP.NET Core setzt alle Startlogik für die Anwendung in eine einzelne Datei, in der die erforderlichen Dienste und Abhängigkeiten definiert und konfiguriert werden können. Er ersetzt die *"Web.config"* Datei in eine flexible Konfiguration-Funktion, die eine Vielzahl von Dateiformaten wie JSON sowie Umgebungsvariablen nutzen können.
