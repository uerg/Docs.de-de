---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: 'Iteration #1 – erstellen Sie die Anwendung (c#) | Microsoft Docs'
author: microsoft
description: 'In der ersten Iteration erstellen wir die Kontakt-Manager in der einfachsten möglich. Wir fügen die Unterstützung für die grundlegenden Datenbankvorgängen: erstellen, lesen, aktualisieren und d...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: 30f626511164363fea2195a05e73aeee5764933b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877384"
---
<a name="iteration-1--create-the-application-c"></a>Iteration #1 – erstellen Sie die Anwendung (c#)
====================
durch [Microsoft](https://github.com/microsoft)

[Herunterladen von Code](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> In der ersten Iteration erstellen wir die Kontakt-Manager in der einfachsten möglich. Wir fügen die Unterstützung für die grundlegenden Datenbankvorgängen: erstellen, lesen, aktualisieren und löschen (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Erstellen einer Kontaktverwaltung ASP.NET MVC-Anwendung (VB)

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

In diesem ersten Iteration erstellen wir die einfache Anwendung. Das Ziel ist die Kontakt-Manager auf die schnellste und einfachste Möglichkeit, die mögliche erstellt wird. In späteren Iterationen wir den Entwurf der Anwendung verbessert werden.

Die Kontakt-Manager-Anwendung ist eine einfache Datenbank basierende Anwendung. Sie können die Anwendung zum Erstellen neuer Kontakte, existierenden Kontakte bearbeiten und Löschen von Kontakten verwenden.

In dieser Iteration ausführen wir die folgenden Schritte:

1. ASP.NET MVC-Anwendung
2. Erstellen Sie eine Datenbank zum Speichern von unseren Kontakte
3. Generieren Sie eine Modellklasse für die Datenbank mit dem Microsoft Entity Framework
4. Erstellen Sie eine Controlleraktion und die Sicht, die wir alle Kontakte in der Datenbank anzeigen können
5. Erstellen Sie Controlleraktionen und eine Sicht, die es uns zum Erstellen eines neuen Kontakts in der Datenbank
6. Erstellen Sie Controlleraktionen und eine Sicht, die es uns zum Bearbeiten eines vorhandenen Kontakts in der Datenbank
7. Erstellen Sie Controlleraktionen und eine Sicht, die wir einen vorhandenen Kontakt in der Datenbank löschen können

## <a name="software-prerequisites"></a>Erforderliche Software

In ASP.NET MVC-Anwendungen benötigen Sie Visual Studio 2008 oder Visual Web Developer 2008. auf Ihrem Computer (Visual Web Developer ist eine kostenlose Version von Visual Studio, die nicht alle der erweiterten Funktionen von Visual Studio enthalten ist) installiert. Sie können entweder die Testversion von Visual Studio 2008 oder Visual Web Developer von der folgenden Adresse herunterladen:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> ASP.NET MVC-Anwendungen mit Visual Web Developer müssen Sie Visual Web Developer Service Pack 1 installiert haben. Ohne Service Pack 1 erstellen keine Webanwendungsprojekte.


ASP.NET MVC-Framework. Sie können das ASP.NET MVC-Framework unter folgender Adresse herunterladen:

[https://www.asp.net/mvc](../../../index.md)

In diesem Lernprogramm verwenden wir das Entity Framework von Microsoft auf eine Datenbank zuzugreifen. Das Entity Framework ist im Lieferumfang von .NET Framework 3.5 Service Pack 1. Sie können dieses Servicepack von folgendem Speicherort herunterladen:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;Displaylang = En](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Als Alternative zum Ausführen der einzelnen diese Downloads nacheinander können Sie den Webplattform-Installer (Web PI) nutzen. Sie können die Web-PI aus der folgenden Adresse herunterladen:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>ASP.NET MVC Project

ASP.NET MVC-Webanwendungsprojekt. Starten Sie Visual Studio, und wählen Sie die Menüoption **Datei, neues Projekt**. Die **neues Projekt** Dialogfeld wird angezeigt (siehe Abbildung 1). Wählen Sie die **Web** Projekttyp und die **ASP.NET MVC-Webanwendung** Vorlage. Nennen Sie das neue Projekt *ContactManager* , und klicken Sie auf die Schaltfläche "OK".


Stellen Sie sicher, dass Sie .NET Framework 3.5 in der Dropdownliste am oberen ausgewählt haben rechts von der **neues Projekt** Dialogfeld. Andernfalls wird die ASP.NET MVC-Webanwendung Vorlage/t gewonnen angezeigt.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**Abbildung 01**: Dialogfeld "das neue Projekt" ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image2.png))


ASP.NET MVC-Anwendung, die **Komponententestprojekt erstellen** Dialogfeld wird angezeigt. Sie können dieses Dialogfeld verwenden, um anzugeben, dass Sie erstellen und ein Komponententestprojekt zur Projektmappe hinzufügen, wenn Sie die ASP.NET MVC-Anwendung erstellen möchten. Obwohl wir t/gewonnen werden. erstellen Komponententests in dieser Iteration, sollten Sie die Option auswählen **Ja, Komponententestprojekt erstellen** da wir planen die Komponententests in einer späteren Iteration hinzuzufügen. Ein Testprojekt hinzuzufügen, bei der Erstellung eines neuen ASP.NET MVC-Projekts ist sehr viel einfacher als ein Testprojekt hinzuzufügen, nachdem das ASP.NET MVC-Projekt erstellt wurde.

> [!NOTE] 
> 
> Da Visual Web Developer Testprojekte nicht unterstützt wird, erhalten Sie bei Verwendung von Visual Web Developer nicht das Dialogfeld Komponententestprojekt erstellen.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**Abbildung 02**: das Komponententestprojekt erstellen Dialogfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image4.png))


ASP.NET MVC-Anwendung wird im Projektmappen-Explorer von Visual Studio-Fenster angezeigt (siehe Abbildung 3). Wenn Sie Ich möchte den Projektmappen-Explorer-Fenster angezeigt, wird Sie dieses Fenster öffnen können, indem Sie im Menü die Option **Ansicht, Projektmappen-Explorer**. Beachten Sie, dass die Projektmappe zwei Projekte enthält: das ASP.NET MVC-Projekt und das Testprojekt. Das ASP.NET MVC-Projekt ist ContactManager und das Testprojekt ContactManager.Tests benannt ist.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**Abbildung 03**: der Projektmappen-Explorer-Fenster ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Löschen die Beispiel-Projektdateien

Die ASP.NET MVC-Projektvorlage enthält Beispieldateien für Controller und Ansichten. Bevor Sie eine neue ASP.NET MVC-Anwendung erstellen, sollten Sie diese Dateien löschen. Sie können die Dateien und Ordner im Projektmappen-Explorer löschen, indem Sie eine Datei oder einen Ordner mit der rechten Maustaste, und wählen die Menüoption **löschen**.

Sie müssen die folgenden Dateien aus dem ASP.NET MVC-Projekt zu löschen:

- \Controllers\HomeController.cs

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

Und Sie müssen die folgende Datei aus dem Testprojekt zu löschen:

\Controllers\HomeControllerTest.cs

## <a name="creating-the-database"></a>Erstellen der Datenbank

Die Kontakt-Manager-Anwendung ist eine Datenbank-driven Webanwendung. Wir verwenden eine Datenbank, um die Kontaktinformationen zu speichern.

Das ASP.NET-MVC-Framework bei jeder modernen Datenbank, einschließlich Microsoft SQL Server, Oracle, MySQL und IBM DB2-Datenbanken. In diesem Lernprogramm verwenden wir eine Microsoft SQL Server-Datenbank. Wenn Sie Visual Studio installieren, erhalten Sie mit der Option für die Installation von Microsoft SQL Server Express die ist eine kostenlose Version von Microsoft SQL Server-Datenbank.

Erstellen einer neuen Datenbank durch Rechtsklick auf die App\_Datenordner im Projektmappen-Explorer angezeigt und im Menü die Option **hinzufügen, neue Element**. In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **Daten** Kategorie und die **SQL Server-Datenbank** Vorlage (siehe Abbildung 4). Benennen Sie die neue Datenbank ContactManagerDB.mdf, und klicken Sie auf die Schaltfläche "OK".


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**Abbildung 04**: Erstellen einer neuen Microsoft SQL Server Express-Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image8.png))


Nachdem Sie die neue Datenbank erstellt haben, wird die Datenbank in der App\_Datenordner im Projektmappen-Explorer-Fenster. Doppelklicken Sie auf die ContactManager.mdf-Datei, um das Server-Explorer-Fenster öffnen und Herstellen einer Verbindung mit der Datenbank.

> [!NOTE] 
> 
> Das Server-Explorer-Fenster wird das Datenbank-Explorer-Fenster im Fall von Microsoft Visual Web Developer aufgerufen.


Das Server-Explorer-Fenster können neue Datenbankobjekte wie Tabellen, Sichten, Trigger und gespeicherte Prozeduren erstellt. Mit der rechten Maustaste in des Tabellen-Ordners, und wählen Sie die Menüoption **neue Tabelle hinzufügen**. Die Datenbank-Tabellen-Designer wird angezeigt (siehe Abbildung 5).


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**Abbildung 05**: die Datenbank-Tabellen-Designer ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image10.png))


Wir benötigen eine Tabelle erstellen, die folgenden Spalten enthält:

<a id="0.1_table01"></a>


| **Spaltenname** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | int | False |
| FirstName | Nvarchar(50) | False |
| LastName | Nvarchar(50) | False |
| Telefon | Nvarchar(50) | False |
| E-Mail | nvarchar(255) | False |


Die erste Spalte, die Id-Spalte ist ein Sonderfall. Sie müssen die Id-Spalte als eine Identity-Spalte und eine Primary Key-Spalte zu markieren. Sie angeben, dass eine Spalte eine Identitätsspalte ist, durch Erweitern von Eigenschaften der Spalte (Suchen Sie am unteren Rand der Abbildung 6) und Durchführen eines Bildlaufs auf die Spezifikation der Spaltenidentität-Eigenschaft. Legen Sie die **(ist Identity)** -Eigenschaft auf den Wert **Ja**.

Markieren Sie eine Spalte als eine Primary Key-Spalte, indem Sie die Spalte auswählen und auf die Schaltfläche mit dem Symbol eines Schlüssels. Nachdem eine Spalte als eine Primary Key-Spalte markiert ist, wird ein Symbol eines Schlüssels neben der Spalte angezeigt (siehe Abbildung 6).

Nachdem Sie die Tabelle erstellt haben, klicken Sie auf die Schaltfläche "Speichern" (die Schaltfläche mit einem Symbol einer Diskette), um die neue Tabelle zu speichern. Geben Sie den Namen der neuen Tabelle *Kontakte*.

Nach Ende die Datenbanktabelle Kontakte erstellen sollten Sie einige Datensätze in der Tabelle hinzufügen. Mit der rechten Maustaste in der Tabelle "Kontakte" im Server-Explorer-Fenster, und wählen Sie die Menüoption **Tabellendaten anzeigen**. Geben Sie einen oder mehrere Kontakte im Raster, das angezeigt wird.

## <a name="creating-the-data-model"></a>Erstellen das Datenmodell

ASP.NET MVC-Anwendung besteht der Modelle, Ansichten und Controllern aus. Zunächst erstellen eine Modellklasse, die die Contacts-Tabelle darstellt, die im vorherigen Abschnitt erstellt wurde.

In diesem Lernprogramm verwenden wir das Entity Framework von Microsoft zum automatischen Generieren von eine Modellklasse aus der Datenbank.

> [!NOTE] 
> 
> ASP.NET MVC-Framework ist nicht an Microsoft Entity Framework in keiner Weise gebunden. Sie können ASP.NET MVC mit alternativen datenzugriffstechnologien einschließlich NHibernate, LINQ to SQL und ADO.NET verwenden.


Führen Sie diese Schritte aus, um die Datenklassen für das Modell zu erstellen:

1. Mit der rechten Maustaste im Projektmappen-Explorer-Fenster Ordner Models, und wählen Sie **hinzufügen, neue Element**. Die **neues Element hinzufügen** Dialogfeld wird angezeigt (siehe Abbildung 6).
2. Wählen Sie die **Daten** Kategorie und die **ADO.NET Entity Data Model** Vorlage. Benennen Sie Ihr Datenmodell *ContactManagerModel.edmx* , und klicken Sie auf die **hinzufügen** Schaltfläche. Der Assistent für Entity Data Model wird angezeigt (siehe Abbildung 7).
3. In der **Modellinhalte** Schritt wählen **aus Datenbank generieren** (siehe Abbildung 7).
4. In der **wählen Sie Ihre Datenverbindung** Schritt, wählen Sie die ContactManagerDB.mdf-Datenbank aus, und geben Sie den Namen *ContactManagerDBEntities* für die Entitätsverbindungseinstellungen (siehe Abbildung 8).
5. In der **Datenbankobjekte auswählen** Schritt aktivieren Sie das Kontrollkästchen mit der Bezeichnung Tabellen (siehe Abbildung 9). Das Datenmodell enthält alle Tabellen in der Datenbank (es ist nur eine der Contacts-Tabelle vorhanden). Geben Sie den Namespace *Modelle*. Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**Abbildung 06**: Dialogfeld das neue Element in einer hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image12.png))


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**Abbildung 07**: Modellinhalt auswählen ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image14.png))


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**Abbildung 08**: Wählen Sie Ihre Datenverbindung ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image16.png))


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**Abbildung 09**: Wählen Sie Ihre Datenbankobjekte ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image18.png))


Nach Abschluss des Assistenten für Entity Data Model wird im Entity Data Model-Designer angezeigt. Der Designer zeigt eine Klasse, die entspricht jeder Tabelle modelliert wird. Eine Klasse, die mit dem Namen Kontakte sollte angezeigt werden.

Der Assistent für Entity Data Model generiert Klassennamen basierend auf den Datenbanknamen für die Tabelle. Sie müssen fast immer den Namen der vom Assistenten generierte Klasse ändern. Mit der rechten Maustaste im Designer der Kontakte-Klasse, und wählen Sie die Menüoption **umbenennen**. Ändern Sie den Namen der Klasse von Kontakten (plural) Kontakt (Singular). Nachdem Sie den Klassennamen ändern, sollte die Klasse wie Abbildung 10 angezeigt werden.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**Abbildung 10**: die Kontakt-Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image20.png))


An diesem Punkt haben wir unsere Datenbankmodell erstellt. Verwenden wir die Kontakt-Klasse, um einem bestimmten Datensatz in der Datenbank darzustellen.

## <a name="creating-the-home-controller"></a>Erstellen der Home-Controller

Der nächste Schritt besteht darin unsere Home-Controller zu erstellen. Die Home-Controller ist der Standard-Domänencontroller in einer ASP.NET MVC-Anwendung aufgerufen werden.

Erstellen die Home-Controller-Klasse, indem Sie den Ordner Controllers im Fenster Projektmappen-Explorer mit der rechten Maustaste, und wählen die Menüoption **hinzufügen, Controller** (siehe Abbildung 11). Beachten Sie das Kontrollkästchen mit der Bezeichnung **Hinzufügen von Aktionsmethoden für erstellen, aktualisieren und Details Szenarien**. Stellen Sie sicher, dass dieses Kontrollkästchen aktiviert ist, bevor Sie auf die **hinzufügen** Schaltfläche.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**Abbildung 11**: Hinzufügen von den Home-Controller ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image22.png))


Beim Erstellen der Home-Controller erhalten Sie die Klasse im Codebeispiel 1.

**1 – Controllers\HomeController. cs auflisten**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>Auflisten der Kontakte

Um die Datensätze in der Datenbanktabelle Kontakte anzeigen zu können, muss eine Index() Aktion und die Indexansicht ein erstellt werden.

Die Home-Controller enthält bereits eine Aktion Index(). Wir müssen diese Methode ändern, damit es auflisten 2 aussieht.

**Auflisten von 2 – Controllers\HomeController. cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

Beachten Sie, dass die Home-Controller-Klasse in Codebeispiel 2 ein privates Feld mit dem Namen enthält \_Entitäten. Die \_Entitäten Feld die Entitäten aus dem Datenmodell darstellt. Wir verwenden die \_Entitäten Feld, um die Kommunikation mit der Datenbank.

Die Index()-Methode gibt eine Ansicht, die alle Kontakte aus der Datenbanktabelle Kontakte darstellt. Der Ausdruck \_Entitäten. ContactSet.ToList() gibt die Liste der Kontakte als eine generische Liste zurück.

Jetzt, dass wir erstellt den Controller Index haben wir als Nächstes die Indexansicht erstellen müssen. Bevor Sie erstellen die Indexansicht, kompilieren Sie die Anwendung im Menü die Option **erstellen, Projektmappe**. Kompilieren Sie Ihr Projekt immer vor dem Hinzufügen einer Ansicht in der Reihenfolge der Liste der Modellklassen in angezeigt werden die **Ansicht hinzufügen** Dialogfeld.

Sie können die Indexansicht erstellen, indem die Index()-Methode mit der rechten Maustaste, und wählen die Menüoption **Ansicht hinzufügen** (siehe Abbildung 12). Durch das Auswählen dieser Option wird die **Ansicht hinzufügen** Dialogfeld (siehe Abbildung 13).


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**Abbildung 12**: die Indexansicht hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image24.png))


In der **Ansicht hinzufügen** Dialogfeld, aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**. Wählen Sie die Datenklasse anzeigen ContactManager.Models.Contact und in der Inhaltsliste anzeigen. Bei Auswahl dieser Optionen generiert eine Sicht, die eine Liste der Kontakt-Datensätze anzeigt.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**Abbildung 13**: die Ansicht der hinzufügen-Dialogfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image26.png))


Beim Klicken auf die **hinzufügen** Schaltfläche, die Indexansicht auflisten 3 wird generiert. Beachten Sie, dass die &lt;% @ Page %&gt; Richtlinie, die am oberen Rand der Datei angezeigt wird. Die Indexansicht erbt die ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; Klasse. Das heißt, stellt der Skriptobjektmodell-Klasse in der Ansicht eine Liste der Kontakt-Entitäten dar.

Der Text, der die Indexansicht enthält eine foreach-Schleife, die Durchlaufen aller Kontakte, dargestellt durch die Modell-Klasse. Der Wert für jede Eigenschaft der Klasse Kontakt ist innerhalb einer HTML-Tabelle angezeigt.

**Auflisten von 3 – Views\Home\Index.aspx (unverändert)**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

Wir müssen die Indexansicht eine Änderung vornehmen. Da wir eine Detailansicht nicht erstellen, können wir den Link Details zu entfernen. Suchen Sie und entfernen Sie den folgenden Code aus der Sicht des Indexes:

{Id = Element. ID}) "&gt;

Nachdem Sie die Indexansicht geändert haben, können Sie die Kontakt-Manager-Anwendung ausführen. Wählen Sie die Menüoption Debuggen, Debugging starten, oder drücken Sie einfach F5. Beim ersten der Anwendung ausführen erhalten Sie im Dialogfeld in Abbildung 14. Wählen Sie die Option **ändern Sie die Datei "Web.config" zum Aktivieren des Debuggens** , und klicken Sie auf die Schaltfläche "OK".


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**Abbildung 14**: Aktivieren des Debuggens ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image28.png))


Standardmäßig wird die Indexansicht zurückgegeben. In dieser Ansicht werden alle Daten aus der Datenbanktabelle Kontakte (siehe Abbildung 15).


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**Abbildung 15**: die Indexansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image30.png))


Beachten Sie, dass die Indexansicht einen Link mit der Bezeichnung "neu erstellen", am unteren Rand der Ansicht enthält. Im nächsten Abschnitt erfahren Sie, wie zum Erstellen neuer Kontakte.

## <a name="creating-new-contacts"></a>Erstellen neuer Kontakte

Um Benutzern das Erstellen neuer Kontakte zu aktivieren, müssen wir den Home-Controller zwei Create()-Aktionen hinzugefügt. Wir müssen eine Create()-Aktion erstellen, die einem HTML-Formular zum Erstellen eines neuen Kontakts zurückgibt. Wir müssen eine zweite Create()-Aktion zu erstellen, die die aktuelle Datenbank einfügen des neuen Kontakts ausführt.

Die neue Create()--Methoden, die wir den Home-Controller hinzufügen müssen, sind in Auflisten von 4 enthalten.

**Auflisten von 4 – Controllers\HomeController. cs (mit Methoden erstellen)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

Die erste Create()--Methode kann mit HTTP GET aufgerufen werden, während die zweite Create()--Methode, die nur über eine HTTP POST aufgerufen werden kann. Das heißt, kann die zweite Create()--Methode aufgerufen werden, nur, wenn eine HTML-Formular Posten von Beiträgen. Die erste Create()--Methode gibt einfach eine Sicht, die zum Erstellen eines neuen Kontakts HTML-Formular enthält. Die zweite Create()--Methode ist viel interessanter: der Datenbank den neuen Kontakt hinzugefügt.

Beachten Sie, dass die zweite Create()--Methode geändert wurde, um eine Instanz der Klasse Kontakt zu akzeptieren. Die Formularwerte aus dem HTML-Formular gebucht werden automatisch diese Klasse erhalten Sie vom ASP.NET MVC-Framework gebunden. Jedem Formularfeld von dem Erstellen von HTML-Formular ist an eine Eigenschaft des Parameters Kontakt zugewiesen.

Beachten Sie, dass der Parameter erhalten Sie mit einem [Bind]-Attribut ergänzt wird. Das Attribut [Bind] Dient zum Ausschließen der Bindung der Kontakt-Id-Eigenschafts. Da die Id-Eigenschaft eine Identity-Eigenschaft darstellt, Verschlüsselungskennwort wir t die Id-Eigenschaft festlegen möchten.

Im Text der Methode Create()-Entity Framework verwendet, um dem neuen Kontakt in die Datenbank einzufügen. Der neue Kontakt wird der vorhandene Satz von Kontakten hinzugefügt, und die SaveChanges()-Methode wird aufgerufen, um diese Änderungen wieder in der zugrunde liegenden Datenbank bereitzustellen.

Sie können einem HTML-Formular für das Erstellen neuer Kontakte von einer dieser beiden Create()--Methoden mit der rechten Maustaste, und wählen die Menüoption generieren **Ansicht hinzufügen** (siehe Abbildung 16).


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**Abbildung 16**: die Erstellungsansicht hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image32.png))


In der **Ansicht hinzufügen** wählen Sie im Dialogfeld die **ContactManager.Models.Contact** Klasse und die **erstellen** Option für den Inhalt anzeigen (siehe Abbildung 17). Beim Klicken auf die **hinzufügen** Schaltfläche Ansicht wird automatisch generiert, erstellen.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**Abbildung 17**: Anzeigen einer Seite auflösen ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image34.png))


Die Erstellungsansicht enthält Felder für jede Eigenschaft der Klasse wenden Sie sich an. Der Code für die Erstellungsansicht ist im Codebeispiel 5 enthalten.

**5 – Views\Home\Create.aspx auflisten**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

Nachdem Sie die Create()-Methoden ändern, und fügen Sie die Erstellungsansicht hinzu, können Sie die Kontakt-Manager-Anwendung ausführen und Erstellen neuer Kontakte. Klicken Sie auf die **neu erstellen** Link, der in der Index angezeigt, zu dem die Erstellungsansicht navigiert wird. Die Ansicht in Abbildung 18 sollte angezeigt werden.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**Abbildung 18**: die Create View ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image36.png))


## <a name="editing-contacts"></a>Bearbeiten von Kontakten

Hinzufügen der Funktionalität für die Bearbeitung von eines Datensatzes ist sehr ähnlich, die Funktionen zum Erstellen neuer Kontakt Datensätze hinzufügen. Zunächst müssen wir zwei neue Methoden der Bearbeitung der Home-Controller-Klasse hinzufügen. Diese neuen Edit()-Methoden sind in 6 auflisten enthalten.

**Auflisten von 6 – Controllers\HomeController. cs (mit bearbeiten-Methoden)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

Die erste Edit()-Methode wird von einem HTTP-GET-Vorgang aufgerufen. Ein Id-Parameter wird an diese Methode übergeben, das die Id der dem zu bearbeitenden Kontaktdatensatz darstellt. Das Entity Framework wird verwendet, um einen Kontakt abrufen, der der ID übereinstimmt. Wird zurückgegeben, eine Sicht, die eine HTML-Formular zum Bearbeiten eines Datensatzes enthält.

Die zweite Edit() Methode führt das eigentliche Update mit der Datenbank. Diese Methode akzeptiert eine Instanz der Klasse erhalten Sie als Parameter an. Das ASP.NET-MVC-Framework bindet die Felder aus dem Formular bearbeiten diese Klasse automatisch. Beachten Sie, dass Sie t Verschlüsselungskennwort enthalten das Attribut [Bind] beim Bearbeiten eines Kontakts (wir benötigen den Wert der Id-Eigenschaft).

Das Entity Framework dient zum Speichern der geänderten erhalten Sie in der Datenbank. Der ursprüngliche Kontakt muss zuerst aus der Datenbank abgerufen werden. Als Nächstes wird die Entity Framework ApplyPropertyChanges()-Methode aufgerufen, um die Änderungen an den Kontakt zu erfassen. Schließlich wird das Entity Framework SaveChanges()-Methode aufgerufen, um die Änderungen an der zugrunde liegenden Datenbank beibehalten.

Sie können die Sicht zu generieren, die den bearbeiten-Formular enthält, indem Sie mit der rechten Maustaste der Edit()-Methode, und wählen Sie die Menüoption im Ansicht hinzufügen. Klicken Sie im Dialogfeld "Ansicht hinzufügen" Wählen Sie die **ContactManager.Models.Contact** Klasse und die **bearbeiten** Inhalt anzeigen (siehe Abbildung 19).


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**Abbildung 19**: Hinzufügen einer Ansicht bearbeiten ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image38.png))


Wenn Sie die Schaltfläche "hinzufügen" klicken, wird eine neue Bearbeitungsansicht automatisch generiert. Das HTML-Formular, das generiert wird, enthält die Felder, die die Eigenschaften der Klasse wenden Sie sich an (Siehe auflisten von 7) entsprechen.

**Auflisten von 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Löschen von Kontakten

Wenn Sie die Kontakte löschen möchten, müssen Sie zwei Delete() Aktionen der Home-Controller-Klasse hinzufügen. Die erste Delete() Aktion wird ein Delete-Bestätigung-Formular angezeigt. Die zweite Delete() Aktion führt das eigentliche löschen.

> [!NOTE] 
> 
> Später, ändern in Iteration #7, wir der Kontakt-Manager, damit sie einen einem Schritt löschen Ajax unterstützt.


Auflisten von 8 sind die beiden neuen Delete() Methoden enthalten.

**Auflisten von 8 - Controllers\HomeController. cs (Delete-Methoden)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

Gibt eine Bestätigungsformular für das Löschen von einem Datensatz aus der Datenbank die erste Delete()-Methode (Siehe Figure20). Die zweite Delete()-Methode führt die tatsächlichen Löschvorgang für die Datenbank. Nachdem der ursprünglichen Kontakt aus der Datenbank abgerufen wurden, werden die Entity Framework DeleteObject() und SaveChanges() Methoden aufgerufen, um das Löschen der Datenbank auszuführen.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**Abbildung 20**: die Ansicht für das Löschen-Bestätigung ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image40.png))


Wir müssen die Indexansicht ändern, sodass sie einen Link zum Löschen von Kontaktdatensatz enthält (siehe Abbildung 21). Sie müssen den folgenden Code in der gleichen Tabellenzelle hinzufügen, die den Bearbeitungslink enthält:

Html.ActionLink( { id=item.Id }) %&gt;


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**Abbildung 21**: indizieren Sie die Sicht mit Bearbeitungslink ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image42.png))


Als Nächstes müssen wir die Löschansicht Bestätigung zu erstellen. Mit der rechten Maustaste der Delete()-Methode in der Home-Controller-Klasse, und wählen Sie im Menü die option Ansicht hinzufügen. Das Dialogfeld "Ansicht hinzufügen" wird angezeigt (siehe Abbildung 22).

Anders als im Fall der Sichten Liste erstellen und Bearbeiten enthält das Dialogfeld "Ansicht hinzufügen" eine Option zum Erstellen einer Löschansicht keine. Wählen Sie stattdessen die **ContactManager.Models.Contact** Datenklasse und **leere** Inhalte anzuzeigen. Leere Ansicht, dass wir uns die Ansicht zu erstellen benötigen Content-Option auswählen.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**Abbildung 22**: Hinzufügen der Löschansicht Bestätigung ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image44.png))


Der Inhalt der Ansicht für das Löschen ist im Codebeispiel 9 enthalten. Diese Ansicht enthält ein Formular, das überprüft, ob auf ein bestimmter Kontakt werden soll (siehe Abbildung 21) gelöscht.

**Auflisten von 9 – Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Ändern den Namen des Controllers an Standardeinstellung

Es kann darauf hingewiesen, dass der Name der unsere Controllerklasse zum Arbeiten mit Kontakten HomeController-Klasse heißt. Sollte nicht t der Controller ContactController benannt werden?

Dieses Problem ist einfach zu beheben. Zunächst müssen wir den Namen des Home-Controllers umzugestalten. HomeController-Klasse in Visual Studio Code-Editor zu öffnen, klicken Sie mit der mit der rechten Maustaste auf den Namen der Klasse, und wählen Sie die Menüoption **Umgestalten Rename**. Mit dieser Option auswählen, wird das Dialogfeld "Umbenennen" geöffnet.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**Abbildung 23**: Refactoring Controllernamen ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image46.png))


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**Abbildung 24**: verwenden das Dialogfeld "Umbenennen" ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image48.png))


Wenn Sie die Controllerklasse umbenennen, wird Visual Studio den Namen des Ordners, in den Ordner Views ebenfalls aktualisieren. Visual Studio wird die \Views\Home-Ordner in den Ordner \Views\Contact umbenennen.

Nachdem Sie diese Änderung vorgenommen haben, wird die Anwendung nicht mehr einen Home-Controller verfügen. Wenn Sie Ihre Anwendung ausführen, erhalten Sie die Seite "Fehler" in Abbildung 25.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**Abbildung 25**: kein Standard-Controller ([klicken Sie hier, um das Bild in voller Größe angezeigt](iteration-1-create-the-application-cs/_static/image50.png))


Wir müssen die Standardroute in der Datei "Global.asax" des Controllers Kontakt anstelle der Home-Controller verwendet zu aktualisieren. Öffnen Sie die Datei "Global.asax", und ändern Sie den Standard-Controller verwendet, die für die Standardroute (Siehe auflisten von 10).

**Auflisten von 10 - Global.asax.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

Nachdem Sie diese Änderungen vorgenommen haben, wird der Kontakt-Manager ordnungsgemäß ausgeführt. Nun wird die Controllerklasse Kontakt als Standard-Controller verwendet.

## <a name="summary"></a>Zusammenfassung

In dieser ersten Iteration haben wir eine einfache Kontakt-Manager-Anwendung in die schnellste Möglichkeit möglich. Wir nutzte Visual Studio automatisch den anfänglichen Code für unsere Controller und Ansichten generieren. Wir haben auch genutzt, das Entity Framework zum automatischen Generieren von unserem Modell Datenbankklassen.

Wir können derzeit auflisten, erstellen, bearbeiten und Löschen von Kontaktdatensatz mit der Anwendung Manager wenden Sie sich an. Wir können also alle den grundlegenden Datenbankvorgängen, die von einer Datenbank-driven Webanwendung benötigte ausführen.

Leider hat die Anwendung einige Probleme auf. First und Exchange activesync in jedem Fall dies geben, die Kontakt-Manager-Anwendung ist nicht besonders für zielumgebungen Anwendung. Einige datenbankentwurfsarbeit benötigt. In der nächsten Iteration betrachten wir, wie wir die Ansicht Standardmasterseite und cascading Stylesheet, um die Darstellung der Anwendung zu verbessern ändern können.

Zweitens haben wir alle Form-Überprüfung nicht implementiert. Angenommen, ist es nichts zum verhindern, dass Sie Kontaktformular erstellen ohne eingeben von Werten für alle Felder des Formulars zu senden. Darüber hinaus können Sie die Telefonnummer ist ungültig Telefonnummern und e-Mail-Adressen eingeben. Beginnen wir das Problem der formularvalidierung in Iteration #3 zu konfigurieren.

Schließlich und am wichtigsten ist, kann nicht die aktuelle Iteration der der Kontakt-Manager-Anwendung einfach geändert oder beibehalten werden. Beispielsweise ist die Datenbank-Zugriffslogik rechts in der Controlleraktionen integrierten. Dies bedeutet, dass wir unsere Datenzugriffscode ändern können, ohne zu unserem Domänencontroller ändern. In späteren Iterationen untersuchen wir die Software-Entwurfsmuster, die wir implementieren können, um den Vorgesetzten des Kontakts stabiler so ändern Sie vornehmen.

> [!div class="step-by-step"]
> [Nächste](iteration-2-make-the-application-look-nice-cs.md)
