---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Erstellen einen benutzerdefinierte AJAX steuern Toolkit Extendersteuerelement (c#) | Microsoft Docs
author: microsoft
description: "Benutzerdefinierte Extender ermöglichen es Ihnen, anpassen und erweitern die Funktionen der ASP.NET-Steuerelementen, ohne neue Klassen erstellen zu müssen."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 2ae03484dd1161c65b77f4718bb8cedb5abfdd82
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>Erstellen eine benutzerdefinierte AJAX-Steuerelement-Toolkit-Extendersteuerelement (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> Benutzerdefinierte Extender ermöglichen es Ihnen, anpassen und erweitern die Funktionen der ASP.NET-Steuerelementen, ohne neue Klassen erstellen zu müssen.


In diesem Lernprogramm erfahren Sie, wie eine benutzerdefinierte AJAX Control Toolkit Extendersteuerelement erstellen. Erstellen wir eine einfache, aber hilfreich, neue Extender, die deaktiviert bzw. aktiviert, wenn Sie Text in ein Textfeld eingeben, wird der Status einer Schaltfläche geändert. Nach dem Lesen dieses Lernprogramms, werden Sie können das ASP.NET AJAX-Toolkit mit Ihren eigenen-Extender erweitert.

Sie können benutzerdefinierte Steuerelement Extender mit Visual Studio oder Visual Web Developer erstellen (Stellen Sie sicher, dass Sie die neueste Version von Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Übersicht über den DisabledButton Extender

Unsere neue Extendersteuerelement heißt den DisabledButton Extender. Diesen Extender müssen drei Eigenschaften:

- TargetControlID - Textfeld, das das Steuerelement erweitert.
- TargetButtonIID - Schaltfläche, mit der aktiviert oder deaktiviert ist.
- DisabledText - der Text, der ursprünglich in der Schaltfläche angezeigt wird. Wenn Sie mit der Eingabe beginnen, zeigt die Schaltfläche den Wert der Eigenschaft Schaltflächentext.

Sie verknüpfen die DisabledButton Extender an ein Textfeld und Schaltfläche-Steuerelement. Bevor Sie einen beschreibenden Text eingeben, die Schaltfläche ist deaktiviert, und das Textfeld und einer Schaltfläche wie folgt aussehen:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))


Nach dem Text eingeben starten, wird die Schaltfläche aktiviert ist, und das Textfeld und einer Schaltfläche wie folgt aussehen:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))


Um unseren Extendersteuerelement erstellen zu können, müssen wir die folgenden drei Dateien zu erstellen:

- DisabledButtonExtender.cs – diese Datei ist der serverseitigen Control-Klasse, die Erstellen des Extenders verwalten und ermöglichen es Ihnen, die Eigenschaften zur Entwurfszeit festgelegt. Außerdem definiert die Eigenschaften, die für Ihre Extender festgelegt werden können. Diese Eigenschaften über den Code auch zur Entwurfszeit zugegriffen werden kann und in der Datei DisableButtonBehavior.js definierte Eigenschaften übereinstimmen.
- DisabledButtonBehavior.js – Diese Datei befindet, in dem Sie alle Ihre Client-Skript-Logik hinzugefügt werden.
- DisabledButtonDesigner.cs - kann diese Klasse Entwurfszeitfunktionen. Benötigen Sie diese Klasse, wenn das Extendersteuerelement ordnungsgemäß mit dem Visual Studio/Visual Web Developer-Designer ausgeführt werden soll.

Daher besteht ein Extendersteuerelement ein serverseitiges Steuerelement, ein Client-Side-Verhalten und einer serverseitigen Designerklasse. Erfahren Sie, wie Sie alle drei Dateien in den folgenden Abschnitten zu erstellen.

## <a name="creating-the-custom-extender-website-and-project"></a>Erstellen der benutzerdefinierten Extender Website und das Projekt

Der erste Schritt besteht im Visual Studio/Visual Web Developer eine Klassenbibliotheksprojekt und die Website erstellen. Wir ll benutzerdefinierte Extender in das Klassenbibliotheksprojekt erstellen und testen den benutzerdefinierten Extender auf der Website.

Lassen Sie s mit der Website zu starten. Führen Sie diese Schritte aus, um die Website zu erstellen:

1. Wählen Sie die Menüoption **Datei, die neue Website**.
2. Wählen Sie die **ASP.NET-Website** Vorlage.
3. Benennen Sie die neue Website *Website1*.
4. Klicken Sie auf die **OK** Schaltfläche.

Als Nächstes müssen wir das Klassenbibliotheksprojekt zu erstellen, das den Code für das Extendersteuerelement enthält:

1. Wählen Sie die Menüoption **Datei hinzufügen, neues Projekt**.
2. Wählen Sie die **-Klassenbibliothek** Vorlage.
3. Benennen Sie die neue Klassenbibliothek mit dem Namen **CustomExtenders**.
4. Klicken Sie auf die **OK** Schaltfläche.

Nachdem Sie diese Schritte abgeschlossen haben, sieht das Fenster Projektmappen-Explorer wie in Abbildung 1.


[![Lösung mit der Website und die Klasse Library-Projekts](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**Abbildung 01**: Projektmappe mit der Website und die Klasse Bibliotheksprojekt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))


Als Nächstes müssen Sie alle auf das Klassenbibliotheksprojekt die notwendigen Assemblyverweise hinzugefügt werden:

1. Mit der rechten Maustaste des CustomExtenders-Projekts, und wählen Sie die Menüoption **Verweis hinzufügen**.
2. Wählen Sie die Registerkarte ".NET".
3. Fügen Sie Verweise auf die folgenden Assemblys hinzu:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Wählen Sie die Registerkarte "Durchsuchen".
5. Fügen Sie einen Verweis auf die AjaxControlToolkit.dll-Assembly. Diese Assembly befindet sich im Ordner, in dem Sie das AJAX-Steuerelement-Toolkit heruntergeladen haben.

Nachdem Sie diese Schritte abgeschlossen haben, sollte der Ordner Klassenbibliothekprojekts Verweise wie in Abbildung 2 aussehen.


[![Ordner "Verweise" mit der erforderlichen Verweise](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**Abbildung 02**: Ordner "Verweise" mit der erforderlichen Verweise ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Erstellen die benutzerdefinierte Extendersteuerelement

Nun, da wir unsere Klassenbibliothek haben, beginnen wir unsere Extendersteuerelement erstellen. Lassen Sie s mit der Texten ein benutzerdefiniertes Extender-Steuerelementklasse (Siehe Programmbeispiel 1) beginnen.

**1 – MyCustomExtender.cs auflisten**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

Es gibt mehrere Dinge, die Sie zu der Extender Steuerelementklasse in Codebeispiel 1 fest. Erstens ist zu beachten, dass die Klasse von der Basisklasse ExtenderControlBase erbt. Alle AJAX Control Toolkit Extendersteuerelementen werden von dieser Basisklasse abgeleitet. Beispielsweise enthält die Basisklasse der TargetID-Eigenschaft, die eine erforderliche Eigenschaft des Extenders wird jedes Steuerelement ist.

Als Nächstes Beachten Sie, dass die Klasse die folgenden zwei Attribute, die im Zusammenhang mit Clientskripts umfasst:

- WebResource - bewirkt, dass eine Datei als eingebettete Ressource in einer Assembly enthalten sein.
- ClientScriptResource - bewirkt, dass eine Ressource "Script" aus einer Assembly abgerufen werden sollen.

Das WebResource-Attribut wird verwendet, um die MyControlBehavior.js JavaScript-Datei in die Assembly eingebettet, wenn der benutzerdefinierte Extender kompiliert wird. Das ClientScriptResource-Attribut wird verwendet, um das MyControlBehavior.js-Skript aus der Assembly abgerufen werden, wenn der benutzerdefinierte Extender in einer Webseite verwendet wird.


In der Reihenfolge für die Attribute WebResource und ClientScriptResource arbeiten müssen Sie die JavaScript-Datei als eingebettete Ressource kompilieren. Wählen Sie im Fenster Projektmappen-Explorer die Datei, öffnen Sie die Eigenschaftenseite, und weisen Sie den Wert *eingebettete Ressource* auf die **Buildvorgang** Eigenschaft.


Beachten Sie, dass der Steuerelement-Extender auch ein TargetControlType-Attribut enthält. Dieses Attribut wird verwendet, um den Typ des Steuerelements anzugeben, die durch das Extendersteuerelement erweitert wird. Bei einem Codebeispiel 1 ist das Extendersteuerelement verwendet, um ein Textfeld zu erweitern.

Beachten Sie, dass der benutzerdefinierte Extender eine Eigenschaft "MyProperty" enthält. Die Eigenschaft ist mit dem ExtenderControlProperty-Attribut markiert. Die Methoden GetPropertyValue() und SetPropertyValue() werden verwendet, den Wert der Eigenschaft aus der Extender serverseitiges Steuerelement an das Verhalten des clientseitigen übergeben.

Lassen Sie s, fahren Sie fort, und implementieren Sie den Code für unsere DisabledButton Extender. Der Code für diesen Extender finden Sie im Codebeispiel 2.

**Auflisten von 2 – DisabledButtonExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

Der Extender DisabledButton auflisten 2 verfügt über zwei Eigenschaften, die mit dem Namen TargetButtonID und DisabledText. Die IDReferenceProperty auf die Eigenschaft TargetButtonID angewendet verhindert, dass Sie etwas anderes als die ID des einem Schaltflächen-Steuerelement diese Eigenschaft zuweisen.

Die Attribute WebResource und ClientScriptResource zuordnen ein clientseitigen Verhaltens befindet sich in einer Datei namens DisabledButtonBehavior.js mit diesen Extender. Dieser JavaScript-Datei im nächsten Abschnitt erörtert.

## <a name="creating-the-custom-extender-behavior"></a>Erstellen das benutzerdefinierte Extender-Verhalten

Die Client-Side-Komponente des ein Extendersteuerelement ist ein Verhalten wird aufgerufen. Die eigentliche Logik zum Deaktivieren und aktivieren die Schaltfläche befindet sich im DisabledButton Verhalten. Der JavaScript-Code für das Verhalten ist im Codebeispiel 3 enthalten.

**3 – DisabledButton.js auflisten**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

Die JavaScript-Datei Auflisten von 3 enthält eine Client-Side-Klasse, die mit dem Namen DisabledButtonBehavior. Diese Klasse wird wie die serverseitige und enthält zwei Eigenschaften, die mit dem Namen TargetButtonID und DisabledText, die Sie zugreifen können, mithilfe von get\_TargetButtonID/Set\_TargetButtonID und\_DisabledText/Set\_ DisabledText.

Die Initialize()-Methode ordnet einen Keyup-Ereignis-Handler für das Verhalten der Target-Element. Jedes Mal, Sie geben Sie einen Buchstaben in das Textfeld ein, die mit diesem Verhalten verbundenen, führt das Keyup-Handler. Der Handler Keyup entweder aktiviert oder deaktiviert die Schaltfläche abhängig davon, ob das Textfeld mit dem Verhalten verknüpft einen beschreibenden Text enthält.

Denken Sie daran, dass Sie die JavaScript-Datei auflisten 3 als eingebettete Ressource kompilieren müssen. Wählen Sie im Fenster Projektmappen-Explorer die Datei, öffnen Sie die Eigenschaftenseite, und weisen Sie den Wert *eingebettete Ressource* auf die **Buildvorgang** -Eigenschaft (siehe Abbildung 3). Diese Option ist in Visual Studio und Visual Web Developer verfügbar.


[![Hinzufügen einer JavaScript-Datei als eingebettete Ressource](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**Abbildung 03**: Hinzufügen einer JavaScript-Datei als eingebettete Ressource ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Erstellen des benutzerdefinierten Extender-Designers

Es wird eine letzte-Klasse, die wir erstellen, um unsere Extender abschließen müssen. Wir müssen die Designerklasse auflisten 4 erstellen. Diese Klasse ist erforderlich, um den Extender verhält sich ordnungsgemäß mit dem Visual Studio/Visual Web Developer-Designer.

**4 – DisabledButtonDesigner.cs auflisten**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

Ordnen Sie den Designer in 4 Auflisten der DisabledButton Extender mit dem Designer-Attribut. Sie müssen das Designer-Attribut auf die Klasse DisabledButtonExtender wie folgt anwenden:

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>Mithilfe des benutzerdefinierten Extenders

Nun, dass wir das Extendersteuerelement DisabledButton erstellt haben, ist es Zeit für die Verwendung in unserer ASP.NET-Website. Zunächst müssen wir die benutzerdefinierte Extender zur Toolbox hinzu. Führen Sie folgende Schritte aus:

1. Öffnen Sie eine ASP.NET-Seite durch Doppelklicken auf die Seite im Projektmappen-Explorer-Fenster.
2. Mit der rechten Maustaste in der Toolbox, und wählen Sie die Menüoption **Elemente auswählen**.
3. Navigieren Sie zu der Assembly CustomExtenders.dll, klicken Sie im Dialogfeld "Toolboxelemente auswählen".
4. Klicken Sie auf die **OK** Schaltfläche, um das Dialogfeld zu schließen.

Nachdem Sie diese Schritte abgeschlossen haben, sollte die DisabledButton Extendersteuerelement in der Toolbox angezeigt (siehe Abbildung 4).


[![DisabledButton in der toolbox](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**Abbildung 04**: DisabledButton in der Toolbox ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))


Als Nächstes müssen wir eine neue ASP.NET-Seite zu erstellen. Führen Sie folgende Schritte aus:

1. Erstellen Sie eine neue, mit dem Namen ShowDisabledButton.aspx ASP.NET-Seite.
2. Ziehen Sie ein ScriptManager auf der Seite "ein.
3. Ziehen Sie ein TextBox-Steuerelement auf der Seite "ein.
4. Ziehen Sie ein Button-Steuerelement auf der Seite "ein.
5. Ändern Sie im Fenster Eigenschaften die Schaltfläche ID-Eigenschaft auf den Wert *hinzu* und der Text-Eigenschaft auf den Wert *speichern\**.
  

Wir haben eine Seite mit einem Standardsteuerelement ASP.NET Textfeld und Schaltfläche erstellt.

Als Nächstes müssen wir das TextBox-Steuerelement mit dem DisabledButton Extender erweitern:

1. Wählen Sie die **Extender hinzufügen** Aufgabe Option, um das Dialogfeld Extender-Assistenten zu öffnen (siehe Abbildung 5). Beachten Sie, dass das Dialogfeld "unsere benutzerdefinierte DisabledButton Extender enthält.
2. Wählen Sie den DisabledButton Extender, und klicken Sie auf die **OK** Schaltfläche.


[![Das Dialogfeld Extender-Assistenten](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**Abbildung 05**: der Extender-Assistent-Dialogfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))


Schließlich können wir die Eigenschaften des Extenders DisabledButton festlegen. Sie können die Eigenschaften des Extenders DisabledButton durch Ändern der Eigenschaften des Textfeld-Steuerelements ändern:

1. Wählen Sie im Designer das Textfeld ein.
2. Erweitern Sie im Fenster Eigenschaften die Extender-Knoten (siehe Abbildung 6).
3. Weisen Sie den Wert *speichern* DisabledText-Eigenschaft und der Wert *hinzu* der TargetButtonID-Eigenschaft.


[![Einstellungseigenschaften für extender](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**Abbildung 06**: Extendereigenschaften festlegen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))


Wenn Sie die Seite "(durch Drücken von F5) ausführen, ist anfangs das Schaltflächen-Steuerelement deaktiviert. Sobald Sie Text in das Textfeld eingeben starten, aktiviert die Schaltfläche mit dem Steuerelement befindet (siehe Abbildung 7).


[![Der Extender DisabledButton in Aktion](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**Abbildung 07**: die DisabledButton Extender in Aktion ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))


## <a name="summary"></a>Zusammenfassung

Das Ziel dieses Lernprogramms wurde zur Erläuterung, wie Sie das AJAX-Steuerelement-Toolkit mit benutzerdefinierten Extendersteuerelementen erweitern können. In diesem Lernprogramm haben wir eine einfache DisabledButton Extendersteuerelement erstellt. Wir implementiert diesen Extender durch Erstellen einer DisabledButtonExtender-Klasse, ein DisabledButtonBehavior JavaScript-Verhalten und einer DisabledButtonDesigner-Klasse. Sie führen Sie eine ähnliche Reihe von Schritten, wenn Sie einen benutzerdefiniertes Steuerelement Extender erstellen.

>[!div class="step-by-step"]
[Zurück](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
[Weiter](get-started-with-the-ajax-control-toolkit-vb.md)
