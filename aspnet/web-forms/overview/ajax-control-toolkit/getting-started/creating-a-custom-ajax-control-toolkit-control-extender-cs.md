---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Erstellen einen benutzerdefinierten AJAX Toolkit-Steuerelement-Extender (c#) Steuerelements | Microsoft-Dokumentation
author: microsoft
description: Benutzerdefiniertes Extender können Sie anpassen und erweitern die Funktionen von ASP.NET-Steuerelementen, ohne neue Klassen zu erstellen.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 0dd243ec1709324174ff48b52e7dfb5185d3aaa2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390952"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>Erstellen einen benutzerdefinierten AJAX Control Toolkit-Steuerelement-Extender (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> Benutzerdefiniertes Extender können Sie anpassen und erweitern die Funktionen von ASP.NET-Steuerelementen, ohne neue Klassen zu erstellen.


In diesem Tutorial erfahren Sie, wie Sie einen benutzerdefinierten AJAX Control Toolkit-Steuerelement-Extenders zu erstellen. Wir erstellen eine einfache, aber hilfreich, neue Extender, der den Zustand einer Schaltfläche, deaktiviert ändert bzw. aktiviert wird, wenn Sie Text in ein Textfeld eingeben. Nach diesem Tutorial werden Sie dem ASP.NET AJAX-Toolkit, mit Ihren eigenen-Extender erweitern können.

Sie können benutzerdefinierte Steuerelement-Extendern, die mit Visual Studio oder Visual Web Developer erstellen (Stellen Sie sicher, dass Sie die neueste Version von Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Übersicht über die der Extender DisabledButton

Unsere neuen Steuerelement-Extender wird den Extender DisabledButton benannt. Dieses Extenders müssen drei Eigenschaften:

- TargetControlID - Textfeld, das das Steuerelement erweitert.
- TargetButtonIID - Schaltfläche mit den, der aktiviert oder deaktiviert ist.
- DisabledText - der Text, der ursprünglich in der Schaltfläche angezeigt wird. Wenn Sie mit der Eingabe beginnen, zeigt die Schaltfläche den Wert der Schaltflächentext-Eigenschaft.

Sie verknüpfen die DisabledButton Extender an ein TextBox- und Button-Steuerelement. Bevor Sie einen beliebigen Text eingeben, die Schaltfläche ist deaktiviert, und das Textfeld und die Schaltfläche wie folgt aussehen:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([Klicken Sie, um das Bild in voller Größe anzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))


Eingeben von Text an, wenn die Schaltfläche ist aktiviert und die Textfeld- und Schaltflächen wie folgt aussehen:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([Klicken Sie, um das Bild in voller Größe anzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))


Um unsere Extender-Steuerelements erstellen zu können, müssen wir die folgenden drei Dateien zu erstellen:

- DisabledButtonExtender.cs – diese Datei ist das serverseitige Steuerelement-Klasse, die verwalten, erstellen den Extender und ermöglichen es Ihnen, die die Eigenschaften zur Entwurfszeit festgelegt. Sie definiert auch die Eigenschaften, die für den Extender festgelegt werden können. Diese Eigenschaften über den Code auch zur Entwurfszeit zugegriffen werden kann und in der Datei DisableButtonBehavior.js definierte Eigenschaften übereinstimmen.
- DisabledButtonBehavior.js – Diese Datei ist, in dem Sie alle Ihre Client-Skriptlogik hinzufügen.
- DisabledButtonDesigner.cs - ermöglicht diese Klasse während der Entwurfszeit-Funktionalität. Wenn die Extender-Steuerelements mit dem Visual Studio/Visual Web Developer-Designer ordnungsgemäß ausgeführt werden sollen, benötigen Sie diese Klasse.

Daher besteht aus ein Steuerelement-Extender ein serverseitiges Steuerelement, ein clientseitiges Verhalten und einer serverseitigen-Designer-Klasse. Erfahren Sie, wie Sie alle drei dieser Dateien in den folgenden Abschnitten erstellen.

## <a name="creating-the-custom-extender-website-and-project"></a>Erstellen der benutzerdefinierten Extender-Website und das Projekt

Der erste Schritt ist die Erstellung von einem Klassenbibliotheksprojekt und die Website in Visual Studio/Visual Web Developer. Wir ll den benutzerdefinierten Extender in das Klassenbibliotheksprojekt zu erstellen und Testen Sie auf der Website den benutzerdefinierten Extender.

Lassen Sie s, die mit der Website zu starten. Um die Website erstellt haben, gehen Sie wie folgt vor:

1. Wählen Sie die Menüoption **-Datei, die neue Website**.
2. Wählen Sie die **ASP.NET-Website** Vorlage.
3. Benennen Sie die neue Website *Website1*.
4. Klicken Sie auf die **OK** Schaltfläche.

Als Nächstes müssen wir das Klassenbibliotheksprojekt zu erstellen, das den Code für den Steuerelement-Extender enthält:

1. Wählen Sie die Menüoption **Datei hinzufügen "," Neues Projekt**.
2. Wählen Sie die **Klassenbibliothek** Vorlage.
3. Benennen Sie die neue Klassenbibliothek mit dem Namen **CustomExtenders**.
4. Klicken Sie auf die **OK** Schaltfläche.

Nachdem Sie diese Schritte abgeschlossen haben, sieht das Fenster des Projektmappen-Explorer wie in Abbildung 1.


[![Lösung mit Website "und"-Klasse-Steuerelementbibliothek-Projekt](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**Abbildung 01**: Projektmappe mit der Website und Klasse-Bibliotheksprojekt ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))


Als Nächstes müssen Sie die notwendigen Assemblyverweise, um das Klassenbibliotheksprojekt hinzugefügt:

1. Mit der rechten Maustaste in des CustomExtenders-Projekts, und wählen Sie die Menüoption **Verweis hinzufügen**.
2. Wählen Sie die Registerkarte für .NET.
3. Fügen Sie Verweise auf die folgenden Assemblys hinzu:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Wählen Sie die Registerkarte "Durchsuchen".
5. Fügen Sie einen Verweis auf die AjaxControlToolkit.dll-Assembly hinzu. Diese Assembly befindet sich im Ordner, in dem Sie das AJAX Control Toolkit heruntergeladen haben.

Nachdem Sie diese Schritte abgeschlossen haben, sollte Ihr Ordner Klassenbibliothekprojekts Verweise wie in Abbildung 2 aussehen.


[![Ordner "Verweise" mit der erforderlichen Verweise](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**Abbildung 02**: Ordner "Verweise" mit allen erforderlichen Referenzen ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Erstellen den benutzerdefinierte Steuerelement-Extender

Nun, da wir unsere Klassenbibliothek haben, können wir beginnen, unsere Extender-Steuerelement zu erstellen. Lassen Sie s, die mit den Texten, der eine benutzerdefinierte extendersteuerelementklasse (siehe Codebeispiel 1) beginnen.

**1 – MyCustomExtender.cs auflisten**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

Es gibt mehrere Dinge, die Sie über die Steuerelement-Extender-Klasse in Codebeispiel 1 zu sehen. Erstens ist zu beachten, dass die Klasse von der ExtenderControlBase Basisklasse erbt. Alle AJAX Control Toolkit-Extender-Steuerelemente, die von dieser Basisklasse abgeleitet werden. Beispielsweise enthält die Basisklasse der TargetID-Eigenschaft, die eine erforderliche Eigenschaft von jedem Steuerelement-Extender ist.

Als Nächstes, beachten Sie, dass die Klasse die folgenden beiden Attribute, die im Zusammenhang mit dem Clientskript enthält:

- WebResource - bewirkt, dass eine Datei als eingebettete Ressource in einer Assembly eingeschlossen werden sollen.
- ClientScriptResource - bewirkt, dass eine Skriptressource aus einer Assembly abgerufen werden sollen.

Das WebResource-Attribut wird verwendet, um MyControlBehavior.js JavaScript-Datei in die Assembly eingebettet werden, wenn die benutzerdefinierten Extenders kompiliert wird. Das ClientScriptResource-Attribut wird verwendet, um das Skript MyControlBehavior.js aus der Assembly abzurufen, wenn es sich bei der benutzerdefinierte Extender auf einer Webseite verwendet wird.


In der Reihenfolge für die Attribute WebResource- und ClientScriptResource funktioniert müssen Sie die JavaScript-Datei als eingebettete Ressource kompilieren. Wählen Sie die Datei im Projektmappen-Explorer-Fenster, öffnen Sie das Eigenschaftenblatt, und weisen Sie den Wert *eingebettete Ressource* auf die **Buildvorgang** Eigenschaft.


Beachten Sie, dass der Steuerelement-Extender auch ein TargetControlType-Attribut enthält. Dieses Attribut wird verwendet, um den Typ des Steuerelements anzugeben, die vom Steuerelement-Extender erweitert wird. Im Fall von Codebeispiel 1 wird die Extender-Steuerelements verwendet, um ein Textfeld zu erweitern.

Beachten Sie, dass es sich bei der benutzerdefinierte Extender eine Eigenschaft, die mit dem Namen MyProperty enthält. Die Eigenschaft wird mit dem ExtenderControlProperty-Attribut markiert. Die GetPropertyValue() und SetPropertyValue()-Methoden werden verwendet, um den Wert der Eigenschaft aus der serverseitiges Steuerelement-Extender für das clientseitige Verhalten zu übergeben.

Lassen Sie s, fahren Sie fort, und implementieren Sie den Code für unsere DisabledButton-Extender. Der Code für diesen Extender finden Sie im Codebeispiel 2.

**Codebeispiel 2 - DisabledButtonExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

Der Extender DisabledButton Programmausdruck 2 verfügt über zwei Eigenschaften namens TargetButtonID und DisabledText. Die IDReferenceProperty angewendet auf die TargetButtonID-Eigenschaft verhindert, dass Sie diese Eigenschaft nichts anderes als die ID eines Schaltflächen-Steuerelements zugewiesen.

Die Attribute WebResource- und ClientScriptResource ordnen Sie ein clientseitiges Verhalten befindet sich in einer Datei namens DisabledButtonBehavior.js mit diesen Extender. Diese JavaScript-Datei im nächsten Abschnitt erläutert.

## <a name="creating-the-custom-extender-behavior"></a>Erstellen das Verhalten des benutzerdefinierten Extenders

Die clientseitige Komponente von einem Steuerelement-Extender wird ein Verhalten aufgerufen werden. Die eigentliche Logik zum Deaktivieren und aktivieren die Schaltfläche befindet sich im DisabledButton Verhalten. Der JavaScript-Code für das Verhalten ist in Programmausdruck 3 enthalten.

**Codebeispiel 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

Die JavaScript-Datei in Programmausdruck 3 enthält eine Client-Side-Klasse, die mit dem Namen DisabledButtonBehavior. Diese Klasse wird wie seinem zwilling serverseitige enthält zwei Eigenschaften, die mit dem Namen TargetButtonID, und erhalten Sie DisabledText, die Sie zugreifen können, mit\_TargetButtonID/Set-\_TargetButtonID und\_DisabledText/Set-\_ DisabledText.

Die Initialize()-Methode Target-Elements für das Verhalten einen Keyup-Ereignis-Handler zugeordnet. Jedes Mal, Sie geben Sie einen Buchstaben in das Textfeld ein, die dieses Verhalten zugeordnet, führt der Keyup-Handler aus. Der Keyup-Handler entweder aktiviert oder deaktiviert die Schaltfläche ", je nachdem, ob das Textfeld ein, die mit diesem Verhalten verbundenen Text enthält.

Denken Sie daran, dass Sie die JavaScript-Datei in Programmausdruck 3 als eingebettete Ressource kompilieren müssen. Wählen Sie die Datei im Projektmappen-Explorer-Fenster, öffnen Sie das Eigenschaftenblatt, und weisen Sie den Wert *eingebettete Ressource* auf die **Buildvorgang** Eigenschaft (siehe Abbildung 3). Diese Option ist in Visual Studio und Visual Web Developer verfügbar.


[![Hinzufügen einer JavaScript-Datei als eingebettete Ressource](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**Abbildung 03**: Hinzufügen einer JavaScript-Datei als eingebettete Ressource ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Erstellen von benutzerdefinierten Extender-Designer

Es gibt eine letzte-Klasse, die wir erstellen, um unsere Extender abschließen müssen. Wir müssen die Designerklasse in Listing 4 zu erstellen. Diese Klasse wird benötigt, zu der Extender, der mit dem Visual Studio/Visual Web Developer-Designer ordnungsgemäß verhält.

**Programmausdruck 4 - DisabledButtonDesigner.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

Ordnen Sie den Designer in Listing 4 der DisabledButton Extender mit dem Designer-Attribut. Sie müssen das Designer-Attribut auf die DisabledButtonExtender-Klasse wie folgt angewendet:

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>Verwendung des benutzerdefinierten Extenders

Nun, dass wir mit dem Erstellen des DisabledButton-Extenders-Steuerelements abgeschlossen haben, ist es Zeit für die Verwendung in unserem ASP.NET-Website. Zunächst müssen wir den benutzerdefinierten Extender zur Toolbox hinzufügen. Führen Sie folgende Schritte aus:

1. Öffnen Sie eine ASP.NET-Seite durch Doppelklicken auf die Seite im Projektmappen-Explorer-Fenster.
2. Mit der rechten Maustaste in der Toolbox, und wählen Sie die Menüoption **Elemente auswählen**.
3. Navigieren Sie in das Dialogfeld "Toolboxelemente auswählen" auf die Assembly CustomExtenders.dll.
4. Klicken Sie auf die **OK** Schaltfläche, um das Dialogfeld zu schließen.

Nachdem Sie diese Schritte abgeschlossen haben, sollte die DisabledButton-Extender-Steuerelements in der Toolbox angezeigt (siehe Abbildung 4).


[![DisabledButton in der toolbox](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**Abbildung 04**: DisabledButton in der Toolbox ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))


Als Nächstes müssen wir eine neue ASP.NET-Seite erstellen. Führen Sie folgende Schritte aus:

1. Erstellen Sie eine neue ASP.NET-Seite mit dem Namen ShowDisabledButton.aspx.
2. Ziehen Sie ein ScriptManager-Steuerelement auf der Seite ein.
3. Ziehen Sie ein TextBox-Steuerelement auf der Seite ein.
4. Ziehen Sie ein Schaltflächen-Steuerelement auf der Seite ein.
5. Ändern Sie im Fenster Eigenschaften die Schaltflächen-ID-Eigenschaft auf den Wert <em>BtnSave</em> und die Text-Eigenschaft auf den Wert *speichern\**.
  

Wir haben eine Seite mit einem standardmäßigen ASP.NET TextBox- und Button-Steuerelement erstellt.

Als Nächstes müssen wir das TextBox-Steuerelement mit dem DisabledButton-Extender erweitern:

1. Wählen Sie die **Extender hinzufügen** aufgabenoption, um die Extender-Assistent-Dialogfeld zu öffnen (siehe Abbildung 5). Beachten Sie, dass das Dialogfeld unserer benutzerdefinierten DisabledButton Extenders enthält.
2. Wählen Sie den Extender DisabledButton aus, und klicken Sie auf die **OK** Schaltfläche.


[![Die Extender-Assistent-Dialogfeld](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**Abbildung 05**: die Extender-Assistent-Dialogfeld ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))


Schließlich können wir die Eigenschaften des Extenders DisabledButton festlegen. Sie können die Eigenschaften des Extenders DisabledButton ändern, durch Ändern der Eigenschaften der TextBox-Steuerelement:

1. Wählen Sie im Designer das Textfeld ein.
2. Erweitern Sie im Fenster Eigenschaften die Extender-Knoten (siehe Abbildung 6).
3. Weisen Sie den Wert *speichern* die DisabledText-Eigenschaft und der Wert *BtnSave* der TargetButtonID-Eigenschaft.


[![Festlegen von Eigenschaften des Extenders](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**Abbildung 06**: Festlegen von Eigenschaften des Extenders ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))


Wenn Sie die Seite (durch Drücken von F5) ausführen, ist das Schaltflächen-Steuerelement ursprünglich deaktiviert. Sobald Sie beginnen das Eingeben von Text in das Textfeld ein, aktiviert die Schaltfläche "Steuerelement" (siehe Abbildung 7).


[![Die DisabledButton-Extender in Aktion](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**Abbildung 07**: die DisabledButton-Extender in Aktion ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))


## <a name="summary"></a>Zusammenfassung

Das Ziel in diesem Tutorial wurde erläutert, wie Sie das AJAX Control Toolkit mit benutzerdefinierten Extender-Steuerelemente erweitern können. In diesem Tutorial haben wir einen einfache DisabledButton-Extender-Steuerelements erstellt. Wir haben dieses Extenders durch Erstellen einer DisabledButtonExtender-Klasse, ein DisabledButtonBehavior JavaScript-Verhalten und einer DisabledButtonDesigner-Klasse implementiert. Sie führen Sie eine ähnliche Reihe von Schritten, wenn Sie einen benutzerdefiniertes Steuerelement-Extender erstellen.

> [!div class="step-by-step"]
> [Zurück](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [Weiter](get-started-with-the-ajax-control-toolkit-vb.md)
