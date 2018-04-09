---
uid: whitepapers/ms03-32-issue
title: Fehlerbehebung für "Serveranwendung nicht verfügbar" Fehler nach dem Anwenden der Sicherheitsupdate für Internet Explorer | Microsoft Docs
author: rick-anderson
description: Dieses Whitepaper beschreibt den Patch, der ein Problem mit dem MS03-32-Sicherheitsupdate für Internet Explorer behebt, die ASP.NET 1.0-Anwendungen unter Wi wirkt sich auf...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: dd1a564cd347364abc3ca5ac0a9ffda448bcede8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Update für "Serveranwendung nicht verfügbar" Fehler nach dem Anwenden der Sicherheitsupdate für Internet Explorer
====================
> Dieses Whitepaper beschreibt den Patch, der ein Problem mit dem MS03-32-Sicherheitsupdate für Internet Explorer behebt, die ASP.NET 1.0-Anwendungen unter Windows XP Professional wirkt sich auf.
> 
> Gilt für ASP.NET 1.0 und Windows XP Professional.


Microsoft identifiziert ein Problem mit der MS03-32-Sicherheitsupdate für Internet Explorer-Sicherheitspatch und ASP.NET 1.0 auf Windows XP ausgeführt wird. Dieses Update kann manuell oder durch Abrufen der neuesten kritischen Updates von Windows Update-Website installiert werden.

Die Anzeichen für dieses Problem ist, dass nach der Installation des Patches auf einem Windows XP-Computer alle Anforderungen für ASP.NET-Anwendungen auf dem lokalen Webserver von IIS 5.1 ausgeführt wird eine Fehlermeldung angezeigt "Server-Anwendung nicht verfügbar" führen. Anforderungen für remote-Web-Server sind nicht betroffen.

Dieses Problem wirkt sich nur auf Installationen unter Windows XP auf ASP.NET 1.0 aus. Er wirkt sich nicht auf Computern unter Windows 2000 oder Windows Server 2003. Computer, auf denen Windows XP mit ASP.NET 1.1 installiert hat es auch keine Auswirkungen.

Bitte beachten Sie, dass dieses Problem **nicht** Sicherheitsfehler mit ASP.NET. Es **nicht** öffnen, oder lassen Sie alle böswillige Angriffe auf einer ASP.NET-Anwendung oder eines Servers. Stattdessen wird ausschließlich ein funktionaler Fehler zurückzuführen sind, den Patch selbst.

Wir arbeiten hart auf eine dauerhafte Lösung für dieses Problem. In der Zwischenzeit können Sie die folgende Batchdatei als problemumgehung für das Problem ausführen. Die Batchdatei führt Folgendes aus:

1. Beendet die Status-Dienste von IIS und ASP.NET
2. Löscht und neu erstellt das ASPNET-Konto mit einem bekannten temporäre Kennwort
3. Verwendet die Windows `runas` Befehl aus, um eine ausführbare Datei starten, die einem ASPNET-Benutzerprofil erstellt.
4. Erneut registriert ASP.NET. Dadurch wird ein neues zufälliges Kennwort für das Konto erstellt und gilt standardmäßig ASP.NET zugriffssteuerungseinstellungen dafür
5. Startet den IIS-Dienst neu

Die Batchdatei enthält ein hartcodierte temporäre Kennwort "<strong>1pass@word</strong>" wird u. aufgefordert, geben für das Runas Befehl, wenn die Batchdatei ausgeführt wird. Nachdem der Befehl "Runas" abgeschlossen wurde, wird das Kennwort für das ASPNET-Konto mit einem starken, zufälligen Wert neu erstellt. Beachten Sie, dass die Batchdatei fehlschlagen, wenn das hartcodiert-Kennwort nicht die kennwortanforderungen für die Komplexität in Ihrer Umgebung erfüllt. Wenn dies der Fall ist, können Sie es in einen anderen Wert ändern, die für Ihre Umgebung geeignet ist.

*> [!IMPORTANT]* Wenn Sie benutzerdefinierte zugriffssteuerungseinstellungen oder Datenbank Kontoberechtigungen für das ASPNET-Konto hinzugefügt haben, müssen sie neu erstellt werden, nachdem diese Batchdatei abgeschlossen ist. Dies liegt daran, wenn das Konto neu erstellt wird, eine neue Sicherheits-ID (SID) abgerufen werden sollen.

*> [!IMPORTANT]* Wenn Sie den ASP.NET-Arbeitsprozess mit einem benutzerdefinierten Konto als das ASPNET-Konto ausgeführt werden, sollten Sie diese Batchdatei nicht ausführen. Stattdessen sollten Sie interaktiv anzumelden oder verwenden den Befehl "Runas" mit dem Konto, das ein Benutzerprofil für dieses Konto erstellt werden.

Die Batchdatei ist im folgenden selbstextrahierende Archiv enthalten. Sie verwenden:

1. Sie müssen als ein Konto mit Administratorrechten ausführen
2. [Herunter, und öffnen Sie die selbstextrahierende ausführbare Datei](ms03-32-issue/_static/fixup1.exe)
3. Extrahieren Sie den Inhalt c:\
4. Wählen Sie über das Startmenü ausführen..., und geben Sie `cmd.exe`
5. Geben Sie in den geöffneten Befehlsfenstern `c:\fixup.cmd`.
6. Wenn Sie dazu aufgefordert werden, geben Sie <strong>1pass@word</strong> als Kennwort ein.
7. Wenn Sie zuvor benutzerdefinierte zugriffssteuerungseinstellungen oder Datenbank Kontoberechtigungen für das ASPNET-Konto haben, müssen Sie diese Einstellungen nun erneut anwenden.

Viele möchten uns bei Ihnen für die Unannehmlichkeiten, die dies verursacht hat. Wir werden zusätzliche Informationen bereitstellen, sobald sie verfügbar sind.

Die Matrix unter Details Plattformen und Versionen, die von diesem Problem betroffen.

| .NET Framework | Plattform | Betroffen |
| --- | --- | --- |
| Version 1.0 | Windows 2000 Professional | Nein |
| Version 1.0 | Windows 2000 Server | Nein |
| Version 1.0 | Windows XP Professional | Ja |
| Version 1.0 | Windows Server 2003 | Nein |
| Version 1.0 | Windows XP Home mit Cassini | Nein |
| Version 1.1 | Windows 2000 Professional | Nein |
| Version 1.1 | Windows 2000 Server | Nein |
| Version 1.1 | Windows XP Professional | Nein |
| Version 1.1 | Windows Server 2003 | Nein |
| Version 1.1 | Windows XP Home mit Cassini | Nein |

Vielen Dank,   
 Das Team ASP.NET
