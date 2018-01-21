---
title: ASP.NET Core MVC mit EF-Kern - Datenmodell - 5, 10
author: tdykstra
description: "In diesem Lernprogramm fügen Sie weitere Entitäten und Beziehungen und das Datenmodell anpassen, indem Sie Formatierung, Überprüfung und Zuordnungsregeln für die Datenbank angeben."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: 5b5645936504333573950b5bd17f5a037ffd984f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a><span data-ttu-id="39fd6-103">Erstellen ein Modell mit komplexen Daten - EF-Core mit ASP.NET Core MVC-Lernprogramm (5 10)</span><span class="sxs-lookup"><span data-stu-id="39fd6-103">Creating a complex data model - EF Core with ASP.NET Core MVC tutorial (5 of 10)</span></span>

<span data-ttu-id="39fd6-104">Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="39fd6-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="39fd6-105">Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen, die mit Entity Framework Core und Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39fd6-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="39fd6-106">Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](intro.md).</span><span class="sxs-lookup"><span data-stu-id="39fd6-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="39fd6-107">In den vorherigen Lernprogrammen, die Sie mit der ein einfaches Datenmodell, das aus drei Entitäten zusammengesetzt wurde gearbeitet haben.</span><span class="sxs-lookup"><span data-stu-id="39fd6-107">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="39fd6-108">In diesem Lernprogramm fügen Sie weitere Entitäten und Beziehungen, und Sie müssen das Datenmodell anpassen, indem Sie Formatierung, Überprüfung und Zuordnungsregeln für die Datenbank angeben.</span><span class="sxs-lookup"><span data-stu-id="39fd6-108">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="39fd6-109">Wenn Sie fertig sind, werden die Entitätsklassen abgeschlossenen Datenmodell bilden, die in der folgenden Abbildung gezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="39fd6-109">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![Entitätsdiagramm](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="39fd6-111">Anpassen des Datenmodells mithilfe von Attributen</span><span class="sxs-lookup"><span data-stu-id="39fd6-111">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="39fd6-112">In diesem Abschnitt sehen Sie zum Anpassen des Datenmodells mithilfe von Attributen, die die Formatierung, Überprüfung und Zuordnungsregeln für die Datenbank angeben.</span><span class="sxs-lookup"><span data-stu-id="39fd6-112">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="39fd6-113">Klicken Sie dann Attribute in mehreren der folgenden Abschnitte erstellen Sie nun das vollständige Modell "School" Daten durch Hinzufügen von den Klassen Sie bereits erstellt haben, und erstellen neue Klassen für die verbleibenden Entitätstypen im Modell.</span><span class="sxs-lookup"><span data-stu-id="39fd6-113">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="39fd6-114">Das DataType-Attribut</span><span class="sxs-lookup"><span data-stu-id="39fd6-114">The DataType attribute</span></span>

<span data-ttu-id="39fd6-115">Für Student Registrierung Datumsangaben Anzeigen aller Web Pages derzeit der Zeit zusammen mit dem Datum, obwohl alle, können Sie für dieses Feld ist das Datum.</span><span class="sxs-lookup"><span data-stu-id="39fd6-115">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="39fd6-116">Mit Datenattributen für die Anmerkung, stellen Sie eine codeänderung, die das Anzeigeformat in jeder Ansicht behoben werden, die Daten anzeigt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-116">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="39fd6-117">Sehen Sie ein Beispiel dazu, dass Sie ein Attribut zum Hinzufügen der `EnrollmentDate` Eigenschaft in der `Student` Klasse.</span><span class="sxs-lookup"><span data-stu-id="39fd6-117">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="39fd6-118">In *Models/Student.cs*, Hinzufügen einer `using` -Anweisung für die `System.ComponentModel.DataAnnotations` Namespace und fügen `DataType` und `DisplayFormat` -Attribute verwenden, um die `EnrollmentDate` -Eigenschaft verwenden, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="39fd6-118">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="39fd6-119">Das `DataType`-Attribut wird verwendet, um einen Datentyp anzugeben, der spezifischer als der datenbankinterne Typ ist.</span><span class="sxs-lookup"><span data-stu-id="39fd6-119">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="39fd6-120">In diesem Fall möchten wir nur zum Nachverfolgen der Datum nicht das Datum und die Uhrzeit.</span><span class="sxs-lookup"><span data-stu-id="39fd6-120">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="39fd6-121">Die `DataType` Enumeration stellt für viele Datentypen, wie Datum, Uhrzeit, Währung, PhoneNumber, EmailAddress und vieles mehr.</span><span class="sxs-lookup"><span data-stu-id="39fd6-121">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="39fd6-122">Das `DataType`-Attribut kann der Anwendung auch ermöglichen, typspezifische Features bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-122">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="39fd6-123">Beispielsweise kann ein `mailto:`-Link für `DataType.EmailAddress` erstellt werden, und eine Datumsauswahl kann in Browsern mit Unterstützung für HTML5 für `DataType.Date` bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="39fd6-123">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="39fd6-124">Die `DataType` Attribut ausgibt HTML 5 `data-` (ausgesprochen als Daten Dash) Attribute, die HTML 5-Browser zu bringen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="39fd6-125">Die `DataType` Attribute bieten keine Validierung.</span><span class="sxs-lookup"><span data-stu-id="39fd6-125">The `DataType` attributes do not provide any validation.</span></span>

<span data-ttu-id="39fd6-126">`DataType.Date` gibt nicht das Format des Datums an, das angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="39fd6-126">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="39fd6-127">Standardmäßig wird das Feld "Daten" entsprechend der Standardformate basierend auf dem Server CultureInfo angezeigt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-127">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="39fd6-128">Das `DisplayFormat`-Attribut dient zum expliziten Angeben des Datumsformats:</span><span class="sxs-lookup"><span data-stu-id="39fd6-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="39fd6-129">Die Einstellung `ApplyFormatInEditMode` gibt an, dass die Formatierung auch angewendet werden soll, wenn der Wert in einem Textfeld zur Bearbeitung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="39fd6-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="39fd6-130">(Sie möchten nicht, dass für einige Felder – z. B. für Währungsangaben, nicht das Währungssymbol in das Textfeld für die Bearbeitung empfehlenswert.)</span><span class="sxs-lookup"><span data-stu-id="39fd6-130">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="39fd6-131">Können Sie die `DisplayFormat` Attribut von selbst, aber es ist im Allgemeinen empfiehlt sich, verwenden Sie die `DataType` auch Attribut.</span><span class="sxs-lookup"><span data-stu-id="39fd6-131">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="39fd6-132">Die `DataType` Attribut übermittelt, die die Semantik der Daten im Gegensatz zu wie auf dem Bildschirm gerendert werden soll, und bietet die folgenden Vorteile, die Sie nicht mit erhalten `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="39fd6-132">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="39fd6-133">Der Browser HTML5-Funktionen aktivieren kann (z. B. zum Anzeigen eines Kalendersteuerelements, dem Gebietsschema entsprechende Währungssymbol, Links per e-Mail einige clientseitige Eingabe Validierung usw.).</span><span class="sxs-lookup"><span data-stu-id="39fd6-133">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="39fd6-134">Standardmäßig rendert der Browser Daten mit dem ordnungsgemäßen auf Ihrem Gebietsschema basierenden Format.</span><span class="sxs-lookup"><span data-stu-id="39fd6-134">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="39fd6-135">Weitere Informationen finden Sie unter der [ \<input >-Tag Helper Dokumentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="39fd6-135">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="39fd6-136">Führen Sie die app aus, wechseln Sie zu der Studenten Indexseite, und beachten Sie, dass die Zeiten für die Datumsangaben für die Registrierung nicht mehr angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="39fd6-136">Run the app, go to the Students Index page and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="39fd6-137">"True" für jede Ansicht wird übereinstimmen, die der Student-Objektmodell verwendet.</span><span class="sxs-lookup"><span data-stu-id="39fd6-137">The same will be true for any view that uses the Student model.</span></span>

![Studenten Indexseite mit Datumsangaben ohne Zeiten](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="39fd6-139">Das StringLength-Attribut</span><span class="sxs-lookup"><span data-stu-id="39fd6-139">The StringLength attribute</span></span>

<span data-ttu-id="39fd6-140">Sie können auch Regeln für die Überprüfung von Daten und Verwenden von Attributen Überprüfungsfehlermeldungen angeben.</span><span class="sxs-lookup"><span data-stu-id="39fd6-140">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="39fd6-141">Die `StringLength` Attribut definiert die maximale Länge in der Datenbank und bietet clientseitige und serverseitige Validierung für ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="39fd6-141">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="39fd6-142">Sie können auch die minimale Zeichenfolgenlänge in diesem Attribut angeben, aber der minimale Wert hat keine Auswirkungen auf das Datenbankschema.</span><span class="sxs-lookup"><span data-stu-id="39fd6-142">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="39fd6-143">Angenommen, Sie möchten sicherstellen, dass Benutzer nicht mehr als 50 Zeichen für einen Namen eingeben.</span><span class="sxs-lookup"><span data-stu-id="39fd6-143">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="39fd6-144">Um diese Einschränkung hinzuzufügen, fügen Sie `StringLength` -Attribute verwenden, um die `LastName` und `FirstMidName` Eigenschaften, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="39fd6-144">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="39fd6-145">Die `StringLength` Attribut wird nicht verhindern, dass einen Benutzer Leerzeichen für einen Namen eingeben.</span><span class="sxs-lookup"><span data-stu-id="39fd6-145">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="39fd6-146">Sie können die `RegularExpression` Attribut Einschränkungen auf die Eingabe anwenden.</span><span class="sxs-lookup"><span data-stu-id="39fd6-146">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="39fd6-147">Beispielsweise erfordert, dass der folgende Code das erste Zeichen Großbuchstaben bestehen muss und die übrigen Zeichen ein Buchstabe sein:</span><span class="sxs-lookup"><span data-stu-id="39fd6-147">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="39fd6-148">Die `MaxLength` Attribut bietet ähnliche Funktionen der `StringLength` -Attribut aber nicht den clientseitigen Validierung.</span><span class="sxs-lookup"><span data-stu-id="39fd6-148">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="39fd6-149">Das Datenbankmodell hat jetzt so geändert, die eine Änderung im Datenbankschema erfordert.</span><span class="sxs-lookup"><span data-stu-id="39fd6-149">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="39fd6-150">Verwenden Sie Migrationen, um das Schema aktualisieren, ohne Verlust von Daten, die Sie möglicherweise mit der Datenbank hinzugefügt haben, mithilfe der Benutzeroberfläche der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="39fd6-150">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="39fd6-151">Speichern Sie die Änderungen zu, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-151">Save your changes and build the project.</span></span> <span data-ttu-id="39fd6-152">Klicken Sie dann öffnen Sie das Befehlsfenster zu, in den Projektordner, und geben Sie die folgenden Befehle:</span><span class="sxs-lookup"><span data-stu-id="39fd6-152">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="39fd6-153">Die `migrations add` Befehl gibt eine Warnung aus, dass Daten verloren gehen können, da die Änderung die maximale Länge für zwei Spalten kürzer ist.</span><span class="sxs-lookup"><span data-stu-id="39fd6-153">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="39fd6-154">Migrationen erstellt eine Datei namens  *\<Zeitstempel > _MaxLengthOnNames.cs*.</span><span class="sxs-lookup"><span data-stu-id="39fd6-154">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="39fd6-155">Diese Datei enthält Code, in der `Up` -Methode, die die Datenbank entsprechend der aktuellen Datenmodell aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="39fd6-155">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="39fd6-156">Die `database update` -Befehl ausgeführt haben, diesen Code.</span><span class="sxs-lookup"><span data-stu-id="39fd6-156">The `database update` command ran that code.</span></span>

<span data-ttu-id="39fd6-157">Der Zeitstempel, der den Dateinamen Migrationen vorangestellt wird vom Entity Framework verwendet, um die Migrationen zu bestellen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-157">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="39fd6-158">Sie können mehrere livemigrationen erstellen, vor dem Ausführen des Update-Database-Befehls, und klicken Sie dann alle die Migrationen in der Reihenfolge, in der sie erstellt wurden, angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="39fd6-158">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="39fd6-159">Die app auszuführen, wählen Sie die **Studenten** auf **neu erstellen**, und geben Sie entweder mehr als 50 Zeichen ein.</span><span class="sxs-lookup"><span data-stu-id="39fd6-159">Run the app, select the **Students** tab, click **Create New**, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="39fd6-160">Beim Klicken auf **erstellen**, clientseitige Validierung zeigt eine Fehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-160">When you click **Create**, client side validation shows an error message.</span></span>

![Studenten Indexseite mit Zeichenfolge LÄNGENFEHLER](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="39fd6-162">Das Spaltenattribut</span><span class="sxs-lookup"><span data-stu-id="39fd6-162">The Column attribute</span></span>

<span data-ttu-id="39fd6-163">Attribute können auch steuern, wie die Klassen und Eigenschaften in der Datenbank zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="39fd6-163">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="39fd6-164">Angenommen, Sie den Namen verwendet haben `FirstMidName` für den Vornamen-Feld verwendet wird, weil das Feld kann auch ein zweiter Vorname enthalten.</span><span class="sxs-lookup"><span data-stu-id="39fd6-164">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="39fd6-165">Die Datenbankspalte benannt werden, möchten jedoch `FirstName`, da der Benutzer, die Ad-hoc-Abfragen für die Datenbank geschrieben werden auf diesen Namen daran gewöhnt sind.</span><span class="sxs-lookup"><span data-stu-id="39fd6-165">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="39fd6-166">Um diese Zuordnung vornehmen, können Sie die `Column` Attribut.</span><span class="sxs-lookup"><span data-stu-id="39fd6-166">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="39fd6-167">Die `Column` Attribut gibt an, wenn die Datenbank erstellt wurde, ist die Spalte von der `Student` Tabelle, zugeordnet der `FirstMidName` Eigenschaft den Namen `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="39fd6-167">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="39fd6-168">Das heißt, wenn Ihr Code bezieht sich auf `Student.FirstMidName`, die Daten aus stammen oder aktualisiert werden, der `FirstName` Spalte die `Student` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="39fd6-168">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="39fd6-169">Wenn Sie keinen Spaltennamen angeben, werden ihm den gleichen Namen wie den Namen der Eigenschaft zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-169">If you don't specify column names, they are given the same name as the property name.</span></span>

<span data-ttu-id="39fd6-170">In der *Student.cs* hinzufügen. eine `using` -Anweisung für `System.ComponentModel.DataAnnotations.Schema` und Hinzufügen der Spalte Name-Attribut, das `FirstMidName` Eigenschaft, die in den folgenden hervorgehobenen Code dargestellt:</span><span class="sxs-lookup"><span data-stu-id="39fd6-170">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="39fd6-171">Das Hinzufügen der `Column` Attribut ändert die Sicherung des Modells die `SchoolContext`, sodass die Datenbank keine Übereinstimmung wird nicht.</span><span class="sxs-lookup"><span data-stu-id="39fd6-171">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="39fd6-172">Speichern Sie die Änderungen zu, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-172">Save your changes and build the project.</span></span> <span data-ttu-id="39fd6-173">Klicken Sie dann öffnen Sie das Befehlsfenster zu, in den Projektordner, und geben Sie die folgenden Befehle zum Erstellen von einem anderen Migration:</span><span class="sxs-lookup"><span data-stu-id="39fd6-173">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="39fd6-174">In **Objekt-Explorer von SQL Server**, öffnen Sie die Student-Tabellen-Designer durch Doppelklicken auf die **Student** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="39fd6-174">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![Students-Tabelle in SSOX nach der Migration](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="39fd6-176">Bevor Sie die ersten beiden Migrationen angewendet, wurden die Namensspalten von Typ nvarchar(MAX).</span><span class="sxs-lookup"><span data-stu-id="39fd6-176">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="39fd6-177">Sie sind jetzt nvarchar(50) und der Spaltennamen FirstMidName FirstName geändert hat.</span><span class="sxs-lookup"><span data-stu-id="39fd6-177">They are now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="39fd6-178">Wenn Sie versuchen, die kompiliert werden, bevor Sie alle der Entitätsklassen in den folgenden Abschnitten erstellt haben, erhalten Sie möglicherweise Compilerfehler angezeigt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-178">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="39fd6-179">Letzte Änderungen an der Student-Entität</span><span class="sxs-lookup"><span data-stu-id="39fd6-179">Final changes to the Student entity</span></span>

![Student-Entität](complex-data-model/_static/student-entity.png)

<span data-ttu-id="39fd6-181">In *Models/Student.cs*, die von Ihnen hinzugefügte Code zuvor durch den folgenden Code ersetzen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-181">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="39fd6-182">Die Änderungen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="39fd6-182">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="39fd6-183">Das erforderliche Attribut</span><span class="sxs-lookup"><span data-stu-id="39fd6-183">The Required attribute</span></span>

<span data-ttu-id="39fd6-184">Die `Required` Attribut macht die erforderlichen Felder des Name-Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="39fd6-184">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="39fd6-185">Die `Required` Attribut ist nicht erforderlich, für NULL-Typen wie Werttypen ("DateTime", "Int", double, float usw.).</span><span class="sxs-lookup"><span data-stu-id="39fd6-185">The `Required` attribute is not needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="39fd6-186">Typen, die nicht null sein können, werden automatisch als Pflichtfelder behandelt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-186">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="39fd6-187">Könnten Sie entfernen die `Required` Attribut, und Ersetzen Sie ihn durch einen Parameter Mindestlänge für die `StringLength` Attribut:</span><span class="sxs-lookup"><span data-stu-id="39fd6-187">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="39fd6-188">Das Attribut anzeigen</span><span class="sxs-lookup"><span data-stu-id="39fd6-188">The Display attribute</span></span>

<span data-ttu-id="39fd6-189">Die `Display` Attribut gibt an, dass die Beschriftung für die Textfelder "Vorname", "Last Name", "Full Name" und "Registrierungsdatum" anstelle des Namens der Eigenschaft in jeder Instanz werden soll (der kein Leerzeichen, die die Wörter geteilt besitzt).</span><span class="sxs-lookup"><span data-stu-id="39fd6-189">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="39fd6-190">Die berechnete FullName-Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="39fd6-190">The FullName calculated property</span></span>

<span data-ttu-id="39fd6-191">`FullName`ist eine berechnete Eigenschaft, die einen Wert zurückgibt, der durch Verketten von zwei weitere Eigenschaften erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="39fd6-191">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="39fd6-192">Daher verfügt über einen Get-Accessor und keine `FullName` Spalte in der Datenbank generiert werden.</span><span class="sxs-lookup"><span data-stu-id="39fd6-192">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="39fd6-193">Die Instructor-Entität erstellen</span><span class="sxs-lookup"><span data-stu-id="39fd6-193">Create the Instructor Entity</span></span>

![Instructor-Entität](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="39fd6-195">Erstellen Sie *Models/Instructor.cs*, ersetzen den Code durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="39fd6-195">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="39fd6-196">Beachten Sie, dass mehrere Eigenschaften in den Entitäten Student "und" Dozenten identisch sind.</span><span class="sxs-lookup"><span data-stu-id="39fd6-196">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="39fd6-197">In der [Implementieren von Vererbung](inheritance.md) weiter unten in diesem Lernprogramm müssen Sie diesen Code aus, um die Redundanz auszuschließen Umgestalten.</span><span class="sxs-lookup"><span data-stu-id="39fd6-197">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="39fd6-198">Sie können mehrere Attribute in einer Zeile einfügen, deshalb können Sie auch schreiben konnte die `HireDate` Attribute wie folgt:</span><span class="sxs-lookup"><span data-stu-id="39fd6-198">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="39fd6-199">Die Navigationseigenschaften CourseAssignments und OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="39fd6-199">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="39fd6-200">Die `CourseAssignments` und `OfficeAssignment` Eigenschaften sind Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="39fd6-200">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="39fd6-201">Ein Kursleiter kann eine beliebige Anzahl von Kurse werden folgende Themen behandelt, sodass `CourseAssignments` als eine Auflistung definiert ist.</span><span class="sxs-lookup"><span data-stu-id="39fd6-201">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="39fd6-202">Wenn eine Navigationseigenschaft über mehrere Entitäten enthalten kann, muss der Typ in der Einträge hinzugefügt, gelöscht und aktualisiert werden können, eine Liste.</span><span class="sxs-lookup"><span data-stu-id="39fd6-202">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="39fd6-203">Sie können angeben, `ICollection<T>` oder ein Typ sein wie z. B. `List<T>` oder `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="39fd6-203">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="39fd6-204">Bei Angabe von `ICollection<T>`, EF erstellt eine `HashSet<T>` Auflistung standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="39fd6-204">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="39fd6-205">Der Grund, warum dies sind `CourseAssignment` Entitäten wird unten im Abschnitt über viele-zu-viele-Beziehungen erläutert.</span><span class="sxs-lookup"><span data-stu-id="39fd6-205">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="39fd6-206">Contoso University Geschäftsregeln der Status, der ein Kursleiter nur höchstens eine Niederlassung haben, kann daher die `OfficeAssignment` Eigenschaft enthält eine einzelne OfficeAssignment-Entität (die möglicherweise null, wenn keine Office zugewiesen ist).</span><span class="sxs-lookup"><span data-stu-id="39fd6-206">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="39fd6-207">Erstellen Sie die Entität OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="39fd6-207">Create the OfficeAssignment entity</span></span>

![OfficeAssignment-Entität](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="39fd6-209">Erstellen Sie *Models/OfficeAssignment.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="39fd6-209">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="39fd6-210">Das Key-Attribut</span><span class="sxs-lookup"><span data-stu-id="39fd6-210">The Key attribute</span></span>

<span data-ttu-id="39fd6-211">Es ist eine auf 0 (null) oder eine 1-Beziehung zwischen den Dozenten und den OfficeAssignment Entitäten.</span><span class="sxs-lookup"><span data-stu-id="39fd6-211">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="39fd6-212">Eine bürozuweisung existiert nur in Bezug auf den Dozenten, dem sie zugewiesen ist, und primäre Schlüssel ist daher auch die Fremdschlüssel für die Instructor-Entität.</span><span class="sxs-lookup"><span data-stu-id="39fd6-212">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="39fd6-213">Aber das Entity Framework nicht InstructorID automatisch als primären Schlüssel für diese Entität erkannt werden, da der Name der ID oder ClassnameID-Benennungskonvention folgen, nicht.</span><span class="sxs-lookup"><span data-stu-id="39fd6-213">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="39fd6-214">Aus diesem Grund die `Key` Attribut wird verwendet, um sie als Schlüssel zu identifizieren:</span><span class="sxs-lookup"><span data-stu-id="39fd6-214">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="39fd6-215">Sie können auch die `Key` Attribut, wenn die Entität verfügt über einen eigenen Primärschlüssel, aber die Eigenschaft als ClassnameID oder ID etwas benennen möchten</span><span class="sxs-lookup"><span data-stu-id="39fd6-215">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="39fd6-216">Standardmäßig behandelt EF des Schlüssels als nicht-datenbankgeneriert, da die Spalte für eine identifizierende Beziehung ist.</span><span class="sxs-lookup"><span data-stu-id="39fd6-216">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="39fd6-217">Die Navigationseigenschaft für Dozenten</span><span class="sxs-lookup"><span data-stu-id="39fd6-217">The Instructor navigation property</span></span>

<span data-ttu-id="39fd6-218">Die Instructor-Entität verfügt über einen `OfficeAssignment` Navigationseigenschaft (da eine bürozuweisung möglicherweise nicht über ein Kursleiter verfügen), und die Entität "OfficeAssignment" hat eine NULL- `Instructor` Navigationseigenschaft (da eine bürozuweisung kann nicht ohne einen Kursleiter--vorhanden `InstructorID` ist NULL-Werte zulässt).</span><span class="sxs-lookup"><span data-stu-id="39fd6-218">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="39fd6-219">Wenn eine Instructor-Entität eine verknüpfte OfficeAssignment Entität verfügen, wird jede Entität einen Verweis auf die andere in die Navigationseigenschaft verfügen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-219">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="39fd6-220">Sie setzen konnte eine `[Required]` Attribut für die Instructor-Navigationseigenschaft, um anzugeben, dass eine verwandte Dozenten muss, aber Sie nicht verfügen, da dies der `InstructorID` Fremdschlüssel (also auch die Taste, um diese Tabelle) ist NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-220">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="39fd6-221">Ändern Sie die Kurs-Entität</span><span class="sxs-lookup"><span data-stu-id="39fd6-221">Modify the Course Entity</span></span>

![Kurs-Entität](complex-data-model/_static/course-entity.png)

<span data-ttu-id="39fd6-223">In *Models/Course.cs*, die von Ihnen hinzugefügte Code zuvor durch den folgenden Code ersetzen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-223">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="39fd6-224">Die Änderungen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="39fd6-224">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="39fd6-225">Die Kurs-Entität verfügt über eine Fremdschlüsseleigenschaft `DepartmentID` hat das auf die verknüpfte Entität Department und zeigt eine `Department` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="39fd6-225">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="39fd6-226">Das Entity Framework erfordern nicht, dass Sie eine foreign Key-Eigenschaft mit dem Datenmodell hinzufügen, wenn eine Navigationseigenschaft für einer verknüpften Entität sind.</span><span class="sxs-lookup"><span data-stu-id="39fd6-226">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="39fd6-227">EF Fremdschlüssel in der Datenbank erstellt wird, werden im Bedarfsfall automatisch und erstellt [Shadowing von Eigenschaften](https://docs.microsoft.com/ef/core/modeling/shadow-properties) für sie.</span><span class="sxs-lookup"><span data-stu-id="39fd6-227">EF automatically creates foreign keys in the database wherever they are needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="39fd6-228">Jedoch müssen die Fremdschlüssel im Datenmodell kann Updates vornehmen, einfacher und effizienter.</span><span class="sxs-lookup"><span data-stu-id="39fd6-228">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="39fd6-229">Z. B. Wenn Sie eine Entität Kurs bearbeiten abzurufen, die Entität Department ist null, wenn Sie es nicht geladen werden, also bei der Aktualisierung von der Entität Kurs Sie müssten zuerst die Entität Department abzurufen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-229">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="39fd6-230">Wenn die Fremdschlüsseleigenschaft `DepartmentID` enthalten ist in das Datenmodell, müssen Sie die Entität Department zu abzurufen, bevor Sie aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="39fd6-230">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="39fd6-231">Das DatabaseGenerated-Attribut</span><span class="sxs-lookup"><span data-stu-id="39fd6-231">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="39fd6-232">Die `DatabaseGenerated` -Attribut mit dem `None` Parameter auf die `CourseID` Eigenschaft gibt an, dass Primärschlüsselwerte sind vom Benutzer bereitgestellte anstatt von der Datenbank generiert.</span><span class="sxs-lookup"><span data-stu-id="39fd6-232">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="39fd6-233">Entity Framework geht standardmäßig davon aus, dass Primärschlüssel von der Datenbank generiert werden.</span><span class="sxs-lookup"><span data-stu-id="39fd6-233">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="39fd6-234">Das ist in den meisten Fällen erwünscht.</span><span class="sxs-lookup"><span data-stu-id="39fd6-234">That's what you want in most scenarios.</span></span> <span data-ttu-id="39fd6-235">Allerdings für die Kurs-Entitäten, Sie verwenden eine benutzerdefinierte Kurs Zahl wie z. B. eine Reihe von 1000 für eine Abteilung, eine Reihe 2000 für eine andere Abteilung und usw.</span><span class="sxs-lookup"><span data-stu-id="39fd6-235">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="39fd6-236">Die `DatabaseGenerated` Attribut kann auch verwendet werden, um Default-Werte zu generieren, wie im Fall von Datenbankspalten verwendet, um das Datum aufzuzeichnen eine Zeile erstellt oder aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="39fd6-236">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="39fd6-237">Weitere Informationen finden Sie unter [Eigenschaften generiert](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="39fd6-237">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="39fd6-238">Foreign Key- und Navigation-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="39fd6-238">Foreign key and navigation properties</span></span>

<span data-ttu-id="39fd6-239">Die Fremdschlüsseleigenschaften und Navigationseigenschaften der Entität Kurs reflektieren die folgenden Beziehungen:</span><span class="sxs-lookup"><span data-stu-id="39fd6-239">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="39fd6-240">Ein Kurs wird an eine Abteilung zugewiesen, daher besteht eine `DepartmentID` Fremdschlüssel und einem `Department` Navigationseigenschaft für die oben genannten Gründe.</span><span class="sxs-lookup"><span data-stu-id="39fd6-240">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="39fd6-241">Ein Kurs kann eine beliebige Anzahl von Lernende, haben also die `Enrollments` Navigationseigenschaft ist eine Sammlung:</span><span class="sxs-lookup"><span data-stu-id="39fd6-241">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="39fd6-242">Ein Kurs kann von mehreren Lehrkräfte vermittelten werden daher die `CourseAssignments` Navigationseigenschaft ist eine Auflistung (den Typ `CourseAssignment` wird erläutert [später](#many-to-many-relationships)):</span><span class="sxs-lookup"><span data-stu-id="39fd6-242">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="39fd6-243">Erstellen Sie die Entität Department</span><span class="sxs-lookup"><span data-stu-id="39fd6-243">Create the Department entity</span></span>

![Entität Department](complex-data-model/_static/department-entity.png)


<span data-ttu-id="39fd6-245">Erstellen Sie *Models/Department.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="39fd6-245">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="39fd6-246">Das Spaltenattribut</span><span class="sxs-lookup"><span data-stu-id="39fd6-246">The Column attribute</span></span>

<span data-ttu-id="39fd6-247">Zuvor Sie verwendet die `Column` Attribut, um die Zuordnung für die Namen von Spalten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="39fd6-247">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="39fd6-248">Im Code für die Entität Department der `Column` Attribut wird verwendet, um das Ändern der SQL datentypzuordnung so, dass die Spalte mit den SQL Server-Money-Datentyp in der Datenbank definiert werden:</span><span class="sxs-lookup"><span data-stu-id="39fd6-248">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="39fd6-249">Die Zuordnung ist im Allgemeinen nicht erforderlich, da das Entity Framework wählt die entsprechenden SQL Server-Datentyp, der basierend auf dem CLR-Typ, den Sie für die Eigenschaft zu definieren.</span><span class="sxs-lookup"><span data-stu-id="39fd6-249">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="39fd6-250">Die CLR `decimal` wird zugeordnet zu SQL Server `decimal` Typ.</span><span class="sxs-lookup"><span data-stu-id="39fd6-250">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="39fd6-251">In diesem Fall wissen, dass die Spalte wird Währungsbeträge belegen, und der Money-Datentyp eignet sich besser für die aber.</span><span class="sxs-lookup"><span data-stu-id="39fd6-251">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="39fd6-252">Foreign Key- und Navigation-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="39fd6-252">Foreign key and navigation properties</span></span>

<span data-ttu-id="39fd6-253">Die folgenden Beziehungen entsprechen den foreign key- und Navigation-Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="39fd6-253">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="39fd6-254">Eine Abteilung kann oder besitzen möglicherweise nicht von einem Administrator und ein Administrator ist immer einen Kursleiter.</span><span class="sxs-lookup"><span data-stu-id="39fd6-254">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="39fd6-255">Aus diesem Grund die `InstructorID` Eigenschaft ist als der Fremdschlüssel für die Instructor-Entität enthalten, und ein Fragezeichen wird hinzugefügt, nachdem die `int` Geben Sie die Bezeichnung, markieren Sie die Eigenschaft als NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-255">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="39fd6-256">Die Navigationseigenschaft heißt `Administrator` jedoch eine Instructor-Entität enthält:</span><span class="sxs-lookup"><span data-stu-id="39fd6-256">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="39fd6-257">Eine Abteilung möglicherweise viele Kurse, daher eine Navigationseigenschaft Kurse besteht:</span><span class="sxs-lookup"><span data-stu-id="39fd6-257">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="39fd6-258">Gemäß der Konvention ermöglicht das Entity Framework kaskadierendes Delete für die NULL-Fremdschlüssel und für viele-zu-viele-Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-258">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="39fd6-259">Dies kann zu zirkuläre Cascade Delete Regeln führen, der eine Ausnahme verursacht wird, wenn Sie versuchen, eine Migration hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-259">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="39fd6-260">Z. B. Wenn Sie die Eigenschaft Department.InstructorID als auf NULL festlegbar definiert haben, würden EF eine Cascade Löschregel konfigurieren den Dozenten löschen, beim Löschen der Abteilung, also nicht passieren sollen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-260">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which is not what you want to have happen.</span></span> <span data-ttu-id="39fd6-261">Bei Bedarf Ihre Geschäftsregeln der `InstructorID` Eigenschaft nicht NULL-Werte zulässt, müssten Sie die folgende fluent-API-Anweisung verwenden, um kaskadierte Löschung auf der Beziehung zu deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="39fd6-261">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="39fd6-262">Ändern Sie die Registrierung-Entität</span><span class="sxs-lookup"><span data-stu-id="39fd6-262">Modify the Enrollment entity</span></span>

![Registrierung-Entität](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="39fd6-264">In *Models/Enrollment.cs*, die von Ihnen hinzugefügte Code zuvor durch den folgenden Code ersetzen:</span><span class="sxs-lookup"><span data-stu-id="39fd6-264">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="39fd6-265">Foreign Key- und Navigation-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="39fd6-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="39fd6-266">Die foreign Key-Eigenschaften und Navigationseigenschaften reflektieren die folgenden Beziehungen:</span><span class="sxs-lookup"><span data-stu-id="39fd6-266">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="39fd6-267">Ein Anmeldungsdatensatz ist für eine einzelnes Kurs, es ist also eine `CourseID` foreign Key-Eigenschaft und eine `Course` Navigationseigenschaft:</span><span class="sxs-lookup"><span data-stu-id="39fd6-267">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="39fd6-268">Ein Anmeldungsdatensatz ist für einen einzelnen Studenten, es ist also eine `StudentID` foreign Key-Eigenschaft und eine `Student` Navigationseigenschaft:</span><span class="sxs-lookup"><span data-stu-id="39fd6-268">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="39fd6-269">n:n-Beziehungen</span><span class="sxs-lookup"><span data-stu-id="39fd6-269">Many-to-Many Relationships</span></span>

<span data-ttu-id="39fd6-270">Besteht eine m: n-Beziehung zwischen den Entitäten Student "und" Kurs und die Registrierung Entität fungiert als Jointabelle m: n- *mit Nutzlast* in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="39fd6-270">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="39fd6-271">"Mit der Nutzlast" bedeutet, dass die Enrollment-Tabelle zusätzliche Daten als Fremdschlüssel für den verknüpften Tabellen (in diesem Fall eine primary key- und eine Eigenschaft Grade) enthält.</span><span class="sxs-lookup"><span data-stu-id="39fd6-271">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="39fd6-272">Die folgende Abbildung zeigt, wie diese Beziehungen in einem Entitätsdiagramm aussieht.</span><span class="sxs-lookup"><span data-stu-id="39fd6-272">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="39fd6-273">(Dieses Diagramm generiert wurde, verwenden die Entity Framework-Powertools für EF 6.x, erstellen das Diagramm ist nicht Teil des Lernprogramms wird gerade verwendet wird hier als veranschaulicht.)</span><span class="sxs-lookup"><span data-stu-id="39fd6-273">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![Student-Kurs m: n-Beziehung](complex-data-model/_static/student-course.png)

<span data-ttu-id="39fd6-275">Jede Beziehungslinie hat eine 1 an einem Ende und ein Sternchen (\*) am anderen Eintrag, der angibt, einer 1: n-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="39fd6-275">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="39fd6-276">Wenn die Enrollment-Tabelle Grade Informationen nicht enthalten waren, müssten sie nur die zwei Fremdschlüssel CourseID und StudentID enthalten.</span><span class="sxs-lookup"><span data-stu-id="39fd6-276">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="39fd6-277">In diesem Fall wäre es eine m: n-Jointabelle ohne Nutzlast (oder eine reine Jointabelle) in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="39fd6-277">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="39fd6-278">Die Instructor und Kurs Entitäten sein, diese Art von m: n-Beziehung, und der nächste Schritt ist die Erstellung eine Entitätsklasse zur als eine Jointabelle ohne Nutzlast fungieren.</span><span class="sxs-lookup"><span data-stu-id="39fd6-278">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="39fd6-279">(EF 6.x unterstützt, die implizite Verknüpfung von Tabellen für m: n-Beziehungen, aber EF Core nicht.</span><span class="sxs-lookup"><span data-stu-id="39fd6-279">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core does not.</span></span> <span data-ttu-id="39fd6-280">Weitere Informationen finden Sie unter der [Diskussion in der EF-Core-GitHub-Repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span><span class="sxs-lookup"><span data-stu-id="39fd6-280">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span> 

## <a name="the-courseassignment-entity"></a><span data-ttu-id="39fd6-281">Die CourseAssignment-Entität</span><span class="sxs-lookup"><span data-stu-id="39fd6-281">The CourseAssignment entity</span></span>

![CourseAssignment-Entität](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="39fd6-283">Erstellen Sie *Models/CourseAssignment.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="39fd6-283">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a><span data-ttu-id="39fd6-284">Entitätsnamen verknüpfen</span><span class="sxs-lookup"><span data-stu-id="39fd6-284">Join entity names</span></span>

<span data-ttu-id="39fd6-285">Eine Jointabelle in der Datenbank für die Instructor-Kurse-viele-zu-viele-Beziehung erforderlich ist, und er muss über eine Entitätenmenge dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="39fd6-285">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="39fd6-286">Es ist üblich, benennen Sie eine Joinentität `EntityName1EntityName2`, die in diesem Fall wäre `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="39fd6-286">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="39fd6-287">Allerdings wird empfohlen, dass Sie einen Namen auswählen, der die Beziehung beschreibt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-287">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="39fd6-288">Datenmodelle fangen einfache und hinausgeht, mit ohne-Nutzlast Joins häufig Nutzlasten später abrufen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-288">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="39fd6-289">Wenn Sie mit einem aussagekräftigen Entitätsname beginnen, müssen Sie den Namen später ändern.</span><span class="sxs-lookup"><span data-stu-id="39fd6-289">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="39fd6-290">Im Idealfall würden die Joinentität einen eigene natürlichen (möglicherweise einzelne Wort)-Namen in der Business-Domäne haben.</span><span class="sxs-lookup"><span data-stu-id="39fd6-290">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="39fd6-291">Beispielsweise konnte Büchern und Kunden über Bewertungen verknüpft werden.</span><span class="sxs-lookup"><span data-stu-id="39fd6-291">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="39fd6-292">Für diese Beziehung `CourseAssignment` ist besser geeignet als `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="39fd6-292">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="39fd6-293">Zusammengesetzter Schlüssel</span><span class="sxs-lookup"><span data-stu-id="39fd6-293">Composite key</span></span>

<span data-ttu-id="39fd6-294">Da der Fremdschlüssel keine NULL-Werte zulässt und zusammen eindeutig sind jede Zeile der Tabelle zu identifizieren, besteht keine Notwendigkeit für eine separate Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="39fd6-294">Since the foreign keys are not nullable and together uniquely identify each row of the table, there is no need for a separate primary key.</span></span> <span data-ttu-id="39fd6-295">Die *InstructorID* und *CourseID* Eigenschaften als einen zusammengesetzten Primärschlüssel fungieren soll.</span><span class="sxs-lookup"><span data-stu-id="39fd6-295">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="39fd6-296">Die einzige Möglichkeit zum Identifizieren von zusammengesetzten Primärschlüssel zur EF ist die Verwendung der *fluent-API* (kann nicht mithilfe von Attributen ausgeführt werden).</span><span class="sxs-lookup"><span data-stu-id="39fd6-296">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="39fd6-297">Gewusst wie: Konfigurieren von zusammengesetzten Primärschlüssel im nächsten Abschnitt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-297">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="39fd6-298">Des zusammengesetzten Schlüssels wird sichergestellt, dass während Sie über mehrere Zeilen für einen Kurs und mehrere Zeilen für einen Kursleiter verfügen können, Sie mehrere Zeilen für die gleiche Kursleiter und Kurs nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="39fd6-298">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="39fd6-299">Die `Enrollment` Joinentität definiert einen eigenen Primärschlüssel aus, damit Duplikate dieser Art möglich sind.</span><span class="sxs-lookup"><span data-stu-id="39fd6-299">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="39fd6-300">Um zu verhindern, dass solche Duplikate, konnte Sie einen eindeutigen Index für die foreign Key-Felder hinzufügen oder konfigurieren `Enrollment` mit einem zusammengesetzten Primärschlüssel ähnelt `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="39fd6-300">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="39fd6-301">Weitere Informationen finden Sie unter [Indizes](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="39fd6-301">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="39fd6-302">Aktualisieren Sie den Datenbankkontext</span><span class="sxs-lookup"><span data-stu-id="39fd6-302">Update the database context</span></span>

<span data-ttu-id="39fd6-303">Fügen Sie den folgenden hervorgehobenen Code in die *Data/SchoolContext.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="39fd6-303">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="39fd6-304">Dieser Code fügt das neuen Entitäten und konfiguriert die CourseAssignment Entität zusammengesetzten Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="39fd6-304">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="39fd6-305">Fluent-API-Alternative zu Attributen</span><span class="sxs-lookup"><span data-stu-id="39fd6-305">Fluent API alternative to attributes</span></span>

<span data-ttu-id="39fd6-306">Der Code in der `OnModelCreating` Methode der `DbContext` -Klasse verwendet die *fluent-API* EF Verhalten konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="39fd6-306">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="39fd6-307">Die API wird "fluent" bezeichnet, da es häufig von einer Reihe von Methodenaufrufen, die zusammen in einer einzigen Anweisung, wie in diesem Beispiel aus Vermittlungsstellen verwendet die [EF Basisdokumentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="39fd6-307">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="39fd6-308">In diesem Lernprogramm verwenden Sie die fluent-API nur für datenbankzuordnung, die Sie mit Attributen können nicht.</span><span class="sxs-lookup"><span data-stu-id="39fd6-308">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="39fd6-309">Allerdings auch können die fluent-API Sie die meisten Formatierung, Überprüfung und Zuordnungsregeln an, denen Sie durchführen können, mithilfe von Attributen angeben.</span><span class="sxs-lookup"><span data-stu-id="39fd6-309">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="39fd6-310">Einige Attribute wie z. B. `MinimumLength` kann nicht mit der fluent-API angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="39fd6-310">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="39fd6-311">Wie bereits erwähnt, `MinimumLength` ändert nicht das Schema gilt nur für eine Validierungsregel Client und Server-Seite.</span><span class="sxs-lookup"><span data-stu-id="39fd6-311">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="39fd6-312">Einige Entwickler bevorzugen die fluent-API ausschließlich, damit sie ihre Entitätsklassen "bereinigen" bleibt</span><span class="sxs-lookup"><span data-stu-id="39fd6-312">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="39fd6-313">Sie können Attribute und fluent-API kombinieren, wenn werden sollen, und es gibt einige Anpassungen, die nur mithilfe der fluent-API ausgeführt werden können, aber im Allgemeinen wird empfohlen, wählen eine dieser beiden Ansätze und konsistent, so weit wie möglich verwenden.</span><span class="sxs-lookup"><span data-stu-id="39fd6-313">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="39fd6-314">Wenn Sie beides verwenden, beachten Sie, dass immer ein Konflikt vorliegt, Fluent-API für Attribute überschreibt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-314">If you do use both, note that wherever there is a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="39fd6-315">Weitere Informationen zu Attributen im Vergleich zu fluent-API finden Sie unter [Methoden der Konfiguration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="39fd6-315">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="39fd6-316">Anzeigen von Entitätsbeziehungen-Diagramm</span><span class="sxs-lookup"><span data-stu-id="39fd6-316">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="39fd6-317">Die folgende Abbildung zeigt das Diagramm, das den Entity Framework-Power-Tools für das abgeschlossene Modell "School" zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-317">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![Entitätsdiagramm](complex-data-model/_static/diagram.png)

<span data-ttu-id="39fd6-319">Neben den Zeilen 1: n-Beziehung (1 bis \*), sehen Sie hier zu 0 (null) oder eins eine Beziehungslinie (1 zu 0. 1) zwischen den Entitäten Dozenten und OfficeAssignment und 0 (null)-oder-1-n-Beziehung (0. 1 auf *) zwischen den Instructor "und" Department-Entitäten.</span><span class="sxs-lookup"><span data-stu-id="39fd6-319">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="39fd6-320">Ausgangswert für die Datenbank mit Testdaten</span><span class="sxs-lookup"><span data-stu-id="39fd6-320">Seed the Database with Test Data</span></span>

<span data-ttu-id="39fd6-321">Ersetzen Sie den Code in der *Data/DbInitializer.cs* Datei durch den folgenden Code um Seed-Daten für die neuen Entitäten zu gewährleisten Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="39fd6-321">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="39fd6-322">Im ersten Lernprogramm haben Sie gesehen, wird die meisten dieser Code einfach neue Entitätsobjekte erstellt und lädt Beispieldaten in die Eigenschaften nach Bedarf zu Testzwecken.</span><span class="sxs-lookup"><span data-stu-id="39fd6-322">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="39fd6-323">Beachten Sie, wie die m: n-Beziehungen behandelt werden: der Code erstellt Beziehungen durch das Erstellen von Entitäten in der `Enrollments` und `CourseAssignment` Entitätenmengen verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-323">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="39fd6-324">Fügen Sie eine migration</span><span class="sxs-lookup"><span data-stu-id="39fd6-324">Add a migration</span></span>

<span data-ttu-id="39fd6-325">Speichern Sie die Änderungen zu, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-325">Save your changes and build the project.</span></span> <span data-ttu-id="39fd6-326">Klicken Sie dann das Befehlsfenster zu öffnen, in den Projektordner, und geben Sie die `migrations add` Befehl (nicht ausführen den Update-Database-Befehl noch):</span><span class="sxs-lookup"><span data-stu-id="39fd6-326">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="39fd6-327">Sie erhalten eine Warnung bezüglich eines möglichen Datenverlusts.</span><span class="sxs-lookup"><span data-stu-id="39fd6-327">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="39fd6-328">Falls Sie versucht haben, führen Sie die `database update` an diesem Punkt Befehl (nicht gerade noch), erhalten Sie die folgende Fehlermeldung:</span><span class="sxs-lookup"><span data-stu-id="39fd6-328">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="39fd6-329">Die ALTER TABLE-Anweisung steht in Konflikt mit der FOREIGN KEY-Einschränkung "FK_dbo. Course_dbo. Department_DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="39fd6-329">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="39fd6-330">Der Konflikt trat in der Datenbank "ContosoUniversity", Tabelle "Dbo. Abteilung", Spalte"DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="39fd6-330">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="39fd6-331">In einigen Fällen, wenn Sie Migrationen mit vorhandenen Daten ausführen, müssen Sie zum Einfügen von Stubdaten in die Datenbank foreign Key-Einschränkungen nicht erfüllen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-331">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="39fd6-332">Der generierte Code in der `Up` -Methode die Course-Tabelle NULL-Fremdschlüssel "DepartmentID" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-332">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="39fd6-333">Wenn Zeilen vorhanden bereits in die Course-Tabelle beim Ausführen des Codes sind die `AddColumn` Vorgang schlägt fehl, da SQL Server nicht weiß, welcher Wert in die Spalte einfügen, die darf nicht null sein.</span><span class="sxs-lookup"><span data-stu-id="39fd6-333">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="39fd6-334">In diesem Lernprogramm führen Sie die Migration auf eine neue Datenbank, jedoch in einer produktionsanwendung müssten Sie stellen die Migration vorhandene Daten verarbeiten, daher die folgenden Anweisungen ein Beispiel hierzu anzeigen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-334">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="39fd6-335">Stellen diese Migration arbeiten mit vorhandenen Daten, die Sie so ändern Sie den Code so erteilen Sie der neuen Spalte einen Standardwert verfügen, und erstellen Sie eine Abteilung ein Stub mit dem Namen "Temp" als Standard-Abteilung zu agieren.</span><span class="sxs-lookup"><span data-stu-id="39fd6-335">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="39fd6-336">Folglich vorhandenen Kurs Zeilen werden alle werden im Zusammenhang mit der Abteilung "Temp" nach der `Up` Methode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="39fd6-336">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="39fd6-337">Öffnen der *{timestamp}_ComplexDataModel.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="39fd6-337">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span> 

* <span data-ttu-id="39fd6-338">Kommentieren Sie die Codezeile, die die Course-Tabelle die Spalte "DepartmentID" hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-338">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* <span data-ttu-id="39fd6-339">Fügen Sie nach dem Code, der die Department-Tabelle erstellt die folgenden hervorgehobenen Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="39fd6-339">Add the following highlighted code after the code that creates the Department table:</span></span>

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="39fd6-340">In einer produktionsanwendung würde Sie schreiben Code oder Skripts für die Abteilung Zeilen hinzuzufügen, und die neuen Zeilen der Abteilung Kurs Zeilen beziehen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-340">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="39fd6-341">Die Abteilung "Temp" oder den Standardwert für die Spalte Course.DepartmentID würden Sie dann nicht mehr benötigen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-341">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="39fd6-342">Speichern Sie die Änderungen zu, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-342">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="39fd6-343">Ändern Sie die Verbindungszeichenfolge und aktualisieren die Datenbank</span><span class="sxs-lookup"><span data-stu-id="39fd6-343">Change the connection string and update the database</span></span>

<span data-ttu-id="39fd6-344">Sie haben jetzt die neuen Code, der `DbInitializer` -Klasse, die mit einer leeren Datenbank Ausgangswert Daten für die neue Entitäten hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-344">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="39fd6-345">Um EF erstellen Sie eine neue leere Datenbank zu machen, ändern Sie den Namen der Datenbank in der Verbindungszeichenfolge in *appsettings.json* ContosoUniversity3 oder einen anderen Namen, die Sie verwenden, die Sie nicht auf dem Computer verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="39fd6-345">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="39fd6-346">Speichern Sie die Änderung zu *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="39fd6-346">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="39fd6-347">Als Alternative zum Ändern des Datenbanknamens können Sie die Datenbank löschen.</span><span class="sxs-lookup"><span data-stu-id="39fd6-347">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="39fd6-348">Verwendung **Objekt-Explorer von SQL Server** (SSOX) oder die `database drop` CLI-Befehl:</span><span class="sxs-lookup"><span data-stu-id="39fd6-348">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="39fd6-349">Nachdem Sie den Datenbanknamen geändert oder die Datenbank gelöscht haben, führen Sie die `database update` im Befehlsfenster die Migrationen auszuführende Befehl.</span><span class="sxs-lookup"><span data-stu-id="39fd6-349">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="39fd6-350">Ausführen die app, die dazu führen, dass die `DbInitializer.Initialize` Methode zum Ausführen und die neue Datenbank aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="39fd6-350">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="39fd6-351">Öffnen Sie die Datenbank im SSOX, wie zuvor, und erweitern Sie die **Tabellen** Knoten, um anzuzeigen, dass alle Tabellen erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="39fd6-351">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="39fd6-352">(Wenn Sie aus der früheren Zeitpunkt öffnen SSOX weiterhin bestehen, klicken Sie auf die **aktualisieren** Schaltfläche.)</span><span class="sxs-lookup"><span data-stu-id="39fd6-352">(If you still have SSOX open from the earlier time, click the **Refresh** button.)</span></span>

![Tabellen in SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="39fd6-354">Führen Sie die app, um den Initialisierer Code auslösen, der die Datenbank startet.</span><span class="sxs-lookup"><span data-stu-id="39fd6-354">Run the app to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="39fd6-355">Mit der rechten Maustaste die **CourseAssignment** Tabelle, und wählen Sie **Ansichtsdaten** um sicherzustellen, dass es Daten enthält.</span><span class="sxs-lookup"><span data-stu-id="39fd6-355">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![CourseAssignment Daten in SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="39fd6-357">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="39fd6-357">Summary</span></span>

<span data-ttu-id="39fd6-358">Sie haben jetzt eine komplexere Datenmodell und die entsprechende Datenbank.</span><span class="sxs-lookup"><span data-stu-id="39fd6-358">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="39fd6-359">Im folgenden Lernprogramm erfahren Sie mehr über Zugriff auf zugehörige Daten.</span><span class="sxs-lookup"><span data-stu-id="39fd6-359">In the following tutorial, you'll learn more about how to access related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="39fd6-360">[Zurück](migrations.md)
[Weiter](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="39fd6-360">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>  
