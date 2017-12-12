---
uid: whitepapers/side-by-side-with-10
title: "ASP.NET-Seite-an-Seite-Ausführung von .NET Framework 1.0 und 1.1 | Microsoft Docs"
author: rick-anderson
description: "In diesem Whitepaper wird beschrieben, wie .NET 1.0 und 1.1 von .NET installieren, auf dem Computer, ermöglicht eine ASP.NET-Webanwendung auf eine Version von den Fram ausgeführt wird..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>ASP.NET-Seite-an-Seite-Ausführung von .NET Framework 1.0 und 1.1
====================
> Dieses Whitepaper beschreibt, wie auf dem Computer, ermöglicht eine ASP.NET-Webanwendung zum Ausführen auf beide Versionen des Frameworks .NET 1.0 und 1.1 von .NET zu installieren.
> 
> Gilt für ASP.NET 1.0 und ASP.NET 1.1.


In ASP.NET Datenändernde Anwendungen parallel ausgeführt werden, wenn sie auf dem gleichen Computer installiert sind, jedoch verschiedene Versionen von .NET Framework verwenden. Das folgende Thema enthält Informationen zum Konfigurieren von ASP.NET-Anwendungen für Seite-an-Seite-Ausführung und enthält detaillierte Schritte für:

- [Verwalten Sie Ihre Webanwendung-Zuordnung für .NET Framework, Version 1.0, während der installation](#1)
- [Ordnen Sie eine Webanwendung auf eine bestimmte Version von .NET Framework](#2)
- [Suchen Sie die Version von .NET Framework, die eine Website verwendet wird](#3)

Wenn eine Komponente oder Anwendung auf einem Computer aktualisiert wird, wird die ältere Version bisher entfernt und durch die neuere Version ersetzt. Wenn die neue Version nicht mit der vorherigen Version kompatibel ist, wird dies in der Regel andere Anwendungen, die der Komponente oder Anwendung verwenden unterbrochen. .NET Framework bietet Unterstützung für Seite-an-Seite-Ausführung, bei dem mehrere Versionen einer Assembly oder Anwendung gleichzeitig auf demselben Computer installiert werden kann. Da mehrere Versionen gleichzeitig installiert werden können, können verwaltete Anwendungen auswählen, welche Version Sie verwenden, ohne Auswirkungen auf Anwendungen, die eine andere Version verwenden.

Standardmäßig werden während der Installation von .NET Framework, Version 1.1, alle vorhandene ASP.NET-Anwendungen automatisch konfiguriert die neueste Version von .NET Framework verwenden. Wenn Sie Ihre ASP.NET-Anwendungen .NET Framework 1.1 standardmäßig nicht wünschen, klicken Sie auf [hier](#1) zu erfahren, wie Sie dies während der Installation zu verhindern.

Wenn Sie Ihre Web-Server, auf .NET Framework 1.1 aktualisieren und eine oder mehrere Webanwendungen zu .NET Framework 1.0 ausführen möchten, müssen Sie die Zuordnung der Internetinformationsdienste (Internet Information Services, IIS)-Skript zu aktualisieren. Die Skriptzuordnung ist der Mechanismus zum Zuordnen der Erweiterungs für eine bestimmte Webanwendung auf eine Version von .NET Framework. Klicken Sie auf [hier](#2) Informationen zum Zuordnen einer Webanwendung auf eine bestimmte Version von .NET Framework.

Können Sie den Internetinformationsdienste-Manager Informationen oder die IIS-Registrierungstool (Aspnet\_regiis.exe) um herauszufinden, welche .NET Framework-Version eine bestimmte Webanwendung ausgeführt wird. Klicken Sie auf [hier](#3) um zu erfahren, wie Sie die Version von .NET Framework zu ermitteln, die eine Website verwendet wird.

Import berücksichtigen beim Migrieren zu .NET Framework 1.1 ist, dass jede Version von .NET Framework eine eigene Datei "Machine.config" verwendet. Folglich müssen ein Webadministrator Änderungen an der Datei "Machine.config" vorgenommen wurde, diese Änderungen in die Datei Machine.config von .NET Framework 1.1 migriert werden.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Verwalten Ihre Webanwendung Zuordnung zu .NET Framework 1.0 während der installation

Standardmäßig werden alle vorhandene ASP.NET-Anwendungen automatisch während der Installation auf die neuere Version von .NET Framework neu konfiguriert. Verwenden die neuere Version von .NET Framework, können Anwendungen vollständige Verbesserungen und neuen Features in der neuen Version nutzen. Zur gleichen Zeit Administrator Web, präzise steuern, welche Anwendungen sollten aktualisiert werden, können verhindern, dass die automatische neuzuordnung der alle vorhandenen ASP.NET-Anwendungen während der Installation von .NET Framework.

Um zu verhindern, dass die automatische neuzuordnung der gesamte ASP.NET-Anwendung auf die neuere Version von .NET Framework kann die Web-Administrator die Befehlszeilenoption /noaspupgrade in das Setupprogramm Dotnetfx.exe verwenden.

**Um zu verhindern, insgesamt neuzuordnung der ASP.NET-Anwendung auf neuere version**

1. Wechseln Sie zu **starten**.
2. Klicken Sie auf **ausführen**.
3. Geben Sie **cmd** ein.
4. Klicken Sie auf **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. Geben Sie an der Eingabeaufforderung die folgende Zeile zum Starten der Installation von .NET Framework: **Dotnetfx.exe c: "/noaspupgrade installieren?**.  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Klicken Sie auf **Ja** in der Microsoft .NET Framework 1.1-Setuphilfe. Dadurch wird den Setupvorgang von .NET Framework 1.1 gestartet.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Ordnen Sie eine Webanwendung auf eine bestimmte Version von .NET Framework

Jede Version von .NET Framework enthält eine Version des ASP.NET IIS-Registrierungstool (Aspnet\_regiis.exe). Mit diesem Tool können Administratoren, um anzugeben, dass eine Webanwendung unter einer bestimmten Version von .NET Framework ausgeführt werden. Dies wird als Zuordnung von einer Webanwendung auf eine Version von .NET Framework bezeichnet. Administratoren müssen auswählen, dass das Aspnet\_regiis.exe, der die Version von .NET Framework entspricht, die die Web-Anwendung zugeordnet werden soll. Beispielsweise muss ein Administrator, um anzugeben, dass eine Website mit .NET Framework 1.1 verwenden möchte, das Aspnet verwenden\_regiis.exe, der mit .NET Framework 1.1 enthalten ist.

Das Aspnet\_regiis.exe für Version 1.0 befindet sich unter:

- C:\WINDOWS\Microsoft.NET\Framework\**Version 1.0.3705** \aspnet\_Regiis

Das Aspnet\_regiis.exe für Version 1,1 befindet sich unter:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_Regiis

Das Aspnet\_regiis.exe bietet zwei Optionen für Skripts, die eine Webanwendung zuordnen:

- **-s** legt die Skriptzuordnung in den Pfad und den untergeordneten Verzeichnissen.
- **-"sn"** legt die Skriptzuordnung in nur den Pfad fest.

Den Pfad definierten der IIS-Metadaten Webanwendungspfad, definiert in Form von W3SVC/ROOT / {WebSiteNumber} / {Anwendung\_Name}. Für eine Webanwendung namens Portal befindet sich unter der Standardwebsite ist z. B. der Metabasispfad W3SVC/1/ROOT/Portal an.

![](side-by-side-with-10/_static/image4.gif)

Beachten Sie, dass Sie ein Tool Namens der Metabase-Editor auch zum Abrufen der Metabasispfad verwenden können. Sie können dieses Tool von der Microsoft-Support-Website unter herunterladen [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Führen Sie Aspnet\_Zuordnung und seine Subapplication regiis.exe -s W3SVC/1/ROOT /-Portal, um das IIS-Portal Aktualisieren von Skripts.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Führen Sie Aspnet\_regiis.exe-"sn" W3SVC/1/ROOT /-Portal, um das Portal IIS-Skript aktualisieren zuordnen, ohne Einfluss auf Anwendungen in das Portal? s Unterverzeichnisse.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Suchen Sie die .NET Framework-Version, die eine Webanwendung verwendet wird

Ein Administrator kann Internetdienste-Manager verwenden, um zu ermitteln, welche Version von .NET Framework eine Website ausgeführt wird. Starten Sie verschiedenen Betriebssystemversionen Internetdienste-Manager unterschiedlich. Um den Dienst-Manager zu starten, führen Sie die unten aufgeführten Schritte aus.

**Um Internetdienst-Manager zu starten.**

1. Wechseln Sie zu **starten**.
2. Klicken Sie auf **ausführen**.
3. Typ **Inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Wählen Sie aus dem Internetdienst-Manager, die Web-Anwendung, deren Version von .NET Framework, die Sie wissen möchten.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Mit der rechten Maustaste auf die Webanwendung, und klicken Sie auf **Eigenschaften.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. Wählen Sie das Eigenschaftenfenster **Konfiguration.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. Wählen Sie in der Zuordnungstabelle Anwendung **aspx**, und klicken Sie auf **bearbeiten**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Aus der **ausführbare Datei** Textfeld Blick auf das Versionsverzeichnis durch Ausführen eines Bildlaufs. Wenn das Versionsverzeichnis v.1.1.4322 ist, wird die Anwendung auf .NET Framework 1.1 zugeordnet. Wenn das Versionsverzeichnis Version 1.0.3705 ist, wird hingegen die Anwendung .NET Framework 1.0 zugeordnet.  
  
    ![](side-by-side-with-10/_static/image12.gif)
