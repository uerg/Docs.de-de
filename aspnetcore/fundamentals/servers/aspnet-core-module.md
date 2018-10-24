---
title: ASP.NET Core-Modul
author: rick-anderson
description: Erfahren Sie, wie der Kestrel-Webserver durch das ASP.NET Core-Modul IIS oder IIS Express als Reverseproxyserver zu verwenden.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 2f73a34b7d311c9e98ad2ecba11894d27bb2aa4d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910888"
---
# <a name="aspnet-core-module"></a>ASP.NET Core-Modul

Von [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) und [Chris Ross](https://github.com/Tratcher)

::: moniker range=">= aspnetcore-2.2"

Mit dem ASP.NET Core-Modul können ASP.NET Core-Apps in einem IIS-Arbeitsprozess (*In-Process*) oder hinter IIS in einer Reverseproxy-Konfiguration (*Out-of-Process*) ausgeführt werden. IIS bietet mehr Sicherheit für Web-Apps und Features für die Verwaltbarkeit.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Mit dem ASP.NET Core-Modul können ASP.NET Core-Apps hinter IIS in einer Reverseproxy-Konfiguration ausgeführt werden. IIS bietet mehr Sicherheit für Web-Apps und Features für die Verwaltbarkeit.

::: moniker-end

Unterstützte Windows-Versionen:

* Windows 7 oder höher
* Windows Server 2008 R2 oder höher

::: moniker range=">= aspnetcore-2.2"

Beim In-Process-Hosting verfügt das Modul über eine eigene Serverimplementierung, `IISHttpServer`.

Beim Out-of-Process-Hosting funktioniert das Modul nur mit Kestrel. Das Modul ist nicht kompatibel mit [HTTP.sys](xref:fundamentals/servers/httpsys) (ehemals [WebListener](xref:fundamentals/servers/weblistener)).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Das Modul funktioniert nur mit Kestrel. Das Modul ist nicht kompatibel mit [HTTP.sys](xref:fundamentals/servers/httpsys) (ehemals [WebListener](xref:fundamentals/servers/weblistener)).

::: moniker-end

## <a name="aspnet-core-module-description"></a>Beschreibung des ASP.NET Core-Moduls

::: moniker range=">= aspnetcore-2.2"

Das ASP.NET Core-Modul ist ein natives IIS-Modul, das in die IIS-Pipeline integriert wird, um eine dieser Aktionen auszuführen:

* Hosten einer ASP.NET Core-App innerhalb des IIS-Arbeitsprozesses (`w3wp.exe`), was als [In-Process-Hostingmodell](#in-process-hosting-model) bezeichnet wird.

* Weiterleiten von Webanforderungen an eine Back-End-ASP.NET Core-App, die den [Kestrel-Server](xref:fundamentals/servers/kestrel) ausführt, was als [Out-of-Process-Hostingmodell](#out-of-process-hosting-model) bezeichnet wird.

### <a name="in-process-hosting-model"></a>In-Process-Hostingmodell

Beim Einsatz von In-Process-Hosting wird eine ASP.NET Core-App im gleichen Prozess wie ihr IIS-Arbeitsprozess ausgeführt. Dadurch wird die Leistungseinbuße der Proxyumleitung von Anforderungen über den Loopbackadapter vermieden, die das Out-of-Process-Hostingmodell mit sich bringt. IIS erledigt das Prozessmanagement mit dem [Windows-Prozessaktivierungsdienst (Process Activation Service, WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Das ASP.NET Core-Modul:

* Führt die Initialisierung von Apps aus.
  * Lädt die [CoreCLR](/dotnet/standard/glossary#coreclr).
  * Ruft `Program.Main`.
* Behandelt die Lebensdauer der nativen IIS-Anforderung.

Das folgende Diagramm zeigt die Beziehung zwischen IIS, dem ASP.NET Core-Modul und einer In-Process gehosteten App:

![ASP.NET Core-Modul](aspnet-core-module/_static/ancm-inprocess.png)

Eine Anforderung geht aus dem Web beim HTTP.sys-Treiber im Kernelmodus ein. Der Treiber leitet die native Anforderung an IIS auf dem konfigurierten Port der Webseite weiter, normalerweise 80 (HTTP) oder 443 (HTTPS). Das Modul empfängt die native Anforderung und übergibt die Steuerung an `IISHttpServer`, wodurch die Anforderung aus einer nativen in eine verwaltete Anforderung überführt wird.

Nachdem `IISHttpServer` die Anforderung erhalten hat, wird die Anforderung in die Middleware-Pipeline von ASP.NET Core eingestellt. Die Middleware-Pipeline behandelt die Anforderung und gibt sie als `HttpContext`-Instanz an die App-Logik weiter. Die Antwort der App wird dann an IIS zurückgegeben, wo sie per Push an den HTTP-Client zurückgegeben wird, der die Anforderung initiiert hat.

### <a name="out-of-process-hosting-model"></a>Out-of-Process-Hostingmodell

Da ASP.NET Core-Apps in einem Prozess getrennt vom IIS-Arbeitsprozess ausgeführt werden, führt das Modul die Prozessverwaltung durch. Das Modul startet den Prozess für die ASP.NET Core-App, wenn die erste Anforderung eingeht und startet die App neu, wenn sie heruntergefahren wird oder abstürzt. Dies ist im Prinzip das gleiche Verhalten wie bei Apps, die prozessintern ausgeführt und durch den [Windows-Prozessaktivierungsdienst (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) verwaltet werden.

Das folgende Diagramm zeigt die Beziehung zwischen IIS, dem ASP.NET Core-Modul und einer Out-of-Process gehosteten App:

![ASP.NET Core-Modul](aspnet-core-module/_static/ancm-outofprocess.png)

Anforderungen gehen aus dem Internet an den Treiber „HTTP.sys“ ein, der im Kernelmodus betrieben wird. Der Treiber leitet die Anforderungen an IIS auf dem konfigurierten Port der Webseite weiter, normalerweise 80 (HTTP) oder 443 (HTTPS). Das Modul leitet die Anforderung an Kestrel auf einem zufälligen Port der App weiter, der nicht Port 80 oder 443 entspricht.

Das Modul gibt den Port über die Umgebungsvariable beim Start an. Die Middleware für die Integration von IIS konfiguriert den Server so, dass er auf `http://localhost:{port}` lauscht. Zusätzliche Überprüfungen werden durchgeführt. Anforderungen, die nicht vom Modul stammen, werden abgelehnt. Das Modul unterstützt die HTTPS-Weiterleitung nicht. Deshalb werden Anforderungen über HTTP weitergeleitet, selbst wenn sie von IIS über HTTPS empfangen wurden.

Nachdem Kestrel die Anforderung vom Modul erhalten hat, wird die Anforderung in die Middleware-Pipeline von ASP.NET Core eingestellt. Die Middleware-Pipeline behandelt die Anforderung und gibt sie als `HttpContext`-Instanz an die App-Logik weiter. Die durch IIS-Integration hinzugefügte Middleware aktualisiert das Schema, die Remote-IP und die Pfadbasis, um der Weiterleitung der Anforderung an Kestrel Rechnung zu tragen. Die Antwort der App wird dann an IIS zurückgegeben, wo sie per Push an den HTTP-Client zurückgegeben wird, der die Anforderung initiiert hat.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Das ASP.NET Core-Modul ist ein natives IIS-Modul, das in die IIS-Pipeline integriert wird, um Webanforderungen an Back-End-ASP.NET Core-Apps weiterzuleiten.

Da ASP.NET Core-Apps in einem Prozess getrennt vom IIS-Arbeitsprozess ausgeführt werden, führt das Modul auch Prozessverwaltung durch. Das Modul startet den Prozess für die ASP.NET Core-App, wenn die erste Anforderung eingeht und startet die App neu, wenn sie abstürzt. Dies ist im Prinzip das gleiche Verhalten wie bei ASP.NET 4.x-Apps, die prozessintern in IIS ausgeführt und durch [Windows Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) verwaltet werden.

Das folgende Diagramm zeigt die Beziehung zwischen IIS, dem ASP.NET Core-Modul und einer App:

![ASP.NET Core-Modul](aspnet-core-module/_static/ancm-outofprocess.png)

Anforderungen gehen aus dem Internet an den Treiber „HTTP.sys“ ein, der im Kernelmodus betrieben wird. Der Treiber leitet die Anforderungen an IIS auf dem konfigurierten Port der Webseite weiter, normalerweise 80 (HTTP) oder 443 (HTTPS). Das Modul leitet die Anforderung an Kestrel auf einem zufälligen Port der App weiter, der nicht Port 80 oder 443 entspricht.

Das Modul gibt den Port über die Umgebungsvariable beim Start an. Die Middleware für die Integration von IIS konfiguriert den Server so, dass er auf `http://localhost:{port}` lauscht. Zusätzliche Überprüfungen werden durchgeführt. Anforderungen, die nicht vom Modul stammen, werden abgelehnt. Das Modul unterstützt die HTTPS-Weiterleitung nicht. Deshalb werden Anforderungen über HTTP weitergeleitet, selbst wenn sie von IIS über HTTPS empfangen wurden.

Nachdem Kestrel die Anforderung vom Modul erhalten hat, wird die Anforderung in die Middleware-Pipeline von ASP.NET Core eingestellt. Die Middleware-Pipeline behandelt die Anforderung und gibt sie als `HttpContext`-Instanz an die App-Logik weiter. Die durch IIS-Integration hinzugefügte Middleware aktualisiert das Schema, die Remote-IP und die Pfadbasis, um der Weiterleitung der Anforderung an Kestrel Rechnung zu tragen. Die Antwort der App wird dann an IIS zurückgegeben, wo sie per Push an den HTTP-Client zurückgegeben wird, der die Anforderung initiiert hat.

::: moniker-end

Viele native Module, z.B. die Windows-Authentifizierung, bleiben aktiv. Weitere Informationen zu IIS-Modulen, die im Modul aktiv sind, finden Sie unter <xref:host-and-deploy/iis/modules>.

Das ASP.NET Core-Modul hat noch andere Funktionen. Das Modul kann:

* Umgebungsvariablen für den Arbeitsprozess festlegen
* Die stdout-Ausgabe im Dateispeicher protokollieren, um Probleme beim Start zu beheben
* Windows-Authentifizierungstoken weiterleiten

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>So installieren und verwenden Sie das ASP.NET Core-Modul

Detaillierte Anweisungen zum Installieren und Verwenden des ASP.NET Core-Moduls finden Sie unter <xref:host-and-deploy/iis/index>. Informationen zum Konfigurieren des Moduls finden Sie unter <xref:host-and-deploy/aspnet-core-module>.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [ASP.NET Core Module GitHub repository (source code) (ASP.NET Core-Modul GitHub-Repository (Quellcode))](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
