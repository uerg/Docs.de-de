---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: 'Iteration #3 – Hinzufügen von formularvalidierung (c#) | Microsoft Docs'
author: microsoft
description: In der dritten Iteration fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Formulars ohne erforderlichen Felder des Formulars abzuschließen. Wir überprüfen auch Emai...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: b9353c32b2839fd760513982c5742bb8f521e94a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="iteration-3--add-form-validation-c"></a>Iteration #3 – Hinzufügen von formularvalidierung (c#)
====================
durch [Microsoft](https://github.com/microsoft)

[Herunterladen von Code](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> In der dritten Iteration fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Formulars ohne erforderlichen Felder des Formulars abzuschließen. Wir überprüfen e-Mail-Adressen und Telefonnummern.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Erstellen einer ASP.NET MVC-Anwendung Kontaktverwaltung (c#)
  

In dieser Reihe von Lernprogrammen erstellen wir eine vollständige Contact Management-Anwendung von Grund auf Fertig stellen. Die Kontakt-Manager-Anwendung können Sie zum Speichern von Kontaktinformationen - Namen, Telefonnummern und e-Mail-Adressen - eine Liste der Personen.

Erstellen der Anwendung über mehrere Iterationen. Bei jeder Iteration verbessern wir allmählich die Anwendung. Das Ziel dieses Ansatzes für mehrere Iteration ist, um den Grund für jede Änderung verstehen können.

- Iteration #1 – Erstellen der Anwendung. In der ersten Iteration erstellen wir die Kontakt-Manager in der einfachsten möglich. Wir fügen die Unterstützung für die grundlegenden Datenbankvorgängen: erstellen, lesen, aktualisieren und löschen (CRUD).

- Iteration #2 – Stellen Sie die Anwendung gut aussehen. In dieser Iteration haben wir die Darstellung der Anwendung verbessern, indem Ändern der Standard-Gestaltungsvorlage für ASP.NET MVC-Ansicht und cascading Stylesheet.

- Iteration #3 - formularvalidierung hinzufügen. In der dritten Iteration fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Formulars ohne erforderlichen Felder des Formulars abzuschließen. Wir überprüfen e-Mail-Adressen und Telefonnummern.

- Iteration #4 – Stellen Sie die Anwendung, die lose miteinander verbunden. In dieser dritten Iteration nutzen wir mehrere Software-Entwurfsmuster, damit sie leichter zu verwalten und ändern die Kontakt-Manager-Anwendung. Beispielsweise gestalten wir unsere Anwendung in das Repository und die Abhängigkeitsinjektion-Muster verwenden.

- Iteration #5: Erstellen von Komponententests. In der fünften Iteration stellen wir unsere Anwendung einfacher zu verwalten und ändern, indem Sie Komponententests hinzufügen. Wir unsere Daten Modellklassen modellieren und erstellen Sie Komponententests für unsere Controller und die Validierungslogik.

- Iteration #6 – verwenden Sie eine testgesteuerte Entwicklung. In dieser Iteration sechste wir neue Funktionalität Hinzufügen der Anwendung durch Schreiben von Komponententests zuerst und Code für die Komponententests schreiben. In dieser Iteration fügen wir Kontakt Gruppen hinzu.

- Iteration #7 - Ajax-Funktionen hinzufügen. In der siebten Iteration werden die Reaktionsfähigkeit und die Leistung der Anwendung durch Hinzufügen von Unterstützung für Ajax verbessern.


## <a name="this-iteration"></a>Diese Iteration

In dieser zweite Iteration der Kontakt-Manager-Anwendung fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Kontakts ohne Werte für die erforderlichen Felder eingeben. Wir überprüfen Telefonnummern und e-Mail-Adressen (siehe Abbildung 1).


[![Das Dialogfeld "Neues Projekt"](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**Abbildung 01**: ein Formular mit Überprüfung ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-3-add-form-validation-cs/_static/image2.png))


In dieser Iteration eine wir direkt an die Controlleraktionen die Validierungslogik hinzufügen. Im Allgemeinen ist dies nicht die empfohlene Methode zum Hinzufügen von Validierung zu einer ASP.NET MVC-Anwendung. Ein besserer Ansatz ist, platzieren Sie eine Anwendung s Validierungslogik in einer separaten [Dienstschicht](http://martinfowler.com/eaaCatalog/serviceLayer.html). In der nächsten Iteration gestalten wir die Kontakt-Manager-Anwendung, um die Anwendung sind besser verwaltbar zu machen.

In dieser Iteration zur Vereinfachung der Dinge schreiben wir alle der Validierungscode per hand katalogisiert wird. Anstelle der Validierungscode schreiben, uns, konnten wir einen Validierungsframework nutzen. Microsoft Enterprise Library Überprüfung Application Block (VAB) können Sie z. B. um die Validierungslogik für die ASP.NET MVC-Anwendung zu implementieren. Weitere Informationen zu den Anwendungsblock für die Validierung finden Sie unter:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Hinzufügen einer Validierung auf die Ansicht erstellen

Lassen Sie s starten, indem Sie die Erstellungsansicht Validierungslogik hinzufügen. Glücklicherweise da wir die Ansicht für das Erstellen mit Visual Studio generiert, enthält die Erstellungsansicht bereits alle erforderlichen Benutzeroberflächenlogik validierungsmeldungen angezeigt. Auflisten von 1 enthalten die Erstellungsansicht.

**Listing 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

Beachten Sie den Aufruf an die Html.ValidationSummary()-Hilfsmethode, die direkt über das HTML-Formular angezeigt wird. Wenn Überprüfung Fehler Nachrichten vorhanden sind, zeigt diese Methode validierungsmeldungen in einer Aufzählung an.

Beachten Sie, dass darüber hinaus die Aufrufe Html.ValidationMessage(), die neben jedem Formularfeld angezeigt werden. Das Hilfsobjekt ValidationMessage() zeigt eine einzelne Validierungsfehlermeldung angezeigt. Bei Auflisten von 1 wird ein Sternchen angezeigt, wenn es ein Validierungsfehler tritt.

Schließlich rendert das Hilfsobjekt Html.TextBox() automatisch eine Cascading Style Sheet-Klasse aus, wenn es ein Validierungsfehler, die der Eigenschaft angezeigt werden, indem Sie das Hilfsprogramm zugeordnet tritt. Das Hilfsobjekt Html.TextBox() rendert eine Klasse namens **Eingabe Validierungsfehler**.

Wenn Sie eine neue ASP.NET MVC-Anwendung erstellen, wird automatisch ein Stylesheet, mit dem Namen "Site.CSS" ändern im Ordner "Content" erstellt. Dieses Stylesheet enthält die folgenden Definitionen für CSS-Klassen, die im Zusammenhang mit der Darstellung von Überprüfungsfehlermeldungen an:

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

Das Feld Validierungsfehler-Klasse wird verwendet, zum Formatieren der Ausgabe, die durch das Hilfsprogramm Html.ValidationMessage() gerendert. Die Eingabe Validierungsfehler-Klasse wird verwendet, so formatieren Sie das Textfeld ein (Eingabe) durch das Hilfsprogramm Html.TextBox() gerendert. Die Zusammenfassung Validierungsfehler-Klasse wird verwendet, so formatieren Sie die ungeordnete Liste durch das Hilfsprogramm Html.ValidationSummary() gerendert.

> [!NOTE] 
> 
> Sie können Stylesheet-Klassen beschrieben, die in diesem Abschnitt, um die Darstellung von Validierungsfehlermeldungen anzupassen.


## <a name="adding-validation-logic-to-the-create-action"></a>Hinzufügen von Validierungslogik auf die Aktion zu erstellen

Jetzt, die Erstellungsansicht Überprüfungsfehlermeldungen nie zeigt an, da es die Logik zum Generieren von Fehlermeldungen nicht geschrieben haben. Um Überprüfungsfehlermeldungen anzuzeigen, müssen Sie die Fehlermeldungen ModelState hinzufügen.

> [!NOTE] 
> 
> Die Methode UpdateModel() hinzugefügt Fehlermeldungen ModelState automatisch bei einem Fehler den Wert eines Formularfelds einer Eigenschaft zuweisen. Z. B. Wenn Sie versuchen, eine Eigenschaft "BirthDate" die Zeichenfolge "Apple" zuweisen, das DateTime-Werte akzeptiert, fügt dann die UpdateModel()-Methode einen Fehler hinzu ModelState.


Die geänderte Create()--Methode im Codebeispiel 2 enthält einen neuen Abschnitt, der die Eigenschaften der Klasse wenden Sie sich an überprüft, bevor Sie der neue Kontakt in die Datenbank eingefügt wird.

**Auflisten von 2 – Controllers\ContactController.cs (mit der Validierung erstellen)**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

Die Validate-Abschnitt erzwingt vier unterschiedliche Validierungsregeln:

- Die FirstName-Eigenschaft muss eine Länge größer als 0 (null) haben (und es darf nicht ausschließlich aus Leerzeichen bestehen)
- Die LastName-Eigenschaft muss eine Länge größer als 0 (null) haben (und es darf nicht ausschließlich aus Leerzeichen bestehen)
- Wenn Sie den Phone-Eigenschaft einen Wert aufweist (hat eine Länge größer als 0) und dann die Phone-Eigenschaft einen regulären Ausdruck entsprechen muss.
- Wenn die e-Mail-Eigenschaft einen Wert hat (hat eine Länge größer als 0) und dann die e-Mail-Eigenschaft auf einen regulären Ausdruck entsprechen muss.

Bei einem Verstoß gegen eine Codeanalyseregel Überprüfung, wird ModelState eine Fehlermeldung mit Hilfe der AddModelError() Methode hinzugefügt werden. Wenn Sie eine Nachricht ModelState hinzufügen, geben Sie an den Namen einer Eigenschaft und der Text, der eine Validierungsfehlermeldung angezeigt. Diese Fehlermeldung wird von den Hilfsmethoden Html.ValidationSummary() und Html.ValidationMessage() in der Ansicht angezeigt.

Nachdem Sie die Validierungsregeln ausgeführt werden, wird die IsValid-Eigenschaft des ModelState überprüft. IsValid-Eigenschaft gibt "false" zurück, wenn Validierungsfehlermeldungen ModelState hinzugefügt wurden. Bei einem Überprüfungsfehler wird in die Fehlermeldungen der erstellen-Form erneut.

> [!NOTE] 
> 
> Ich habe erhalten die regulären Ausdrücke zur Überprüfung der Telefonnummer und e-Mail-Adresse aus dem Repository regulären Ausdruck zur [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Hinzufügen von Validierungslogik auf die Aktion bearbeiten

Die Aktion Edit() aktualisiert einen Kontakt. Die Aktion Edit() muss genau die gleiche Validierung als Create()-Aktion durchführen. Anstelle den gleichen Validierungscode Duplizierung sollten wir die Kontakt-Controller so umgestalten, dass Aktionen Create()-"und" Edit() dieselbe Überprüfung Methode aufrufen.

Die geänderte Kontakt Controllerklasse ist in 3 auflisten enthalten. Diese Klasse verfügt über eine neue ValidateContact()-Methode, die in Aktionen Create()-"und" Edit() aufgerufen wird.

**3 – Controllers\ContactController.cs auflisten**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>Zusammenfassung

In dieser Iteration haben wir für unsere Kontakt-Manager-Anwendung grundlegende formularvalidierung hinzugefügt. Überprüfungslogik verhindert, dass Benutzer übermitteln eines neuen Kontakts oder Bearbeiten eines vorhandenen Kontakts ohne Angabe von Werten für die Eigenschaft FirstName und LastName. Darüber hinaus müssen Benutzer über gültige Telefonnummer Telefonnummern und e-Mail-Adressen angeben.

In dieser Iteration haben wir die Validierungslogik für unsere Kontakt-Manager-Anwendung in die einfachste Möglichkeit, die mögliche hinzugefügt. Allerdings wird das Mischen von Überprüfungslogik in unserer Controllerlogik Probleme für uns langfristig erstellen. Die Anwendung wird schwieriger zu verwalten und mit der Zeit ändern.

In der nächsten Iteration gestalten wir unsere Validierungslogik und Datenbank-Zugriffslogik aus unserem Controller. Es müssen mehrere Software-Entwurfsprinzipien, um es uns zum Erstellen einer Anwendung lose gekoppelten und sind besser verwaltbaren ermöglichen nutzen.

> [!div class="step-by-step"]
> [Zurück](iteration-2-make-the-application-look-nice-cs.md)
> [Weiter](iteration-4-make-the-application-loosely-coupled-cs.md)
