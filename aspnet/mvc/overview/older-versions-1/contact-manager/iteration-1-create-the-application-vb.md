---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: 'Iteration #1 – Erstellen der Anwendung (VB) | Microsoft-Dokumentation'
author: microsoft
description: 'In der ersten Iteration wir der Contact Manager in der einfachsten maximal möglicher Größe erstellen. Wir fügen Unterstützung für grundlegender Datenbankvorgänge: erstellen, lesen, aktualisieren und d...'
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: 1a61699ef3ac2936df85ea38cd472ad3f107c070
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396126"
---
<a name="iteration-1--create-the-application-vb"></a>Iteration #1 – Erstellen der Anwendung (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[Code herunterladen](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> In der ersten Iteration wir der Contact Manager in der einfachsten maximal möglicher Größe erstellen. Wir fügen Unterstützung für grundlegender Datenbankvorgänge: erstellen, lesen, aktualisieren und löschen (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Erstellen einer Kontaktverwaltung ASP.NET MVC-Anwendung (VB)

In dieser Reihe von Tutorials erstellen wir eine gesamte Anwendung Kontaktverwaltung ab Beginn um den Vorgang abzuschließen. Die Contact Manager-Anwendung können Sie zum Speichern von Kontaktinformationen - Namen, Telefonnummern und e-Mail-Adressen – eine Liste der Personen an.

Wir erstellen die Anwendung über mehrere Iterationen. Bei jeder Iteration verbessern wir nach und nach der Anwendung. Das Ziel dieses mehrere Iteration-Ansatzes ist, um den Grund für jede Änderung verstehen können.

- Iteration #1 – Erstellen der Anwendung. In der ersten Iteration wir der Contact Manager in der einfachsten maximal möglicher Größe erstellen. Wir fügen Unterstützung für grundlegender Datenbankvorgänge: erstellen, lesen, aktualisieren und löschen (CRUD).

- Iteration #2 – Optimieren der Anwendung gut. In dieser Iteration verbessern wir die Darstellung der Anwendung durch Ändern der Standard-Masterseite für ASP.NET MVC-Ansicht und cascading Stylesheet.

- Iteration #3 – Hinzufügen der formularüberprüfung. In der dritten Iteration fügen wir grundlegende formularvalidierung hinzu. Es wird verhindert, dass Personen senden eines Formulars ohne erforderlichen Felder des Formulars abzuschließen. Wir überprüfen auch die e-Mail-Adressen und Telefonnummern.

- Stellen Sie Iteration #4 – lose koppeln der Anwendung. In dieser vierten Iteration nutzen wir einige Entwurfsmuster für Software zu verwalten und ändern Sie die Kontakt-Manager-Anwendung zu vereinfachen. Z. B. gestalten wir unsere Anwendung, die dem Repositorymuster und dem Dependency Injection-Muster verwenden.

- Iteration #5 – Erstellen von Komponententests. In der fünften Iteration stellen wir unsere Anwendung einfacher zu verwalten und zu ändern, indem Sie die Komponententests hinzufügen. Wir unsere Data Model-Klassen modellieren und erstellen Sie Komponententests für unseren Controller und die Validierungslogik.

- Iteration #6 – Verwenden der testgesteuerten Entwicklung. In dieser Iteration sechsten fügen wir neue Funktionen zu unserer Anwendung zuerst Schreiben von Komponententests und Code für die Komponententests schreiben hinzu. In dieser Iteration fügen wir wenden Sie sich an Gruppen hinzu.

- Iteration #7 - Ajax-Funktionen hinzufügen. In der siebten Iteration verbessern wir die Reaktionsfähigkeit und Leistung der Anwendung durch Hinzufügen von Unterstützung für Ajax.

## <a name="this-iteration"></a>Diese Iteration

In diesem ersten Iteration erstellen wir die grundlegende Anwendung. Das Ziel ist, auf die schnellste und einfachste Möglichkeit, die mögliche Contact Manager integrieren. In späteren Iterationen verbessern wir den Entwurf der Anwendung.

Die Kontakt-Manager-Anwendung ist eine einfache Anwendung zur Datenbank basieren. Sie können die Anwendung verwenden, um neue Kontakte erstellen, bearbeiten die existierenden Kontakte und Löschen von Kontakten.

In dieser Iteration führen wir die folgenden Schritte aus:

1. ASP.NET MVC-Anwendung
2. Erstellen Sie eine Datenbank zum Speichern unserer Kontakte
3. Generieren Sie eine Modellklasse für unsere Datenbank mit dem Microsoft Entity Framework
4. Erstellen Sie eine Controlleraktion und die Ansicht, die wir zum Auflisten aller Kontakte in der Datenbank ermöglicht
5. Erstellen von Controlleraktionen und eine Ansicht, die wir in der Datenbank einen neuen Kontakt erstellen können
6. Erstellen von Controlleraktionen und eine Ansicht, die wir zum Bearbeiten eines vorhandenen Kontakts in der Datenbank können
7. Erstellen von Controlleraktionen und eine Ansicht, die uns ermöglicht, ein schon vorhandenen Kontakts in der Datenbank zu löschen

## <a name="software-prerequisites"></a>Erforderliche Software

In ASP.NET MVC-Anwendungen benötigen Sie entweder Visual Studio 2008 oder Visual Web Developer 2008 auf Ihrem Computer (Visual Web Developer ist eine kostenlose Version von Visual Studio, die nicht alle erweiterten Funktionen von Visual Studio enthalten ist) installiert. Sie können entweder die Testversion von Visual Studio 2008 oder Visual Web Developer aus der folgenden Adresse herunterladen:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> ASP.NET MVC-Anwendungen mit Visual Web Developer müssen Sie Visual Web Developer Service Pack 1 installiert haben. Ohne Service Pack 1 können Sie die Web Application Projects erstellen.


ASP.NET MVC-Framework. Sie können das ASP.NET MVC-Framework von der folgenden Adresse herunterladen:

[https://www.asp.net/mvc](../../../index.md)

In diesem Tutorial verwenden wir das Microsoft Entity Framework für den Datenbankzugriff. Das Entity Framework ist im Lieferumfang von .NET Framework 3.5 Service Pack 1. Sie können dieses Servicepack von folgendem Speicherort herunterladen:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp; Displaylang = En](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Als Alternative zur Durchführung aller diese Downloads einzeln nacheinander können Sie den Webplattform-Installer (Web PI) nutzen. Sie können die Web-PI aus der folgenden Adresse herunterladen:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>ASP.NET MVC-Projekt

ASP.NET MVC-Webanwendungsprojekt. Starten Sie Visual Studio, und wählen Sie die Menüoption **Datei, neues Projekt**. Die **neues Projekt** angezeigt (siehe Abbildung 1). Wählen Sie die **Web** Projekttyp und die **ASP.NET MVC-Webanwendung** Vorlage. Nennen Sie das neue Projekt *ContactManager* , und klicken Sie auf die Schaltfläche "OK".


Stellen Sie sicher, dass Sie .NET Framework 3.5 in der Dropdownliste oben ausgewählt haben rechts von der **neues Projekt** Dialogfeld. Die ASP.NET MVC-Web-Application-Vorlage, die gewonnen t, andernfalls angezeigt werden soll.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)

**Abbildung 01**: das neue Projektdialogfeld ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image2.png))


ASP.NET MVC-Anwendung, die **Komponententestprojekt erstellen** Dialogfeld wird angezeigt. Sie können dieses Dialogfeld verwenden, um anzugeben, dass Sie erstellen und ein Komponententestprojekt der Projektmappe hinzufügen, wenn Sie Ihre ASP.NET MVC-Anwendung erstellen möchten. Obwohl wir t gewonnen werden. erstellen Komponententests in dieser Iteration, wählen Sie die Option **Ja, Komponententestprojekt erstellen** , da wir Komponententests in einer späteren Iteration hinzufügen möchten. Ein Testprojekt hinzuzufügen, wenn Sie zuerst ein neues ASP.NET MVC-Projekt erstellen, ist viel einfacher als ein Testprojekt hinzuzufügen, nachdem das ASP.NET MVC-Projekt erstellt wurde.

> [!NOTE] 
> 
> Da Projekte in Visual Web Developer nicht unterstützt wird, erhalten Sie bei Verwendung von Visual Web Developer nicht das Dialogfeld "Komponententestprojekt erstellen".


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)

**Abbildung 02**: Dialogfeld für das Komponententestprojekt erstellen ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image4.png))


ASP.NET MVC-Anwendung, die im Projektmappen-Explorer von Visual Studio-Fenster angezeigt wird (siehe Abbildung 3). Wenn Sie Ich möchte das Projektmappen-Explorer-Fenster angezeigt, und Sie dieses Fenster öffnen können, im Menü die Option **anzeigen, Projektmappen-Explorer**. Beachten Sie, dass die Projektmappe zwei Projekte enthält: die ASP.NET MVC-Projekt und das Testprojekt. Das ASP.NET MVC-Projekt, ist ContactManager ", und das Testprojekt" ContactManager.Tests.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)

**Abbildung 03**: der Projektmappen-Explorer-Fenster ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Löschen die Beispiel-Projektdateien

ASP.NET MVC-Projektvorlage enthält Beispieldateien für Controller und Ansichten. Bevor Sie eine neue ASP.NET MVC-Anwendung erstellen, sollten Sie diese Dateien löschen. Sie können die Dateien und Ordner im Projektmappen-Explorer löschen, indem Sie mit der rechten Maustaste eine Datei oder einen Ordner, und wählen die Menüoption **löschen**.

Sie müssen die folgenden Dateien aus dem ASP.NET MVC-Projekt zu löschen:

- \Controllers\HomeController.vb

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

Und Sie müssen die folgende Datei aus dem Testprojekt zu löschen:

\Controllers\HomeControllerTest.vb

## <a name="creating-the-database"></a>Erstellen der Datenbank

Die Kontakt-Manager-Anwendung ist eine Datenbank-gestützten Webanwendung. Wir verwenden eine Datenbank, um die Kontaktinformationen zu speichern.

ASP.NET MVC-Framework mit einer beliebigen modernen Datenbank, einschließlich Microsoft SQL Server, Oracle, MySQL und IBM DB2-Datenbanken. In diesem Tutorial verwenden wir eine Microsoft SQL Server-Datenbank. Wenn Sie Visual Studio installieren, werden Sie mit der Option der Installation von Microsoft SQL Server Express handelt es sich eine kostenlose Version von Microsoft SQL Server-Datenbank bereitgestellt.

Erstellen einer neuen Datenbank mit der rechten Maustaste in die App\_Ordner im Projektmappen-Explorer, und wählen die Menüoption **hinzufügen, neue Element**. In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **Daten** Kategorie und die **SQL Server-Datenbank** Vorlage (siehe Abbildung 4). Benennen Sie die neue Datenbank ContactManagerDB.mdf, und klicken Sie auf die Schaltfläche "OK".


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)

**Abbildung 04**: Erstellen einer neuen Microsoft SQL Server Express-Datenbank ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image8.png))


Nachdem Sie die neue Datenbank erstellt haben, wird die Datenbank in der App\_Ordner im Projektmappen-Explorer-Fenster. Doppelklicken Sie auf die ContactManager.mdf-Datei, um das Server-Explorer-Fenster öffnen, und Verbinden mit der Datenbank.

> [!NOTE] 
> 
> Das Server-Explorer-Fenster wird das Datenbank-Explorer-Fenster bei Microsoft Visual Web Developer aufgerufen.


Das Server-Explorer-Fenster können zum Erstellen neuer Datenbankobjekte, z. B. Datenbanktabellen, Sichten, Trigger und gespeicherte Prozeduren. Mit der rechten Maustaste in den Ordner "Tabellen", und wählen Sie die Menüoption **neue Tabelle hinzufügen**. Die Datenbank-Tabellen-Designer wird angezeigt (siehe Abbildung 5).


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)

**Abbildung 05**: der Datenbank-Tabellen-Designer ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image10.png))


Wir müssen eine Tabelle erstellen, die folgenden Spalten enthält:

<a id="0.2_table01"></a>


| **Name der Spalte** | **Datentyp** | **NULL-Werte zulassen** |
| --- | --- | --- |
| Id | int | False |
| FirstName | nvarchar(50) | False |
| LastName | nvarchar(50) | False |
| Telefon | nvarchar(50) | False |
| E-Mail | nvarchar(255) | False |


Die erste Spalte, der Id-Spalte ist ein Sonderfall. Sie müssen die Id-Spalte als Identitätsspalte und eine Primary Key-Spalte zu kennzeichnen. Sie angeben, dass eine Spalte eine Identitätsspalte ist, erweitern Spalte Eigenschaften mit Getter (Suchen Sie am unteren Rand der Abbildung 6), und Scrollen auf die Spezifikation der Spaltenidentität-Eigenschaft. Legen Sie die **(ist Identity)** -Eigenschaft auf den Wert **Ja**.

Markieren Sie eine Spalte als eine Primary Key-Spalte, indem Sie die Spalte auswählen, und klicken auf die Schaltfläche mit dem Symbol eines Schlüssels. Nachdem eine Spalte als eine Primary Key-Spalte markiert ist, wird ein Symbol eines Schlüssels neben der Spalte angezeigt (siehe Abbildung 6).

Nachdem Sie die Tabelle erstellt haben, klicken Sie auf die Schaltfläche "Speichern" (die Schaltfläche mit einem Symbol für eine Diskette), um die neue Tabelle zu speichern. Geben Sie den Namen Ihrer neuen Tabelle *Kontakte*.

Nach dem Ende die Kontakte-Datenbanktabelle erstellen sollten Sie einige Datensätze in der Tabelle hinzufügen. Mit der rechten Maustaste in der Tabelle "Contacts" im Server-Explorer-Fenster, und wählen Sie die Menüoption **Tabellendaten anzeigen**. Geben Sie einen oder mehrere Kontakte im angezeigten Raster aus.

## <a name="creating-the-data-model"></a>Erstellen des Datenmodells

Die ASP.NET MVC-Anwendung besteht aus Modellen, Ansichten und Controllern. Zunächst wird das Erstellen einer Modellklasse, die die Contacts-Tabelle darstellt, die wir im vorherigen Abschnitt erstellt haben.

In diesem Tutorial verwenden wir das Microsoft Entity Framework, um eine Modellklasse aus der Datenbank automatisch zu generieren.

> [!NOTE] 
> 
> ASP.NET MVC-Framework ist nicht an Microsoft Entity Framework in keiner Weise gebunden. Sie können ASP.NET MVC mit alternativen datenzugriffstechnologien einschließlich NHibernate, LINQ to SQL oder ADO.NET verwenden.


Um den datenmodellklassen erstellen, gehen Sie wie folgt vor:

1. Mit der rechten Maustaste im Projektmappen-Explorer den Ordner "Models", und wählen Sie **hinzufügen, neue Element**. Die **neues Element hinzufügen** angezeigt (siehe Abbildung 6).
2. Wählen Sie die **Daten** Kategorie und die **ADO.NET Entity Data Model** Vorlage. Benennen Sie Ihr Datenmodell *ContactManagerModel.edmx* , und klicken Sie auf die **hinzufügen** Schaltfläche. Der Entity Data Model-Assistent wird angezeigt (siehe Abbildung 7).
3. In der **auswählen des Modellinhalts** Schritt select **aus Datenbank generieren** (siehe Abbildung 7).
4. In der **wählen Sie Ihre Datenverbindung** Schritt, wählen Sie die ContactManagerDB.mdf-Datenbank aus, und geben Sie den Namen *ContactManagerDBEntities* für die Entitätsverbindungseinstellungen in app.config/Web.config (siehe Abbildung 8).
5. In der **Datenbankobjekte auswählen** Schritt, wählen Sie das Kontrollkästchen für Tabellen (siehe Abbildung 9). Das Datenmodell enthält alle Tabellen in der Datenbank (es ist nur ein einziges, der Contacts-Tabelle vorhanden). Geben Sie den Namespace *Modelle*. Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)

**Abbildung 06**: Dialogfeld für das neue Element in einer hinzufügen ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image12.png))


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)

**Abbildung 07**: Auswählen des Modellinhalts ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image14.png))


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)

**Abbildung 08**: Wählen Sie Ihre Datenverbindung ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image16.png))


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)

**Abbildung 09**: Datenbankobjekte auswählen ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image18.png))


Nach Abschluss des Assistenten für Entity Data Model wird dem Entity Data Model-Designer angezeigt. Der Designer zeigt eine Klasse, die entspricht jeder Tabelle, das modelliert wird. Daraufhin sollte eine Klasse, die mit dem Namen Kontakte.

Der Entity Data Model-Assistent generiert basierte auf Datenbanknamen Klassennamen. Sie müssen fast immer den Namen der vom Assistenten generierte Klasse zu ändern. Mit der rechten Maustaste im Designer auf der Kontakte-Klasse, und wählen Sie die Menüoption **umbenennen**. Ändern Sie den Namen der Klasse von Kontakten (plural), Kontakt (Singular). Nachdem Sie den Namen der Klasse ändern, sollte die Klasse wie Abbildung 10 angezeigt werden.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)

**Abbildung 10**: der Contact-Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image20.png))


An diesem Punkt haben wir unser Datenbankmodell erstellt. Wir können die Contact-Klasse verwenden, um einen bestimmten Kontakteintrag in unserer Datenbank darzustellen.

## <a name="creating-the-home-controller"></a>Erstellen den Home-Controller

Der nächste Schritt besteht darin den Home-Controller zu erstellen. Die Home-Controller wird der Standardcontroller in einer ASP.NET MVC-Anwendung aufgerufen.

Erstellen die Home-Controller-Klasse mit der rechten Maustaste im Fenster Projektmappen-Explorer den Ordner "Controllers", und wählen die Menüoption **hinzufügen, Controller** (siehe Abbildung 11). Beachten Sie, dass das Kontrollkästchen **Hinzufügen von Aktionsmethoden für Create, Update und Details Szenarien**. Stellen Sie sicher, dass dieses Kontrollkästchen aktiviert ist, bevor Sie auf die **hinzufügen** Schaltfläche.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)

**Abbildung 11**: Hinzufügen von den Home-Controller ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image22.png))


Wenn Sie den Home-Controller erstellen, erhalten Sie in Codebeispiel 1 die-Klasse.

**1 – Controllers\HomeController.vb auflisten**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a>Auflisten von Kontakten

Um Datensätze in der Datenbanktabelle Kontakte anzeigen möchten, müssen wir eine Aktion Index() und eine Ansicht "Index" zu erstellen.

Die Home-Controller enthält bereits eine Aktion Index(). Wir müssen diese Methode zu ändern, sodass sie aussieht wie Codebeispiel 2 dargestellt.

**Codebeispiel 2 - Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

Beachten Sie, dass die Home-Controller-Klasse im Codebeispiel 2 mit ein privates Feld namens enthält \_Entitäten. Die \_Entitäten Feld stellt die Entitäten aus dem Datenmodell. Wir verwenden die \_Entitäten Feld, um die Kommunikation mit der Datenbank.

Die Methode Index() gibt es sich um eine Ansicht, die alle Kontakte aus der Contacts-Datenbanktabelle darstellt. Der Ausdruck \_Entitäten. ContactSet.ToList() gibt die Liste der Kontakte als eine generische Liste zurück.

Nun, wir haben erstellt den Index-Controller, müssen wir als Nächstes erstellen die Ansicht "Index". Vor dem Erstellen der Ansicht "Index", kompilieren Sie Ihre Anwendung durch Auswählen der Menüoption **erstellen "," Projektmappe erstellen**. Kompilieren Sie das Projekt immer vor dem Hinzufügen einer Ansicht in der Reihenfolge für die Liste der ViewModel-Klassen, die in angezeigt werden die **Ansicht hinzufügen** Dialogfeld.

Sie erstellen die Ansicht "Index", indem mit der rechten Maustaste der Index()-Methode, und wählen die Menüoption **Ansicht hinzufügen** (siehe Abbildung 12). Durch Auswählen dieser Menüoption wird das **Ansicht hinzufügen** Dialogfeld (siehe Abbildung 13).


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)

**Abbildung 12**: Hinzufügen der Ansicht "Index" ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image24.png))


In der **Ansicht hinzufügen** Dialogfeld aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**. Wählen Sie die Datenklasse anzeigen ContactManager.Contact und die Inhaltsliste anzeigen. Wählen diese Optionen generiert eine Ansicht, die eine Liste der Kontakt-Datensätze werden angezeigt.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)

**Abbildung 13**: die Ansicht der hinzufügen-Dialogfeld ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image26.png))


Beim Klicken auf die **hinzufügen** Schaltfläche Ansicht "Index" in Codebeispiel 3 wird generiert. Beachten Sie, dass die &lt;% @ Page %&gt; Richtlinie, die am Anfang der Datei angezeigt wird. Ansicht "Index" erbt von der ViewPage&lt;"IEnumerable"&lt;ContactManager.Models.Contact&gt; &gt; Klasse. Das heißt, stellt die Model-Klasse in der Ansicht eine Liste der Kontaktelemente dar.

Der Text der Ansicht "Index" enthält eine Foreach-Schleife, die durchläuft jedes der Kontakte mit dem durch die Model-Klasse dargestellt wird. Der Wert für jede Eigenschaft der Contact-Klasse wird innerhalb einer HTML-Tabelle angezeigt.

**Codebeispiel 3 - Views\Home\Index.aspx (unverändert)**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

Wir müssen eine Änderung an den Index. Da wir keine Detailansicht erstellen, können wir den Link Details entfernen. Suchen Sie und entfernen Sie den folgenden Code aus der Ansicht "Index":

{.id = Element. ID der}) %&gt;

Nachdem Sie die Ansicht "Index" geändert haben, können Sie die Kontakt-Manager-Anwendung ausführen. Die Option im Menü Debuggen, Debugging starten, oder drücken Sie einfach F5. Beim ersten der Anwendung ausführen erhalten Sie im Dialogfeld in Abbildung 14. Wählen Sie die Option **ändern Sie die Datei "Web.config" zum Aktivieren von debugging** , und klicken Sie auf die Schaltfläche "OK".


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)

**Abbildung 14**: Aktivieren des Debuggens ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image28.png))


Standardmäßig wird die Ansicht "Index" zurückgegeben. In dieser Ansicht werden alle Daten aus der Contacts-Datenbanktabelle (siehe Abbildung 15).


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)

**Abbildung 15**: der Index-Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image30.png))


Beachten Sie, dass die Ansicht "Index" eine Verknüpfung mit der Bezeichnung "neu erstellen", am unteren Rand der Ansicht enthält. Im nächsten Abschnitt erfahren Sie, wie Sie neue Kontakte zu erstellen.

## <a name="creating-new-contacts"></a>Erstellen neuer Kontakte

Damit um Benutzer neue Kontakte erstellen zu können, müssen wir den Home-Controller zwei Create() Aktionen hinzugefügt. Wir müssen eine Create()-Aktion zu erstellen, die ein HTML-Formular zum Erstellen eines neuen Kontakts zurückgibt. Wir müssen eine zweite Create() Aktion erstellen, die die aktuelle Datenbank Einfügung des neuen Kontakts ausführt.

Die neuen Create()-Methoden, die den Home-Controller hinzufügen müssen, sind in Listing 4 enthalten.

**Programmausdruck 4 - Controllers\HomeController.vb (mit der Create-Methoden)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

Die erste Create()-Methode kann mit HTTP GET aufgerufen werden, während die zweite Create()-Methode, die nur von einem HTTP POST aufgerufen werden kann. Das heißt, kann die zweite Create()-Methode aufgerufen werden, nur, wenn ein HTML-Formular zu veröffentlichen. Die erste Create()-Methode gibt einfach eine Ansicht, die das HTML-Formular zum Erstellen eines neuen Kontakts enthält, zurück. Die zweite Create()-Methode ist sehr viel interessanter: die Datenbank den neuen Kontakt hinzugefügt.

Beachten Sie, dass die zweite Methode der Create() geändert wurde, um eine Instanz des der Contact-Klasse akzeptieren. Die Formularwerte aus dem HTML-Formular gebucht werden automatisch auf diese Klasse wenden Sie sich an vom ASP.NET MVC-Framework gebunden. Jedem Formularfeld aus dem Erstellen der HTML-Formular wird eine Eigenschaft des Parameters Kontakt zugewiesen werden.

Beachten Sie, dass der Kontakt-Parameter mit einem [Bind]-Attribut ergänzt wird. Das Attribut [Bind] wird verwendet, zum Ausschließen der Bindung der Kontakt-Id-Eigenschaft. Da die Id-Eigenschaft eine Identity-Eigenschaft darstellt, Einbau wir, die die Id-Eigenschaft festgelegt werden soll, t zusätzlichen.

In den Text der Methode Create() Entity Framework verwendet, um den neuen Kontakt in die Datenbank einzufügen. Die neue Kontakt wird für die vorhandenen Kontakte hinzugefügt, und die SaveChanges()-Methode wird aufgerufen, um diese Änderungen zurück an den zugrunde liegenden Datenbank mithilfe von Push übertragen.

Sie können ein HTML-Formular für neue Kontakte erstellen, indem Sie mit der rechten Maustaste eine der beiden Create() Methoden und durch Auswählen der Menüoption generieren **Ansicht hinzufügen** (siehe Abbildung 16).


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)

**Abbildung 16**: Hinzufügen der Ansicht "erstellen" ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image32.png))


In der **Ansicht hinzufügen** wählen Sie im Dialogfeld die **ContactManager.Contact** Klasse und die **erstellen** Option zum Anzeigen von Inhalten (siehe Abbildung 17). Beim Klicken auf die **hinzufügen** Schaltfläche Ansicht wird automatisch generiert, erstellen.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)

**Abbildung 17**: eine Seite explode angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image34.png))


Ansicht "erstellen" enthält Felder für jede der Eigenschaften der Klasse wenden Sie sich an. Der Code für die Ansicht "erstellen" ist in Listing 5 enthalten.

**Programmausdruck 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

Nachdem Sie die Create() Methoden ändern, und fügen Sie die Erstellungsansicht hinzu, können Sie die Kontakt-Manager-Anwendung ausführen und erstellen neue Kontakte. Klicken Sie auf die **neu erstellen** Link, der in der Ansicht "Index", um zur Ansicht "erstellen" zu navigieren. Die Ansicht in Abbildung 18 sollte angezeigt werden.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)

**Abbildung 18**: die Create View ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image36.png))


## <a name="editing-contacts"></a>Bearbeiten von Kontakten

Hinzufügen der Funktionalität zum Bearbeiten eines Datensatzes wenden Sie sich an ist sehr ähnlich ist, die Funktionen zum Erstellen von neuen Kontaktdatensatz hinzufügen. Zunächst müssen wir zwei neue Edit-Methoden der Home-Controller-Klasse hinzufügen. Diese neuen Edit()-Methoden sind in Codebeispiel 6 enthalten.

**Codebeispiel 6: Controllers\HomeController.vb (mit der Edit-Methoden)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

Die erste Edit()-Methode wird von einem HTTP-GET-Vorgang aufgerufen. Für diese Methode steht für die Id des Kontaktdatensatzes bearbeitet wird, wird ein Id-Parameter übergeben. Das Entity Framework wird verwendet, um einen Kontakt abrufen, der der ID übereinstimmt. Eine Ansicht, die ein HTML-Formular zum Bearbeiten eines Datensatzes enthält, wird zurückgegeben.

Die zweite Edit()-Methode führt das eigentliche Update in der Datenbank. Diese Methode akzeptiert eine Instanz der Contact-Klasse als Parameter an. ASP.NET MVC-Framework die Formularfelder aus dem Datenbearbeitungsformular für diese Klasse wird automatisch gebunden. Beachten Sie, dass Sie keine Nachteile für Ihr das [Bind]-Attribut enthalten, beim Bearbeiten eines Kontakts (wir brauchen den Wert der Id-Eigenschaft).

Das Entity Framework wird verwendet, um den geänderten Kontakt mit der Datenbank zu speichern. Der ursprüngliche Kontakt muss zuerst aus der Datenbank abgerufen werden. Als Nächstes wird die Entity Framework ApplyPropertyChanges()-Methode aufgerufen, um die Änderungen an den Kontakt zu erfassen. Schließlich wird die Entity Framework SaveChanges()-Methode aufgerufen, um die Änderungen an der zugrunde liegenden Datenbank beibehalten.

Sie können die Sicht zu generieren, die das Bearbeitungsformular enthält, indem Sie mit der rechten Maustaste der Edit()-Methode, und wählen die Menüoption "" Add View. Wählen Sie in das Dialogfeld "Ansicht hinzufügen" die **ContactManager.Models.Contact** Klasse und die **bearbeiten** Inhalt anzeigen (siehe Abbildung 19).


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)

**Abbildung 19**: Hinzufügen einer Ansicht bearbeiten ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image38.png))


Wenn Sie die Schaltfläche "hinzufügen" klicken, wird eine neue Bearbeitungsansicht automatisch generiert. Das HTML-Formular, das generiert wird, enthält die Felder, die jeweils die Eigenschaften der Contact-Klasse (siehe Codebeispiel 7) entsprechen.

**Auflisten von 7 – Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Löschen von Kontakten

Wenn Sie Kontakte zu löschen, müssen Sie zwei Delete() Aktionen der Home-Controller-Klasse hinzufügen möchten. Die erste Delete()-Aktion wird ein Delete-Bestätigung-Formular angezeigt. Die zweite Delete()-Aktion führt das eigentliche löschen.

> [!NOTE] 
> 
> Später ändern in Iteration #7 wir Contact Manager, damit sie unterstützt eine einschrittigen Ajax zu löschen.


Die zwei neuen Delete()-Methoden sind in Codebeispiel 8 enthalten.

**Auflisten von 8 – Controllers\HomeController.vb (Delete-Methoden)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

Gibt die erste Delete()-Methode ein Bestätigungsformular zum Löschen von einem Datensatz aus der Datenbank (Siehe Figure20). Die zweite Delete()-Methode führt den tatsächlichen Löschvorgang für die Datenbank. Nach der ursprüngliche Kontakt aus der Datenbank abgerufen wurde, werden die Entity Framework DeleteObject() und SaveChanges() Methoden aufgerufen, um das Löschen der Datenbank auszuführen.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)

**Abbildung 20**: die Ansicht für das Löschen bestätigen ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image40.png))


Wir müssen die Ansicht "Index" zu ändern, sodass sie einen Link zum Löschen von Kontaktdatensatz enthält (siehe Abbildung 21). Sie müssen den folgenden Code in der gleichen Tabellenzelle hinzufügen, die den Link "Bearbeiten" enthält:

{.id = Element. ID der}) %&gt;


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)

**Abbildung 21**: indizieren Sie die Sicht mit dem Link "Bearbeiten" ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image42.png))


Als Nächstes müssen wir die Ansicht für das Löschen bestätigen zu erstellen. Mit der rechten Maustaste der Delete()-Methode in der Home-Controller-Klasse, und wählen Sie im Menü die option Ansicht hinzufügen. Das Dialogfeld "Ansicht hinzufügen" angezeigt (Abbildung 22).

Im Gegensatz zu bei der Ansichten auflisten, erstellen und Bearbeiten enthält das Dialogfeld "Ansicht hinzufügen" eine Option zum Erstellen einer Delete-Ansicht nicht. Wählen Sie stattdessen die **ContactManager.Models.Contact** Datenklasse und **leere** Inhalt anzeigen. Wählen die leere Ansicht, dass die Content-Option erfordern, die Ansicht selbst zu erstellen.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)

**Abbildung 22**: Hinzufügen der Ansicht für das Löschen bestätigen ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image44.png))


Der Inhalt der Ansicht für das Löschen ist im Codebeispiel 9 enthalten. Diese Ansicht enthält ein Formular, das bestätigt, ob auf ein bestimmter Kontakt sein soll (siehe Abbildung 21) gelöscht.

**Codebeispiel 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Ändern des Namens der Standardcontroller

Es kann darauf hingewiesen werden, dass der Name der unser Controllerklasse für das Arbeiten mit Kontakten HomeController-Klasse heißt. Treten normalerweise t der Controller werden mit dem Namen ContactController?

Dieses Problem ist recht einfach beheben. Zunächst müssen wir den Namen des dem Home-Controller zu gestalten. Öffnen Sie die HomeController-Klasse im Visual Studio Code-Editor, klicken Sie mit der rechten Maustaste auf den Namen der Klasse, und wählen Sie die Menüoption **umbenennen**. Durch Auswählen dieser Menüoption wird das Dialogfeld "Umbenennen" geöffnet.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)

**Abbildung 23**: Umgestalten eines Controllernamens ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image46.png))


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)

**Abbildung 24**: verwenden das Dialogfeld "Umbenennen" ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image48.png))


Wenn Sie Ihre Controller-Klasse umbenennen, aktualisiert Visual Studio den Namen des Ordners, in dem auch der Ordner "Views". Visual Studio wird den Ordner \Views\Home in den Ordner \Views\Contact umbenannt werden.

Nachdem Sie diese Änderung vorgenommen haben, wird die Anwendung nicht mehr einen Home-Controller verfügen. Wenn Sie Ihre Anwendung ausführen, erhalten Sie die Seite "Fehler" in Abbildung 25.


[![Das Dialogfeld "Neues Projekt"](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)

**Abbildung 25**: kein Standardcontroller ([klicken Sie, um das Bild in voller Größe anzeigen](iteration-1-create-the-application-vb/_static/image50.png))


Wir müssen die Standardroute in der Datei "Global.asax" anstelle von den Home-Controller mit der Contact-Controller zu aktualisieren. Öffnen Sie die Datei "Global.asax", und ändern Sie den Standardcontroller verwendet, die von der Standardroute (siehe Codebeispiel 10).

**Auflisten von 10 – Global.asax.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

Nachdem Sie diese Änderungen vorgenommen haben, wird der Kontakt-Manager ordnungsgemäß ausgeführt. Nun wird die Contact-Controller-Klasse als der Standardcontroller verwendet.

## <a name="summary"></a>Zusammenfassung

In diesem ersten Iteration haben wir eine einfache Contact Manager-Anwendung in die schnellste Möglichkeit möglich. Wir haben die Vorteile von Visual Studio, um den anfänglichen Code für unsere Controller und Ansichten automatisch zu generieren. Wir haben auch genutzt, Entity Framework für unsere Datenbank Modellklassen automatisch zu generieren.

Wir können derzeit auflisten, erstellen, bearbeiten und Löschen von Kontaktdatensatz mit der Contact Manager-Anwendung. Wir können also alle den grundlegender Datenbankvorgänge, die von einer Datenbank-gestützten Webanwendung erforderlich ausführen.

Leider hat unsere Anwendung einige Probleme. Zögern Sie zuerst, und ich dies zugeben, die Kontakt-Manager-Anwendung ist nicht der attraktivsten Anwendung. Entwurfsarbeit benötigt wird. In der nächsten Iteration betrachten wir, wie wir die Standardmasterseite für Ansicht und cascading Stylesheets verbessert die Darstellung der Anwendung ändern können.

Zweitens haben wir alle formularüberprüfung nicht implementiert. Angenommen, gibt es nichts zu verhindern, dass Sie das Kontaktformular erstellen ohne Eingabe von Werten für alle Felder des Formulars zu senden. Darüber hinaus können Sie die Telefonnummer ist ungültig und e-Mail-Adressen eingeben. Beginnen wir damit das Problem der formularvalidierung in Iteration #3 in Angriff zu nehmen.

Wichtiger ist, und schließlich kann nicht die aktuelle Iteration der Contact Manager-Anwendung leicht geändert oder beibehalten werden. Beispielsweise ist die Datenbank-Zugriffslogik rechts in Controlleraktionen integriert. Dies bedeutet, dass wir unsere Datenzugriffscode nicht ändern können, ohne zu unserem Controller ändern. In späteren Iterationen untersuchen wir die Software-Entwurfsmuster, die wir implementieren können, um Contact Manager so ändern Sie stabiler zu machen.

> [!div class="step-by-step"]
> [Zurück](iteration-7-add-ajax-functionality-cs.md)
> [Weiter](iteration-2-make-the-application-look-nice-vb.md)
