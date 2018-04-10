---
uid: web-pages/overview/data/working-with-files
title: Arbeiten mit Dateien an einem Standort der ASP.NET Web Pages (Razor) | Microsoft Docs
author: tfitzmac
description: In diesem Kapitel wird erläutert, wie lesen, schreiben, anfügen, löschen und Hochladen von Dateien.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 0f119f8fb4873e55292203f21a2efd8f26793ae4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Arbeiten mit Dateien an einem Standort der ASP.NET Web Pages (Razor)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie lesen, schreiben, anfügen, löschen und Hochladen von Dateien an einem Standort der ASP.NET Web Pages (Razor).
> 
> > [!NOTE]
> > Wenn zum Hochladen von Images und bearbeitet werden sollen (z. B. kippen, oder passen Sie sie), finden Sie unter [arbeiten mit Bildern in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202897).
> 
> 
> **Lernen Sie:** 
> 
> - Informationen zum Erstellen einer Textdatei und Daten in sie schreiben.
> - Wie Sie Daten an eine vorhandene Datei anfügen.
> - Informationen zum Lesen einer Datei und daraus anzuzeigen.
> - Vorgehensweise: Löschen von Dateien von einer Website.
> - Wie können Benutzer eine oder mehrere Dateien hochladen.
> 
> Hierbei handelt es sich um den ASP.NET Programmieren von Funktionen, die im Artikel:
> 
> - Die `File` -Objekt, das eine Möglichkeit zum Verwalten von Dateien bereitstellt.
> - Die `FileUpload` Helper.
> - Die `Path` -Objekt, das Methoden bereitstellt, mit denen Sie den Pfad und den Dateinamen zu ändern.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Dieses Lernprogramm funktioniert auch mit WebMatrix-3.


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Erstellen einer Textdatei, und Schreiben von Daten zu

Zusätzlich zur Verwendung einer Datenbank in Ihrer Website, können Sie mit Dateien arbeiten. Sie können z. B. Textdateien als eine einfache Möglichkeit zum Speichern von Daten für den Standort verwenden. (Eine Textdatei, die verwendet wird, zum Speichern von Daten bezeichnet eine *Flatfile*.) Textdateien kann in verschiedenen Formaten wie z. B. *".txt"*, *XML*, oder *CSV* (durch Trennzeichen getrennte Werte).

Wenn Sie Daten in einer Textdatei speichern möchten, können Sie mithilfe der `File.WriteAllText` Methode zum Angeben der zu erstellenden Datei und die Daten zu schreiben. In diesem Verfahren erstellen Sie eine Seite, die ein einfaches Formular mit drei enthält `input` Elemente (Vorname, Nachname und e-Mail-Adresse) und ein **Absenden** Schaltfläche. Wenn der Benutzer das Formular sendet, müssen Sie die Eingaben des Benutzers in einer Textdatei speichern.

1. Erstellen Sie einen neuen Ordner namens *App\_Daten*, sofern er nicht bereits vorhanden.
2. Erstellen Sie im Stammverzeichnis der Website eine neue Datei namens *UserData.cshtml*.
3. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    Das HTML-Markup erstellt ein Formular mit drei Textfelder. Verwenden Sie im Code die `IsPost` Eigenschaft, um zu bestimmen, ob die Seite übermittelt wurde, bevor Sie die Verarbeitung gestartet.

    Die erste Aufgabe wird zum Abrufen der Benutzereingabe und Variablen zuweisen. Der Code verkettet die Werte von zwei separate Variablen klicken Sie dann in eine durch Trennzeichen getrennte Zeichenfolge, die dann in eine andere Variable gespeichert werden. Beachten Sie, dass das Komma als Trennzeichen eine Zeichenfolge in Anführungszeichen (","), da buchstäblich ein Komma in großen Zeichenfolge einzubettenden, die Sie erstellen. Fügen Sie am Ende der Daten, die Sie verketten, `Environment.NewLine`. Dies fügt einen Zeilenumbruch (ein neue Zeilenumbruchzeichen). Was Sie mit der alle diese Verkettung erstellen, ist eine Zeichenfolge, die wie folgt aussieht:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Mit einem unsichtbaren Zeilenumbruch am Ende.)

    Anschließend erstellen Sie eine Variable (`dataFile`), die den Speicherort und Namen der Datei zum Speichern der Daten enthält. Festlegen des Speicherorts erfordert einige besondere Behandlung. Auf Websites, ist es kein empfehlenswertes Verfahren, die in absolute Pfade wie im Code finden Sie unter *C:\Folder\File.txt* für Dateien auf dem Webserver. Wenn eine Website verschoben wird, wird ein absoluter Pfad falsch sein. Darüber hinaus kennen nicht für eine gehostete Website (im Gegensatz zur auf Ihrem eigenen Computer) Sie in der Regel sogar der richtige Pfad, wenn Sie den Code schreiben.

    Aber in einigen Fällen (z. B. Now, für das Schreiben einer Datei) benötigen Sie einen vollständigen Pfad. Die Lösung besteht darin, mithilfe der `MapPath` Methode der `Server` Objekt. Dies gibt den vollständigen Pfad zu Ihrer Website zurück. Beim Abrufen des Pfads für den Stammordner der Website, Sie als Benutzer der `~` Operator (Represen die Website des virtuellen Stamm), `MapPath`. (Sie können auch einen Unterordnernamen, übergeben, z. B. *~/App\_Daten /*, um den Pfad für diese Unterordner abzurufen.) Sie können dann verketten Zusatzinformationen auf unabhängig von die Methode zurückgegeben wird, um einen vollständigen Pfad zu erstellen. In diesem Beispiel fügen Sie einen Dateinamen an. (Weitere Informationen zum Arbeiten mit Datei- und Ordnerpfade in [Einführung in ASP.NET Web Pages Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    Die Datei wird gespeichert, der *App\_Daten* Ordner. Dieser Ordner ist ein spezieller Ordner, die zum Speichern von Datendateien, verwendet wird, wie beschrieben in ASP.NET [Einführung in die Arbeit mit einer Datenbank in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=195209).

    Die `WriteAllText` Methode der `File` Objekt schreibt die Daten in der Datei. Diese Methode akzeptiert zwei Parameter: den Namen (Pfad) der Datei zum Schreiben und die tatsächlichen Daten schreiben. Beachten Sie, dass mit der Namen des ersten Parameters ein `@` Zeichen als Präfix. Dies weist ASP.NET, die Sie eine wörtliche Zeichenfolge bereitgestellt, und Zeichen wie "/" nicht auf besondere Weise interpretiert werden soll. (Weitere Informationen finden Sie unter [Einführung in ASP.NET Web-Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > In der Reihenfolge für den Code zum Speichern von Dateien in den *App\_Daten* Ordner der Anwendung muss Lese-/ Schreibzugriff Berechtigungen für diesen Ordner. Auf Ihrem Computer ist dies nicht in der Regel ein Problem. Wenn Sie Ihre Website mit einem Hostinganbieter Webserver veröffentlichen, müssen Sie diese Berechtigungen explizit festzulegen. Wenn Sie diesen Code auf einem Hostinganbieter Server ausführen und Fehler gemeldet werden, überprüfen Sie mit dem Hostinganbieter, herauszufinden, wie diese Berechtigungen festlegen.

- Führen Sie die Seite in einem Browser aus. 

    ![](working-with-files/_static/image1.jpg)
- Geben Sie Werte in die Felder aus, und klicken Sie dann auf **Absenden**.
- Schließen Sie den Browser.
- Kehren Sie zum zurück, und aktualisieren Sie die Ansicht.
- Öffnen der *data.txt* Datei. Die Daten, die Sie in das Formular gesendet werden, in der Datei. 

    ![[Image]](working-with-files/_static/image2.jpg)
- Schließen der *data.txt* Datei.

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Daten an eine vorhandene Datei anfügen

Im vorherigen Beispiel, verwendet Sie `WriteAllText` zum Erstellen einer Textdatei, die nur ein Teil der Daten in diese Dateien enthält. Wenn Sie die Methode erneut aufrufen und den Dateinamen zu übergeben, wird die vorhandene Datei vollständig überschrieben. Allerdings nach der Erstellung einer Datei häufig neue Daten an das Ende der Datei hinzufügen möchten. Erreichen Sie, dass die Verwendung der `AppendAllText` Methode der `File` Objekt.

1. Stellen Sie auf der Website, eine Kopie der *UserData.cshtml* Datei, und nennen Sie die Kopie *UserDataMultiple.cshtml*.
2. Ersetzen Sie vor dem Öffnen den Codeblock `<!DOCTYPE html>` Tag mit den folgenden Codeblock: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Dieser Code enthält eine Änderung aus dem vorherigen Beispiel. Anstatt `WriteAllText`, verwendet er `the AppendAllText` Methode. Die Methoden sind ähnlich, außer dass `AppendAllText` fügt die Daten an das Ende der Datei. Wie bei `WriteAllText`, `AppendAllText` erstellt die Datei aus, wenn er nicht bereits vorhanden.
3. Führen Sie die Seite in einem Browser aus.
4. Geben Sie Werte für die Felder aus, und klicken Sie dann auf **Absenden**.
5. Fügen Sie weitere Daten hinzu, und senden Sie das Formular erneut.
6. Zurück zu Ihrem Projekt rechten Maustaste auf den Projektordner, und klicken Sie dann auf **aktualisieren**.
7. Öffnen der *data.txt* Datei. Es enthält nun die neuen Daten, die Sie gerade eingegeben haben. 

    ![[Image]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Lesen und Anzeigen von Daten aus einer Datei

Auch wenn Sie keine Daten in eine Textdatei schreiben müssen, müssen Sie möglicherweise gelegentlich zum Lesen von Daten aus einer. Zu diesem Zweck können Sie erneut die `File` Objekt. Können Sie die `File` Objekt jede Zeile einzeln gelesen (getrennt durch Zeilenumbrüche) oder zum Lesen einzelnen Artikels unabhängig davon, wie sie getrennt sind.

Dieses Verfahren wird gezeigt, wie zum Lesen und Anzeigen der Daten, die Sie im vorherigen Beispiel erstellt.

1. Erstellen Sie im Stammverzeichnis der Website eine neue Datei namens *DisplayData.cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Der Code beginnt mit dem Lesen der Datei, die Sie im vorherigen Beispiel erstellt, in einer Variablen namens haben `userData`, mithilfe dieser Methodenaufruf:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Der Code dazu befindet sich innerhalb einer `if` Anweisung. Wenn Sie eine Datei zu lesen möchten, ist es eine gute Idee, verwenden Sie die `File.Exists` Methode, um zunächst zu bestimmen, ob die Datei verfügbar ist. Der Code überprüft außerdem, ob die Datei leer ist.

    Der Text der Seite enthält zwei `foreach` Schleifen, die andere geschachtelten. Die äußere `foreach` Schleife Ruft eine Zeile zu einem Zeitpunkt aus der Datendatei. In diesem Fall werden die Zeilen durch Zeilenumbrüche in der Datei definiert &#8212; also ist jedes Datenelement in einer eigenen Zeile. Die äußere Schleife erstellt ein neues Element (`<li>` Element) in eine geordnete Liste (`<ol>` Element).

    Die innere Schleife teilt jede Datenzeile in Elemente (Felder) mit einem Komma als Trennzeichen. (Basierend auf dem vorhergehenden Beispiel, das bedeutet, dass jede Zeile drei Felder enthält &#8212; der Vorname, Nachname und e-Mail-Adresse, jeweils durch ein Komma getrennt.) Die innere Schleife erstellt außerdem eine `<ul>` Liste und zeigt eine Liste Element für jedes Feld in der Datenzeile.

    Der Code wird veranschaulicht, wie zwei Typen von Daten, ein Array verwenden und die `char` -Datentyp. Das Array ist erforderlich, da die `File.ReadAllLines` -Methode Daten als Array zurückgibt. Die `char` -Datentyp ist erforderlich, da die `Split` Methode gibt ein `array` in dem jedes Element des Typs ist `char`. (Weitere Informationen zu Arrays finden Sie unter [Einführung in ASP.NET Web-Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Führen Sie die Seite in einem Browser aus. Die Daten für den vorherigen Beispielen eingegebene werden angezeigt. 

    ![[Image]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Anzeigen von Daten aus einer Microsoft Excel durch Trennzeichen getrennte Datei**
> 
> Verwenden Sie Microsoft Excel, um die Daten in eine Tabelle als eine durch Trennzeichen getrennte Datei zu speichern (*CSV* Datei). In diesem Fall wird die Datei im nur-Text nicht in Excel-Format gespeichert. Jede Zeile im Arbeitsblatt wird getrennt durch einen Zeilenumbruch in der Textdatei, und jedes Datenelement wird durch ein Komma getrennt. Den Code im vorherigen Beispiel gezeigt können Sie eine durch Trennzeichen getrennten Excel-Datei lesen, indem Sie einfach den Namen der Datendatei im Code zu ändern.


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Löschen von Dateien

Um Dateien aus der Website löschen, können Sie die `File.Delete` Methode. Dieses Verfahren wird gezeigt, wie können Benutzer ein Bild löschen (*jpg* Datei) aus einer *Bilder* Ordner, wenn sie wissen, dass der Name der Datei.

> [!NOTE] 
> 
> **Wichtige** In eine Produktionswebsite Sie in der Regel einschränken, wer darf hat, um die Daten zu ändern. Informationen zum Einrichten der Mitgliedschaft und zu den Möglichkeiten zum Autorisieren von Benutzern für Aufgaben auf der Website finden Sie unter [Sicherheit hinzufügen und die Mitgliedschaft in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Erstellen Sie auf der Website einen Unterordner namens *Bilder*.
2. Kopieren einer oder mehrerer *jpg* Dateien in der *Bilder* Ordner.
3. Erstellen Sie im Stammverzeichnis der Website, eine neue Datei namens *FileDelete.cshtml*.
4. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Diese Seite enthält eine Form, in dem Benutzer den Namen einer Bilddatei eingeben können. Sie geben Sie nicht die *jpg* Dateinamenerweiterung; durch Beschränken der Dateiname sieht, Sie Hilfe verhindert, dass Benutzer beliebige Dateien auf Ihrer Website gelöscht.

    Der Code liest den Dateinamen an, dass der Benutzer eingegeben hat, und klicken Sie dann einen vollständigen Pfad erstellt. So erstellen Sie den Pfad, verwendet der Code den Pfad der aktuellen Website (wie vom der `Server.MapPath` Methode), die *Bilder* Ordnernamen, den Namen, den der Benutzer bereitgestellt hat und ".jpg" als Literalzeichenfolge.

    So löschen Sie die Datei, die der Code Ruft die `File.Delete` -Methode, und übergeben sie den vollständigen Pfad, die Sie soeben erstellt haben. Am Ende das Markup zeigt Code für eine bestätigungsmeldung an, dass die Datei gelöscht wurde.
5. Führen Sie die Seite in einem Browser aus. 

    ![[Image]](working-with-files/_static/image5.jpg)
6. Geben Sie den Namen der Datei zu löschen, und klicken Sie dann auf **Absenden**. Wenn die Datei gelöscht wurde, wird der Name der Datei am unteren Rand der Seite angezeigt.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Infrastrukturcode Benutzer Hochladen einer Datei

Die `FileUpload` -Hilfsobjekt ermöglicht Benutzern, die Dateien auf Ihre Website hochzuladen. Im folgenden Verfahren wird gezeigt, wie können Benutzer, die eine einzelne Datei hochladen.

1. Der ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), wenn Sie zuvor hinzugefügt haben.
2. In der *App\_Daten* Ordner, erstellen Sie ein neues einen Ordner, und nennen Sie sie *UploadedFiles*.
3. Erstellen Sie im Stammverzeichnis, eine neue Datei namens *FileUpload.cshtml*.
4. Ersetzen Sie den vorhandenen Inhalt auf der Seite mit den folgenden: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    Der Body-Teil der Seite "verwendet die `FileUpload` Hilfsmethode zum Erstellen der Upload-Feld und die Schaltflächen, die Sie wahrscheinlich mit vertraut sind:

    ![[Image]](working-with-files/_static/image6.jpg)

    Die Eigenschaften, die Sie, für festlegen die `FileUpload` Helper angeben, dass Sie ein einzelnes Kontrollkästchen für die Datei hochladen möchten, und dass die Schaltfläche "Absenden" gelesen werden soll **hochladen**. (Sie werden später in diesem Artikel weitere Felder hinzufügen.)

    Wenn der Benutzer klickt **hochladen**, der Code am oberen Rand der Seite ruft die Datei ab und speichert es. Die `Request` -Objekt, das Sie normalerweise verwenden, um das Abrufen von Werten aus Formularfelder verfügt auch über eine `Files` Array, das die Datei (oder Dateien) enthält, die hochgeladen worden sein. Sie können einzelne Dateien aus bestimmten Positionen im Array abrufen &#8212; zum Abrufen der ersten hochgeladenen Datei erhalten Sie z. B. `Request.Files[0]`, um die zweite Datei abzurufen erhalten Sie `Request.Files[1]`und so weiter. (Beachten Sie, dass bei der Programmierung zählen in der Regel auf 0 (null) beginnt.)

    Wenn Sie eine hochgeladene Datei abzurufen, Sie fügen Sie ihn in einer Variablen (hier `uploadedFile`), damit Sie diese bearbeiten können. Den Namen der hochgeladenen Datei ermitteln möchten, erhalten Sie nur seine `FileName` Eigenschaft. Jedoch, wenn der Benutzer wird eine Datei hochgeladen, `FileName` enthält den Namen des Benutzers ursprünglichen, darunter den vollständigen Pfad. Es kann wie folgt aussehen:

    *C:\Users\Public\Sample.txt*

    Sie soll nicht alle dieser Pfadinformationen allerdings, da dies der Pfad auf dem Computer des Benutzers, nicht für den Server ist. Sie möchten nur den tatsächlichen Dateinamen (*"Sample.txt"*). Sie müssen nur die Datei aus einem Pfad entfernen können, mithilfe der `Path.GetFileName` -Methode, wie folgt:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    Die `Path` Objekt ist ein Hilfsprogramm, das eine Reihe von Methoden wie folgt, die Sie verwenden können verfügt, um Pfade zu entfernen, Kombinieren von Pfaden und so weiter.

    Nachdem Sie den Namen der hochgeladenen Datei gelangt sind, können Sie erstellen einen neuen Pfad für die hochgeladene Datei auf Ihrer Website gespeichert werden soll. In diesem Fall kombinieren Sie `Server.MapPath`, die Ordnernamen (*App\_Daten/UploadedFiles*), und der neu blanker Dateiname, erstellen Sie einen neuen Pfad. Rufen Sie dann der hochgeladenen Datei `SaveAs` Methode, um die Datei zu speichern.
5. Führen Sie die Seite in einem Browser aus. 

    ![[Image]](working-with-files/_static/image7.jpg)
6. Klicken Sie auf **Durchsuchen** und wählen Sie dann eine Datei zum Hochladen. 

    ![[Image]](working-with-files/_static/image8.jpg)

    Das Textfeld neben der **Durchsuchen** Schaltfläche enthält den Pfad und den Dateispeicherort.

    ![[Image]](working-with-files/_static/image9.jpg)
7. Klicken Sie auf **Hochladen**.
8. Klicken Sie auf der Website mit der rechten Maustaste in des Projektordners, und klicken Sie dann auf **aktualisieren**.
9. Öffnen der *UploadedFiles* Ordner. Die Datei, die Sie hochgeladen haben, wird im Ordner "". 

    ![[Image]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Ermöglicht Benutzern Hochladen mehrerer Dateien

Im vorherigen Beispiel können Sie Benutzer, die eine Datei hochladen. Sie können jedoch die `FileUpload` Hilfsfunktion, um mehrere Dateien gleichzeitig hochladen. Dies ist praktisch für Szenarien wie beim Hochladen von Fotos, in denen Hochladen einer Datei zu einem Zeitpunkt aufwändig ist. (Finden Sie Informationen zum Hochladen von Fotos in [arbeiten mit Bildern in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202897).) Dieses Beispiel zeigt, wie Benutzer, die zwei gleichzeitig hochladen können, obwohl Sie das gleiche Verfahren verwenden können, um mehr als die hochzuladen.

1. Der ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.
2. Erstellen Sie eine neue Seite mit dem Namen *FileUploadMultiple.cshtml*.
3. Ersetzen Sie den vorhandenen Inhalt auf der Seite mit den folgenden:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    In diesem Beispiel wird die `FileUpload` Helper im Hauptteil der Seite "ist so konfiguriert, dass Benutzer, die zwei Dateien hochladen können standardmäßig. Da `allowMoreFilesToBeAdded` festgelegt ist, um `true`, die-Hilfsmethode rendert einen Link zum Benutzer, die weitere Upload-Felder hinzufügen:

    ![[Image]](working-with-files/_static/image11.jpg)

    Um die Dateien zu verarbeiten, die der Benutzer hochgeladen, der Code verwendet das gleiche grundlegende Verfahren, die Sie im vorherigen Beispiel verwendet &#8212; Abrufen einer Datei aus `Request.Files` und speichern Sie sie. (Einschließlich verschiedene Dinge müssen Sie tun können, um den richtigen Dateinamen und Pfad.) Die Innovation diesmal ist, dass der Benutzer möglicherweise mehrerer Dateien hochladen und viele nicht kennen. Um herauszufinden, erhalten Sie `Request.Files.Count`.

    Mit dieser Nummer in Hand, können Sie eine Schleife über `Request.Files`, jede Datei wiederum abrufen und speichern Sie sie. Wenn eine bekannte Anzahl von Malen durch eine Auflistung in einer Schleife werden sollen, können Sie eine `for` Schleife wie folgt:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    Die Variable `i` ist ein temporärer Zähler, die aus 0 (null), um die Obergrenze aufgenommen werden, festlegen. In diesem Fall wird die Obergrenze für die Anzahl der Dateien. Da der Zähler auf 0 (null), wie für das zählen in Szenarien in ASP.NET wird gestartet wird, die Obergrenze weist jedoch tatsächlich um eins kleiner als die Anzahl der Dateien. (Wenn drei Dateien hochgeladen wurden, ist die Anzahl 0 (null) auf 2.)

    Die `uploadedCount` Variable summiert die Dateien, die erfolgreich hochgeladen und gespeichert werden. Dieser Code Konten für die Möglichkeit, die eine erwartete Datei möglicherweise nicht hochgeladen werden können.
4. Führen Sie die Seite in einem Browser aus. Der Browser zeigt die zwei Upload-Feldern und die Seite.
5. Wählen Sie zwei Dateien hochladen.
6. Klicken Sie auf **fügen Sie eine andere Datei**. Die Seite zeigt ein Feld "Neues hochladen". 

    ![[Image]](working-with-files/_static/image12.jpg)
7. Klicken Sie auf **Hochladen**.
8. Klicken Sie auf der Website mit der rechten Maustaste in des Projektordners, und klicken Sie dann auf **aktualisieren**.
9. Öffnen der *UploadedFiles* sehen Sie die Dateien erfolgreich hochgeladen.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Arbeiten mit Bildern in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202897)

[Exportieren als CSV-Datei](https://msdn.microsoft.com/library/ms155919.aspx)
