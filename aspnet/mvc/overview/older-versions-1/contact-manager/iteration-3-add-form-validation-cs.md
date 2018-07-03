---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: 'Iteration #3 – Hinzufügen der formularüberprüfung (c#) | Microsoft-Dokumentation'
author: microsoft
description: In der dritten Iteration fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Formulars ohne erforderlichen Felder des Formulars abzuschließen. Wir überprüfen außerdem Emai...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: a76e75f2d343764038142235c92e2db04e807778
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377005"
---
<a name="iteration-3--add-form-validation-c"></a>Iteration #3 – Hinzufügen der formularüberprüfung (c#)
====================
durch [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> In der dritten Iteration fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Formulars ohne erforderlichen Felder des Formulars abzuschließen. Wir überprüfen auch die e-Mail-Adressen und Telefonnummern.


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

In dieser zweiten Iteration der Contact Manager-Anwendung fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Benutzer einen Kontakt ohne Eingabe von Werten für die erforderlichen Felder des Formulars zu senden. Wir überprüfen auch die Telefonnummern und e-Mail-Adressen (siehe Abbildung 1).


[![Das Dialogfeld "Neues Projekt"](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**Abbildung 01**: ein Formular mit Überprüfung ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-3-add-form-validation-cs/_static/image2.png))


In dieser Iteration fügen wir die Validierungslogik, direkt auf die Controlleraktionen. Im Allgemeinen ist dies nicht die empfohlene Methode zum Hinzufügen der Validierung zu einer ASP.NET MVC-Anwendung. Ein besserer Ansatz besteht darin die Validierungslogik einer Anwendung s in einem separaten [Dienstschicht](http://martinfowler.com/eaaCatalog/serviceLayer.html). In der nächsten Iteration gestalten wir die Contact Manager-Anwendung, um die Anwendung besser verwaltbar zu machen.

In dieser Iteration, aus Gründen der Einfachheit schreiben wir alle der Validierungscode manuell. Anstelle der Validierungscode schreiben, selbst, könnten wir einen Validierungsframework nutzen. Beispielsweise können Sie die Microsoft Enterprise Library Validation Application Block (VAB) zum Implementieren von Validierungslogik für Ihre ASP.NET MVC-Anwendung aus. Weitere Informationen zu der Validation Application Block finden Sie unter:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Hinzufügen einer Validierung zu Ansicht "erstellen"

Lassen Sie s starten, indem Sie die Erstellungsansicht Validierungslogik hinzugefügt. Glücklicherweise da wir die Erstellungsansicht mit Visual Studio generiert, enthält die Erstellungsansicht bereits aller erforderlichen Benutzeroberflächenlogik validierungsmeldungen werden angezeigt. Ansicht "erstellen" ist in Codebeispiel 1 enthalten.

**Codebeispiel 1 – \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

Beachten Sie, dass des Aufrufs an die Html.ValidationSummary()-Hilfsmethode, die direkt über das HTML-Formular angezeigt wird. Wenn es Meldungen für Validierungsfehler sind, zeigt diese Methode validierungsmeldungen in eine Liste mit Aufzählungszeichen.

Beachten Sie außerdem die Aufrufe an Html.ValidationMessage(), die neben jedem Formularfeld angezeigt werden. Das Hilfsprogramm ValidationMessage() zeigt eine einzelne Validierungsfehlermeldung angezeigt. Im Fall von Codebeispiel 1 wird ein Sternchen angezeigt, wenn ein Überprüfungsfehler vorliegt.

Zum Abschluss rendert das Hilfsobjekt Html.TextBox() automatisch eine Cascading Style Sheet-Klasse, bei der Überprüfungsfehler angezeigt, die vom Hilfsprogramm Eigenschaft zugeordnet ist. Das Hilfsprogramm Html.TextBox() rendert eine Klasse namens **Eingabe-Überprüfungsfehlern**.

Wenn Sie eine neue ASP.NET MVC-Anwendung erstellen, wird automatisch ein Stylesheet, mit dem Namen "Site.CSS" im Ordner "Content" erstellt. Dieses Stylesheet enthält die folgenden Definitionen für CSS-Klassen beziehen sich auf die Darstellung von Validierungsfehlermeldungen:

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

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

**Codebeispiel 2 - Controllers\ContactController.cs (erstellen mit Überprüfung)**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

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

**Codebeispiel 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>Zusammenfassung

In dieser Iteration haben wir grundlegende formularvalidierung zu unserer Contact Manager-Anwendung hinzugefügt. Unsere Validierungslogik verhindert, dass Benutzer übermitteln einen neuen Kontakt oder bearbeiten einen vorhandenen Kontakt ohne Angabe der Werte für die FirstName und LastName-Eigenschaften. Darüber hinaus müssen die Benutzer gültige Telefonnummer und e-Mail-Adressen angeben.

In dieser Iteration haben wir die Validierungslogik zu unserer Contact Manager-Anwendung in die einfachste Möglichkeit, die mögliche hinzugefügt. Allerdings wird Mischen von unserem Validierungslogik in unserer Controllerlogik Probleme für uns auf lange Sicht erstellen. Die Anwendung werden schwieriger zu verwalten und im Laufe der Zeit ändern.

In der nächsten Iteration werden wir unsere Validierungslogik und Datenbankzugriffslogik aus unserem Controller gestalten. Es werden mehrere Prinzipien der Softwareentwicklung zu uns ermöglichen, Erstellen einer zunehmend lockerer verkoppelt und wartungsfreundlicher, nutzen.

> [!div class="step-by-step"]
> [Zurück](iteration-2-make-the-application-look-nice-cs.md)
> [Weiter](iteration-4-make-the-application-loosely-coupled-cs.md)
