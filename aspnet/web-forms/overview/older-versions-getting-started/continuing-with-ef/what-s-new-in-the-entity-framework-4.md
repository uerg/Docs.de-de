---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Neuigkeiten in Entity Framework 4.0 | Microsoft Docs
author: tdykstra
description: Diese Reihe von Lernprogrammen baut auf der Contoso-University-Webanwendung, die von den ersten Schritten mit der Entity Framework 4.0 Tutorial Reihe erstellt wird. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 04444ce98fa60045cf617a6c518dd55677258148
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="whats-new-in-the-entity-framework-40"></a>Was ist neu im Entity Framework 4.0
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Diese Reihe von Lernprogrammen in der Contoso-University Webanwendung durch die erstellte builds der [erste Schritte mit dem Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) Reihe von Lernprogrammen. Wenn Sie die frühere Lernprogramme nicht abgeschlossen wurde, als Ausgangspunkt für dieses Lernprogramm können Sie [Herunterladen der Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , die Sie erstellt haben würden. Sie können auch [Herunterladen der Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , die durch das vollständige Lernprogramm Reihe erstellt wird. Wenn Sie Fragen zu den Lernprogrammen haben, können Sie stellen Sie diese auf die [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx).


Im vorherigen Lernprogramm haben Sie gesehen, einige Methoden zum Maximieren der Leistung einer Webanwendung, die das Entity Framework verwendet wird. In diesem Lernprogramm überprüft einige der wichtigsten neuen Funktionen in Entity Framework, Version 4, und es enthält links zu Ressourcen, die eine umfassendere Einführung in alle neuen Funktionen zu bieten. Die Funktionen, die in diesem Lernprogramm hervorgehoben umfassen Folgendes:

- Foreign Key-Zuordnungen.
- Ausführen von benutzerdefinierten SQL-Befehle.
- Model First-Entwicklung.
- POCO-Unterstützung.

Darüber hinaus wird das Lernprogramm eine kurze Einführung *Code-First-Entwicklung*, eine Funktion, die in der nächsten Version von Entity Framework stammt.

Um das Lernprogramm zu starten, starten Sie Visual Studio, und öffnen Sie die Contoso University-Web-Anwendung, die Sie im vorherigen Lernprogramm gearbeitet haben.

## <a name="foreign-key-associations"></a>Foreign Key-Zuordnungen

Fremdschlüssel-Eigenschaften nicht im Datenmodell enthalten, jedoch das Entity Framework, Version 3.5 enthalten Navigationseigenschaften. Z. B. die `CourseID` und `StudentID` Spalten von der `StudentGrade` Tabelle würde weggelassen werden, aus der `StudentGrade` Entität.

[![Image01 abgerufen wird](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Der Grund für diesen Ansatz war, dass der streng genommen ist Fremdschlüssel einer physischen Implementierungsdetail beibehalten werden und nicht in einem Konzeptdatenmodell gehören. Aus praktischen Gründen ist es jedoch häufig einfacher, arbeiten mit Entitäten im Code, wenn Sie direkten Zugriff auf die Fremdschlüssel aufweisen.

Für ein Beispiel wie Fremdschlüssel in das Datenmodell Code vereinfachen kann, erwägen Sie, wie Sie Code hätte die *DepartmentsAdd.aspx* Seite ohne sie. In der `Department` Entität, die `Administrator` Eigenschaft ist ein Fremdschlüssel, der entspricht `PersonID` in der `Person` Entität. Gewahrt für die Zuordnung zwischen einer neuen Abteilung und der Administrator wurde alle Sie musste legen Sie den Wert für die `Administrator` Eigenschaft in der `ItemInserting` -Ereignishandler des datengebundenen Steuerelements:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Ohne die Fremdschlüssel im Datenmodell, behandeln Sie die `Inserting` -Ereignis für das Datenquellensteuerelement statt der `ItemInserting` Ereignis des datengebundenen Steuerelements, um einen Verweis auf die Entität selbst zu erhalten, bevor die Entität der Entitätssammlung hinzugefügt wird. Wenn Sie diesen Verweis haben, richten Sie die Zuordnung, die Verwendung von Code wie in den folgenden Beispielen:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Siehe des Entity Framework-Teams [Blogbeitrag auf Foreign Key-Zuordnungen](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), stehen Sie anderen Fällen, in denen die Differenz in Codekomplexität viel größer ist. Um die Bedürfnisse erfüllen, die lieber mit Details zur Implementierung im konzeptionellen Datenmodell aus Gründen der einfacher Code live, können Sie das Entity Framework nun die Möglichkeit, einschließlich der Fremdschlüssel im Datenmodell.

In Entity Framework-Terminologie, wenn Sie die Fremdschlüssel im Datenmodell enthalten Sie verwenden *foreign Key-Zuordnungen*, und wenn Sie Fremdschlüssel auszuschließen, Sie verwenden *unabhängige Zuordnungen*.

## <a name="executing-user-defined-sql-commands"></a>Ausführen von benutzerdefinierten SQL-Befehlen

In früheren Versionen von Entity Framework gab es keine einfache Möglichkeit, eine eigene SQL-Befehle auf einfache Weise erstellen und diese ausführen. Das Entity Framework dynamisch generierte SQL-Befehle für Sie oder mussten Sie eine gespeicherte Prozedur erstellen und importieren Sie es als eine Funktion. Fügt der Version 4 `ExecuteStoreQuery` und `ExecuteStoreCommand` Methoden der `ObjectContext` -Klasse, die für die Übergabe einer beliebigen Abfrage direkt an die Datenbank zu vereinfachen.

Angenommen Sie, Sie möchten, dass Contoso University Administratoren in der Datenbank massenänderungen ausführen, ohne den Prozess zum Erstellen einer gespeicherten Prozedur und importieren es in das Datenmodell durchlaufen kann. Die erste Anforderung ist für eine Seite, in dem sie die Anzahl der Gutschriften für alle Kurse in der Datenbank ändern kann. Auf der Webseite, möchten sie zu verwenden, um den Wert der zu multiplizierende Zahl eingeben können alle `Course` Zeile `Credits` Spalte.

Erstellen Sie eine neue Seite, verwendet der *Site.Master* Masterseite, und nennen Sie sie *UpdateCredits.aspx*. Das folgende Markup auf Grundlage des domänenwissens der `Content` Steuerelement namens `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Dieses Markup erstellt einen `TextBox` Steuerelement, in dem der Benutzer kann den Multiplikatorwert eingeben, einer `Button` Steuerelement klicken, um den Befehl auszuführen und eine `Label` -Steuerelement für die Anzahl der betroffenen Zeilen angibt.

Open *UpdateCredits.aspx.cs*, und fügen Sie die folgenden `using` -Anweisung und einen Handler für der Schaltfläche `Click` Ereignis:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Dieser Code führt die SQL `Update` Befehl mit dem Wert in das Textfeld und verwendet die Bezeichnung, um die Anzahl der betroffenen Zeilen anzuzeigen. Führen Sie vor dem Ausführen der Seite die *Courses.aspx* Seite, um ein Bild "vor" einiger Daten abzurufen.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Führen Sie *UpdateCredits.aspx*, geben Sie als Multiplikator für die "10", und klicken Sie dann auf **Execute**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Führen Sie die *Courses.aspx* Seite erneut, um die geänderten Daten anzuzeigen.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Wenn Sie die Anzahl der Gutschriften zurück in ihre ursprünglichen Werte festlegen möchten *UpdateCredits.aspx.cs* ändern `Credits * {0}` auf `Credits / {0}` und führen Sie die Seite, die 10 eingeben, wie des Divisors erneut aus.)

Weitere Informationen zum Ausführen von Abfragen, die Sie im Code definieren, finden Sie unter [Vorgehensweise: direkt führen Befehle für die Datenquelle](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Model First-Entwicklung

In diesen exemplarischen Vorgehensweisen müssen Sie zuerst die Datenbank erstellt und anschließend generiert das Datenmodell basierend auf der Datenbankstruktur. In Entity Framework 4 können Sie stattdessen mit dem Datenmodell starten und der Datenbank, basierend auf der Modellstruktur Daten generieren. Wenn Sie eine Anwendung erstellen, für die Datenbank bereits vorhanden ist, ermöglicht der Model First-Ansatz Erstellen von Entitäten und Beziehungen, die sinnvoll konzeptionell für die Anwendung, während nicht kümmern physischen Implementierungsdetails . (Dies gilt nur in den ersten Phasen der Entwicklung, jedoch. Schließlich die Datenbank erstellt werden und muss Produktionsdaten darin, und erstellen Sie Sie neu aus dem Modell nicht mehr praktikabel; an diesem Punkt werden Sie an der Datenbank-First-Ansatz sein.)

In diesem Abschnitt des Lernprogramms Sie erstellen ein einfaches Datenmodell und die Datenbank daraus generieren.

In **Projektmappen-Explorer**, mit der rechten Maustaste die *DAL* Ordner, und wählen **neues Element hinzufügen**. In der **neues Element hinzufügen** Dialogfeld unter **installierte Vorlagen** wählen **Daten** und wählen Sie dann die **ADO.NET Entity Data Model** Vorlage . Nennen Sie die neue Datei *AlumniAssociationModel.edmx* , und klicken Sie auf **hinzufügen**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Dadurch wird das Entity Data Model-Assistenten gestartet. In der **Modellinhalte** Schritt wählen **leeres Modell** , und klicken Sie dann auf **Fertig stellen**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

Die **Entity Data Model Designer** wird mit einer leeren Entwurfsoberfläche geöffnet. Ziehen Sie ein **Entität** Element aus der **Toolbox** auf die Entwurfsoberfläche.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Ändern Sie den Entitätsnamen aus `Entity1` auf `Alumnus`, ändern Sie die `Id` Eigenschaftennamen, die `AlumnusId`, und fügen eine neue skalare Eigenschaft mit dem Namen `Name`. Um neue Eigenschaften hinzufügen, können Sie die EINGABETASTE drücken, nach dem Ändern des Namens der `Id` Spalte oder mit der rechten Maustaste in der Entität, und wählen Sie **skalare Eigenschaft hinzufügen**. Der Standardtyp für neue Eigenschaften ist `String`, also gut für dieses einfache Demo, aber natürlich können Sie ändern, z. B.-Datentyp in der **Eigenschaften** Fenster.

Erstellen Sie eine andere Entität die gleiche Weise, und nennen Sie sie `Donation`. Ändern der `Id` Eigenschaft `DonationId` und fügen Sie eine skalare Eigenschaft namens `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Zum Hinzufügen einer Zuordnung zwischen diesen beiden Entitäten Maustaste die `Alumnus` Entität select **hinzufügen**, und wählen Sie dann **Zuordnung**.

[![image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Die Standardwerte den **Zuordnung hinzufügen** Dialogfeld vorstellungen entsprechen (1-zu-viele Navigationseigenschaften einschließen, Fremdschlüssel enthalten), also nur auf **OK**.

[![image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Der Designer Fügt eine Assoziationslinie und eine foreign Key-Eigenschaft.

[![image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Nun können Sie die Datenbank zu erstellen. Mit der rechten Maustaste in des Entwurfs, die Entwurfsoberfläche, und wählen **zur Datenbankgenerierung aus Modell**.

[![image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Dies startet den Assistenten zum Generieren einer Datenbank. (Wenn Sie Warnungen, die darauf hinweisen anzeigen, dass die Entitäten zugeordnet sind, können Sie die wird ignorieren.)

In der **wählen Sie Ihre Datenverbindung** Schritt, klicken Sie auf **neue Verbindung**.

[![image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

In der **Verbindungseigenschaften** Dialogfeld Feld, wählen Sie die lokale SQL Server Express-Instanz aus, und nennen Sie die Datenbank `AlumniAsssociation`.

[![image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Klicken Sie auf **Ja** Wenn Sie gefragt, ob die Datenbank erstellt werden soll. Wenn die **wählen Sie Ihre Datenverbindung** Schritt erneut angezeigt wird, klicken Sie auf **Weiter**.

In der **Zusammenfassung und Einstellungen** Schritt, klicken Sie auf **Fertig stellen**.

[![image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Ein *.sql* -Datei mit den Befehlen Data Definition Language (DDL) erstellt, aber noch nicht die Befehle noch ausgeführt wurde.

[![image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Verwenden Sie ein Tool wie z. B. **SQL Server Management Studio** führen Sie das Skript, und erstellen Sie die Tabellen aus, wie dies möglicherweise beim Erstellen geschehen ist der `School` -Datenbank für den [im ersten Lernprogramm Getting Started Tutorial Reihe ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (Es sei denn, Sie die Datenbank heruntergeladen haben.)

Jetzt können Sie die `AlumniAssociation` Datenmodell in Ihre Web-Seiten die gleiche Weise haben Sie verwendet wurden die `School` Modell. Um dies zu testen, fügen Sie einige Daten in die Tabellen aus, und erstellen Sie eine Webseite, die die Daten anzeigt.

Mit **Server-Explorer**, fügen Sie die folgenden Zeilen, die `Alumnus` und `Donation` Tabellen.

[![image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Erstellen Sie eine neue Webseite mit dem Namen *Alumni.aspx* , verwendet der *Site.Master* Masterseite. Fügen Sie das folgende Markup zum Rendern der `Content` Steuerelement namens `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Dieses Markup erstellt geschachtelte `GridView` Steuerelemente, die die äußeren Alumni Anzeigenamen und den inneren Spende und-Beträge angezeigt.

Open *Alumni.aspx.cs*. Hinzufügen einer `using` -Anweisung für die Daten zugreifen, Ebene und einem Handler für das äußere `GridView` des Steuerelements `RowDataBound` Ereignis:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Dieser Code stellt die innere `GridView` steuern Sie mithilfe der `Donations` Navigationseigenschaft, die von der aktuellen Zeile `Alumnus` Entität.

Führen Sie die Seite.

[![image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Hinweis: Diese Seite ist im herunterladbaren Projekt enthalten, aber Sie funktionieren müssen die Datenbank in der lokalen SQL Server Express-Instanz erstellen; die Datenbank ist nicht als ein *mdf* in der Datei die *App\_ Daten* Ordner.)

Weitere Informationen zur Verwendung von Entity Framework die Model First-Funktion finden Sie unter [Model First in Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>POCO-Unterstützung

Bei Verwendung von Domain driven Design Methodik entwerfen Sie Datenklassen, die darstellen, Daten und das Verhalten, die relevant für die Business-Domäne ist. Diese Klassen sollten unabhängig von einer bestimmten Technologie zum Speichern verwendet werden (beibehalten) der Daten. Das heißt, sie muss *keine Persistenz*. Persistenz Unkenntnis kann auch einfacher eine Klasse Komponententest da das Komponententestprojekt verwenden kann, den Persistenz-Technologie am einfachsten zu Testzwecken ist. Frühere Versionen von Entity Framework angeboten eingeschränkte Unterstützung für Persistenz Unkenntnis aus, da Entitätsklassen erben musste die `EntityObject` Klasse, und daher sehr viel systemverarbeitungszeit in Entity Framework-spezifischen Funktionen enthalten.

Das Entity Framework 4 ermöglicht den Zugriff auf die Entitätsklassen verwenden, die von erben nicht die `EntityObject` Klasse und Persistenz sind daher ignoriert. Im Rahmen des Entity Framework-Klassen wie dies in der Regel aufgerufen *Plain-Old CLR Objects* (POCO oder POCOs). POCO-Klassen können manuell schreiben, oder Sie können basierend auf der ein vorhandenes Data-Modell mithilfe von Text Template Transformation Toolkit Toolkit (T4)-Vorlagen, die vom Entity Framework bereitgestellten automatisch generieren.

Weitere Informationen zur Verwendung von POCOs im Entity Framework finden Sie unter den folgenden Ressourcen:

- [Arbeiten mit POCO-Entitäten](https://msdn.microsoft.com/library/dd456853.aspx). Dies ist ein MSDN-Dokument, das eine Übersicht über POCOs, mit Links zu anderen Dokumenten, die detailliertere Informationen haben.
- [Exemplarische Vorgehensweise: POCO-Vorlage für das Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) Dies ist das Entity Framework-Entwicklungsteam mit Links zu anderen Blogbeiträge zu POCOs eine finden Sie im Blogbeitrag.

## <a name="code-first-development"></a>Code First-Entwicklung

POCO-Unterstützung in Entity Framework 4 erfordert, dass Sie ein Datenmodell erstellen und verknüpfen die Entitätsklassen in das Datenmodell. Die nächste Version von Entity Framework enthält eine Funktion namens *Code-First-Entwicklung*. Diese Funktion können Sie das Entity Framework mit Ihren eigenen POCO-Klassen verwenden, ohne dem Modell-Designer oder Modell XML-Datendatei zu verwenden. (Daher, diese Option auch aufgerufen wurde *reinen*; *Code First* und *reinen* beide verweisen auf die gleiche Entity Framework-Funktion.)

Weitere Informationen zum Verwenden der Code First-Ansatz für die Entwicklung, finden Sie unter den folgenden Ressourcen:

- [Code First-Entwicklung mit Entity Framework 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Dies ist ein Blogbeitrag von Scott Guthrie Einführung zum Code First-Entwicklung.
- [Entity Teamblogs Framework-Entwicklung - markierten CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Entity Blogbeiträge Framework-Entwicklung Team - markierten Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [MVC-Music Store-Lernprogramm – Teil 4: Modelle und Datenzugriff](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Erste Schritte mit MVC 3 – Teil 4: Entity Framework Code First-Entwicklung](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Darüber hinaus ein neues MVC Code First-Lernprogramm, in dem eine Anwendung ähnelt der Universität von Contoso-Anwendung erstellt projiziert wird, um in der-Version vom Frühjahr 2011 am veröffentlicht werden [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Weitere Informationen

Dies schließt die Übersicht, was neu in der Entity Framework und diese Fortsetzen der Tutorial Entity Framework-Reihe ist. Weitere Informationen zu neuen Funktionen in Entity Framework 4, die hier nicht abgedeckt sind, finden Sie unter den folgenden Ressourcen:

- [Neuigkeiten in ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) MSDN-Thema zu neuen Funktionen in Version 4 von Entity Framework.
- [Ankündigung der Version von Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) das Entity Framework-Entwicklungsteam finden Sie im Blogbeitrag zu neuen Funktionen in Version 4.

> [!div class="step-by-step"]
> [Vorherige](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
