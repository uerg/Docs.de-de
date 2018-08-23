---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Aktualisieren von ASP.NET MVC 1.0-Anwendungen zu ASP.NET MVC 2 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Dokument wird beschrieben wie Sie manuell, und mit einem Assistenten eine ASP.NET MVC 1.0-Anwendung zu ASP.NET MVC 2 aktualisieren. In diesem Dokument finden Sie auch für d...
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 3de69df7e80037de35c2609232f4574bc9d03c80
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826562"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Aktualisieren von ASP.NET MVC 1.0-Anwendungen zu ASP.NET MVC 2
====================
> In diesem Dokument wird beschrieben wie Sie manuell, und mit einem Assistenten eine ASP.NET MVC 1.0-Anwendung zu ASP.NET MVC 2 aktualisieren. In diesem Dokument finden Sie auch für [herunterladen](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Einführung

ASP.NET MVC 2 kann parallel zu ASP.NET MVC 1.0 auf dem gleichen Server installiert werden. Dadurch wird die Anwendung Entwickler Flexibilität bei der Entscheidung, wann eine ASP.NET MVC 1.0-Anwendung zu ASP.NET MVC 2 aktualisiert.

Visual Studio 2010 enthält einen Assistenten, Upgrades, die vorhandene ASP.NET MVC 1.0-Projekten mit Visual Studio 2008 zu ASP.NET MVC 2 erstellt haben. Der Upgrade-Assistent wird initiiert, von einer ASP.NET MVC 1.0-Projekt in Visual Studio 2010 geöffnet.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Upgrade-Assistent für ASP.NET MVC 1.0 für Visual Studio 2008 SP1

Um eine ASP.NET MVC 1.0-Anwendung in ASP.NET MVC 2 in Visual Studio 2008 SP1 zu aktualisieren, verwenden Sie die (nicht unterstützte) MvcAppConverter-Anwendung ein. Sie können diese Anwendung von folgender URL herunterladen:

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Manuelles Aktualisieren eines ASP.NET MVC 1.0-Projekts

Um eine vorhandene ASP.NET MVC 1.0-Anwendung auf Version 2 zu aktualisieren, gehen Sie folgendermaßen vor:

1. Erstellen Sie eine Sicherungskopie des vorhandenen Projekts.
2. Klicken Sie in einem Text-Editor öffnen Sie die Projektdatei (die Datei mit der Dateinamenerweiterung .csproj oder .vbproj), und suchen Sie nach den ProjectTypeGuid-Element. Ersetzen Sie die GUID-{603c0e0b-db56-11dc-be95-000d561079b0} mit {F85E285D-A4E0-4152-9332-AB1D724D3325}, als der Wert dieses Elements. Wenn Sie fertig sind, sollte der Wert dieses Elements wie folgt lauten: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Bearbeiten Sie die Datei "Web.config" im Stammordner Web-Anwendung. Suchen Sie nach der System.Web.Mvc, Version = 1.0.0.0, und Ersetzen Sie alle Instanzen mit System.Web.Mvc, Version = 2.0.0.0.
4. Wiederholen Sie den vorherigen Schritt für die Datei "Web.config" im Ordner "Views" ein.
5. Öffnen Sie das Projekt mit Visual Studio, und klicken Sie in **Projektmappen-Explorer**, erweitern Sie die **Verweise** Knoten. Löschen Sie den Verweis auf System.Web.Mvc (die auf die Version 1.0-Assembly verwiesen wird). Fügen Sie einen Verweis auf System.Web.Mvc (v2.0.0.0) hinzu.
6. Fügen Sie der Datei "Web.config" im Stammverzeichnis Anwendung, die im Abschnitt "Konfiguration" mit dem folgenden BindingRedirect-Element hinzu:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Erstellen Sie eine neue leere ASP.NET MVC 2-Anwendung. Kopieren Sie die Dateien aus dem Ordner "Scripts" für die neue Anwendung in den Ordner "Scripts" der vorhandenen Anwendung ein.
8. Aktualisieren Sie die vorhandene Webanwendung€™ s CSS-Datei mit der CSS-Formatdefinitionen in der Datei "Site.CSS".
9. Kompilieren Sie die Anwendung, und führen Sie ihn aus. Wenn Fehler auftreten, finden Sie im Abschnitt grundlegende Änderungen in der die [Neuigkeiten in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) Seite.
