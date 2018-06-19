---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Automatisierte Komponententests | Microsoft Docs
author: microsoft
description: Schritt 12 zeigt, wie eine Suite von automatisierte Komponententests entwickeln, überprüfen, ob unsere NerdDinner-Funktionalität und die geben uns des vertrauen Änderungen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: fede08be7e06327c6d04fa5d36f7dd818d79b380
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875629"
---
<a name="enable-automated-unit-testing"></a>Aktivieren Sie automatisierte Komponententests
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 12 mit einer kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.
> 
> Schritt 12 zeigt, wie eine Suite von automatisierte Komponententests entwickeln, zu überprüfen, ob unsere NerdDinner-Funktionalität und die geben uns der Gewissheit, Änderungen und Verbesserungen für die Anwendung in der Zukunft zu vereinfachen.
> 
> Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner Schritt 12: UnitTests

Wir entwickeln eine Suite von automatisierte Komponententests zu überprüfen, ob unsere NerdDinner-Funktionalität und die geben uns der Gewissheit, Änderungen und Verbesserungen für die Anwendung in der Zukunft zu vereinfachen.

### <a name="why-unit-test"></a>Warum Komponententest?

Auf dem Laufwerk, in die Arbeit einer Morgen müssen Sie einen plötzlichen Flash von Inspiration zu einer Anwendung, auf dem Sie arbeiten. Sie können eine Änderung, die Sie implementieren können, die die Anwendung erheblich verbessern wird. Es kann sein, ein refactoring, die der Code bereinigt, fügt ein neues Feature oder einen Fehler behebt.

Ist die Frage, die Sie confronts, wenn Sie bei Ihrem Computer ankommen – "beschreibt die sichere dieser Verbesserung vornehmen?" Was geschieht, wenn der Änderung aufweist Nebeneffekte oder unterbrochen etwas? Die Änderung kann einfach sein und dauern nur einige Minuten, bis zu implementieren, führt aber was geschieht, wenn Stunden manuell in allen Anwendungsszenarien zu testen? Was geschieht, wenn Sie vergessen, ein Szenario behandelt, und eine fehlerhafte Anwendung wechselt in die Produktion? Ist dieser Verbesserung sollte alle Aufwand wirklich vornehmen?

Automatisierte Komponententests ermöglichen Sicherheitsnetz, die Ihnen ermöglicht, Ihre Anwendungen kontinuierlich verbessern und vermeiden, wird von der Code, dem Sie arbeiten. Auf diese Weise müssen von automatisierten Tests, die schnell zu, dass die Funktionalität ermöglicht es Ihnen überprüfen, code mit vertrauen –, und geben Sie Sie Sie andernfalls nicht vertraut sind haben möglicherweise Verbesserungen vorzunehmen. Außerdem unterstützen Sie die Lösungen sind besser verwaltbar zu erstellen und haben eine Lebensdauer - Investitionen zu einer wesentlich höheren zurückzugeben.

Das ASP.NET MVC-Framework vereinfacht das einfache und natürliche Unit Test Anwendungsfunktionen. Außerdem können einen Test Driven Development (TDD)-Workflow, der je Test-First-Entwicklung ermöglicht.

### <a name="nerddinnertests-project"></a>NerdDinner.Tests Project

Wenn wir unsere NerdDinner Anwendung am Anfang dieses Lernprogramms erstellt haben, wurden wir aufgefordert, ein Dialogfeld gefragt, ob wir, erstellen Sie ein Komponententestprojekt zusammen mit dem Anwendungsprojekt zu wechseln möchten:

![](enable-automated-unit-testing/_static/image1.png)

Wir beibehalten der "Ja, Komponententestprojekt erstellen" Optionsfeld aktiviert – resultierte in einem Projekt auf "NerdDinner.Tests" unserer Projektmappe hinzugefügt wird:

![](enable-automated-unit-testing/_static/image2.png)

Das NerdDinner.Tests-Projekt verweist auf die NerdDinner Anwendung Projektassembly und ermöglicht es, fügen Sie einfach automatisierte Tests hinzu, durch die die Funktionalität der Anwendung überprüft.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Erstellen von Komponententests für unsere Dinner Skriptobjektmodell-Klasse

Fügen Sie einige Tests zu unserem NerdDinner.Tests-Projekt, das Überprüfen von der Dinner-Klasse, wenn wir unsere Modellebene erstellt erstellt wurde.

Wir beginnen, erstellen Sie einen neuen Ordner in unserem Testprojekt mit der Bezeichnung "Modelle", in dem wir unsere Tests Modell bezogene platzieren müssen. Wir mit der rechten Maustaste auf den Ordner und wählen Sie dann die **Add -&gt;neuen Test** Menübefehl. Hierdurch wird das Dialogfeld "Neuen Test hinzufügen" angezeigt.

Wählen wir zum Erstellen eines "Komponententests", und nennen Sie sie "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Beim Klicken auf die Schaltfläche "ok" Visual Studio hinzufügen (eine DinnerTest.cs-Datei zum Projekt und öffnen):

![](enable-automated-unit-testing/_static/image4.png)

Die Visual Studio Unit Test Standardvorlage verfügt über eine Reihe von Code mit Codebausteinen, darin, die ich etwas unübersichtlichen finden. Wir bereinigen sie einfach den folgenden Code enthält:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

[TestClass]-Attribut auf die DinnerTest-Klasse, die oben genannten identifiziert sie als eine Klasse, die Tests, als auch optionale testinitialisierung und Beendigungscode enthalten soll. Wir können Tests darin definieren, durch das Hinzufügen von öffentlichen Methoden, die einem [TestMethod]-Attribut verfügen.

Im folgenden sind die erste von zwei Tests, die wir hinzufügen, die unsere Dinner-Klasse ausführen. Der erste Test überprüft, dass unsere Dinner ungültig ist, wenn eine neue Dinner erstellt wird, ohne alle Eigenschaften richtig festgelegt wird. Der zweite Test stellt sicher, dass unsere Dinner gültig ist, wenn eine Dinner alle Eigenschaften, die durch gültige Werte festgelegt wurden:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Sie werden über bemerken, dass unsere Testnamen sehr explizite (und etwas verbose) werden. Wir sind dabei, da wir möglicherweise erschöpft, Hunderten oder Tausenden von kleinen Tests erstellen, und wir können Sie schnell feststellen, die Absicht und das Verhalten der einzelnen von ihnen (insbesondere, wenn es über eine Liste von Fehlern in einem Test Runner suchen) zu erleichtern möchten. Nach der die Funktionalität, die sie testen möchten, sollten die Testnamen benannt werden. Oben verwenden wir ein "Nomen\_sollten\_Verb" Benennungsmuster.

Wir werden die Tests mit dem Muster – das steht für "Arrange, Act, Assert" testen "AAA" strukturieren:

- Angeordnet werden: Die Einheit, die zu testenden Setup
- ACT: Führen Sie die Einheit unter dem Test, und erfassen Sie der Ergebnisse
- Assert-: Überprüfen Sie, ob das Verhalten

Beim Schreiben von wir führen zu viele Tests zu vermeiden, dass die einzelnen Tests werden soll. Jeder Test sollten stattdessen nur ein einzelnes Konzept überprüfen (die wesentlich zum Ermitteln der Ursache von Ausführungsfehlern erleichtern). Als Richtschnur besteht darin zu versuchen und nur eine einzelne assert-Anweisung für jeden Test. Wenn Sie mehr als eine assert-Anweisung in einer Testmethode haben, stellen Sie sicher, dass sie alle verwendet werden, um dasselbe Konzept zu testen. Stellen Sie im Zweifelsfall einen anderen Test.

### <a name="running-tests"></a>Tests werden ausgeführt

Visual Studio 2008 Professional (oder höhere Edition) enthält einen integrierten Test Runner, der zum Ausführen von Visual Studio-Komponententest-Projekten in der IDE verwendet werden kann. Wählen wir die **Test -&gt;ausführen -&gt;alle Tests in der Projektmappe** Menü Befehlsklasse (oder Typ STRG R, A) um alle unsere Komponententests ausgeführt werden. Alternativ können wir unsere Cursor innerhalb einer bestimmten Klasse oder Test Testmethode positionieren und Verwenden der **Test -&gt;ausführen -&gt;Tests im aktuellen Kontext** Menübefehl (oder Typ STRG R, T), um eine Teilmenge der Komponententests auszuführen.

Wir unsere Cursor innerhalb der Klasse DinnerTest positionieren aus, und geben Sie "STRG R, T" zum Ausführen der beiden Tests, die soeben definiert. Wenn wir dies ein Fenster "Testergebnisse" durchführen, werden in Visual Studio angezeigt, und wir sehen die Ergebnisse der unsere darin aufgeführten Testlauf:

![](enable-automated-unit-testing/_static/image5.png)

*Hinweis: Die Testergebnisse (Fenster) im Vergleich wird die Klassennamen-Spalte nicht standardmäßig angezeigt. Sie können diese hinzufügen, indem Sie mit der rechten Maustaste im Fenster Testergebnisse und verwenden den Menübefehl Spalten hinzufügen/entfernen.*

Unsere zwei Tests hat nur einen Bruchteil einer Sekunde ausgeführt – und Sie können ausführliche Informationen finden Sie beide übergeben. Wir können jetzt fahren Sie und erweitern diese, indem Sie zusätzliche Tests, die spezielle Regel Überprüfungen überprüfen sowie zwei Hilfsmethoden - IsUserHost() und IsUserRegisterd() –, die wir, die Klasse Dinner hinzugefügt abdecken zu erstellen. Müssen alle diese Tests vorhanden, für die Klasse Dinner wird wesentlich einfacher und sicherer zu neuen Geschäftsregeln und Überprüfungen in Zukunft hinzufügen erleichtern. Wir können unsere neue Regellogik Dinner hinzufügen und überprüfen dann innerhalb von Sekunden, dass sie unseren vorherigen Logik Funktionalität beeinträchtigt wurde nicht.

Beachten Sie, wie unter Verwendung eines Namens beschreibenden Text ganz einfach schnell herausfinden, welche Tests überprüft werden. Ich empfiehlt sich die **Tools –&gt;Optionen** Menübefehl, öffnen den Test Tools –&gt;Konfigurationsbildschirm für die Ausführung zu testen und überprüfen "durch Doppelklicken auf eine fehlerhafte oder nicht eindeutig Komponententestergebnis angezeigt. Zeitpunkt des Fehlers im Test"das Kontrollkästchen. Dadurch können Sie doppelklicken auf einen Fehler in die Testergebnisse (Fenster), und springen sofort auf den Assertionsfehler.

### <a name="creating-dinnerscontroller-unit-tests"></a>Erstellen von Komponententests DinnersController

Jetzt erstellen wir einige Komponententests, die unsere DinnersController-Funktionalität zu überprüfen. Wir starten, indem Sie mit der rechten Maustaste auf den Ordner "Controller" in unserem Test-Projekt und wählen Sie dann die **Add -&gt;neuen Test** Menübefehl. Wir erstellen eine "Komponententests" und nennen Sie sie "DinnersControllerTest.cs".

Wir erstellen zwei Testmethoden, die die Aktionsmethode Details() auf die DinnersController zu überprüfen. Die erste überprüft, dass eine Sicht zurückgegeben wird, wenn eine vorhandene Dinner angefordert wird. Die zweite überprüft, dass eine Ansicht "NotFound" zurückgegeben wird, wenn eine nicht existierende Dinner angefordert wird:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Der obige Code kompiliert bereinigt. Wenn wir die Tests ausgeführt haben, fehlschlagen, beide:

![](enable-automated-unit-testing/_static/image6.png)

Wenn Sie die Fehlermeldungen betrachten, sehen wir, dass die Ursache der Tests fehlgeschlagen war, da unsere DinnersRepository-Klasse mit einer Datenbank herstellen konnte. Unsere NerdDinner-Anwendung verwendet eine Verbindungszeichenfolge in einer lokalen SQL Server Express-Datei die unter der \App lebt\_Datenverzeichnis NerdDinner-Anwendungsprojekt. Da unser NerdDinner.Tests Projekt kompiliert und anschließend das Anwendungsprojekt in einem anderen Verzeichnis ausgeführt, ist der relative Pfad unsere Verbindungszeichenfolge falsch.

Wir *konnte* beheben Sie dies durch Kopieren von SQL Express-Datenbankdatei in unserem Test-Projekt, und fügen Sie eine Verbindungszeichenfolge entsprechenden Test hinzu, in der Datei "App.config" unsere Testprojekt. Dies würde der obigen Tests "Blockierung aufgehoben" und Ausführen von abrufen.

Komponententests für Code unter Verwendung einer realen Datenbank bringt jedoch mit eine Reihe von Herausforderungen. Dies gilt insbesondere in folgenden Fällen:

- Er verlangsamt erheblich die Ausführungszeit der Komponententests. Je länger dauert die Ausführung von Tests, desto weniger wahrscheinlich werden Sie sie häufig ausführen. Im Idealfall werden die Komponententests in der Lage, die in Sekunden – ausgeführt werden und etwas, das Sie als natürliche Weise als Kompilieren des Projekts werden soll.
- Es wird die Logik Setup- und Bereinigungsskripts in Tests komplizierter. Jeder Komponententest isoliert und unabhängig von anderen (mit keine nachteiligen Auswirkungen haben oder Abhängigkeiten) werden soll. Bei der Arbeit in einer realen Datenbank müssen Sie beachten, des Status werden und es zwischen Tests zurücksetzen.

Sehen wir uns ein Entwurfsmuster namens "Abhängigkeitsinjektion", die helfen uns diese Probleme umgehen, und vermeiden Sie eine echte Datenbank mit unseren Tests verwendet werden müssen.

### <a name="dependency-injection"></a>Dependency Injection

Zurzeit ist DinnersController eng "der Klasse DinnerRepository verbunden". "Verbinden" verweist auf eine Situation, in dem eine Klasse explizit in einer anderen Klasse benötigt, um arbeiten:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Da die Klasse DinnerRepository Zugriff auf eine Datenbank erforderlich ist, weist die eng verkoppelten Abhängigkeit DinnersController-Klasse DinnerRepository Schluss müssen uns eine Datenbank damit DinnersController Aktionsmethoden getestet werden müssen.

Wir können dieses-Versionsdaten Entwurfsmuster "Abhängigkeitsinjektion" – also einen Ansatz, in Abhängigkeiten (z. B. Repositoryklassen, die den Datenzugriff zu ermöglichen) nicht mehr implizit innerhalb von Klassen erstellt werden, die diese verwenden. Stattdessen Abhängigkeiten können explizit übergeben werden, die Klasse, die sie verwendet Konstruktorargumenten verwenden. Wenn die Abhängigkeiten über Schnittstellen definiert sind, haben wir klicken Sie dann die Flexibilität, "gefälschte" Abhängigkeit Implementierungen für Testszenarien Einheit zu übergeben. Dies ermöglicht es, zu der Abhängigkeit von Test-spezifische Implementierungen zu erstellen, die eigentlich keinen Zugriff auf eine Datenbank erfordern.

Um dies in Aktion zu sehen, implementieren wir Abhängigkeitsinjektion mit unserem DinnersController ein.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extrahieren einer Schnittstelle IDinnerRepository

Der erste Schritt werden Sie eine neue IDinnerRepository-Benutzeroberfläche zu erstellen, die den Vertrag Repository kapselt benötigten unserer Domänencontroller zum Abrufen und Aktualisieren von Abendessen.

Wir können diese Schnittstellenvertrag manuell definieren, indem Sie mit der rechten Maustaste auf den Ordner \Models, und klicken Sie dann die **Add -&gt;neues Element** Menübefehl und erstellen eine neue Schnittstelle namens IDinnerRepository.cs.

Alternativ können wir verwenden die Extrahierung Tools erstellt in Visual Studio Professional (oder höhere Edition) auf automatisch Umgestaltung und erstellen Sie eine Schnittstelle für uns aus der vorhandenen DinnerRepository-Klasse. Um diese Schnittstelle, die mithilfe von VS zu extrahieren, einfach positionieren Sie den Cursor in die Text-Editor für die DinnerRepository-Klasse, und klicken Sie dann mit der rechten Maustaste und wählen Sie die **Umgestalten -&gt;Schnittstelle extrahieren** Menübefehl:

![](enable-automated-unit-testing/_static/image7.png)

Das Dialogfeld "Schnittstelle extrahieren" starten wird und uns für den Namen der Schnittstelle zum Erstellen auffordern. Es wird standardmäßig IDinnerRepository, und wählen Sie automatisch alle öffentliche Methoden in der vorhandenen DinnerRepository-Klasse, die Schnittstelle hinzu:

![](enable-automated-unit-testing/_static/image8.png)

Wenn wir auf die Schaltfläche "ok" klicken, wird Visual Studio eine neue IDinnerRepository Schnittstelle unsere Anwendung hinzugefügt:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

Und unsere vorhandene DinnerRepository-Klasse wird aktualisiert, damit sie die Schnittstelle implementiert:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Aktualisieren von DinnersController Konstruktoreinfügung unterstützen

Wir werden die DinnersController-Klasse, um die neue Oberfläche verwenden jetzt aktualisieren.

Derzeit ist DinnersController hartcodiert, dass das Feld "DinnerRepository" immer eine DinnerRepository-Klasse ist:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Wir müssen es ändern, damit das Feld "DinnerRepository" des Typs statt DinnerRepository IDinnerRepository ist. Wir fügen Sie dann zwei öffentliche DinnersController Konstruktoren. Einer der Konstruktoren kann eine IDinnerRepository als Argument übergeben werden. Die andere ist ein Standardkonstruktor, der unsere vorhandene DinnerRepository-Implementierung verwendet:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Da ASP.NET-MVC standardmäßig unter Verwendung von Standardkonstruktoren Controllerklassen erstellt, unsere DinnersController zur Laufzeit die DinnerRepository-Klasse verwenden weiterhin auf Daten zugreift.

Wir haben unsere Komponententests, aber jetzt aktualisieren, um eine "gefälschte" Dinner Repository-Implementierung, die mit dem Parameter-Konstruktor übergeben. Dieses Repository "gefälschte" Dinner benötigen Sie Zugriff auf eine echte Datenbank keine und stattdessen Beispiel zu in-Memory-Daten.

#### <a name="creating-the-fakedinnerrepository-class"></a>Erstellen der FakeDinnerRepository-Klasse

Wir erstellen eine FakeDinnerRepository-Klasse.

Wir beginnen mit dem Erstellen eines Verzeichnisses "Fakes" in unseren NerdDinner.Tests-Projekt und klicken Sie dann eine neue FakeDinnerRepository-Klasse hinzugefügt (mit der rechten Maustaste auf den Ordner, und wählen Sie **Add -&gt;neue Klasse**):

![](enable-automated-unit-testing/_static/image9.png)

Wir werden den Code aktualisieren, sodass die FakeDinnerRepository-Klasse die IDinnerRepository-Schnittstelle implementiert. Wir können dann mit der rechten Maustaste darauf und wählen Sie den Kontextmenübefehl "Schnittstelle implementieren IDinnerRepository":

![](enable-automated-unit-testing/_static/image10.png)

Dies bewirkt, dass Visual Studio, um unsere FakeDinnerRepository-Klasse mit standardimplementierungen "stub-" automatisch auf alle IDinnerRepository Schnittstelle Mitglieder hinzuzufügen:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Wir aktualisieren Sie dann die FakeDinnerRepository-Implementierung, die nicht in einer in-Memory-Liste funktionieren&lt;Dinner&gt; Auflistung als Konstruktorargument übergeben:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Wir haben jetzt eine gefälschte IDinnerRepository-Implementierung, die eine Datenbank ist nicht erforderlich, und stattdessen deaktiviert eine in-Memory-Liste von Dinner Objekten arbeiten kann.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Verwenden die FakeDinnerRepository mit Komponententests

Wir zurück an die DinnersController-Komponententests, die zuvor aufgrund die Datenbank nicht verfügbar war. Wir können die Testmethoden aus, um eine FakeDinnerRepository mit Beispieldaten in-Memory-Dinner, die mithilfe des folgenden Codes DinnersController aufgefüllt verwenden aktualisieren:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Und nun bei Ausführung dieser Tests beide übergeben:

![](enable-automated-unit-testing/_static/image11.png)

Beste daran, sie nehmen nur einen Bruchteil einer Sekunde ausgeführt und erfordern keine komplizierte Setup/Bereinigungslogik. Wir können jetzt alle unsere DinnersController-Methodencode für die Aktion (einschließlich auflisten, Details, paging erstellen, aktualisieren und löschen) bei Komponententest ohne jemals mit einer realen Datenbank herzustellen.

| **Seite Thema: Dependency Injection Frameworks** |
| --- |
| Ausführen manueller abhängigkeiteneinschleusung (wie wir oben) funktioniert, aber wird schwieriger zu verwalten als die Anzahl der Abhängigkeiten, und erhöht die Komponenten in einer Anwendung. Mehrere Dependency Injection Frameworks vorhanden für .NET, die größere Flexibilität bei der Verwaltung von Abhängigkeit helfen können. Diese Frameworks, auch bezeichnet "Inversion of Control" (IoC) Container, bieten Mechanismen, die ein höheres Maß an Unterstützung für die Konfiguration für angeben und das Übergeben von Abhängigkeiten für Objekte zur Laufzeit (mit am häufigsten Konstruktoreinfügung aktivieren ). Einige der gängigeren OSS-Abhängigkeitsinjektion / IOC-Frameworks in .NET include: AutoFac, Ninject Spring.NET, StructureMap und Windsor. ASP.NET MVC macht-Erweiterbarkeits-APIs, mit denen Entwickler zur Teilnahme an der Auflösung und Instanziierung von Controllern und wodurch Abhängigkeitsinjektion / IoC-Frameworks ordnungsgemäß innerhalb dieses Prozesses integriert werden. Mithilfe eines DI/IOC würde auch aktivieren wir unsere DinnersController – den Standardkonstruktor aufheben dadurch die Kopplung zwischen ihm und dem DinnerRepositorys vollständig entfernt wird, würde. Wir nicht verwenden eine Abhängigkeitsinjektion / IOC-Framework mit unserer NerdDinner-Anwendung. Aber es ist etwas, das wir für die Zukunft gehen konnte die NerdDinner Codebasis und Funktionen vergrößert wurde. |

### <a name="creating-edit-action-unit-tests"></a>Erstellen von Komponententests für Bearbeiten-Aktion

Jetzt erstellen wir einige Komponententests, die die Funktionalität Bearbeiten von der DinnersController zu überprüfen. Wir beginnen mit Testen der HTTP-GET-Version des unserer Aktion bearbeiten:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Wir erstellen ein Tests, das überprüft, ob eine Sicht, die Sicherung von einem DinnerFormViewModel-Objekt zurück, wenn Sie eine gültige Dinner angefordert wird gerendert wird:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Wenn wir den Test ausführen, aber fest wir, dass es schlägt fehl, weil eine null-Verweisausnahme auftreten ausgelöst wird, wenn die Edit-Methode der User.Identity.Name-Eigenschaft der Dinner.IsHostedBy() Überprüfung zugreift.

Das Benutzerobjekt, auf die Basisklasse für Controller kapselt Informationen über den angemeldeten Benutzer und wird von ASP.NET MVC aufgefüllt, wenn sie den Controller zur Laufzeit erstellt. Da wir die DinnersController außerhalb einer Web-Server-Umgebung testen die, das Benutzerobjekt ist nicht festgelegt (also die Nullverweis Ausnahme).

### <a name="mocking-the-useridentityname-property"></a>Simulieren der User.Identity.Name-Eigenschaft

Mockframeworks stellen testen vereinfachen durch Aktivieren von uns falsche Versionen der abhängigen Objekte dynamisch zu erstellen, die unsere Tests unterstützen. Wir können z. B. ein pseudoframework in unserem Test wurde Aktion Bearbeiten verwenden, um ein Benutzerobjekt dynamisch zu erstellen, die unsere DinnersController verwenden können, einen Benutzernamen für die simulierten nachgeschlagen werden. Dadurch wird vermieden, dass einen null-Verweis ausgelöst wird, wenn wir unsere Tests ausführen.

Es gibt viele .NET pseudoframeworks, die mit ASP.NET MVC verwendet werden kann (Sie können eine Liste von ihnen hier finden Sie unter: [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/)). Zum Testen unserer NerdDinner-Anwendung verwenden wir eine open Source-Framework namens "Moq" imitieren, die heruntergeladen werden kann kostenlos von [ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq).

Nachdem das Download abgeschlossen ist, fügen einen Verweis in unserem NerdDinner.Tests-Projekt auf die Assembly Moq.dll wir:

![](enable-automated-unit-testing/_static/image12.png)

Diese wird dann eine Hilfsmethode "CreateDinnersControllerAs(username)" zu unserem Testklasse hinzugefügt, die die User.Identity.Name-Eigenschaft für die Instanz DinnersController "mocks" nimmt einen Benutzernamen als Parameter an, und die:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Wir sind höher Moq verwenden, um ein Mock-Objekt zu erstellen, die ein Objekt ControllerContext fakes (das ist, was zu Controllerklassen Common Language Runtime-Objekte, z. B. Benutzer, Anforderung, Antwort und Sitzung verfügbar zu machen ASP.NET-MVC übergibt). Wir sind Aufrufen der Methode "SetupGet" für das Mock, um anzugeben, dass die Eigenschaft HttpContext.User.Identity.Name ControllerContext Username-Zeichenfolge zurückgeben soll, wenn, die es an die Hilfsmethode übergeben.

Wir können eine beliebige Anzahl von ControllerContext Eigenschaften und Methoden modellieren. Um dies zu veranschaulichen habe ich auch hinzugefügt SetupGet() aufrufen für die Request.IsAuthenticated-Eigenschaft (die nicht tatsächlich benötigt für die folgenden – Tests jedoch; dadurch kann der veranschaulichen, wie Sie Anforderungseigenschaften modellieren können). Wenn wir fertig sind fügen wir der DinnersController unsere Hilfsmethode gibt eine Instanz des ControllerContext Mock zuweisen.

Jetzt können Sie Komponententests schreiben, mit denen diese Hilfsmethode um bearbeiten Szenarien im Zusammenhang mit anderen Benutzern zu testen:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

Und bei Ausführung der Tests jetzt übertragen:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Testszenarien UpdateModel()

Wir haben Tests erstellt, die die HTTP-GET-Version der Aktion bearbeiten abdecken. Jetzt erstellen wir einige Tests, die die HTTP-POST-Version der Aktion bearbeiten zu überprüfen:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Interessante neue Testszenario für uns zur Unterstützung bei dieser Aktionsmethode ist die Verwendung der UpdateModel() Hilfsmethode für die Controller-Basisklasse. Wir werden diese Hilfsmethode verwenden, um die Formular-Post-Werte an unsere Dinner Objektinstanz zu binden.

Im folgenden sind zwei Tests, die zeigt, wie wir die Werte für die UpdateModel() Hilfsmethode mit Formular bereitstellen können. Wir dazu erstellen und Auffüllen eines Objekts FormCollection und klicken Sie dann die Eigenschaft "ValueProvider" auf dem Controller zuweisen.

Der erste Test überprüft, dass nach einer erfolgreichen Speichern Browser an die Aktion Details umgeleitet wird. Der zweite Test stellt sicher, dass die Aktion die Bearbeitungsansicht erneut mit einer Fehlermeldung gleichzusetzen, wenn ungültige Eingabe zurückgesendet wird.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Testen der Zusammenfassung

Wir haben die Core-Konzepte, die zum Testen Controllerklassen Einheit behandelt. Wir können diese Techniken verwenden, auf einfache Weise Hunderte von einfacher Tests erstellen, die das Verhalten der Anwendung überprüft.

Da unsere Controller und Modell Tests eine reale Datenbank nicht erforderlich sind, sind sie sehr schnell und einfach ausgeführt. Es wird möglich, führen Sie Hunderte von automatisierten Tests in Sekunden, und sofort abrufen Rückmeldung über etwas unterbrochen, ob eine Änderung, die wird wurde vorgenommen. Dies hilft uns das Vertrauen kontinuierlich zu verbessern, Umgestaltung und verfeinern die Anwendung bereitstellen.

Wir betroffenen Tests als im letzten Thema in diesem Kapitel – aber nicht, da die Testphase etwas ist sollten Sie am Ende einen Entwicklungsprozess tun! Im Gegensatz dazu, sollten Sie die automatisierten Tests im Entwicklungsprozess so früh wie möglich schreiben. Dadurch kann daher Sie unmittelbar Feedback erhalten, wie Sie entwickeln, können Sie Ihre Anwendung verwendungsfallbeispielen sorgfältig überlegen, und führt Sie zum Entwerfen Ihrer Anwendung mit die fehlerfreie überlagern, und beachten Sie hierzu.

Eine der folgenden Kapitel im Buch besprechen (Test Driven Development, TDD), und wie für die Verwendung mit ASP.NET MVC. Testgesteuerte Entwicklung ist eine iterative Programmierstil, Schreiben Sie zuerst die Tests, die daraus resultierende Code erfüllen. Mit TDD beginnen Sie jede Funktion, durch das Erstellen eines Tests, das die Funktionalität überprüft, die Sie implementieren. Schreiben der Einheit Test zuerst wird sichergestellt, dass Sie verstehen, die Funktion und wie es funktionieren soll. Nur, nachdem der Test geschrieben (und Sie sichergestellt haben, dass ein Fehler auftritt) führen Sie dann implementieren Sie die aktuelle Funktionalität, die überprüft, der Test ob. Da Sie bereits Zeit darum geht, die Verwendung Groß-/Kleinschreibung wie das Feature funktionieren soll Ausgaben, müssen Sie ein besseres Verständnis der Anforderungen und wie am besten zu ihrer Implementierung. Wenn Sie mit der Implementierung abgeschlossen haben, können Sie den Test – erneut ausführen und Abrufen von unmittelbares Feedback im Vergleich zu, gibt an, ob die Funktion ordnungsgemäß funktioniert. TDD mehr in Kapitel 10 beschrieben.

### <a name="next-step"></a>Nächster Schritt

Einige endgültigen eingebunden Kommentare.

> [!div class="step-by-step"]
> [Zurück](use-ajax-to-implement-mapping-scenarios.md)
> [Weiter](nerddinner-wrap-up.md)
