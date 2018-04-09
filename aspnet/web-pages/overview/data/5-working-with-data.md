---
uid: web-pages/overview/data/5-working-with-data
title: Einführung in die Arbeit mit einer Datenbank in der ASP.NET Web Pages (Razor) Standorte | Microsoft Docs
author: tfitzmac
description: In diesem Kapitel wird beschrieben, wie auf Daten aus einer Datenbank zugreifen, und zeigen Sie es mithilfe von ASP.NET Web Pages wird.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 563074cf3e60717c2e6c336a2c282b4203f73b8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Einführung in die Arbeit mit einer Datenbank in der ASP.NET Web Pages (Razor)-Websites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird beschrieben, wie Microsoft WebMatrix-Tools verwenden, um eine Datenbank in einer ASP.NET Web Pages (Razor)-Website zu erstellen und zum Erstellen von Seiten, mit denen Sie die anzeigen, hinzufügen, bearbeiten und Löschen von Daten.
> 
> **Lernen Sie:** 
> 
> - Vorgehensweise: erstellen eine Datenbank.
> - Herstellen der Verbindung mit einer Datenbank.
> - Vorgehensweise beim Anzeigen von Daten in einer Webseite.
> - Zum Einfügen, aktualisieren und Löschen von Datenbank-Datensätze.
> 
> Dies sind die Funktionen, die im Artikel:
> 
> - Arbeiten mit einer Microsoft SQL Server Compact Edition-Datenbank.
> - Arbeiten mit SQL-Abfragen.
> - Der `Database`-Klasse.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Dieses Lernprogramm funktioniert auch mit WebMatrix-3. Sie können ASP.NET Web Pages 3 und Visual Studio 2013 (oder Visual Studio Express 2013 für Web); die Benutzeroberfläche wird jedoch abweichen.


## <a name="introduction-to-databases"></a>Einführung in Datenbanken

Angenommen Sie, ein typisches Adressbuch. Für jeden Eintrag im Adressbuch (d. h. für jede Person) stehen Ihnen verschiedene Angaben wie z. B. Vorname, Nachname, Adresse, e-Mail-Adresse und Telefonnummer.

Eine typische Möglichkeit zum Bilddaten wie folgt ist als eine Tabelle mit Zeilen und Spalten. In der Datenbanksprache wird jede Zeile häufig als Datensatz bezeichnet. Jede Spalte (auch als Felder bezeichnet) enthält einen Wert für jeden Typ von Daten: Vorname, letzte Name und So weiter.

| **ID** | **FirstName** | **LastName** | **Adresse** | **E-Mail** | **Telefonnummer** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Oliver | Adams | 1234 Main St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Für die meisten-Datenbanktabellen enthalten sind muss die Tabelle eine Spalte aufweisen, die einen eindeutigen Bezeichner, z. B. eine Kundennummer, Kontonummer usw. enthält. Dies bezeichnet man der Tabelle *Primärschlüssel*, und Sie verwenden, um jede Zeile in der Tabelle zu identifizieren. Im Beispiel ist die ID-Spalte der Primärschlüssel für das Adressbuch.

Mit diesem Grundkenntnisse der Datenbanken können Sie Informationen zum Erstellen einer einfachen Datenbank und Vorgänge wie das Hinzufügen, ändern und Löschen von Daten.

> [!TIP] 
> 
> **Relationale Datenbanken**
> 
> Sie können Daten in vielen Möglichkeiten, z. B. Textdateien und Tabellen speichern. Für die meisten Unternehmen verwendet werden, Daten in einer relationalen Datenbank gespeichert.
> 
> In diesem Artikel wechseln nicht sehr tief in Datenbanken. Allerdings kann es ein wenig über diese verstehen nützlich. In einer relationalen Datenbank ist die Informationen in separaten Tabellen logisch unterteilt. Beispielsweise kann eine Datenbank für eine Schule separate Tabellen für Studenten und für Angebote Klasse enthalten. Die Datenbank-Software (z. B. SQL Server) unterstützt leistungsstarke Befehle, mit denen Sie dynamisch bilden die Beziehungen zwischen Tabellen. Beispielsweise können Sie die relationale Datenbank, um eine logische Beziehung zwischen Klassen und Studenten herzustellen, um einen Zeitplan zu erstellen. Speichern von Daten in separaten Tabellen reduziert die Komplexität der Tabellenstruktur und verringert den Bedarf an redundante Daten in Tabellen zu halten.


## <a name="creating-a-database"></a>Erstellen einer Datenbank

Dieses Verfahren veranschaulicht das Erstellen einer Datenbank mit dem Namen SmallBakery mithilfe von SQL Server Compact-Datenbank-Entwurfstools, die in WebMatrix enthalten ist. Obwohl Sie eine Datenbank mithilfe von Code erstellen können, ist es häufiger vor, um die Datenbank und die Datenbanktabellen, die mit einem Entwurfstool wie WebMatrix erstellen.

1. Starten Sie WebMatrix, und klicken Sie auf der Seite "Schnellstart" auf **Websitevorlage aus**.
2. Wählen Sie **leere Website**, und klicken Sie in der **Standortname** Feld Geben Sie "SmallBakery", und klicken Sie dann auf **OK**. Die Website wird erstellt und in WebMatrix angezeigt.
3. Klicken Sie im linken Bereich auf die **Datenbanken** Arbeitsbereich.
4. Klicken Sie im Menüband auf **neue Datenbank**. Mit dem gleichen Namen wie Ihr Standort wird eine leere Datenbank erstellt.
5. Erweitern Sie im linken Bereich der **SmallBakery.sdf** Knoten, und klicken Sie dann auf **Tabellen**.
6. Klicken Sie im Menüband auf **neue Tabelle**. WebMatrix wird den Tabellen-Designer geöffnet.

    ![[Image]](5-working-with-data/_static/image1.jpg)
7. Klicken Sie in der **Namen** Spalte, und geben Sie &quot;Id&quot;.
8. In der **Datentyp** Spalte **Int**.
9. Festlegen der **Primärschlüssel ist?** und **ist identifizieren?** Optionen **Ja**.

    Wie der Name bereits vermuten lässt, **Primärschlüssel ist** weist der Datenbank, dafür Primärschlüssel der Tabelle. **Ist Identity** weist die Datenbank aus, um automatisch eine ID-Nummer für jeden neuen Eintrag zu erstellen und sie die nächste laufende Nummer (beginnend mit 1) zugewiesen.
10. Klicken Sie auf die nächste Zeile. Der Editor startet die Definition einer neuen Spalte.
11. Geben Sie für den Namenswert &quot;Namen&quot;.
12. Für **Datentyp**, wählen Sie &quot;Nvarchar&quot; und die Länge auf 50 festgelegt. Die *Var* Teil `nvarchar` weist der Datenbank, dass die Daten für diese Spalte eine Zeichenfolge, deren Größe von Datensatz zu Datensatz variieren. (Die *n* Präfix stellt *national*, gibt an, dass das Feld Zeichendaten enthalten kann, die alle Alphabet darstellt oder ein Schreibsystem &#8212; , also, dass das Feld Unicode-Daten enthält.)
13. Legen Sie die **NULL-Werte zulassen** option **keine**. Dadurch wird erzwungen, die die *Namen* Spalte ist nicht leer.
14. Erstellen Sie mit der gleiche Vorgang, eine Spalte mit dem Namen *Beschreibung*. Legen Sie **Datentyp** "Nvarchar" und 50 für die Länge und **NULL-Werte zulassen** auf "false".
15. Erstellen Sie eine Spalte mit dem Namen *Preis*. Legen Sie **-Datentyp "Money"** und **NULL-Werte zulassen** auf "false".
16. Benennen Sie im Feld oben auf die Tabelle &quot;Produkt&quot;.

    Wenn Sie fertig sind, wird die Definition sieht folgendermaßen aus:

    ![[Image]](5-working-with-data/_static/image2.jpg)
17. Drücken Sie STRG + S, um die Tabelle zu speichern.

## <a name="adding-data-to-the-database"></a>Hinzufügen von Daten in der Datenbank

Jetzt können Sie Beispieldaten in der Datenbank hinzufügen, die Sie später in diesem Artikel verwenden müssen.

1. Erweitern Sie im linken Bereich der **SmallBakery.sdf** Knoten, und klicken Sie dann auf **Tabellen**.
2. Mit der rechten Maustaste in der Product-Tabelle, und klicken Sie dann auf **Daten**.
3. Klicken Sie im Bereich "Bearbeiten" Geben Sie die folgenden Datensätze:

    | **Name** | **Beschreibung** | **Price** |
    | --- | --- | --- |
    | Brot | Integrierte frisch täglich. | 2.99 |
    | Erdbeere Shortcake | In unserem Gartens mit organischem Erdbeeren erstellt wurde. | 9.99 |
    | Apple-Kreisdiagramm | Die zweite nur für die Mom-Kreisdiagramm. | 12.99 |
    | Pecan Kreis | Wenn Sie Pecans möchten, ist dies für Sie. | 10.99 |
    | Zitronen-Kreisdiagramm | Mit der besten Zitronen in der ganzen Welt vorgenommen. | 11.99 |
    | Aber | Ihre Kinder und in Ihren "Kid" werden diese begeistert sein. | 7.99 |

    Denken Sie daran, dass Sie keine Eingabe für die *Id* Spalte. Beim Erstellen der *Id* Spalte festlegen seiner **ist Identity** Eigenschaft auf "true", wodurch sie automatisch ausgefüllt werden.

    Wenn Sie die Daten eingegeben haben, wird der Tabellen-Designer wie folgt aussehen:

    ![[Image]](5-working-with-data/_static/image3.jpg)
4. Schließen Sie die Registerkarte, die Daten in der Datenbank enthält.

## <a name="displaying-data-from-a-database"></a>Anzeigen von Daten aus einer Datenbank

Nachdem Sie eine Datenbank mit Daten darin ist, können Sie die Daten in einer ASP.NET Web-Seite anzeigen. Um die Zeilen der Tabelle zum Anzeigen auszuwählen, verwenden Sie eine SQL-Anweisung, um einen Befehl handelt, den Sie an die Datenbank übergeben.

1. Klicken Sie im linken Bereich auf die **Dateien** Arbeitsbereich.
2. Erstellen Sie im Stammverzeichnis der Website, eine neue CSHTML-Seite mit dem Namen *ListProducts.cshtml*.
3. Ersetzen Sie das vorhandene Markup durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    Öffnen Sie in der ersten Codeblock, der *SmallBakery.sdf* Datei (Datenbank), die Sie zuvor erstellt haben. Die `Database.Open` Methode setzt voraus, dass die *.sdf* Datei befindet sich in Ihrer Website *App\_Daten* Ordner. (Beachten Sie, die Sie benötigen, geben Sie die *.sdf* Erweiterung &#8212; tatsächlich, wenn Sie dies tun, die `Open` -Methode funktioniert nicht,.)

    > [!NOTE]
    > Die *App\_Daten* ist ein spezieller Ordner in ASP.NET, die zum Speichern der Datendateien verwendet wird. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einer Datenbank](#SB_ConnectingToADatabase) weiter unten in diesem Artikel.

    Stellen Sie eine Anforderung zum Abfragen der Datenbank mit der folgenden SQL `Select` Anweisung:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    In der Anweisung `Product` identifiziert die Tabelle, Abfrage. Die `*` Zeichen gibt an, dass die Abfrage alle Spalten aus der Tabelle zurückgeben soll. (Sie können auch Spalten einzeln auflisten durch Kommas getrennt werden, wenn Sie nur einige der Spalten finden Sie unter.) Die `Order By` -Klausel zeigt an, wie die Daten sortiert werden sollen &#8212; in diesem Fall durch die *Namen* Spalte. Dies bedeutet, dass die Daten in alphabetischer Reihenfolge basierend auf den Wert der sortiert werden die *Namen* Spalte für jede Zeile.

    Im Text der Seite erstellt das Markup eine HTML-Tabelle, die zum Anzeigen der Daten verwendet werden. Innerhalb der `<tbody>` Element, Sie verwenden eine `foreach` Schleife einzeln auf jede Datenzeile abgerufen, die von der Abfrage zurückgegeben wird. Für jede Datenzeile, die Sie Erstellen einer HTML-Tabellenzeile (`<tr>` Element). Anschließend Sie HTML-Tabellenzellen erstellen (`<td>` Elemente) für jede Spalte. Bei jedem der Schleife durchlaufen, die nächste verfügbare Zeile aus der Datenbank ist der `row` Variable (diese Einstellung wird der `foreach` Anweisung). Um eine einzelne Spalte aus der Zeile zu erhalten, können Sie `row.Name` oder `row.Description` oder einen beliebigen den Namen der Spalte ist.
4. Führen Sie die Seite in einem Browser aus. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Die Seite zeigt eine Liste wie folgt:

    ![[Image]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL ist eine Programmiersprache, die in den meisten relationalen Datenbanken verwendet wird, um Daten in einer Datenbank zu verwalten. Es enthält Befehle, mit deren Hilfe Sie die Daten abzurufen und zu aktualisieren, und, mit deren Hilfe Sie erstellen, ändern und Verwalten von Datenbanktabellen. SQL unterscheidet sich von einer Programmiersprache Ihrer Wahl (z. B. die gerade von Ihnen in WebMatrix verwendeten), da mit "SQL" oder die Idee dabei ist, dass Sie die Datenbank feststellen, was soll, und es Aufgabe der Datenbank ist, um zu erfahren, wie die Daten abgerufen und die Ausführung der Aufgabe. Nachfolgend sind Beispiele für einige SQL-Befehle und was sie tun:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Dies ruft die *Id*, *Namen*, und *Preis* Spalten aus Datensätzen in der *Produkt* Tabelle, wenn den Wert der *Preis* mehr als 10 ist, und die Ergebnisse zurückgibt, in alphabetischer Reihenfolge basierend auf den Werten der *Namen* Spalte. Dieser Befehl gibt ein Resultset zurück, die die Datensätze, die die Kriterien oder ein leeres Resultset zu erfüllen enthält, wenn keine Datensätze entsprechen.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Fügt einen neuen Datensatz in die *Produkt* Tabelle Festlegen der *Namen* Spalte &quot;Croissant&quot;, *Beschreibung* Spalte &quot; Eine arbeitende erfreuen&quot;, und der Preis 1.99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Dieser Befehl löscht Einträge in der *Produkt* Tabelle, deren Ablauf des Date-Spalte vor dem 1. Januar 2008 liegt. (Dies setzt voraus, dass die *Produkt* Tabelle besitzt eine Spalte, natürlich.) Das Datum wird im Format MM/TT/JJJJ hier eingegeben, aber es eingegeben werden in das Format, das für Ihr Gebietsschema verwendet wird.
> 
> Die `Insert Into` und `Delete` Befehle keine Resultsets zurückgeben. Stattdessen geben sie eine Zahl, die Sie informiert, wie viele Datensätze mit dem Befehl betroffen sind zurück.
> 
> Für einige dieser Vorgänge (wie einfügen und Löschen von Datensätzen) muss der Prozess, der den Vorgang anfordert, wird in der Datenbank berechtigt. Dies ist deshalb für Produktionsdatenbanken häufig stehen Ihnen zum Angeben eines Benutzernamens und Kennworts beim Herstellen einer Verbindung mit der Datenbank.
> 
> Es gibt Dutzende von SQL-Befehle, die folgen einem Muster wie folgt jedoch. Sie können SQL-Befehle erstellen Sie die Datenbanktabellen, die Anzahl der Datensätze in einer Tabelle, Preise berechnen und viele weitere Vorgänge ausführen.


## <a name="inserting-data-in-a-database"></a>Einfügen von Daten in einer Datenbank

In diesem Abschnitt wird gezeigt, wie auf eine Seite zu erstellen, kann der Benutzer ein neues Produkt zum Hinzufügen, der *Produkt* Datenbanktabelle. Nach dem Einfügen eines neuen Produktdatensatzes, die Seite zeigt die aktualisierte Tabelle mithilfe der *ListProducts.cshtml* Seite, die Sie im vorherigen Abschnitt erstellt haben.

Die Seite enthält die Überprüfung, um sicherzustellen, dass die Daten, die der Benutzer eingibt, für die Datenbank gültig sind. Auf der Seite Code macht z. B. sicher, dass für alle erforderlichen Spalten ein Wert eingegeben wurde.

1. Erstellen Sie eine neue CSHTML-Datei mit dem Namen der Website *InsertProducts.cshtml*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Der Text der Seite enthält ein HTML-Formular mit drei Textfelder, mit die Benutzer ein Name, Beschreibung und Preis eingeben können. Wenn der Benutzer klicken auf die **einfügen** Schaltfläche, die der Code am oberen Rand der Seite öffnet eine Verbindung mit der *SmallBakery.sdf* Datenbank. Sie erhalten dann die Werte, die der Benutzer mit übermittelt hat die `Request` Objekt, und weisen Sie diese Werte zu lokalen Variablen.

    Um zu überprüfen, dass der Benutzer einen Wert für jede erforderliche Spalte eingegeben haben, registrieren Sie jedes `<input>` Element, das Sie überprüfen möchten:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    Die `Validation` Helper wird überprüft, ob es ein Wert in jedes der Felder, die Sie registriert haben. Sie können testen, ob alle Felder überprüft die Überprüfung bestanden `Validation.IsValid()`, die Sie in der Regel tun, bevor Sie die Informationen Sie vom Benutzer erhalten zu verarbeiten:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (Die `&&` Operator bedeutet und – mit diesem Test wird *ist dies einer Formularübergabe und alle Felder haben die Überprüfung bestanden*.)

    Wenn alle Spalten überprüft (keine leer waren), fahren Sie fort, und erstellen Sie eine SQL-Anweisung, um die Daten einzufügen, und führen Sie es als Nächstes dargestellt:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Für die einzufügenden Werte, die Sie einschließen Parameterplatzhalter (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Als Sicherheitsmaßnahme müssen übergeben Sie Werte immer an eine SQL-Anweisung mit Parametern, wie Sie im vorherigen Beispiel sehen. Dies bietet Ihnen die Möglichkeit zum Überprüfen der Daten des Benutzers sowie dient zum Schutz gegen versucht, böswillige Befehle an der Datenbank (auch als SQL Injection-Angriffen bezeichnet) zu senden.

    Um die Abfrage auszuführen, verwenden Sie diese Anweisung übergeben, die Variablen, die die Werte als Ersatz für die Platzhalter enthalten:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Nach der `Insert Into` -Anweisung ausgeführt hat, senden Sie dem Benutzer zur Seite, die der Produkte, die mit dieser Zeile aufgeführt sind:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Wenn die Überprüfung war nicht erfolgreich, überspringen Sie die einfügen. Stattdessen müssen Sie eine Hilfsmethode auf der Seite, die die kumulierte Fehlermeldungen (sofern vorhanden) angezeigt werden kann:

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Beachten Sie, dass der Style-Block im Markup eine CSS-Klassendefinition mit dem Namen enthält `.validation-summary-errors`. Dies ist der Name der CSS-Klasse, die standardmäßig für die `<div>` Element, das Validierungsfehler enthält. In diesem Fall wird in CSS-Klasse gibt an, dass Zusammenfassung Validierungsfehler in Rot angezeigt werden und in Fettdruck, aber Sie definieren können, die `.validation-summary-errors` zum Anzeigen der keine Formatierung anwenden soll.

### <a name="testing-the-insert-page"></a>Testen die Seite "Einfügen"

1. Die Seite in einem Browser anzuzeigen. Die Seite zeigt ein Formular, mit dem Umwandlungsoperator ist, die in der folgenden Abbildung dargestellt ist.

    ![[Image]](5-working-with-data/_static/image5.jpg)
2. Geben Sie Werte für alle Spalten, aber stellen Sie sicher, dass Sie lassen die *Preis* Spalte leer.
3. Klicken Sie auf **einfügen**. Die Seite zeigt eine Fehlermeldung an, wie in der folgenden Abbildung dargestellt. (Keine neuer Datensatz wird erstellt.)

    ![[Image]](5-working-with-data/_static/image6.jpg)
4. Füllen Sie das Formular vollständig, und klicken Sie dann auf **einfügen**. Dieses Mal die *ListProducts.cshtml* Seite wird angezeigt und zeigt den neuen Datensatz.

## <a name="updating-data-in-a-database"></a>Aktualisieren von Daten in einer Datenbank

Nachdem Daten in eine Tabelle eingegeben wurde, müssen Sie möglicherweise zu aktualisieren. Dieses Verfahren wird gezeigt, wie zwei Seiten erstellen, die den Werten ähneln, die Sie zum Einfügen von Daten weiter oben erstellt haben. Die erste Seite Produkte angezeigt und ermöglicht Benutzern, die eine ändern auswählen. Die zweite Seite kann die Benutzer tatsächlich nehmen die Änderungen vor, und speichern Sie sie.

> [!NOTE] 
> 
> **Wichtige** In eine Produktionswebsite Sie in der Regel einschränken, wer darf hat, um die Daten zu ändern. Informationen zum Einrichten der Mitgliedschaft und zu den Möglichkeiten zum Autorisieren von Benutzern für Aufgaben auf der Website finden Sie unter [Sicherheit hinzufügen und die Mitgliedschaft in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Erstellen Sie eine neue CSHTML-Datei mit dem Namen der Website *EditProducts.cshtml*.
2. Ersetzen Sie das vorhandene Markup in der Datei durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    Der einzige Unterschied zwischen dieser Seite und die *ListProducts.cshtml* Seite zuvor stammt, dass die HTML-Tabelle auf dieser Seite eine zusätzliche Spalte enthält, die zeigt eine **bearbeiten** Link. Wenn Sie diesen Link klicken, gelangen Sie Sie auf die *UpdateProducts.cshtml* -Seite (die Sie als Nächstes erstellen) in dem Sie den ausgewählten Datensatz bearbeiten können.

    Sehen Sie sich den Code, erstellt der **bearbeiten** Link:

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Dies erstellt eine HTML `<a>` Element, dessen `href` Attribut dynamisch festgelegt ist. Die `href` Attribut gibt an, die Seite angezeigt, wenn der Benutzer auf den Link klickt. Sie übergibt außerdem die `Id` Wert der aktuellen Zeile auf den Link. Wenn die Seite ausgeführt wird, könnte die Seitenquelle Links wie diese enthalten:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Beachten Sie, dass die `href` -Attributsatz zur `UpdateProducts/n`, wobei *n* ist eine Produkt an. Wenn ein Benutzer einen dieser Links klickt, wird die ausgegebene URL etwa wie folgt aussehen:

    `http://localhost:18816/UpdateProducts/6`

    Das heißt, werden die Produktnummer bearbeitet werden in der URL übergeben.
3. Die Seite in einem Browser anzuzeigen. Die Seite zeigt die Daten in einem Format wie folgt:

    ![[Image]](5-working-with-data/_static/image7.jpg)

    Als Nächstes erstellen Sie die Seite, mit dem Benutzer die Daten zu aktualisieren. Die Seite "Updates" umfasst eine Überprüfung der zum Überprüfen der Daten, die der Benutzer eingibt. Auf der Seite Code macht z. B. sicher, dass für alle erforderlichen Spalten ein Wert eingegeben wurde.
4. Erstellen Sie eine neue CSHTML-Datei mit dem Namen der Website *UpdateProducts.cshtml*.
5. Ersetzen Sie das vorhandene Markup in der Datei durch Folgendes.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Der Hauptteil der Seite "enthält ein HTML-Formular, in dem ein Produkt angezeigt wird und in dem Sie Benutzer bearbeiten können. Um zum Anzeigen des Produkts zu erhalten, verwenden Sie die folgende SQL-Anweisung:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Dadurch werden ausgewählt, das Produkt, dessen ID entspricht dem Wert, der übergeben, der `@0` Parameter. (Da *Id* ist die primary key- und aus diesem Grund muss eindeutig sein, kann nur ein Produktdatensatz jemals auf diese Weise ausgewählt werden.) Zum Abrufen der ID-Wert für die Übergabe dieser `Select` -Anweisung, lesen Sie den Wert, der auf der Seite "als Teil der URL übergeben wird, mithilfe der folgenden Syntax:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Um tatsächlich Produktdatensatz abzurufen, verwenden Sie die `QuerySingle` -Methode, die nur ein Datensatz zurückgegeben wird:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    Die einzelne Zeile zurückgegeben wird, in dem `row` Variable. Sie können Daten aus jeder Spalte abzurufen und weisen Sie diese lokalen Variablen wie folgt:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    In das Markup für das Formular werden diese Werte automatisch in einzelne Textfelder angezeigt, mithilfe von eingebettetem Code wie folgt:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Dieser Teil des Codes zeigt Produktdatensatz aktualisiert werden. Nachdem der Datensatz angezeigt wurde, kann der Benutzer die einzelne Spalten bearbeiten.

    Wenn der Benutzer das Formular übermittelt, indem Sie auf die **Update** Schaltfläche, den Code in der `if(IsPost)` -Block ausgeführt. Dadurch wird der Benutzer Werte aus der `Request` Objekt, speichert die Werte in Variablen und überprüft, dass jede Spalte gefüllt wurde. Wenn die Clusterüberprüfung erfolgreich war, erstellt der Code die folgende SQL-Update-Anweisung aus:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    In einer SQL `Update` -Anweisung geben Sie jede Spalte zu aktualisieren und den Wert auf festgelegt ist, dass. In diesem Code werden die Werte angegeben, über die Parameterplatzhalter `@0`, `@1`, `@2`und so weiter. (Wie oben erwähnt aus Sicherheitsgründen sollten Sie immer Werte für eine SQL­Anweisung übergeben mit Parametern.)

    Beim Aufrufen der `db.Execute` -Methode, übergeben Sie die Variablen, die die Werte in der Reihenfolge enthalten, die die Parameter in der SQL-Anweisung entspricht:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Nach der `Update` Anweisung ausgeführt wurde, rufen Sie die folgende Methode, um den Benutzer zur Bearbeitungsseite zurückgeleitet:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    Der Effekt ist, dass eine aktualisierte Liste der Daten in der Datenbank angezeigt, und der Benutzer ein anderes Produkt bearbeiten kann.
6. Speichern Sie die Seite.
7. Führen Sie die *EditProducts.cshtml* Seite (nicht die Update-Seite), und klicken Sie dann auf **bearbeiten** , wählen Sie ein Produkt zu bearbeiten. Die *UpdateProducts.cshtml* Seite wird angezeigt, mit dem Datensatz, die Sie ausgewählt haben.

    ![[Image]](5-working-with-data/_static/image8.jpg)
8. Eine Änderung vornehmen, und klicken Sie auf **Update**. Die Liste der Produkte wird mit der aktualisierten Daten erneut angezeigt.

## <a name="deleting-data-in-a-database"></a>Löschen von Daten in einer Datenbank

In diesem Abschnitt wird gezeigt, wie können Benutzer löschen Sie ein Produkt aus der *Produkt* Datenbanktabelle. Das Beispiel besteht aus zwei Seiten. Wählen Sie auf der ersten Seite Benutzer einen Datensatz zu löschen. Der Datensatz gelöscht werden, wird in einer zweiten Seite angezeigt, mit dem sie bestätigen, dass sie den Datensatz löschen möchten.

> [!NOTE] 
> 
> **Wichtige** In eine Produktionswebsite Sie in der Regel einschränken, wer darf hat, um die Daten zu ändern. Informationen über das Einrichten von Mitgliedschaft und Methoden zum Autorisieren von Benutzer zum Ausführen von Aufgaben auf der Website finden Sie unter [Sicherheit hinzufügen und die Mitgliedschaft in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Erstellen Sie eine neue CSHTML-Datei mit dem Namen der Website *ListProductsForDelete.cshtml*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Auf dieser Seite ähnelt der *EditProducts.cshtml* Seite von oben. Jedoch, anstatt ein **bearbeiten** Link für jedes Produkt, es zeigt eine **löschen** Link. Die **löschen** Link wird erstellt, mit dem folgenden eingebetteten Code in das Markup:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Dies erstellt eine URL, die wie folgt aussieht, wenn Benutzer auf den Link klicken:

    `http://<server>/DeleteProduct/4`

    Die URL aufruft, eine Seite mit dem Namen *DeleteProduct.cshtml* (die Sie als Nächstes erstellen) und übergibt sie die ID des Produkts zu löschen (in diesem Fall 4).
3. Speichern Sie die Datei, aber lassen Sie es offen.
4. Erstellen Sie eine andere CHTML-Datei mit dem Namen *DeleteProduct.cshtml*. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Auf dieser Seite wird aufgerufen, indem *ListProductsForDelete.cshtml* und ermöglicht Benutzern, vergewissern Sie sich, dass sie ein Produkt löschen möchten. Um das Produkt zu löschenden aufzulisten, erhalten Sie die ID des Produkts, löschen Sie die URL mithilfe des folgenden Codes:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    Anschließend müssen Sie die Seite den Benutzer auf eine Schaltfläche, um den Datensatz zu löschen. Dies ist eine Sicherheitsmaßnahme wichtig: Wenn Sie sicherheitsrelevante Vorgänge in Ihrer Website aktualisieren oder Löschen von Daten ausführen, diese Vorgänge sollten immer erfolgen mithilfe eines POST-Vorgangs nicht auf einen GET-Vorgang. Wenn Ihre Website festgelegt ist, damit ein Löschvorgang mit einem GET-Vorgang ausgeführt werden kann, kann jeder Benutzer eine URL wie übergeben `http://<server>/DeleteProduct/4` und beliebige Änderungen aus der Datenbank zu löschen. Durch Hinzufügen der Bestätigung und Codierung der Seite, damit der Löschvorgang ausgeführt werden kann, nur mithilfe einer POST-Anforderung, fügen Sie ein Maß an Sicherheit auf Ihrer Website.

    Der tatsächliche Löschvorgang erfolgt mithilfe des folgenden Codes, die zunächst verifiziert, dass dies eine Post-Vorgang ist und die ID nicht leer ist:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Der Code führt eine SQL­Anweisung, die löscht den angegebenen Datensatz und dann wieder an die Auflistung Seite leitet den Benutzer.
5. Führen Sie *ListProductsForDelete.cshtml* in einem Browser.

    ![[Image]](5-working-with-data/_static/image9.jpg)
6. Klicken Sie auf die **löschen** Link, um eines der Produkte. Die *DeleteProduct.cshtml* Seite wird angezeigt, um sicherzustellen, dass Sie diesen Datensatz löschen möchten.
7. Klicken Sie auf die **löschen** Schaltfläche. Produktdatensatz wird gelöscht, und die Seite wird mit einer aktualisierten Produktliste aktualisiert.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Herstellen einer Verbindung mit einer Datenbank
> 
> Sie können eine Verbindung mit einer Datenbank auf zwei Arten herstellen. Der erste Schritt besteht im Verwenden der `Database.Open` Methode und den Namen der Datenbankdatei angeben (weniger der *.sdf* Erweiterung):
> 
> `var db = Database.Open("SmallBakery");`
> 
> Die `Open` Methode setzt voraus, dass die. *Sdf* Datei befindet sich in der Website *App\_Daten* Ordner. Dieser Ordner ist speziell für die Aufnahme von Daten dient. Beispielsweise kann die erforderlichen Berechtigungen für die Website zum Lesen und Schreiben von Daten ermöglichen, und als eine Sicherheitsmaßnahme WebMatrix lässt keine Zugriff auf Dateien aus diesem Ordner.
> 
> Die zweite Möglichkeit ist die Verwendung eine Verbindungszeichenfolge. Eine Verbindungszeichenfolge enthält Informationen zum Herstellen einer Verbindung mit einer Datenbank. Dies kann einen Pfad einschließen, oder er kann den Namen des SQL Server-Datenbank auf einem lokalen oder remote-Server zusammen mit einem Benutzernamen und das Kennwort für die Verbindung zu diesem Server enthalten. (Wenn Sie Daten in einer zentral verwalteten Version von SQL Server speichern, verwenden z. B. auf einem Hostinganbieter-Website Sie immer eine Verbindungszeichenfolge an den Verbindungsinformationen zur Datenbank.)
> 
> In WebMatrix Verbindungszeichenfolgen werden in der Regel gespeichert in einer XML-Datei mit dem Namen *"Web.config"*. Wie der Name schon sagt, können Sie eine *"Web.config"* Datei im Stammverzeichnis der Website zum Speichern der Website-Konfigurationsinformationen, einschließlich Verbindungszeichenfolgen, die Ihre Website erforderlich sind. Ein Beispiel für eine Verbindungszeichenfolge in einer *"Web.config"* Datei könnte wie folgt aussehen:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> Im Beispiel zeigt die Verbindungszeichenfolge zu einer Datenbank in einer Instanz von SQL Server, die auf einem Server an einer beliebigen Stelle ausgeführt wird (im Gegensatz zu einer lokalen *.sdf* Datei). Sie müssten zu ersetzen, die entsprechenden Namen für `myServer` und `myDatabase`, und geben Sie Datenwerte von SQL Server-Anmeldung für `username` und `password`. (Die Werte für Benutzername und Kennwort sind nicht unbedingt identisch als Ihre Windows-Anmeldeinformationen oder die Werte, die von Ihrem Hostinganbieter Sie zum Anmelden an ihren Servern akzeptiert hat. Erkundigen Sie sich der Administrator für die genauen Werte, die Sie benötigen.)
> 
> Die `Database.Open` Methode ist flexibel, da Sie entweder den Namen einer Datenbank übergeben können *.sdf* Datei oder den Namen einer Verbindungszeichenfolge, die in gespeichert ist die *"Web.config"* Datei. Im folgenden Beispiel wird das Herstellen einer Verbindung mit der Datenbank mithilfe der Verbindungszeichenfolge, die im vorherigen Beispiel veranschaulicht:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Wie bereits erwähnt, die `Database.Open` Methode können Sie einen Datenbanknamen oder eine Verbindungszeichenfolge zu übergeben, und es werden verwendet. Dies ist sehr nützlich, bei der Bereitstellung (Veröffentlichung) Ihrer Website. Können Sie eine *.sdf* in der Datei die *App\_Daten* Ordner, wenn Sie entwickeln und Testen Ihrer Website. Wenn Sie Ihre Website auf einen Produktionsserver verschieben, können Sie eine Verbindungszeichenfolge im Verwenden der *"Web.config"* Datei mit dem gleichen Namen wie Ihre *.sdf* Datei, aber verweist auf des Hostinganbieters &#8212;alle ohne den Code ändern.
> 
> Abschließend, wenn Sie direkt mit einer Verbindungszeichenfolge arbeiten möchten, können rufen Sie die `Database.OpenConnectionString` Methode und übergeben es die tatsächliche Verbindungszeichenfolge statt nur den Namen eines in der *"Web.config"* Datei. Dies ist möglicherweise hilfreich in Situationen, in denen aus irgendeinem Grund Sie keinen Zugriff auf die Verbindungszeichenfolge haben (oder Werte, z. B. die *.sdf* Dateiname), bis die Seite ausgeführt wird. Allerdings in den meisten Szenarien können Sie `Database.Open` wie in diesem Artikel beschrieben.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Herstellen einer Verbindung mit einer SQL Server- oder MySQL-Datenbank in WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Überprüfen der Benutzereingabe in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=253002)
