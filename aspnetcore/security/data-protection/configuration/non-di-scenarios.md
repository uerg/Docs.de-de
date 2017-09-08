---
title: Beachten Sie nicht DI-Szenarien
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a7d8a962-80ff-48e3-96f6-8472b7ba2df9
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 54a930c26f9f48ea0e6f7865e2927bcde0f4d6c0
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="non-di-aware-scenarios"></a>Beachten Sie nicht DI-Szenarien

Die Datenschutzsystem dient normalerweise [einen Dienstcontainer hinzuzufügende](../consumer-apis/overview.md) und abhängige Komponenten über eine DI-Mechanismus zur Verfügung gestellt wird. Möglicherweise gibt es jedoch einige Fälle, in denen dies nicht möglich, ist insbesondere dann, wenn das System in eine vorhandene Anwendung importieren.

Zur Unterstützung dieser Szenarien enthält das Paket Microsoft.AspNetCore.DataProtection.Extensions einen konkreten Typ DataProtectionProvider die bietet eine einfache Möglichkeit, das Data Protection-System zu verwenden, ohne DI-spezifische Codepfade durchlaufen. Der Typ selbst implementiert IDataProtectionProvider und erstellen es ist so einfach wie das Bereitstellen einer DirectoryInfo, in denen dieser Anbieter kryptografischen Schlüssel gespeichert werden sollen.

Zum Beispiel:

[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]

>[!WARNING]
> Standardmäßig die DataProtectionProvider konkreter Typ nicht verschlüsselt unformatierten Schlüsselmaterial vor dem Speichern im Dateisystem. Dies ist zur Unterstützung von Szenarien, in denen die Developer-verweist auf ein Netzwerk freigeben, in diesem Fall nicht der Datenschutzsystem automatisch einen entsprechenden-ruhender Verschlüsselungsmechanismus ableiten.
>
>Darüber hinaus den konkrete Typ DataProtectionProvider nicht [Anwendungen isolieren](overview.md#data-protection-configuration-per-app-isolation) standardmäßig können alle Anwendungen, auf dem gleichen Schlüssel Verzeichnis verwiesen wird Nutzlasten freigeben, solange ihre Zweck-Parameter übereinstimmen.

Der Anwendungsentwickler kann beide adressieren, falls gewünscht. Der DataProtectionProvider-Konstruktor akzeptiert ein [optionale Konfiguration Rückruf](overview.md#data-protection-configuration-callback) die verwendet werden können, um das Verhalten des Systems zu optimieren. Im folgenden Beispiel wird veranschaulicht, Wiederherstellen von Isolierung über ein expliziter Aufruf von SetApplicationName, und es veranschaulicht auch die Konfiguration des Systems, um persistente Schlüssel mithilfe von Windows DPAPI automatisch zu verschlüsseln. Wenn das Verzeichnis auf einer UNC-Freigabe verweist, Sie möchten möglicherweise eine freigegebene Zertifikat für alle relevanten Computer zu verteilen und zum Konfigurieren des Systems zum Verwenden der zertifikatbasierten Verschlüsselung stattdessen über einen Aufruf an [ProtectKeysWithCertificate](overview.md#configuring-x509-certificate).

[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]

>[!TIP]
> Instanzen von den konkreten Typ DataProtectionProvider sind aufwendig zu erstellen. Wenn eine Anwendung mehrere Instanzen dieses Typs verwaltet und sie alle auf den gleichen Schlüssel Speicherverzeichnis zeigt sind, kann die Anwendungsleistung beeinträchtigt. Die beabsichtigte Verwendung ist, dass Entwickler der Anwendung dieses Typs einmal instanziiert und Wiederverwenden von dieser einzelnen Verweis so weit wie möglich beibehalten. Der DataProtectionProvider-Typ und alle IDataProtector-Instanzen, die erstellt wurden, sind threadsicher für mehrere Aufrufer.
