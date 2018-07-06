---
uid: whitepapers/side-by-side-with-10
title: ASP.NET-Seite-an-Seite-Ausführung von .NET Framework 1.0 und 1.1 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Whitepaper wird beschrieben, wie sowohl .NET 1.0 und 1.1 von .NET auf dem Computer, können eine ASP.NET-Webanwendung zum Ausführen auf einer Version des der zeitsteuerung der installieren...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 1018845e3d2c6603732bfbbde78f4a9183e49d5d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808394"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>ASP.NET-Seite-an-Seite-Ausführung von .NET Framework 1.0 und 1.1
====================
> In diesem Whitepaper wird beschrieben, wie zum Installieren von sowohl .NET 1.0 und 1.1 von .NET auf dem Computer, sodass einer ASP.NET-Webanwendung auf beiden Versionen des Frameworks ausgeführt werden.
> 
> ASP.NET 1.1 zu ASP.NET 1.0 angewendet.


In ASP.NET Datenändernde Anwendungen nebeneinander ausgeführt werden, wenn sie auf dem gleichen Computer installiert sind, jedoch verschiedene Versionen von .NET Framework verwenden. Das folgende Thema enthält Informationen zum Konfigurieren von ASP.NET-Anwendungen für Seite-an-Seite-Ausführung und bietet eine schrittweise Anleitung:

- [Verwalten Sie Ihrer Webanwendung-Zuordnung zu .NET Framework, Version 1.0, während der installation](#1)
- [Ordnen Sie einer Webanwendung in einer bestimmten Version von .NET Framework](#2)
- [Suchen Sie die Version von .NET Framework, die eine Website verwendet wird](#3)

Wenn eine Komponente oder Anwendung auf einem Computer aktualisiert wird, wird die ältere Version in der Vergangenheit entfernt und durch die neuere Version ersetzt. Wenn die neue Version nicht mit der vorherigen Version kompatibel ist, unterbricht dies in der Regel andere Anwendungen, die der Komponente oder Anwendung verwenden. .NET Framework bietet Unterstützung für Seite-an-Seite-Ausführung, bei dem mehrere Versionen einer Assembly oder Anwendung gleichzeitig auf demselben Computer installiert werden kann. Da mehrere Versionen gleichzeitig installiert werden können, können verwaltete Anwendungen auswählen, welche Version Sie verwenden, ohne Auswirkungen auf Anwendungen, die eine andere Version verwenden.

Standardmäßig werden während der Installation von .NET Framework, Version 1.1, alle vorhandene ASP.NET-Anwendungen automatisch neu konfiguriert um die neueste Version von .NET Framework zu verwenden. Wenn Sie Ihre ASP.NET-Anwendungen .NET Framework 1.1 standardmäßig nicht möchten, klicken Sie auf [hier](#1) zu erfahren, wie Sie dies während der Installation zu verhindern.

Wenn Sie Ihrem Webserver auf .NET Framework 1.1 aktualisieren und einen oder mehrere Webanwendungen auf .NET Framework 1.0 ausführen möchten, müssen Sie die Zuordnung der Internetinformationsdienste (Internet Information Services, IIS)-Skript aktualisiert. Der Skriptzuordnung ist der Mechanismus zum Zuordnen der Erweiterungs für eine bestimmte Webanwendung auf eine Version von .NET Framework. Klicken Sie auf [hier](#2) erfahren, wie eine Webanwendung auf eine bestimmte Version von .NET Framework zugeordnet.

Sie können den Internetinformationsdienste-Manager Informationen oder das ASP.NET IIS Registration-Tool verwenden (Aspnet\_regiis.exe) zu ermitteln, welche .NET Framework-Version eine bestimmte Webanwendung ausgeführt wird. Klicken Sie auf [hier](#3) zu erfahren, wie Sie die Version von .NET Framework zu ermitteln, die eine Website verwendet wird.

Ein Import Aspekt bei der Migration zu .NET Framework 1.1 ist, dass jede Version von .NET Framework eine eigene Datei "Machine.config" verwendet. Daher müssen ein Webadministrator die Datei "Machine.config" geändert hat, diese Änderungen in der Datei Machine.config-Datei von .NET Framework 1.1 migriert werden.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Verwalten Ihre Webanwendung-Zuordnung zu .NET Framework 1.0, während der installation

Standardmäßig werden alle vorhandene ASP.NET-Anwendungen automatisch während der Installation auf die neuere Version von .NET Framework neu konfiguriert. Verwenden die neuere Version von .NET Framework, können Anwendungen vollständige Verbesserungen und neuen Features in der neuen Version nutzen. Zur gleichen Zeit wird der Administrator Web präzise steuern, welche Anwendungen sollten aktualisiert werden, kann verhindern, dass die automatische neuzuordnung der alle vorhandenen ASP.NET-Anwendungen während der Installation von .NET Framework.

Um zu verhindern, die Automatisches Neuzuordnen von der gesamten ASP.NET-Anwendung auf die neuere Version von .NET Framework, kann der Web-Administrator die Befehlszeilenoption "/noaspupgrade" in das Setupprogramm Dotnetfx.exe verwenden.

**Um zu verhindern, insgesamt neuzuordnung der ASP.NET-Anwendung auf neuere version**

1. Wechseln Sie zu **starten**.
2. Klicken Sie auf **ausführen**.
3. Geben Sie **cmd** ein.
4. Klicken Sie auf **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. Geben Sie an der Eingabeaufforderung die folgende Zeile zum Starten der Installation von .NET Framework: **Dotnetfx.exe/c: "Installieren von /noaspupgrade?**.  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Klicken Sie auf **Ja** im Setup für Microsoft .NET Framework 1.1. Dadurch wird der Einrichtung von .NET Framework 1.1 gestartet.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Ordnen Sie einer Webanwendung in einer bestimmten Version von .NET Framework

Jede Version von .NET Framework umfasst eine Version des ASP.NET IIS Registration-Tool (Aspnet\_regiis.exe). Dieses Tool ermöglicht Administratoren, um anzugeben, dass eine Webanwendung unter einer bestimmten Version von .NET Framework ausgeführt werden. Dies wird wie die Zuordnung einer Webanwendung in eine Version von .NET Framework bezeichnet. Administratoren müssen das Aspnet auswählen\_regiis.exe, der die Version von .NET Framework entspricht, die mit der Web-Anwendung verknüpft werden. Beispielsweise muss ein Administrator, um anzugeben, dass eine Website mit .NET Framework 1.1 verwenden möchte, das Aspnet verwenden\_regiis.exe, die in .NET Framework 1.1 enthalten ist.

Das Aspnet\_regiis.exe für Version 1.0 befindet sich unter:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_Regiis

Das Aspnet\_regiis.exe für Version 1,1 befindet sich unter:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_Regiis

Das Aspnet\_regiis.exe bietet zwei Optionen für Skripts, die eine Webanwendung zuordnen:

- **-s** legt die Skriptzuordnung in den Pfad und in untergeordneten Verzeichnissen.
- **-sn** legt die Skriptzuordnung im Pfad nur.

Der Pfad definiert, die IIS-Metadaten Webanwendungspfad, die in Form von W3SVC/ROOT definiert ist / {WebSiteNumber} / {Anwendung\_Name}. Für eine Webanwendung namens Portal befindet sich unter der Standardwebsite ist beispielsweise der Metabasepfad des W3SVC/1/ROOT/Portal an.

![](side-by-side-with-10/_static/image4.gif)

Beachten Sie, dass Sie auch ein Tool Namens der Metabase-Editor verwenden können, um den Metabase-Pfad abzurufen. Sie können dieses Tool von der Microsoft-Support-Website unter [ https://support.microsoft.com/default.aspx?scid=kb; En-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Führen Sie Aspnet\_Zuordnung und die Subapplication regiis.exe -s W3SVC/1/ROOT /-Portal, um das IIS-Portal aktualisieren ein Skript.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Führen Sie Aspnet\_regiis.exe -sn W3SVC/1/ROOT /-Portal, um das Skript für Portals IIS aktualisieren zuzuordnen, ohne Auswirkungen auf Anwendungen im Portal? s Unterverzeichnisse.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Suchen Sie die .NET Framework-Version, die eine Webanwendung verwendet wird

Administratoren können den Internetdienste-Manager ermitteln, welche Version von .NET Framework eine Website ausgeführt wird. Starten Sie verschiedenen Betriebssystemversionen Internetdienste-Manager unterschiedlich. Um den Dienst-Manager zu starten, führen Sie die nachstehenden Schritte aus.

**Um Internetdienste-Manager zu starten.**

1. Wechseln Sie zu **starten**.
2. Klicken Sie auf **ausführen**.
3. Typ **Inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Wählen Sie vom Internet-Manager, die Web-Anwendung, deren Version von .NET Framework, die Sie wissen möchten.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Mit der rechten Maustaste auf die Webanwendung, und klicken Sie auf **Eigenschaften.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. Wählen Sie das Eigenschaftenfenster **Konfiguration.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. Wählen Sie in der Zuordnungstabelle für die Anwendung werden **aspx**, und klicken Sie auf **bearbeiten**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Von der **ausführbare Datei** Textfeld Blick auf das Versionsverzeichnis, indem Sie einen Bildlauf. Wenn das Versionsverzeichnis v.1.1.4322 ist, wird die Anwendung .NET Framework 1.1 zugeordnet. Wenn das Versionsverzeichnis Version 1.0.3705 ist, wird im Gegensatz dazu die Anwendung .NET Framework 1.0 zugeordnet.  
  
    ![](side-by-side-with-10/_static/image12.gif)
