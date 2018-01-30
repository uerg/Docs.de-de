---
title: "IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core"
author: shirhatti
description: "Erhalten Sie Unterstützung für das Debuggen von ASP.NET Core-Anwendungen, wenn sie hinter IIS unter Windows Server ausgeführt werden."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a5f727dd21ac0c6702691df2215c42f4adc0ec27
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>IIS-Support zum Zeitpunkt der Entwicklung in Visual Studio für ASP.NET Core

Von [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Dieser Artikel beschreibt die Unterstützung für [Visual Studio](https://www.visualstudio.com/vs/) für das Debuggen von ASP.NET Core-Anwendungen, die hinter IIS unter Windows Server ausgeführt werden. Dieses Thema führt Sie durch die Aktivierung dieser Funktion und Einrichten eines Projekts.

## <a name="prerequisites"></a>Erforderliche Komponenten

* Visual Studio (2017/Version 15.3 oder höher)
* ASP.NET und die Workload für die Webentwicklung *ODER* die plattformübergreifende Workload für die .NET Core-Entwicklung

## <a name="enable-iis"></a>Aktivieren von IIS

Aktivieren Sie IIS. Navigieren Sie zu **Systemsteuerung** > **Programme** > **Programme und Features** > **Windows-Features aktivieren oder deaktivieren** (links auf dem Bildschirm). Aktivieren Sie das Kontrollkästchen **Internetinformationsdienste**.

![Windows-Features, die das aktivierte Kontrollkästchen der Internetinformationsdienste als schwarzes Quadrat anzeigt (nicht mit einem Häkchen), was angibt, dass einige der IIS-Features aktiviert sind](development-time-iis-support/_static/enable_iis.png)

Wenn die IIS-Installation einen Neustart erforderlich ist, starten Sie das System neu.

## <a name="enable-development-time-iis-support"></a>Aktivieren der Entwicklungszeit-IIS-Unterstützung

Starten Sie Visual Studio-Installer, um die vorhandene Visual Studio-Installation zu ändern, nach der Installation von IIS. Wählen Sie im Installer die Komponente **Entwicklungszeit-IIS-Unterstützung** aus. Die Komponente ist als optionale Komponente im Bereich **Zusammenfassung** für die Workload **ASP.NET und Webentwicklung** aufgeführt. Dadurch wird das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) installiert, was ein natives IIS-Modul ist, das zur Ausführung von ASP.NET Core-Anwendungen erforderlich ist.

![Ändern von Visual Studio-Features: Die Registerkarte „Workloads“ ist ausgewählt. Im Bereich „Web und Cloud“ ist der Bereich „ASP.NET und Webentwicklung“ ausgewählt. Auf rechts im Bereich "Optional" der Bereiche "Zusammenfassung" befindet sich ein Kontrollkästchen für die Entwicklungszeit, IIS zu unterstützen.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Konfigurieren des Projekts

Erstellen Sie ein neues Startprofil, um die Entwicklungszeit-IIS-Unterstützung hinzuzufügen. Klicken Sie im **Projektmappen-Explorer** von Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Eigenschaften** aus. Klicken Sie auf die Registerkarte **Debuggen**. Wählen Sie **IIS** aus dem Dropdownfeld **Start** aus. Bestätigen Sie, dass die Funktion **Launch browser** (Browser starten) mit der richtigen URL aktiviert ist.

![Fenster „Projekteigenschaften“, in dem die Registerkarte „Debuggen“ ausgewählt ist Die Profil- und Starteinstellungen werden auf IIS festgelegt. Die Funktion „Browser starten“ wird mit der Adresse http://localhost/WebApplication2 aktiviert. Die selbe Adresse wird auch im Feld „App-URL“ des Bereichs für Webservereinstellungen bereitgestellt, und „Anonyme Authentifizierung aktivieren“ ist aktiviert.](development-time-iis-support/_static/project_properties.png)

Auch manuell hinzufügen ein Profils starten, das die [launchSettings.json](http://json.schemastore.org/launchsettings) Datei in der app:

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

Visual Studio möglicherweise einen Neustart aufgefordert, wenn Sie nicht als Administrator ausführen. Wenn Sie dazu aufgefordert werden, starten Sie Visual Studio neu.

Herzlichen Glückwunsch! An diesem Punkt ist das Projekt für IIS Entwicklungszeit Unterstützung konfiguriert. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Hosten von ASP.NET Core unter Windows mit IIS](xref:host-and-deploy/iis/index)
* [Einführung in das ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module)
* [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module)
