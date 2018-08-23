---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: 'Iteration #6 – Verwenden der testgesteuerten Entwicklung (VB) | Microsoft-Dokumentation'
author: microsoft
description: In dieser Iteration sechsten fügen wir neue Funktionen zu unserer Anwendung zuerst Schreiben von Komponententests und Code für die Komponententests schreiben hinzu. In dieser Iteration...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 7b1700e0ccece543c381dbb4fa7d6243de57ed4d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827411"
---
<a name="iteration-6--use-test-driven-development-vb"></a>Iteration #6 – Verwenden der testgesteuerten Entwicklung (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> In dieser Iteration sechsten fügen wir neue Funktionen zu unserer Anwendung zuerst Schreiben von Komponententests und Code für die Komponententests schreiben hinzu. In dieser Iteration fügen wir wenden Sie sich an Gruppen hinzu.


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

In vorherigen Iterationen der Contact Manager-Anwendung haben wir die Komponententests um ein Sicherheitsnetz für unser Code bereitzustellen. Die Motivation für die Erstellung eines Komponententests war unser Code ändern zu gewährleisten. Bei Komponententests vorhanden können wir Glücklicherweise keine Änderungen an unseren Code und wissen sofort, ob wir die vorhandene Funktionalität unterbrochen wurde.

In dieser Iteration verwenden wir die Komponententests für einen ganz anderen Zweck. In dieser Iteration, verwenden wir Komponententests als Teil des Namens Entwurfsphilosophie von einer Anwendung *testgesteuerte Entwicklung*. Wenn Sie Test-driven Development, üben Sie zuerst Tests schreiben, und klicken Sie dann Code für die Tests zu schreiben.

Genauer gesagt, bei der testgesteuerten Entwicklung eignen, sind drei Schritte, die Sie abschließen, wenn Sie Code erstellen (Rot / Grün/refaktorieren):

1. Schreiben Sie einen Komponententest, der nicht (Rot)
2. Schreiben von Code, übergeben den Komponententest (Grün)
3. Gestalten Sie Ihren Code (Refactoring)

Zunächst schreiben Sie den Komponententest. Der Komponententest sollten Ihre möchte, wie Sie Ihren Code erwarten Verhalten express. Wenn Sie zuerst den Komponententest erstellen, sollte der Komponententest fehlschlägt. Der Test sollte fehlschlagen, da Sie noch nicht Anwendungscode geschrieben haben, der den Test erfüllt.

Als Nächstes schreiben Sie die gerade genug Code in der Reihenfolge für den Komponententest übergeben. Das Ziel ist, auf die Weise laziest, sloppiest und schnellste Möglichkeit den Code zu schreiben. Sie sollten nicht nachgedacht über die Architektur Ihrer Anwendung verschwendet. Stattdessen konzentrieren sollten, Sie schreiben die minimale Menge Code erforderlich, um die Absicht, die vom Komponententest ausgedrückt zu erfüllen.

Nach dem Sie viel Code geschrieben haben, können Sie schließlich Schritt zurück und betrachten Sie die allgemeine Architektur Ihrer Anwendung. In diesem Schritt haben Sie (Refactoring) schreiben Sie Ihren Code durch die Nutzung des Softwareentwurfs Muster – z. B. das Repository-Muster, damit Ihr Code besser verwaltbar ist. Sie können den Code in diesem Schritt spielerverwaltung umschreiben, da der Code von Komponententests abgedeckt ist.

Es gibt viele Vorteile, die aus eignen, die testgesteuerte Entwicklung entstehen. Erste, testgesteuerte Entwicklung erzwingt, dass Sie sich auf Code zu konzentrieren, die tatsächlich geschrieben werden muss. Da Sie ständig auf das Schreiben von Code übergeben Sie einen bestimmten Test konzentrieren möchten, können Sie nicht mehr Firma noch wandering in die schädlicher Pflanzen und Schreiben große Mengen von Code, die Sie niemals verwenden.

Andererseits eine designmethodik "zuerst testen" sind Sie gezwungen, Schreiben Sie Code aus der Perspektive des wie Ihr Code verwendet werden soll. Das heißt, sind bei der testgesteuerten Entwicklung eignen, Sie ständig Ihre Tests aus der Benutzerperspektive schreiben. Aus diesem Grund kann die testgesteuerte Entwicklung zu klarer und verständlicher-APIs führen.

Schließlich erzwingt testgesteuerte Entwicklung Sie Komponententests im Rahmen des normalen Prozesses des Schreibens von einer Anwendung zu schreiben. Wenn ein Projektstichtag nähert, ist das Testen in der Regel als erstes, die aus dem Fenster gesendet wird. Wenn eignen, die testgesteuerte Entwicklung, können Sie auf der anderen Seite eher virtuous zum Schreiben von Komponententests, da testgesteuerte Entwicklung Komponententests an den Prozess der Erstellung einer Anwendung zentrale wird.

> [!NOTE] 
> 
> Weitere Informationen zu Test-driven Development, ich empfehle die Lektüre von Michael Feathers Buch **Working Effectively with Legacy Code**.


In dieser Iteration hinzugefügt unsere Contact Manager-Anwendung ein neues Feature. Wir fügen Unterstützung für wenden Sie sich an Gruppen hinzu. Sie können Gruppen von wenden Sie sich an Ihre Kontakte in Kategorien wie Business zu organisieren und Friend verwenden.

Wir werden unsere Anwendung folgt ein Vorgang der testgesteuerten Entwicklung dieser neuen Funktionalität hinzufügen. Wir zuerst unsere Komponententests schreiben, und wir alle unsere Code für diese Tests schreiben.

## <a name="what-gets-tested"></a>Ruft ab, was getestet

Wie in vorherigen Iterationen erläutert, in der Regel nicht Schreiben von Komponententests für die Logik für den Datenzugriff oder Logik anzeigen. Da beim Zugriff auf eine Datenbank als relativ langsamer Vorgang ist Einbau t Schreiben von Komponententests für Datenzugriffslogik zusätzlichen. Da der Zugriff auf eine Ansicht bedeutet das Einrichten eines Webservers erforderlich ist, die als relativ langsamer Vorgang ist Einbau t Schreiben von Komponententests für ansichtslogik zusätzlichen. Sie treten normalerweise t einen Komponententest schreiben, es sei denn, der Test immer wieder sehr schnell ausgeführt werden kann

Da die testgesteuerte Entwicklung von Komponententests gesteuert wird, konzentrieren wir uns zunächst auf Controller und Geschäftslogik zu schreiben. Wir vermeiden, verändern die Datenbank oder Sichten. Wir haben t ändern die Datenbank oder erstellen Sie die Ansichten, bis das Ende des in diesem Tutorial. Wir beginnen mit, was getestet werden kann.

## <a name="creating-user-stories"></a>Erstellung von User Storys

Bei der testgesteuerten Entwicklung eignen, zunächst Sie immer einen Test zu schreiben. Dies führt sofort die Frage: wie entscheiden Sie, welcher Test zuerst schreiben? Um diese Frage zu beantworten, sollten Sie einen Satz von schreiben [ *User Storys*](http://en.wikipedia.org/wiki/User_stories).

Eine User Story ist eine sehr kurze (in der Regel einen Satz) Beschreibung der Software erforderlich. Es sollte eine technische Beschreibung einer Anforderung aus der Perspektive des Benutzers geschrieben sein.

Hier ist s den Satz von User Storys, die die durch die neue Gruppe Kontakt-Funktion erforderlichen Funktionen zu beschreiben:

1. Benutzer kann eine Liste der Kontakt Gruppen anzeigen.
2. Benutzer kann eine neue Gruppe von Kontakte erstellen.
3. Der Benutzer kann es sich um eine vorhandene Gruppe von Kontakten löschen.
4. Benutzer kann eine Gruppe von Kontakte auswählen, wenn Sie einen neuen Kontakt erstellen.
5. Benutzer kann eine Gruppe von Kontakte wählen Sie beim Bearbeiten eines vorhandenen Kontakts.
6. Eine Liste der Kontakt Gruppen wird in der Ansicht "Index" angezeigt.
7. Klickt ein Benutzer eine Gruppe von Kontakte, wird eine Liste der übereinstimmenden Kontakte angezeigt.

Beachten Sie, dass diese Liste von User Stories von einem Kunden vollkommen verständlich ist. Es gibt keine Erwähnung von Details zur technischen Implementierung.

Während im Prozess der Erstellung Ihrer Anwendung, kann die Zusammenstellung von User Stories besser optimiert werden. Sie können zur funktionsunfähigkeit einer User Story in mehrere Storys (Anforderungen). Sie könnten z. B., dass das Erstellen einer neuen Kontakten Gruppenstatus Validierung beinhalten soll. Senden eine Gruppe von Kontakte ohne Name sollte ein Überprüfungsfehler zurückgeben.

Nachdem Sie eine Liste von User Storys erstellt haben, können Sie Ihre erste Komponententest schreiben. Wir beginnen, erstellen Sie einen Komponententest für die Anzeige der Liste der Kontakt-Gruppen.

## <a name="listing-contact-groups"></a>Wenden Sie sich an Gruppen auflisten

Unsere erste Benutzertextabschnitt ist, dass ein Benutzer eine Liste der Kontakt Gruppen anzeigen kann sollen. Sie müssen diese Story mit einem Test express.

Erstellen ein neuen Komponententests durch Rechtsklick auf den Ordner "Controllers" im Projekt ContactManager.Tests auswählen **hinzufügen "," Neuer Test**, und wählen die **Komponententest** Vorlage (siehe Abbildung 1). Name die neue Einheit GroupControllerTest.vb zu testen, und klicken Sie auf die **OK** Schaltfläche.


[![Hinzufügen der GroupControllerTest-Komponententests](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Abbildung 01**: Hinzufügen des Komponententests GroupControllerTest ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-6-use-test-driven-development-vb/_static/image2.png))


Der erste Komponententest ist in Codebeispiel 1 enthalten. Dieser Test überprüft, ob die Index()-Methode des Controllers Gruppe einen Satz von Gruppen zurück. Der Test überprüft, dass die Daten in der Ansicht in eine Auflistung der Gruppen zurückgegeben werden.

**1 – Controllers\GroupControllerTest.vb auflisten**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

Wenn Sie zuerst den Code in Codebeispiel 1 in Visual Studio eingeben, erhalten Sie einen Großteil der roten Wellenlinien. Wir haben nicht die Klassen GroupController oder eine Gruppe erstellt.

An diesem Punkt können wir sogar t-Build die Anwendung dazu t ausführen, der erste Komponententest. Gute s. Die zählt als einen fehlgeschlagenen Test. Aus diesem Grund haben wir jetzt über die Berechtigung zum Starten der Anwendungscode schreiben. Wir müssen genug Code zum Ausführen von unserem Test schreiben.

Die Gruppe Controllerklasse in Liste 2 enthält das absolute Minimum von Code erforderlich, um den Komponententest zu übergeben. Die Index()-Aktion gibt eine statisch codierte Liste der Gruppen, die (in Programmausdruck 3 ist die Gruppe-Klasse definiert) zurück.

**Codebeispiel 2 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**Codebeispiel 3 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Nachdem wir unser Projekt Klassen GroupController und -Gruppe hinzugefügt haben, wird der erste Komponententest erfolgreich abgeschlossen (siehe Abbildung 2). Wir haben die Arbeitsschritte, die mindestens erforderlich, um der Test erfolgreich durchgeführt. Es ist Zeit zu feiern.


[![Success!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Abbildung 02**: Erfolg! () [Klicken Sie, um das Bild in voller Größe anzeigen](iteration-6-use-test-driven-development-vb/_static/image4.png))


## <a name="creating-contact-groups"></a>Wenden Sie sich an Gruppen erstellen

Jetzt können wir mit dem zweiten Benutzertextabschnitt fortfahren. Wir müssen neue wenden Sie sich an Gruppen erstellen können. Wir müssen dies in einem Test express.

Der Test in Listing 4 überprüft, die die Methode mit einer neuen Gruppe die Gruppe zur Liste der Gruppen, die von der Index()-Methode zurückgegebene fügt Create() aufruft. Das heißt, wenn ich eine neue Gruppe erstellen sollte dann ich die neue Gruppe aus der Liste der Gruppen, die von der Index()-Methode zurückgegebene zurückkehren.

**Programmausdruck 4 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

Der Test in Listing 4 wird den Controller Gruppe Create()-Methode mit einer neuen Kontakt Gruppe aufgerufen. Als Nächstes überprüft der Test an, dass die neue Gruppe den Gruppe Controller Index()-Methode aufrufen in Ansichtsdaten zurückgegeben werden.

Der geänderte Gruppe Controller in Listing 5 enthält das absolute Minimum erforderlichen Änderungen an der neuen Test erfolgreich abgeschlossen.

**Programmausdruck 5 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Die Gruppe Controller in Listing 5 hat es sich um eine neue Create()-Aktion. Diese Aktion wird eine Auflistung von Gruppen eine Gruppe hinzugefügt. Beachten Sie, dass die Aktion Index() geändert wurde, um den Inhalt der Auflistung der Gruppen zurückzugeben.

Auch hier haben wir das absolute Minimum der erforderlich ist, übergeben den Komponententest ausgeführt. Nachdem wir diese Änderungen an dem Controller für die Gruppe vornehmen, alle unsere Komponententests wurden bestanden.

## <a name="adding-validation"></a>Hinzufügen der Validierung

Diese Anforderung wurde in der User Story nicht explizit angegeben werden. Allerdings ist es sinnvoll, die erfordern, dass eine Gruppe einen Namen haben. Andernfalls würde Kontakte in Gruppen organisieren nicht sehr nützlich sein.

Codebeispiel 6 enthält einen neuen Test, der die Absicht ausdrückt. Dieser Test überprüft, die versuchen, eine Gruppe zu erstellen, ohne Angabe von einem Namen führt eine Überprüfungsfehlermeldung in Modellstatus.

**Codebeispiel 6: Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

Um diesen Test erfüllen zu können, müssen wir eine Name-Eigenschaft unserer Gruppe-Klasse (siehe Codebeispiel 7) hinzufügen. Darüber hinaus müssen wir unsere Gruppe Controller s Create() Aktion (siehe Codebeispiel 8) einen kleinen Teil der Validierungslogik hinzugefügt.

**Auflisten von 7 – Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**Auflisten von 8 – Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Beachten Sie, dass der Controller Gruppe Create() Aktion jetzt die Validierung und die Datenbank Logik enthält. Derzeit besteht aus die Datenbank, die vom Controller Gruppe nicht mehr als eine in-Memory-Auflistung.

## <a name="time-to-refactor"></a>Zeit für das Umgestalten

Der dritte Schritt im Rot/Grün/refaktorieren ist der Teil der Umgestaltung. An diesem Punkt müssen wir einen Schritt zurück in unserem Code, und erwägen, wie wir unsere Anwendung zur Verbesserung der entsprechenden Entwurfs Umgestalten können. Die Refactor-Phase ist die Phase, an der wir über die beste Möglichkeit der Implementierung von Prinzipien der Softwareentwicklung und Muster vorstellen.

Wir können unseren Code in irgendeiner Weise zu ändern, die wir wählen, um den Entwurf des Codes zu verbessern. Wir haben ein Sicherheitsnetz bereitstellen, der Komponententests, die verhindert, vorhandene Funktionalität zu beschädigen.

Moment, der Controller für die Gruppe ist ein Chaos aus der Perspektive des guten Software-Entwurf. Der Controller für die Gruppe enthält ein verworrenes Chaos überprüfungs-und Datenzugriffscode. Um zu vermeiden, gegen das Prinzip der einzigen Verantwortung, müssen wir diese Probleme in verschiedene Klassen trennen.

Unsere umgestaltete Gruppe Controllerklasse ist im Codebeispiel 9 enthalten. Der Controller wurde geändert, um die ContactManager-Dienstebene verwenden. Dies ist die gleiche Dienstebene, die wir mit der Contact-Controller verwenden.

Codebeispiel 10 enthält die neuen Methoden hinzugefügt, um die Dienstebene "ContactManager" zu überprüfen, auflisten und Erstellen von Gruppen zu unterstützen. Die IContactManagerService-Schnittstelle wurde aktualisiert, um die neuen Methoden enthalten.

Codebeispiel 11 enthält eine neue FakeContactManagerRepository-Klasse, die die IContactManagerRepository-Schnittstelle implementiert. Im Gegensatz zu den EntityContactManagerRepository-Klasse, die auch die IContactManagerRepository-Schnittstelle implementiert, wird unsere neue FakeContactManagerRepository-Klasse nicht mit der Datenbank kommunizieren. Die FakeContactManagerRepository-Klasse verwendet eine speicherinterne Auflistung als Proxy für die Datenbank. Wir verwenden diese Klasse in unserer Komponententests als eine gefälschte Repository-Ebene.

**Codebeispiel 9 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**Auflisten von 10 – Controllers\ContactManagerService.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**Codebeispiel 11 - Controllers\FakeContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]


Ändern die Schnittstelle erfordert IContactManagerRepository verwenden, um die CreateGroup() und ListGroups()-Methode in der EntityContactManagerRepository-Klasse zu implementieren. Die laziest und schnellste Möglichkeit hierzu besteht darin die Stubmethoden hinzufügen, die wie folgt aussehen:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]


Zum Schluss uns dazu auffordern, diese Änderungen am Entwurf der Anwendung einige Änderungen an unseren Komponententests vornehmen. Nun müssen wir die FakeContactManagerRepository zu verwenden, wenn die Komponententests ausführen. Die aktualisierte GroupControllerTest-Klasse ist im Codebeispiel 12 enthalten.

**Codebeispiel 12 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Nachdem wir immer ändert sich, auch hier alle unsere Komponententests bestanden. Wir haben den gesamten Rot/Grün/refaktorieren-Zyklus abgeschlossen. Wir haben die ersten beiden User Stories implementiert. Wir haben jetzt die Unterstützung von Komponententests für die Anforderungen, die in die User Stories ausgedrückt. Zur Implementierung des Rest der User Storys müssen die wiederholen denselben Zyklus der Rot/Grün/refaktorieren.

## <a name="modifying-our-database"></a>Ändern der Datenbank

Leider, obwohl wir alle unsere Komponententests ausgedrückt Anforderungen erfüllt haben, unsere Arbeit erfolgt nicht. Wir müssen noch unsere Datenbank ändern.

Wir müssen eine neue Datenbanktabelle für die Gruppe zu erstellen. Führen Sie folgende Schritte aus:

1. Klicken Sie im Server-Explorer-Fenster, mit der rechten Maustaste in den Ordner "Tabellen", und wählen Sie die Menüoption **neue Tabelle hinzufügen**.
2. Geben Sie unten im Tabellen-Designer zwei Spalten.
3. Markieren Sie die Id-Spalte als Primärschlüssel und Identity-Spalte.
4. Speichern Sie die neue Tabelle mit den Namen Gruppen durch Klicken auf das Symbol für die Diskette an.

<a id="0.12_table01"></a>


| **Name der Spalte** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | int | False |
| name | nvarchar(50) | False |


Als Nächstes müssen wir alle Daten aus der Contacts-Tabelle löschen (andernfalls, wir haben gewonnen t Lage, eine Beziehung zwischen den Tabellen Kontakte und Gruppen zu erstellen). Führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste der Contacts-Tabelle, und wählen Sie die Menüoption **Tabellendaten anzeigen**.
2. Löschen Sie alle Zeilen aus.

Als Nächstes müssen wir eine Beziehung zwischen der Tabelle der Datenbank Gruppen und die vorhandene Datenbanktabelle für die Kontakte zu definieren. Führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die Tabelle "Contacts" im Server-Explorer-Fenster zu den Tabellen-Designer zu öffnen.
2. Fügen Sie eine neue Spalte mit ganzen Zahlen, die Contacts-Tabelle, die mit dem Namen GroupId hinzu.
3. Klicken Sie auf die Schaltfläche "Beziehung", um das Dialogfeld "Fremdschlüsselbeziehungen" zu öffnen (siehe Abbildung 3).
4. Klicken Sie auf die Schaltfläche "hinzufügen".
5. Klicken Sie auf die Schaltfläche mit den Auslassungszeichen neben der Schaltfläche, Tabellen- und Spaltenspezifikation.
6. Wählen Sie Tabellen und Spalten im Dialogfeld Gruppen als die Primärschlüsseltabelle und die Id als primäre Schlüsselspalte. Kontakte als Fremdschlüsseltabelle und Gruppen-ID der foreign Key-Spalte auswählen (siehe Abbildung 4). Klicken Sie auf die Schaltfläche "OK".
7. Klicken Sie unter **INSERT- und UPDATE-Spezifikation**, wählen Sie den Wert **Cascade** für **Regel löschen**.
8. Klicken Sie auf die Schaltfläche "Schließen", um das Dialogfeld "Fremdschlüsselbeziehungen" zu schließen.
9. Klicken Sie auf die Schaltfläche "Speichern", um die Änderungen an der Contacts-Tabelle speichern.


[![Erstellen einer Beziehung der Datenbank-Tabelle](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Abbildung 03**: Erstellen einer Datenbank-Tabelle-Beziehung ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-6-use-test-driven-development-vb/_static/image6.png))


[![Angeben von tabellenbeziehungen](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Abbildung 04**: Angeben von tabellenbeziehungen ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-6-use-test-driven-development-vb/_static/image8.png))


### <a name="updating-our-data-model"></a>Unser Datenmodell aktualisieren

Als Nächstes müssen wir aktualisieren unsere Datenmodell, um die neue Datenbanktabelle darstellen. Führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die ContactManagerModel.edmx-Datei im Ordner "Models", die Entity-Designer zu öffnen.
2. Mit der rechten Maustaste in die Oberfläche des Designers, und wählen Sie die Menüoption **Modell aus der Datenbank aktualisieren**.
3. Im Update-Assistenten auf die Schaltfläche auswählen, die Gruppen, Tabelle, und klicken Sie auf das Beenden (siehe Abbildung 5).
4. Mit der rechten Maustaste in der Gruppen-Entität, und wählen Sie die Menüoption **umbenennen**. Ändern des Namens der *Gruppen* Entität *Gruppe* (im singular).
5. Mit der rechten Maustaste in der Gruppen-Navigationseigenschaft, die am unteren Rand der Entität "Contact" angezeigt wird. Ändern des Namens der *Gruppen* Navigationseigenschaft *Gruppe* (im singular).


[![Aktualisieren eines Entity Framework-Modells aus der Datenbank](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Abbildung 05**: Aktualisieren eines Entity Framework-Modells aus der Datenbank ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-6-use-test-driven-development-vb/_static/image10.png))


Nachdem Sie diese Schritte abgeschlossen haben, wird Ihr Datenmodell der Kontakte und Gruppen darstellen. Der Entity Designer sollten beide Entitäten angezeigt (siehe Abbildung 6).


[![Entity-Designer zum Anzeigen von Gruppe, und wenden Sie sich an](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Abbildung 06**: Entity-Designer zum Anzeigen von Group und Contact ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-6-use-test-driven-development-vb/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Erstellen die Repositoryklassen

Als Nächstes müssen wir unsere "Repository"-Klasse zu implementieren. Im Verlauf dieser Iteration haben wir mehrere neue Methoden der Schnittstelle IContactManagerRepository beim Schreiben von Code zum erfüllen unserer Komponententests hinzugefügt. Die endgültige Version der IContactManagerRepository-Schnittstelle ist im Codebeispiel 14 enthalten.

**Codebeispiel 14 - Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

Wir noch nicht tatsächlich eine der Methoden für die Arbeit mit wenden Sie sich an Gruppen in der realen EntityContactManagerRepository-Klasse implementiert. Derzeit enthält die EntityContactManagerRepository-Klasse Stubmethoden für jede Gruppe von Kontakten Methoden aufgeführt, die in der IContactManagerRepository-Schnittstelle. Beispielsweise sieht die Methode ListGroups() derzeit folgendermaßen aus:

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

Die Stubmethoden ermöglicht, uns auf unserer Anwendung zu kompilieren, und übergeben die Komponententests. Jetzt ist es jedoch Zeit, diese Methoden implementieren. Die endgültige Version der EntityContactManagerRepository-Klasse ist im Codebeispiel 13 enthalten.

**Codebeispiel 13 - Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Erstellen von Ansichten

ASP.NET MVC-Anwendung, wenn Sie die ASP.NET standardansichts-Engine verwenden. Also, Sie Ich möchte Ansichten als Reaktion auf einen bestimmten Komponententest erstellen. Da eine Anwendung ohne Ansichten unbrauchbar wäre, wir können jedoch t dieser Iteration abschließen, ohne erstellen und ändern die Ansichten, die in der Contact Manager-Anwendung enthalten.

Wir müssen die folgenden neuen Ansichten zum Verwalten von Gruppen von wenden Sie sich an, der (siehe Abbildung 7) zu erstellen:

- Views\Group\Index.aspx - zeigt eine Liste der Kontakt-Gruppen
- Views\Group\Delete.aspx - zeigt Bestätigungsformular für das Löschen einer Gruppenstatus von Kontakten


[![Die Gruppe Indexansicht](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Abbildung 07**: Ansicht "Index für die Gruppe" ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-6-use-test-driven-development-vb/_static/image14.png))


Wir müssen die folgenden vorhandenen Ansichten zu ändern, sodass sie wenden Sie sich an Gruppen enthalten:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Sie sehen die geänderten Ansichten anhand der Visual Studio-Anwendung, die in diesem Lernprogramm begleitet. Abbildung 8 veranschaulicht beispielsweise die Ansicht "Wenden Sie sich an Index".


[![Ansicht "Wenden Sie sich an Index"](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Abbildung 08**: der Indexansicht für den Kontakt ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-6-use-test-driven-development-vb/_static/image16.png))


## <a name="summary"></a>Zusammenfassung

In dieser Iteration haben wir neue Funktionen zu unserer Contact Manager-Anwendung anhand von designmethodik eine testgesteuerte Entwicklung-Anwendung hinzugefügt. Da fingen wir durch eine Reihe von User Stories zu erstellen. Es erstellt einen Satz von Komponententests, der die die Anforderungen, die durch die User Stories ausgedrückt entspricht. Schließlich haben wir gerade genug Code zum Erfüllen der Anforderungen von den Komponententests ausgedrückt geschrieben.

Nachdem wir das Schreiben von Code erfüllen die Anforderungen von den Komponententests ausgedrückt haben, aktualisiert es Ihre Datenbank und die Ansichten. Wir unsere Datenbank eine neue Gruppentabelle hinzugefügt und aktualisiert das Entity Framework Data Model. Wir können auch erstellt und einen Satz von Ansichten geändert.

In der nächsten Iteration: die letzte Iteration – schreiben wir unsere Anwendung Ajax nutzen. Durch die Nutzung von Ajax, werden wir die Reaktionsfähigkeit und Leistung der Contact Manager-Anwendung verbessern.

> [!div class="step-by-step"]
> [Zurück](iteration-5-create-unit-tests-vb.md)
> [Weiter](iteration-7-add-ajax-functionality-vb.md)
