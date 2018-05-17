---
title: ASP.NET Core-Modul
author: tdykstra
description: Erfahren Sie, wie der Kestrel-Webserver durch das ASP.NET Core-Modul IIS oder IIS Express als Reverseproxyserver zu verwenden.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4e842544f861c3704ba7798fa693b36435d54731
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
---
# <a name="aspnet-core-module"></a>ASP.NET Core-Modul

Von [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) und [Chris Ross](https://github.com/Tratcher) 

Mit dem ASP.NET Core-Modul können ASP.NET Core-Apps hinter IIS in einer Reverseproxy-Konfiguration ausgeführt werden. IIS bietet mehr Sicherheit für Web-Apps und Features für die Verwaltbarkeit.

Unterstützte Windows-Versionen:

* Windows 7 oder höher
* Windows Server 2008 R2 oder höher

Das ASP.NET Core-Modul funktioniert nur mit Kestrel. Das Modul ist nicht kompatibel mit [HTTP.sys](xref:fundamentals/servers/httpsys) (ehemals [WebListener](xref:fundamentals/servers/weblistener)).

## <a name="aspnet-core-module-description"></a>Beschreibung des ASP.NET Core-Moduls

Das ASP.NET Core-Modul ist ein natives IIS-Modul, das in die IIS-Pipeline integriert wird, um Webanforderungen an Back-End-ASP.NET Core-Apps umzuleiten. Viele native Module, z.B. die Windows-Authentifizierung, bleiben aktiv. Weitere Informationen zu IIS-Modulen, die im Modul aktiv sind, finden Sie unter [IIS modules (IIS-Module)](xref:host-and-deploy/iis/modules).

Da ASP.NET Core-Apps in einem Prozess getrennt vom IIS-Arbeitsprozess ausgeführt werden, führt das Modul auch Prozessverwaltung durch. Das Modul startet den Prozess für die ASP.NET Core-App, wenn die erste Anforderung eingeht und startet die App neu, wenn sie abstürzt. Dies ist im Prinzip das gleiche Verhalten wie bei ASP.NET 4.x-Apps, die prozessintern in IIS ausgeführt und durch [Windows Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) verwaltet werden.

Das folgende Diagramm zeigt die Beziehung zwischen IIS, dem ASP.NET Core-Modul und ASP.NET Core-Apps:

![ASP.NET Core-Modul](aspnet-core-module/_static/ancm.png)

Anforderungen gehen aus dem Internet an den Treiber „HTTP.sys“ ein, der im Kernelmodus betrieben wird. Der Treiber leitet die Anforderungen an IIS auf dem konfigurierten Port der Webseite weiter, normalerweise 80 (HTTP) oder 443 (HTTPS). Das Modul leitet die Anforderung an Kestrel auf einem zufälligen Port der App weiter, der nicht Port 80 oder 443 entspricht.

Das Modul gibt den Port über die Umgebungsvariable beim Start an. Die Middleware für die Integration von IIS konfiguriert den Server so, dass er auf `http://localhost:{port}` lauscht. Zusätzliche Überprüfungen werden durchgeführt. Anforderungen, die nicht vom Modul stammen, werden abgelehnt. Das Modul unterstützt die HTTPS-Weiterleitung nicht. Deshalb werden Anforderungen über HTTP weitergeleitet, selbst wenn sie von IIS über HTTPS empfangen wurden.

Nachdem Kestrel eine Anforderung vom Modul erhalten hat, wird die Anforderung in die Middleware-Pipeline von ASP.NET Core aufgenommen. Die Middleware-Pipeline behandelt die Anforderung und gibt sie als `HttpContext`-Instanz an die App-Logik weiter. Die Antwort der App wird dann an IIS zurückgegeben, wo sie per Push an den HTTP-Client zurückgegeben wird, der die Anforderung initiiert hat.

Das ASP.NET Core-Modul hat noch andere Funktionen. Das Modul kann:

* Umgebungsvariablen für den Arbeitsprozess festlegen
* Die stdout-Ausgabe im Dateispeicher protokollieren, um Probleme beim Start zu beheben
* Windows-Authentifizierungstoken weiterleiten

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>So installieren und verwenden Sie das ASP.NET Core-Modul

Detaillierte Anweisungen zum Installieren und Verwenden des ASP.NET Core-Moduls finden Sie unter [Hosten von ASP.NET Core unter Windows mit IIS](xref:host-and-deploy/iis/index). Informationen zum Konfigurieren des Moduls finden Sie unter [ASP.NET Core Module configuration reference (Konfigurationsreferenz für das ASP.NET Core-Modul)](xref:host-and-deploy/aspnet-core-module).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Hosten unter Windows mit IIS](xref:host-and-deploy/iis/index)
* [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module)
* [ASP.NET Core Module GitHub repository (source code) (ASP.NET Core-Modul GitHub-Repository (Quellcode))](https://github.com/aspnet/AspNetCoreModule)
