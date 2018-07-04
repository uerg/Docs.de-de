---
uid: whitepapers/aspnet-and-iis6
title: Ausführen von ASP.NET 1.1 mit IIS 6.0 | Microsoft-Dokumentation
author: rick-anderson
description: Während Windows Server 2003, IIS 6.0 und ASP.NET 1.1 enthält, werden diese Komponenten standardmäßig deaktiviert. In diesem Whitepaper wird beschrieben, wie IIS 6.0 ermöglichen eine...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1983ef5b4902acc303ae224d5973eb2644284585
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383424"
---
<a name="running-aspnet-11-with-iis-60"></a>Ausführen von ASP.NET 1.1 mit IIS 6.0
====================
> Während Windows Server 2003, IIS 6.0 und ASP.NET 1.1 enthält, werden diese Komponenten standardmäßig deaktiviert. Dieses Whitepaper enthält Informationen zum Aktivieren von IIS 6.0 und ASP.NET 1.1 und verschiedene Konfigurationseinstellungen für die optimale Leistung zu erzielen, von IIS und ASP.NET empfohlen.
> 
> Gilt für ASP.NET 1.1 und IIS 6.0.


Im Lieferumfang von ASP.NET 1.1 ist Windows Server 2003, die auch die neueste Version von Internet Information Server (IIS) Version 6.0 enthält. IIS 6.0 und ASP.NET 1.1 integrieren dienen und ASP.NET wird nun standardmäßig die neue IIS 6.0-Worker-Prozessmodell.

## <a name="aspnet-11-is-not-installed-by-default"></a>ASP.NET 1.1 ist nicht standardmäßig installiert.

Im Gegensatz zu früheren Versionen von Microsoft Server-Betriebssysteme ist Internet Information Server (IIS) nicht standardmäßig aktiviert. Es ist von ASP.NET 1.1. Es gibt zwei Optionen zum Aktivieren von IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Aktivieren von IIS, Option #1 - Serverkonfigurations-Assistent

Windows Server 2003 umfasst eine neue "Serverkonfigurations-Assistent" damit Sie Ihren Server ordnungsgemäß in den gewünschten Modus konfigurieren können.

Zum Starten des Assistenten - Hinweis: zum Ausführen des Assistenten Sie als Administrator - angemeldet sein müssen finden Sie unter: Start | Programme | Verwaltung und die Option "Konfiguration des Servers".

Nach dem Auswählen der Bildschirm "Serverkonfigurations-Assistent" Öffnen sollte angezeigt werden:

![](aspnet-and-iis6/_static/image1.jpg)

Klicken Sie auf "Weiter &gt;":

![](aspnet-and-iis6/_static/image2.jpg)

Klicken Sie auf "Weiter &gt;"

![](aspnet-and-iis6/_static/image3.jpg)

Auf diesem Bildschirm müssen Sie auf ' Anwendungsserver (IIS, ASP.NET) als die Optionen zum Konfigurieren.

Klicken Sie auf "Weiter &gt;".

![](aspnet-and-iis6/_static/image4.jpg)

Nach der Auswahl den Server als Anwendungsserver konfigurieren, wird dieser Bildschirm angezeigt, aufgefordert wird, welche zusätzlichen Funktionen installiert werden soll. Keine der beiden Optionen ist standardmäßig ausgewählt. Um ASP.NET automatisch zu aktivieren, müssen Sie auf "Aktivieren von ASP. NET ".

Klicken Sie auf "Weiter &gt;".

![](aspnet-and-iis6/_static/image5.jpg)

Dieser Bildschirm zeigt an, welche Optionen sind, installiert werden.

Klicken Sie auf "Weiter &gt;".

![](aspnet-and-iis6/_static/image6.jpg)

Sie werden dieser Bildschirm angezeigt, während die von Ihnen gewählten Option installiert werden. Es ist normal, andere Dialogfeld angezeigt, die Felder angezeigt werden, wie Dienste installiert werden. Sie können außerdem den Speicherort von der Windows 2003 Server-Installations-CD aufgefordert.

Klicken Sie auf "Weiter &gt;" Wenn Sie fertig sind.

![](aspnet-and-iis6/_static/image7.jpg)

Klicken Sie auf "Fertig stellen": der Windows Server 2003 ist jetzt konfiguriert, um IIS 6.0 und ASP.NET 1.1 zu unterstützen.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Aktivieren von IIS, Option #2 – manuelle Konfiguration von IIS und ASP.NET

Wenn Sie nicht, verwenden Sie die "Serverkonfigurations-Assistent möchten" können Sie optional IIS 6.0 und ASP.NET 1.1 mithilfe von "Programme hinzufügen oder entfernen" in der Systemsteuerung installieren.

Öffnen Sie zunächst die Systemsteuerung:

![](aspnet-and-iis6/_static/image8.jpg)

Klicken Sie anschließend auf "Hinzufügen/entfernen Windows Components" werden der Windows-Komponenten-Assistent geöffnet:

![](aspnet-and-iis6/_static/image9.jpg)

Markieren Sie und überprüfen Sie 'Anwendungsserver' aus, und klicken Sie dann auf 'Details'? Schaltfläche:

![](aspnet-and-iis6/_static/image10.jpg)

Überprüfen Sie zum Installieren von ASP.NET "ASP. NET ".

Klicken Sie auf "OK", um den Assistenten der Windows-Komponenten zurückgegeben. Klicken Sie auf "Weiter &gt;" über den Assistenten zum Installieren von Windows-Komponenten:

![](aspnet-and-iis6/_static/image11.jpg)

Es ist normal, andere Dialogfeld angezeigt, die Felder angezeigt werden, wie Dienste installiert werden. Sie können außerdem den Speicherort von der Windows 2003 Server-Installations-CD aufgefordert.

Wenn die Installation abgeschlossen ist, sehen Sie den letzten Bildschirm des Assistenten für Windows-Komponente:

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6.0 und ASP.NET 1.1 sind nun konfiguriert und verfügbar.

## <a name="recommended-settings"></a>Empfohlene Einstellungen

Bei der Ausführung von ASP.NET 1.1 mit IIS 6.0 stehen mehrere Konfigurationseinstellungen, die empfohlen werden, um eine optimale Leistung von ASP.NET zu erhalten:

- Konfigurieren von Worker-Prozess-Speicherlimits
- Konfigurieren von Arbeitsprozessen

### <a name="configuring-worker-process-memory-limits"></a>Konfigurieren von Worker-Prozess-Speicherlimits

Standardmäßig wird IIS 6.0 keine Beschränkung auf die Größe des Arbeitsspeichers festgelegt, die IIS verwenden darf. ASP. NET Cache-Funktion basiert auf eine Einschränkung des Arbeitsspeichers, damit der Cache nicht verwendete Elemente proaktiv aus dem Arbeitsspeicher entfernen kann.

Es wird empfohlen, dass Sie die speicherwiederverwendung-Feature von IIS 6.0 konfigurieren. So konfigurieren Sie diese öffnen Internetinformationsdienste-Manager (Start | Programme | Verwaltung | Internet Information Services). Sobald geöffnet ist, erweitern Sie den Ordner "Anwendungspools" aus:

Für jeden Anwendungspool:

![](aspnet-and-iis6/_static/image13.jpg)

1. Mit der rechten Maustaste auf Anwendungspools, z. B. "DefaultAppPool", und wählen Sie "Eigenschaften":

![](aspnet-and-iis6/_static/image14.jpg)

2. Aktivieren Sie als Nächstes Arbeitsspeicher wiederverwenden, indem Sie entweder auf "maximal verbrauchter Speicher (in MB):". Der Wert darf nicht mehr als die Menge des Arbeitsspeichers (nicht virtuellen) auf dem Server sein, eine gute Annäherung 60 % des physischen Arbeitsspeichers, d. h. für einen Server mit 512MB physischen Arbeitsspeicher wählen 310. Es wird außerdem empfohlen, dass die maximale 800 MB nicht überschreiten, wenn Sie einen Adressraum von 2 GB verwenden. Wenn der Speicherbereich für die Adresse des Servers/3 GB ist, kann der Höchstwert des Arbeitsspeichers für den Arbeitsprozess so hoch wie 1 800 MB sein:

![](aspnet-and-iis6/_static/image15.jpg)

Klicken Sie auf "Übernehmen" und "OK", um das Dialogfeld "Eigenschaften" zu beenden. Wiederholen Sie diesen Schritt für alle verfügbaren Anwendungspools.

### <a name="configuring-worker-recycling"></a>Konfigurieren von Worker-Wiederverwendung

IIS 6.0 ist standardmäßig so konfiguriert, um ihren Arbeitsprozess benötigt alle 29 Stunden wiederverwendet werden. Dies ist etwas aggressiver für eine Anwendung, die Ausführung von ASP.NET aus, und es wird empfohlen, dass automatische Arbeitsprozessen deaktiviert ist.

Um automatische Arbeitsprozessen zu deaktivieren, öffnen Sie zunächst Internetinformationsdienste-Manager (Start | Programme | Verwaltung | Internet Information Services). Sobald geöffnet ist, erweitern Sie den Ordner "Anwendungspools" aus:

![](aspnet-and-iis6/_static/image16.jpg)

Für jeden Anwendungspool:

1. Mit der rechten Maustaste auf Anwendungspools, z. B. "DefaultAppPool", und wählen Sie "Eigenschaften":

![](aspnet-and-iis6/_static/image17.jpg)

2. Deaktivieren Sie "Rbeitsprozesse (in Minuten):":

![](aspnet-and-iis6/_static/image18.jpg)

Klicken Sie auf "Übernehmen" und "OK", um das Dialogfeld "Eigenschaften" zu beenden. Wiederholen Sie diesen Schritt für alle verfügbaren Anwendungspools.

## <a name="granting-write-access-to-the-file-system"></a>Gewährung des Schreibzugriffs auf das Dateisystem

Wenn Ihre Anwendung Schreibzugriff auf das Dateisystem benötigt, und Sie NTFS, die Sie benötigen verwenden, so ändern Sie eine Access Zugriffssteuerungsliste (ACL) auf den Ordner oder die Datei um ASP.NET Zugriff zu gewähren.

Z. B. ASP.NET gewähren Schreibzugriff auf die c:\inetpub\wwwroot zunächst öffnen Sie Explorer, und navigieren Sie zu dem Verzeichnis:

![](aspnet-and-iis6/_static/image19.jpg)

Als Nächstes mit der rechten Maustaste auf das Verzeichnis, z. B. "Wwwroot" und wählen Sie die Eigenschaften. Wählen Sie das Dialogfeld "Eigenschaften" die Registerkarte "Sicherheit":

![](aspnet-and-iis6/_static/image20.jpg)

Das Verzeichnis c:\inetpub\wwwroot\ ist eines speziellen Verzeichnisses, die spezielle Gruppe für IIS 6.0 "IIS\_WPG" Read wurde bereits erteilt &amp; Berechtigungen ausführen, Ordnerinhalt auflisten und lesen. Um Schreibberechtigungen zu gewähren, müssen Sie jedoch das Kontrollkästchen Zulassen für Schreibvorgänge zu aktivieren:

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6.0 ist jetzt Schreibberechtigung für diesen Ordner. Gewähren von Berechtigungen zum Schreiben in anderen Ordnern, führen Sie diese Schritte aus: Beachten Sie, Sie müssen möglicherweise IIS hinzufügen\_WPG-Gruppe, wenn es nicht bereits vorhanden ist.

> [!CAUTION]
> Gewähren von Schreibberechtigungen für IIS\_WPG jeder ASP.NET-Anwendung in dieses Verzeichnis schreiben können.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Unterstützt die integrierte Authentifizierung mit SQL Server

Integrierte Authentifizierung ermöglicht für SQL Server, Windows NT-Authentifizierung zu nutzen, überprüfen Sie die Anmeldekonten für SQL Server. Dadurch kann der Benutzer, den Standardprozess des SQL Server-Anmeldung zu umgehen. Bei diesem Ansatz kann ein Netzwerkbenutzer eine SQL Server-Datenbank zugreifen, ohne eine separate Anmeldung ID oder ein Kennwort angeben, da SQL Server die Kennwortinformationen zu Benutzern und das aus dem Windows NT Network-Security-Prozess erhält.

Auswählen der integrierten Authentifizierung für ASP.NET-Anwendungen ist eine gute Wahl, da keine Anmeldeinformationen in der Verbindungszeichenfolge für Ihre Anwendung ständig gespeichert werden. Stattdessen wird die Verbindungszeichenfolge zur Verbindung mit SQL wie folgt aussehen:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Diese Verbindungszeichenfolge weist SQL Server, die Windows-Anmeldeinformationen der Anwendung versucht, den Zugriff auf SQL Server verwenden. Im Fall von ASP.NET/IIS-Schnittstellen 6 wäre dies ein Konto in der IIS\_WPG-Gruppe.

Zum Aktivieren der integrierten Authentifizierung zwischen SQL Server und ASP.NET müssen Sie zuerst sicherstellen, dass SQL Server, für die integrierte Authentifizierung oder im gemischten Modus Authentifizierung konfiguriert ist - überprüfen Sie für Ihren Datenbankadministrator frei, um dies zu ermitteln. Wenn SQL Server in einem dieser beiden Modi ist, können Sie die integrierte Authentifizierung.

Öffnen Sie SQL Server Enterprise Manager (Start | Programme | Microsoft SQL Server | Enterprise Manager), wählen Sie den entsprechenden Server, und erweitern Sie den Ordner Sicherheit:

![](aspnet-and-iis6/_static/image22.jpg)

Wenn "BUILTINT\IIS\_WPG" Gruppe ist nicht aufgeführt, mit der rechten Maustaste auf Anmeldungen, und wählen Sie "Neue Anmeldung":

![](aspnet-and-iis6/_static/image23.jpg)

In der "Name:" Textfeld Geben Sie "[Name des Servers bzw. der Domäne] \IIS\_WPG" oder klicken Sie auf die Schaltfläche mit den Auslassungspunkten, um die Auswahl der Windows NT-Benutzers oder einer Gruppe zu öffnen:

![](aspnet-and-iis6/_static/image24.jpg)

Wählen Sie den aktuellen Computer IIS\_WPG-Gruppe, und klicken Sie auf "Hinzufügen" und OK, um die Auswahl zu schließen.

Sie müssen auch die Standarddatenbank und die Berechtigungen zum Zugriff auf die Datenbank festlegen. Zum Festlegen die Standarddatenbank wählen Sie aus der Dropdownliste aus, z. B. folgenden Northwind ausgewählt ist:

![](aspnet-and-iis6/_static/image25.jpg)

Klicken Sie anschließend auf der Registerkarte "Datenbankzugriff":

![](aspnet-and-iis6/_static/image26.jpg)

Klicken Sie auf das Kontrollkästchen Zulassen für jede Datenbank, die Sie Zugriff auf zulassen möchten. Sie müssen auch Datenbankrollen auswählen überprüfen Db\_Besitzer wird sichergestellt, dass die Anmeldung verfügt über alle erforderlichen Berechtigungen zum Verwalten und verwenden die ausgewählte Datenbank.

Klicken Sie auf OK, um das Dialogfeld "Eigenschaft" zu beenden. Ihre ASP.NET-Anwendung ist jetzt konfiguriert, um die Unterstützung der integrierten SQL Server-Authentifizierung.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>Führen Sie ASP.NET 1.0 nicht im einheitlichen Modus von IIS 6.0

ASP.NET 1.0 unter IIS 6.0 wird nur in IIS 5-Kompatibilitätsmodus unterstützt.

Zum Konfigurieren von ASP.NET 1.0 im IIS 5.0-Kompatibilitätsmodus ausgeführt öffnen Sie Internetdienste-Manager zu, und klicken Sie mit der rechten Maustaste auf die Websites ein, und wählen Sie Eigenschaften:

![](aspnet-and-iis6/_static/image27.jpg)

Wechseln Sie zur Registerkarte "Dienst", und überprüfen? Ausführen WWW-Dienst im IIS 5.0-Isolationsmodus?

![](aspnet-and-iis6/_static/image28.jpg)
