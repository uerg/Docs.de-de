---
uid: whitepapers/ms03-32-issue
title: Fehlerbehebung für "Serveranwendung nicht verfügbar" Fehler nach dem Anwenden von Sicherheitsupdates für IE | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Artikel wird beschrieben, den Patch an, der ein Problem mit dem MS03-32-Sicherheitsupdate für Internet Explorer behoben werden, die ASP.NET 1.0-Anwendungen, die unter WLAN wirkt sich auf...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: ''
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: 186ed7ea7ad9b317506fb3951a974682b44b27a1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402469"
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Korrektur für "Server nicht verfügbar" Anwendungsfehler, nach dem Anwenden von Sicherheitsupdates für IE
====================
> In diesem Artikel wird beschrieben, den Patch an, der ein Problem mit dem MS03-32-Sicherheitsupdate für Internet Explorer behoben werden, die ASP.NET 1.0-Anwendungen, die auf Windows XP Professional ausgeführt wird, wirkt sich auf.
> 
> Gilt für ASP.NET 1.0 und Windows XP Professional.


Microsoft ermittelt ein Problem mit der MS03-32-Sicherheitsupdate für Internet Explorer-Sicherheitspatch und ASP.NET 1.0 unter Windows XP. Dieser Patch kann manuell oder durch Abrufen der aktuellen wichtigen Updates über die Windows Update-Website installiert werden.

Das Symptom dieses Problems ist, dass nach der Installation des Patches auf einem Windows XP-Computer, alle Anforderungen für ASP.NET-Anwendungen auf dem lokalen Webserver von IIS 5.1 ausgeführt wird eine Fehlermeldung angezeigt "Server-Anwendung nicht verfügbar" führen. Anforderungen an remote-Web-Server sind nicht betroffen.

Dieses Problem wirkt sich nur Installationen, die Ausführung von ASP.NET 1.0 unter Windows XP. Es wirkt sich nicht auf Computern, auf denen Windows 2000 oder Windows Server 2003 ausgeführt wird. Computer mit Windows XP mit ASP.NET 1.1 installiert hat es auch keine Auswirkungen.

Beachten Sie, dass dieses Problem **nicht** eines Sicherheitsfehlers mit ASP.NET. Es **nicht** öffnen oder böswilligen Angriffen gegen einer ASP.NET-Anwendung oder eines Servers zu ermöglichen. Stattdessen ist es sich um rein funktionale aufgrund eines Fehlers durch den Patch selbst.

Wir arbeiten hart an eine dauerhafte Lösung für dieses Problem. In der Zwischenzeit können Sie die folgende Batchdatei als problemumgehung für das Problem ausführen. Die Batchdatei führt Folgendes aus:

1. Beendet die Status-Dienste von IIS und ASP.NET
2. Löscht und neu erstellt das ASPNET-Konto mit einem bekannten temporäre Kennwort
3. Verwendet die Windows `runas` Befehl aus, um eine ausführbare Datei starten, die ein Profil der ASPNET-Benutzer erstellt.
4. Erneutes Registrieren von ASP.NET. Dies erstellt ein neues zufälliges Kennwort für das Konto, und ASP.NET Standardeinstellungen der Zugriffssteuerung dafür gilt
5. Die IIS-Dienst wird neu gestartet

Die Batchdatei enthält ein hartcodierte temporäre Kennwort "<strong>1pass@word</strong>" aufgefordert wird, geben für das ausführende Befehl, wenn die Batchdatei ausgeführt wird. Nachdem der Befehl "Runas" abgeschlossen ist, wird das Kennwort für das ASPNET-Konto mit einem starken, zufälligen Wert neu erstellt. Beachten Sie, dass die Batchdatei fehlschlagen, wenn das Kennwort hartcodiert die Anforderungen an die Kennwortkomplexität in Ihrer Umgebung nicht geeignet ist. Wenn dies der Fall ist, können Sie es in einen anderen Wert ändern, die für Ihre Umgebung geeignet ist.

*> [!IMPORTANT]* Wenn Sie benutzerdefinierte Einstellungen für die Zugriffssteuerung oder Datenbank-Kontoberechtigungen für das ASPNET-Konto hinzugefügt haben, müssen sie neu erstellt werden, nachdem diese Batchdatei abgeschlossen ist. Das liegt, wenn das Konto neu erstellt wird, eine neue Sicherheits-ID (SID) abgerufen werden sollen.

*> [!IMPORTANT]* Wenn Sie mit einem benutzerdefinierten Konto als das ASPNET-Konto ASP.NET-Arbeitsprozess ausführen, sollten Sie nicht diese Batchdatei ausführen. Stattdessen sollten Sie melden Sie sich interaktiv oder verwenden den Befehl "Runas" mit dem Konto ein Benutzerprofils für das Konto erstellt wird.

Die Batchdatei ist im folgenden selbstextrahierendes Archiv enthalten. So verwenden Sie es:

1. Sie müssen mit einem Konto mit Administratorrechten ausgeführt werden
2. [Herunterladen Sie und öffnen Sie die selbstextrahierende ausführbare Datei](ms03-32-issue/_static/fixup1.exe)
3. Extrahieren Sie den Inhalt nach c:\
4. Wählen Sie im Startmenü ausführen..., und geben Sie `cmd.exe`
5. Geben Sie den Befehl "Öffnen" Windows `c:\fixup.cmd`.
6. Wenn Sie dazu aufgefordert werden, geben Sie <strong>1pass@word</strong> als Kennwort.
7. Wenn Sie vorher benutzerdefinierte Einstellungen für die Zugriffssteuerung oder Datenbank-Kontoberechtigungen für das ASPNET-Konto haben, müssen Sie diese Einstellungen jetzt erneut anwenden.

Viele möchten uns bei Ihnen für die Unannehmlichkeiten, die ausgelöst hat. Wir werden zusätzliche Informationen veröffentlichen, sobald diese verfügbar werden.

Die folgende Matrix wird erläutert, Plattformen und Versionen, die von diesem Problem betroffen sind.

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

Nochmals vielen Dank,   
 Das ASP.NET-Team
