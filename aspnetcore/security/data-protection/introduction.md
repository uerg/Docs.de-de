---
title: ASP.NET Core-Datenschutz
author: rick-anderson
description: Erfahren Sie mehr über das Konzept des Schutzes von Daten und den Entwurfsprinzipien von ASP.NET Core Datenschutz-APIs.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/introduction
ms.openlocfilehash: 29a2bbef6f2fd9b61541173af143926ca82bfad7
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095694"
---
# <a name="aspnet-core-data-protection"></a>ASP.NET Core-Datenschutz

Webanwendungen müssen häufig zum Speichern von vertraulichen Daten. Windows bietet DPAPI für desktop-Anwendungen, aber dies ist möglich, ungeeignet für Webanwendungen. ASP.NET Core-Stapel für den Schutz der Daten bieten eine einfache, benutzerfreundliche kryptographische API, die ein Entwickler zum Schutz von Daten, einschließlich der schlüsselverwaltung und-Rotation verwenden kann.

ASP.NET Core-Stapel für den Schutz der Daten als langfristige Ersatz für dienen soll die &lt;MachineKey&gt; Element in ASP.NET 1.x - 4.x. Es wurde speziell auf viele die Nachteile von den alten kryptografischen Stapel und gleichzeitig eine Out-of-the-Box-Lösung für die meisten Anwendungsfälle, die moderne Anwendungen wahrscheinlich konfrontiert werden.

## <a name="problem-statement"></a>Problembeschreibung

Die allgemeine Problem-Anweisung kann in einem einzigen Satz kurz angegeben werden: vertrauenswürdigen Informationen für den späteren Abruf beibehalten werden muss, aber ich nicht den Dauerhaftigkeitsmechanismus vertrauen. Web ausgedrückt kann dies geschrieben werden, als "Ich Round-Trip vertrauenswürdigen Status über ein nicht vertrauenswürdiger Client muss."

Das kanonische Beispiel hierfür ist ein Authentifizierungscookie oder bearertoken token. Der Server generiert eine "Ich bin Groot und Xyz berechtigt" token, und übergibt diesen an dem Client. Irgendwann wird der Client dieses Token an den Server angezeigt, aber der Server benötigt eine Art von Gewissheit, dass der Client das Token gefälschten noch nicht. Daher die erste Anforderung: Echtheitszertifikat (auch als) Integrität, vor unbefugtem Zugriff sichern).

Da der beibehaltene Zustand vom Server als vertrauenswürdig eingestuft wird, erwarten wir, dass es sich bei diesem Status Informationen enthalten kann, die spezifisch für die betriebsumgebung ist. Dies kann in Form von einen Dateipfad, eine Berechtigung, ein Handle oder andere indirekten Verweis oder einem anderen Ort von Server-spezifische Daten sein. Solche Informationen sollten in der Regel nicht zu einem nicht vertrauenswürdigen Client offengelegt werden. Daher die zweite Anforderung: Vertraulichkeit.

Da moderne Anwendungen in Komponenten gegliedert sind, ist wie wir gesehen haben, dass die einzelne Komponenten dieses System unabhängig von anderen Komponenten im System nutzen möchten. Z. B. wenn eine Bearer-token-Komponente dieser Stapel verwendet wird, sollte es ohne Störungen durch eine Anti-CSRF-Methode ausgeführt werden, die auch denselben Stapel verwendet werden kann. Daher die letzte Voraussetzung: Isolation.

Wir können weitere Einschränkungen bereitstellen, um die Anforderungen einzugrenzen. Es wird davon ausgegangen, dass alle Dienste innerhalb der Kryptografiesystem gleichermaßen vertrauenswürdig sind und die Daten müssen nicht generiert oder außerhalb der Dienste in unserem Einflussbereich direkt genutzt werden. Darüber hinaus benötigen wir, dass Vorgänge sind so schnell wie möglich, da jede Anforderung an den Webdienst der Kryptografiesystem einmal oder mehrmals durchlaufen kann. Dadurch symmetrische Kryptografie ideal für unser Szenario, und wir können asymmetrischen Kryptografie Rabatt, bis z. B. eine Zeit, die er benötigt wird.

## <a name="design-philosophy"></a>Entwurfsphilosophie

Da fingen wir durch Erkennen von Problemen mit dem vorhandenen Stapel. Sobald wir, die hatten, werden wir Umfrage unter Einhaltung von vorhandenen Lösungen und hat ergeben, dass keine vorhandene Lösung ganz die Funktionen verfügt, die wir gesucht. Wir konzipiert, klicken Sie dann eine Lösung basierend auf einige grundlegende Prinzipien.

* Das System sollte die Vereinfachung der Konfiguration bieten. Im Idealfall wird das System wäre ohne Konfigurationsaufwand, und Entwickler können sofort loslegen. In Situationen, in denen Entwickler so konfigurieren Sie einen bestimmten Aspekt (z. B. das Key-Repository müssen) sollte berücksichtigt werden, diese bestimmten Konfigurationen einfach darauf.

* Bieten Sie eine einfache Endverbraucher-API. Die APIs sollte für die ordnungsgemäße Verwendung einfach und schwierig, falsch zu verwenden.

* Entwickler sollten nicht schlüsselverwaltung Prinzipien lernen. Das System sollte Auswahl des Algorithmus und die Gültigkeitsdauer des Entwicklers im Auftrag verarbeiten können. Im Idealfall sollte der Entwickler nie Zugriff auf das unformatierte Schlüsselmaterial haben.

* Schlüssel sollten im Ruhezustand, wenn möglich geschützt werden. Das System sollte eine entsprechende Standardeinstellung Schutzmechanismus herausfinden und automatisch angewendet.

Mit diesen Prinzipien Beachten Sie, die wir entwickelt einer einfaches, [benutzerfreundliche](xref:security/data-protection/using-data-protection) der Stapel zum Schutz von Daten.

Die ASP.NET Core die Datenschutz-APIs sind in erster Linie für unbestimmte Persistenz vertrauliche Nutzlasten nicht vorgesehen. Andere Technologien wie [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) und [Azure Rights Management](https://docs.microsoft.com/rights-management/) eignen sich besser für das Szenario unbegrenzten Speicher, und dementsprechend sichere schlüsselverwaltung Funktionen haben. Allerdings nichts verbietet Entwickler mithilfe der ASP.NET Core die Datenschutz-APIs für den langfristigen Schutz von vertraulichen Daten.

## <a name="audience"></a>Zielgruppe

Das System zum Schutz von Daten ist in fünf wichtigsten Pakete aufgeteilt. Verschiedene Aspekte dieser APIs Zielgruppen drei wichtigsten;

1. Die [Übersicht über Consumer-APIs](xref:security/data-protection/consumer-apis/overview) Anwendungs- und Framework-Entwickler ausgerichtet.

   "Ich möchte nicht erfahren Sie mehr über die Funktionsweise des Stapels oder darüber, wie es konfiguriert ist. Ich möchte einfach einigen Vorgängen so einfach wie möglich eine Weise mit hoher Wahrscheinlichkeit erfolgreich mithilfe der APIs ausführen."

2. Die [Konfigurations-APIs](xref:security/data-protection/configuration/overview) Anwendungsentwickler und Systemadministratoren als Ziel festlegen.

   "Ich möchte System zum Schutz von Daten zu informieren, dass meine Umgebung Nichtstandard-Pfade oder Einstellungen erfordert."

3. Die Erweiterbarkeit APIs Ziel Entwickler verantwortlich für die benutzerdefinierte Richtlinie implementieren. Verwendung dieser APIs würde auf seltenen Fällen beschränkt und aufgetreten ist, handelt es sich bei Sicherheit bewusst Entwickler.

   "Ich möchte eine gesamte Komponente innerhalb des Systems zu ersetzen, da ich rundum einzigartigen verhaltensanforderungen verfügen. Ich bin bereit, um zu erfahren ungewöhnlicherweise verwendete Teile der API-Oberfläche, um ein Plug-in zu erstellen, die meine Anforderungen erfüllt."

## <a name="package-layout"></a>Paketlayout

Stapel für den Schutz von Daten besteht aus fünf Paketen.

* Microsoft.AspNetCore.DataProtection.Abstractions enthält die grundlegenden IDataProtectionProvider und IDataProtector-Schnittstellen. Sie enthält auch nützliche Erweiterungsmethoden, die beim Arbeiten mit diesen Typen (z. B. Überladungen der IDataProtector.Protect) unterstützen. Finden Sie im Abschnitt Schnittstellen Consumer, Weitere Informationen. Wenn eine andere Person verantwortlich ist für das System zum Schutz von Daten zu instanziieren und Sie werden einfach die APIs nutzen, sollten Sie sich auf Microsoft.AspNetCore.DataProtection.Abstractions-Verweis.

* Microsoft.AspNetCore.DataProtection enthält die Core-Implementierung des Data Protection Systems, einschließlich Core kryptografischen Vorgänge, schlüsselverwaltung, Konfiguration und Erweiterbarkeit. Wenn Sie für das Instanziieren das System zum Schutz der Daten verantwortlich sind (z. B., das sie eine "iservicecollection") oder ändern, oder erweitern das Verhalten, sollten Sie Verweis Microsoft.AspNetCore.DataProtection.

* Microsoft.AspNetCore.DataProtection.Extensions enthält zusätzliche APIs die Entwicklern können hilfreich sein, aber nicht die im Core-Paket gehören. Dieses Paket enthält z. B. eine einfache "instanziiert das System, zeigen Sie auf einem angegebenen Schlüsselspeicheranbieter-Verzeichnis ohne Installation des Dependency Injection"-API (Weitere Informationen). Es enthält auch Erweiterungsmethoden zum Beschränken der Lebensdauer von geschützten Payloads (Weitere Informationen).

* Microsoft.AspNetCore.DataProtection.SystemWeb installiert werden kann, in eine vorhandene ASP.NET 4.x-Anwendung zum Umleiten der &lt;MachineKey&gt; Vorgänge zu verwenden. der Stapel für den neuen Daten-Schutz. Finden Sie unter [Kompatibilität](xref:security/data-protection/compatibility/replacing-machinekey#compatibility-replacing-machinekey) für Weitere Informationen.

* Microsoft.AspNetCore.Cryptography.KeyDerivation stellt eine Implementierung der Routine für PBKDF2 Kennworthashs und kann von Systemen, bei denen Benutzerkennwörter auf sichere Weise behandeln müssen verwendet werden. Finden Sie unter [Kennwörter zu hashen](xref:security/data-protection/consumer-apis/password-hashing) für Weitere Informationen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

<xref:host-and-deploy/web-farm>
