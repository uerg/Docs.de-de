---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Unstrukturierte Blob-Speicher (Building Real-World Cloud Apps with Azure) | Microsoft Docs
author: MikeWasson
description: "Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 6cb77e8ef301c2eeef7df3e391e14f4e2c0364e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Unstrukturierte Blob-Speicher (Building Real-World Cloud Apps with Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Weitere Informationen zu e-Book herunterladen, finden Sie unter [im Kapitel über das erste](introduction.md).


Im vorherigen Kapitel wir Partitionierungsschemas betrachtet und erläutert, wie die app zu beheben Bilder in der Azure-Speicher-Blob-Dienst und andere Daten in Azure SQL-Datenbank speichert. In diesem Kapitel werden tiefer gehende der Blob-Dienst und anzeigen, wie es in korrigieren Projektcode implementiert wird.

## <a name="what-is-blob-storage"></a>Was ist die Blob-Speicher?

Der Azure-Speicher-Blob-Dienst bietet eine Möglichkeit zum Speichern von Dateien in der Cloud. Der Blob-Dienst verfügt über eine Reihe von Vorteilen gegenüber der Speicherung von Dateien in einem lokalen Netzwerk-Dateisystem:

- Es ist äußerst skalierbar. Ein einzelnes Speicherkonto speichern kann [Hunderte von Terabyte](https://msdn.microsoft.com/en-us/library/windowsazure/dn249410.aspx), und Sie können mehrere Speicherkonten haben. Einige der wichtigsten Kunden von Azure speichern Hunderte von Petabytes. Microsoft SkyDrive verwendet Blob-Speicher.
- Es ist dauerhaft. Jede Datei, die Sie in den Blob-Dienst speichern wird automatisch gesichert.
- Es bietet eine hohe Verfügbarkeit. Die [SLA zum Speicher](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) Zusagen 99,9 % oder 99,99 % Verfügbarkeit, je nach ausgewählter Option Geo-Redundanz gewünscht.
- Es ist eine Platform-as-a-Service (PaaS)-Funktion von Azure, was bedeutet, dass Sie nur zu speichern und Abrufen von Dateien, die nur für die tatsächliche Menge an Speicherplatz, die Sie verwenden, und Azure automatisch übernimmt das Einrichten und verwalten alle virtuellen Computer und Laufwerke, die erforderlich sind, für die -Dienst.
- Sie können den Blob-Dienst über eine REST-API oder mithilfe einen Programmiersprache Ihrer Wahl-API zugreifen. SDKs sind für .NET, Java und Ruby verfügbar.
- Wenn Sie eine Datei im Blob-Dienst speichern, können Sie problemlos öffentlich verfügbaren über das Internet erleichtern.
- Daher können Dateien im Blob-Dienst können nur durch autorisierte Benutzer zugegriffen gesichert werden, oder Sie können temporäre Zugriffstoken, die sie nur für einen begrenzten Zeitraum für eine Person zur Verfügung gestellt bereitstellen.

Jedes Mal, wenn Sie eine app für Azure erstellen und Sie möchten eine große Datenmenge zu speichern, die in einer lokalen Umgebung in Dateien – z. B. Bildern, Videos, gehen würde berücksichtigen PDF-Dateien, Kalkulationstabellen usw. – Sie die Blob-Dienst.

## <a name="creating-a-storage-account"></a>Erstellen eines Speicherkontos

Um die ersten Schritte mit Blob-Dienst erstellen Sie ein Speicherkonto in Azure. Klicken Sie im Portal auf **neu** -- **Datendienste** -- **Speicher** -- **Schnellerfassung**, und geben dann eine URL und einen Speicherort im Rechenzentrum. Der rechenzentrumsstandort darf es sich um Ihre Web-app identisch sein.

![Erstellen Sie ein Speicherkonto](unstructured-blob-storage/_static/image1.png)

Möchten Sie den Inhalt zu speichern, und falls gewünscht, wählen Sie die primären Region der [geografische Replikation](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) auswählen, erstellt Azure Replikate aller Daten in einem anderen Rechenzentrum in einer anderen Region des Landes. Angenommen, falls gewünscht kopiert im Westen der USA-Rechenzentrum, wenn Sie zu eine Datei speichern, die im Westen der USA-Rechenzentrum, aber im Hintergrund Azure auch darin auf einen anderen US-Rechenzentren. Fall ein notfalls in einer Region des Landes sind Ihre Daten immer noch sicher ist.

Azure wird nicht replizieren von Daten über geografische politische hinweg: Wenn Ihre primäre Standort in den USA ist, Ihre Dateien werden nur repliziert, in einer anderen Region innerhalb der USA; Wenn Ihre primäre Speicherort Australien ist, werden Ihre Dateien nur zu einem anderen Rechenzentrum in Australien repliziert.

Natürlich können Sie auch ein Speicherkonto erstellen durch Ausführen von Befehlen aus einem Skript bereits zuvor gesehen. Hier ist ein Windows PowerShell-Befehl aus, um ein Speicherkonto erstellen:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Nachdem Sie ein Speicherkonto haben, können Sie sofort das Speichern von Dateien in den Blob-Dienst.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Mit dem Blob-Speicher in der app zu beheben

Die app zu beheben, können Sie Fotos hochladen.

![Erstellen einer Aufgabe zu beheben](unstructured-blob-storage/_static/image2.png)

Beim Klicken auf **erstellen die FixIt**, die Anwendung lädt die angegebene Datei hoch und speichert sie in den Blob-Dienst.

### <a name="set-up-the-blob-container"></a>Richten Sie den Blob-container

Um eine Datei im Blob-Dienst zu speichern, Sie müssen, eine *Container* in speichern. Ein Blob-Dienstcontainer entspricht einem Dateisystemordner. Die Umgebung Erstellungsskripts, die wir in überprüft die [automatisieren alles Kapitel](automate-everything.md) das Speicherkonto zu erstellen, aber nicht können, erstellen Sie einen Container. Daher den Zweck der `CreateAndConfigure` Methode der `PhotoService` Klasse um einen Container zu erstellen, wenn er nicht bereits vorhanden ist. Diese Methode wird aufgerufen, aus der `Application_Start` Methode im *"Global.asax"*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

Die Speicher-Konto und den Zugriffsschlüssel werden gespeichert, der `appSettings` Auflistung von der *"Web.config"* -Datei ein, und im code der `StorageUtils.StorageAccount` Methode verwendet diese Werte, um eine Verbindungszeichenfolge erstellen und eine Verbindung herzustellen:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

Die `CreateAndConfigureAsync` -Methode erstellt dann ein Objekt, das den Blob-Dienst darstellt, und ein Objekt, das einen Container (Ordner) darstellt, mit dem Namen "Bilder" im Blob-Dienst:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Wenn ein Container mit der Bezeichnung "Images" existiert noch--nicht der ist "true", beim ersten der app für ein neues Speicherkonto Ausführen – der Code erstellt den Container und legt die Berechtigungen, um diese öffentliches Profil festzulegen. (Standardmäßig neue Blob-Container privat sind und nur für Benutzer mit Zugriffsberechtigung für das Speicherkonto zugegriffen werden.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Das hochgeladene Foto im Blob-Speicher zu speichern.

Hochladen und speichern die Bilddatei, die app verwendet einen `IPhotoService` Schnittstelle und eine Implementierung der Schnittstelle in der `PhotoService` Klasse. Die *PhotoService.cs* -Datei enthält alle des Codes in der korrigieren-app, die mit dem Blob-Dienst kommuniziert.

Die folgende MVC-Controller-Methode wird aufgerufen, wenn der Benutzer klickt auf **erstellen die FixIt**. In diesem Code `photoService` bezieht sich auf einer Instanz von der `PhotoService` -Klasse, und `fixittask` bezieht sich auf einer Instanz von der `FixItTask` Entitätsklasse, die Daten für eine neue Aufgabe gespeichert.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

Die `UploadPhotoAsync` Methode in der `PhotoService` Klasse speichert die hochgeladene Datei im Blob-Dienst, und gibt eine URL, die auf das neue Blob verweist.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Wie in der `CreateAndConfigure` -Methode, der Code eine Verbindung mit dem Speicherkonto her und erstellt ein Objekt, das "images" Blob-Container darstellt, außer es wird hier vorausgesetzt der Container bereits vorhanden ist.

Anschließend erstellt es einen eindeutigen Bezeichner für das Image durch einen neuen GUID-Wert mit der Dateierweiterung verketten hochgeladen werden sollen:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Klicken Sie dann der Code der Blob-Container-Objekt und die neue eindeutige ID zum Erstellen eines Blob-Objekts verwendet, wird ein Attribut für dieses Objekt, der angibt, welche Art von Datei es ist, und verwendet dann die Blob-Objekt, um die Datei im Blob-Speicher zu speichern.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Zum Schluss wird eine URL, die auf das Blob abgerufen. Diese URL wird in der Datenbank gespeichert werden und kann in Korrigieren von Webseiten zur Anzeige des Bilds hochgeladene verwendet werden.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Diese URL wird als eine der Spalten der Tabelle FixItTask in der Datenbank gespeichert.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Nur die URL in der Datenbank, und Bilder im Blob-Speicher behält die app korrigieren die Datenbank klein, skalierbare und kostengünstige, während die Bilder gespeichert sind, in dem Speicher billig und Verarbeiten von Kostenfaktor ist. Ein Speicherkonto kann Hunderte von Terabyte korrigieren Fotos speichern, und Sie bezahlen nur für die tatsächliche Verwendung. Damit Sie kleine zahlenden 9 Cent für das erste Gigabyte beginnen und weitere Bilder für Cents Abschneiden pro zusätzliche Gigabyte hinzufügen können.

### <a name="display-the-uploaded-file"></a>Zeigen Sie die hochgeladene Datei

Die korrigieren-Anwendung zeigt die hochgeladene Datei beim Anzeigen der Details für eine Aufgabe.

![So beheben sie Aufgabendetails mit Foto](unstructured-blob-storage/_static/image3.png)

Um das Bild angezeigt wird, muss das MVC-Ansicht nur, lediglich enthalten die `PhotoUrl` Wert im HTML-Code an den Browser gesendet. Die Webserver und die Datenbank sind nicht mit dem Zyklen zur Anzeige des Bilds, sie nur wenige Bytes der Bild-URL bedient werden. Im folgenden Razor-Code `Model` bezieht sich auf einer Instanz von der `FixItTask` Entitätsklasse.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Wenn Sie HTML-Code von der Seite, die anzeigt betrachten, sehen Sie die URL verweist direkt auf das Bild im Blob-Speicher, etwa wie folgt:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Zusammenfassung

Sie haben gesehen, wie die app zu beheben Bilder im Blob-Dienst und nur Bild-URLs in der SQL-Datenbank speichert. Mithilfe des Blob-Diensts behält die SQL-Datenbank weit weniger umfangreich als andernfalls wäre, es möglich, skaliert und an eine nahezu unbegrenzte Anzahl von Aufgaben macht sind und ausgeführt werden können, ohne viel Code schreiben zu müssen.

Sie können Hunderte von Terabyte in einem Speicherkonto, und viel Speicher beansprucht ist wesentlich kostengünstiger als SQL-Datenbankspeicher, beginnend ab ca. 3 US-Cent pro GB pro Monat sowie Gebühren kleinen Transaktion. Und denken Sie daran, die Sie nicht bezahlen, für die maximale Kapazität jedoch nur für die Menge, die Sie tatsächlich speichern, damit Ihre app zum Skalieren bereit ist, jedoch sind Sie nicht für diesen zusätzliche Kapazität Zahlen, die beibehalten.

In der [nächsten Kapitels](design-to-survive-failures.md) sprechen wir über die Wichtigkeit versteht man eine Cloud-app-Fehler ordnungsgemäß verarbeiten kann.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie unter den folgenden Ressourcen:

- [Eine Einführung in Azure BLOB-Speicher](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog von Mike Holz.
- [Gewusst wie: Verwenden des Azure-Blob-Speicherdiensts in .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Offizieller Dokumentation auf der Website MicrosoftAzure.com. Eine kurze Einführung in den blob-Speicher, gefolgt von Codebeispielen, die Darstellung des Blob-Speicher verbinden Container erstellen, hochladen und Herunterladen von Blobs usw.
- [FailSafe: Erstellen von skalierbaren, robusten Cloud-Dienste](https://channel9.msdn.com/Series/FailSafe). Videoreihe Marc Mercuri, Ulrich Homann und Mark Simms neun Teil. Bietet allgemeine Konzepte und Architekturprinzipien auf eine Weise zugegriffen werden kann, und interessante Storys, die von Microsoft Customer Advisory Team (CAT) anstelle von Erfahrungen mit Kunden gezeichnet. Eine Erläuterung der Azure-Speicherdienst und Blobs finden Sie in der Folge 5 35:13 ab.
- [Microsoft Patterns and Practices - Azure-Leitfaden](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Siehe Valet Schlüssel Muster.

>[!div class="step-by-step"]
[Zurück](data-partitioning-strategies.md)
[Weiter](design-to-survive-failures.md)
