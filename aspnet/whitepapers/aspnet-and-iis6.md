---
uid: whitepapers/aspnet-and-iis6
title: Ausführen von ASP.NET 1.1 mit IIS 6.0 | Microsoft Docs
author: rick-anderson
description: Während Windows Server 2003 mit IIS 6.0 und ASP.NET 1.1 enthalten, sind diese Komponenten standardmäßig deaktiviert. Dieses Whitepaper enthält Informationen zum Aktivieren von IIS 6.0 ein...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530109"
---
<a name="running-aspnet-11-with-iis-60"></a>Ausführen von ASP.NET 1.1 mit IIS 6.0
====================
> Während Windows Server 2003 mit IIS 6.0 und ASP.NET 1.1 enthalten, sind diese Komponenten standardmäßig deaktiviert. Dieses Whitepaper enthält Informationen zum Aktivieren von IIS 6.0 und ASP.NET 1.1 und empfiehlt mehrere Konfigurationseinstellungen, die eine optimale Leistung von IIS und ASP.NET zu erhalten.
> 
> Gilt für ASP.NET 1.1 und IIS 6.0.


Im Lieferumfang von ASP.NET 1.1 ist Windows Server 2003, die auch die neueste Version von Internet Information Server (IIS) Version 6.0 umfasst. IIS 6.0 und ASP.NET 1.1 sind nahtlos integriert und ASP.NET wird jetzt standardmäßig auf das neue IIS 6.0-Worker-Prozessmodell.

## <a name="aspnet-11-is-not-installed-by-default"></a>ASP.NET 1.1 ist standardmäßig nicht installiert.

Im Gegensatz zu früheren Versionen von Microsoft Server-Betriebssystemen ist Internet Information Server (IIS) nicht standardmäßig aktiviert. noch wird von ASP.NET 1.1. Es gibt zwei Optionen zum Aktivieren von IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Aktivieren von IIS, Option #1 - Serverkonfigurations-Assistent

Windows Server 2003 bereitgestellt wird, eine neue "Serverkonfigurations-Assistent" können Sie den Server in den gewünschten Modus ordnungsgemäß zu konfigurieren.

Beachten Sie, dass zum Ausführen des Assistenten Sie als Administrator - angemeldet werden müssen zum Starten des Assistenten - wechseln Sie zu: Start | Programme | Verwaltung, und wählen Sie "Konfigurieren des Servers".

Nach dem Auswählen der Bildschirm "Serverkonfigurations-Assistent" Öffnen sollte angezeigt werden:

![](aspnet-and-iis6/_static/image1.jpg)

Klicken Sie auf "Weiter &gt;":

![](aspnet-and-iis6/_static/image2.jpg)

Klicken Sie auf "Weiter &gt;"

![](aspnet-and-iis6/_static/image3.jpg)

Auf diesem Bildschirm müssen, wählen Sie ' Anwendungsserver (IIS, ASP.NET) als die Optionen zum Konfigurieren.

Klicken Sie auf "Weiter &gt;".

![](aspnet-and-iis6/_static/image4.jpg)

Nach der Auswahl, um den Server als Anwendungsserver konfigurieren, wird dieser Bildschirm angezeigt werden aufgefordert, zusätzlichen Funktionen installiert werden soll. Keine Option ist standardmäßig aktiviert. Um ASP.NET automatisch zu aktivieren, müssen Sie auswählen ' ASP aktivieren. NET ".

Klicken Sie auf "Weiter &gt;".

![](aspnet-and-iis6/_static/image5.jpg)

Dieser Bildschirm wird angezeigt, welche Optionen sind, installiert werden.

Klicken Sie auf "Weiter &gt;".

![](aspnet-and-iis6/_static/image6.jpg)

Dieser Bildschirm wird angezeigt, während die ausgewählten Optionen installiert werden. Es ist normal, andere Dialogfeld angezeigt, die Felder angezeigt, wie Dienste installiert werden. Sie können außerdem für den Speicherort der Windows 2003 Server-Installations-CD aufgefordert.

Klicken Sie auf "Weiter &gt;" nach dem Abschluss.

![](aspnet-and-iis6/_static/image7.jpg)

Klicken Sie auf "Fertig stellen": der Windows Server 2003 ist zur Unterstützung von IIS 6.0 und ASP.NET 1.1 jetzt konfiguriert.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Aktivieren von IIS, Option #2: Manuelles Konfigurieren von IIS und ASP.NET

Wenn Sie nicht, verwenden Sie die "Serverkonfigurations-Assistent möchten" können Sie optional die IIS 6.0 und ASP.NET 1.1 verwenden "Programme hinzufügen oder entfernen" in der Systemsteuerung installieren.

Öffnen Sie zunächst die Systemsteuerung:

![](aspnet-and-iis6/_static/image8.jpg)

Klicken Sie dann auf "Hinzufügen/entfernen Windows Components" Öffnen den Assistenten zum Komponenten von Windows auf:

![](aspnet-and-iis6/_static/image9.jpg)

Markieren Sie, und überprüfen Sie 'Anwendungsserver', und klicken Sie dann auf 'Details'? Schaltfläche:

![](aspnet-and-iis6/_static/image10.jpg)

Überprüfen Sie zum Installieren von ASP.NET ' ASP. NET ".

Klicken Sie auf "OK", um zum Assistenten für Windows-Komponenten zurückgegeben. Klicken Sie auf "Weiter &gt;" klicken Sie im Assistenten für Windows-Komponente zu Beginn der Installation:

![](aspnet-and-iis6/_static/image11.jpg)

Es ist normal, andere Dialogfeld angezeigt, die Felder angezeigt, wie Dienste installiert werden. Sie können außerdem für den Speicherort der Windows 2003 Server-Installations-CD aufgefordert.

Wenn die Installation abgeschlossen ist, sehen Sie der letzten Seite des Assistenten-Komponente für Windows:

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6.0 und ASP.NET 1.1 sind nun konfiguriert und verfügbar.

## <a name="recommended-settings"></a>Empfohlene Einstellungen

Beim Ausführen von ASP.NET 1.1 mit IIS 6.0 stehen mehrere Konfigurationseinstellungen, die empfohlen werden, um die optimale Leistung von ASP.NET zu erhalten:

- Konfigurieren von Worker-Prozess-Arbeitsspeichergrenze
- Konfigurieren von Arbeitsprozessen

### <a name="configuring-worker-process-memory-limits"></a>Konfigurieren von Worker-Prozess-Arbeitsspeichergrenze

Standardmäßig wird IIS 6.0 keine die Menge an Arbeitsspeicher begrenzen, die IIS verwenden darf. ASP IST. NET Cache-Funktion basiert auf eine Einschränkung des Arbeitsspeichers, damit der Cache proaktiv nicht verwendete Elemente aus dem Arbeitsspeicher entfernen kann.

Es wird empfohlen, dass Sie die Wiederverwendung von IIS 6.0 Funktion Arbeitsspeicher konfigurieren. So konfigurieren Sie diese öffnen Internetinformationsdienste-Manager (Start | Programme | Verwaltung | Internet Information Services). Sobald geöffnet ist, erweitern Sie den Ordner "Anwendungspools":

Für jeden Anwendungspool:

![](aspnet-and-iis6/_static/image13.jpg)

1. Mit der rechten Maustaste auf den Anwendungspool, z. B. "DefaultAppPool", und wählen Sie "Eigenschaften":

![](aspnet-and-iis6/_static/image14.jpg)

2. Aktivieren Sie als Nächstes Arbeitsspeicher wiederverwenden, indem Sie entweder auf "maximal verbrauchter Speicher (in MB):". Der Wert darf nicht größer als die Menge des Arbeitsspeichers (nicht virtuell) auf dem Server sein, eine gute Näherung 60 % des physischen Arbeitsspeichers, d. h. für einen Server mit 512MB physischen Arbeitsspeicher Select 310. Es wird außerdem empfohlen, dass die maximale 800 MB nicht überschreiten, wenn Sie einen Adressraum von 2 GB verwenden. Wenn der Speicherbereich für die Adresse des Servers/3 GB ist, kann die Höchstwert des Arbeitsspeichers für den Arbeitsprozess so hoch wie 1 800 MB sein:

![](aspnet-and-iis6/_static/image15.jpg)

Klicken Sie auf "Übernehmen" und "OK", um das Eigenschaftendialogfeld zu beenden. Wiederholen Sie diesen Schritt für alle Anwendungspools verfügbar.

### <a name="configuring-worker-recycling"></a>Konfigurieren von Worker-Wiederverwendung

Standardmäßig wird IIS 6.0 konfiguriert, um einen Arbeitsprozess alle 29 Stunden durchzuführen. Dies ist etwas aggressiver für eine Anwendung, die ASP.NET ausführt, und es wird empfohlen, dass automatische Arbeitsprozessen deaktiviert ist.

Öffnen Sie zum Deaktivieren der automatischen Arbeitsprozessen zuerst Internetinformationsdienste-Manager (Start | Programme | Verwaltung | Internet Information Services). Sobald geöffnet ist, erweitern Sie den Ordner "Anwendungspools":

![](aspnet-and-iis6/_static/image16.jpg)

Für jeden Anwendungspool:

1. Mit der rechten Maustaste auf den Anwendungspool, z. B. "DefaultAppPool", und wählen Sie "Eigenschaften":

![](aspnet-and-iis6/_static/image17.jpg)

2. Deaktivieren Sie "Workerprozess wiederverwenden (in Minuten):":

![](aspnet-and-iis6/_static/image18.jpg)

Klicken Sie auf "Übernehmen" und "OK", um das Eigenschaftendialogfeld zu beenden. Wiederholen Sie diesen Schritt für alle Anwendungspools verfügbar.

## <a name="granting-write-access-to-the-file-system"></a>Erteilen Schreibzugriff auf das Dateisystem

Wenn Ihre Anwendung Schreibzugriff auf das Dateisystem benötigt und Sie NTFS verwenden, müssen Sie ein Zugriff Zugriffssteuerungsliste (ACL) auf den Ordner oder eine Datei zum Gewähren des Zugriffs von ASP.NET zu ändern.

Beispielsweise ASP.NET gewähren Schreibzugriff auf die c:\inetpub\wwwroot zunächst öffnen Sie Explorer und navigieren Sie zu dem Verzeichnis:

![](aspnet-and-iis6/_static/image19.jpg)

Als Nächstes mit der rechten Maustaste auf das Verzeichnis, z. B. "Wwwroot" und wählen Sie Eigenschaften aus. Nachdem das Dialogfeld geöffnet wurde, wählen Sie die Registerkarte "Sicherheit":

![](aspnet-and-iis6/_static/image20.jpg)

Das Verzeichnis c:\inetpub\wwwroot\ ist eine spezielle Verzeichnis in, das die spezielle Gruppe von IIS 6.0 "IIS\_WPG' Read wurde bereits zugewiesen &amp; Berechtigungen ausführen, Ordnerinhalt auflisten und lesen. Um über die Schreibberechtigung erteilen, müssen Sie jedoch das Kontrollkästchen Zulassen klicken Sie zum Schreiben auf:

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6.0 verfügt jetzt über die Schreibberechtigung für diesen Ordner. Gewähren Sie Schreibberechtigungen für andere Ordner folgendermaßen – Beachten Sie, müssen Sie möglicherweise IIS hinzufügen\_WPG-Gruppe, wenn sie nicht bereits vorhanden ist.

> [!CAUTION]
> Erteilen von Schreibberechtigungen für IIS\_WPG lässt ASP.NET-Anwendung in dieses Verzeichnis geschrieben.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Die integrierte Authentifizierung mit SQL Server unterstützen

Integrierte Authentifizierung ermöglicht für SQL Server, Windows NT-Authentifizierung zu nutzen, so überprüfen Sie die SQL Server-Anmeldekontos. Dies ermöglicht es dem Benutzer, die standardmäßige SQL Server-Anmeldevorgang zu umgehen. Bei diesem Ansatz kann ein Netzwerkbenutzers eine SQL Server-Datenbank zugreifen, ohne eine separate Anmelde-ID oder ein Kennwort angeben, da SQL Server aus dem Windows NT Network-Security-Prozess die Benutzer- und Kennwortinformationen erhält.

Auswählen der integrierten Authentifizierung für ASP.NET-Anwendungen ist eine gute Wahl, da keine Anmeldeinformationen in der Verbindungszeichenfolge für die Anwendung gespeichert sind. Stattdessen wird die Verbindungszeichenfolge zur Verbindung mit SQL wie folgt aussehen:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Diese Verbindungszeichenfolge weist SQL Server die Windows-Anmeldeinformationen, der die Anwendung versucht, den Zugriff auf SQL Server verwenden. Im Fall von für ASP.NET/IIS werden erteilt 6 wäre dies ein Konto in der IIS-\_WPG-Gruppe.

Zum Aktivieren der integrierten Authentifizierung zwischen SQL Server und ASP.NET müssen Sie zunächst sicherstellen, dass SQL Server, für die integrierte Authentifizierung oder gemischten Authentifizierungsmodus konfiguriert ist - überprüfen Sie mit Ihrem DBA um dies zu ermitteln. Wenn SQL Server in einem der folgenden beiden Modi ist, können Sie die integrierte Authentifizierung verwenden.

Öffnen Sie SQL Server Enterprise Manager (Start | Programme | Microsoft SQL Server | Enterprise Manager), wählen Sie den entsprechenden Server, und erweitern Sie den Ordner Sicherheit:

![](aspnet-and-iis6/_static/image22.jpg)

Wenn "BUILTINT\IIS\_WPG" Gruppe nicht vorhanden ist, mit der rechten Maustaste auf Anmeldungen aus, und wählen Sie "Neue Anmeldung":

![](aspnet-and-iis6/_static/image23.jpg)

In der "Name:" Geben Sie entweder ein Textfeld "[Server/Domänenname] \IIS\_WPG", klicken Sie auf die Schaltfläche mit den Auslassungspunkten, um die Auswahl der Windows NT-Benutzer/Gruppe zu öffnen:

![](aspnet-and-iis6/_static/image24.jpg)

Wählen Sie den aktuellen Computer IIS\_WPG-Gruppe, und klicken Sie auf 'Hinzufügen', und OK, um die Auswahl zu schließen.

Sie müssen auch die Standarddatenbank und die Berechtigungen zum Zugriff auf die Datenbank festlegen. Zum Festlegen der Standarddatenbank wählen Sie aus der Dropdownliste aus, z. B. unten Northwind ausgewählt ist:

![](aspnet-and-iis6/_static/image25.jpg)

Klicken Sie dann auf der Registerkarte "Datenbankzugriff" auf:

![](aspnet-and-iis6/_static/image26.jpg)

Klicken Sie auf das Kontrollkästchen "zulassen" für jede Datenbank, die Sie Zugriff auf zulassen möchten. Sie müssen auch Datenbankrollen auswählen Überprüfung Db\_Besitzer stellt sicher, dass die Anmeldung verfügt über alle erforderlichen Berechtigungen zum Verwalten und verwenden die ausgewählte Datenbank.

Klicken Sie auf OK, um das Dialogfeld "Eigenschaft" zu beenden. Ihre ASP.NET-Anwendung ist jetzt für die Unterstützung der integrierten SQL Server-Authentifizierung konfiguriert.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>Führen Sie ASP.NET 1.0 nicht im einheitlichen Modus von IIS 6.0

ASP.NET 1.0 auf IIS 6.0 wird nur in IIS 5-Kompatibilitätsmodus unterstützt.

Zum Konfigurieren von ASP.NET 1.0 im IIS 5.0-Kompatibilitätsmodus ausgeführt öffnen Sie Internetdienste-Manager zu, und klicken Sie mit der rechten Maustaste auf Websites, und wählen Sie Eigenschaften:

![](aspnet-and-iis6/_static/image27.jpg)

Wechseln Sie zur Registerkarte "Dienst", und überprüfen? Ausführen WWW-Dienst im IIS 5.0-Isolationsmodus?

![](aspnet-and-iis6/_static/image28.jpg)
