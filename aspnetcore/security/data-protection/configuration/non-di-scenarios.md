---
title: "Beachten Sie nicht-DI-Szenarien für den Datenschutz in ASP.NET Core"
author: rick-anderson
description: "Erfahren Sie, wie Sie Data Protection Szenarien zu unterstützen, bzw. können keinen von Abhängigkeitsinjektion bereitgestellten Dienst verwenden möchten."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 1c84cfcf44086359a7d6900ca52781dc6f3b1b10
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>Beachten Sie nicht-DI-Szenarien für den Datenschutz in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Das ASP.NET Core-Datenschutz-System ist normalerweise [hinzugefügt, um einen Dienstcontainer](xref:security/data-protection/consumer-apis/overview) und abhängige Komponenten über abhängigkeiteneinschleusung (DI) genutzt. Es gibt jedoch Fälle, in denen dies ist nicht möglich oder gewünscht ist, insbesondere, wenn das System in einer vorhandenen app zu importieren.

Um diese Szenarien zu unterstützen die [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) Paket bietet einen konkreten Typ [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), welcher bietet eine einfache Möglichkeit zum Schutz von Daten verwenden ohne Rückgriff auf DI. Die `DataProtectionProvider` Typ implementiert [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). Erstellen von `DataProtectionProvider` erfordert nur die Bereitstellung einer [DirectoryInfo](/dotnet/api/system.io.directoryinfo) Instanz, um anzugeben, wo die Anbieter-Kryptografieschlüssel gespeichert werden sollen, wie im folgenden Codebeispiel dargestellt:

[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]

Wird standardmäßig die `DataProtectionProvider` konkreter Typ nicht unformatierten Schlüsselmaterial verschlüsseln sie zuvor im Dateisystem beibehalten. Dies ist zur Unterstützung von Szenarien, in denen die Entwickler Punkte mit einem freigegebenen Netzwerkordner und die Datenschutz-System automatisch einen entsprechenden-ruhender Verschlüsselungsmechanismus hergeleitet werden können.

Darüber hinaus die `DataProtectionProvider` konkreter Typ nicht [Isolieren von apps](xref:security/data-protection/configuration/overview#per-application-isolation) standardmäßig. Alle apps, die mit dem gleichen Schlüssel Verzeichnis können genauso lang wie die Nutzlasten Freigeben ihrer [Zweck Parameter](xref:security/data-protection/consumer-apis/purpose-strings) übereinstimmen.

Die [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) Konstruktor akzeptiert einen optionale Konfiguration-Rückruf, der verwendet werden kann, um das Verhalten des Systems anzupassen. Im Beispiel unten zeigt wiederherstellen Isolation ein expliziter Aufruf von [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). Das Beispiel zeigt auch das Konfigurieren des Systems automatisch permanente Schlüsseln mithilfe von Windows DPAPI verschlüsselt. Wenn das Verzeichnis auf einer UNC-Freigabe verweist, werden Sie ggf. zur Verteilung von einem freigegebenen Zertifikat für alle relevanten Computer sowie zum Konfigurieren des Systems zum Verwenden der zertifikatbasierten Verschlüsselung mit einem Aufruf von [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Instanzen der `DataProtectionProvider` konkreter Typ sind aufwendig zu erstellen. Wenn eine Anwendung mehrere Instanzen dieses Typs verwaltet und sie alle den gleichen Schlüssel Speicherverzeichnis verwenden, beeinträchtigt möglicherweise die Leistung der app. Bei Verwendung der `DataProtectionProvider` Typ, es wird empfohlen, dass Sie dieses Typs einmal erstellen und verwenden Sie ihn so weit wie möglich wieder. Die `DataProtectionProvider` Typ und alle [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) Instanzen erstellt wurden, sind threadsicher für mehrere Aufrufer.
