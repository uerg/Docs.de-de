---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Erstellen im Kapitel über das Downloads für die EF 5 MVC 4 Lernprogramme | Microsoft Docs
author: Rick-Anderson
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: bda7effabd715e4658d2472e1f0a66d7bba18cab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878515"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Erstellen im Kapitel über das Downloads für EF 5 MVC 4-Lernprogramme
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


## <a name="building-the-chapter-downloads"></a>Erstellen die Downloads Kapitel

1. Herunterladen Sie und Entzippen Sie die Project-Beispiel Zip-Datei. In das Downloadpaket entpackt finden Sie zusätzliche Zip-Dateien, eine für den Abschluss der einzelnen Kapitel.
2. Klicken Sie mit der rechten Maustaste auf die gewünschte Zip-Datei, klicken Sie auf **Eigenschaften**, und klicken Sie auf die **zum Aufheben der Sperre** Schaltfläche.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Entzippen Sie die Datei an.
4. Doppelklicken Sie auf die *CUx.sln* Datei zum Starten von Visual Studio.
5. Aus der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager**, klicken Sie dann **Package Manager Console**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. Klicken Sie in der Paket-Manager-Konsole (PMC) auf **wiederherstellen**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Beenden Sie Visual Studio.
8. Visual Studio neu starten, öffnen die Projektmappendatei Sie im vorherigen Schritt geschlossen.
9. Geben Sie in der Paket-Manager-Konsole (PMC) die `Update-Database` Befehl:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Wenn Sie die folgende Fehlermeldung angezeigt:  
    >   
    >  *Der Begriff 'Update-Database' ist nicht als Namen für ein Cmdlet, Funktion, Skriptdatei oder eines ausführbaren Programms erkannt. Überprüfen Sie die Schreibweise des Namens, oder wenn ein Pfad enthalten ist, stellen Sie sicher, dass der Pfad korrekt ist, und versuchen Sie es erneut.*  
    > Beenden Sie und starten Sie Visual Studio.

    Jeder Migration wird ausgeführt, und der Seed-Methode wird ausgeführt. Sie können jetzt die app ausführen.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Vorherige](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
