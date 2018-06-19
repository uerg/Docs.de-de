---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: 'Iteration #6 – verwenden Sie eine testgesteuerte Entwicklung (c#) | Microsoft Docs'
author: microsoft
description: In dieser Iteration sechste wir neue Funktionalität Hinzufügen der Anwendung durch Schreiben von Komponententests zuerst und Code für die Komponententests schreiben. In dieser Iteration...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: 94502625f66d3eb08a24b8f2a369bf456a3367b1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876292"
---
<a name="iteration-6--use-test-driven-development-c"></a>Iteration #6 – verwenden Sie eine testgesteuerte Entwicklung (c#)
====================
durch [Microsoft](https://github.com/microsoft)

[Herunterladen von Code](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> In dieser Iteration sechste wir neue Funktionalität Hinzufügen der Anwendung durch Schreiben von Komponententests zuerst und Code für die Komponententests schreiben. In dieser Iteration fügen wir Kontakt Gruppen hinzu.


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

In vorherigen Iterationen der Kontakt-Manager-Anwendung erstellt Komponententests zur getesteten Codes Sicherheitsnetz verliehen wird. Die Motivation für die Erstellung eines Komponententests wurde damit unser Code stabiler zu ändern. Bei Komponententests erfüllt können wir stört keine Änderungen an den getesteten Codes und sofort zu wissen, ob wir bestehenden Funktionen außer Kraft gesetzt wurden.

In dieser Iteration verwenden wir Komponententests für eine vollkommen andere Zweck. In dieser Iteration verwenden wir Komponententests als Teil einer Anwendung Entwurfsphilosophie aufgerufen *testorientierte Entwicklung*. Wenn Sie eine testgesteuerte Entwicklung üben Sie zuerst Tests schreiben und Schreiben Sie Code anhand der Tests.

Genauer gesagt, wenn testorientierte Entwicklung zu üben, umfasst drei Schritte, die Sie, beim Erstellen von Code abschließen (Rot / Grün/Refactor):

1. Schreiben eines Komponententests, die nicht (Rot)
2. Schreiben Sie Code, übergeben den Komponententest (Grün)
3. Umgestalten des Codes (Refactor)

Zuerst, Schreiben Sie den Komponententest. Der Komponententest sollten so formulieren Ihre Absicht für wie Codes erwartet verhält. Wenn Sie zuerst den Komponententest erstellen, werden sollte, schlägt der Komponententest fehl. Der Test sollte fehlschlagen, da Sie noch keine Anwendungscode geschrieben haben, der den Test erfüllt.

Als Nächstes schreiben Sie nur so viel Code in der Reihenfolge für den Komponententest zu übergeben. Das Ziel ist bei der Datenerfassung laziest, sloppiest und schnellste Möglichkeit den Code zu schreiben. Sie sollten Zeit darum geht, die Architektur der Anwendung nicht verschwendet. Stattdessen sollten Sie die minimale Menge Code erforderlich, erfüllen die Absicht ausgedrückt werden, indem Sie den Komponententest schreiben konzentrieren.

Nach dem Sie so viel Code verfasst haben, können Sie schließlich Schritt zurück und die allgemeine Architektur Ihrer Anwendung berücksichtigen. In diesem Schritt haben Sie (Refactor) schreiben Sie Code nutzen Softwareentwurf dafür Muster – z. B. das Repositorymuster--, damit Ihr Code sind besser verwaltbar ist. Sie können Code in diesem Schritt fearlessly umschreiben, da Code durch Komponententests abgedeckt ist.

Es gibt viele Vorteile, die aus testorientierte Entwicklung zu üben. Erste, Test-driven Development erzwingt, dass Sie sich auf Code zu konzentrieren, die tatsächlich geschrieben werden muss. Da Sie ständig befasst sind neben dem Schreiben von genügend Code aus, um einen bestimmten Test bestehen, werden Sie daran gehindert neugierigen in die schädlicher Pflanzen und Schreiben große Mengen des Codes, die nie verwendet werden.

Zweitens erzwingt Entwurf "test zuerst" Methoden zum Schreiben von Code aus der Perspektive des wie Codes verwendet werden soll. Das heißt, wenn testorientierte Entwicklung zu üben, Schreiben ständig die Tests im Hinblick auf Benutzer Sie. Aus diesem Grund kann eine testgesteuerte Entwicklung leichter verständlich und übersichtlicher-APIs führen.

Schließlich erzwingt eine testgesteuerte Entwicklung Schreiben von Komponententests im Rahmen der üblichen Prozess der Schreiben einer Anwendung. Ein Projektstichtag erreicht, ist das Testen in der Regel zunächst, die das Fenster wechselt. Wenn testorientierte Entwicklung zu üben können Sie andererseits, wahrscheinlicher virtuous über das Schreiben von Komponententests, da eine testgesteuerte Entwicklung Komponententests zentralen an den Prozess der Erstellung einer Anwendung werden können.

> [!NOTE] 
> 
> Weitere Informationen zum Test-driven Development, Michael Feathers Buch zu lesen sollten **effektiv arbeiten, mit Legacycode**.


In dieser Iteration fügen wir ein neues Feature für unsere Kontakt-Manager-Anwendung hinzu. Wir fügen Sie Unterstützung für erhalten Sie Gruppen hinzu. Sie können erhalten Sie Gruppen zum Organisieren von Kontakten in Kategorien, z. B. Business und "Friend" verwenden.

Wir werden unsere Anwendung anhand eines Prozesses von testorientierte Entwicklung diese neuen Funktionen hinzugefügt. Wir unsere Komponententests zuerst schreiben, und wir alle unsere Code für diese Tests schreiben.

## <a name="what-gets-tested"></a>Was ruft getestet

Wie in vorherigen Iterationen erläutert, in der Regel nicht Schreiben von Komponententests für Datenzugriffslogik oder Logik anzeigen. Sie Verschlüsselungskennwort t Schreiben von Komponententests für Datenzugriffslogik, da den Zugriff auf eine Datenbank um einen relativ langsam Vorgang handelt. Sie Verschlüsselungskennwort t Schreiben von Komponententests für ansichtslogik, da der Zugriff auf eine Sicht Spinvorgänge Einrichten eines Webservers erforderlich ist. dies ein relativ langsamer Vorgang ist. Sie sollte nicht t einen Komponententest schreiben, es sei denn, der Test immer wieder sehr schnell ausgeführt werden kann

Da eine testgesteuerte Entwicklung durch Komponententests gesteuert wird, konzentrieren wir uns zunächst auf dem Controller und Geschäftslogik schreiben. Wir die berühren, die Datenbank oder Sichten vermeiden. Wir/t gewonnen Änderungen an der Datenbank oder unsere Ansichten bis zum Ende dieses Lernprogramms erstellen. Beginnen wir mit was getestet werden können.

## <a name="creating-user-stories"></a>Erstellen von User Storys

Wenn eine testgesteuerte Entwicklung zu üben, beginnen Sie immer durch einen Test zu schreiben. Dies löst sofort die Frage: wie Sie sich entscheiden, welcher Test zuerst schreiben? Um diese Frage zu beantworten, Schreiben Sie eine Reihe von [ **User Storys**](http://en.wikipedia.org/wiki/User_stories).

Eine User Story ist eine sehr kurze (in der Regel einen Satz) Beschreibung der Software erforderlich. Es sollte eine nichttechnische Beschreibung einer Anforderung im Hinblick auf Benutzer geschrieben sein.

Hier s die Zusammenstellung von User Stories, die durch die neue Gruppe Kontakt-Funktion erforderlichen Funktionen zu beschreiben:

1. Benutzer kann eine Liste der Kontakt Gruppen anzeigen.
2. Benutzer kann eine neue Gruppe von Kontakte erstellen.
3. Benutzer kann eine vorhandene Kontakte Gruppe löschen.
4. Benutzer kann eine Gruppe von Kontakte auswählen, für die Erstellung ein neues Kontakts.
5. Benutzer kann eine Gruppe von Kontakte auswählen, beim Bearbeiten eines vorhandenen Kontakts.
6. Eine Liste der Kontakt Gruppen wird in die Indexansicht angezeigt.
7. Wenn ein Benutzer eine Gruppe von Kontakte klickt, wird eine Liste der übereinstimmenden Kontakte angezeigt.

Beachten Sie, dass diese Liste von User Stories von einem Kunden verständlich ist. Es ist nicht erwähnt technische Implementierungsdetails.

Beim Erstellen der Anwendung, kann die Zusammenstellung von User Stories verfeinerten werden. Sie können zur funktionsunfähigkeit von einer User Storys in mehrere Storys (Anforderungen). Sie könnten z. B., dass das Erstellen einer neuen Kontakten Gruppe Validierung beinhalten. Senden eine Gruppe von Kontakte ohne Namen sollten tritt ein Validierungsfehler zurück.

Nachdem Sie eine Liste von User Storys erstellt haben, können Sie den ersten Komponententest schreiben. Wir beginnen mit der Erstellung eines Komponententests für die Anzeige der Liste der Kontakt-Gruppen.

## <a name="listing-contact-groups"></a>Wenden Sie sich an Auflistungsgruppen

Unsere erste Benutzertextabschnitt ist, dass ein Benutzer eine Liste der Kontakt Gruppen anzeigen werden soll. Dieser Artikel in einem Test express ist erforderlich.

Erstellen ein neuen Komponententests durch Rechtsklicken auf den Ordner Controllers in das Projekt ContactManager.Tests auswählen **hinzufügen, testen Sie neue**, und wählen die **Komponententest** Vorlage (siehe Abbildung 1). Name die neue Einheit GroupControllerTest.cs testen, und klicken Sie auf die **OK** Schaltfläche.


[![Hinzufügen des GroupControllerTest-Komponententests](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)

**Abbildung 01**: Hinzufügen von den Komponententest GroupControllerTest ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-6-use-test-driven-development-cs/_static/image2.png))


Unsere ersten Komponententest ist im Codebeispiel 1 enthalten. Es wird überprüft, dass die Index()-Methode des Controllers Gruppe gibt einen Satz von Gruppen zurück. Es wird überprüft, dass eine Auflistung von Gruppen in der Sicht Daten zurückgegeben werden.

**1 – Controllers\GroupControllerTest.cs auflisten**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

Wenn Sie zunächst den Code im Codebeispiel 1 in Visual Studio eingeben, erhalten Sie viele roten Wellenlinien. Wir haben nicht die Klassen GroupController oder eine Gruppe erstellt.

An diesem Punkt können wir auch t-Build die Anwendung, damit wir t können führen Sie unseren ersten Komponententest. Gute s. Die zählt als einen fehlgeschlagenen Test. Aus diesem Grund haben wir jetzt über die Berechtigung zum Starten der Anwendungscode schreiben. Wir müssen so viel Code zur Ausführung von unserem Test zu schreiben.

Die Controllerklasse Gruppe 2 auflisten enthält die absolute Mindestanforderungen des Codes erforderlich, um den Komponententest zu übergeben. Die Index()-Aktion gibt eine statisch codierte Liste der Gruppen (die Gruppenklasse auflisten 3 definiert ist).

**Auflisten von 2 – Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

**3 – Models\Group.cs auflisten**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

Nachdem wir unser Projekt die Klassen GroupController und Gruppe hinzugefügt haben, wird unsere ersten Komponententest erfolgreich abgeschlossen (siehe Abbildung 2). Wir haben die Arbeitsschritte, die mindestens erforderlich, um die Überprüfung erfolgreich ausgeführt. Es ist Zeit zu.


[![War erfolgreich!](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)

**Abbildung 02**: Erfolg! () [Klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-6-use-test-driven-development-cs/_static/image4.png))


## <a name="creating-contact-groups"></a>Wenden Sie sich an Gruppen erstellen

Nachdem wir die zweite User Story verschieben können. Wir müssen in der Lage, zum Erstellen neuer Kontakt Gruppen sein. Wir müssen diese Absicht in einem Test.

Der Test wird im Codebeispiel 4 überprüft, die eine Signatures-Auflistung, die Methode mit einer neuen Gruppe die Gruppe zur Liste der Gruppen, die von der Index()-Methode zurückgegebene fügt aufruft. Das heißt, wenn ich eine neue Gruppe erstellen sollte dann ich die neue Gruppe aus der Liste der Gruppen, die von der Index()-Methode zurückgegebene abzurufen können.

**4 – Controllers\GroupControllerTest.cs auflisten**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

Der Test wird im Codebeispiel 4 Ruft die Gruppe Controller Create()--Methode mit eines neuen Kontakts Gruppe. Als Nächstes überprüft der Test an, dass die neue Gruppe den Gruppe Controller Index()-Methode aufrufen in Ansichtsdaten zurückgegeben werden.

Der geänderte Gruppe Controller auflisten 5 enthält die absolute Mindestanforderungen Änderungen erforderlich, um den neuen Test bestehen.

**5 – Controllers\GroupController.cs auflisten**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Der Controller Gruppe auflisten 5 verfügt über eine neue Create()--Aktion. Diese Aktion wird eine Auflistung von Gruppen eine Gruppe hinzugefügt. Beachten Sie, dass die Aktion Index() geändert wurde, um den Inhalt der Auflistung der Gruppen zurückzugeben.

Wir haben noch einmal: bare mindestens ein Videospeicher von Arbeitsaufwand, übergeben den Komponententest ausgeführt. Nachdem wir dieser Änderungen an dem Controller für die Gruppe aller unserer Komponententests wurden bestanden.

## <a name="adding-validation"></a>Hinzufügen der Validierung

Diese Anforderung wurde in die User Story nicht explizit angegeben werden. Allerdings ist es sinnvoll, um festzulegen, dass eine Gruppe einen Namen aufweisen. Andernfalls würde Kontakte in Gruppen organisieren nicht sehr hilfreich sein.

Auflisten von 6 enthält einen neuen Test, der die Absicht ausdrückt. Dieser Test prüft, die versuchen, eine Gruppe zu erstellen, ohne Angabe eines Name führt in eine Überprüfungsfehlermeldung in Modellstatus.

**6 – Controllers\GroupControllerTest.cs auflisten**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

Um diesen Test gerecht zu werden, müssen wir eine Name-Eigenschaft der Gruppe-Klasse (Siehe auflisten von 7) hinzufügen. Darüber hinaus müssen wir unsere Gruppe Controller s Create()-Aktion (Siehe auflisten von 8) einen kleinen Teil der Validierungslogik hinzufügen.

**Auflisten von 7 - Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

**Auflisten von 8 - Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

Beachten Sie, dass die Gruppe-Controller jetzt Create()-Aktion Überprüfung und Datenbank-Logik enthält. Derzeit besteht aus die Datenbank verwendet, die vom Controller Gruppe nichts weiter als eine Auflistung im Arbeitsspeicher.

## <a name="time-to-refactor"></a>Zeit für die Umgestaltung

Der dritte Schritt im Rot/Grün/Umgestalten ist der Teil der Umgestaltung. An diesem Punkt müssen wir Schritt von getesteten Codes und überlegen, wie wir unsere Anwendung zur Verbesserung der des Entwurfs Umgestalten können. Die Umgestaltung-Phase ist die Phase, an der wir Festplatten über die beste Möglichkeit, Software-Entwurfsprinzipien und Muster zu implementieren sind.

Wir können unsere Code in irgendeiner Weise ändern, die wir auswählen, um den Entwurf des Codes zu verbessern. Wir haben Sicherheitsnetz von Komponententests, die verhindern, dass vorhandene Funktionalität zu beschädigen.

Jetzt, unsere Gruppe Controller ist ein Chaos aus der Perspektive des Entwurfs für gute Software. Der Controller für die Gruppe enthält ein kompliziertes Chaos der Überprüfung und Datenzugriffscode. Um zu vermeiden, gegen das Prinzip der einzelnen Verantwortung, müssen wir diese Probleme in anderen Klassen zu trennen.

Unser umgestalteten Gruppe Controllerklasse ist im Codebeispiel 9 enthalten. Der Controller wurde geändert, um die Dienstebene ContactManager zu verwenden. Dies ist die gleiche Dienstebene, die wir mit dem Kontakt-Controller verwenden.

Auflisten von 10 enthält die neuen Methoden für die Dienstebene ContactManager zur Unterstützung von überprüfen, auflisten und Erstellen von Gruppen hinzugefügt. Die IContactManagerService-Schnittstelle wurde aktualisiert, damit die neuen Methoden enthalten.

Auflisten von 11 enthält eine neue FakeContactManagerRepository-Klasse, die die IContactManagerRepository-Schnittstelle implementiert. Im Gegensatz zu der EntityContactManagerRepository-Klasse, die auch die IContactManagerRepository-Schnittstelle implementiert, wird unsere neue FakeContactManagerRepository-Klasse mit der Datenbank nicht kommunizieren. Die FakeContactManagerRepository-Klasse verwendet eine speicherinterne Auflistung als Proxy für die Datenbank. Wir verwenden diese Klasse in unserer Komponententests als eine gefälschte Repository-Ebene.

**Auflisten von 9 – Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

**Auflisten von 10 - Controllers\ContactManagerService.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

**Auflisten von 11 - Controllers\FakeContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

Ändern die Schnittstelle erfordert IContactManagerRepository verwenden, um die CreateGroup() und ListGroups()-Methode in der EntityContactManagerRepository-Klasse zu implementieren. Die laziest schnellste und einfachste Möglichkeit hierzu besteht darin, Stubmethoden hinzuzufügen, die wie folgt aussehen:   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]


Schließlich müssen diese Änderungen am Entwurf von unserer Anwendung uns einige Änderungen an unseren Komponententests. Jetzt müssen wir die FakeContactManagerRepository verwenden, wenn die Komponententests ausführen. Auflisten von 12 ist die aktualisierte GroupControllerTest-Klasse enthalten.

**12 - Controllers\GroupControllerTest.cs auflisten**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

Nach dem wir all diese ändern, ändert sich, noch einmal: alle unsere Komponententests bestanden. Wir haben den gesamten Zyklus von Rot/Grün/Umgestalten abgeschlossen. Wir haben die ersten beiden User Stories implementiert. Wir haben jetzt die Unterstützung von Komponententests für die Anforderungen in User Storys ausgedrückt. Implementieren den Rest der User Storys umfasst das wiederholte denselben Zyklus von Rot/Grün/Umgestalten.

## <a name="modifying-our-database"></a>Ändern die Datenbank

Obwohl wir alle unsere Komponententests ausgedrückt Anforderungen erfüllt, wird unsere Arbeit leider nicht ausgeführt. Wir müssen weiterhin die Datenbank zu ändern.

Wir müssen eine neue Gruppe Datenbanktabelle zu erstellen. Führen Sie folgende Schritte aus:

1. Klicken Sie im Server-Explorer mit der rechten Maustaste in des Tabellen-Ordners, und wählen Sie die Menüoption **neue Tabelle hinzufügen**.
2. Geben Sie die beiden Spalten finden Sie unten im Tabellen-Designer.
3. Markieren Sie die Id-Spalte als Primärschlüssel und Identity-Spalte.
4. Speichern Sie die neue Tabelle mit dem Namen Gruppen durch Klicken auf das Symbol des Diskettenlaufwerk.

<a id="0.11_table01"></a>


| **Spaltenname** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | int | False |
| name | Nvarchar(50) | False |


Als Nächstes müssen wir alle Daten aus der Contacts-Tabelle löschen (andernfalls wir gewonnen t Lage, eine Beziehung zwischen den Tabellen Kontakte und Gruppen zu erstellen). Führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste der Contacts-Tabelle, und wählen Sie die Menüoption **Tabellendaten anzeigen**.
2. Löschen Sie alle Zeilen aus.

Als Nächstes müssen wir eine Beziehung zwischen den Gruppen-Datenbanktabelle und die vorhandene Datenbank Kontakttabelle definieren. Führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die Kontakttabelle im Server-Explorer-Fenster zu den Tabellen-Designer zu öffnen.
2. Fügen Sie eine neue Spalte mit ganzen Zahlen, die mit dem Namen GroupId Kontakttabelle hinzu.
3. Klicken Sie auf die Schaltfläche mit den Beziehung, um das Dialogfeld "Foreign Key Relationships" zu öffnen (siehe Abbildung 3).
4. Klicken Sie auf die Schaltfläche "hinzufügen".
5. Klicken Sie auf die Schaltfläche mit den Auslassungszeichen neben der Schaltfläche "Tabellen- und Spaltenspezifikation".
6. Wählen Sie im Dialogfeld Tabellen und Spalten Gruppen als die Primärschlüsseltabelle und die Id als primäre Schlüsselspalte. Wählen Sie die Kontakte als Fremdschlüsseltabelle und GroupId als die Fremdschlüsselspalte (siehe Abbildung 4). Klicken Sie auf die Schaltfläche "OK".
7. Klicken Sie unter **INSERT- und UPDATE-Spezifikation**, wählen Sie den Wert **Cascade** für **Regel löschen**.
8. Klicken Sie auf die Schaltfläche "Schließen", um das Dialogfeld "Foreign Key Relationships" zu schließen.
9. Klicken Sie auf die Schaltfläche "Speichern", um die Änderungen der Contacts-Tabelle zu speichern.


[![Erstellen eine datenbankbeziehung-Tabelle](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)

**Abbildung 03**: Erstellen einer Datenbank-tabellenbeziehung ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-6-use-test-driven-development-cs/_static/image6.png))


[![Angeben von tabellenbeziehungen](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)

**Abbildung 04**: Angeben von tabellenbeziehungen ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-6-use-test-driven-development-cs/_static/image8.png))


### <a name="updating-our-data-model"></a>Unsere Datenmodell aktualisieren

Als Nächstes müssen wir aktualisieren unsere Datenmodell, um die neue Datenbanktabelle darstellen. Führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die ContactManagerModel.edmx-Datei im Ordner "Modelle" im Entity Designer geöffnet.
2. Mit der rechten Maustaste in die Entwurfsoberfläche, und wählen Sie die Menüoption **Modell aus der Datenbank aktualisieren**.
3. Im Update-Assistenten auf die Schaltfläche auswählen, die Gruppen Tabelle, und klicken Sie auf das Fertigstellen (siehe Abbildung 5).
4. Mit der rechten Maustaste der Gruppen-Entität, und wählen Sie die Menüoption **umbenennen**. Ändern Sie den Namen des der *Gruppen* Entität *Gruppe* (singular).
5. Klicken Sie auf die Gruppen-Navigationseigenschaft, die am unteren Rand der Contact-Entität angezeigt wird. Ändern Sie den Namen des der *Gruppen* -Navigationseigenschaft auf *Gruppe* (singular).


[![Aktualisieren ein Modell im Entity Framework aus der Datenbank](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)

**Abbildung 05**: Aktualisieren eines Entity Framework-Datenmodells aus der Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-6-use-test-driven-development-cs/_static/image10.png))


Nachdem Sie diese Schritte abgeschlossen haben, wird Ihr Datenmodell der Kontakte und Gruppen darstellen. Im Entity Designer sollten beide Entitäten angezeigt (siehe Abbildung 6).


[![Entitätsdesigner zum Anzeigen von Gruppen und wenden Sie sich an](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)

**Abbildung 06**: Entity Designer zum Anzeigen von Group und Contact ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-6-use-test-driven-development-cs/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Unsere Repositoryklassen erstellen

Als Nächstes müssen wir unsere Repository-Klasse zu implementieren. Im Verlauf dieser Iteration haben wir mehrere neue Methoden der Schnittstelle IContactManagerRepository beim Schreiben von Code zur Erfüllung von unseren Komponententests hinzugefügt. Die endgültige Version der IContactManagerRepository-Schnittstelle ist im Codebeispiel 14 enthalten.

**Auflisten von 14 - Models\IContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

Wir noch nicht tatsächlich implementiert eine der Methoden im Zusammenhang mit der Arbeit mit Gruppen wenden Sie sich an. Gegenwärtig kann die Klasse EntityContactManagerRepository Stubmethoden für jede Gruppe von Kontakten Methoden aufgeführt, die in der IContactManagerRepository-Schnittstelle. Beispielsweise sieht die ListGroups() Methode derzeit wie folgt:

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

Die Stubmethoden aktiviert wir unsere Anwendung kompilieren und übergeben die Komponententests. Jetzt ist es allerdings Zeit für diese Methoden zu implementieren. Die endgültige Version der Klasse EntityContactManagerRepository ist im Codebeispiel 13 enthalten.

**Auflisten von 13 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a>Erstellen von Ansichten

ASP.NET MVC-Anwendung, wenn Sie ASP.NET Standardansichtsmodul verwenden. Deshalb vergessen Ansichten als Antwort auf einen bestimmten Komponententest erstellen. Da eine Anwendung ohne Ansichten unbrauchbar wäre, wir können jedoch t dieser Iteration abgeschlossen, ohne erstellen und Ändern von Ansichten in der Contact-Manager-Anwendung enthalten sind.

Wir müssen die folgenden neuen Ansichten zum Verwalten von Gruppen wenden Sie sich an, der (siehe Abbildung 7) erstellen:

- Views\Group\Index.aspx - zeigt eine Liste der Kontakt-Gruppen
- Views\Group\Delete.aspx - zeigt Bestätigungsformular für das Löschen einer Gruppenstatus von Kontakten


[![Die Gruppe Indexansicht](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)

**Abbildung 07**: die Gruppe Indexansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-6-use-test-driven-development-cs/_static/image14.png))


Wir müssen die folgenden vorhandenen Ansichten ändern, sodass sie wenden Sie sich an Gruppen enthalten:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Sehen Sie die geänderten Ansichten durch einen Blick auf die Visual Studio-Anwendung, die in diesem Lernprogramm begleitet. Abbildung 8 zeigt beispielsweise die Indexansicht wenden Sie sich an.


[![Die Kontakt-Indexansicht](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)

**Abbildung 08**: die Indexansicht Kontakt ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-6-use-test-driven-development-cs/_static/image16.png))


## <a name="summary"></a>Zusammenfassung

In dieser Iteration haben wir neue Funktionen für unsere Kontakt-Manager-Anwendung gemäß testorientierte Entwicklung Anwendung Entwurf Methoden hinzugefügt. Wir gestartet, indem eine Reihe von User Storys zu erstellen. Wir erstellt einen Satz von Komponententests, der die Anforderungen, die durch die User Stories ausgedrückt entspricht. Schließlich haben wir nur so viel Code erfüllen die Anforderungen, die von den Komponententests ausgedrückt geschrieben.

Wir nach dem Schreiben von viel Code erfüllen die Anforderungen, die von den Komponententests ausgedrückt aktualisiert wir unsere Datenbank und Ansichten. Wir unsere Datenbank eine neue Gruppentabelle hinzugefügt und unsere Entity Framework-Datenmodells aktualisiert. Wir auch erstellt und einen Satz von Ansichten geändert.

In der nächsten Iteration – die letzte Iteration--schreiben wir unsere Anwendung Ajax nutzen. Durch Ajax nutzen, müssen die Reaktionsfähigkeit und die Leistung der Anwendung wenden Sie sich an Manager verbessert werden.

> [!div class="step-by-step"]
> [Zurück](iteration-5-create-unit-tests-cs.md)
> [Weiter](iteration-7-add-ajax-functionality-cs.md)
