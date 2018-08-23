---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Erstellen eines Modells mit Geschäftsregelüberprüfungen | Microsoft-Dokumentation
author: microsoft
description: Schritt 3 veranschaulicht, wie ein Modell zu erstellen, können wir, um beide Abfragen verwenden und aktualisieren Sie die Datenbank für die NerdDinner-Anwendung.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: b735c414c629b9ff6617dbf80782d57543306c00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836858"
---
<a name="build-a-model-with-business-rule-validations"></a>Erstellen eines Modells mit Geschäftsregelüberprüfungen
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 3 ein kostenloses ["NerdDinner"-webanwendungstutorial](introducing-the-nerddinner-tutorial.md) , die führt – Exemplarische Vorgehensweise erstellen eine kleine, jedoch abgeschlossen haben, Web-Anwendung mithilfe von ASP.NET MVC-1.
> 
> Schritt 3 veranschaulicht, wie ein Modell zu erstellen, können wir, um beide Abfragen verwenden und aktualisieren Sie die Datenbank für die NerdDinner-Anwendung.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner, Schritt 3: Erstellen des Modells

In einem Model-View-Controller-Framework bezieht sich der Begriff "Model" auf die Objekte, die darstellen, die Daten von der Anwendung sowie die zugehörige Domänenlogik, die überprüfungs- und Geschäftsregeln er integriert ein. Das Modell ist in vielerlei Hinsicht das "Herz" einer MVC-basierten Anwendung, und später im Wesentlichen sehen steuert das Verhalten des Zertifikats.

ASP.NET MVC-Framework unterstützt die Verwendung jeder neue datenzugriffstechnologie, und Entwickler können aus einer Vielzahl von umfangreichen .NET Datenoptionen, implementieren Sie ihre Modelle, einschließlich: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM oder nur unformatierte ADO. NET-DataReader oder DataSets.

Für die NerdDinner-Anwendung werden wir LINQ to SQL verwenden, um ein einfaches Modell zu erstellen, das unsere Datenbankentwurf ziemlich genau entspricht, und fügt einige benutzerdefinierte Validierungsregeln Datenlogik und Geschäftsregeln. Wir klicken Sie dann Implementieren einer Repository-Klasse, die abstrakte sofort hilft Implementierung des Persistenz vom Rest der Anwendung und ermöglicht es uns ganz einfach Einheit testen.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL ist ein ORM (Object relational Mapper), die als Teil von .NET 3.5 enthalten ist.

LINQ to SQL bietet eine einfache Möglichkeit, die Datenbanktabellen .NET Klassen zugeordnet werden, die wir für code können. Für die NerdDinner-Anwendung wird es verwendet, um die Dinner und RSVP-Tabellen in unserer Datenbank Dinner und RSVP-Klassen zuordnen. Die Spalten der Dinner und RSVP Tabellen entsprechen den Eigenschaften für die Dinner und RSVP-Klassen. Jedes Objekt Dinner und RSVP stellt eine separate Zeile innerhalb der Dinner oder RSVP Tabellen in der Datenbank dar.

LINQ to SQL ermöglicht uns, um zu vermeiden, dass SQL-Anweisungen zum Abrufen und Aktualisieren von Dinner und RSVP manuell zu erstellen Objekte mit Datenbankdaten. Stattdessen werden wir die Dinner und RSVP-Klassen, deren Zuordnung/aus der Datenbank und die Beziehungen zwischen ihnen definieren. LINQ to SQL wird dann übernimmt die Betreuung der Generieren der entsprechenden SQL-Ausführungslogik, die zur Laufzeit verwendet werden soll, wenn wir interagieren und diese verwenden.

Wir können die LINQ-sprachunterstützung in VB und c# verwenden, um ausdrucksvolle Abfragen schreiben, die Dinner und RSVP abrufen Objekte aus der Datenbank. Dies minimiert die Menge an Datencode zu schreiben, wir müssen uns ermöglicht, wirklich fehlerfrei Anwendungen zu erstellen.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Hinzufügen von LINQ to SQL-Klassen zu Ihrem Projekt

Wir beginnen, indem Sie mit der rechten Maustaste auf den Ordner "Models" in das Projekt, und wählen die **Add-&gt;neues Element** Menübefehl:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Hierdurch wird das Dialogfeld "Neues Element hinzufügen" angezeigt. Wir filtern, indem Sie die Kategorie "Data" und wählen Sie die Vorlage "LINQ to SQL-Klassen" darin:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Wir nennen Sie das Element "NerdDinner" und klicken Sie auf die Schaltfläche "Hinzufügen". Visual Studio fügen Sie eine Datei NerdDinner.dbml unsere \Models-Verzeichnis, und öffnen Sie dann auf die LINQ to SQL Objektrelationaler Designer:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Erstellen von Datenmodellklassen mit LINQ to SQL

LINQ to SQL ermöglicht Data Model-Klassen aus vorhandenen Datenbankschema schnell zu erstellen. To-do dies wir öffnen Sie die NerdDinner-Datenbank im Server-Explorer, und wählen Sie die Tabellen wir darin modellieren möchten:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Wir können dann die Tabellen in der LINQ auf SQL-Designeroberfläche ziehen. Wenn der wir diese LINQ to SQL erstellt automatisch Dinner und RSVP-Klassen, die mit dem Schema der Tabellen (mit der Eigenschaften der Klasse, die den Spalten der Datenbanktabelle zugeordnet):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Standardmäßig ist die LINQ to SQL "in plural-Designer automatisch" Tabellen- und Spaltennamen beim Erstellen von Klassen auf Grundlage eines Datenbankschemas. Zum Beispiel: in der Tabelle "Dinner" im obigen Beispiel hat eine "Dinner"-Klasse. Benennen von dieser Klasse kann die Modelle mit .NET Benennungskonventionen konsistent zu machen und in der Regel finde, dass durch den Designer Fix dies sich praktisch (insbesondere, wenn viele Tabellen hinzufügen). Wenn Ihnen nicht gefällt, dass der Name einer Klasse oder Eigenschaft, die der Designer generiert jedoch, Sie können immer überschreiben, und ändern Sie ihn in einen beliebigen Namen gewünschten. Sie können dies entweder durch Bearbeiten der Entität bzw. der Eigenschaft Name inline innerhalb des Designers oder ändern ihn über das Eigenschaftenraster vornehmen.

Wird standardmäßig die LINQ to SQL-Designer auch der Primär-/Fremdschlüssel-Beziehungen der Tabellen überprüft und automatisch basierend darauf erstellt "Beziehung sicherheitszuordnungen" zwischen den verschiedenen Modellklassen, die sie erstellt. Beispielsweise, wenn wir die Dinner gezogen und RSVP, der auf die LINQ to SQL-Designer eine Zuordnung 1: n Beziehung zwischen den beiden Tabellen wurde abgeleitet basiert auf der Tatsache, dass die RSVP-Tabelle einen Fremdschlüssel für die Dinner-Tabelle haben (Dies wird angegeben, durch den Pfeil in der -Designer):

![](build-a-model-with-business-rule-validations/_static/image6.png)

Die oben genannten Zuordnung führt dazu, dass LINQ to SQL eine stark typisierte "Dinner"-Eigenschaft der RSVP-Klasse hinzufügen, mit denen Entwickler auf der Dinner einer bestimmten RSVP zugeordnet. Es wird auch dazu führen, dass die Dinner-Klasse, um eine Auflistungseigenschaft "RSVPs" zu erhalten, mit der Entwickler zum Abrufen und Aktualisieren von RSVP-Objekten, die ein bestimmtes Essen zugeordnet.

Im folgenden sehen Sie ein Beispiel für Intellisense in Visual Studio beim Erstellen wir ein neues RSVP-Objekt und einem Dinner Bestätigungen Auflistung hinzuzufügen. Beachten Sie, wie LINQ to SQL automatisch eine Auflistung von "Bestätigungen" das Dinner-Objekt hinzugefügt wird:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Durch Hinzufügen des RSVP-Objekts auf der Dinner Bestätigungen Auflistung sagen wir LINQ to SQL, um eine fremdschlüsselbeziehung zwischen der Dinner und der RSVP-Zeile in der Datenbank zuzuordnen:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Wenn Ihnen nicht gefallen wie der Designer modelliert oder mit dem Namen einer Tabelle Zuordnung verfügt, können Sie sie überschreiben. Nur klicken Sie auf den Pfeil "Zuordnung" im Designer und den Zugriff auf seine Eigenschaften über das Eigenschaftenraster umbenennen, löschen oder ändern Sie sie. Für die NerdDinner-Anwendung jedoch die Standardregeln für die Zuordnung eignen sich gut für die Data-Model-Klassen, die wir erstellen und verwenden wir nur das Standardverhalten.

### <a name="nerddinnerdatacontext-class"></a>NerdDinnerDataContext-Klasse

Visual Studio erstellt automatisch .NET Klassen, die die Modelle und datenbankbeziehungen mit LINQ to SQL-Designer definiert darstellen. Für jede LINQ to SQL-Designer-Datei der Projektmappe hinzugefügt wird auch eine LINQ to SQL-DataContext-Klasse generiert. Da wir unsere LINQ to SQL-Klassenelements "NerdDinner" benannt haben, wird die DataContext-Klasse erstellt "NerdDinnerDataContext" aufgerufen. Diese Klasse NerdDinnerDataContext ist der einfachste Weg, die wir mit der Datenbank interagiert.

Unsere NerdDinnerDataContext-Klasse macht zwei Eigenschaften: "Dinner" und "RSVPs -" mit den beiden Tabellen, die wir in der Datenbank erstellt. Wir können c# Sie um LINQ-Abfragen für diese Eigenschaften in der Abfrage und Abrufen von Dinner und RSVP Objekten aus der Datenbank zu schreiben.

Der folgende Code veranschaulicht, wie ein NerdDinnerDataContext-Objekt instanziiert, und führen Sie eine LINQ-Abfrage für sie um eine Sequenz von Dinner zu erhalten, die in der Zukunft liegen. Visual Studio bietet vollständiges Intellisense, wenn die LINQ-Abfrage schreiben, und die von ihm zurückgegebenen Objekte sind stark typisiert und unterstützt auch Intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Zusätzlich zu der es uns ermöglicht, Abfragen für Dinner und RSVP-Objekte, verfolgt einen NerdDinnerDataContext automatisch auch alle Änderungen, die wir anschließend an die Dinner und RSVP-Objekte, die wir über sie abrufen. Wir können diese Funktion verwenden, um ganz einfach die Änderungen zu speichern, in der Datenbank – ohne explizite SQL Update Code schreiben zu müssen.

Der folgende Code zeigt beispielsweise, wie mit einer LINQ-Abfrage ein einzelnes Dinner-Objekt aus der Datenbank abrufen, aktualisieren zwei Eigenschaften Dinner und speichern Sie die Änderungen in der Datenbank:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

Das NerdDinnerDataContext-Objekt im Code über verfolgt automatisch die an die Dinner-Objekt, das wir von ihm abgerufenen vorgenommenen eigenschaftenänderungen. Wenn wir die Methode "SubmitChanges()" aufgerufen wird, führt es eine entsprechende SQL "UPDATE"-Anweisung, mit der Datenbank beibehalten, werden die aktualisierten Werte zurück.

### <a name="creating-a-dinnerrepository-class"></a>Erstellen einer "dinnerrepository"-Klasse

Für kleine Anwendungen ist es mitunter zu Controller funktioniert direkt mit einer LINQ to SQL-DataContext-Klasse, und betten Sie LINQ-Abfragen in den Controllern haben. Wenn Anwendungen größer werden, wird dieser Ansatz jedoch mühsam, verwalten und testen. Sie können auch an die gleiche LINQ-Abfragen an mehreren Orten duplizieren führen.

Ein Ansatz, mit denen kann Anwendungen einfacher zu verwalten und Testen ist ein Muster "Repository" verwendet. Eine repositoryklasse kann Abfragen von Daten und Logik der Datenpersistenz und abstrahiert die Implementierungsdetails der Persistenz von Daten aus der Anwendung kapseln. Zusätzlich zum Anwendungscode sauberer ist mit einem Repositorymuster können sie leichter datenspeicherimplementierungen Daten in der Zukunft ändern, und es kann eine Anwendung ohne eine echte Datenbank Komponententests erleichtern.

Für die NerdDinner-Anwendung definieren wir eine DinnerRepository-Klasse, mit der folgenden Signatur:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Hinweis: In diesem Kapitel wir eine "idinnerrepository"-Schnittstelle von dieser Klasse extrahieren und Abhängigkeitsinjektion mit ihm auf unsere-Controller zu aktivieren. Zunächst werden jedoch Wir fangen Sie einfach und direkt mit der Klasse "dinnerrepository" funktioniert.*

Diese Klasse implementieren wir mit der rechten Maustaste auf den Ordner "Models" und wählen Sie die **Add-&gt;neues Element** Menübefehl. Das Dialogfeld "Neues Element hinzufügen" in "Wir wählen Sie die Vorlage"Class"und nennen Sie die Datei"DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

Wir können unsere DinnerRespository-Klasse, die anhand des folgenden Codes implementieren:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Abrufen, aktualisieren, einfügen und Löschen mithilfe der Klasse "dinnerrepository"

Nun, wir unsere Klasse "dinnerrepository" erstellt haben, sehen wir uns einige Codebeispiele, in denen häufige Aufgaben veranschaulicht, die wir damit tun können:

#### <a name="querying-examples"></a>Beispiele für Abfragen

Der folgende Code Ruft die einem einzelnen Dinner unter Verwendung des Werts DinnerID ab:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Der folgende Code Ruft alle anstehenden Dinner und Schleifen über diese ab:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>INSERT- und Update-Beispiele

Der folgende Code veranschaulicht das Hinzufügen von zwei neuen Dinner. Ergänzungen/Änderungen im Repository nicht mit der Datenbank ein Commit ausgeführt, bis auf die Methode "Save()""aufgerufen wird. LINQ to SQL wird automatisch alle Änderungen in einer Datenbanktransaktion – umgebrochen, damit alle Änderungen auftreten oder keine von ihnen tun, wenn es sich bei unserem Repository speichert:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Der folgende Code Ruft ein vorhandenes Dinner-Objekt und zwei Eigenschaften ändert. Die Änderungen sind in der Datenbank ein Commit ausgeführt, wenn die Methode "Save()""für unsere Repositorys aufgerufen wird:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Der folgende Code Ruft die einem Dinner ab, und klicken Sie dann eine Antwort hinzugefügt. Dies geschieht mithilfe der Bestätigungen-Auflistung auf der Dinner-Objekt, das LINQ to SQL für uns erstellt wird (da es eine Primärschlüssel-Schlüssel/Fremdschlüssel-Beziehung zwischen den beiden in der Datenbank ist). Diese Änderung wird an die Datenbank als neue Zeile RSVP Tabelle beibehalten, wenn die Methode "Save()""für das Repository aufgerufen wird:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>DELETE-Beispiel

Der folgende Code Ruft ein vorhandenes Dinner-Objekt ab, und klicken Sie dann markiert es gelöscht werden soll. Wenn die Methode "Save()"", auf das Repository aufgerufen wird wird es den Löschvorgang in der Datenbank committen:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integrieren von Überprüfung und Geschäftslogik für die Regel ViewModel-Klassen

Die Integration von Validierungs- und Business-Regel, dass die Logik ein wichtiger Bestandteil einer beliebigen Anwendung ist, die mit Daten arbeitet.

#### <a name="schema-validation"></a>Schema-Validierung

Wenn Modellklassen mit LINQ to SQL-Designer definiert sind, entsprechen die Datentypen der Eigenschaften in den datenmodellklassen die Datentypen der Datenbanktabelle. Zum Beispiel: Wenn die Spalte "EventDate" in der Tabelle "Dinner" ein "Datetime" ist, werden die Data Model-Klasse, die von LINQ to SQL erstellt vom Typ "DateTime" (die einen integrierten .NET-Datentyp). Dies bedeutet, dass Sie Kompilierungsfehler angezeigt werden, wenn Sie versuchen, eine ganze Zahl oder einen booleschen Wert aus Code zuzuweisen, und es Fehler automatisch ausgelöst werden, wenn Sie versuchen, einen ungültigen Zeichenfolgentyp implizit, zur Laufzeit zu konvertieren.

LINQ to SQL wird auch automatisch übernimmt Escapezeichen SQL-Werten für Sie nach der Verwendung von Zeichenfolgen – um Sie vor SQL Injection-Angriffen zu schützen, wenn es verwendet.

#### <a name="validation-and-business-rule-logic"></a>Überprüfung und Geschäftslogik für die Regel

Schema-Validation eignet sich als ersten Schritt, aber nur selten reicht. Die meisten realen Szenarien erfordern die Fähigkeit, umfangreichere Validierungslogik angeben, die umfassen mehrere Eigenschaften, führen Sie Code und haben häufig Informationen zu einem Modellzustand können (z. B.: wird es erstellt/aktualisiert/gelöscht oder in einem Zustand mit Domänenspezifischer wie "archiviert"). Es gibt eine Vielzahl von verschiedenen Mustern und Frameworks, die zum Definieren und Anwenden von Validierungsregeln auf ViewModel-Klassen verwendet werden können, und es gibt mehrere .NET Framework-basierte Frameworks, die verwendet werden können, um dies zu unterstützen. Sie können praktisch alle der in ASP.NET MVC-Anwendungen verwenden.

Im Rahmen unserer NerdDinner-Anwendung verwenden wir ein Muster für relativ einfache und unkomplizierte, in denen verfügbar zu machen wir eine "IsValid"-Eigenschaft und eine GetRuleViolations()-Methode für das Dinner-Modell-Objekt. Die Eigenschaft "IsValid" gibt "true" oder "false", je nachdem, ob die Überprüfung und Geschäftsregeln gültig sind. Die GetRuleViolations()-Methode gibt eine Liste der Regelfehler zurück.

Wir werden "IsValid" und GetRuleViolations() für unser Modell Dinner implementieren, indem Sie eine "partielle Klasse" Unser Projekt hinzufügen. Partielle Klassen können verwendet werden, um das Hinzufügen von Methoden/Eigenschaften/Ereignisse für Klassen, die von einem Visual Studio-Designer (z. B. die Dinner-Klasse, die von der LINQ to SQL-Designer generierten) verwaltet und das Tool aus, Sie müssen sich mit unserem Code vermeiden. Wir können unser Projekt mit der rechten Maustaste auf den Ordner "\Models" eine neue partielle Klasse hinzugefügt, und wählen Sie dann auf den Menübefehl "Neues Element hinzufügen". Wir können dann wählen Sie die Vorlage "Class" im Dialogfeld "Neues Element hinzufügen" und nennen Sie sie Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Klicken Sie auf die Schaltfläche "Hinzufügen" wird eine Dinner.cs-Datei Ihrem Projekt hinzufügen, und öffnen sie in der IDE. Wir können dann implementieren, eine grundlegende Regel Erzwingung Framework verwendet den folgenden Code:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Einige Hinweise zu den obigen Code:

- Die Dinner-Klasse ist mit einem "partiell" Schlüsselwort – vorangestellt, was bedeutet der darin enthaltene Code kombiniert mit der Klasse generiert/verwaltet vom LINQ to SQL-Designer und in einer einzelnen Klasse kompiliert.
- Die RuleViolation-Klasse ist eine Hilfsklasse, die wir dem Projekt hinzufügen, die wir weitere Details zu einem Verstoß gegen bereitstellen können.
- Die Dinner.GetRuleViolations()-Methode führt dazu, dass unsere Validierung und Geschäftsregeln ausgewertet werden soll (wir implementieren sie in Kürze). Wird dann wieder eine Sequenz von RuleViolation-Objekten, die weitere Details zu Regelfehlern bereitstellen.
- Die Dinner.IsValid-Eigenschaft enthält eine praktische Hilfsfunktion-Eigenschaft, die angibt, ob die Dinner-Objekt alle aktiven RuleViolations verfügt. Sie können proaktiv von einem Entwickler, die unter Verwendung des Dinner-Objekts zu einem beliebigen Zeitpunkt überprüft werden (und löst keine Ausnahme).
- Die Dinner.OnValidate() partielle Methode ist ein Hook, den LINQ to SQL bietet, mit der wir um benachrichtigt zu werden jedes Mal, wenn das Dinner-Objekt wird in der Datenbank persistent gespeichert werden. Unsere OnValidate()"-Implementierung, die oben genannten wird sichergestellt, dass die Dinner keine RuleViolations verfügt, bevor es gespeichert wird. Wenn es in einem ungültigen Zustand ist es eine Ausnahme auslöst, dadurch LINQ to SQL die Transaktion abgebrochen wird.

Dieser Ansatz bietet ein einfaches Framework, dem wir die überprüfungs- und Geschäftsregeln in integrieren können. Fügen Sie jetzt die folgenden Regeln, um unsere GetRuleViolations()-Methode:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Wir verwenden die "yield Return"-Funktion von C#-, um eine Sequenz von jedem RuleViolations zurückgibt. Die ersten sechs regelüberprüfungen, die oben genannten erzwingen einfach, dass die Eigenschaften auf unsere Dinner null oder leer sein darf nicht. Die letzte Regel ist ein wenig interessanter und Aufrufe PhoneValidator.IsValidNumber(), um eine Hilfsmethode, dass wir unser Projekt zu überprüfen, ob die ContactPhone hinzufügen können Zahl Format entspricht der Dinner Land.

Wir können. NET Unterstützung von regulären Ausdrücken zur Implementierung dieser Unterstützung der Phone-Überprüfung. Im folgenden finden Sie eine einfache PhoneValidator-Implementierung, die wir hinzufügen können auf das Projekt, das wir länderspezifische Regex-Muster Überprüfungen hinzufügen kann:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Behandeln von Überprüfung und Business Logic Verstöße

Nun, wir obigen Validierung und Business-Regelcode jederzeit, die wir versuchen hinzugefügt haben, erstellen oder zu eine Dinner zu aktualisieren, werden unsere Logik Validierungsregeln ausgewertet und erzwungen werden soll.

Entwickler können code wie unten proaktiv zu bestimmen, ob es sich bei einem Dinner-Objekt gültig ist, und rufen Sie eine Liste alle Verstöße gegen die darin ohne Auslösen von Ausnahmen schreiben:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Wenn wir versuchen, eine Dinner in einem ungültigen Zustand zu speichern, wird eine Ausnahme ausgelöst werden, wenn wir die Save() auf die "dinnerrepository" aufrufen. Dies tritt auf, da LINQ to SQL wird automatisch unsere Dinner.OnValidate() partielle Methode aufruft, bevor er die Dinner Änderungen speichert, und wir Code, um Dinner.OnValidate() zum Auslösen einer Ausnahme hinzugefügt, wenn Verletzungen der Schwellenwertregeln in das Dinner vorhanden sind. Wir können diese Ausnahme abfangen und reaktiv Abrufen einer Liste Verstöße gegen die Fehler beheben:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Da unsere Validierung und Geschäftsregeln unsere Modellschicht, und nicht innerhalb der UI-Ebene implementiert werden, werden sie angewendet und in allen Szenarien innerhalb der Anwendung verwendet werden. Wir können später ändern oder Hinzufügen von Geschäftsregeln und gesamten Code, arbeitet mit unseren Dinner-Objekten, die sie berücksichtigen.

Die Flexibilität, Geschäftsregeln an einem Ort ändern ist, ohne diese Änderungen in der Anwendung und die UI-Logik ripple ein Anzeichen für eine gut geschriebene Anwendung und ein Vorteil, den ein MVC-Framework unterstützt empfehlen.

### <a name="next-step"></a>Nächster Schritt

Wir haben jetzt ein Modell, mit denen wir sowohl Abfragen und Aktualisieren der Datenbank.

Lassen Sie uns nun einige Controller und Ansichten zum Projekt hinzufügen, das wir verwenden können, um eine HTML-UI-Erfahrung, um es herum zu erstellen.

> [!div class="step-by-step"]
> [Zurück](create-a-database.md)
> [Weiter](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
