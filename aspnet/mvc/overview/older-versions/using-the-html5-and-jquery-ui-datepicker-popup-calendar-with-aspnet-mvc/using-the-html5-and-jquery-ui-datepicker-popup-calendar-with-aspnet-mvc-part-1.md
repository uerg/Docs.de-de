---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: "Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 1 | Microsoft Docs"
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Arbeit mit Editorvorlagen Anzeigevorlagen und dem jQuery UI Datepicker-Popupkalenders in einer ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: 9320c8a2aadb3b3c5bd6cd90b59d8a72db384c0c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Verwenden des HTML5 und jQuery UI Datepicker-Popupkalenders mit ASP.NET MVC – Teil 1
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm erfahren Sie die Grundlagen der Arbeit mit Editorvorlagen Anzeigevorlagen und jQuery UI Datepicker-Popupkalenders in einer ASP.NET MVC-Webanwendung.


In diesem Lernprogramm lernen Sie die Grundlagen der Arbeit mit Editorvorlagen Anzeigevorlagen und dem jQuery [UI Datepicker-Popupkalenders](http://plugins.jquery.com/project/datepicker) in einer ASP.NET MVC-Webanwendung. Für dieses Lernprogramm können Sie Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;), ist eine kostenlose Version von Microsoft Visual Studio, oder Sie können Visual Studio 2010 SP1 verwenden, wenn Sie bereits, die verfügen.

Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderliche Software mithilfe der folgenden Links einzeln installieren:

- [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)

Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

In diesem Lernprogramm wird davon ausgegangen, Sie haben die [Einstieg in MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) Lernprogramm oder dass Sie bei der Entwicklung mit ASP.NET MVC vertraut sind. In diesem Lernprogramm beginnt mit abgeschlossenes Projekt aus der [Einstieg in MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) Lernprogramm.

In diesem Lernprogramm wird Code in c#. Allerdings die [Startprojekt](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) und abgeschlossenen Projekts werden auch in Visual Basic verfügbar.

Ein Visual Studio-Projekt mit c# und Visual Basic-Quellcode ist zu diesem Thema steht zur Verfügung: [herunterladen](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Was müssen Sie erstellen

Fügen Sie Vorlagen (insbesondere bearbeiten und Anzeigevorlagen) für die einfache Film-Angebot-Anwendung, die in erstellt wurde die [Einstieg in MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) Lernprogramm. Fügen Sie auch eine [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) Popupkalenders vereinfachen das Eingeben von Daten. Der folgende Screenshot zeigt die geänderte Anwendung mit dem jQuery UI Datepicker-Popupkalenders angezeigt.

![fertige jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Fähigkeiten, die Sie erfahren

Hier ist Sie lernen:

- Gewusst wie: Verwenden von Attributen aus der [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) Namespace zu steuern das Format der Daten aus, wenn er angezeigt wird und wenn befindet sich im Bearbeitungsmodus befindet.
- Erstellen von Vorlagen (bearbeiten und Anzeigevorlagen) zum Steuern der Formatierung von Daten.
- Gewusst wie: Hinzufügen der [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) als eine Möglichkeit, Datumsfelder einzugeben.

### <a name="getting-started"></a>Erste Schritte

Wenn Sie die Anwendung Film-Angebot aus dem Startprojekt noch nicht haben, downloadmöglichkeit finden Sie über den folgenden Link: [herunterladen](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800). Klicken Sie dann im Windows-Explorer mit der Maustaste die *MvcMovie.zip* Datei, und wählen Sie **Eigenschaften**. In der **MvcMovie.zip Eigenschaften** wählen Sie im Dialogfeld **zum Aufheben der Sperre**. (Zum Aufheben der Blockierung verhindert, dass eine sicherheitswarnung angezeigt, das auftritt, wenn Sie versuchen, eine *ZIP* -Datei, die Sie aus dem Web heruntergeladen haben.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Mit der rechten Maustaste die *MvcMovie.zip* Datei, und wählen Sie **alle extrahieren** um die Datei zu entpacken. Öffnen Sie in Visual Studio 2010 oder Visual Web Developer die *MvcMovieCS\_TU.sln* Datei.

In **Projektmappen-Explorer**, doppelklicken Sie auf die *Views\Shared\\_Layout.cshtml* um ihn zu öffnen. Ändern der `H1` -Header **MVC Film-App** auf **Film jQuery**. Drücken Sie STRG + F5, um die Anwendung auszuführen, und klicken Sie auf die **Home** Registerkarte, um benötigen die `Index` -Methode des Controllers Film. Um die Anwendung testen, wählen Sie die **bearbeiten** Link und der **Details** Link für eines der Filme. Beachten Sie, dass in den Ansichten Index, bearbeiten und Details der Veröffentlichungsdatum und Preis ordentlich formatiert werden:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

Die Formatierung für das Datum und der Preis ist das Ergebnis der Verwendung der [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) -Attribut auf Eigenschaften von der `Movie` Klasse.

Öffnen der *Movie.cs* Datei, und kommentieren Sie dann die `DisplayFormat` -Attribut auf die `ReleaseDate` und `Price` Eigenschaften. Das resultierende `Movie` Klasse sieht wie folgt aus:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Drücken Sie STRG + F5, um die Anwendung auszuführen, und wählen Sie die **Home** Tab, um die Filmliste anzuzeigen. Dieses Mal das Veröffentlichungsdatum zeigt das Datum und die Uhrzeit und in diesem Preisfeld wird nicht mehr das Währungssymbol. Die Änderung in der `Movie` Klasse die rückgängig gemacht wurde, die schöne Formatierung, den Sie zuvor gesehen haben, aber Sie korrigieren dies in wenigen Augenblicken.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Verwenden der DataAnnotations DataType-Attributs, um den Datentyp anzugeben

Ersetzen Sie den auskommentierten `DisplayFormat` -Attribut für die `ReleaseDate` Eigenschaft mit der [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) -Attribut mit der `Date` Enumeration. Ersetzen der `DisplayFormat` -Attribut für die `Price` Eigenschaft mit der [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) -Attribut erneut, diese Zeit mit der `Currency` Enumeration. Dies ist der vollständige Code ähnelt:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Führen Sie die Anwendung aus. Nun werden das Veröffentlichungsdatum und Preis Eigenschaften richtig formatiert (die entsprechenden Formate für Datum und Währung verwendet). Die [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) Attribut Typmetadaten für den integrierten ASP.NET MVC stellt Vorlagen bereit, damit die Felder im richtigen Format rendern. Mithilfe der `DataType` Attribut ist vorzuziehen, mit der `DisplayFormat` -Attribut, das ursprünglich im Code, da wurde die `DataType` Attribut macht das Modell für Zwecke wie Internationalisierung flexibler und übersichtlicher.

Im nächsten Abschnitt sehen Sie, wie Sie benutzerdefinierte Vorlagen Date-Felder angezeigt werden sollen.

>[!div class="step-by-step"]
[Nächste](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
