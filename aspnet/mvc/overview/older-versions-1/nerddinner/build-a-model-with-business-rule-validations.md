---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: "Erstellen eines Modells mit Business Regel Überprüfungen | Microsoft Docs"
author: microsoft
description: "Schritt 3 veranschaulicht, wie ein Modell zu erstellen, können wir, um beide Abfrage verwenden und die Datenbank für unsere NerdDinner-Anwendung zu aktualisieren."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: dbe6370979f218988c168df3e80314ef9b338fbd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="build-a-model-with-business-rule-validations"></a>Erstellen eines Modells mit Business Regel Überprüfungen
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 3 mit einer kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.
> 
> Schritt 3 veranschaulicht, wie ein Modell zu erstellen, können wir, um beide Abfrage verwenden und die Datenbank für unsere NerdDinner-Anwendung zu aktualisieren.
> 
> Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner, Schritt 3: Erstellen des Modells

Bezieht sich der Begriff "Modell" auf die Objekte, die die Daten von der Anwendung sowie die entsprechenden Domänenlogik, überprüfungs- und Geschäftsregeln mit den es darstellen, in einem Model-View-Controller-Framework. Das Modell ist in vielerlei Hinsicht die "Basis" einer MVC-basierte Anwendung und später im Grunde eingehendem steuert das Verhalten des Zertifikats.

ASP.NET MVC-Framework unterstützt eine beliebige Data Access-Technologie verwenden, und Entwickler haben die Wahl aus einer Vielzahl von umfangreichen .NET Datenoptionen auf ihre Modelle, einschließlich implementieren: LINQ to Entitäten, die LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM oder nur die unformatierten ADO. NET-DataReaders oder DataSets.

Unsere NerdDinner-Anwendung werden wir LINQ to SQL verwenden, um ein einfaches Modell zu erstellen, das unsere Datenbankentwurf ziemlich genau entspricht, und einige benutzerdefinierte Validierungsregeln Datenlogik und Geschäftsregeln hinzugefügt. Wir anschließend Implementieren einer Repository-Klasse, die abstrakte unterwegs hilft der dauerhaftigkeitsimplementierung Daten vom Rest der Anwendung und ermöglicht uns problemlos Einheit testen.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL ist ein ORM (objektrelationale Zuordnungen), die als Teil von .NET 3.5 geliefert wird.

LINQ to SQL bietet eine einfache Möglichkeit, die Datenbanktabellen .NET Klassen zuordnen, die wir für code können. Für unsere NerdDinner Anwendung verwenden wir es, um die Tabellen Abendessen und Antwort in unserer Datenbank für Dinner und Antwort-Klassen zuordnen. Die Spalten der Tabellen Abendessen und Antwort entsprechen den Eigenschaften auf der Dinner und Antwort-Klasse. Jedes Objekt Dinner und Antwort wird eine separate Zeile innerhalb der Abendessen oder Antwort Tabellen in der Datenbank darstellen.

LINQ to SQL kann wir zu vermeiden, dass manuell erstellt SQL-Anweisungen zum Abrufen und Aktualisieren von Essen und Antwort Objekte mit Datenbankdaten. Stattdessen definieren wir die Dinner und Antwort-Klassen, wie die Zuordnung zum/aus der Datenbank und die Beziehungen zwischen ihnen. LINQ to SQL übernimmt dann generieren Sie die entsprechenden SQL-Ausführungslogik zur Laufzeit verwendet werden soll, wenn wir interagieren, und sie verwenden sorgfältig.

Wir können die LINQ-sprachunterstützung in VB und c# verwenden, um ausdrucksvolle Abfragen schreiben, die Dinner und Antwort abrufen Objekte aus der Datenbank. Dadurch wird die Menge an Datencode schreiben, wir müssen minimiert und erlaubt es uns wirklich saubere Anwendungen zu erstellen.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Hinzufügen von LINQ to SQL-Klassen zum Projekt

Wir beginnen, indem Sie mit der rechten Maustaste auf den Ordner "Modelle" im Projekt, und wählen Sie die **Add -&gt;neues Element** Menübefehl:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Hierdurch wird das Dialogfeld "Neues Element hinzufügen" angezeigt. Wir filtern, indem Sie die "Datenkategorie" und wählen Sie die Vorlage "LINQ to SQL-Klassen" darin:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Wir nennen Sie das Element "NerdDinner" und klicken Sie auf die Schaltfläche "Hinzufügen". Visual Studio eine Datei NerdDinner.dbml unsere \Models-Verzeichnis hinzufügen, und öffnen Sie dann die LINQ to SQL Objektrelationaler Designer:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Erstellen von Modellklassen für Daten mit LINQ to SQL

LINQ to SQL kann wir schnell Modellklassen Daten aus vorhandenen Datenbankschema zu erstellen. To-do dies wir die NerdDinner-Datenbank im Server-Explorer öffnen und wählen Sie die Tabellen in diesem Modell werden soll:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Wir können dann die Tabellen in der LINQ auf SQL-Designeroberfläche ziehen. Wenn wir diese LINQ to SQL durchführen Dinner automatisch erstellt und die Antwort-Klassen, die mit dem Schema der Tabellen (mit Klasseneigenschaften, die den Spalten der Datenbanktabelle zugeordnet wird):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Standardmäßig ist die LINQ to SQL "in den plural setzt Designer automatisch" Tabellen- und Spaltennamen beim Erstellen von Klassen, die anhand eines Datenbankschemas. Zum Beispiel: die Tabelle "Abendessen" im vorliegenden Beispiel in einer Klasse "Dinner" geführt hat. Benennen von dieser Klasse unterstützt unsere Modelle mit .NET Benennungskonventionen konsistent zu machen und in der Regel suchen, dass der Designer Korrekturen dies einrichten einfache (insbesondere, wenn viele Tabellen hinzufügen). Wenn Sie nicht möchten, dass der Name einer Klasse oder eine Eigenschaft, die der Designer generiert jedoch, dass Sie immer außer Kraft setzen und auf den Namen eines gewünschten ändern können. Sie erreichen dies durch Bearbeiten der Entität bzw. der Eigenschaft Name inline im Designer oder durch diese über das Eigenschaftenraster zu ändern.

LINQ to SQL-Designer auch der Primär-/Fremdschlüssel-Beziehungen der Tabellen überprüft und automatisch auf ihrer Grundlage standardmäßig erstellt "Beziehung Zuordnungen" zwischen den anderen Modellklassen, die es erstellt. Angenommen, wenn wir die Abendessen gezogen und Antwort, der auf LINQ to SQL-Designer eine Zuordnung 1: n-Beziehung zwischen den beiden Tabellen wurde abgeleitet basiert sowohl, dass die Antwort-Tabelle einen Fremdschlüssel für die Tabelle Abendessen hatten (Dies wird angegeben, durch den Pfeil in der -Designer):

![](build-a-model-with-business-rule-validations/_static/image6.png)

Die oben genannten Zuordnung bewirkt, LINQ to SQL die Antwort-Klasse eine stark typisierte "Dinner"-Eigenschaft hinzu, mit denen Entwickler können auf der Dinner mit einer bestimmten Antwort verknüpft ist. Es führt auch dazu, dass die Dinner-Klasse, um eine Auflistungseigenschaft "RSVPs" verfügen, die ermöglicht es Entwicklern, abrufen und aktualisieren die Antwort-Objekte, die mit einer bestimmten Dinner zugeordnet sind.

Im folgenden sehen Sie ein Beispiel für Intellisense in Visual Studio, wenn wir ein neues Objekt für die Antwort zu erstellen und eine Dinner Einladung Auflistung hinzuzufügen. Beachten Sie, wie LINQ to SQL automatisch die entsprechenden eine Auflistung von "Einladung" für das Objekt Dinner hinzugefügt:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Durch Hinzufügen der Antwort-Objekt auf der Dinner Einladung Auflistung sagen wir LINQ to SQL eine foreign Key-Beziehung zwischen der Dinner und die Antwort-Zeile in der Datenbank zugeordnet:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Wenn Ihnen nicht gefallen wie der Designer modelliert oder mit dem Namen einer Tabelle Zuordnung verfügt, können Sie sie überschreiben. Einfach klicken Sie auf den Pfeil Zuordnung im Designer und den Zugriff auf ihre Eigenschaften über das Eigenschaftenraster, um umbenennen, löschen oder ändern Sie sie. Für unsere Anwendung NerdDinner jedoch die Standardregeln für die Zuordnung funktioniert gut für die Modell-Datenklassen, die wir erstellen und es können nur das Standardverhalten.

### <a name="nerddinnerdatacontext-class"></a>NerdDinnerDataContext-Klasse

Visual Studio erstellt automatisch .NET Klassen, die die Modelle und datenbankbeziehungen mit LINQ to SQL-Designer definiert darstellen. Für jede LINQ to SQL-Designer-Datei der Projektmappe hinzugefügt wird auch eine LINQ to SQL-DataContext-Klasse generiert. Da wir unsere LINQ to SQL-Klassenelement "NerdDinner" benannt, wird der DataContext-Klasse, die erstellt wurde "NerdDinnerDataContext" aufgerufen. Diese Klasse NerdDinnerDataContext ist die primäre Methode, mit denen, die wir mit der Datenbank interagiert wird.

Unsere NerdDinnerDataContext Klasse macht zwei Eigenschaften - "Abendessen" und "RSVPs -", die die beiden Tabellen darstellen, die wir in der Datenbank erstellt. Wir können c# verwenden zum Schreiben von LINQ-Abfragen für diese Eigenschaften in der Abfrage und rufen Sie Essen und Antwort-Objekte aus der Datenbank.

Der folgende Code veranschaulicht, wie ein NerdDinnerDataContext-Objekt instanziiert, und führen eine LINQ-Abfrage für eine Sequenz von Abendessen abrufen, die in der Zukunft liegen. Beim Schreiben von LINQ-Abfrage, stellt Visual Studio vollständige IntelliSense-Funktionalität bereit, und die von ihr zurückgegebenen Objekte sind stark typisiert und unterstützen auch Intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Außerdem uns Dinner und Antwort Objekte Abfragen, verfolgt eine NerdDinnerDataContext automatisch auch alle Änderungen, die wir anschließend an die Dinner und Antwort Objekte vorzunehmen, die wir über ihn abzurufen. Wir können diese Funktion verwenden, um leicht die Änderungen zu speichern, wieder in der Datenbank – ohne explizite SQL Update Code schreiben zu müssen.

Der folgenden Code wird beispielsweise veranschaulicht, wie eine LINQ-Abfrage verwenden, um ein einzelnes Dinner-Objekt aus der Datenbank abrufen, aktualisieren zwei Eigenschaften Dinner und speichern Sie die Änderungen wieder in der Datenbank:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

Das NerdDinnerDataContext-Objekt im Code über verfolgt automatisch die Eigenschaft vorgenommenen Dinner-Objekts, das wir daraus abgerufen. Wenn wir die Methode "SubmitChanges()" aufgerufen wird, führt es eine entsprechende SQL "UPDATE"-Anweisung, mit der Datenbank beibehalten, werden die aktualisierten Werte zurück.

### <a name="creating-a-dinnerrepository-class"></a>Erstellen einer DinnerRepository-Klasse

Bei kleineren Anwendungen ist es manchmal für die Controller funktionieren direkt mit einer LINQ to SQL-DataContext-Klasse, und Einbetten von LINQ-Abfragen in die Controller haben. Wie größere Anwendungen erhalten, kann dieser Ansatz sich schwierig zu verwalten und zu testen. Sie können auch Sie uns Ihre Meinung duplizieren die gleichen LINQ-Abfragen an mehreren Stellen führen.

Ein Ansatz besteht darin, die kann Anwendungen leichter zu verwalten und Testen ist die Verwendung von "Repositorymusters". Eine Repository-Klasse hilft dabei, Daten abzufragen und Persistenzlogik und sofort abstrahiert die Implementierungsdetails der Datenpersistenz aus der Anwendung zu kapseln. Zusätzlich zu den Anwendungscode Bereinigung vorgenommen werden, können unter Verwendung eines Repositorymusters erleichtert jedoch das Speicher-Implementierungen von Daten in der Zukunft ändern, und sie Komponententests eine Anwendung ohne einer realen Datenbank zu ermöglichen.

Für unsere NerdDinner Anwendung definieren wir eine DinnerRepository-Klasse, mit der folgenden Signatur:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Hinweis: In diesem Kapitel wir extrahieren eine Schnittstelle IDinnerRepository von dieser Klasse und Abhängigkeitsinjektion mit ihm auf unserem Controllern aktivieren. Zunächst werden aber wir zum Starten einfach und direkt mit der Klasse DinnerRepository funktioniert.*

Diese Klasse implementieren wir mit der rechten Maustaste auf unserem "Modelle" Ordner und wählen Sie die **Add -&gt;neues Element** Menübefehl. Innerhalb des Dialogfelds "Neues Element hinzufügen" Wir wählen Sie die Vorlage "Class" und nennen Sie die Datei "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

Dann können wir unsere DinnerRespository-Klasse, die mit den folgenden Code implementieren:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Abrufen, aktualisieren, einfügen und Löschen mithilfe der DinnerRepository-Klasse

Nun, da wir unsere DinnerRepository-Klasse erstellt haben, sehen wir uns einige Codebeispiele, in denen allgemeine Aufgaben veranschaulicht, die wir damit tun können:

#### <a name="querying-examples"></a>Beispiele für Abfragen

Der folgende Code Ruft eine einzelne Dinner mithilfe des Werts DinnerID ab:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Der folgende Code Ruft alle anstehenden Abendessen und Schleifen über diese ab:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>INSERT- und Update-Beispiele

Der folgende Code veranschaulicht das Hinzufügen von zwei neuen Abendessen angezeigt. Hinzufügungen/Änderungen im Repository nicht an die Datenbank übergeben, bis die Methode "Save()" aufgerufen wird. LINQ to SQL umschließt alle Änderungen automatisch in einer Datenbanktransaktion –, damit alle Änderungen der Fall sein, oder keine von ihnen ausführen, wenn unsere Repository gespeichert:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Der folgende Code Ruft ein vorhandenes Dinner-Objekt ab und zwei Eigenschaften darauf ändert. Die Änderungen sind wieder in der Datenbank ein Commit ausgeführt, wenn die Methode "Save()" für unsere Repository aufgerufen wird:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Der folgende Code Ruft eine Dinner und anschließend eine Antwort hinzugefügt. Dies geschieht mithilfe der Einladung-Auflistung auf der Dinner-Objekt, das LINQ to SQL für uns erstellt werden (da es eine Primärschlüssel-Schlüssel/Fremdschlüssel-Beziehung zwischen den beiden in der Datenbank ist). Diese Änderung ist wieder in der Datenbank als neue Antwort Tabellenzeile beibehalten, wenn die Methode "Save()" für das Repository aufgerufen wird:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Beispiel für Delete

Der folgende Code Ruft ein vorhandenes Dinner-Objekt ab und markiert es gelöscht werden soll. Beim Aufrufen der Methode "Save()" im Repository führt es wieder in der Datenbank löschen commit:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Modellklassen integrieren Überprüfung und Regel Geschäftslogik

Integration von Validierungs- und Geschäftsregel Logik ein wesentlicher Bestandteil des jeder Anwendung ist, die mit Daten arbeitet.

#### <a name="schema-validation"></a>Schema-Validierung

Wenn Modellklassen mit LINQ to SQL-Designer definiert sind, entsprechen die Datentypen der Eigenschaften in das Modell Datenklassen die Datentypen der Datenbanktabelle. Zum Beispiel: Wenn die Spalte "EventDate" in der Tabelle Abendessen "Datetime" ist, werden die Datenmodellklasse von LINQ to SQL erstellt vom Typ "DateTime" (Dies ist eine integrierte .NET-Datentyp). Dies bedeutet, dass Sie Kompilierungsfehler angezeigt werden, wenn Sie versuchen, eine ganze Zahl oder einen booleschen Wert aus dem Code zugewiesen, und es ein Fehler automatisch ausgelöst wird, wenn Sie versuchen, einen ungültige Zeichenfolgen-Datentyp, zur Laufzeit implizit zu konvertieren.

LINQ to SQL wird auch automatisch Escapezeichen SQL-Werten für Sie verarbeitet, wenn die Zeichenfolgen - verwenden Sie vor SQL Injection-Angriffen zu schützen, wenn sie verwendet.

#### <a name="validation-and-business-rule-logic"></a>Überprüfung und Geschäftslogik-Regel

Schema-Validation eignet sich als ersten Schritt, jedoch selten reicht. In den meisten realen Szenarien erfordern die Fähigkeit, umfangreichere Validierungslogik angeben, die erstrecken sich über mehrere Eigenschaften, Code ausführen und häufig die Übersicht über Wirkungsbereichs des Zustands eines Modells können (z. B.: wird es erstellt/aktualisiert/gelöscht oder in einem Zustand domänenspezifische "z. B. archiviert). Es gibt eine Vielzahl von verschiedenen Mustern und Frameworks, die zum Definieren und Anwenden von Validierungsregeln auf Modellklassen verwendet werden können, und es gibt mehrere .NET Framework-basierte Frameworks es, die verwendet werden können, um dieses Problem zu. Sie können sehr viel jeder von ihnen in ASP.NET MVC-Anwendungen verwenden.

Im Rahmen unserer Anwendung NerdDinner verwenden wir ein relativ einfach und selbsterklärend Muster, in denen verfügbar zu machen wir eine IsValid-Eigenschaft und eine GetRuleViolations()-Methode auf unserem Dinner Model-Objekts. "True" oder "false", je nachdem, ob die Überprüfung und Geschäftsregeln gültig sind, gibt die IsValid-Eigenschaft zurück. Die GetRuleViolations()-Methode wird eine Liste der Regelfehler zurückgegeben.

IsValid und GetRuleViolations() implementieren für unsere Dinner Modell wir unser Projekt eine "partielle Klasse" hinzu. Partielle Klassen können verwendet werden, um die Klassen, die von einem VS-Designer (z. B. die Dinner-Klasse, die von LINQ to SQL-Designer generierten) verwaltet Methoden/Eigenschaften/Ereignisse hinzugefügt und das Tool von der auszuführenden mit Code zu vermeiden. Wir können unsere Projekt eine neue partielle Klasse hinzugefügt, mit der rechten Maustaste auf den Ordner \Models, und wählen Sie dann den Befehl "Neues Element hinzufügen". Wir können dann die Vorlage "Class" innerhalb des Dialogfelds "Neues Element hinzufügen" auswählen und nennen Sie sie Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Klicken auf die Schaltfläche "Hinzufügen" wird eine Dinner.cs-Datei zum Projekt hinzufügen, und öffnen sie in der IDE. Wir können dann implementieren, eine grundlegende Regel Erzwingung Framework verwenden den folgenden Code:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Einige Hinweise zu den oben aufgeführten Code:

- Die Dinner-Klasse wird mit einem "partiell" Schlüsselwort – vorangestellt, d. h. der darin enthaltene Code wird mit der Klasse generiert/aufrechterhalten kombiniert werden, indem die LINQ to SQL-Designer und in eine einzelne Klasse kompiliert werden.
- Die RuleViolation-Klasse ist eine Hilfsklasse, die wir dem Projekt hinzufügen, die wir weitere Details zu einem Verstoß gegen eine Codeanalyseregel bereitstellen können.
- Die Dinner.GetRuleViolations() Methode bewirkt, dass unsere Überprüfung und Geschäftsregeln ausgewertet werden soll (implementieren wir ihnen in Kürze). Anschließend zurückgegeben wieder eine Sequenz von RuleViolation-Objekten, die weitere Details zu Regelfehlern bereitstellen.
- Die Dinner.IsValid-Eigenschaft enthält eine einfache Hilfsfunktion-Eigenschaft, die angibt, ob das Objekt Dinner alle aktiven RuleViolations verfügt. Es kann von einem Entwickler verwenden jedoch stattdessen das Dinner jederzeit proaktiv überprüft werden (und löst keine Ausnahme aus).
- Die Dinner.OnValidate() partielle Methode handelt es sich um einen Hook, den LINQ to SQL bietet, der uns ermöglicht, die benachrichtigt werden, solange das Dinner-Objekt ist in der Datenbank beibehalten werden sollen. Unsere OnValidate()"-Implementierung, die oben genannten gewährleistet, dass die Dinner keine RuleViolations aufweist, nachdem es gespeichert wurde. Ist in einem ungültigen Zustand er eine Ausnahme auslöst, wodurch LINQ to SQL die Transaktion abbricht.

Dieser Ansatz bietet ein einfache Framework, dem wir auf die überprüfungs- und Geschäftsregeln in integrieren können. Fügen Sie jetzt die folgenden Regeln, um unsere GetRuleViolations()-Methode:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Wir werden die "yield Return"-Funktion von c# verwenden, um eine Sequenz von jeder RuleViolations zurückgeben. Die ersten sechs Regel Überprüfungen, die oben genannten erzwingen einfach an, dass Zeichenfolgeneigenschaften auf unserem Dinner null oder leer sein können. Die letzte Regel ist etwas interessanter und Aufrufe PhoneValidator.IsValidNumber(), um eine Hilfsmethode, dass wir unsere Projekt, um zu überprüfen, ob die ContactPhone hinzufügen können Zahl Format entspricht der Dinner des Landes.

Wir können. NET reguläre Unterstützung zur Implementierung dieser Unterstützung der Phone-Validierung. Im folgenden finden Sie eine einfache PhoneValidator-Implementierung, die wir hinzufügen können unsere Projekt, das wir länderspezifische Regex-Muster Überprüfungen hinzufügen können:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Behandeln von Überprüfung und Business Logic Verstöße

Nun, dass wir den oben genannten Überprüfung und Business Regelcode, jederzeit wir versucht, eine zum Erstellen oder Aktualisieren einer Dinner hinzugefügt haben, werden unsere Logik Validierungsregeln ausgewertet und erzwungen werden.

Entwickler können schreiben Sie code wie unten proaktiv bestimmen Sie, ob ein Dinner Objekt gültig ist, und rufen Sie eine Liste aller Verstöße darin ohne Auslösen von Ausnahmen:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Wenn Sie versuchen, eine Dinner in einem ungültigen Zustand zu speichern, wird eine Ausnahme ausgelöst werden, wenn wir die Save()-Methode in der DinnerRepository aufrufen. Dies tritt auf, da LINQ to SQL automatisch unsere Dinner.OnValidate() partielle Methode aufruft, bevor der Dinner Änderungen gespeichert, und wir Dinner.OnValidate(), um eine Ausnahme auszulösen, wenn alle Verletzungen von Schwellenwertregeln in das Dinner vorhanden Code hinzugefügt. Diese Ausnahme abfangen Sie können und reaktiv Abrufen einer Liste Verstöße gegen die diesen Fehler beheben:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Da unsere Überprüfung und Geschäftsregeln unsere Modellebene, und nicht innerhalb der UI-Ebene implementiert werden, werden sie angewendet und in allen Szenarios innerhalb der Anwendung verwendet werden. Wir können später ändern oder Geschäftsregeln hinzuzufügen und alle Codes, funktioniert mit unserem Dinner Objekte werden berücksichtigt.

Geschäftsregeln an einem Ort ändern ist, ohne diese Änderungen in der gesamten Anwendung und Benutzeroberflächen-Logik ripple ein Anzeichen für eine gut geschriebene Anwendung und einen Vorteil, mit dem ein MVC-Framework kann durchzusetzen.

### <a name="next-step"></a>Nächster Schritt

Wir haben jetzt ein Modell, mit denen wir können Abfragen sowohl die Datenbank zu aktualisieren.

Nehmen wir jetzt einige Controller und Ansichten dem Projekt hinzugefügt, das es verwenden können, um eine HTML-UI-Erfahrung herum zu erstellen.

>[!div class="step-by-step"]
[Zurück](create-a-database.md)
[Weiter](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
