---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Neuigkeiten in Entitätsframework 4.0 | Microsoft-Dokumentation
author: tdykstra
description: Dieser tutorialreihe erstellt in der Contoso University-Webanwendung, die von den ersten Schritten mit der Entity Framework 4.0-Tutorial-Reihe erstellt wird. ICH...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 960aaf7aa7c2cff5c51079ecfe50031fa82d5508
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827840"
---
<a name="whats-new-in-the-entity-framework-40"></a>Neuerungen in Entitätsframework 4.0
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Dieser tutorialreihe erstellt, in der Contoso University-Webanwendung, die erstellt wird die [erste Schritte mit Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) Tutorial-Reihe. Wenn Sie den vorherigen Tutorials wurde nicht abgeschlossen haben, als Ausgangspunkt für dieses Tutorial können Sie [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , die Sie erstellt haben würden. Sie können auch [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , indem Sie die vollständige Reihe von Tutorials erstellt wird. Wenn Sie Fragen zu den Lernprogrammen haben, können Sie sie veröffentlichen das [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx).


Im vorherigen Tutorial haben Sie einige Methoden zur Steigerung der Leistung einer Webanwendung, die das Entity Framework verwendet. In diesem Tutorial werden einige der wichtigsten neuen Features in Version 4 von Entity Framework, und es enthält links zu Ressourcen, die eine umfassendere Einführung in alle der neuen Features bereitstellen. Die folgenden: Funktionen in diesem Tutorial hervorgehoben

- Foreign Key Zuordnungen.
- Ausführen von benutzerdefinierten SQL-Befehle.
- Model First-Entwicklung.
- POCO-Unterstützung.

Darüber hinaus wird das Tutorial eine kurze Einführung *Code-First-Entwicklung*, ein Feature, das in der nächsten Version von Entity Framework stammt.

Um das Lernprogramm zu starten, starten Sie Visual Studio, und öffnen Sie die Contoso University-Webanwendung, die Sie im vorherigen Tutorial verwendet wurden.

## <a name="foreign-key-associations"></a>Fremdschlüssel-Zuordnungen

Entity Framework, Version 3.5 enthalten Navigationseigenschaften, aber es nicht Fremdschlüssel-Eigenschaften im Datenmodell enthalten. Z. B. die `CourseID` und `StudentID` Spalten der `StudentGrade` Tabelle würde weggelassen werden die `StudentGrade` Entität.

[![Image01 abgerufen wird](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Der Grund für diesen Ansatz war, dass, genau genommen Fremdschlüssel einer physischen Implementierungsdetail und nicht in einem konzeptionellen Datenmodell gehören. Ein praktischer Tipp ist es jedoch oft einfacher, mit Entitäten im Code zu arbeiten, wenn Sie über direkten Zugriff auf die Fremdschlüssel verfügen.

Für ein Beispiel wie Fremdschlüssel im Datenmodell Ihren Code vereinfachen kann, erwägen Sie, wie Sie Code, müssten die *DepartmentsAdd.aspx* Seite ohne diese. In der `Department` Entität, die `Administrator` Eigenschaft ist ein Fremdschlüssel, der entspricht `PersonID` in die `Person` Entität. Um die Zuordnung zwischen einer neuen Abteilung und Administrator herzustellen, wurde Sie mussten Sie legen Sie den Wert für die `Administrator` -Eigenschaft in der `ItemInserting` -Ereignishandler des datengebundenen Steuerelements:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Ohne Fremdschlüssel im Datenmodell, verarbeiten Sie die `Inserting` Ereignis des Datenquellen-Steuerelements statt der `ItemInserting` Ereignis des datengebundenen Steuerelements, um einen Verweis auf die Entität selbst zu erhalten, bevor die Entität der Entitätssammlung hinzugefügt wird. Wenn Sie diesen Verweis verfügen, richten Sie die Zuordnung, die Verwendung von Code wie in den folgenden Beispielen:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Wie Sie in der Entity Framework-Team sehen [Blogbeitrag für Foreign Key-Zuordnungen](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), es gibt andere Situationen, in denen der Unterschied in der Komplexität von Code viel größer ist. Um die Anforderungen dieser erfüllen, die lieber mit Informationen zur Implementierung in das konzeptionelle Datenmodell aus Gründen der einfacheren Code Leben, können Sie das Entity Framework nun die Möglichkeit, einschließlich der Fremdschlüssel im Datenmodell.

In der Terminologie von Entity Framework, wenn Sie die Fremdschlüssel im Datenmodell enthalten Sie verwenden *fremdschlüsselzuordnungen*, und wenn Sie Fremdschlüssel ausschließen, Sie verwenden *unabhängigen Zuordnungen*.

## <a name="executing-user-defined-sql-commands"></a>Ausführen von benutzerdefinierten SQL-Befehlen

In früheren Versionen von Entity Framework gab es keine einfache Möglichkeit, eigene SQL-Befehle im Handumdrehen erstellen und führen Sie sie. Das Entity Framework dynamisch generierte SQL-Befehle für Sie oder mussten Sie eine gespeicherte Prozedur erstellen und importieren sie als eine Funktion. Fügt der Version 4 `ExecuteStoreQuery` und `ExecuteStoreCommand` Methoden der `ObjectContext` -Klasse, die für alle Abfragen direkt an die Datenbank übergeben erleichtern.

Angenommen Sie, Sie möchten, dass Administratoren der Contoso University in der Datenbank massenänderungen ausführen, ohne den Prozess zum Erstellen einer gespeicherten Prozedur, und sie in das Datenmodell importieren durchlaufen kann. Die erste Anforderung ist für eine Seite, die die Anzahl der Credits für alle Kurse in der Datenbank ändern kann. Auf der Webseite, sie möchten eine Zahl zu verwenden, um den Wert der zu multiplizierende eingeben jedes `Course` Zeile `Credits` Spalte.

Erstellen Sie eine neue Seite, verwendet der *Site.Master* Masterseite, und nennen Sie sie *UpdateCredits.aspx*. Klicken Sie dann das folgende Markup zum Hinzufügen der `Content` Steuerelement mit dem Namen `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Dieses Markup erstellt eine `TextBox` Steuerelement, in dem der Benutzer kann den Multiplikatorwert eingeben, einer `Button` Steuerelement klicken, um den Befehl ausführen und eine `Label` -Steuerelement für die Anzahl der betroffenen Zeilen angibt.

Open *UpdateCredits.aspx.cs*, und fügen Sie die folgenden `using` -Anweisung und einen Handler für der Schaltfläche " `Click` Ereignis:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Dieser Code führt die SQL-Anweisung `Update` Befehl mit dem Wert in das Textfeld ein, und die Bezeichnung wird verwendet, um die Anzahl der betroffenen Zeilen anzuzeigen. Führen Sie vor dem Ausführen der Seite die *Courses.aspx* Seite, um ein Bild "before", der einige Daten abzurufen.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Führen Sie *UpdateCredits.aspx*, geben Sie als der Multiplikator, der "10", und klicken Sie dann auf **Execute**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Führen Sie die *Courses.aspx* Seite erneut, um die geänderten Daten anzuzeigen.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Wenn Sie die Anzahl der Credits im zurück an ihre ursprünglichen Werte festlegen möchten *UpdateCredits.aspx.cs* ändern `Credits * {0}` zu `Credits / {0}` und führen Sie die Seite, die 10 eingeben, wie des Divisors erneut aus.)

Weitere Informationen zum Ausführen von Abfragen, die Sie im Code definieren, finden Sie unter [Vorgehensweise: direkt führen Befehle für die Datenquelle](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Model-First-Entwicklung

In diesen exemplarischen Vorgehensweisen müssen Sie zuerst die Datenbank erstellt und anschließend das Datenmodell entsprechend der Datenbankstruktur generiert. Im Entity Framework 4 können Sie beginnen mit dem Datenmodell und die Datenbank basierend auf der Modell-Datenstruktur generieren. Wenn Sie eine Anwendung erstellen für die Datenbank noch nicht vorhanden ist, ermöglicht der Model-First-Ansatz Ihnen die Erstellung von Entitäten und Beziehungen, die vom Konzept her für die Anwendung, während keine Gedanken um Details zur physischen Implementierung sinnvoll . (Dies trifft nur über den ersten Phasen der Entwicklung, jedoch. Schließlich die Datenbank erstellt werden und muss sich Produktionsdaten, und erstellen Sie Sie neu aus dem Modell werden nicht mehr praktikabel; an diesem Punkt werden Sie an der Datenbank-First-Ansatz sein.)

In diesem Abschnitt des Tutorials Sie erstellen ein einfaches Datenmodell und generieren die Datenbank aus.

In **Projektmappen-Explorer**, mit der rechten Maustaste die *DAL* Ordner, und wählen **neues Element hinzufügen**. In der **neues Element hinzufügen** Dialogfeld **installierte Vorlagen** wählen **Daten** und wählen Sie dann die **ADO.NET Entity Data Model** Vorlage . Nennen Sie die neue Datei *AlumniAssociationModel.edmx* , und klicken Sie auf **hinzufügen**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Dadurch wird der Assistent für Entity Data Model. In der **auswählen des Modellinhalts** Schritt select **leeres Modell** , und klicken Sie dann auf **Fertig stellen**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

Die **Entity Data Model Designer** wird mit einer leeren Entwurfsoberfläche geöffnet. Ziehen Sie ein **Entität** Element aus der **Toolbox** auf die Entwurfsoberfläche.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Ändern Sie den Entitätsnamen aus `Entity1` zu `Alumnus`, ändern Sie die `Id` Eigenschaftenname `AlumnusId`, und fügen Sie eine neue skalare Eigenschaft mit dem Namen `Name`. Zum Hinzufügen neuer Eigenschaften können Sie die EINGABETASTE drücken, nach dem Ändern des Namens der `Id` Spalte oder mit der rechten Maustaste in der Entitäts, und wählen Sie **hinzufügen Skalareigenschaft**. Der Standardtyp für die neuen Eigenschaften ist `String`, die für diese einfache Demo in Ordnung ist, aber Sie können natürlich ändern, z.B.-Datentyp in der **Eigenschaften** Fenster.

Erstellen Sie eine andere Entität die gleiche Weise, und nennen Sie sie `Donation`. Ändern der `Id` Eigenschaft `DonationId` und Hinzufügen einer skalaren Eigenschaft mit dem Namen `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Um eine Zuordnung zwischen diesen beiden Entitäten hinzuzufügen, Informationen zu diesem mit der rechten Maustaste die `Alumnus` Entität wählen **hinzufügen**, und wählen Sie dann **Zuordnung**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Die Standardwerte den **Zuordnung hinzufügen** im Dialogfeld werden Sie den gewünschten (1-n-, umfassen Navigationseigenschaften, Fremdschlüssel), also einfach auf **OK**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Der Designer Fügt eine Zuordnungslinie sowie eine foreign Key-Eigenschaft.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Sie nun können zum Erstellen der Datenbank. Mit der rechten Maustaste Entwurfsoberfläche, und wählen **Datenbank aus Modell generieren**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Dadurch wird der Assistent zur Datenbankgenerierung gestartet. (Wenn Warnungen angezeigt wird, die angeben, dass die Entitäten nicht zugeordnet sind, können Sie diese vorerst ignorieren.)

In der **wählen Sie Ihre Datenverbindung** auf **neue Verbindung**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

In der **Verbindungseigenschaften** Dialogfeld Feld, wählen Sie die lokale SQL Server Express-Instanz aus, und nennen Sie die Datenbank `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Klicken Sie auf **Ja** Wenn Sie gefragt, ob die Datenbank erstellt werden soll. Wenn die **wählen Sie Ihre Datenverbindung** Schritt erneut angezeigt wird, klicken Sie auf **Weiter**.

In der **Zusammenfassung und Einstellungen** auf **Fertig stellen**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Ein *.sql* -Datei mit den Befehlen für Data Definition Language (DDL) erstellt, aber die Befehle wurden noch nicht ausgeführt.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Verwenden Sie ein Tool wie z. B. **SQL Server Management Studio** führen Sie das Skript aus, und Erstellen von Tabellen, wie Sie getan haben können, bei der Erstellung der `School` -Datenbank für [im ersten Tutorial der erste Schritte-Tutorial-Reihe ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (Es sei denn, Sie die Datenbank heruntergeladen.)

Nun können Sie die `AlumniAssociation` Datenmodell in Ihrer Web-Seiten die gleiche Weise, die Sie haben mithilfe der `School` Modell. Um dies zu testen, fügen Sie einige Daten in die Tabellen aus, und erstellen Sie eine Webseite, die Daten anzeigt.

Mithilfe von **Server-Explorer**, fügen Sie die folgenden Zeilen, die `Alumnus` und `Donation` Tabellen.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Erstellen einer neuen Webseite mit dem Namen *Alumni.aspx* , verwendet der *Site.Master* Masterseite. Fügen Sie das folgende Markup, das `Content` Steuerelement mit dem Namen `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Dieses Markup erstellt geschachtelte `GridView` Steuerelemente, die äußere zum Anzeigen von Alumni Namen und den inneren Spende und-Beträge angezeigt.

Open *Alumni.aspx.cs*. Hinzufügen einer `using` -Anweisung für die Daten zugreifen, Ebene und einem Handler für das äußere `GridView` des Steuerelements `RowDataBound` Ereignis:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Dieser Code stellt die innere `GridView` steuern mit der `Donations` Navigationseigenschaft, die von der aktuellen Zeile `Alumnus` Entität.

Führen Sie die Seite.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Hinweis: auf dieser Seite befindet sich im herunterladbaren Projekt aus, damit Sie funktioniert muss die Datenbank in Ihrer lokalen SQL Server Express-Instanz erstellen, jedoch die Datenbank ist nicht als ein *mdf* Datei die *App\_ Daten* Ordner.)

Weitere Informationen zur Verwendung von Entity Framework Model First finden Sie unter [Model First im Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>POCO-Unterstützung

Bei Verwendung von domänengesteuertem designmethodik entwerfen Sie Datenklassen, die Darstellung von Daten und das Verhalten, die mit dem Geschäftsbereich relevant sind. Diese Klassen sollten unabhängig von der spezifischen Technologie zum Speichern verwendet werden (beibehalten) der Daten Das heißt, sie muss *Persistenzignoranz*. Ignorieren der Persistenz kann außerdem erstellen Sie eine Klasse einfacher Komponententests unterziehen da das Komponententestprojekt beliebige persistenztechnologie am einfachsten zu Testzwecken ist verwenden kann. Frühere Versionen von Entity Framework angeboten eingeschränkte Unterstützung für Persistenzignoranz, da musste, dass die Entitätsklassen erben die `EntityObject` Klasse, und daher eine große Menge an Entity Framework-spezifischen Funktionen enthalten.

Entity Framework 4 führt die Möglichkeit, Entitätsklassen zu verwenden, die von erben nicht die `EntityObject` Klasse und aus diesem Grund sind Dauerhaftigkeit ignorierende. Im Kontext von Entity Framework-Klassen wie dies in der Regel heißen *einfache alte CLR-Objekte* (POCO oder POCOs). Sie können POCO-Klassen manuell schreiben oder Sie können anhand eines vorhandenen Datenmodells mithilfe von Entity Framework bereitgestellten Text Template Transformation Toolkit (T4)-Vorlagen automatisch generieren.

Weitere Informationen zur Verwendung von POCOs in Entity Framework finden Sie unter den folgenden Ressourcen:

- [Arbeiten mit POCO-Entitäten](https://msdn.microsoft.com/library/dd456853.aspx). Dies ist ein MSDN-Dokument, das eine Übersicht über die POCOs, mit Links zu anderen Dokumenten, die detailliertere Informationen haben.
- [Exemplarische Vorgehensweise: POCO-Vorlage für das Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) Dies ist ein Blogbeitrag des Teams für das Entity Framework-Entwicklung, mit Links zu anderen Blogbeiträge zu den POCOs.

## <a name="code-first-development"></a>Code-First-Entwicklung

POCO-Unterstützung in Entity Framework 4 erfordert, dass Sie ein Datenmodell erstellen und verknüpfen die Entitätsklassen des Datenmodells. Die nächste Version von Entity Framework umfasst ein Feature namens *Code-First-Entwicklung*. Dieses Feature können Sie das Entity Framework mit Ihren eigenen POCO-Klassen nutzen, ohne die Data Model-Designer oder Modell XML-Datendatei verwenden. (Aus diesem Grund diese Option auch aufgerufen wurde *reinen*; *Code First* und *reinen* beide verweisen auf die gleiche Entity Framework-Funktion.)

Weitere Informationen zur Verwendung der Code First-Ansatz zur Entwicklung finden Sie unter den folgenden Ressourcen:

- [Code-First-Entwicklung mit Entitätsframework 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Dies ist ein Blogbeitrag von Scott Guthrie Einführung in Code First-Entwicklung.
- [Entity Framework Development-Team-Blog - Beiträge markierten CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Entity Framework Development-Team-Blog - Beiträge markierten Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [MVC Music Store-Tutorial – Teil 4: Modelle und Datenzugriff](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Erste Schritte mit MVC 3 – Teil 4: Entity Framework Code First-Entwicklung](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Darüber hinaus wird ein neues MVC-Code-First-Tutorial, in dem eine Anwendung, die ähnlich der Contoso University-Anwendung erstellt voraussichtlich im Frühjahr 2011 am veröffentlicht werden [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Weitere Informationen

Dadurch wird die Übersicht, was neu im Entity Framework und dieser Vorgang wird fortgesetzt, mit der Entity Framework-Tutorial-Reihe ist abgeschlossen. Weitere Informationen zu neuen Funktionen in Entity Framework 4, die hier nicht abgedeckt sind, finden Sie unter den folgenden Ressourcen:

- [Neues in ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) MSDN-Thema zu neuen Features in Version 4 von Entity Framework.
- [Ankündigung der Version von Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) des Entwicklungsteams Entity Framework-Blogbeitrag zu neuen Funktionen in Version 4.

> [!div class="step-by-step"]
> [Vorherige](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
