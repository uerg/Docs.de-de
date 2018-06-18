---
title: ASP.NET Core Datenschutz
author: rick-anderson
description: In dieser Lektion erfahren Sie, das Konzept des Datenschutzes und der Entwurfsprinzipien von ASP.NET Core Data-Schutz-APIs.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/introduction
ms.openlocfilehash: 56876f43ed48b577f4d870825fd9230f526baec3
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725951"
---
# <a name="aspnet-core-data-protection"></a>ASP.NET Core Datenschutz

Webanwendungen müssen häufig sicherheitsrelevante Daten zu speichern. Windows DPAPI für desktopanwendungen bietet, aber dies ist nicht für Webanwendungen. ASP.NET Core Data Protection Stapel bieten eine einfache, einfach zu verwendende cryptographic API, die ein Entwickler zum Schützen von Daten, einschließlich der schlüsselverwaltung und Drehung verwenden kann.

ASP.NET Core Data Protection Stapel dient als der langfristige Ersatz für dienen der &lt;MachineKey&gt; Element in ASP.NET 1.x - 4.x. Es wurde für viele der Mängel bei der ALTER cryptographic Stapel und gleichzeitig eine Out-of-Box-Lösung für die meisten modernen Anwendungen auftretenden sind Anwendungsfälle behandeln ausgelegt.

## <a name="problem-statement"></a>Problemerläuterung

Das allgemeine Problem-Anweisung kann in einem einzigen Satz kurz angegeben werden: Ich benötige um vertrauenswürdige Informationen für den späteren Abruf persistent zu speichern, aber die Dauerhaftigkeit ist nicht vertrauenswürdig. Web ausgedrückt könnte dies geschrieben sein, wie "Round-Trip vertrauenswürdigen Zustand über einen nicht vertrauenswürdigen Client muss ich."

Das kanonische Beispiel hierfür ist ein Authentifizierungscookie bzw. Bearer Sicherheitstoken. Der Server generiert eine "Ich habe Groot und Xyz berechtigt" token und übergibt diesen an dem Client. Irgendwann wird der Client dieses Token an den Server vorhanden, aber der Server benötigt eine Art von Gewissheit, dass der Client das Token gefälscht wurde nicht. Daher die erste Anforderung: Echtheit (auch als) Integrität, vor unbefugtem Zugriff-Strategien).

Da der persistente Zustand vom Server als vertrauenswürdig eingestuft wird, erwarten wir, dass dieser Status möglicherweise Informationen enthalten, die für das Betriebssystem spezifisch ist. Dies kann in Form von einem Dateipfad, eine Berechtigung, ein Handle oder andere indirekten Verweis oder andere Datenfragmente serverspezifische sein. Diese Informationen sollten im Allgemeinen nicht auf ein nicht vertrauenswürdiger Client offengelegt werden. Daher die zweite Anforderung: Vertraulichkeit.

Schließlich, da moderne Anwendungen componentized sind, was wir gesehen haben ist, dass Einzelkomponenten dieses System unabhängig von der anderen Komponenten im System nutzen möchten. Für die Instanz, wenn eine Bearer-token-Komponente auf diesem Stapel verwendet wird, sollte es ohne Störung möglich sind, aus einem Anti-CSRF-Mechanismus arbeiten, die u. u. auch den gleichen Stapel verwendet werden. Daher die letzte Anforderung: Isolation.

Es können weitere Einschränkungen bereitstellen, um den Überwachungsumfang unsere Anforderungen. Es wird vorausgesetzt, dass alle Dienste, die innerhalb der Kryptografiesystem gleichermaßen vertraut sind und die Daten müssen nicht generiert oder außerhalb der Dienste unter einem unserer direkte Kontrolle genutzt werden. Darüber hinaus müssen Vorgänge sind so schnell wie möglich, da für jede Anforderung an den Webdienst die Kryptografiesystem ein- oder mehrmals durchlaufen kann. Auf diese Weise symmetrische Kryptografie ideal für unser Szenario, und wir können asymmetrische Kryptografie Rabatt, bis etwa eine Zeit, die es benötigt hat.

## <a name="design-philosophy"></a>Entwurfsphilosophie von

Wir Erkennen von Problemen mit dem vorhandenen Stapel gestartet. Sobald wir hatten, wir die Landscape vorhandenen Lösungen befragt und führten zu dem Schluss, dass keine vorhandenen Lösung sehr Funktionen zugewiesen waren, die wir gesucht. Wir Engineering klicken Sie dann eine Lösung basierend auf mehrere grundlegende Prinzipien.

* Das System sollte Vereinfachung der Konfiguration bieten. Im Idealfall würde das System Konfigurationsfreie und Entwickler konnte den Boden ausgeführt erreicht. In Situationen, in denen Entwickler ein bestimmtes Aspekts (z. B. das Key-Repository) konfigurieren müssen, sollten abgewogen werden diese bestimmten Konfigurationen einfacher zu machen.

* Bieten Sie eine einfache kundenorientierten-API. Die APIs sollte einfach der richtigen Verwendung und schwierig zu verwenden, nicht ordnungsgemäß.

* Entwickler sollten nicht schlüsselverwaltung Prinzipien lernen. Das System sollte die Auswahl des Algorithmus und Schlüssel Lebensdauer im Auftrag des Entwicklers behandeln. Im Idealfall sollte der Entwickler Zugriff auf die unformatierten Schlüsselmaterial nie sogar sein.

* Im Ruhezustand nach Möglichkeit sollten Schlüssel geschützt werden. Das System sollte eine entsprechende Standardeinstellung Schutzmechanismus herausfinden und automatisch angewendet.

Diese Grundsätze Bedenken wir eine einfache entwickelt [einfach zu verwendende](xref:security/data-protection/using-data-protection) Data Protection Stapel.

Die ASP.NET Core Datenschutz-APIs sind nicht in erster Linie für unbestimmte Persistenz des vertraulichen Nutzlasten gedacht. Andere Technologien wie [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) und [Azure Rights Management](https://docs.microsoft.com/rights-management/) eignen sich besser auf das Szenario der unbegrenzten Speicher und Verwaltungsfunktionen für entsprechend starken haben. Dies bedeutet, dass keine verbietet einen Entwickler mithilfe der ASP.NET Core Datenschutz-APIs für den langfristigen Schutz von vertraulichen Daten.

## <a name="audience"></a>Zielgruppe

Die Datenschutzsystem ist in fünf main Pakete unterteilt. Verschiedene Aspekte des diese APIs Ziel drei wichtigsten Zielgruppen;

1. Die [Consumer APIs Übersicht](xref:security/data-protection/consumer-apis/overview) Anwendung und Framework-Entwickler ausgerichtet.

   "Ich möchte nicht Weitere Informationen zur Funktionsweise von des Stapels oder darüber, wie es konfiguriert ist. Ich möchte einfach einigen Vorgängen als einfache Weise mit hoher Wahrscheinlichkeit erfolgreich mit den APIs wie möglich ausführen."

2. Die [Konfigurations-APIs](xref:security/data-protection/configuration/overview) ausgelegt, Entwickler und Systemadministratoren.

   "Ich möchte die Datenschutzsystem Teilen Sie, dass meine Umgebung Nichtstandard-Pfade oder Einstellungen erfordert."

3. Die Erweiterbarkeit APIs Ziel Entwickler um die benutzerdefinierte Richtlinie implementieren. Verwendung dieser APIs würden auf seltenen Fällen Anwendung beschränkt und Sicherheit bewusst Entwickler-geräteumleitung.

   "Ich möchte eine gesamte Komponente innerhalb des Systems zu ersetzen, da ich wirklich eindeutige verhaltensanforderungen aufweisen. Ich bin bereit, erfahren Teile uncommonly verwendet, der die API-Oberfläche, um ein Plug-in zu erstellen, die meine Anforderungen erfüllt."

## <a name="package-layout"></a>Paketlayouts

Der Data Protection Stapel besteht aus fünf Paketen.

* Microsoft.AspNetCore.DataProtection.Abstractions enthält die grundlegenden IDataProtectionProvider und IDataProtector-Schnittstellen. Es enthält auch nützliche Erweiterungsmethoden, die beim Arbeiten mit diesen Typen (z. B. Überladungen der IDataProtector.Protect) unterstützen. Finden Sie unter der Consumer Schnittstellen-Abschnitt, um weitere Informationen. Wenn eine andere Person verantwortlich ist für die Instanziierung der Datenschutzsystem, und Sie einfach die APIs nutzen, sollten Sie Verweis Microsoft.AspNetCore.DataProtection.Abstractions.

* Microsoft.AspNetCore.DataProtection enthält die basisimplementierung von Datenschutzsystem, einschließlich der wichtigsten kryptografischen Vorgänge, schlüsselverwaltung, Konfiguration und Erweiterbarkeit. Wenn Sie zum Instanziieren der Datenschutzsystem verantwortlich sind (z. B. hinzufügen, eine IServiceCollection) oder ändern, oder erweitern das Verhalten, müssen Sie Verweis Microsoft.AspNetCore.DataProtection möchten.

* Microsoft.AspNetCore.DataProtection.Extensions enthält zusätzliche APIs, die Entwickler möglicherweise interessant sein, aber die nicht in das Hauptpaket gehören. Dieses Paket enthält z. B. eine einfache "Instanziieren des Systems, zeigen Sie auf einem bestimmten-Speicherverzeichnis keine Dependency Injection-Setup"-API (Weitere Informationen). Es enthält auch die Erweiterungsmethoden für die Lebensdauer des geschützten Nutzlasten (Weitere Informationen) beschränken.

* Microsoft.AspNetCore.DataProtection.SystemWeb installiert werden kann, in eine vorhandene ASP.NET 4.x-Anwendung zum Umleiten der &lt;MachineKey&gt; Vorgänge stattdessen die neuen Data Protection Stapel verwenden. Finden Sie unter [Kompatibilität](xref:security/data-protection/compatibility/replacing-machinekey#compatibility-replacing-machinekey) für Weitere Informationen.

* Microsoft.AspNetCore.Cryptography.KeyDerivation stellt eine Implementierung von der Routine Kennworthashs PBKDF2 und kann von Systemen, bei denen Benutzerkennwörter sicher behandeln müssen verwendet werden. Finden Sie unter [Hashing von Kennwörtern](xref:security/data-protection/consumer-apis/password-hashing) für Weitere Informationen.
