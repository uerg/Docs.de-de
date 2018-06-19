---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Aktualisieren einer ASP.NET MVC 1.0-Anwendung in ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: In diesem Dokument wird sowohl mit einem Assistenten und manuell eine ASP.NET MVC 1.0-Anwendung in ASP.NET MVC 2 aktualisieren. Dieses Dokument ist auch für d verfügbar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530059"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Aktualisieren einer ASP.NET MVC 1.0-Anwendung in ASP.NET MVC 2
====================
> In diesem Dokument wird sowohl mit einem Assistenten und manuell eine ASP.NET MVC 1.0-Anwendung in ASP.NET MVC 2 aktualisieren. Dieses Dokument ist auch zur [herunterladen](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Einführung

ASP.NET MVC 2 kann parallel zu ASP.NET MVC 1.0 auf dem gleichen Server installiert werden. Dies ermöglicht Anwendung Entwickler Flexibilität bei der Wahl, wenn eine 1.0 für ASP.NET MVC-Anwendung für ASP.NET MVC 2 aktualisieren.

Visual Studio 2010 umfasst einen Assistenten, Upgrades vorhandene 1.0 für ASP.NET MVC-Projekte mit Visual Studio 2008 in ASP.NET MVC 2 integriert. Der Upgrade-Assistent wird durch ein 1.0 für ASP.NET MVC-Projekt in Visual Studio 2010 öffnen initiiert.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Upgrade-Assistent für ASP.NET MVC 1.0 für Visual Studio 2008 SP1

Um eine ASP.NET MVC 1.0-Anwendung für ASP.NET MVC 2 in Visual Studio 2008 SP1 zu aktualisieren, verwenden Sie die (nicht unterstützte) MvcAppConverter-Anwendung. Sie können diese Anwendung unter folgender URL herunterladen:

[https://go.Microsoft.com/fwlink/?LinkId=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Manuelles Aktualisieren eines ASP.NET MVC 1,0-Projekts

Um eine vorhandene 1.0 für ASP.NET MVC-Anwendung auf Version 2 zu aktualisieren, gehen Sie folgendermaßen vor:

1. Erstellen Sie eine Sicherungskopie des vorhandenen Projekts.
2. Öffnen Sie in einem Text-Editor die Projektdatei (die Datei mit der Dateierweiterung CSPROJ- oder VBPROJ), und suchen Sie das ProjectTypeGuid-Element. Ersetzen Sie den Wert des jeweiligen Elements die GUID {603c0e0b-db56-11dc-be95-000d561079b0} {F85E285D-A4E0-4152-9332-AB1D724D3325}. Wenn Sie fertig sind, sollte der Wert dieses Elements wie folgt aussehen: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Bearbeiten Sie die Datei "Web.config" im Stammordner Web-Anwendung. Suchen Sie nach System.Web.Mvc, Version = 1.0.0.0, und Ersetzen Sie alle Instanzen mit System.Web.Mvc, Version = 2.0.0.0.
4. Wiederholen Sie den vorherigen Schritt für die Datei "Web.config" im Ordner "Ansichten" ein.
5. Öffnen Sie das Projekt mit Visual Studio, und klicken Sie in **Projektmappen-Explorer**, erweitern Sie die **Verweise** Knoten. Löschen Sie den Verweis auf System.Web.Mvc (die auf die Version 1.0-Assembly verwiesen wird). Fügen Sie einen Verweis auf System.Web.Mvc (v2.0.0.0).
6. Das folgende BindingRedirect-Element in die Datei "Web.config" im Stammverzeichnis Anwendung, klicken Sie im Abschnitt Datenausdrücke hinzufügen:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Erstellen Sie eine neue, leere ASP.NET MVC 2-Anwendung. Kopieren Sie die Dateien aus dem Ordner Scripts, der die neue Anwendung in den Ordner "Skripts" der vorhandenen Anwendung.
8. Aktualisieren Sie die vorhandene Ressourcenkennzeichnung™ s CSS-Datei mit den CSS-Stil-Definitionen in der Datei "Site.CSS" ändern.
9. Kompilieren Sie die Anwendung, und führen Sie es. Falls ein Fehler auftritt, finden Sie im Abschnitt wichtige Änderungen von der [Neuigkeiten in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) Seite.
