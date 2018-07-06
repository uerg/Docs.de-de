---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: 'Iteration #5 – Erstellen von Komponententests (VB) | Microsoft-Dokumentation'
author: microsoft
description: In der fünften Iteration stellen wir unsere Anwendung einfacher zu verwalten und zu ändern, indem Sie die Komponententests hinzufügen. Wir unsere Data Model-Klassen modellieren und Erstellen von Komponententests für o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: 302dfc2a26e3f357818570c673eafe44346330c4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390001"
---
<a name="iteration-5--create-unit-tests-vb"></a>Iteration #5 – Erstellen von Komponententests (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> In der fünften Iteration stellen wir unsere Anwendung einfacher zu verwalten und zu ändern, indem Sie die Komponententests hinzufügen. Wir unsere Data Model-Klassen modellieren und erstellen Sie Komponententests für unseren Controller und die Validierungslogik.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Erstellen einer Kontaktverwaltung ASP.NET MVC-Anwendung (VB)

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

In vorherigen Iterationen der Contact Manager-Anwendung haben wir die Anwendung in loser gekoppelt werden umgestaltet. Wir getrennt, die Anwendung in unterschiedliche Controller-Dienst und Repository-Ebenen. Jede Ebene interagiert mit der Ebene darunter über Schnittstellen.

Wir haben die Anwendung aus, um die Anwendung leichter verwalten und ändern umgestaltet. Z. B. wenn wir eine neue datenzugriffstechnologie verwenden müssen, können wir einfach die Repository-Ebene ändern, ohne durch berühren der Controller oder Dienstebene. Die Kontakt-Manager lose gekoppelt, machen wir Ve vorgenommen die Anwendung stabiler zu ändern.

Aber was geschieht, wenn ein neues Feature der Contact Manager-Anwendung hinzufügen möchten? Oder, was geschieht, wenn wir einen Fehler beheben? Eine traurige Sache, aber auch bewährte Wahrheit beim Schreiben von Code ist, wenn Sie Code, die Sie erstellen, das Risiko der Einführung neuer Fehler berühren.

Beispielsweise kann einen Tag in Ordnung, Ihren Vorgesetzten bitten Sie, eine neue Funktion zum Contact Manager hinzuzufügen. Sie möchte Sie Unterstützung für wenden Sie sich an Gruppen hinzufügen. Er möchte, dass Sie damit Benutzer auf ihre Kontakte in Gruppen wie z. B. Freunde, Business und So weiter organisieren können.

Um dieses neue Feature zu implementieren, müssen Sie alle drei Ebenen der Contact Manager-Anwendung zu ändern. Sie müssen die Controller, die Dienstschicht und dem Repository neue Funktionalität hinzufügen. Sobald Sie beginnen, Code zu ändern, riskieren Sie wichtige Funktionen, die gearbeitet.

Die Anwendung in verschiedenen Schichten, Refactoring, wie in der vorherigen Iteration ist eine gute Sache. Es war eine gute Sache, da es uns, Änderungen auf ganze Ebenen zu machen, ohne zu verändern die übrigen Teile der Anwendung ermöglicht. Wenn Sie den Code innerhalb einer Ebene leichter verwalten und ändern möchten, müssen Sie jedoch, Komponententests für den Code zu erstellen.

Sie verwenden einer Einheit Test zu Test eine einzelne Einheit von Code. Diese Einheiten von Code sind kleiner als die Ebenen der gesamten Anwendung. In der Regel verwenden Sie einen Komponententest um zu überprüfen, ob eine bestimmte Methode in Ihrem Code anders verhält, die Sie erwarten. Beispielsweise würden Sie einen Komponententest für die CreateContact()-Methode, die verfügbar gemacht, von der Klasse ContactManagerService erstellen.

Die Komponententests für eine anwendungsarbeit genau wie ein Sicherheitsnetz bereitstellen. Wenn Sie Code in einer Anwendung ändern, können Sie einen Satz von Komponententests zu überprüfen, ob die Änderung die vorhandene Funktionalität unterbricht ausführen. Komponententests stellen Ihren Code sicher ändern. Komponententests stellen alle des Codes in Ihrer Anwendung stabiler zu ändern.

In dieser Iteration hinzugefügt unsere Anwendung Contact Manager Komponententests. Auf diese Weise können in der nächsten Iteration, wir wenden Sie sich an Gruppen unserer Anwendung hinzufügen ohne sich Gedanken, vorhandene Funktionalität zu beschädigen.

> [!NOTE] 
> 
> Es gibt eine Vielzahl von Komponententest-Frameworks wie NUnit und xUnit.net MbUnit. In diesem Tutorial verwenden wir die Komponententest-Framework in Visual Studio enthalten. Sie können jedoch ganz einfach eines dieser Frameworks alternative verwenden.


## <a name="what-gets-tested"></a>Ruft ab, was getestet

Im besten Fall würde Ihr gesamter Code von Komponententests abgedeckt werden. Im besten Fall müssten Sie die perfekte Sicherheitsnetz Spannen. Sie würden in der Lage, jede Codezeile in Ihrer Anwendung zu ändern und Sie wissen sofort, durch die Komponententests ausführen, ob die Änderung die vorhandene Funktionalität unterbrochen wurde.

Allerdings wird der Einbau zusätzlichen wir t, die in einer perfekten Welt live geöffnet. In der Praxis, wenn Sie Komponententests schreiben konzentrieren Sie sich über das Schreiben von Tests für Ihre Geschäftslogik (z. B. die Validierungslogik). Insbesondere Sie *nicht* Schreiben von Komponententests für Ihre Daten zugreifen, Logik und die ansichtslogik.

Um Sie hilfreich sein, müssen sehr schnell Ausführen von Komponententests. Sie können ganz einfach die Möglichkeit, Hunderte (oder sogar Tausende) von Komponententests für eine Anwendung sammeln. Wenn die Komponententests sehr lange dauert ausgeführt, und Sie vermeiden, diese ausgeführt werden. Das heißt, sind die Ausführung lang andauernder Komponententests für den Tag zu Tag codierungszwecke nutzlos.

Aus diesem Grund werden Sie in der Regel keine Komponententests für Code schreiben, die mit einer Datenbank interagiert. Ausführen von Hunderten von Komponententests für eine Livedatenbank wäre zu langsam ausgeführt. Sie können stattdessen modellieren Ihrer Datenbank und Code schreiben, der Interaktion der simulierten Datenbank zwischen (erörtert, simulieren eine Datenbank, die weiter unten).

Einem ähnlichen Grund ist führen Sie in der Regel von Komponententests für Ansichten nicht geschrieben werden. Um eine Ansicht zu testen, müssen Sie einen Webserver einrichten. Da sich drehende, um einen Webserver eine relativ langsam Prozess ist, wird das Erstellen von Komponententests für Ihre Ansichten nicht empfohlen.

Wenn Ihrer Ansicht komplizierte Logik enthält dann sollten Sie die Logik in Helper-Methoden zu verschieben. Sie können Komponententests für Hilfsmethoden schreiben, die ohne aufstocken von einem Webserver ausgeführt werden.

> [!NOTE] 
> 
> Beim Schreiben von für die Logik für den Datenzugriff Tests oder Anzeigelogik nicht ratsam, ist wenn Komponententests schreiben, können diese Tests sehr wertvoll sein, wenn tests erstellen, die funktionale oder zur Integration.


> [!NOTE] 
> 
> ASP.NET MVC ist die Web Forms-Ansichts-Engine. Während die Web Forms-Ansichts-Engine auf einem Webserver abhängig ist, möglicherweise andere Ansichts-Engines nicht.


## <a name="using-a-mock-object-framework"></a>Mithilfe eines Pseudoobjektframeworks

Wenn Sie Komponententests erstellen, müssen Sie fast immer ein Objekt simulieren-Framework nutzen. Ein Objekt simulieren-Framework können Sie Mocks und Stubs für die Klassen in Ihrer Anwendung zu erstellen.

Sie können z. B. ein Objekt simulieren-Framework verwenden, auf um eine Pseudoversion des Ihr Repository-Klasse zu generieren. Auf diese Weise können Sie die pseudorepository-Klasse anstelle der echten repositoryklasse in Ihren Komponententests verwenden. Verwenden das pseudorepository, können Sie vermeiden Sie die Ausführung von Datenbankcode, beim Ausführen eines Komponententests.

Visual Studio enthält ein Objekt simulieren Framework keine. Es gibt jedoch mehrere kommerzielle und open Source-Objekt simulieren-Frameworks für .NET Framework zur Verfügung:

1. Moq - ist dieses Framework, die unter der open-Source-BSD-Lizenz verfügbar. Sie können die Moq aus [ https://code.google.com/p/moq/ ](https://code.google.com/p/moq/).
2. Rhino Mocks – dieses Framework wird unter der open-Source-BSD-Lizenz verfügbar. Können Sie herunterladen, Rhino Mocks aus [ http://ayende.com/projects/rhino-mocks.aspx ](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock Isolator - ist dies ein kommerzieller Framework. Sie können eine Testversion von [ http://www.typemock.com/ ](http://www.typemock.com/).

In diesem Tutorial entschied ich mich mit Moq. Allerdings genauso einfach können Sie Rhino Mocks oder Typemock Isolator das Mock Erstellen von Objekten für die Kontakt-Manager-Anwendung.

Bevor Sie Moq verwenden können, müssen Sie die folgenden Schritte ausführen:

1. sein.
2. Bevor Sie den Download Entzippen, stellen Sie sicher, dass Sie mit der rechten Maustaste in der das, und klicken Sie auf die Schaltfläche **Unblock** (siehe Abbildung 1).
3. Entzippen Sie den Download aus.
4. Fügen Sie einen Verweis auf die Assembly Moq dem Testprojekt, indem Sie durch Auswählen der Menüoption **-Projekt "," Verweis hinzufügen** zum Öffnen der **Verweis hinzufügen** Dialogfeld. Navigieren Sie zum Ordner, in dem Sie Moq entpackt haben, und wählen Sie die Moq.dll-Assembly, unter der Registerkarte Durchsuchen. Klicken Sie auf die **OK** Schaltfläche (siehe Abbildung 2).


[![Das Aufheben der Blockierung Moq](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Abbildung 01**: Aufheben der Blockierung Moq ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-5-create-unit-tests-vb/_static/image2.png))


[![Verweise, die nach dem Hinzufügen von Moq](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Abbildung 02**: Verweise nach dem Hinzufügen von Moq ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-5-create-unit-tests-vb/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Erstellen von Komponententests für die Dienstschicht

Lassen Sie zunächst erstellen Sie einen Satz von Komponententests für unseren Contact Manager s anwendungsdienstebene s ein. Wir verwenden diese Tests, um zu überprüfen, ob unsere Validierungslogik.

Erstellen Sie einen neuen Ordner namens Modelle im Projekt ContactManager.Tests. Als Nächstes mit der rechten Maustaste in den Ordner "Models", und wählen Sie **hinzufügen "," neuen Test**. Die **neuen Test hinzufügen** in Abbildung 3 dargestellte Dialogfeld wird angezeigt. Wählen Sie die **Komponententest** Vorlage und den neuen Test ContactManagerServiceTest.vb nennen. Klicken Sie auf die **OK** , um den neuen Test zu Ihrem Testprojekt hinzuzufügen.

> [!NOTE] 
> 
> Im Allgemeinen sollten Sie die Ordnerstruktur des Projekts Test in der Ordnerstruktur Ihrer ASP.NET MVC-Projekts entsprechen. Sie können z. B. Controller-Tests in einen Ordner "Controllers", Modell-Tests in einem Ordner "Models" platzieren und so weiter.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Abbildung 03**: Models\ContactManagerServiceTest.cs ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-5-create-unit-tests-vb/_static/image6.png))


Anfänglich, möchten wir uns die CreateContact()-Methode, die von der ContactManagerService-Klasse verfügbar gemacht werden. Wir erstellen die folgenden fünf Tests:

- CreateContact() - Tests, CreateContact() gibt den Wert True zurück, wenn ein gültiger Kontakt an die Methode übergeben wird.
- CreateContactRequiredFirstName() - Tests, dass eine Fehlermeldung Modellstatus, wenn eine Verbindung mit einen fehlenden Vornamen hinzugefügt wird an die CreateContact()-Methode übergeben.
- CreateContactRequredLastName() - Tests, dass eine Fehlermeldung Modellstatus, wenn ein Kontakt mit einem fehlenden Nachnamen hinzugefügt wird an die CreateContact()-Methode übergeben.
- CreateContactInvalidPhone() - Tests, dass die Modellstatus, wenn eine Verbindung mit einer ungültigen Telefonnummer eine Fehlermeldung hinzugefügt wird an die CreateContact()-Methode übergeben.
- CreateContactInvalidEmail() - Tests, dass eine Fehlermeldung Modellstatus, wenn ein Kontakt mit der eine ungültige e-Mail-Adresse hinzugefügt wird an die CreateContact()-Methode übergeben...

Der erste Test überprüft, dass ein gültiger Kontakt keine Überprüfung ein Fehler generiert. Überprüfen Sie die verbleibenden Tests aller Validierungsregeln.

Der Code für diese Tests ist in Codebeispiel 1 enthalten.

**Wenn Sie planen, erstellen Sie Komponententests für Ihre Controller müssen Sie explizite Ansichtsnamen aus Ihre Controlleraktionen zurückzugeben.**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]


Beispielsweise geben eine Ansicht wie folgt zurück: Zurückgeben von View()


Stattdessen geben Sie die Ansicht wie folgt zurück: Zurückgeben von View("Create") Wenn Sie nicht explizit sind bei der Rückgabe einer Ansicht klicken Sie dann zurückgegeben die ViewResult.ViewName-Eigenschaft eine leere Zeichenfolge.

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Codebeispiel 2 - Controllers\ContactControllerTest.vb In dieser Iteration haben wir Komponententests für die Kontakt-Manager-Anwendung erstellt. Wir können diese Komponententests ausführen, um sicherzustellen, dass die Anwendung weiterhin in die Art und Weise verhält sich, die wir erwarten, dass jederzeit.

> [!NOTE] 
> 
> Die Komponententests fungieren als Sicherheitsnetz für die Anwendung ermöglicht uns, unsere Anwendung in der Zukunft sicher zu ändern. Wir haben zwei Sätze von Komponententests erstellt. Zunächst haben wir unsere Validierungslogik durch Erstellen von Komponententests für unseren Dienstebene getestet.


Als Nächstes können wir unsere datenflusskontrolllogik durch Erstellen von Komponententests für unseren Controller-Ebene getestet. Beim Testen unsere Dienstebene isoliert wir unsere Tests für unseren Dienstebene aus unserem Repository-Ebene durch unsere Repositoryschicht imitieren.

Wenn Sie die Controller-Ebene zu testen, isoliert wir unsere Tests für unsere Controller-Ebene durch Simulieren von der Dienstebene. In der nächsten Iteration ändern wir die Kontakt-Manager-Anwendung, damit sie wenden Sie sich an Gruppen unterstützt. Wir werden unsere Anwendung, die unter Verwendung der Software Design sogenannten testgesteuerte Entwicklung dieser neuen Funktionalität hinzufügen. Das heißt, ist eine Methode, die mit dem [TestMethod]-Attribut ergänzt wird ein Komponententest.

Der erste Komponententest, mit dem Namen CreateContact(), stellt sicher, dass der Wert "true" beim Aufrufen von CreateContact() zurückgegeben werden, wenn eine gültige Instanz von der Contact-Klasse an die Methode übergeben wird. Der Test erstellt eine Instanz der Klasse wenden Sie sich an, ruft die CreateContact()-Methode auf und stellt sicher, dass CreateContact() den Wert True zurückgibt.

Die verbleibenden Tests stellen Sie sicher, dass, wenn die CreateContact()-Methode, mit einer ungültigen Kontakt aufgerufen wird Klicken Sie dann die Methode "false" zurückgibt und die erwarteten Überprüfungsfehlermeldung Modellzustand hinzugefügt. Der CreateContactRequiredFirstName() Test erstellt z. B. eine Instanz der Klasse wenden Sie sich an, mit einer leeren Zeichenfolge für die FirstName-Eigenschaft. Als Nächstes wird die CreateContact()-Methode mit dem ungültigen Kontakt aufgerufen. Zum Schluss der Test überprüft, ob CreateContact() "false" zurückgibt und Modellstatus auf der erwartete Überprüfungsfehlermeldung enthält "Vorname ist erforderlich."

Sie können die Komponententests in Codebeispiel 1 ausführen, indem Sie durch Auswählen der Menüoption **Test ausführen, alle Tests in der Projektmappe (STRG + R, A)**. Die Ergebnisse der Tests werden im Fenster Testergebnisse angezeigt (siehe Abbildung 4).


[![Testergebnisse](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Abbildung 04**: Testergebnisse ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-5-create-unit-tests-vb/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Erstellen von Komponententests für Controller

ASP.NET MVC-Anwendung steuern den Fluss der Benutzerinteraktion. Wenn Sie einen Controller zu testen, möchten Sie zu prüfen, ob der Controller die richtige Aktion Ergebnis und die Ansicht Daten zurückgibt. Möglicherweise möchten Sie auch überprüfen, ob ein Controller mit Modellklassen in die Art und Weise erwartet interagiert.

Codebeispiel 2 enthält beispielsweise zwei Komponententests für den Kontakt Controller Create()-Methode. Der erste Komponententest überprüft, ob, wenn ein gültiger Kontakt an die Create()-Methode übergeben wird, und klicken Sie dann die Create()-Methode an die Index-Aktion umleitet. Das heißt, sollten, wenn einen gültigen Kontakt zu übergeben, die Create()-Methode eine RedirectToRouteResult zurück, die Index-Aktion darstellt.

Wir raten t die ContactManager-Dienstebene zu testen, wenn wir die Controller-Ebene testen möchten. Aus diesem Grund simulieren wir die Dienstschicht mit den folgenden Code in der Initialize-Methode:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

In den Komponententest CreateValidContact() simulieren wir das Verhalten von Aufrufen der Dienstschicht CreateContact()-Methode, mit der folgenden Zeile des Codes:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Diese Codezeile führt dazu, dass den mock ContactManager-Dienst den Wert True zurückgegeben, wenn seine CreateContact()-Methode aufgerufen wird. Durch simulieren die Dienstebene, können wir das Verhalten des Controllers testen, ohne Code ausführen, auf der Dienstebene.

Der zweite Unit Test stellt sicher, dass die Aktion Create() Ansicht "erstellen" zurückgibt, wenn eine ungültige wenden Sie sich an, an die Methode übergeben wird. Wir führen die Dienstschicht CreateContact()-Methode, um den Wert "false" mit der folgenden Zeile des Codes zurückzugeben:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Wenn die Methode verhält sich wie wir dann erwarten Create() Ansicht "erstellen" zurückgeben soll, wenn die Dienstebene der Wert "false" zurückgegeben. Auf diese Weise kann der Controller Validierungsfehlermeldungen in der Create-Ansicht und der Benutzer hat die Möglichkeit, zu beheben, wenden Sie sich an wurden ungültige Eigenschaften.


Wenn Sie planen, erstellen Sie Komponententests für Ihre Controller müssen Sie explizite Ansichtsnamen aus Ihre Controlleraktionen zurückzugeben. Beispielsweise geben eine Ansicht wie folgt zurück:

Zurückgeben von View()

Stattdessen geben Sie die Ansicht wie folgt zurück:

Zurückgeben von View("Create")

Wenn Sie nicht explizit sind bei der Rückgabe einer Ansicht klicken Sie dann zurückgegeben die ViewResult.ViewName-Eigenschaft eine leere Zeichenfolge.


**Codebeispiel 2 - Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Zusammenfassung

In dieser Iteration haben wir Komponententests für die Kontakt-Manager-Anwendung erstellt. Wir können diese Komponententests ausführen, um sicherzustellen, dass die Anwendung weiterhin in die Art und Weise verhält sich, die wir erwarten, dass jederzeit. Die Komponententests fungieren als Sicherheitsnetz für die Anwendung ermöglicht uns, unsere Anwendung in der Zukunft sicher zu ändern.

Wir haben zwei Sätze von Komponententests erstellt. Zunächst haben wir unsere Validierungslogik durch Erstellen von Komponententests für unseren Dienstebene getestet. Als Nächstes können wir unsere datenflusskontrolllogik durch Erstellen von Komponententests für unseren Controller-Ebene getestet. Beim Testen unsere Dienstebene isoliert wir unsere Tests für unseren Dienstebene aus unserem Repository-Ebene durch unsere Repositoryschicht imitieren. Wenn Sie die Controller-Ebene zu testen, isoliert wir unsere Tests für unsere Controller-Ebene durch Simulieren von der Dienstebene.

In der nächsten Iteration ändern wir die Kontakt-Manager-Anwendung, damit sie wenden Sie sich an Gruppen unterstützt. Wir werden unsere Anwendung, die unter Verwendung der Software Design sogenannten testgesteuerte Entwicklung dieser neuen Funktionalität hinzufügen.

> [!div class="step-by-step"]
> [Zurück](iteration-4-make-the-application-loosely-coupled-vb.md)
> [Weiter](iteration-6-use-test-driven-development-vb.md)
