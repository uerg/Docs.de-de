---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Unstrukturiertes Blob Storage (erstellen realer Cloud-Apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 1a56a76c9bf27fdd7afb090ca473ebeee4065fe2
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577417"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Unstrukturiertes Blob Storage (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen zu e-Book, finden Sie unter [im ersten Kapitel](introduction.md).


Im vorherigen Kapitel wir Partitionierungsschemas betrachtet und erläutert, wie die app Fix It Images im Azure Storage-Blob-Dienst und andere taskdaten in Azure SQL-Datenbank speichert. In diesem Kapitel haben wir eine tiefer gehende der Blob-Dienst und zeigen, wie es im Projektcode Fix It implementiert wird.

## <a name="what-is-blob-storage"></a>Was ist blobspeicher?

Der Azure Storage-Blob-Dienst bietet eine Möglichkeit zum Speichern von Dateien in der Cloud. Der Blob-Dienst verfügt über eine Reihe von Vorteilen gegenüber der Speicherung von Dateien in einem Dateisystem des lokalen Netzwerks:

- Es ist äußerst skalierbar. Einem einzelnen Speicherkonto kann speichern [Hunderte von Terabyte](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), und Sie können mehrere Speicherkonten. Einige der wichtigsten Azure-Kunden speichern Hunderte von Petabytes. Microsoft SkyDrive verwendet Blob Storage.
- Es ist dauerhaft. Jede Datei, die Sie in den Blob-Dienst speichern, werden automatisch gesichert.
- Es bietet eine hohe Verfügbarkeit. Die [SLA für Speicher](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) Versprechen von mindestens 99,9 % 99,99 % Betriebszeit, je nach ausgewählter Option Geo-Redundanz, die Sie auswählen.
- Es ist ein Platform-as-a-Service (PaaS)-Feature von Azure, was bedeutet, dass Sie nur zu speichern und Abrufen von Dateien, bezahlen nur für die tatsächliche Menge an Speicherplatz, die Sie verwenden, und Azure automatisch übernimmt das Einrichten und verwalten alle virtuellen Computer und Laufwerke, die erforderlich sind, für die -Dienst.
- Sie können den Blob-Dienst über eine REST-API oder mithilfe einer Programmiersprachen Ihrer API zugreifen. SDKs sind für .NET, Java, Ruby und andere Benutzer verfügbar.
- Wenn Sie eine Datei im Blob-Dienst speichern, können Sie problemlos es öffentlich über das Internet zur Verfügung.
- Dateien im Blob-Dienst können sie nur von autorisierten Benutzern zugegriffen können geschützt werden, oder Sie können angeben, dass temporäre Zugriffstoken, die sie an eine Person nur für einen begrenzten Zeitraum zur Verfügung.

Jedes Mal, wenn Sie eine app für Azure erstellen und Sie große Datenmengen zu speichern, die in einer lokalen Umgebung in Dateien – z. B. Bilder, Videos, also möchten PDF-Dokumente, Arbeitsblätter usw. – betrachten Sie den BLOB-Dienst.

## <a name="creating-a-storage-account"></a>Erstellen eines Speicherkontos

Informationen zum Einstieg in den Blob-Dienst erstellen Sie ein Speicherkonto in Azure. Klicken Sie im Portal auf **neu** -- **Datendienste** -- **Storage** -- **Schnellerfassung**, und geben Sie dann eine URL und einen Speicherort im Rechenzentrum. Datencenter-Speicherort muss Ihre Web-app identisch sein.

![Erstellen Sie ein Speicherkonto](unstructured-blob-storage/_static/image1.png)

Sie wählen Sie der primären Region, in dem Sie den Inhalt speichern möchten, und falls gewünscht die [Geo-Replikation](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) verwenden, erstellt Azure Replikate aller Daten in einem anderen Rechenzentrum in einer anderen Region des Landes. Angenommen, falls gewünscht kopiert im Westen der USA-Rechenzentrum, wenn Sie eine Datei speichern, die an das Rechenzentrum für den Westen der USA, jedoch im Hintergrund Azure auch darin es eines der anderen US-amerikanischen Rechenzentren. Wenn ein Notfall in einer Region des Lands, das auftritt, sind Ihre Daten immer noch sicher ist.

Azure wird nicht Daten über die geopolitischen Grenzen hinweg zu replizieren: Wenn der primäre Standort in den USA ist, werden Ihre Dateien nur in einer anderen Region innerhalb der USA; repliziert Wenn der primäre Standort Australien ist, werden Ihre Dateien nur in ein anderes Rechenzentrum in Australien repliziert.

Natürlich können Sie auch ein Speicherkonto erstellen durch Ausführen von Befehlen aus einem Skript, wie wir bereits gesehen haben. Hier ist ein Windows PowerShell-Befehl aus, um ein Speicherkonto zu erstellen:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Nachdem Sie ein Speicherkonto haben, können Sie sofort beginnen, Speichern von Dateien in den Blob-Dienst.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Verwenden von BLOB-Speicher in der Fix It-app

Der Fix It-app können Sie zum Hochladen von Fotos.

![Erstellen Sie einen Fix It-task](unstructured-blob-storage/_static/image2.png)

Beim Klicken auf **erstellen die FixIt**, die Anwendung lädt die angegebene Datei und speichert sie in den Blob-Dienst.

### <a name="set-up-the-blob-container"></a>Richten Sie den Blob-container

Um eine Datei im Blob-Dienst zu speichern, Sie müssen, eine *Container* in zu speichern. Ein Blob-Dienstcontainer entspricht einem Dateisystemordner. Die Umgebung Skripts, die wir in überprüft die [automatisieren alles Kapitel](automate-everything.md) Erstellen des Speicherkontos, aber sie können einen Container nicht erstellen. Also der Zweck der `CreateAndConfigure` -Methode der der `PhotoService` Klasse, die zum Erstellen eines Containers, wenn er nicht bereits vorhanden ist. Diese Methode wird aufgerufen, von der `Application_Start` -Methode in der *"Global.asax"*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

Des speicherkontonamens und kontoschlüssels werden gespeichert, der `appSettings` Auflistung von der *"Web.config"* Datei, und erstellen Sie code in die `StorageUtils.StorageAccount` Methode verwendet die Werte, um eine Verbindungszeichenfolge erstellen und eine Verbindung herzustellen:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

Die `CreateAndConfigureAsync` Methode erstellt dann ein Objekt, das den Blob-Dienst darstellt, und ein Objekt, das einen Container (Ordner) darstellt, mit dem Namen "Bilder" im Blob-Dienst:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Ein Container lautet "Images" ist nicht noch – die "true", beim ersten der app für ein neues Speicherkonto Ausführen – der Code erstellt den Container und legt die Berechtigungen, um es öffentlich machen sein vorhanden. (Standardmäßig neue Blob-Container privat sind und nur für Benutzer, die die Berechtigung zum Zugreifen auf das Speicherkonto zugegriffen werden.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Store der hochgeladenen Fotos im Blob-Speicher

Hochladen und speichern Sie die Image-Datei, die app verwendet eine `IPhotoService` Schnittstelle und eine Implementierung der Schnittstelle in der `PhotoService` Klasse. Die *PhotoService.cs* Datei enthält alle Teil des Codes in der Fix It-app, die mit dem Blob-Dienst kommuniziert.

Die folgende MVC-Controller-Methode wird aufgerufen, wenn der Benutzer klickt **erstellen die FixIt**. In diesem Code `photoService` bezieht sich auf einer Instanz von der `PhotoService` -Klasse, und `fixittask` bezieht sich auf einer Instanz von der `FixItTask` Entitätsklasse, die Daten für eine neue Aufgabe gespeichert.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

Die `UploadPhotoAsync` -Methode in der die `PhotoService` Klasse speichert die hochgeladene Datei im Blob-Dienst, und gibt eine URL, die auf das neue Blob verweist.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Wie in der `CreateAndConfigure` -Methode, der Code eine Verbindung mit dem Storage-Konto her und erstellt ein Objekt, das den "images" Blob-Container darstellt, außer es wird hier vorausgesetzt der Container bereits vorhanden ist.

Anschließend erstellt es einen eindeutigen Bezeichner für das Image durch die Verkettung eines neuen GUID-Wert, mit der Dateierweiterung hochgeladen werden sollen:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Klicken Sie dann der Code das Blob-Container-Objekt und den neuen eindeutigen Bezeichner verwendet, um ein Blob-Objekt zu erstellen, legt ein Attribut für das Objekt, das angibt, welche Art von Datei dabei handelt es sich, klicken Sie dann das Blob-Objekt verwendet, die Datei im Blob-Speicher gespeichert.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Schließlich ruft es eine URL, die das Blob verweist. Diese URL wird in der Datenbank gespeichert und kann in Fix It-Webseiten verwendet werden, um das hochgeladene Bild anzuzeigen.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Diese URL wird als eine der Spalten der Tabelle FixItTask in der Datenbank gespeichert.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Nur die URL in der Datenbank, und Bilder im Blob-Speicher bleibt die Fix It-app die Datenbank klein, skalierbare und kostengünstige, während die Images gespeichert sind, in dem Speicher günstige und kann einen verarbeiten Kostenfaktor ist. Ein Speicherkonto kann Hunderte von Terabyte Fix It Fotos speichern, und Sie bezahlen nur für die tatsächliche Nutzung. Sie können also beginnen klein zahlenden 9 Cent für das erste Gigabyte, und fügen weitere Images für Pennies pro zusätzliches Gigabyte.

### <a name="display-the-uploaded-file"></a>Die hochgeladene Datei anzeigen

Die Fix It-Anwendung zeigt die hochgeladenen Image-Datei, Anzeigen von Details einer Aufgabe.

![Beheben sie den Aufgabendetails mit Foto](unstructured-blob-storage/_static/image3.png)

Um das Bild anzuzeigen, hat die MVC-Ansicht zu tun, ist enthalten die `PhotoUrl` Wert in der HTML-Code an den Browser gesendet. Der Webserver und die Datenbank nicht Zyklen zur Anzeige des Bilds verwenden, sie nur ein paar Bytes versorgt werden, um der Bild-URL. Im folgenden Razor-Code `Model` bezieht sich auf eine Instanz der `FixItTask` Entitätsklasse.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Wenn Sie den HTML-Code der Seite, die anzeigt betrachten, sehen Sie die URL direkt auf das Bild in blobspeicher, könnte folgendermaßen aussehen:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Zusammenfassung

Sie haben gesehen, wie die Fix It-app Bilder im Blob-Dienst und nur über die Bild-URLs in der SQL-Datenbank speichert. Blob-Diensts behält die SQL-Datenbank weit weniger umfangreich als andernfalls wäre, es möglich, zentrales hochskalieren auf eine nahezu unbegrenzte Anzahl von Aufgaben macht und ausgeführt werden können, ohne viel Code schreiben zu müssen.

Sie können Hunderte von Terabyte in einem Speicherkonto, und die Speicherkosten ist weniger aufwendig als das SQL-Datenbankspeicher, beginnend bei ca. 3 Cent pro GB pro Monat plus eine Gebühr von kleinen Transaktion. Und denken Sie daran, die Sie nicht bezahlen für die maximale Kapazität, jedoch nur für die Menge, die Sie tatsächlich speichern Ihre app ist bereit zum Skalieren, aber Sie sind nicht für alle zusätzlichen Kapazität bezahlen.

In der [im nächsten Kapitel](design-to-survive-failures.md) beschäftigen wir uns die Wichtigkeit, machen Sie eine Cloud-app kann einen Fehler ordnungsgemäß zu verarbeiten.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen:

- [Eine Einführung in Azure BLOB Storage](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog von Mike Wood.
- [Gewusst wie: Verwenden des Azure-Blob-Speicherdiensts in .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Offizielle Dokumentation auf der Website MicrosoftAzure.com. Eine kurze Einführung in blob Storage, gefolgt von Codebeispiele zur Veranschaulichung Herstellen einer Verbindung mit Blob-Speicher, Erstellen von Containern, hochladen und Herunterladen von Blobs usw.
- [FailSafe: Erstellen von skalierbaren, robusten Clouddiensten](https://channel9.msdn.com/Series/FailSafe). Videoreihe neun-Teil von Marc Mercuri, Ulrich Homann und Mark Simms. Bietet allgemeine Konzepte und architektonischen Prinzipien auf eine Weise sehr zugegriffen werden kann und interessante Geschichten, die von Microsoft Customer Advisory Teams (CAT) die Erfahrung für tatsächliche Kunden gezeichnet werden. Eine Erläuterung der Azure Storage-Dienst und -Blobs finden Sie in der Folge 5 35:13 ab.
- [Microsoft Patterns and Practices - Leitfaden zur Azure](https://msdn.microsoft.com/library/dn568099.aspx). Muster "Valet-Schlüssel für finden Sie unter".

> [!div class="step-by-step"]
> [Zurück](data-partitioning-strategies.md)
> [Weiter](design-to-survive-failures.md)
