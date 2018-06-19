---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET IIS-Verzeichnisse Zugriff verweigert | Microsoft Docs
author: rick-anderson
description: In diesem Whitepaper wird beschrieben, was Sie tun müssen, wenn eine Anforderung an die ASP.NET-Anwendung, der Fehler "Zugriff verweigert DirectoryName Verzeichnis zurückgegeben. Fehler beim s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: d95423776a6b58fc67ae6c791685543dadd2480c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
ms.locfileid: "30070770"
---
<a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET IIS-Verzeichnisse der Zugriff verweigert
====================
> In diesem Whitepaper wird beschrieben, was Sie tun müssen, wenn eine Anforderung an die ASP.NET-Anwendung, den Fehler zurückgegeben "Zugriff auf verweigert *DirectoryName* Verzeichnis. Fehler beim Starten der Überwachung von verzeichnisänderungen."
> 
> Gilt für ASP.NET 1.0 und ASP.NET 1.1.


ASP.NET V1 RTM jetzt wird ausgeführt, die eine weniger privilegierte Windowskonto – als das Konto "ASPNET" auf einem lokalen Computer registriert.

Auf einige Systeme gesperrt dieses Konto möglicherweise nicht standardmäßig Zugriff auf eine Website Verzeichnisse, das Stammverzeichnis der Anwendung oder das Stammverzeichnis der Website gelesen. In diesem Fall erhalten Sie die folgende Fehlermeldung beim Anfordern von Seiten aus einer bestimmten Webanwendung:

![](denied-access-to-iis-directories/_static/image1.jpg)

Um dieses Problem zu beheben, müssen Sie die Sicherheitsberechtigungen für die entsprechenden Verzeichnisse zu ändern.

Insbesondere ASP.NET erfordert, lesen, ausführen und Listenzugriff für das ASPNET-Konto für das Stammverzeichnis der Website (z. B.: c:\inetpub\wwwroot oder einem alternativen Standort-Verzeichnis, die Sie möglicherweise in IIS konfiguriert haben), das Inhaltsverzeichnis und Stammverzeichnis der Anwendung Um bei Änderungen der Konfigurationsdateien überwacht werden. Das Stammverzeichnis der Anwendung entspricht der Ordnerpfad, der das virtuelle Verzeichnis der Anwendung in der IIS-Verwaltungstool (Inetmgr) zugeordnet.

Betrachten Sie beispielsweise die folgende Anwendungshierarchie unter dem Ordner "Wwwroot" aus.

`C:\inetpub\wwwroot\myapp\default.aspx`

In diesem Beispiel benötigt das ASPNET-Konto der Lese-und Schreibberechtigungen für Inhalte in der "MyApp" und das Verzeichnis "Wwwroot" oben definiert. Eine einzelne vererbte ACL im Stammordner kann auch optional für beide Verzeichnisse verwendet werden, wenn sie geschachtelt sind.

Um Berechtigungen in ein Verzeichnis hinzuzufügen, führen Sie die folgenden Schritte aus:

- Mit dem Windows-Explorer, navigieren Sie zum Verzeichnis
- Klicken Sie mit der rechten Maustaste auf den Ordner, und wählen Sie "Eigenschaften"
- Navigieren Sie zur Registerkarte "Sicherheit" auf das Dialogfeld "Eigenschaft"
- Klicken Sie auf die Schaltfläche "Hinzufügen", und geben Sie den Computernamen gefolgt vom Namen ASPNET-Konto. Auf einem Computer mit dem Namen "Webdev" "", würden Sie z. B. Webdev\ASPNET eingeben und "OK" erreicht.
- Stellen Sie sicher, dass das ASPNET-Konto verfügt über das "Read &amp; ausführen", "Ordnerinhalt auflisten" und "Lesen" Kontrollkästchen aktiviert.
- Drücken Sie OK, um das Dialogfeld zu schließen und speichern Sie die Änderungen.

![](denied-access-to-iis-directories/_static/image2.jpg)

Falls gewünscht, können diese Änderungen mithilfe von Skripts oder "cacls.exe"-Tool, das geliefert wird mit Windows automatisiert werden. Weitere Informationen über das ASPNET-Konto, finden Sie unter der [FAQ Dokument](https://go.microsoft.com/fwlink/?LinkId=5828).

Wenn eine bestimmte Webanwendung davon abhängig, ob Schreibzugriff oder Ändern von Berechtigungen für einen bestimmten Ordner oder eine Datei, kann dies durch das gleiche Verfahren, und überprüfen die Kontrollkästchen für "Write" und/oder "Ändern" erteilt werden.

Auf Computern, die es ermöglichen, "Jeder" oder die Benutzergruppe lesen Zugriff auf diese Verzeichnisse (Dies ist die Standardkonfiguration), keine Probleme auftreten, und ist die oben genannten Schritte nicht erforderlich.
