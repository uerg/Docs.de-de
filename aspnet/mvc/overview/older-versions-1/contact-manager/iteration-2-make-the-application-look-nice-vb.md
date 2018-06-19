---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
title: 'Iteration #2 – Stellen Sie die Anwendung sucht nice (VB) | Microsoft Docs'
author: microsoft
description: In dieser Iteration haben wir die Darstellung der Anwendung verbessern, indem Ändern der Standard-Gestaltungsvorlage für ASP.NET MVC-Ansicht und cascading Stylesheet.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f65cb436-e493-46fd-9608-384b27385aa1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
msc.type: authoredcontent
ms.openlocfilehash: 8545351b099e52533789b372903cd493f533f834
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872766"
---
<a name="iteration-2--make-the-application-look-nice-vb"></a>Iteration #2 – Stellen Sie die Anwendung nice (VB) suchen
====================
durch [Microsoft](https://github.com/microsoft)

[Herunterladen von Code](iteration-2-make-the-application-look-nice-vb/_static/contactmanager_2_vb1.zip)

> In dieser Iteration haben wir die Darstellung der Anwendung verbessern, indem Ändern der Standard-Gestaltungsvorlage für ASP.NET MVC-Ansicht und cascading Stylesheet.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Erstellen einer Kontaktverwaltung ASP.NET MVC-Anwendung (VB)
  

In dieser Reihe von Lernprogrammen erstellen wir eine vollständige Contact Management-Anwendung von Grund auf Fertig stellen. Die Kontakt-Manager-Anwendung können Sie Kontaktinformationen – Namen "," Telefon Telefonnummern und e-Mail-Adressen – eine Liste der Personen zu speichern.

Erstellen der Anwendung über mehrere Iterationen. Bei jeder Iteration verbessern wir allmählich die Anwendung. Das Ziel dieses Ansatzes für mehrere Iteration ist, um den Grund für jede Änderung verstehen können.

- Iteration #1 – Erstellen der Anwendung. In der ersten Iteration erstellen wir die Kontakt-Manager in der einfachsten möglich. Wir fügen die Unterstützung für die grundlegenden Datenbankvorgängen: erstellen, lesen, aktualisieren und löschen (CRUD).

- Iteration #2 – Stellen Sie die Anwendung gut aussehen. In dieser Iteration haben wir die Darstellung der Anwendung verbessern, indem Ändern der Standard-Gestaltungsvorlage für ASP.NET MVC-Ansicht und cascading Stylesheet.

- Iteration #3 – formularvalidierung hinzufügen. In der dritten Iteration fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Formulars ohne erforderlichen Felder des Formulars abzuschließen. Wir überprüfen e-Mail-Adressen und Telefonnummern.

- Iteration #4 – Stellen Sie die Anwendung, die lose miteinander verbunden. In dieser dritten Iteration nutzen wir mehrere Software-Entwurfsmuster, damit sie leichter zu verwalten und ändern die Kontakt-Manager-Anwendung. Beispielsweise gestalten wir unsere Anwendung in das Repository und die Abhängigkeitsinjektion-Muster verwenden.

- Iteration #5: Generieren von Komponententests. In der fünften Iteration stellen wir unsere Anwendung einfacher zu verwalten und ändern, indem Sie Komponententests hinzufügen. Wir unsere Daten Modellklassen modellieren und erstellen Sie Komponententests für unsere Controller und die Validierungslogik.

- Iteration #6 – verwenden Sie eine testgesteuerte Entwicklung. In dieser Iteration sechste wir neue Funktionalität Hinzufügen der Anwendung durch Schreiben von Komponententests zuerst und Code für die Komponententests schreiben. In dieser Iteration fügen wir Kontakt Gruppen hinzu.

- Iteration #7 – Hinzufügen von Ajax-Funktionen. In der siebten Iteration werden die Reaktionsfähigkeit und die Leistung der Anwendung durch Hinzufügen von Unterstützung für Ajax verbessern.

## <a name="this-iteration"></a>Diese Iteration

Das Ziel dieser Iteration ist, um die Darstellung der der Kontakt-Manager-Anwendung zu verbessern. Die Kontakt-Manager verwendet aktuell die Gestaltungsvorlage für ASP.NET MVC-Ansicht und cascading Stylesheet (siehe Abbildung 1). Diese Don ' t ungültige aussehen, aber ich Verschlüsselungskennwort t soll der Manager erhalten Sie genau wie jede andere ASP.NET MVC-Website gesucht werden soll. Ich möchte diese Dateien durch benutzerdefinierte Dateien zu ersetzen.


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)

**Abbildung 01**: die standarddarstellung einer ASP.NET MVC-Anwendung ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-2-make-the-application-look-nice-vb/_static/image2.png))


In dieser Iteration besprechen ich zwei Ansätze für den visuellen Entwurf der Anwendung zu verbessern. Zunächst erfahren Sie, wie der Entwurf von ASP.NET MVC-Galerie, um eine kostenlose ASP.NET MVC-Designvorlage herunterladen nutzen. Die ASP.NET MVC Design Gallery können Sie eine professionelle Webanwendung zu erstellen, ohne eine Aufgabe auszuführen.

Ich möchte keine Vorlage aus dem Katalog ASP.NET MVC-Entwurf für die Kontakt-Manager-Anwendung verwenden. Dagegen musste ich einen benutzerdefinierten Entwurf entwickeln, die von Profis erstellt. Im zweiten Teil dieses Lernprogramms erläutert, wie ich professionelle designbüros So erstellen den endgültigen Entwurf für ASP.NET MVC gearbeitet.

## <a name="the-aspnet-mvc-design-gallery"></a>Der ASP.NET MVC-Design-Gallery

ASP.NET MVC-Design-Katalog ist eine kostenlose Ressource, die von Microsoft bereitgestellt. Die ASP.NET MVC-Katalog befindet sich unter folgender Adresse:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

ASP.NET MVC-Design-Katalog hostet eine Auflistung von dieser Website-Entwürfe, die speziell für die Verwendung in einer ASP.NET MVC-Projekt erstellt wurden. Designs werden von einem Mitglied der Community hochgeladen. Besucher an den Katalog für ihre bevorzugte Designs darüber abstimmen können (siehe Abbildung 2).


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)

**Abbildung 02**: der ASP.NET MVC-Design Gallery ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-2-make-the-application-look-nice-vb/_static/image4.png))


Ich dieses Lernprogramms schreiben, ist die am häufigsten verwendete Entwurf im Katalog ein Entwurf, der Namen David Hauser Oktober. Sie können diesen Entwurf für eine ASP.NET MVC-Projekt verwenden, mithilfe der folgenden Schritte:

1. Klicken Sie auf die **herunterladen** Schaltfläche, um die October.zip-Datei auf Ihren Computer herunterladen.
2. Mit der rechten Maustaste der heruntergeladenen October.zip-Datei, und klicken Sie auf die **zum Aufheben der Sperre** Schaltfläche (siehe Abbildung 3).
3. Entzippen Sie die Datei in einen Ordner namens Oktober.
4. Wählen Sie alle Dateien aus dem DesignTemplate-Ordner im Ordner "Oktober" enthalten, mit der rechten Maustaste in den Dateien und wählen Sie die Menüoption **Kopie**.
5. Mit der rechten Maustaste des ContactManager Projektknoten im Projektmappen-Explorer von Visual Studio-Fenster, und wählen Sie die Menüoption **einfügen** (siehe Abbildung 4).
6. Wählen Sie die Visual Studio-Menüoption **bearbeiten, suchen und Ersetzen Schnellersetzung** , und Ersetzen Sie *["myprojectname"]* mit *ContactManager* (siehe Abbildung 5).


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)

**Abbildung 03**: zum Aufheben der Blockierung einer Datei aus dem Internet heruntergeladene ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-2-make-the-application-look-nice-vb/_static/image6.png))


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)

**Abbildung 04**: Überschreiben von Dateien im Projektmappen-Explorer ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-2-make-the-application-look-nice-vb/_static/image8.png))


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)

**Abbildung 05**: Ersetzen [Projektname] mit ContactManager ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-2-make-the-application-look-nice-vb/_static/image10.png))


Nachdem Sie diese Schritte abgeschlossen haben, wird Ihre Webanwendung das neue Design verwenden. Die Seite in Abbildung 6 veranschaulicht die Darstellung der der Kontakt-Manager-Anwendung mit dem Entwurf für Oktober.


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)

**Abbildung 06**: ContactManager mit der Oktober-Vorlage ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-2-make-the-application-look-nice-vb/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Erstellen eines benutzerdefinieren ASP.NET MVC-Entwurfs

Der ASP.NET MVC-Design-Gallery verfügt über eine gute Auswahl der Design-Formatvorlagen. Der Katalog bietet Ihnen eine problemlose Möglichkeit zum Anpassen der Darstellung Ihrer ASP.NET MVC-Anwendungen. Und natürlich Katalog weist den großen Vorteil, vollständig frei.

Allerdings müssen Sie einen vollständig eindeutigen Entwurf für Ihre Website erstellen. In diesem Fall Sie es sinnvoll, mit einer Website designbüros arbeiten. Ich möchte diesen Ansatz für den Entwurf für die Kontakt-Manager-Anwendung verwenden.

Ich oben wenden Sie sich an den Manager für die Iteration 1 ZIP und das Projekt an der designbüros gesendet. Visual Studio (schade darauf!), deren Besitzer sie nicht, jedoch t Problem darstellen. Sie konnten sich zum Herunterladen von Microsoft Visual Web Developer kostenlos von der [ https://www.asp.net ](https://www.asp.net) Website und Öffnen der Kontakt-Manager-Anwendung in Visual Web Developer. In einigen Tagen mussten sie den Entwurf in Abbildung 7 erstellt.


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)

**Abbildung 07**: der ASP.NET MVC erhalten Sie Manager Entwurf ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-2-make-the-application-look-nice-vb/_static/image14.png))


Das neue Design umfasste zwei Hauptdateien: ein neues CSS-Datei und eine neue Masterseitendatei anzeigen. Eine Ansichtsseite master enthält das Layout und die freigegebenen Inhalte, die für Sichten in einer ASP.NET MVC-Anwendung. Beispielsweise enthält die master-Ansichtsseite der Header, die Navigationsregisterkarten und die Fußzeile, die angezeigt werden in Abbildung 7. Ich überschreibt die vorhandene Sicht Stammwebsite im Ordner Views\Shared ebenfalls einen mit der neuen Datei Site.Master aus der designbüros,

Die designbüros erstellt auch eine neue cascading Stylesheet und einen Satz von Bildern. Diese neue Dateien im Ordner "Content" platziert, und überschrieben wurden vorhandene Datei "Site.CSS" ändern. Sie sollten alle statischen Inhalte im Ordner "Content" platzieren.

Beachten Sie, dass das neue Design für den Kontakt-Manager zum Bearbeiten und Löschen von Kontakten Bilder enthält. Ein Bild bearbeiten und löschen werden neben jeder erhalten Sie in der HTML-Tabelle der Kontakte angezeigt.

Ursprünglich diese Links, die mit der HTML-Code gerendert wurden. ActionLink() Helper wie folgt:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample1.aspx)]

Die Methode Html.ActionLink() unterstützt keine Bilder (die Methode HTML codiert den Text des Links aus Sicherheitsgründen). Aus diesem Grund ersetzt ich die Aufrufe von Html.ActionLink() durch Aufrufe von Url.Action() wie folgt:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample2.aspx)]

Einen vollständige HTML-Link rendert die Html.ActionLink()-Methode. Andererseits, rendert die Url.Action()-Methode, nur die URL ohne die &lt;eine&gt; Tag.

Beachten Sie außerdem, dass das neue Design Registerkarten ausgewählten "und" deaktiviert "enthält. Beispielsweise ist in Abbildung 8: die **neuen Kontakt erstellen** Registerkarte ausgewählt ist und die **Kontakte** Registerkarte nicht ausgewählt ist.


[![Das Dialogfeld "Neues Projekt"](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)

**Abbildung 08**: aktiviert und deaktiviert die Registerkarten ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-2-make-the-application-look-nice-vb/_static/image16.png))


Zur Unterstützung von Registerkarten ausgewählten "und" deaktiviert "Rendern, wurde eine benutzerdefinierte HTML-Hilfsobjekt, das mit dem Namen der MenuItemHelper erstellt. Diese-Hilfsmethode rendert entweder eine &lt;li&gt; Tag oder eine &lt;li-Klasse = "ausgewählt"&gt; Tag, je nachdem, ob die aktuellen Controller und Aktion entspricht dem Controller- und Namen in das Hilfsprogramm übergeben. Der Code für die MenuItemHelper ist im Codebeispiel 1 enthalten.

**1 – Helpers\MenuItemHelper.vb auflisten**

[!code-vb[Main](iteration-2-make-the-application-look-nice-vb/samples/sample3.vb)]

Die MenuItemHelper verwendet die TagBuilder Klasse intern zum Erstellen der &lt;li&gt; HTML-Tag. Die TagBuilder-Klasse ist eine sehr nützliche Hilfsprogrammklasse, die Sie verwenden können, benötigt werden, um ein neues HTML-Tag zu erstellen. Es enthält Methoden zum Hinzufügen von Attributen, CSS-Klassen hinzufügen, Generieren von Ids und ändern das Tag s innere HTML.

## <a name="summary"></a>Zusammenfassung

In dieser Iteration verbessert wir den visuellen Entwurf des unsere ASP.NET MVC-Anwendung. Sie wurden zunächst an den ASP.NET MVC-Design-Katalog eingeführt. Haben Sie gelernt, kostenlose Entwurfsvorlagen aus dem ASP.NET MVC-Design-Katalog herunterladen, die Sie in Ihre ASP.NET MVC-Anwendungen verwenden können.

Als Nächstes wird erläutert, wie Sie einen benutzerdefinierten Entwurf entwickeln durch Ändern der cascading Stylesheet Standarddatei und die Masteransicht Auslagerungsdatei erstellen können. Um das neue Design zu unterstützen, mussten wir einige kleinere Änderungen an unseren Kontakt-Manager-Anwendung vornehmen. Beispielsweise haben wir eine neue HTML-Hilfsobjekt, das mit dem Namen der MenuItemHelper, in dem ausgewählte und nicht ausgewählte Registerkarten angezeigt hinzugefügt.

In der nächsten Iteration konfigurieren wir den sehr wichtigen Betreff der Validierung. Wir hinzufügen, damit ein Benutzer kann nicht ohne Angabe von erforderlichen Werten, z. B. eine Person s zunächst einen neuen Kontakt erstellen und Nachname zu unserem Anwendung Validierungscode.

> [!div class="step-by-step"]
> [Zurück](iteration-1-create-the-application-vb.md)
> [Weiter](iteration-3-add-form-validation-vb.md)
