---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
title: "Iteration #4 – Stellen Sie die Anwendung lose (VB) | Microsoft Docs"
author: microsoft
description: "In dieser dritten Iteration nutzen wir mehrere Software-Entwurfsmuster, damit sie leichter zu verwalten und ändern die Kontakt-Manager-Anwendung. Für ..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 92c70297-4430-4e4e-919a-9c2333a8d09a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c11c89710723c133a306aaf56cc8797cc036475
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="iteration-4--make-the-application-loosely-coupled-vb"></a>Iteration #4 – Stellen Sie die Anwendung lose (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[Herunterladen von Code](iteration-4-make-the-application-loosely-coupled-vb/_static/contactmanager_4_vb1.zip)

> In dieser dritten Iteration nutzen wir mehrere Software-Entwurfsmuster, damit sie leichter zu verwalten und ändern die Kontakt-Manager-Anwendung. Beispielsweise gestalten wir unsere Anwendung in das Repository und die Abhängigkeitsinjektion-Muster verwenden.


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

In diesem vierten Iteration der Kontakt-Manager-Anwendung gestalten wir die Anwendung, damit die Anwendung mehr lose miteinander verbunden. Wenn eine Anwendung lose gekoppelt ist, können Sie den Code in einem Teil der Anwendung ändern, ohne den Code in anderen Teilen der Anwendung ändern. Lose verbundene Anwendungen sind stabiler zu ändern.

Derzeit sämtliche die Daten Zugriff und Validierung Logik, anhand der Kontakt-Manager-Anwendung befinden sich in den Controllerklassen. Dies ist eine gute Idee. Wenn Sie einen Teil Ihrer Anwendung ändern müssen, besteht das Risiko, einführen von Fehlern in einem anderen Teil der Anwendung. Z. B. Wenn Sie Ihre Validierungslogik ändern, besteht das Risiko von neuen Fehlern in Ihre Daten zugreifen oder Controller Logik.

> [!NOTE] 
> 
> (Policies, SRP), eine Klasse verfügen niemals mehr als ein Grund zum Ändern. Mischen von Controller, Überprüfung und Datenbanklogik ist eine massive Verstoß gegen das Prinzip der einzelnen Verantwortung.


Es gibt verschiedene Gründe, die möglicherweise so ändern Sie Ihre Anwendung ein. Müssen Sie möglicherweise ein neues Feature für Ihre Anwendung hinzufügen, müssen Sie möglicherweise einen Fehler in der Anwendung zu beheben, oder müssen Sie möglicherweise ändern, wie eine Funktion der Anwendung implementiert wird. Anwendungen sind selten statisch. Sie tendenziell eher Wachstum und mit der Zeit verändert wird.

Stellen Sie sich vor, z. B., dass Sie die Implementierung der Datenzugriffsebene ändern möchten. Rechts verwendet jetzt die Kontakt-Manager-Anwendung Microsoft Entity Framework Zugriff auf die Datenbank. Sie könnten jedoch zu einem neuen oder einem alternativen datenzugriffstechnologie wie ADO.NET Data Services oder NHibernate zu migrieren. Da die Datenzugriffscode nicht von der Überprüfung und Controller Code isoliert ist, besteht jedoch keine Möglichkeit, den Datenzugriffscode in Ihrer Anwendung ändern, ohne Änderung von anderem Code, der nicht direkt mit Datenzugriff verknüpft ist.

Wenn eine Anwendung lose gekoppelt ist, können andererseits, Sie Änderungen auf einen Teil einer Anwendung, ohne berühren von anderen Teilen einer Anwendung. Beispielsweise können Sie datenzugriffstechnologien wechseln, ohne Ihre Überprüfung oder Controller Logik zu ändern.

In dieser Iteration nutzen wir mehrere Software-Entwurfsmuster, mit die wir unsere Kontakt-Manager-Anwendung in einer lose gekoppelten Anwendung Umgestalten können. Wenn wir fertig sind, der Kontakt-Manager/t gewonnen keinerlei, die es t vornehmen, bevor Sie. Allerdings müssen wir die Anwendung leichter in der Zukunft ändern können.

> [!NOTE] 
> 
> Umgestaltung versteht man das Neuschreiben von einer Anwendung auf eine Weise, dass es keine vorhandene Funktionalität verloren ist.


## <a name="using-the-repository-software-design-pattern"></a>Mithilfe des Repository-Software-Entwurfsmusters

Unsere ersten Änderung wird ein Software-Entwurfsmuster wird aufgerufen, des Repositorymusters nutzen. Das Repositorymuster verwenden wir unser Datenzugriffscode vom Rest der Anwendung isolieren.

Implementieren des Repositorymusters erfordert uns die folgenden zwei Schritte ausführen:

1. Erstellen Sie eine Schnittstelle
2. Erstellen Sie eine konkrete Klasse, die die Schnittstelle implementiert.

Zunächst muss eine Schnittstelle zu erstellen, die alle Datenzugriffsmethoden beschrieben werden, die ausgeführt werden sollen. Die IContactManagerRepository-Schnittstelle, die im Codebeispiel 1 enthalten ist. Diese Schnittstelle werden fünf Methoden beschrieben: CreateContact(), DeleteContact() EditContact(), GetContact und ListContacts().

**1 – Models\IContactManagerRepository.vb auflisten**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample1.vb)]

Als Nächstes müssen wir eine konkrete Klasse zu erstellen, die IContactManagerRepository-Schnittstelle implementiert. Da das Entity Framework von Microsoft für den Datenbankzugriff verwendet wird, erstellen wir eine neue Klasse namens EntityContactManagerRepository. Diese Klasse ist im Codebeispiel 2 enthalten.

**Auflisten von 2 – Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample2.vb)]

Beachten Sie, dass die Klasse EntityContactManagerRepository IContactManagerRepository-Schnittstelle implementiert. Die Klasse implementiert alle fünf Methoden, die von dieser Schnittstelle beschrieben.

Sie Fragen sich vielleicht, warum wir mit einer Schnittstelle kümmern müssen. Warum müssen wir erstellen eine Schnittstelle und eine Klasse, die ihn implementiert?

Die restliche Anwendung interagieren mit einer Ausnahme mit der Schnittstelle und nicht die konkrete Klasse. Anstelle von Aufrufen der Methoden, die von der EntityContactManagerRepository-Klasse verfügbar gemacht werden, rufen wir die Methoden, die von der IContactManagerRepository-Schnittstelle verfügbar gemacht werden.

Auf diese Weise können wir die Schnittstelle mit einer neuen Klasse implementieren, ohne den Rest der Anwendung zu ändern. Möglicherweise möchten wir z. B. späteren Zeitpunkt eine DataServicesContactManagerRepository Klasse implementieren, die die IContactManagerRepository-Schnittstelle implementiert. Die DataServicesContactManagerRepository-Klasse verwendet den Zugriff auf eine Datenbank anstelle der Entity Framework von Microsoft ADO.NET Data Services.

Wenn unsere Anwendungscode für die Schnittstelle IContactManagerRepository anstelle der konkreten EntityContactManagerRepository-Klasse so programmiert ist, können wir konkrete Klassen wechseln, ohne den Rest des getesteten Codes ändern. Beispielsweise können wir von der Klasse EntityContactManagerRepository der DataServicesContactManagerRepository-Klasse wechseln, ohne unsere Daten Zugriff oder Überprüfung der Logik zu ändern.

Programmieren mit Schnittstellen (Abstraktionen) statt konkrete Klassen macht unsere Anwendung stabiler zu ändern.

> [!NOTE] 
> 
> Sie können schnell eine Schnittstelle aus eine konkrete Klasse in Visual Studio erstellen, im Menü die Option Umgestaltung Schnittstelle extrahieren. Sie können beispielsweise erstellen Sie zuerst die EntityContactManagerRepository-Klasse und klicken Sie dann mit der Schnittstelle extrahieren die IContactManagerRepository-Schnittstelle automatisch generiert.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Mithilfe des Dependency Injection-Software-Entwurfsmusters

Wir unsere Datenzugriffscode in eine separate Klasse für Repository migriert haben, müssen wir unser Controller Kontakt zum Verwenden dieser Klasse zu ändern. Es wird ein Software-Entwurfsmuster aufgerufen Abhängigkeitsinjektion zur Verwendung der Repository-Klasse in unserer Controller nutzen.

Der geänderte Kontakt Controller wird in 3 auflisten enthalten.

**3 – Controllers\ContactController.vb auflisten**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample3.vb)]

Beachten Sie, dass die Kontakt-Controller in 3 Auflisten von zwei Konstruktoren ist. Der erste Konstruktor übergibt eine konkrete Instanz die IContactManagerRepository-Schnittstelle, mit dem zweiten Konstruktor. Der Kontakt-Controller-Klasse verwendet *Konstruktor Abhängigkeitsinjektion*.

Der erste Konstruktor wird der und nur die Stelle, dass die EntityContactManagerRepository-Klasse verwendet wird. Der Rest der Klasse verwendet die IContactManagerRepository-Schnittstelle anstelle der konkreten EntityContactManagerRepository-Klasse.

Dies vereinfacht die Implementierung der IContactManagerRepository-Klasse in der Zukunft zu wechseln. Wenn Sie die Klasse DataServicesContactRepository anstelle der EntityContactManagerRepository-Klasse verwenden möchten, ändern Sie nur des ersten Konstruktors.

Abhängigkeitsinjektion Konstruktor macht die Controllerklasse erhalten Sie auch sehr getestet werden können. In den Komponententests können Sie die Kontakt-Controller instanziieren, durch eine pseudoimplementierung des IContactManagerRepository-Klasse übergeben. Diese Funktion abhängigkeiteneinschleusung wird uns in der nächsten Iteration sehr wichtig sein, wenn wir Komponententests für die Kontakt-Manager-Anwendung erstellen.

> [!NOTE] 
> 
> Wenn die Controllerklasse erhalten Sie über eine bestimmte Implementierung der Schnittstelle IContactManagerRepository vollständig entkoppeln werden sollen können dann Sie ein Framework nutzen, die die Abhängigkeitsinjektion z. B. StructureMap oder Microsoft unterstützt Entity Framework (MEF). Durch eine Abhängigkeitsinjektion Framework nutzen, müssen Sie niemals auf eine konkrete Klasse in Ihrem Code verweisen.


## <a name="creating-a-service-layer"></a>Erstellen einer Dienstebene

Möglicherweise haben Sie bemerkt, dass Überprüfungslogik weiterhin mit unserem Controllerlogik in der geänderten Controllerklasse auflisten 3 verwechselt ist. Aus demselben Grund, dass es eine gute Idee, unsere Datenzugriffslogik isoliert ist ist es eine gute Idee, Überprüfungslogik zu isolieren.

Um dieses Problem zu beheben, erstellen wir ein eigenes [Dienstschicht](http://martinfowler.com/eaaCatalog/serviceLayer.html). Die Dienstebene ist einer separaten Schicht, die zwischen unseren Controller und Repositoryklassen eingefügt werden können. Die Dienstebene enthält, einschließlich aller Überprüfungslogik Geschäftslogik.

Die ContactManagerService ist im Codebeispiel 4 enthalten. Es enthält die Validierungslogik aus der Contact-Controller-Klasse.

**4 – Models\ContactManagerService.vb auflisten**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample4.vb)]

Beachten Sie, dass der Konstruktor für die ContactManagerService eine ValidationDictionary erforderlich ist. Die Dienstschicht kommuniziert mit der Controller-Ebene über diese ValidationDictionary. Die ValidationDictionary detailliert im folgenden Abschnitt wird erörtert, wenn, die Decorator-Muster erörtert.

Beachten Sie außerdem, dass die ContactManagerService IContactManagerService-Schnittstelle implementiert. Sie sollten immer bemühen zur Programmierung für Schnittstellen keine konkrete Klassen. Andere Klassen in der Contact-Manager-Anwendung interagieren direkt nicht mit der ContactManagerService-Klasse. Stattdessen wird im weiteren Verlauf der Kontakt-Manager-Anwendung mit einer Ausnahme mit der Schnittstelle IContactManagerService programmiert.

Die IContactManagerService-Schnittstelle, die im Codebeispiel 5 enthalten ist.

**5 – Models\IContactManagerService.vb auflisten**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample5.vb)]

Die geänderte Kontakt Controllerklasse ist in 6 auflisten enthalten. Beachten Sie, dass der Controller wenden Sie sich an das Repository ContactManager nicht mehr interagiert. Stattdessen interagiert die Kontakt-Controller mit der ContactManager-Dienst. Jede Ebene wird so weit wie möglich aus anderen Ebenen isoliert.

**6 – Controllers\ContactController.vb auflisten**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample6.vb)]

Die Anwendung nicht mehr ausgeführt wird afoul der Mandant der einzelnen Verantwortung-Prinzip (SRP). Der Kontakt-Controller im Codebeispiel 6 wurde jeder Verantwortung als steuern den Fluss der Ausführung der Anwendung entfernt. Sich die Validierungslogik wurde aus der Contact-Controller entfernt und in der Dienstebene abgelegt. Die gesamte Datenbanklogik verfügt über ein Push ausgeführt wurde in der Repository-Ebene.

## <a name="using-the-decorator-pattern"></a>Mithilfe des Decorator-Musters

Wir möchten unsere Dienstebene aus unserem Controller Ebene vollständig entkoppeln können. Grundsätzlich sollten wir unsere Dienstschicht in einer separaten Assembly aus unserem Controller-Ebene zu kompilieren, ohne einen Verweis auf die MVC-Anwendung hinzufügen können.

Allerdings muss unsere Dienstschicht Validierung Fehlermeldungen an den Controller-Ebene können. Wie können wir die Dienstebene Überprüfungsfehlermeldungen kommunizieren, ohne den Controller- und Dienstebene Kopplung aktivieren? Wir nutzen ein Software-Entwurfsmuster, mit dem Namen der [Decorator-Muster](http://en.wikipedia.org/wiki/Decorator_pattern).

Ein Controller verwendet eine mit dem Namen ModelState ModelStateDictionary um Validierungsfehler darzustellen. Aus diesem Grund können Sie versucht Übergabe ModelState aus der Controller-Ebene an die Dienstschicht sein. Allerdings würde mit ModelState in der Dienstebene der Dienstebene eine Funktion von ASP.NET MVC-Framework abhängig machen. Dies wäre ungültige da irgendwann Sie ggf. die Dienstebene mit einer WPF-Anwendung statt einer ASP.NET MVC-Anwendung verwenden möchten. In diesem Fall ausreichen t ASP.NET MVC-Framework zur Verwendung der ModelStateDictionary-Klasse verweisen möchten.

Das Decorator-Muster können Sie eine vorhandene Klasse in eine neue Klasse zu umschließen, um eine Schnittstelle zu implementieren. Unsere Vorgesetzten Kontakts-Projekt enthält 7 auflisten enthaltene ModelStateWrapper-Klasse. Die ModelStateWrapper-Klasse implementiert die Schnittstelle 8 auflisten.

**Auflisten von 7 - Models\Validation\ModelStateWrapper.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample7.vb)]

**Auflisten von 8 - Models\Validation\IValidationDictionary.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample8.vb)]

Wenn Sie einen schließen Listing 5 Blick auf dann sehen Sie, dass die Dienstebene ContactManager ausschließlich die IValidationDictionary-Schnittstelle verwendet. Der ContactManager-Dienst ist nicht abhängig von dem ModelStateDictionary-Klasse. Wenn der Controller wenden Sie sich an den ContactManager-Dienst erstellt, umschließt der Controller seine ModelState wie folgt:

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample9.vb)]

## <a name="summary"></a>Zusammenfassung

In dieser Iteration wir keine neuen Funktionen nicht die Kontakt-Manager-Anwendung hinzufügen. Das Ziel dieser Iteration wurde umgestaltet werden, die Kontakt-Manager-Anwendung, dass einfacher zu verwalten und ändern.

Zunächst implementiert haben wir das Repository Software Entwurfsmuster. Wir bei der Migration aller Datenzugriffscode in eine separate ContactManager Repository-Klasse.

Es werden auch Überprüfungslogik von unserer Controllerlogik isoliert. Es erstellt eine separate Dienstebene, die alle unsere Validierungscode enthält. Die Controller-Ebene interagiert mit der Dienstebene und die Dienstebene interagiert mit der Repository-Ebene.

Wenn wir die Dienstebene erstellt, haben wir genutzt des Decorator-Musters ModelState von unserer Dienstebene zu isolieren. In unserem Dienstschicht programmiert wir die IValidationDictionary Schnittstelle anstatt ModelState.

Schließlich haben wir ein Software-Entwurfsmuster, mit dem Namen der Abhängigkeitsinjektion Muster nutzen. Dieses Muster ermöglicht es, zu programmieren Schnittstellen (Abstraktionen) statt konkrete Klassen. Implementieren des Entwurfsmusters Abhängigkeitsinjektion macht getesteten Codes auch mehr getestet werden können. In der nächsten Iteration fügen wir Komponententests unsere Projekt hinzu.

>[!div class="step-by-step"]
[Zurück](iteration-3-add-form-validation-vb.md)
[Weiter](iteration-5-create-unit-tests-vb.md)
