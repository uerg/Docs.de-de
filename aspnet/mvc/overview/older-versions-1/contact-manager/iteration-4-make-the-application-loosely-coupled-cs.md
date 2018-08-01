---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: 'Iteration #4 – Optimieren der Anwendung, die lose verknüpft wird (c#) | Microsoft-Dokumentation'
author: microsoft
description: In dieser vierten Iteration nutzen wir einige Entwurfsmuster für Software zu verwalten und ändern Sie die Kontakt-Manager-Anwendung zu vereinfachen. Für ...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c06609afd6f1adf930a377c99d66937885f78e7
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396028"
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>Iteration #4 – Optimieren der Anwendung, die lose verknüpft wird (c#)
====================
durch [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> In dieser vierten Iteration nutzen wir einige Entwurfsmuster für Software zu verwalten und ändern Sie die Kontakt-Manager-Anwendung zu vereinfachen. Z. B. gestalten wir unsere Anwendung, die dem Repositorymuster und dem Dependency Injection-Muster verwenden.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Erstellen einer Kontaktverwaltung ASP.NET MVC-Anwendung (c#)

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

In dieser vierten Iteration der Contact Manager-Anwendung gestalten wir die Anwendung, damit die Anwendung mehr lose gekoppelt. Wenn eine Anwendung lose gekoppelt ist, können Sie den Code in einem Teil der Anwendung ändern, ohne den Code in anderen Teilen der Anwendung ändern. Lose verbundene Anwendungen sind stabiler, um zu ändern.

Derzeit alle Daten zugreifen und Validierung Logik wird von der Contact Manager-Anwendung ist Bestandteil der Controller-Klassen. Dies ist keine gute Idee. Wenn Sie einen Teil Ihrer Anwendung ändern möchten, riskieren Sie Fehler in einem anderen Teil der Anwendung einführen. Z. B. Wenn Sie eine Validierungslogik ändern, besteht das Risiko von neuen Fehlern in Ihre Daten zugreifen oder Controller Logik.

> [!NOTE] 
> 
> (SRP), sollte eine Klasse nicht mehr als einen Grund zur Änderung haben. Das Kombinieren von Controller, Validierung und Datenbanklogik ist eine umfangreiche Verstoß gegen das Prinzip der einzigen Verantwortung.


Es gibt mehrere Gründe, die Sie benötigen Ihre Anwendung ändern. Müssen Sie möglicherweise ein neues Feature für Ihre Anwendung hinzufügen, müssen Sie möglicherweise einen Fehler in Ihrer Anwendung zu beheben, oder müssen Sie möglicherweise ändern, wie eine Funktion der Anwendung implementiert wird. Anwendungen sind selten statisch. Neigen dazu, vergrößern und im Laufe der Zeit verändern.

Angenommen Sie, Sie ändern, wie Sie der Datenzugriffsebene implementieren können. Rechts wird nun die Kontakt-Manager-Anwendung Microsoft Entity Framework für den Datenbankzugriff. Möglicherweise möchten jedoch auf eine neue oder alternative datenzugriffstechnologie z. B. ADO.NET Data Services oder NHibernate migrieren. Da der Datenzugriffscode nicht von der Überprüfung und Controller Code isoliert ist, besteht jedoch keine Möglichkeit zum Ändern der Datenzugriffscode in Ihrer Anwendung ohne Änderung von anderem Code, der nicht direkt auf den Datenzugriff verknüpft ist.

Bei eine Anwendung lose gekoppelt ist, können auf der anderen Seite Sie Änderungen an einem Teil einer Anwendung vornehmen ohne direkten Zugriff auf andere Teile einer Anwendung. Beispielsweise können Sie Technologien für den Datenzugriff wechseln, ohne die Überprüfung oder Controller Logik zu ändern.

In dieser Iteration nutzen wir einige Entwurfsmuster für Software, mit die wir unsere Contact Manager-Anwendung in einer zunehmend lockerer verkoppelt Anwendung Umgestalten können. Wenn wir fertig sind, Contact Manager gewonnen t nichts, die es hat t zu tun, bevor Sie. Allerdings werden wir die Anwendung leichter in der Zukunft ändern können.

> [!NOTE] 
> 
> Refactoring, ist der Prozess für das erneute Schreiben eine Anwendung so, dass es keine vorhandene Funktionalität verloren ist.


## <a name="using-the-repository-software-design-pattern"></a>Verwenden das Repository-Software-Entwurfsmuster

Die erste Änderung ist ein Software-Entwurfsmuster wird aufgerufen, das Repository-Muster nutzen. Wir verwenden das Repository-Muster, um unsere Datenzugriffscode vom Rest der Anwendung zu isolieren.

Implementieren das Repository-Muster muss die folgenden zwei Schritte ausführen:

1. Erstellen Sie eine Schnittstelle
2. Erstellen Sie eine konkrete Klasse, die die Schnittstelle implementiert

Zunächst müssen wir eine Schnittstelle zu erstellen, die alle die Methoden für den Datenzugriff beschreiben, die wir ausführen müssen. Die IContactManagerRepository-Schnittstelle, die in Codebeispiel 1 enthalten ist. Diese Schnittstelle werden fünf Methoden beschrieben: CreateContact(), DeleteContact(), EditContact(), GetContact und ListContacts().

**1 – Models\IContactManagerRepositiory.cs auflisten**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

Als Nächstes müssen wir eine konkrete Klasse zu erstellen, die IContactManagerRepository-Schnittstelle implementiert. Da wir das Microsoft Entity Framework verwenden, um Zugriff auf die Datenbank zu verwenden, erstellen wir eine neue Klasse namens EntityContactManagerRepository. Diese Klasse ist im Codebeispiel 2 enthalten.

**Codebeispiel 2 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

Beachten Sie, dass die EntityContactManagerRepository-Klasse die IContactManagerRepository-Schnittstelle implementiert. Die Klasse implementiert alle fünf der durch diese Schnittstelle beschriebenen Methoden.

Sie Fragen sich vielleicht, warum müssen wir eine Schnittstelle ist kein erforderlich. Warum brauchen wir erstellen eine Schnittstelle und eine Klasse, die er implementiert?

Der Rest der Anwendung interagiert mit einer Ausnahme mit der Schnittstelle und nicht die konkrete Klasse verwendet wird. Anstatt die Methoden, die von der EntityContactManagerRepository-Klasse verfügbar gemacht werden, rufen wir auf die Methoden, die von der IContactManagerRepository-Schnittstelle verfügbar gemacht werden.

Auf diese Weise können wir die Schnittstelle mit einer neuen Klasse implementieren, ohne den Rest der Anwendung ändern. Z. B. an einem späteren Datum möchten wir eine DataServicesContactManagerRepository-Klasse zu implementieren, die die IContactManagerRepository-Schnittstelle implementiert. ADO.NET Data Services können die DataServicesContactManagerRepository-Klasse für den Datenbankzugriff anstelle von Microsoft Entity Framework.

Wenn unserem Anwendungscode für die IContactManagerRepository-Schnittstelle anstelle der konkreten EntityContactManagerRepository Klasse programmiert ist, können wir konkrete Klassen wechseln, ohne den Rest des Codes ändern. Beispielsweise können wir von der Klasse EntityContactManagerRepository der DataServicesContactManagerRepository-Klasse wechseln, ohne unsere Daten Zugriff oder die Überprüfung der Logik zu ändern.

Programmieren mit Schnittstellen (Abstraktionen) anstelle der konkreten Klassen macht die Anwendung stabiler zu ändern.

> [!NOTE] 
> 
> Sie können eine Schnittstelle schnell von einer konkreten Klasse in Visual Studio erstellen, durch Auswählen der Menüoption Umgestaltung Schnittstelle extrahieren. Beispielsweise können Sie erstellen Sie zunächst die EntityContactManagerRepository-Klasse, und klicken Sie dann mit der Schnittstelle extrahieren die IContactManagerRepository-Schnittstelle automatisch generiert.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Mithilfe des Software-Entwurfsmusters für Dependency Injection

Nun, da wir unsere Datenzugriffscode in eine separate repositoryklasse migriert haben, müssen wir unser wenden Sie sich an Controller zur Verwendung dieser Klasse zu ändern. Es wird ein Software-Entwurfsmuster wird aufgerufen, Dependency Injection zur Verwendung der Repository-Klasse in unserem Controller nutzen.

Der geänderte Contact-Controller ist in Programmausdruck 3 enthalten.

**Codebeispiel 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

Beachten Sie, dass der Contact-Controller in Programmausdruck 3 zwei Konstruktoren. Der erste Konstruktor übergibt eine konkrete Instanz der Schnittstelle IContactManagerRepository an der zweite Konstruktor. Die Kontakt-Controller-Klasse verwendet *Konstruktorbasierte Dependency Injection*.

Der erste Konstruktor wird der und nur die Stelle, an die EntityContactManagerRepository-Klasse verwendet wird. Der Rest der Klasse verwendet die IContactManagerRepository-Schnittstelle anstelle der konkreten EntityContactManagerRepository-Klasse.

Dies erleichtert die Implementierungen der IContactManagerRepository-Klasse in der Zukunft zu wechseln. Wenn Sie die DataServicesContactRepository-Klasse anstelle der EntityContactManagerRepository-Klasse verwenden möchten, ändern Sie einfach des ersten Konstruktors.

Konstruktorbasierte Dependency Injection ist auch die Contact-Controller-Klasse sehr getestet werden. In den Komponententests können Sie den Contact-Controller durch Übergeben einer simulierten Implementierung der IContactManagerRepository-Klasse instanziieren. Dieses Feature von Dependency Injection ist sehr wichtig in der nächsten Iteration, wenn wir zum Erstellen von Komponententests für die Kontakt-Manager-Anwendung.

> [!NOTE] 
> 
> Sollten Sie die Controllerklasse wenden Sie sich an, über eine bestimmte Implementierung der Schnittstelle IContactManagerRepository vollständig zu entkoppeln können dann Sie ein Framework nutzen, die Dependency Injection, z. B. StructureMap oder Microsoft unterstützt Entitätsframework (MEF). Von einem Dependency Injection-Framework nutzen, müssen Sie nie auf eine konkrete Klasse in Ihrem Code verweisen.


## <a name="creating-a-service-layer"></a>Erstellen eine Dienstschicht

Möglicherweise haben Sie bemerkt, dass unsere Validierungslogik weiterhin mit unserem Controllerlogik in der geänderten Controllerklasse in Programmausdruck 3, wird kombiniert wird. Aus demselben Grund ist eine gute Idee, unsere Logik für den Datenzugriff zu isolieren ist es eine gute Idee, unsere Validierungslogik zu isolieren.

Um dieses Problem zu beheben, erstellen wir eine Separate [ *Dienstschicht*](http://martinfowler.com/eaaCatalog/serviceLayer.html). Die Dienstschicht ist einer separaten Schicht, die wir zwischen unsere Controller und Repositoryklassen einfügen können. Die Dienstschicht enthält unsere Geschäftslogik, einschließlich aller unserer Validierungslogik.

Die ContactManagerService ist in Listing 4 enthalten. Sie enthält die Validierungslogik aus der Contact-Controller-Klasse.

**Programmausdruck 4 - Models\ContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

Beachten Sie, dass der Konstruktor für die ContactManagerService eine ValidationDictionary erforderlich ist. Die Dienstschicht kommuniziert mit der Controller-Ebene über diese ValidationDictionary. Wenn des Decorator-Musters erörtert erläutert die ValidationDictionary im folgenden Abschnitt ausführlich.

Beachten Sie außerdem, dass die ContactManagerService der IContactManagerService-Schnittstelle implementiert. Sie sollten möglichst immer Schnittstellen statt konkreten Klassen programmieren. Andere Klassen in der Contact Manager-Anwendung interagieren mit der ContactManagerService-Klasse direkt. Stattdessen wird der Rest der Contact Manager-Anwendung mit einer Ausnahme mit der Schnittstelle IContactManagerService programmiert.

Die IContactManagerService-Schnittstelle, die in Listing 5 enthalten ist.

**Programmausdruck 5 - Models\IContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

Die geänderte Contact-Controller-Klasse ist im Codebeispiel 6 enthalten. Beachten Sie, dass der Contact-Controller nicht mehr mit dem Repository ContactManager interagiert. Stattdessen wird der Contact-Controller mit dem ContactManager-Dienst interagiert. Jede Ebene wird so weit wie möglich aus anderen Ebenen isoliert.

**Codebeispiel 6: Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

Die Anwendung nicht mehr of ausgeführt wird afoul der einzigen Verantwortung Prinzip (SRP). Der Contact-Controller im Codebeispiel 6 wurde jeder Verantwortung als steuern den Fluss der Ausführung der Anwendung entfernt. Alle Validierungslogik wurde aus der Contact-Controller entfernt und in der Dienstebene übertragen. Die gesamte Datenbanklogik wurde übertragen in die Repository-Ebene.

## <a name="using-the-decorator-pattern"></a>Verwenden des Decoratormusters

Wir möchten unsere Dienstebene aus unserem Controller Ebene vollständig zu entkoppeln. Im Prinzip sollten wir unsere Dienstebene in einer separaten Assembly aus unserem Controller-Ebene zu kompilieren, ohne einen Verweis zu unserer MVC-Anwendung hinzufügen können.

Allerdings muss unser Dienstschicht Validierung Fehlermeldungen an die Controller-Ebene können. Wie können wir die Dienstschicht Kommunikation Überprüfungsfehlermeldungen ohne Kopplung der Controller und der Dienstschicht aktivieren? Wir nutzen ein Software-Entwurfsmuster, mit dem Namen der [Decorator-Musters](http://en.wikipedia.org/wiki/Decorator_pattern).

Ein Controller verwendet ein mit dem Namen ModelState ModelStateDictionary Validierungsfehler darstellen. Aus diesem Grund können Sie ModelState aus der Ebene der Controller an die Dienstebene übergeben Versuchung sein. Jedoch wäre ModelState auf der Dienstebene mithilfe Ihrer Dienstebene ein Feature von ASP.NET MVC-Framework abhängig machen. Ungültige wäre dies daran, dass Sie die Dienstschicht mit einer WPF-Anwendung statt einer ASP.NET MVC-Anwendung verwenden möchten. In diesem Fall Sie ausreichen t ASP.NET MVC-Framework zur Verwendung der ModelStateDictionary-Klasse verweisen möchten.

Das Decorator-Musters können Sie eine vorhandene Klasse in eine neue Klasse zu umschließen, um eine Schnittstelle zu implementieren. Unser Projekt Contact Manager umfasst die ModelStateWrapper-Klasse, die in Codebeispiel 7 enthalten. Die ModelStateWrapper-Klasse implementiert die Schnittstelle im Codebeispiel 8.

**Auflisten von 7 – Models\Validation\ModelStateWrapper.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**Auflisten von 8 – Models\Validation\IValidationDictionary.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

Wenn Sie über einen längeren Blick in Listing 5 ergreifen dann sehen Sie, dass die Dienstschicht ContactManager IValidationDictionary Schnittstelle exklusiv verwendet. Der ContactManager-Dienst ist nicht abhängig von dem ModelStateDictionary-Klasse. Wenn der Controller wenden Sie sich an den ContactManager-Dienst erstellt wird, umschließt der Controller die ModelState wie folgt:

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>Zusammenfassung

In dieser Iteration haben wir nicht die neuen Funktionen der Contact Manager-Anwendung hinzugefügt. Das Ziel dieser Iteration wurde zum Umgestalten der Contact Manager-Anwendung so leichter verwalten und zu ändern.

Zunächst haben wir das Repository-Software-Entwurfsmuster implementiert. Alle Datenzugriffscode migriert in eine separate ContactManager-Repository-Klasse.

Wir werden auch unsere Validierungslogik von unseren Controllerlogik isoliert. Wir haben eine separate Dienstschicht, die alle unseres Codes für die Validierung enthält. Die Controller-Ebene interagiert mit der Dienstebene und die Dienstebene, die mit der Repositoryschicht interagiert.

Wenn wir die Dienstebene erstellt, haben wir die Vorteile des Decorator-Musters ModelState aus unserer Dienstebene zu isolieren. In unserem Dienstschicht programmiert wir der IValidationDictionary-Schnittstelle anstelle ModelState.

Schließlich haben wir ein Software-Entwurfsmuster, mit dem Namen der Dependency Injection-Muster nutzen. Dieses Muster ermöglicht es uns zur Programmierung von Schnittstellen (Abstraktionen) anstatt konkrete Klassen. Implementieren des Entwurfsmusters Dependency Injection wird unser Code besser getestet. In der nächsten Iteration hinzugefügt unser Projekt Komponententests.

> [!div class="step-by-step"]
> [Zurück](iteration-3-add-form-validation-cs.md)
> [Weiter](iteration-5-create-unit-tests-cs.md)
