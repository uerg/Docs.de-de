---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: 'Iteration #3 – Hinzufügen der formularüberprüfung (VB) | Microsoft-Dokumentation'
author: microsoft
description: In der dritten Iteration fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Formulars ohne erforderlichen Felder des Formulars abzuschließen. Wir überprüfen außerdem Emai...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: a02178bfb662f180343ad32a6453b910e8e70471
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396207"
---
<a name="iteration-3--add-form-validation-vb"></a>Iteration #3 – Hinzufügen der formularüberprüfung (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> In der dritten Iteration fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Formulars ohne erforderlichen Felder des Formulars abzuschließen. Wir überprüfen auch die e-Mail-Adressen und Telefonnummern.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Erstellen einer Kontaktverwaltung ASP.NET MVC-Anwendung (VB)
  

In dieser Reihe von Tutorials erstellen wir eine gesamte Anwendung Kontaktverwaltung ab Beginn um den Vorgang abzuschließen. Die Contact Manager-Anwendung können Sie zum Speichern von Kontaktinformationen - Namen, Telefonnummern und e-Mail-Adressen – eine Liste der Personen an.

Wir erstellen die Anwendung über mehrere Iterationen. Bei jeder Iteration verbessern wir nach und nach der Anwendung. Das Ziel dieses mehrere Iteration-Ansatzes ist, um den Grund für jede Änderung verstehen können.

- Iteration #1 – Erstellen der Anwendung. In der ersten Iteration wir der Contact Manager in der einfachsten maximal möglicher Größe erstellen. Wir fügen Unterstützung für grundlegender Datenbankvorgänge: erstellen, lesen, aktualisieren und löschen (CRUD).

- Iteration #2 – Optimieren der Anwendung gut. In dieser Iteration verbessern wir die Darstellung der Anwendung durch Ändern der Standard-Masterseite für ASP.NET MVC-Ansicht und cascading Stylesheet.

- Iteration #3 – Hinzufügen der formularüberprüfung. In der dritten Iteration fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Formulars ohne erforderlichen Felder des Formulars abzuschließen. Wir überprüfen auch die e-Mail-Adressen und Telefonnummern.

- Stellen Sie Iteration #4 – lose koppeln der Anwendung. In dieser vierten Iteration nutzen wir einige Entwurfsmuster für Software zu verwalten und ändern Sie die Kontakt-Manager-Anwendung zu vereinfachen. Z. B. gestalten wir unsere Anwendung, die dem Repositorymuster und dem Dependency Injection-Muster verwenden.

- Iteration #5 – Erstellen von Komponententests. In der fünften Iteration stellen wir unsere Anwendung einfacher zu verwalten und zu ändern, indem Sie die Komponententests hinzufügen. Wir unsere Data Model-Klassen modellieren und erstellen Sie Komponententests für unseren Controller und die Validierungslogik.

- Iteration #6 – Verwenden der testgesteuerten Entwicklung. In dieser Iteration sechsten fügen wir neue Funktionen zu unserer Anwendung zuerst Schreiben von Komponententests und Code für die Komponententests schreiben hinzu. In dieser Iteration fügen wir wenden Sie sich an Gruppen hinzu.

- Iteration #7 - Ajax-Funktionen hinzufügen. In der siebten Iteration verbessern wir die Reaktionsfähigkeit und Leistung der Anwendung durch Hinzufügen von Unterstützung für Ajax.


## <a name="this-iteration"></a>Diese Iteration

In dieser zweiten Iteration der Contact Manager-Anwendung fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Benutzer einen Kontakt ohne Eingabe von Werten für die erforderlichen Felder des Formulars zu senden. Wir überprüfen auch die Telefonnummern und e-Mail-Adressen (siehe Abbildung 1).


[![Das Dialogfeld "Neues Projekt"](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Abbildung 01**: ein Formular mit Überprüfung ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-3-add-form-validation-vb/_static/image2.png))


In dieser Iteration fügen wir die Validierungslogik, direkt auf die Controlleraktionen. Im Allgemeinen ist dies nicht die empfohlene Methode zum Hinzufügen der Validierung zu einer ASP.NET MVC-Anwendung. Ein besserer Ansatz besteht darin die Validierungslogik einer Anwendung s in einem separaten [Dienstschicht](http://martinfowler.com/eaaCatalog/serviceLayer.html). In der nächsten Iteration gestalten wir die Contact Manager-Anwendung, um die Anwendung besser verwaltbar zu machen.

In dieser Iteration, aus Gründen der Einfachheit schreiben wir alle der Validierungscode manuell. Anstelle der Validierungscode schreiben, selbst, könnten wir einen Validierungsframework nutzen. Beispielsweise können Sie die Microsoft Enterprise Library Validation Application Block (VAB) zum Implementieren von Validierungslogik für Ihre ASP.NET MVC-Anwendung aus. Weitere Informationen zu der Validation Application Block finden Sie unter:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Hinzufügen einer Validierung zu Ansicht "erstellen"

Lassen Sie s starten, indem Sie die Erstellungsansicht Validierungslogik hinzugefügt. Glücklicherweise da wir die Erstellungsansicht mit Visual Studio generiert, enthält die Erstellungsansicht bereits aller erforderlichen Benutzeroberflächenlogik validierungsmeldungen werden angezeigt. Ansicht "erstellen" ist in Codebeispiel 1 enthalten.

**Codebeispiel 1 – \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

Die Feld-Überprüfungsfehlern-Klasse wird verwendet, zum Formatieren der Ausgabe, die vom Hilfsprogramm Html.ValidationMessage() gerendert. Die Eingabe-Überprüfungsfehlern-Klasse wird verwendet, so formatieren Sie das Textfeld (Eingabe), die vom Hilfsprogramm Html.TextBox() gerendert. Die Zusammenfassung der Validierungsfehler-Klasse wird verwendet, zum Formatieren der unsortierten Liste, die vom Hilfsprogramm Html.ValidationSummary() gerendert.

> [!NOTE] 
> 
> Sie können Stylesheet-Klassen in diesem Abschnitt anpassen die Darstellung von Validierungsfehlermeldungen beschrieben ändern.


## <a name="adding-validation-logic-to-the-create-action"></a>Hinzufügen von Validierungslogik auf die Aktion erstellen

Moment, Ansicht "erstellen" niemals Überprüfungsfehlermeldungen angezeigt, weil wir die Logik, um alle Nachrichten generieren, die nicht geschrieben haben. Um Überprüfungsfehlermeldungen anzuzeigen, müssen Sie die Fehlermeldungen ModelState hinzufügen.

> [!NOTE] 
> 
> Die UpdateModel() Methode hinzufügt, Fehlermeldungen ModelState automatisch Wenn Fehler beim Zuweisen einer Eigenschaft eines Formularfelds. Z. B., wenn Sie versuchen, eine BirthDate-Eigenschaft die Zeichenfolge "Apple" zuweisen, die DateTime-Werte akzeptiert, fügt dann die UpdateModel()-Methode einen Fehler hinzu ModelState.


Die geänderte Create()-Methode in Liste 2 enthält einen neuen Abschnitt, der die Eigenschaften der Contact-Klasse überprüft, bevor die neue Kontakt in die Datenbank eingefügt wird.

**Codebeispiel 2 - Controllers\ContactController.vb (erstellen mit Überprüfung)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

Die Validate-Abschnitt erzwingt vier unterschiedliche Validierungsregeln an:

- Die FirstName-Eigenschaft muss eine Länge größer als 0 (null) haben (und es darf nicht ausschließlich aus Leerzeichen bestehen)
- Die LastName-Eigenschaft muss eine Länge größer als 0 (null) haben (und es darf nicht ausschließlich aus Leerzeichen bestehen)
- Wenn die Phone-Eigenschaft einen Wert aufweist (hat eine Länge größer als 0) und dann die Phone-Eigenschaft auf einen regulären Ausdruck übereinstimmen muss.
- Wenn die e-Mail-Eigenschaft einen Wert aufweist (hat eine Länge größer als 0) und dann die e-Mail-Eigenschaft mit einem regulären Ausdruck entsprechen muss.

Eine Regel validierungsverstoß liegt, wird ModelState eine Fehlermeldung mit Hilfe der Methode AddModelError() hinzugefügt werden. Wenn Sie eine Nachricht ModelState hinzufügen, geben Sie den Namen einer Eigenschaft und den Text der eine Validierungsfehlermeldung angezeigt. Diese Fehlermeldung wird von den Hilfsmethoden Html.ValidationSummary() und Html.ValidationMessage() in der Ansicht angezeigt.

Nachdem die Validierungsregeln ausgeführt werden, wird die Eigenschaft "IsValid" von ModelState überprüft. Die Eigenschaft "IsValid" gibt false zurück, wenn alle Validierungsfehlermeldungen ModelState hinzugefügt wurden. Wenn die Validierung fehlschlägt, wird der erstellen-Form in die Fehlermeldungen erneut angezeigt.

> [!NOTE] 
> 
> Ich habe die regulären Ausdrücke für die Überprüfung der Telefonnummer und e-Mail-Adresse aus dem Repository der reguläre Ausdruck unter [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Die Bearbeitungsaktion Validierungslogik hinzugefügt

Die Aktion Edit() aktualisiert einen Kontakt. Die Aktion Edit() muss genau die gleiche Überprüfungen als Create() Aktion durchführen. Anstelle den gleiche Überprüfungscode zu wiederholen, sollten wir den Contact-Controller so umgestalten, dass sowohl die Edit() die Create() Aktionen rufen Sie die gleiche Validierungsmethode.

Die geänderte Contact-Controller-Klasse ist in Programmausdruck 3 enthalten. Diese Klasse verfügt über eine neue ValidateContact()-Methode, die in Create() sowohl Edit() Aktionen aufgerufen wird.

**Codebeispiel 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Zusammenfassung

In dieser Iteration haben wir grundlegende formularvalidierung zu unserer Contact Manager-Anwendung hinzugefügt. Unsere Validierungslogik verhindert, dass Benutzer übermitteln einen neuen Kontakt oder bearbeiten einen vorhandenen Kontakt ohne Angabe der Werte für die FirstName und LastName-Eigenschaften. Darüber hinaus müssen die Benutzer gültige Telefonnummer und e-Mail-Adressen angeben.

In dieser Iteration haben wir die Validierungslogik zu unserer Contact Manager-Anwendung in die einfachste Möglichkeit, die mögliche hinzugefügt. Allerdings wird Mischen von unserem Validierungslogik in unserer Controllerlogik Probleme für uns auf lange Sicht erstellen. Die Anwendung werden schwieriger zu verwalten und im Laufe der Zeit ändern.

In der nächsten Iteration werden wir unsere Validierungslogik und Datenbankzugriffslogik aus unserem Controller gestalten. Es werden mehrere Prinzipien der Softwareentwicklung zu uns ermöglichen, Erstellen einer zunehmend lockerer verkoppelt und wartungsfreundlicher, nutzen.

> [!div class="step-by-step"]
> [Zurück](iteration-2-make-the-application-look-nice-vb.md)
> [Weiter](iteration-4-make-the-application-loosely-coupled-vb.md)
