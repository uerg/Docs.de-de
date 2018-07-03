---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Aktivieren von automatisierten Komponententests | Microsoft-Dokumentation
author: microsoft
description: Schritt 12 zeigt, wie eine Suite von automatisierten Komponententests entwickeln, zu überprüfen, ob unsere NerdDinner-Funktionen und die wir erhalten der Zuversicht, Änderungen vornehmen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 0d7986bcff7a0ddc89d40c3c03eb4ebb26a8b230
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402482"
---
<a name="enable-automated-unit-testing"></a>Aktivieren von automatisierten Komponententests
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 12, der ein kostenloses ["NerdDinner"-webanwendungstutorial](introducing-the-nerddinner-tutorial.md) , die führt – Exemplarische Vorgehensweise erstellen eine kleine, jedoch abgeschlossen haben, Web-Anwendung mithilfe von ASP.NET MVC-1.
> 
> Schritt 12 zeigt, wie Sie eine Suite von automatisierten Komponententests entwickeln, zu überprüfen, ob unsere NerdDinner-Funktionen, und die wird uns der Zuversicht, Änderungen und Verbesserungen für die Anwendung in der Zukunft zu gestalten.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner Schritt 12: Komponententests

Wir entwickeln eine Suite von automatisierten Komponententests zu überprüfen, ob unsere NerdDinner-Funktionen, und die wird uns der Zuversicht, Änderungen und Verbesserungen für die Anwendung in der Zukunft zu gestalten.

### <a name="why-unit-test"></a>Warum Komponententest?

Auf dem Laufwerk in die Arbeit eines morgens ins Büro haben Sie eine plötzliche Flash Inspiration zu einer Anwendung, die, der Sie gerade arbeiten. Sie stellen fest, dass eine Änderung, die Sie implementieren können, die die Anwendung erheblich besser machen können. Es kann sein, ein refactoring, die den Code bereinigt, fügt ein neues Feature oder ein Fehler behoben.

Die Frage, die Sie confronts, wenn Sie auf dem Computer eintreffen ist: "wie safe ist es, diese Verbesserung machen?" Was geschieht, wenn der Änderung, besitzt Nebenwirkungen oder unterbricht etwas? Die Änderung kann einfach sein, und nehmen nur einige Minuten, bis zu implementieren, aber was geschieht, wenn es sich um Stunden, bis Sie manuell alle die Anwendungsszenarien testen dauert? Was geschieht, wenn vergessen Sie, ein Szenario, und wechselt eine fehlerhafte Anwendung, die in die produktionsumgebung? Ist diese Verbesserung der Mühe Wert wirklich vornehmen?

Automatisierte Komponententests ermöglichen ein Sicherheitsnetz bereitstellen, die Sie Ihre Anwendungen kontinuierlich verbessern können, und verhindern, dass die Aufrüstung des Codes, den Sie gerade arbeiten. Dies müssen von automatisierten Tests, die schnell zu überprüfen, dass die Funktionalität ermöglicht es Ihnen, du programmierst sattelfest – und dabei, nehmen Sie Verbesserungen, die Sie andernfalls nicht vertraut sind der Ansicht verfügen können. Sie dienen zum Erstellen von Lösungen, die besser verwaltbar sind und eine längere Lebensdauer – Dies führt zu einer viel höheren Rendite zurückgegeben haben.

Das ASP.NET MVC-Framework ist es einfach und natürlich auf Unit Test Anwendungsfunktionen. Darüber hinaus können einen Test Driven Development (TDD)-Workflow, der Test-First-Basis-Entwicklung ermöglicht.

### <a name="nerddinnertests-project"></a>NerdDinner.Tests-Projekt

Wenn wir unsere NerdDinner-Anwendung zu Beginn dieses Tutorials erstellt haben, wurden wir aufgefordert, ein Dialogfeld gefragt, ob wir wollten ein Komponententestprojekt zusammen mit dem Application-Projekt zu erstellen:

![](enable-automated-unit-testing/_static/image1.png)

Wir bleiben die "Ja, Komponententestprojekt erstellen" Optionsfeld ausgewählt – was in einem Projekt auf unserer Projektmappe hinzugefügt wird "NerdDinner.Tests" geführt hat:

![](enable-automated-unit-testing/_static/image2.png)

Das NerdDinner.Tests-Projekt verweist auf die Projektassembly des NerdDinner-Anwendung, und es uns ermöglicht, fügen Sie einfach automatisierte Tests hinzu, die die Funktion der Anwendung zu überprüfen.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Erstellen von Komponententests für unseren Dinner-Model-Klasse

Fügen Sie einige Tests unsere NerdDinner.Tests-Projekt, das die Dinner-Klasse zu überprüfen, die wir erstellt haben, wenn wir unsere Modellschicht erstellt.

Wir beginnen, erstellen Sie einen neuen Ordner in unserem Testprojekt namens "Models", in dem wir unsere-modellbezogenes Tests hinzufügen möchten. Wir klicken Sie dann mit der rechten Maustaste auf den Ordner und wählen Sie die **Add-&gt;neuen Test** Menübefehl. Hierdurch wird das Dialogfeld "Neuen Test hinzufügen" angezeigt.

Wählen wir zum Erstellen von "Unit Test", und nennen Sie es mit "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Beim Klicken auf die Schaltfläche "ok" Visual Studio hinzufügen (eine DinnerTest.cs-Datei zum Projekt und öffnen):

![](enable-automated-unit-testing/_static/image4.png)

Die standardmäßige Visual Studio-Komponententestvorlage verfügt über eine Reihe von partitionierungsaufgaben darin, die ich Webbrowsing finden. Lassen Sie uns bereinigen sie Sie einfach den folgenden Code enthalten:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

Die [TestClass]-Attribut auf die oben gezeigte Klasse DinnerTest identifiziert sie als eine Klasse, die Tests als auch optionale testinitialisierung und Beendigungscode enthalten soll. Wir können die Tests in diesem definieren, durch das Hinzufügen von öffentlicher Methoden, die eine [TestMethod]-Attribut verfügen.

Im folgenden finden Sie die erste von zwei Tests, die wir hinzufügen, die unsere Dinner-Klasse ausführen. Der erste Test überprüft, dass unsere Dinner ungültig ist, wenn eine neue Dinner erstellt wird, ohne alle Eigenschaften richtig festgelegt wird. Der zweite Test stellt sicher, dass unsere Dinner gültig ist, wenn es sich bei eine Dinner alle Eigenschaften mit gültigen Werten festgelegt wurde:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Sie sehen oben, dass unsere Testnamen sehr deutlich (und ein wenig ausführlich) sind. Wir sind dabei, da erstellen, Hunderte oder Tausende von Tests kleiner kann am Ende und zu vereinfachen, können Sie schnell feststellen, die Absicht und das Verhalten der einzelnen von ihnen (insbesondere, wenn wir über eine Liste von Fehlern in einem Test Runner betrachten) werden sollten. Nach der die Funktionalität, die sie testen möchten, sollten die Testnamen benannt werden. Oben verwenden wir ein "Nomen\_sollten\_Verb" Benennungsschema folgen.

Wir werden die Tests mit dem Muster – das steht für "Arrange, Act, Assert" testen "AAA" strukturieren:

- Anordnen: Einrichten der Komponententests
- : Führen Sie die Komponenten unter testbedingungen und erfassen Ergebnisse zu
- Assert-: Das Verhalten überprüfen

Beim Schreiben wir führen zu viele Tests zu vermeiden, dass die einzelnen Tests werden soll. Stattdessen sollte jeder Test nur ein einzelnes Konzept überprüfen (wodurch sie viel einfacher, die Ursache der Fehler ermitteln wird). Ein guter Anhaltspunkt ist nur ein einzelnes assert-Anweisung für jeden Test haben. Wenn Sie mehr als eine assert-Anweisung in einer Testmethode haben, stellen Sie sicher, dass sie alle verwendet werden, um das gleiche Konzept zu testen. Stellen Sie im Zweifelsfall einen anderen Test ein.

### <a name="running-tests"></a>Tests werden ausgeführt

Visual Studio 2008 Professional (und höheren Versionen) umfasst eine integrierte Testprogramm, das zum Ausführen von Visual Studio-Komponententest-Projekten in der IDE verwendet werden kann. Ausgewählt werden können die **Test-&gt;ausführen -&gt;alle Tests in der Projektmappe** Menü Befehlsklasse (oder drücken Sie STRG-R, A) um alle unsere Komponententests auszuführen. Oder Alternativ können wir unsere Cursor innerhalb einer bestimmten Klasse oder Test Testmethode zu positionieren und Verwenden der **Test-&gt;ausführen -&gt;Tests im aktuellen Kontext** Menübefehl (oder drücken Sie STRG-R, T) eine Teilmenge der Komponententests auszuführen.

Lassen Sie uns position der Cursor innerhalb der DinnerTest-Klasse, und geben Sie "STRG-R, T", führen Sie die beiden Tests, die soeben definiert. Wenn wir ein Fenster "Testergebnisse" dazu in Visual Studio angezeigt werden, und wir sehen die Ergebnisse des Testlaufs darin aufgeführten:

![](enable-automated-unit-testing/_static/image5.png)

*Hinweis: Visual Studio im Testergebnisfenster wird nicht die Class Name-Spalte wird standardmäßig angezeigt. Sie können dies hinzufügen, indem Sie mit der rechten Maustaste im Fenster Testergebnisse und des Menübefehls für die Spalten hinzufügen/entfernen.*

Unsere zwei Tests dauerten nur einen Bruchteil einer Sekunde ausgeführt – und finden beide übergeben. Wir können jetzt auf und erweitern diese, indem Sie zusätzliche Tests, die spezielle Regel Überprüfungen zu überprüfen, und behandelt die zwei Hilfsmethoden - IsUserHost() und IsUserRegisterd() –, die wir hinzugefügt, dass die Dinner-Klasse zu erstellen. Wenn alle diese Tests vorhanden, für die Dinner-Klasse wird es viel einfacher und sicherer zu neue Geschäftsregeln und Validierungen in der Zukunft hinzufügen machen. Wir können Dinner unsere neue Regellogik hinzu, und Sie können innerhalb von Sekunden stellen Sie sicher, dass sie eine der vorherigen Logik Funktionalität beeinträchtigt noch nicht.

Beachten Sie, wie mit einer beschreibenden Testnamen erleichtert Ihnen schnell verstehen, was jeder Test wird überprüft wird. Ich empfehle die Verwendung der **Tools -&gt;Optionen** Menübefehl, öffnen den Test Tools -&gt;Test Execution-Konfigurationsbildschirm, und überprüfen den "durch Doppelklicken auf eine fehlerhafte oder nicht eindeutig Komponententestergebnis zeigt Zeitpunkt des Fehlers im Test"-Kontrollkästchen. Dadurch können Sie doppelklicken auf einen Fehler im Testergebnisfenster, und fahren Sie direkt mit der Assertionsfehler.

### <a name="creating-dinnerscontroller-unit-tests"></a>Erstellen von Komponententests mit "dinnerscontroller"

Jetzt erstellen wir einige Komponententests, die unsere "dinnerscontroller"-Funktionalität zu überprüfen. Wir beginnen mit der rechten Maustaste auf den Ordner "Controllers" in unserem Test-Projekt und wählen Sie dann die **Add-&gt;neuen Test** Menübefehl. Wir erstellen eine "Unit Test" und nennen Sie es mit "DinnersControllerTest.cs".

Wir erstellen zwei Testmethoden, die die Aktionsmethode Details(), auf die "dinnerscontroller" zu überprüfen. Die erste überprüft, dass eine Sicht zurückgegeben wird, wenn eine vorhandene Dinner angefordert wird. Die zweite gewährleistet, dass es sich bei eine Ansicht "NotFound" zurückgegeben wird, wenn eine nicht existierende Dinner angefordert wird:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Der obige Code kompiliert bereinigen. Wenn die Tests ausgeführt wird, Fehler jedoch beide:

![](enable-automated-unit-testing/_static/image6.png)

Wenn Sie die Fehlermeldungen betrachten, sehen wir, dass der Grund, der die Tests sind fehlgeschlagen ist, da unsere DinnersRepository-Klasse mit einer Datenbank herstellen konnte. Die NerdDinner-Anwendung verwendet eine Verbindungszeichenfolge in einer lokalen SQL Server Express-Datei die unter der \App lebt\_Datenverzeichnis das Projekt für die NerdDinner-Anwendung. Der relativen Pfad-Speicherort, der die Verbindungszeichenfolge ist falsch, da unser NerdDinner.Tests Projekt kompiliert und dann in einem anderen Verzeichnis das Anwendungsprojekt führt.

Wir *konnte* dies beheben, indem Sie die SQL Express-Datenbankdatei in unserem Testprojekt kopiert, und fügen Sie eine entsprechende Test-Verbindungszeichenfolge hinzu, in der Datei App.config für unser Testprojekt. Dadurch erhalten die vorgenannten Tests aufgehoben, und der ausgeführt wird.

Komponententests für Code mit einer echten Datenbank enthält jedoch eine Reihe von Herausforderungen. Dies gilt insbesondere in folgenden Fällen:

- Sie verlangsamt erheblich die Ausführungszeit von Komponententests. Je länger dauert es zum Ausführen von Tests, desto weniger wahrscheinlich sind Sie auf die sie häufig ausführen. Im Idealfall sollten Sie die Komponententests in der Lage, die in Sekunden – ausgeführt werden und werden dies ist als auf natürliche Weise als beim Kompilieren des Projekts.
- Es erschwert die Setup- und Bereinigungsskripts Logik in Tests. Jeder Komponententest isoliert und unabhängig von anderen (mit keine Nebenwirkungen oder Abhängigkeiten) werden soll. Beim Arbeiten mit einer echten Datenbank müssen Sie achten Sie darauf, dass Sie Status, und es zwischen Tests zurücksetzen.

Wir sehen uns ein Entwurfsmuster namens "Dependency Injection", mit denen wir diese Probleme umgehen, und vermeiden, dass eine echte Datenbank mit unseren Tests verwenden kann.

### <a name="dependency-injection"></a>Dependency Injection

Zurzeit ist "dinnerscontroller"eng "der Klasse" dinnerrepository "verbunden". "Koppeln" bezieht sich auf eine Situation, in dem eine Klasse explizit in einer anderen Klasse benötigt, um arbeiten:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Da die Klasse "dinnerrepository" Zugriff auf eine Datenbank erforderlich ist, hat die Verarbeitung eng gekoppelter Abhängigkeit die Klasse "dinnerscontroller" auf "dinnerrepository" Schluss müssen wir eine Datenbank in der Reihenfolge für die Aktionsmethoden "dinnerscontroller" getestet werden.

Wir können Umgehung dieses Problems durch den Einsatz von ein Entwurfsmuster namens "Dependency Injection" – das ist ein Ansatz, in denen Abhängigkeiten (z. B. die Repositoryklassen, die den Datenzugriff zu ermöglichen) nicht mehr implizit in Klassen erstellt werden, die diese verwenden. Stattdessen Abhängigkeiten übergeben werden können explizit auf die Klasse, die sie verwendet Konstruktorargumenten verwenden. Wenn die Abhängigkeiten über Schnittstellen definiert sind, haben wir dann die Flexibilität, die in "gefälschtes" Dependency-Implementierungen für Testszenarien Einheit zu übergeben. Dadurch können wir zum Erstellen von Test-spezifischen Abhängigkeit Implementierungen, die eigentlich keinen Zugriff auf eine Datenbank erfordern.

Um dies in Aktion zu sehen, implementieren Sie Abhängigkeitsinjektion mit unserer "dinnerscontroller".

#### <a name="extracting-an-idinnerrepository-interface"></a>Extrahieren einer Schnittstelle "idinnerrepository"

Im ersten Schritt wird eine neue Schnittstelle für "idinnerrepository" zu erstellen, die den Vertrag Repository, die, den unsere-Controller zu benötigen kapselt, abrufen und Aktualisieren von Dinner, sein.

Wir können diese Schnittstellenvertrag manuell definieren, indem mit der rechten Maustaste auf den Ordner "\Models" auswählen und dann die **Add-&gt;neues Element** Menübefehl und eine neue Schnittstelle namens IDinnerRepository.cs erstellen.

Alternativ können wir verwenden die Umgestaltung Tools integriert in Visual Studio Professional (und höhere Editionen) mit automatisch extrahieren und erstellen Sie eine Schnittstelle für uns von unseren vorhandenen "dinnerrepository"-Klasse. Um diese Schnittstelle, die mithilfe von Visual Studio zu extrahieren, einfach positionieren des Cursors im Text-Editor in der Klasse "dinnerrepository", und klicken Sie dann mit der rechten Maustaste und wählen Sie die **Umgestalten -&gt;Schnittstelle extrahieren** Menübefehl:

![](enable-automated-unit-testing/_static/image7.png)

Das Dialogfeld "Schnittstelle extrahieren" Öffnen wird und uns für den Namen der Schnittstelle zum Erstellen auffordern. Es wird standardmäßig auf "idinnerrepository", und wählen Sie automatisch alle öffentliche Methoden für die vorhandene "dinnerrepository"-Klasse, die Schnittstelle hinzu:

![](enable-automated-unit-testing/_static/image8.png)

Wenn wir auf die Schaltfläche "ok" klicken, wird Visual Studio eine neue Schnittstelle für "idinnerrepository" der Anwendung hinzugefügt:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

Und unsere vorhandenen "dinnerrepository"-Klasse aktualisiert werden, sodass sie die Schnittstelle implementiert:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Aktualisieren von "dinnerscontroller" zur Unterstützung der Konstruktorinjektion

Wir führen nun die DinnersController-Klasse, um die neue Schnittstelle verwenden.

Derzeit "dinnerscontroller" hartcodiert, das Feld "DinnerRepository" immer eine DinnerRepository-Klasse ist:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Wir werden es so, dass das Feld "DinnerRepository" vom Typ "idinnerrepository" anstelle von "dinnerrepository" ändern. Wir werden dann zwei öffentliche Konstruktoren von "dinnerscontroller" hinzufügen. Einer der Konstruktoren kann es sich um ein "idinnerrepository" als Argument übergeben werden. Die andere ist eine Standard-Konstruktor, der die vorhandene Implementierung von "dinnerrepository" verwendet:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Da ASP.NET MVC standardmäßig die Controller-Klassen mit Standardkonstruktoren erstellt, unsere "dinnerscontroller" zur Laufzeit die Klasse "dinnerrepository" verwenden weiterhin auf Daten zugreift.

Wir können unsere Komponententests, allerdings jetzt aktualisieren, um ein "gefälschtes" Dinner-Repository-Implementierung, die mit dem parameterkonstruktor übergeben. Dieses Repository "gefälschtes" Dinner muss nicht den Zugriff auf eine echte Datenbank, und verwenden stattdessen Beispiel zu in-Memory-Daten.

#### <a name="creating-the-fakedinnerrepository-class"></a>Erstellen der FakeDinnerRepository-Klasse

Wir erstellen eine FakeDinnerRepository-Klasse.

Wir beginnen mit dem Erstellen eines "Fakes"-Verzeichnisses, in unserem NerdDinner.Tests-Projekt und klicken Sie dann eine neue FakeDinnerRepository-Klasse hinzugefügt (mit der rechten Maustaste auf den Ordner, und wählen Sie **Add-&gt;neue Klasse**):

![](enable-automated-unit-testing/_static/image9.png)

Wir aktualisieren den Code, damit die FakeDinnerRepository-Klasse, die "idinnerrepository"-Schnittstelle implementiert. Wir können dann mit der rechten Maustaste darauf und wählen Sie den Befehl im Kontextmenü "Schnittstelle implementieren" idinnerrepository"":

![](enable-automated-unit-testing/_static/image10.png)

Dies bewirkt, dass Visual Studio automatisch alle den Schnittstellenmember "idinnerrepository" unserer Klasse FakeDinnerRepository mit standardimplementierungen "Abriss" hinzuzufügen:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Wir können dann die FakeDinnerRepository-Implementierung funktioniert nicht in einer in-Memory-Liste aktualisieren&lt;Dinner&gt; Auflistung als Konstruktorargument übergeben:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Wir haben jetzt eine Fakeimplementierung "idinnerrepository", die eine Datenbank ist nicht erforderlich, und stattdessen aus einer in-Memory-Liste von Dinner-Objekten arbeiten kann.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Verwenden die FakeDinnerRepository mit Komponententests

Kehren wir zurück zu "dinnerscontroller" Komponententests, die zuvor aufgrund die Datenbank nicht verfügbar war. Wir können die Testmethoden, die Verwendung einer FakeDinnerRepository mit Beispieldaten im Arbeitsspeicher Dinner, die anhand des folgenden Codes "dinnerscontroller" aufgefüllt aktualisieren:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Und nun, wenn wir diese Tests werden ausgeführt, die beide übergeben:

![](enable-automated-unit-testing/_static/image11.png)

Das beste daran, sie nur einen Bruchteil einer Sekunde ausgeführt werden und komplexe Einrichtung/Bereinigungslogik ist nicht erforderlich. Wir können nun alle unsere "dinnerscontroller"-Methodencode für die Aktion (einschließlich auflisten, Paginierung, Informationen zu erstellen, aktualisieren und löschen) bei Komponententests ohne jemals eine Verbindung mit einer echten Datenbank herstellen.

| **Seite Thema: Abhängigkeitsinjektionsframeworks** |
| --- |
| Ausführen von manuellen Abhängigkeitsinjektion (wie wir oben) funktioniert, jedoch wird schwieriger zu verwalten als die Anzahl der Abhängigkeiten, und Komponenten in einer Anwendung steigt. Mehrere abhängigkeitsinjektionsframeworks sind für .NET, die bieten größere Flexibilität bei der Verwaltung von Abhängigkeiten vorhanden. Diesen Frameworks können die manchmal auch genannt "Inversion of Control" (IoC) Container, bieten Mechanismen, die eine zusätzliche Ebene der Unterstützung für angeben, und übergeben Abhängigkeiten für Objekte zur Laufzeit (mit den meisten Fällen Konstruktorinjektion die Konfiguration zu aktivieren. ). Einige der gängigen OSS Dependency Injection / IOC-Frameworks in .NET include: AutoFac, Ninject, Spring.NET, StructureMap und Windsor. ASP.NET MVC macht Erweiterbarkeits-APIs, mit denen Entwickler zur Teilnahme an der Auflösung und Instanziierung von Controllern und dem Dependency Injection ermöglicht / IoC-Frameworks, die innerhalb dieses Prozesses ordnungsgemäß integriert werden. Mit einem DI/IOC-Framework würde auch können wir den Standardkonstruktor aus unserer "dinnerscontroller" – zu entfernen, wodurch die Kopplung zwischen ihnen und dem DinnerRepositorys vollständig zu entfernen, würde. Wir verwenden nicht die eine Abhängigkeitsinjektion / IOC-Framework mit unserer NerdDinner-Anwendung. Aber es ist etwas, das wir für die Zukunft verwenden können, wenn die NerdDinner-Codebasis und Funktionen vergrößert wurde. |

### <a name="creating-edit-action-unit-tests"></a>Erstellen von Komponententests für Edit-Aktion

Jetzt erstellen wir einige Komponententests, die die Funktionalität Bearbeiten der "dinnerscontroller" zu überprüfen. Wir beginnen mit die HTTP-GET-Version von unserem Bearbeitungsaktion testen:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Wir erstellen einen Test, der überprüft, ob eine Ansicht unterstützt, die von einem Objekt DinnerFormViewModel gerendert wird, zurück, wenn Sie eine gültige Dinner angefordert wird:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Wenn den Test ausgeführt wird, jedoch feststellen wir, dass es schlägt fehl, weil eine null-Verweis-Ausnahme ausgelöst wird, wenn Edit-Methode die Eigenschaft "User.Identity.Name" zum Ausführen der Dinner.IsHostedBy() zugreift.

Die User-Objekt, auf der Basis-Controllerklasse kapselt Informationen über den angemeldeten Benutzer, und wird von ASP.NET MVC aufgefüllt, wenn es sich bei den Controller zur Laufzeit erstellt. Da wir den "dinnerscontroller" außerhalb einer Web-Server-Umgebung testen, das Benutzerobjekt nicht festgelegt (daher der nullverweisausnahme auftreten).

### <a name="mocking-the-useridentityname-property"></a>Simulieren der User.Identity.Name-Eigenschaft

Mockframeworks erleichtern des Testens durch Aktivieren der falsche Versionen abhängiger Objekte dynamisch zu erstellen, die unseren Tests zu unterstützen. Beispielsweise können wir ein Mockframework im Test-Aktion Bearbeiten verwenden, um dynamisch ein Benutzerobjekt zu erstellen, die unsere "dinnerscontroller" verwenden können, um eine simulierte Benutzername suchen. Dadurch wird vermieden, dass einen null-Verweis ausgelöst wird, wenn wir unseren Test ausführen.

Es gibt viele .NET mocking-Frameworks, die mit ASP.NET MVC verwendet werden können (Sie finden eine Liste von ihnen hier: [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/)). Zum Testen unserer verwenden wir eine open Source-Framework namens "Moq" mocking NerdDinner-Anwendung, die heruntergeladen werden kann kostenlos von [ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq).

Nach dem Herunterladen, fügen einen Verweis in unserem NerdDinner.Tests-Projekt auf die Assembly Moq.dll wir:

![](enable-automated-unit-testing/_static/image12.png)

Wir fügen klicken Sie dann eine Hilfsmethode "CreateDinnersControllerAs(username)" für unseren Testklasse, die einen Benutzernamen als Parameter an, und die verwendet werden. anschließend "mocks" die Eigenschaft "User.Identity.Name" auf der Instanz von "dinnerscontroller":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Oben verwenden wir Moq, um ein Mock-Objekt zu erstellen, die fakes-ein ControllerContext-Objekt (das ist was ASP.NET MVC zu Controllerklassen Common Language Runtime-Objekte, z. B. Benutzer, Anforderung, Antwort und -Sitzung verfügbar zu machen übergibt). Wir sind Aufrufen der Methode "SetupGet" für das Mock, um anzugeben, dass die Eigenschaft HttpContext.User.Identity.Name ControllerContext die Benutzernamen-Zeichenfolge zurückgeben soll, die wir an die Hilfsmethode übergeben.

Wir können eine beliebige Anzahl von ControllerContext Eigenschaften und Methoden simulieren. Um dies zu veranschaulichen habe ich auch hinzugefügt einen SetupGet()-Aufruf für die Request.IsAuthenticated-Eigenschaft (die nicht tatsächlich benötigt für die folgenden – Tests aber wodurch veranschaulichen, wie Sie die Anforderungseigenschaften simulieren können). Wenn wir fertig sind fügen wir die gibt die Hilfsmethode "dinnerscontroller" eine Instanz des Mock ControllerContext zuweisen.

Wir können nun die Komponententests schreiben, die diese Hilfsmethode verwenden, um bearbeiten-Szenarios, die im Zusammenhang mit anderen Benutzern zu testen:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

Und wenn wir die Tests ausführen jetzt übertragen:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>UpdateModel() Testszenarien

Wir haben die Tests erstellt, die die HTTP-GET-Version, der die Bearbeitungsaktion abdecken. Als Nächstes erstellen wir einige Tests, die die HTTP-POST-Version von der Aktion "Bearbeiten" zu überprüfen:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Das interessante neue Tests Szenario für die Unterstützung von mit dieser Aktionsmethode ist die Nutzung der UpdateModel() Hilfsmethode für die Controller-Basisklasse. Wir werden diese Hilfsmethode verwenden, um die Formular-Post-Werte an unsere Dinner Objekt zu binden.

Im folgenden finden Sie zwei Tests, die zeigt, wie wir Formular bereitgestellt Werte für die UpdateModel()-Hilfsmethode verwendet bereitstellen können. Wir zu diesem Zweck erstellen und Auffüllen einer FormCollection-Objekt, und klicken Sie dann die Eigenschaft "ValueProvider" auf dem Controller zuweisen.

Der erste Test überprüft, dass auf einen erfolgreichen Speichervorgang der Browser, um die Details-Aktion umgeleitet wird. Im zweite Test wird überprüft, wenn ungültige Eingabe gepostet wird die Aktion die Bearbeitungsansicht erneut mit einer Fehlermeldung erneut angezeigt wird.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Testen die Zusammenfassung

Wir haben die grundlegenden Konzepte Unit Test Controller-Klassen beteiligt behandelt. Wir können diese Techniken verwenden, auf einfache Weise auf Hunderte von einfachen Tests erstellen, die das Verhalten der Anwendung zu überprüfen.

Da es sich bei unseren Controller und den Modell-Tests nicht über eine echte Datenbank benötigen, sind sie sehr schnell und einfach ausführen. Wir werden Hunderte von automatisierten Tests in Sekunden ausgeführt und erhalten Sie unmittelbar Feedback, etwas unterbrochen, ob eine Änderung, die wir vorgenommen wurde. Dies hilft uns die Gewissheit kontinuierlich zu verbessern, Umgestaltung und verfeinern die Anwendung bereitzustellen.

Wir betroffenen Tests als im letzten Thema in diesem Kapitel – aber nicht, da das Testen etwas ist tun Sie am Ende einen Entwicklungsprozess! Im Gegenteil, sollten Sie die automatisierten Tests in Ihren Entwicklungsprozess so früh wie möglich schreiben. Dies ermöglicht daher unmittelbares Feedback erhalten, wie Sie entwickelt haben, können Sie die anwendungsfallszenarien für Ihre Anwendung so zuvorkommend denken, und führt Sie zum Entwerfen Ihrer Anwendung mit die saubere Schichten und denken Sie daran gekoppelt.

Eine der folgenden Kapitel in dem Buch besprechen (Test Driven Development, TDD), und wie sie mit ASP.NET MVC zu verwenden. Testgesteuerte Entwicklung ist eine iterative Programmierstil, Schreiben Sie zuerst die Tests, die sich ergebende Code erfüllt. Mit TDD zunächst erstellen Sie jede Funktion einen Test, der die Funktionalität wird, die Sie überprüft nun zu implementieren. Schreiben der Einheit Test zuerst trägt dazu bei, dass Sie eindeutig verstehen, die Funktion und wie sie funktionieren soll. Nur, nachdem der Test wird geschrieben (und Sie überprüft haben, dass ein Fehler auftritt) führen Sie dann implementieren Sie die eigentliche Funktionalität, die der Test überprüft. Da Sie bereits gearbeitet, haben Zeit Gedanken über den Anwendungsfall wie das Feature funktionieren soll, müssen Sie ein besseres Verständnis der Anforderungen und ihre Implementierung wie am besten. Wenn Sie mit der Implementierung fertig sind, Sie führen Sie die Tests erneut aus, und erhalten unmittelbar Feedback zu können, ob das Feature ordnungsgemäß funktioniert. Genau darum TDD in Kapitel 10.

### <a name="next-step"></a>Nächster Schritt

Einige abschließende Wrap von Kommentaren.

> [!div class="step-by-step"]
> [Zurück](use-ajax-to-implement-mapping-scenarios.md)
> [Weiter](nerddinner-wrap-up.md)
