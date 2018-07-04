---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET – Verweigerung des Zugriffs auf IIS-Verzeichnisse | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Whitepaper wird beschrieben, was Sie tun müssen, wenn eine Anforderung an die ASP.NET-Anwendung gibt, den Fehler "Zugriff verweigert Verzeichnisname-Verzeichnis zurück. Fehler beim s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 22868738e02472f5f433c729967ac324131a0fdc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383061"
---
<a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET – Verweigerung des Zugriffs auf IIS-Verzeichnisse
====================
> In diesem Whitepaper wird beschrieben, was Sie tun müssen, wenn eine Anforderung an Ihre ASP.NET-Anwendung den Fehler zurückgibt "verweigert den Zugriff auf *Verzeichnisname* Verzeichnis. Fehler beim Starten der Überwachung von verzeichnisänderungen."
> 
> ASP.NET 1.1 zu ASP.NET 1.0 angewendet.


ASP.NET V1 RTM lässt sich jetzt mit einem Less privilegiertes Windowskonto – als das Konto "ASPNET" auf einem lokalen Computer registriert ist.

Auf einigen Systemen gesperrt dieses Konto möglicherweise nicht standardmäßig Security Zugriff auf Inhalte einer Website-Verzeichnisse, Stammverzeichnis der Anwendung oder das Stammverzeichnis der Website gelesen. In diesem Fall erhalten Sie den folgenden Fehler beim Anfordern von Seiten in einer bestimmten Webanwendung:

![](denied-access-to-iis-directories/_static/image1.jpg)

Zum Beheben dieses Problems müssen Sie die Sicherheitsberechtigungen für die entsprechenden Verzeichnisse zu ändern.

Insbesondere ASP.NET erfordert, lesen, ausführen und den Zugriff für das ASPNET-Konto für den Websitestamm Liste (z. B.: c:\inetpub\wwwroot oder einem alternativen Standort-Verzeichnis, das Sie möglicherweise in IIS konfiguriert haben), das Inhaltsverzeichnis und Stammverzeichnis der Anwendung Um für Änderungen der Konfigurationsdateien zu überwachen. Stammverzeichnis der Anwendung entspricht der Pfad des virtuellen Verzeichnisses in das IIS-Verwaltungstool (Inetmgr) zugeordnet.

Betrachten Sie beispielsweise die folgende Anwendungshierarchie unter dem Ordner "Wwwroot".

`C:\inetpub\wwwroot\myapp\default.aspx`

In diesem Beispiel benötigt das ASPNET-Konto die Leseberechtigungen für den Inhalt in die "MyApp" und das Verzeichnis "Wwwroot" oben definierten. Eine einzelne vererbte ACL für den Stammordner kann auch optional für beide Verzeichnisse verwendet werden, wenn sie geschachtelt sind.

Um Berechtigungen in ein Verzeichnis hinzuzufügen, führen Sie die folgenden Schritte aus:

- Mit dem Windows-Explorer, navigieren Sie zum Verzeichnis
- Klicken Sie mit der rechten Maustaste auf den Ordner "Verzeichnis", und wählen Sie "Eigenschaften"
- Navigieren Sie zur Registerkarte "Sicherheit" auf das Dialogfeld "Eigenschaft"
- Klicken Sie auf die Schaltfläche "Hinzufügen", und geben Sie den Computernamen gefolgt vom Namen ASPNET-Konto. Beispielsweise würde Sie auf einem Computer mit dem Namen "Webdev" "", geben Sie Webdev\ASPNET und klicke auf "OK".
- Stellen Sie sicher, dass das ASPNET-Konto verfügt über die "Read &amp; ausführen", "Ordnerinhalt auflisten" und "Lesen" Kontrollkästchen aktiviert.
- Klicke auf OK, um das Dialogfeld zu schließen und speichern Sie die Änderungen.

![](denied-access-to-iis-directories/_static/image2.jpg)

Falls gewünscht, können diese Änderungen mithilfe von Skripts oder dem Tool "cacls.exe", das bereitgestellt wird, mit Windows automatisiert werden. Weitere Informationen über das ASPNET-Konto, finden Sie unter den [FAQ-Dokument](https://go.microsoft.com/fwlink/?LinkId=5828).

Wenn eine bestimmte Webanwendung basiert auf schreiben, oder Ändern von Berechtigungen für die Datei oder eines bestimmten Ordners, kann dies durch die gleichen Schritte aus, und überprüfen die Kontrollkästchen für "Write" und/oder "Ändern" erteilt werden.

Auf Computern, die alle Benutzer oder den Benutzern Gruppe den Zugriff auf diese Verzeichnisse (was die Standardkonfiguration ist) ermöglichen, werden keine Probleme auftreten, und die oben genannten Schritte nicht erforderlich sein.
