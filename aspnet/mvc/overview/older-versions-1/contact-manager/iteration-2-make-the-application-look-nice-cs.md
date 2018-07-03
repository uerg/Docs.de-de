---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: 'Iteration #2 – Optimieren der Anwendung gut (c#) | Microsoft-Dokumentation'
author: microsoft
description: In dieser Iteration verbessern wir die Darstellung der Anwendung durch Ändern der Standard-Masterseite für ASP.NET MVC-Ansicht und cascading Stylesheet.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: e78383516c752748da67f058c37aeb66d7004707
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377928"
---
<a name="iteration-2--make-the-application-look-nice-c"></a>Iteration #2 – Optimieren der Anwendung gut (c#)
====================
durch [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> In dieser Iteration verbessern wir die Darstellung der Anwendung durch Ändern der Standard-Masterseite für ASP.NET MVC-Ansicht und cascading Stylesheet.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Erstellen einer Kontaktverwaltung ASP.NET MVC-Anwendung (c#)
  

In dieser Reihe von Tutorials erstellen wir eine gesamte Anwendung Kontaktverwaltung ab Beginn um den Vorgang abzuschließen. Die Contact Manager-Anwendung können Sie zum Speichern von Kontaktinformationen - Namen, Telefonnummern und e-Mail-Adressen – eine Liste der Personen an.

Wir erstellen die Anwendung über mehrere Iterationen. Bei jeder Iteration verbessern wir nach und nach der Anwendung. Das Ziel dieses mehrere Iteration-Ansatzes ist, um den Grund für jede Änderung verstehen können.

- Iteration #1 – Erstellen der Anwendung. In der ersten Iteration wir der Contact Manager in der einfachsten maximal möglicher Größe erstellen. Wir fügen Unterstützung für grundlegender Datenbankvorgänge: erstellen, lesen, aktualisieren und löschen (CRUD).

- Iteration #2 – Optimieren der Anwendung gut. In dieser Iteration verbessern wir die Darstellung der Anwendung durch Ändern der Standard-Masterseite für ASP.NET MVC-Ansicht und cascading Stylesheet.

- Iteration #3 – Hinzufügen der formularüberprüfung. In der dritten Iteration fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Formulars ohne erforderlichen Felder des Formulars abzuschließen. Wir überprüfen auch die e-Mail-Adressen und Telefonnummern.

- Stellen Sie Iteration #4 – lose koppeln der Anwendung. In dieser dritten Iteration nutzen wir einige Entwurfsmuster für Software zu verwalten und ändern Sie die Kontakt-Manager-Anwendung zu vereinfachen. Z. B. gestalten wir unsere Anwendung, die dem Repositorymuster und dem Dependency Injection-Muster verwenden.

- Iteration #5 – Erstellen von Komponententests. In der fünften Iteration stellen wir unsere Anwendung einfacher zu verwalten und zu ändern, indem Sie die Komponententests hinzufügen. Wir unsere Data Model-Klassen modellieren und erstellen Sie Komponententests für unseren Controller und die Validierungslogik.

- Iteration #6 – Verwenden der testgesteuerten Entwicklung. In dieser Iteration sechsten fügen wir neue Funktionen zu unserer Anwendung zuerst Schreiben von Komponententests und Code für die Komponententests schreiben hinzu. In dieser Iteration fügen wir wenden Sie sich an Gruppen hinzu.

- Iteration #7 - Ajax-Funktionen hinzufügen. In der siebten Iteration verbessern wir die Reaktionsfähigkeit und Leistung der Anwendung durch Hinzufügen von Unterstützung für Ajax.

## <a name="this-iteration"></a>Diese Iteration

Das Ziel dieser Iteration ist, um die Darstellung der Contact Manager-Anwendung zu verbessern. Contact Manager wird derzeit verwendet, die standardmäßige ASP.NET MVC-Ansichtsseite master und cascading Stylesheet (siehe Abbildung 1). Diese Don ' t ist nach dem, aber ich nicht t möchten Contact Manager, genau wie jede andere ASP.NET MVC-Website zu suchen. Ich möchte diese Dateien mit benutzerdefinierten Dateien zu ersetzen.


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Abbildung 01**: die standarddarstellung der einer ASP.NET MVC-Anwendung ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-2-make-the-application-look-nice-cs/_static/image2.png))


In dieser Iteration erläutere ich zwei Ansätze zum Verbessern der des visuellen Entwurfs der Anwendung. Zunächst erfahren Sie, wie ASP.NET MVC-Entwurf Katalog aus, um eine kostenlose ASP.NET MVC-Design-Vorlage herunterladen nutzen. Die ASP.NET MVC Design Gallery ermöglicht Ihnen die Erstellung eine professionellen Web-Anwendung, ohne eine Aufgabe auszuführen.

Ich habe mich entschieden, eine Vorlage aus dem Katalog der ASP.NET MVC-Entwurf für die Kontakt-Manager-Anwendung nicht zu verwenden. Stattdessen musste ich einen benutzerdefinierten Entwurf entwickeln, die von den Profis erstellt. Im zweiten Teil dieses Lernprogramms wird erläutert, wie ich bei einem professionelles Design-Unternehmen, um den endgültigen Entwurf für die ASP.NET MVC erstellen funktioniert hat.

## <a name="the-aspnet-mvc-design-gallery"></a>Der ASP.NET MVC-Design-Gallery

Die ASP.NET MVC-Design-Katalog ist eine kostenlose Ressource, die von Microsoft bereitgestellt werden. Die ASP.NET MVC-Katalog befindet sich unter folgender Adresse:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

Die ASP.NET MVC-Design-Katalog hostet eine Sammlung von kostenlose Website-Entwürfe, die speziell für die Verwendung in einem ASP.NET MVC-Projekt erstellt wurden. Designs werden von Mitgliedern der Community hochgeladen. Besucher im Katalog für ihre bevorzugten Entwürfe darüber abstimmen können (siehe Abbildung 2).


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Abbildung 02**: die ASP.NET MVC-Design-Gallery ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-2-make-the-application-look-nice-cs/_static/image4.png))


Wie ich in diesem Tutorial schreiben, ist der am häufigsten verwendete Entwurf im Katalog ein Entwurf, Oktober von David Hauser benannt wird. Sie können dieses Entwurfs für ein ASP.NET MVC-Projekt verwenden, durch die folgenden Schritte aus:

1. Klicken Sie auf die **herunterladen** Schaltfläche, um die October.zip-Datei auf Ihren Computer herunterladen.
2. Mit der rechten Maustaste in der heruntergeladenen October.zip-Datei, und klicken Sie auf die **Unblock** Schaltfläche (siehe Abbildung 3).
3. Entzippen Sie die Datei in einen Ordner namens Oktober aus.
4. Wählen Sie alle Dateien aus dem DesignTemplate-Ordner, in der Oktober-Ordner enthalten, mit der rechten Maustaste in der Dateien und wählen Sie die Menüoption **Kopie**.
5. Mit der rechten Maustaste des Knotens der ContactManager-Projekt im Projektmappen-Explorer von Visual Studio-Fenster, und wählen Sie die Menüoption **einfügen** (siehe Abbildung 4).
6. Wählen Sie die Visual Studio-Menüoption **bearbeiten, suchen und ersetzen, Schnellersetzung** , und Ersetzen Sie *[MyProjectName]* mit *ContactManager* (siehe Abbildung 5).


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Abbildung 03**: Aufhebung der Blockierung einer Datei aus dem Internet heruntergeladene ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-2-make-the-application-look-nice-cs/_static/image6.png))


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Abbildung 04**: Überschreiben von Dateien im Projektmappen-Explorer ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-2-make-the-application-look-nice-cs/_static/image8.png))


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Abbildung 05**: Ersetzen [Projektname] durch ContactManager ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-2-make-the-application-look-nice-cs/_static/image10.png))


Nachdem Sie diese Schritte abgeschlossen haben, wird Ihre Webanwendung das neue Design verwenden. Die Seite in Abbildung 6 zeigt die Darstellung der Contact Manager-Anwendung mit der Oktober-Entwurf.


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Abbildung 06**: ContactManager mit der Oktober-Vorlage ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-2-make-the-application-look-nice-cs/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Erstellen eines benutzerdefinieren ASP.NET MVC-Entwurfs

Die ASP.NET MVC-Design-Katalog verfügt über eine gute Auswahl von verschiedenen entwurfsstile. Der Katalog bietet Ihnen eine problemlose Möglichkeit anpassen die Darstellung von ASP.NET MVC-Anwendungen. Und natürlich der Katalog enthält den großen Vorteil, völlig kostenlos.

Allerdings müssen Sie ein völlig Design für Ihre Website erstellen. In diesem Fall Sie es sinnvoll, ein Website-Entwurfs-Unternehmen gearbeitet. Ich beschloss, diesen Ansatz für den Entwurf für die Kontakt-Manager-Anwendung verwenden.

Ich ZIP Einrichten der Contact Manager aus Iteration 1 und der designbüros des Projekts an. Visual Studio (schade darauf!), deren Besitzer sie nicht, aber hat t ein Problem darstellen. Sie konnten für den kostenlosen download von Microsoft Visual Web Developer aus der [ https://www.asp.net ](https://www.asp.net) -Website und öffnen Sie die Kontakt-Manager-Anwendung in Visual Web Developer. In einigen Tagen mussten sie den Entwurf in Abbildung 7 erstellt.


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Abbildung 07**: die ASP.NET MVC Contact Manager-Entwurf ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-2-make-the-application-look-nice-cs/_static/image14.png))


Das neue Design Bestand aus zwei Hauptdateien: ein neues CSS-Datei und eine neue Masterseitendatei anzeigen. Eine Masterseite für die Ansicht enthält das Layout und die freigegebene Inhalte für Ansichten in ASP.NET MVC-Anwendungen. Die Masterseite für die Ansicht enthält z. B. Header, Navigationsregisterkarten und Fußzeile, die angezeigt werden in Abbildung 7. Ich überschreibt die vorhandene Ansicht Stammwebsite im Ordner "Views\Shared" mit der neuen Site.Master-Datei aus dem Unternehmen entwerfen,

Das Unternehmen Entwurf erstellt auch eine neue cascading Stylesheets und einen Satz von Bildern. Ich habe diese neuen Dateien in den Ordner "Content" platziert und überschrieben wurden der vorhandene Datei "Site.CSS". Sie sollten alle statischen Inhalte im Inhaltsordner platzieren.

Beachten Sie, dass das neue Design für den Kontakt-Manager-Images für bearbeiten und Löschen von Kontakten enthält. Ein Bild bearbeiten und löschen, die neben einzelnen Kontakte in der HTML-Tabelle der Kontakte angezeigt werden.

Ursprünglich diese Links, die mit dem HTML gerendert wurden. ActionLink() Hilfsprogramm wie folgt:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

Die Methode Html.ActionLink() unterstützt keine Bilder (die HTML-Methode codiert den Text des Links aus Sicherheitsgründen). Aus diesem Grund ersetzt ich die Aufrufe an Html.ActionLink() durch Aufrufe von Url.Action() wie folgt:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

Die Html.ActionLink()-Methode rendert eines gesamten HTML-Links. Rendert die Url.Action()-Methode, auf der anderen Seite einfach die URL ohne den &lt;eine&gt; Tag.

Beachten Sie außerdem, dass das neue Design ausgewählte und nicht ausgewählte Registerkarten enthält. Zum Beispiel in Abbildung 8: die **neuen Kontakt erstellen** Registerkarte ausgewählt ist und die **Meine Kontakte** Registerkarte nicht ausgewählt ist.


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Abbildung 08**: aktiviert und deaktiviert Sie Registerkarten ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-2-make-the-application-look-nice-cs/_static/image16.png))


Um Registerkarten für ausgewählte und nicht ausgewählte rendering zu unterstützen, erstellt habe ich eine benutzerdefinierte HTML-Hilfsobjekt, das mit dem Namen der MenuItemHelper. Diese Hilfsmethode rendert entweder eine &lt;li&gt; Tag oder eine &lt;li-Klasse = "ausgewählt"&gt; Tag, je nachdem, ob der aktuelle Controller und Aktion entspricht dem Controller- und Aktionsnamen Namen übergeben an das Hilfsprogramm. Der Code für die MenuItemHelper befindet sich in Codebeispiel 1.

**1 – Helpers\MenuItemHelper.cs auflisten**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

Die MenuItemHelper verwendet die TagBuilder-Klasse intern zum Erstellen der &lt;li&gt; HTML-Tag. TagBuilder-Klasse ist eine sehr nützlicher Dienstprogrammfunktionen-Klasse, die Sie verwenden können, wann immer Sie benötigen, um ein neues HTML-Tag zu erstellen. Sie enthält Methoden zum Hinzufügen von Attributen, Hinzufügen von CSS-Klassen, Generieren von Ids und ändern das Tag s innere HTML-Code.

## <a name="summary"></a>Zusammenfassung

In dieser Iteration verbessert es den visuellen Entwurf Ihrer ASP.NET MVC-Anwendung. Zunächst wurden Sie in der ASP.NET MVC-Design-Gallery eingeführt. Haben Sie gelernt, kostenlose Entwurfsvorlagen aus dem ASP.NET MVC-Design-Katalog herunterladen, die Sie in ASP.NET MVC-Anwendungen verwenden können.

Als Nächstes wird erläutert, wie Sie einen benutzerdefinierten Entwurf entwickeln durch Ändern der standardmäßigen CSS-Datei und die Auslagerungsdatei Masteransicht erstellen können. Um das neue Design unterstützen zu können, mussten wir einige kleinen Änderungen an unseren Contact Manager-Anwendung vornehmen. Beispielsweise haben wir eine neue HTML-Hilfsobjekt, das mit dem Namen der MenuItemHelper, die ausgewählte und nicht ausgewählte Registerkarten anzeigt hinzugefügt.

In der nächsten Iteration behandeln wir die sehr wichtigen Thema der Überprüfung. Wir hinzufügen Validierungscode, damit ein Benutzer keine neuen Kontakt erstellen, ohne zuerst Angabe der erforderlichen Werte, z. B. eine Person s und Nachname zu unserer Anwendung.

> [!div class="step-by-step"]
> [Zurück](iteration-1-create-the-application-cs.md)
> [Weiter](iteration-3-add-form-validation-cs.md)
