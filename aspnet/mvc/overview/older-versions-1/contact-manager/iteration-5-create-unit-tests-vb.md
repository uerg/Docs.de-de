---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: "Iteration #5 – Erstellen von Komponententests (VB) | Microsoft Docs"
author: microsoft
description: "In der fünften Iteration stellen wir unsere Anwendung einfacher zu verwalten und ändern, indem Sie Komponententests hinzufügen. Wir unsere Daten Modellklassen modellieren und Erstellen von Komponententests für o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: ab9ff5629cb468b785f5b82178f9f6247a55cacb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="iteration-5--create-unit-tests-vb"></a>Iteration #5: Generieren von Komponententests (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[Herunterladen von Code](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> In der fünften Iteration stellen wir unsere Anwendung einfacher zu verwalten und ändern, indem Sie Komponententests hinzufügen. Wir unsere Daten Modellklassen modellieren und erstellen Sie Komponententests für unsere Controller und die Validierungslogik.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Erstellen einer Kontaktverwaltung ASP.NET MVC-Anwendung (VB)

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

In vorherigen Iterationen der Kontakt-Manager-Anwendung umgestaltet wir die Anwendung mehr lose miteinander verbunden werden. Wir getrennt die Anwendung in unterschiedliche Controller-Dienst und Repository Ebenen. Jede Ebene die Interaktion mit der Ebene darunter über Schnittstellen.

Wir haben umgestaltet, die Anwendung, damit die Anwendung leichter verwalten und ändern. Z. B. wenn wir eine neue datenzugriffstechnologie verwenden müssen, können wir einfach die Repository-Ebene ändern, ohne durch berühren der Controller oder die Dienstebene. Die Kontakt-Manager lose gekoppelt, machen wir Ve versucht die Anwendung stabiler zu ändern.

Aber was geschieht, wenn wir ein neues Feature für die Kontakt-Manager-Anwendung hinzufügen müssen? Oder, was geschieht, wenn es einen Fehler beheben? Ein Wahrheitswert sad, aber auch bewährte beim Schreiben von Code ist, wenn Sie Code, die Sie erstellen, das Risiko von neuen Fehlern berühren.

Z. B. Fragen ein Tag, Ihre Vorgesetzte Sie sich, ein neues Feature in der Contact-Manager hinzufügen. Sie möchte die Unterstützung für Kontaktgruppen hinzuzufügen. Möchte Ihnen das Aktivieren von Benutzern, ihre Kontakte in Gruppen wie z. B. Freunde, Geschäfts- und So weiter zu organisieren.

Um dieses neue Feature zu implementieren, müssen Sie alle drei Ebenen der Kontakt-Manager-Anwendung zu ändern. Sie müssen die Domänencontroller, die Dienstebene und das Repository neue Funktionalität hinzufügen. Sobald Sie das Ändern von Code beginnen, besteht das Risiko, wichtige Funktionen, die vor dem gearbeitet hat.

Umgestaltung unsere Anwendung in verschiedenen Ebenen, wie in vorherigen Iterationen ist gut. Da wir ganze Ebenen ändern, ohne den Rest der Anwendung können war gut. Wenn Sie den Code innerhalb einer Ebene leichter verwalten und ändern möchten, müssen Sie jedoch Komponententests für den Code zu erstellen.

Sie verwenden einer Einheit Test zu Test eine einzelne Einheit von Code. Diese Einheiten des Codes sind kleiner als der gesamte Anwendungsebenen. Normalerweise verwenden Sie einen Komponententest um zu überprüfen, ob eine bestimmte Methode im Code auf die Weise verhält, die Sie erwarten. Beispielsweise würden Sie einen Komponententest für die CreateContact()-Methode, die von der ContactManagerService-Klasse verfügbar gemacht werden erstellen.

Komponententests für eine Anwendung Arbeit einfach wie ein Sicherheitsnetz. Wenn Sie Code in einer Anwendung ändern, können Sie einen Satz von Komponententests zu überprüfen, ob die Änderung die vorhandene Funktionalität unterbricht ausführen. Komponententests Lesbarkeit Ihres Codes sicher ist, ändern. Komponententests stellen alle des Codes in Ihrer Anwendung stabiler zu ändern.

In dieser Iteration werden Komponententests für unsere Kontakt-Manager-Anwendung hinzufügen. Auf diese Weise können in der nächsten Iteration wir Kontakt zu Gruppen unsere Anwendung hinzufügen ohne wichtige vorhandenen Funktionen kümmern zu müssen.

> [!NOTE] 
> 
> Es gibt eine Vielzahl von Komponententest-Frameworks wie NUnit, xUnit.net und MbUnit. In diesem Lernprogramm verwenden wir die UnitTest-Framework mit Visual Studio enthalten. Allerdings können Sie genauso einfach eines dieser Alternativen Frameworks verwenden.


## <a name="what-gets-tested"></a>Was ruft getestet

In der Idealerweise würden alle des Codes durch Komponententests abgedeckt werden. In der idealerweise müssten Sie die perfekte Sicherheitsnetz. Sie können wäre kann jede Codezeile in der Anwendung ändern, und wissen sofort, durch die Komponententests ausführen, ob die Änderung die vorhandene Funktionalität unterbrochen wurde.

Allerdings Verschlüsselungskennwort wir idealerweise live t. In der Praxis beim Schreiben von Komponententests konzentrieren Sie sich zum Schreiben von Tests für Ihre Geschäftslogik (z. B. Validierungslogik). Insbesondere Sie *nicht* Schreiben von Komponententests für Ihre Daten Logik oder Ihre ansichtslogik zugreifen.

Um nützlich zu sein, müssen sehr schnelle Ausführen von Komponententests. Sie können problemlos Hunderte (oder sogar Tausende) von Komponententests für eine Anwendung kumuliert werden. Wenn die Komponententests ausführen dauert müssen Sie vermeiden, ausgeführt werden. Ausführung lang andauernder Komponententests sind also für den Tag zu Tag codierungszwecke nutzlos.

Aus diesem Grund Sie in der Regel nicht Komponententests für Code schreiben, die mit einer Datenbank interagiert. Hunderte von Komponententests für eine live-Datenbank ausgeführt wird, würde zu langsam sein. Stattdessen modellieren die Datenbank und Code schreiben, der die simulierten Datenbank interagiert (erörtert, simulieren eine Datenbank, die weiter unten).

Eine ähnliche Grund sind in der Regel Komponententests für Sichten nicht geschrieben werden. Um eine Ansicht zu testen, müssen Sie einen Webserver zu starten. Da Spinvorgänge Einrichten eines Webservers prozessspezifisch relativ langsam ist, wird das Erstellen von Komponententests für Ihre Ansichten nicht empfohlen.

Wenn Ihre Sicht komplexe Logik enthält dann sollten Sie die Logik in Hilfsmethoden verschoben. Sie können Komponententests für Hilfsmethoden schreiben, die ohne sich um einen Webserver ausgeführt.

> [!NOTE] 
> 
> Beim Schreiben von für Datenzugriffslogik Tests oder ansichtslogik nicht empfehlenswert, ist wenn Komponententests schreiben, können diese Tests sehr nützlich sein, beim Erstellen von funktionalen oder Integration testet.


> [!NOTE] 
> 
> ASP.NET MVC ist das Web Forms-Ansichtsmodul. Während der Web Forms-Ansichtsmodul auf einem Webserver abhängig ist, möglicherweise andere Ansichtsmodule nicht.


## <a name="using-a-mock-object-framework"></a>Mithilfe eines simulierten-Objekt

Wenn Sie Komponententests erstellen, müssen Sie fast immer ein Objekt modellieren Framework nutzen. Ein Objekt modellieren-Framework können Sie Mocks und Stubs für die Klassen in Ihrer Anwendung zu erstellen.

Ein Objekt modellieren-Framework können Sie z. B. um eine Pseudoversion des Repository-Klasse zu generieren. Auf diese Weise können Sie das pseudorepository-Klasse anstelle der tatsächlichen Repository-Klasse in Ihren Komponententests verwenden. Das pseudorepository mit können Sie die Ausführung von Datenbankcode beim Ausführen eines Komponententests zu vermeiden.

Visual Studio enthält ein Objekt modellieren Framework keine. Es gibt jedoch mehrere kommerzielle und open-Source-Objekt modellieren-Frameworks für .NET Framework verfügbar:

1. Moq - dieses Framework wird unter der open-Source-BSD-Lizenz verfügbar. Download von Moq [https://code.google.com/p/moq/](https://code.google.com/p/moq/).
2. Rhino Mocks – dieses Framework finden Sie unter der open-Source-BSD-Lizenz. Download Rhino Mocks aus [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock Isolator - ist dies ein kommerziellen Framework. Sie können eine Testversion von herunterladen [http://www.typemock.com/](http://www.typemock.com/).

In diesem Lernprogramm möchte ich Moq verwenden. Allerdings können Sie genauso einfach verwenden Rhino Mocks oder Typemock Isolator das Mock Erstellen von Objekten für die Kontakt-Manager-Anwendung.

Bevor Sie Moq verwenden können, müssen Sie die folgenden Schritte ausführen:

1. .
2. Bevor Sie den Download entpacken, stellen Sie sicher, dass Sie mit der rechten Maustaste in der das, und klicken Sie auf die Schaltfläche "mit der Bezeichnung" **zum Aufheben der Sperre** (siehe Abbildung 1).
3. Entpacken Sie das herunterladen.
4. Fügen Sie einen Verweis auf die Assembly Moq dem Testprojekt, indem Sie im Menü die Option **Projekt "," Verweis hinzufügen** So öffnen die **Verweis hinzufügen** Dialogfeld. Navigieren Sie unter der Registerkarte "Durchsuchen" zu dem Ordner, in dem Sie Moq entzippt, und wählen Sie die Assembly Moq.dll. Klicken Sie auf die **OK** Schaltfläche (siehe Abbildung 2).


[![Das Aufheben der Blockierung Moq](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Abbildung 01**: das Aufheben der Blockierung Moq ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-5-create-unit-tests-vb/_static/image2.png))


[![Nach dem Hinzufügen der Moq Verweise](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Abbildung 02**: Verweise nach dem Hinzufügen der Moq ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-5-create-unit-tests-vb/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Erstellen von Komponententests für die Dienstebene

Lassen Sie s starten, indem Sie eine Reihe von Komponententests für unsere Kontakt-Manager-s-Dienst-Anwendungsebene erstellen. Wir verwenden diese Tests Überprüfungslogik zu überprüfen.

Erstellen Sie einen neuen Ordner namens Modelle im Projekt ContactManager.Tests. Als Nächstes mit der rechten Maustaste in den Ordner Models, und wählen Sie **hinzufügen, neue Test**. Die **neuen Test hinzufügen** Dialogfeld wie in Abbildung 3 wird angezeigt. Wählen Sie die **Komponententest** Vorlage und Ihren neuen Test ContactManagerServiceTest.vb nennen. Klicken Sie auf die **OK** Schaltfläche, um Ihren neuen Test zu Ihrem Testprojekt hinzuzufügen.

> [!NOTE] 
> 
> Im Allgemeinen möchten Sie die Ordnerstruktur des Projekts Test in der Ordnerstruktur des ASP.NET MVC-Projekts entsprechen. Z. B. Controller Tests in einen Ordner Controller, Modell-Tests in einem Ordner Modelle platzieren und so weiter.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Abbildung 03**: Models\ContactManagerServiceTest.cs ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-5-create-unit-tests-vb/_static/image6.png))


Zu Beginn möchten wir testen die CreateContact()-Methode, die von der ContactManagerService-Klasse verfügbar gemacht werden. Wir erstellen die folgenden fünf Tests:

- CreateContact() - Tests, CreateContact() gibt den Wert "true" zurück, wenn ein gültiger Kontakt an die Methode übergeben wird.
- CreateContactRequiredFirstName() - Tests, dass eine Fehlermeldung Modellstatus, wenn ein Kontakt mit einem fehlenden Vornamen hinzugefügt wird an die CreateContact()-Methode übergeben.
- CreateContactRequredLastName() - Tests, dass eine Fehlermeldung Modellstatus, wenn ein Kontakt mit einem fehlenden Nachname hinzugefügt wird an die CreateContact()-Methode übergeben.
- CreateContactInvalidPhone() - Tests, dass eine Fehlermeldung Modellstatus, wenn eine Verbindung mit einer ungültigen Telefonnummer hinzugefügt wird an die CreateContact()-Methode übergeben.
- CreateContactInvalidEmail() - Tests, dass eine Fehlermeldung Modellstatus, wenn eine Verbindung mit der eine ungültige e-Mail-Adresse hinzugefügt wird an die CreateContact()-Methode übergeben...

Der erste Test prüft, dass ein gültiger Kontakt keine tritt ein Validierungsfehler generiert. Überprüfen Sie jede der Validierungsregeln, die verbleibenden Tests.

Der Code für diese Tests ist im Codebeispiel 1 enthalten.

**1 – Models\ContactManagerServiceTest.vb auflisten**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]


Da wir die Kontakt-Klasse in Codebeispiel 1 verwenden, müssen wir unsere Testprojekt einen Verweis zu Microsoft Entity Framework hinzugefügt. Fügen Sie einen Verweis auf die Assembly System.Data.Entity.


Auflisten von 1 enthält eine Methode namens Initialize(), die mit dem [TestInitialize]-Attribut ergänzt wird. Diese Methode wird automatisch aufgerufen, bevor Komponententests ausgeführt wird (es wird 5-Mal rechts vor jedem der Komponententests bezeichnet). Die Initialize()-Methode erstellt eine simulierte Repository mit der folgenden Codezeile:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Diese Codezeile verwendet das Framework Moq, um eine simulierte Repository von der Schnittstelle IContactManagerRepository zu generieren. Das simulierte Repository wird anstelle der tatsächlichen EntityContactManagerRepository verwendet, um zu vermeiden, Zugriff auf die Datenbank aus, wenn jeder Komponententest ausgeführt wird. Das simulierte Repository implementiert die Methoden der Schnittstelle IContactManagerRepository, jedoch Methoden Stefan t eigentlich nichts.

> [!NOTE] 
> 
> Wenn Sie das Moq-Framework verwenden, besteht ein Unterschied zwischen \_MockRepository und \_mockRepository.Object. Die erste bezieht sich auf die Mock (der IContactManagerRepository)-Klasse enthält Methoden zur Angabe, wie sich das pseudorepository verhält. Der zweite Wert bezieht sich auf die tatsächlichen pseudorepository, das der IContactManagerRepository-Schnittstelle implementiert.


Das simulierte Repository wird in die Initialize()-Methode verwendet beim Erstellen einer Instanz der Klasse ContactManagerService. Alle für die einzelnen Komponententests, diese Instanz der Klasse ContactManagerService verwenden.

Auflisten von 1 enthält fünf Methoden, die jeweils von den Komponententests entsprechen. Jede dieser Methoden wird mit dem [TestMethod]-Attribut ergänzt. Wenn Sie die Komponententests ausführen, wird jede Methode, die dieses Attribut hat aufgerufen. Das heißt, ist jede Methode, die mit dem [TestMethod]-Attribut ergänzt wird ein Komponententest.

Der erste Komponententest, mit dem Namen CreateContact(), stellt sicher, dass der Wert "true" beim Aufrufen von CreateContact() zurückgegeben werden, wenn eine gültige Instanz der Klasse wenden Sie sich an die Methode übergeben wird. Der Test erstellt eine Instanz der Klasse wenden Sie sich an, ruft die Methode CreateContact() und stellt sicher, dass CreateContact() gibt den Wert "true" zurück.

Die verbleibenden Tests stellen Sie sicher, dass beim Aufrufen der CreateContact()-Methode mit einer ungültigen wenden Sie sich an die Methode "false" zurückgegeben, und die erwarteten Überprüfungsfehlermeldung Modellzustand hinzugefügt. Der Test CreateContactRequiredFirstName() erstellt z. B. eine Instanz der Klasse wenden Sie sich an mit einer leeren Zeichenfolge für die zugehörige Eigenschaft FirstName an. Als Nächstes wird die CreateContact()-Methode mit dem ungültigen Kontakt aufgerufen. Der Test abschließend vergewissert sich, dass CreateContact() "false" zurückgibt und Modellstatus auf der erwartete Überprüfungsfehlermeldung enthält "Vorname ist erforderlich."

Sie können die Komponententests in Codebeispiel 1 ausführen, indem Sie im Menü die Option **Test führen Sie alle Tests in der Projektmappe (STRG + R, A)**. Die Ergebnisse der Tests werden im Fenster Testergebnisse angezeigt (siehe Abbildung 4).


[![Testergebnisse](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Abbildung 04**: Testergebnisse ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-5-create-unit-tests-vb/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Erstellen von Komponententests für Domänencontroller

ASP.NET MVC-Anwendung kontrollieren des Flusses Eingreifen des Benutzers. Beim Testen eines Controllers möchten Sie testen, ob der Controller die richtigen Aktion Ergebnis und die Ansicht Daten zurückgibt. Möglicherweise möchten Sie auch überprüfen, ob ein Controller mit Modellklassen in der erwarteten Weise interagiert.

Auflisten von 2 enthält beispielsweise zwei Komponententests für den Kontakt Controller Create()--Methode. Der erste Komponententest stellt sicher, dass wenn ein gültiger Kontakt an die Create()--Methode übergeben wird, und klicken Sie dann die Create()--Methode, die Index-Aktion umleitet. Das heißt, sollten wenn einen gültigen Kontakt zu übergeben, die Create()--Methode eine RedirectToRouteResult zurück, die Index-Aktion darstellt.

Wir raten t ContactManager-Dienstebene zu testen, wenn wir die Controller-Ebene testen möchten. Daher modellieren wir die Dienstschicht mit den folgenden Code in die Initialize-Methode:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

In den Komponententest CreateValidContact() modellieren wir das Verhalten von aufrufenden die Dienstebene CreateContact()-Methode, mit der folgenden Codezeile:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Diese Codezeile führt dazu, dass den simulierten ContactManager-Dienst den Wert "true" zurückgeben, wenn seine CreateContact()-Methode aufgerufen wird. Durch simulieren die Dienstebene, können das Verhalten des unser Controller testen, ohne Ausführen von Code in der Dienstebene.

Im zweite Komponententest wird überprüft, ob die Create()--Aktion die Erstellungsansicht zurück, wenn Sie ein ungültiger Kontakt an die Methode übergeben wird. Wir führen die Dienstebene CreateContact()-Methode, um den Wert "false", mit der folgenden Zeile des Codes zurückgeben:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Wenn die Methode Create()-verhält sich wie erwartet sollte er die Erstellungsansicht zurückgeben, wenn die Dienstebene der Wert "false" zurückgibt. Auf diese Weise kann der Controller die Überprüfungsfehlermeldungen in die Erstellungsansicht und der Benutzer hat die Möglichkeit, diese ungültigen Kontakteigenschaften korrigieren.


Wenn Sie Komponententests für Ihre Domänencontroller erstellen möchten, müssen Sie explizite Sichtnamen aus Ihrer Controlleraktionen zurückzugeben. Beispielsweise geben eine Ansicht wie folgt zurück:

View() zurückgeben

Geben Sie die Sicht wie folgt:

View("Create") zurückgeben

Wenn Sie nicht explizit sind bei der Rückgabe einer Sicht werden für die ViewResult.ViewName-Eigenschaft eine leere Zeichenfolge zurückgegeben.


**Auflisten von 2 – Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Zusammenfassung

In dieser Iteration wurde Komponententests für unsere Kontakt-Manager-Anwendung erstellt. Wir können diese Komponententests ausführen, um sicherzustellen, dass die Anwendung weiterhin auf die Weise verhält sich, das wir voraussichtlich jederzeit. Die Komponententests fungieren als Sicherheitsnetz für unsere Anwendung aktivieren uns so ändern Sie die Anwendung in der Zukunft sicher.

Wir erstellt zwei Sätze von Komponententests. Zunächst haben wir Überprüfungslogik durch das Erstellen von Komponententests für unsere Dienstschicht getestet. Als Nächstes haben wir unsere datenflusskontrolllogik durch das Erstellen von Komponententests für unsere Controller-Ebene getestet. Beim Testen unserer Dienstschicht isoliert wir unsere Tests für unsere Dienstschicht aus unserem Repository-Ebene durch unsere Repository-Ebene zu simulieren. Beim Testen der Controller-Ebene isoliert wir unsere Tests für unsere Controller Ebene durch die Dienstebene imitieren.

In der nächsten Iteration ändern wir die Kontakt-Manager-Anwendung, sodass er Kontaktgruppen unterstützt. Diese neue Funktionalität fügen wir in unserer-Anwendung mit einer Software-Entwurfsprozess testorientierte Entwicklung aufgerufen.

>[!div class="step-by-step"]
[Zurück](iteration-4-make-the-application-loosely-coupled-vb.md)
[Weiter](iteration-6-use-test-driven-development-vb.md)
