---
uid: web-pages/overview/data/working-with-files
title: Arbeiten mit Dateien in einer ASP.NET Web Pages (Razor)-Website | Microsoft-Dokumentation
author: tfitzmac
description: In diesem Kapitel wird erläutert, wie lesen, schreiben, anfügen, löschen und Hochladen von Dateien.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: bd2b432eaa8bff31a8c4432d249308049a4c6aec
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364671"
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Arbeiten mit Dateien in einer ASP.NET Web Pages (Razor)-Website
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie lesen, schreiben, anfügen, löschen und Hochladen von Dateien in einer ASP.NET Web Pages (Razor)-Website.
> 
> > [!NOTE]
> > Zum Hochladen von Bildern sowie zum Bearbeiten dieser Wunsch (z. B. kippen oder ihre Größe ändern), finden Sie unter [arbeiten mit Bildern in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202897).
> 
> 
> **Sie lernen Folgendes:** 
> 
> - So erstellen Sie eine Textdatei, und Schreiben von Daten in diese.
> - Wie Sie Daten an eine vorhandene Datei angefügt werden sollen.
> - Informationen zum Lesen einer Datei, und von ihm angezeigt.
> - Vorgehensweise: Löschen von Dateien auf einer Website.
> - Informationen zum Benutzer, die eine oder mehrere Dateien hochladen können.
> 
> Hierbei handelt es sich um das ASP.NET-PROGRAMMIERMODELL in diesem Artikel vorgestellten Funktionen dar:
> 
> - Die `File` -Objekt, das bietet eine Möglichkeit, Dateien zu verwalten.
> - Die `FileUpload` Helper.
> - Die `Path` -Objekt, das Methoden bereitstellt, mit denen Sie die Pfad-und Dateinamen zu ändern.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> In diesem Tutorial funktioniert auch mit WebMatrix 3.


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Erstellen einer Textdatei, und Schreiben von Daten

Zusätzlich zur Verwendung einer Datenbank auf Ihrer Website, können Sie mit Dateien arbeiten. Sie können z. B. Textdateien wie eine einfache Möglichkeit zum Speichern von Daten für den Standort verwenden. (Eine Textdatei, die verwendet wird, zum Speichern von Daten bezeichnet ein *Flatfile*.) Textdateien können in unterschiedlichen Formaten werden, wie z. B. *.txt*, *XML*, oder *CSV* (durch Trennzeichen getrennte Werte).

Wenn Sie Daten in einer Textdatei speichern möchten, können Sie mithilfe der `File.WriteAllText` -Methode zur Angabe der zu erstellenden Datei, und die Daten zu schreiben. In diesem Verfahren erstellen Sie eine Seite, ein einfaches Formular mit drei enthält `input` Elemente (Vorname, Nachname und e-Mail-Adresse) und ein **senden** Schaltfläche. Wenn der Benutzer das Formular sendet, müssen Sie die Eingabe des Benutzers in einer Textdatei speichern.

1. Erstellen Sie einen neuen Ordner namens *App\_Daten*, wenn er nicht bereits vorhanden.
2. Erstellen Sie im Stammverzeichnis der Website eine neue Datei namens *UserData.cshtml*.
3. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    Das HTML-Markup erstellt das Formular mit den drei Feldern. In den Code, verwenden Sie die `IsPost` Eigenschaft, um zu bestimmen, ob die Seite übermittelt wurde, bevor Sie die Verarbeitung gestartet.

    Die erste Aufgabe ist die Benutzereingabe zu erhalten und auf Variablen zuweisen. Der Code verkettet die Werte der separaten Variablen dann in eine durch Trennzeichen getrennte Zeichenfolge, die dann in eine andere Variable gespeichert werden. Beachten Sie, dass das Komma als Trennzeichen eine Zeichenfolge in Anführungszeichen (","), da buchstäblich ein Komma in der großen Zeichenfolge einzubettenden, die Sie erstellen. Fügen Sie am Ende der Daten, die Sie zu verketten, `Environment.NewLine`. Dies fügt einen Zeilenumbruch (Zeilenumbruchzeichen). Was Sie mit der alle diese Verkettung erstellen, ist eine Zeichenfolge, die folgendermaßen aussieht:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Mit einem unsichtbaren Zeilenumbruch am Ende.)

    Klicken Sie dann erstellen Sie eine Variable (`dataFile`), der den Speicherort und Namen der Datei zum Speichern der Daten enthält. Festlegen des Speicherorts ist eine besondere Behandlung erforderlich. In Websites, ist es kein empfehlenswertes Verfahren, verweisen im Code in absolute Pfade wie *C:\Folder\File.txt* für Dateien auf dem Webserver. Wenn eine Website verschoben wird, wird ein absoluter Pfad falsch sein. Darüber hinaus nicht für eine gehostete Website (im Gegensatz zu auf dem lokalen Computer) wissen Sie in der Regel einmal, was der Pfad korrekte ist, wenn Sie den Code schreiben.

    Jedoch in einigen Fällen (z. B. Now, für das Schreiben einer Datei) benötigen Sie einen vollständigen Pfad. Die Lösung ist die Verwendung der `MapPath` Methode der `Server` Objekt. Dies gibt den vollständigen Pfad zu Ihrer Website zurück. Beim Abrufen des Pfads für das Stammverzeichnis der Website, die Benutzer Sie die `~` Operator (Represen die Website die virtuellen Stamm), `MapPath`. (Sie können auch einen Unterordnernamen, übergeben, z. B. *~/App\_Daten /*, um den Pfad für diesen Unterordner zu erhalten.) Sie können dann verketten Zusatzinformationen auf unabhängig von die Methode zurückgegeben wird, um einen vollständigen Pfad zu erstellen. In diesem Beispiel fügen Sie einen Dateinamen hinzu. (Weitere Informationen zum Arbeiten mit Datei- und Ordnerpfade in [Einführung in ASP.NET Web Pages-Programmierung verwenden die Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    Die Datei wird gespeichert, der *App\_Daten* Ordner. Dieser Ordner ist ein spezieller Ordner in ASP.NET, die zum Speichern von Datendateien verwendet wird, wie in beschrieben [Einführung in die Arbeit mit einer Datenbank in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=195209).

    Die `WriteAllText` Methode der `File` Objekt schreibt die Daten in der Datei. Diese Methode akzeptiert zwei Parameter: den Namen (mit Pfad), der die Datei zu schreiben und die tatsächlichen Daten schreiben. Beachten Sie, dass der Name des ersten Parameters einer `@` -Zeichen als Präfix. Dies weist ASP.NET an, die Sie ein ausführliches Zeichenfolgenliteral bereitgestellt und die Zeichen wie "/" nicht auf besondere Weise interpretiert werden soll. (Weitere Informationen finden Sie unter [Einführung in ASP.NET Web-Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > In der Reihenfolge für Ihren Code zum Speichern von Dateien in die *App\_Daten* Ordner der Anwendung benötigt Lese-/ Schreibzugriff Berechtigungen für diesen Ordner. Auf dem Entwicklungscomputer ist dies nicht in der Regel ein Problem. Wenn Sie Ihre Website eines Hostinganbieters Webserver veröffentlichen, müssen Sie jedoch die Berechtigungen explizit festzulegen. Wenn Sie diesen Code auf einem Hostinganbieter Server ausführen und Fehler auftreten, überprüfen Sie mit der hosting-Anbieter finden Sie heraus, wie Sie diese Berechtigungen festlegen.

- Führen Sie die Seite in einem Browser. 

    ![](working-with-files/_static/image1.jpg)
- Geben Sie Werte in die Felder aus, und klicken Sie dann auf **senden**.
- Schließen Sie den Browser.
- Zurück auf das Projekt, und aktualisieren Sie die Ansicht.
- Öffnen der *data.txt* Datei. Die Daten, die Sie in das Formular übermittelt ist in der Datei. 

    ![[Image]](working-with-files/_static/image2.jpg)
- Schließen der *data.txt* Datei.

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Daten an eine vorhandene Datei anfügen

Im vorherigen Beispiel, verwendet Sie `WriteAllText` eine Textdatei erstellen, die es gibt nur ein Teil der Daten hat. Wenn Sie die Methode erneut aufrufen und übergeben es mit den gleichen Namen haben, wird die vorhandene Datei vollständig überschrieben. Jedoch nach der Erstellung einer Datei möchten Sie häufig zum Hinzufügen von neuer Daten an das Ende der Datei. Sie können hierzu die `AppendAllText` -Methode der der `File` Objekt.

1. Erstellen Sie eine Kopie der Website der *UserData.cshtml* Datei, und benennen Sie *UserDataMultiple.cshtml*.
2. Ersetzen Sie den Codeblock vor dem öffnenden `<!DOCTYPE html>` Tag mit den folgenden Codeblock: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Dieser Code enthält eine Änderung aus dem vorherigen Beispiel. Anstelle von `WriteAllText`, verwendet `the AppendAllText` Methode. Die Methoden sind ähnlich, außer dass `AppendAllText` fügt die Daten an das Ende der Datei. Wie bei `WriteAllText`, `AppendAllText` erstellt die Datei aus, wenn er nicht bereits vorhanden.
3. Führen Sie die Seite in einem Browser.
4. Geben Sie Werte für die Felder aus, und klicken Sie dann auf **senden**.
5. Hinzufügen von weiteren Daten, und senden Sie das Formular erneut.
6. Zurück zu Ihrem Projekt, mit der rechten Maustaste in des Projektordners, und klicken Sie dann auf **aktualisieren**.
7. Öffnen der *data.txt* Datei. Es enthält jetzt die neuen Daten, die Sie gerade eingegeben haben. 

    ![[Image]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Lesen und Anzeigen von Daten aus einer Datei

Auch wenn Sie keine Daten in eine Textdatei schreiben möchten, müssen Sie möglicherweise manchmal zum Lesen von Daten aus einem. Zu diesem Zweck können Sie erneut die `File` Objekt. Können Sie die `File` Objekt, das jede Zeile einzeln zu lesen (getrennt durch einen Zeilenumbruch) oder für das Lesen der einzelnen Elements unabhängig davon, wie sie getrennt sind.

Dieses Verfahren zeigt, wie zum Lesen und Anzeigen der Daten, die Sie im vorherigen Beispiel erstellt haben.

1. Erstellen Sie im Stammverzeichnis der Website eine neue Datei namens *DisplayData.cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Der Code beginnt mit dem Lesen der Datei, die Sie im vorherigen Beispiel erstellt haben, in einer Variablen namens `userData`, verwenden diesen Methodenaufruf:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Der Code hierfür befindet sich innerhalb einer `if` Anweisung. Wenn Sie eine Datei zu lesen möchten, ist es eine gute Idee, verwenden Sie die `File.Exists` Methode, um zunächst überprüfen, ob die Datei verfügbar ist. Der Code überprüft außerdem, ob die Datei leer ist.

    Der Hauptteil der Seite enthält zwei `foreach` ineinander geschachtelten Schleifen. Die äußere `foreach` Schleife Ruft eine Zeile zu einem Zeitpunkt ab, aus der Datendatei. In diesem Fall werden die Zeilen durch Zeilenumbrüche in der Datei definiert &#8212; jedes Datenelement wird in einer eigenen Zeile. Die äußere Schleife erstellt ein neues Element (`<li>` Element) in eine geordnete Liste (`<ol>` Element).

    Die innere Schleife unterteilt jede Datenzeile in Elemente (Felder) durch ein Komma als Trennzeichen. (Basierend auf dem vorherigen Beispiel, das bedeutet, dass jede Zeile drei Felder enthält &#8212; der Vorname, Nachname und e-Mail-Adresse, ist jede durch ein Komma getrennt.) Die innere Schleife erstellt außerdem eine `<ul>` Liste und zeigt eine Liste-Element für jedes Feld in der Datenzeile.

    Der Code wird veranschaulicht, wie zwei Typen von Daten, ein Array verwenden und die `char` -Datentyp. Das Array ist erforderlich, da die `File.ReadAllLines` -Methode gibt die Daten als Bytearray zurück. Die `char` -Datentyp ist erforderlich, da die `Split` Methode gibt ein `array` in dem jedes Element des Typs ist `char`. (Weitere Informationen zu Arrays finden Sie unter [Einführung in ASP.NET Web-Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Führen Sie die Seite in einem Browser. Die eingegebenen Daten zu den vorangehenden Beispielen werden angezeigt. 

    ![[Image]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Anzeigen von Daten aus einer durch Trennzeichen getrennte Datei von Microsoft Excel**
> 
> Sie können Microsoft Excel verwenden, um die Daten in einer Tabelle als eine durch Trennzeichen getrennte Datei zu speichern (*CSV* Datei). Wenn Sie dies tun, wird die Datei im nur-Text nicht in Excel-Format gespeichert. Jede Zeile im Arbeitsblatt wird getrennt durch einen Zeilenumbruch in der Textdatei, und jedes Datenelement wird durch ein Komma getrennt. Sie können den Code, der im vorherigen Beispiel gezeigt verwenden, eine durch Trennzeichen getrennten Excel-Datei lesen, indem Sie einfach den Namen der Datendatei in Ihrem Code ändern.


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Löschen von Dateien

Um Dateien auf Ihrer Website zu löschen, können Sie die `File.Delete` Methode. Dieses Verfahren veranschaulicht, um ein Image löschen, Benutzern zu ermöglichen (*jpg* Datei) aus einer *Images* Ordner aus, wenn sie wissen, dass der Name der Datei.

> [!NOTE] 
> 
> **Wichtige** In eine Produktionswebsite, Sie in der Regel einschränken, wer darf hat, um die Daten zu ändern. Weitere Informationen über das Einrichten der Mitgliedschaft und zu Möglichkeiten, um Benutzern das Ausführen von Aufgaben auf der Website zu autorisieren, finden Sie unter [Hinzufügen von Sicherheit und die Mitgliedschaft in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Erstellen Sie auf der Website einen Unterordner namens *Images*.
2. Kopieren einer oder mehrerer *jpg* Dateien in die *Images* Ordner.
3. Erstellen Sie im Stammverzeichnis der Website, eine neue Datei namens *FileDelete.cshtml*.
4. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Diese Seite enthält ein Formular, in denen Benutzer den Namen der Bilddatei eingeben können. Sie geben Sie nicht die *jpg* Dateinamenerweiterung; durch Einschränken der Dateiname sieht, Sie Hilfe verhindert, dass Benutzer beliebige Dateien auf Ihrer Website wird gelöscht.

    Der Code liest den Dateinamen an, dass der Benutzer eingegeben hat, und dann einen vollständigen Pfad erstellt. Um den Pfad zu erstellen, der Code verwendet den Pfad der aktuellen Website (wie vom die `Server.MapPath` Methode), wird die *Images* Ordnernamen, den Namen, die der Benutzer bereitgestellt wurde und ".jpg" als Literalzeichenfolge.

    Zum Löschen der Datei, die der Code Ruft die `File.Delete` -Methode, und übergeben sie den vollständigen Pfad, den Sie gerade erstellt. Am Ende des Markups Zeigt Code für eine bestätigungsmeldung an, dass die Datei gelöscht wurde.
5. Führen Sie die Seite in einem Browser. 

    ![[Image]](working-with-files/_static/image5.jpg)
6. Geben Sie den Namen der Datei zu löschen, und klicken Sie dann auf **senden**. Wenn die Datei gelöscht wurde, wird der Name der Datei am unteren Rand der Seite angezeigt.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Damit können Benutzer Hochladen einer Datei

Die `FileUpload` Hilfsprogramm ermöglicht Benutzern das Hochladen von Dateien auf Ihrer Website. Das folgende Verfahren erfahren Sie, wie Sie Benutzer, die eine einzelne Datei hochladen können.

1. Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), wenn Sie zuvor hinzugefügt haben.
2. In der *App\_Daten* Ordner einen Ordner eine neue erstellen, und nennen Sie sie *UploadedFiles*.
3. Erstellen Sie im Stammverzeichnis, eine neue Datei namens *FileUpload.cshtml*.
4. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    Der Body-Teil der Seite verwendet die `FileUpload` Hilfsmethode zum Erstellen der Upload-Feld und die Schaltflächen, die Sie sicherlich kennen:

    ![[Image]](working-with-files/_static/image6.jpg)

    Die Eigenschaften, die Sie, für festlegen die `FileUpload` Helper angeben, dass Sie ein einzelnes Feld für die Datei zum Hochladen und, dass Sie die Schaltfläche "Senden" zum lesen möchten **hochladen**. (Sie werden später in diesem Artikel weitere Felder hinzufügen.)

    Wenn der Benutzer klickt **hochladen**, der Code am oberen Rand der Seite ruft die Datei ab und speichert sie. Die `Request` -Objekt, das Sie normalerweise verwenden, um das Abrufen von Werten aus Formularfeldern verfügt auch über eine `Files` Array, das die Datei (oder Dateien) enthält, die hochgeladen wurden. Erhalten Sie einzelne Dateien über bestimmten Positionen im Array &#8212; beispielsweise um die erste hochgeladene Datei zu erhalten, erhalten Sie `Request.Files[0]`, um der zweiten Datei abzurufen, erhalten Sie `Request.Files[1]`und so weiter. (Beachten Sie, dass bei der Programmierung die Zählung in der Regel bei Null beginnt.)

    Wenn Sie eine hochgeladene Datei abzurufen, platzieren Sie es in einer Variablen (hier `uploadedFile`), damit Sie diese bearbeiten können. Um den Namen der hochgeladenen Datei zu ermitteln, erhalten Sie nur die `FileName` Eigenschaft. Aber wenn der Benutzer eine Datei hochlädt, `FileName` enthält des Benutzers ursprünglichen Namen, die den vollständigen Pfad enthält. Es könnte folgendermaßen aussehen:

    *C:\Users\Public\Sample.txt*

    Sie sollten alle diese Pfadinformationen, jedoch nicht da ist der Pfad auf dem Computer des Benutzers, nicht für Ihren Server. Sie möchten nur den tatsächlichen Dateinamen (*"Sample.txt"*). Sie können sich nur auf die Datei aus einem Pfad entfernen, mit der `Path.GetFileName` -Methode wie folgt:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    Die `Path` Objekt ist ein Hilfsprogramm, das eine Reihe von Methoden wie folgt, die Sie verwenden können verfügt, um Pfade zu entfernen, Kombinieren von Pfaden und so weiter.

    Nachdem Sie den Namen der hochgeladenen Datei gelangt sind, können Sie erstellen einen neuen Pfad für die hochgeladene Datei auf Ihrer Website gespeichert werden soll. In diesem Fall Sie kombinieren `Server.MapPath`, die Ordnernamen (*App\_Daten/UploadedFiles*), und der neu entfernten Dateiname, erstellen Sie einen neuen Pfad. Rufen Sie anschließend der hochgeladenen Datei `SaveAs` Methode, um die Datei zu speichern.
5. Führen Sie die Seite in einem Browser. 

    ![[Image]](working-with-files/_static/image7.jpg)
6. Klicken Sie auf **Durchsuchen** und wählen Sie dann eine Datei zum Hochladen. 

    ![[Image]](working-with-files/_static/image8.jpg)

    Das Textfeld neben der **Durchsuchen** Schaltfläche enthält den Pfad und den Dateispeicherort.

    ![[Image]](working-with-files/_static/image9.jpg)
7. Klicken Sie auf **Hochladen**.
8. Klicken Sie auf der Website mit der rechten Maustaste in des Projektordners, und klicken Sie dann auf **aktualisieren**.
9. Öffnen der *UploadedFiles* Ordner. Die Datei, die Sie hochgeladen haben, ist im Ordner "". 

    ![[Image]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Ermöglicht Benutzern, mehrere Dateien hochladen

Im vorherigen Beispiel können Sie Benutzer, die eine Datei hochladen. Sie können jedoch die `FileUpload` Hilfsfunktion, um mehrere Dateien gleichzeitig hochladen. Dies ist nützlich für Szenarien wie das Hochladen von Fotos, in denen Hochladen einer Datei zu einem Zeitpunkt mühsam ist. (Informationen zum Hochladen von Fotos in [arbeiten mit Bildern in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202897).) Dieses Beispiel zeigt, wie Sie Benutzer, die zwei gleichzeitig hochladen können, obwohl Sie das gleiche Verfahren verwenden können, mehr hochladen.

1. Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.
2. Erstellen Sie eine neue Seite mit dem Namen *FileUploadMultiple.cshtml*.
3. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    In diesem Beispiel die `FileUpload` -Hilfsmethode in den Hauptteil der Seite ist so konfiguriert, dass Benutzer, die zwei Dateien hochladen können standardmäßig. Da `allowMoreFilesToBeAdded` nastaven NA hodnotu `true`, die-Hilfsmethode rendert einen Link zum Benutzer weitere Felder für Uploads hinzuzufügen:

    ![[Image]](working-with-files/_static/image11.jpg)

    Um die Dateien zu verarbeiten, die der Benutzer lädt, verwendet der Code derselben grundlegende Technik, die Sie im vorherigen Beispiel verwendet &#8212; erhalten Sie eine Datei aus `Request.Files` und speichern Sie sie. (Einschließlich der verschiedene Aspekte müssen Sie tun, um den richtigen Dateinamen und den Pfad zu erhalten.) Die Innovationen dieses Mal ist, dass der Benutzer möglicherweise auch das Hochladen mehrerer Dateien, und Sie nicht, viele wissen. Um herauszufinden, erhalten Sie `Request.Files.Count`.

    Mit dieser Nummer verfügen, können Sie eine Schleife über `Request.Files`, jede Datei im Gegenzug abrufen und speichern Sie sie. Wenn Sie eine bekannte Anzahl von Malen durch eine Auflistung in einer Schleife möchten, können Sie eine `for` Schleife wie folgt:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    Die Variable `i` ist ein temporärer Zähler, das Festlegen von 0 (null), auf den oberen Grenzwert gespeichert wird. In diesem Fall ist die obere Grenze der Anzahl der Dateien. Aber da der Zähler auf 0 (null), wie üblich für das zählen in Szenarien, in ASP.NET gestartet wird, die obere Grenze ist tatsächlich eine kleiner als die Anzahl der Dateien. (Wenn drei Dateien hochgeladen wurden, ist die Anzahl 0 (null) auf 2.)

    Die `uploadedCount` Variable zeilengesamtwerte alle Dateien, die erfolgreich hochgeladen und gespeichert werden. Dieser Code berücksichtigt die Möglichkeit, die eine erwartete Datei hochgeladen werden möglicherweise nicht.
4. Führen Sie die Seite in einem Browser. Der Browser zeigt die Seite und die zwei Upload-Felder.
5. Wählen Sie zwei Dateien hochladen.
6. Klicken Sie auf **fügen Sie eine andere Datei**. Die Seite zeigt ein neues Feld für den Upload an. 

    ![[Image]](working-with-files/_static/image12.jpg)
7. Klicken Sie auf **Hochladen**.
8. Klicken Sie auf der Website mit der rechten Maustaste in des Projektordners, und klicken Sie dann auf **aktualisieren**.
9. Öffnen der *UploadedFiles* Ordner, um die hochgeladenen Dateien anzuzeigen.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Arbeiten mit Bildern in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202897)

[Exportieren in eine CSV-Datei](https://msdn.microsoft.com/library/ms155919.aspx)
