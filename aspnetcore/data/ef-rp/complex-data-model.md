---
title: Razor-Seiten mit EF-Kern - Datenmodell - 5 8
author: rick-anderson
description: "In diesem Lernprogramm fügen Sie weitere Entitäten und Beziehungen und das Datenmodell anpassen, indem Sie Formatierung, Überprüfung und Zuordnungsregeln für die Datenbank angeben."
keywords: ASP.NET Core, Entity Framework Core-datenanmerkungen
ms.author: riande
manager: wpickett
ms.date: 10/25/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: c2761f79fa4836d29541526782969bb0fd47778b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a><span data-ttu-id="89e7e-104">Erstellen ein Modell mit komplexen Daten - EF-Core mit Razor-Seiten Lernprogramm (5 von 8)</span><span class="sxs-lookup"><span data-stu-id="89e7e-104">Creating a complex data model - EF Core with Razor Pages tutorial (5 of 8)</span></span>

<span data-ttu-id="89e7e-105">Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="89e7e-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="89e7e-106">In den vorherigen Lernprogrammen arbeitet mit einer einfachen Datenmodell, das von drei Entitäten verfasst wurde.</span><span class="sxs-lookup"><span data-stu-id="89e7e-106">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="89e7e-107">In diesem Lernprogramm:</span><span class="sxs-lookup"><span data-stu-id="89e7e-107">In this tutorial:</span></span>

* <span data-ttu-id="89e7e-108">Weitere Entitäten und Beziehungen werden hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-108">More entities and relationships are added.</span></span>
* <span data-ttu-id="89e7e-109">Das Datenmodell wird durch Angeben von Formatierung, Überprüfung und Zuordnungsregeln für die Datenbank angepasst.</span><span class="sxs-lookup"><span data-stu-id="89e7e-109">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="89e7e-110">Die Entitätsklassen für das vollständige Datenmodell ist in der folgenden Abbildung dargestellt:</span><span class="sxs-lookup"><span data-stu-id="89e7e-110">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Entitätsdiagramm](complex-data-model/_static/diagram.png)

<span data-ttu-id="89e7e-112">Wenn Probleme können nicht zu lösen auftreten, laden Sie die [abgeschlossene Anwendung für diese Stufe](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span><span class="sxs-lookup"><span data-stu-id="89e7e-112">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="89e7e-113">Das Datenmodell mit Attributen anpassen</span><span class="sxs-lookup"><span data-stu-id="89e7e-113">Customize the data model with attributes</span></span>

<span data-ttu-id="89e7e-114">In diesem Abschnitt wird das Datenmodell mithilfe von Attributen angepasst.</span><span class="sxs-lookup"><span data-stu-id="89e7e-114">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="89e7e-115">Das DataType-Attribut</span><span class="sxs-lookup"><span data-stu-id="89e7e-115">The DataType attribute</span></span>

<span data-ttu-id="89e7e-116">Die Student-Seiten derzeit zeigt an, der das Registrierungsdatum.</span><span class="sxs-lookup"><span data-stu-id="89e7e-116">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="89e7e-117">Date-Felder anzeigen in der Regel nur das Datum und nicht die Zeit.</span><span class="sxs-lookup"><span data-stu-id="89e7e-117">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="89e7e-118">Update *Models/Student.cs* mit den folgenden hervorgehobenen Code:</span><span class="sxs-lookup"><span data-stu-id="89e7e-118">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="89e7e-119">Die [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) Attribut gibt an, einen Datentyp, der als Typ der systeminternen genauer bestimmt ist.</span><span class="sxs-lookup"><span data-stu-id="89e7e-119">The [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="89e7e-120">In diesem Fall nur das Datum angezeigt werden soll, nicht das Datum und die Uhrzeit.</span><span class="sxs-lookup"><span data-stu-id="89e7e-120">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="89e7e-121">Die [DataType-Enumeration](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) stellt für viele Datentypen, z. B. Datum, Uhrzeit, PhoneNumber, Währung, EmailAddress usw. bereit. Die `DataType` Attribut kann auch aktivieren, die app-spezifische Funktionen automatisch bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-121">The [DataType Enumeration](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="89e7e-122">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="89e7e-122">For example:</span></span>

* <span data-ttu-id="89e7e-123">Die `mailto:` Link wird automatisch erstellt, für die `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="89e7e-123">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="89e7e-124">Die Auswahl von Datum dient zur `DataType.Date` in den meisten Browsern.</span><span class="sxs-lookup"><span data-stu-id="89e7e-124">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="89e7e-125">Die `DataType` Attribut ausgibt HTML 5 `data-` (ausgesprochen als Daten Dash) Attribute, die HTML 5-Browser zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="89e7e-125">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="89e7e-126">Die `DataType` Attribute bieten keine Validierung.</span><span class="sxs-lookup"><span data-stu-id="89e7e-126">The `DataType` attributes do not provide validation.</span></span>

<span data-ttu-id="89e7e-127">`DataType.Date` gibt nicht das Format des Datums an, das angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="89e7e-127">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="89e7e-128">Standardmäßig wird die Date-Felds gemäß der Standardformate basierend auf dem Server angezeigt [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span><span class="sxs-lookup"><span data-stu-id="89e7e-128">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="89e7e-129">Das `DisplayFormat`-Attribut dient zum expliziten Angeben des Datumsformats:</span><span class="sxs-lookup"><span data-stu-id="89e7e-129">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="89e7e-130">Die `ApplyFormatInEditMode` Einstellung gibt an, dass die Formatierung auf die Bearbeitungsoberfläche auch angewendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="89e7e-130">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="89e7e-131">Einige Felder nicht verwenden sollten `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="89e7e-131">Some fields should not use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="89e7e-132">Beispielsweise sollte das Währungssymbol im Allgemeinen nicht in einem Text-Bearbeitungsfeld angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="89e7e-132">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="89e7e-133">Die `DisplayFormat` -Attribut allein verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="89e7e-133">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="89e7e-134">Es wird im Allgemeinen empfiehlt sich, verwenden Sie die `DataType` -Attribut mit dem `DisplayFormat` Attribut.</span><span class="sxs-lookup"><span data-stu-id="89e7e-134">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="89e7e-135">Die `DataType` Attribut übermittelt, die die Semantik der Daten im Gegensatz zu wie auf dem Bildschirm gerendert werden soll.</span><span class="sxs-lookup"><span data-stu-id="89e7e-135">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="89e7e-136">Die `DataType` Attribut bietet die folgenden Vorteile, die in nicht verfügbar sind `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="89e7e-136">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="89e7e-137">Browser kann HTML5-Funktionen aktivieren.</span><span class="sxs-lookup"><span data-stu-id="89e7e-137">The browser can enable HTML5 features.</span></span> <span data-ttu-id="89e7e-138">Zeigen Sie z. B. ein Monatskalender-Steuerelement, das Gebietsschema entsprechende Währungssymbol, Links per e-Mail, die clientseitige Validierung von Benutzereingaben, usw. ein.</span><span class="sxs-lookup"><span data-stu-id="89e7e-138">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="89e7e-139">Standardmäßig wird der Browser Daten im richtigen Format basierend auf dem Gebietsschema gerendert.</span><span class="sxs-lookup"><span data-stu-id="89e7e-139">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="89e7e-140">Weitere Informationen finden Sie unter der [ \<input >-Tag-Helper-Dokumentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="89e7e-140">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="89e7e-141">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="89e7e-141">Run the app.</span></span> <span data-ttu-id="89e7e-142">Navigieren Sie zu der Studenten Indexseite.</span><span class="sxs-lookup"><span data-stu-id="89e7e-142">Navigate to the Students Index page.</span></span> <span data-ttu-id="89e7e-143">Wie oft werden nicht mehr angezeigt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-143">Times are no longer displayed.</span></span> <span data-ttu-id="89e7e-144">Jede Ansicht, verwendet die `Student` Modell zeigt das Datum ohne Uhrzeit an.</span><span class="sxs-lookup"><span data-stu-id="89e7e-144">Every view that uses the `Student` model displays the date without time.</span></span>

![Studenten Indexseite mit Datumsangaben ohne Zeiten](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="89e7e-146">Das StringLength-Attribut</span><span class="sxs-lookup"><span data-stu-id="89e7e-146">The StringLength attribute</span></span>

<span data-ttu-id="89e7e-147">Regeln für die Überprüfung von Daten und Überprüfungsfehlermeldungen können Attribute angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="89e7e-147">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="89e7e-148">Die [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) Attribut gibt an, die minimalen und maximalen Länge von Zeichen, die in einem Datenfeld zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="89e7e-148">The [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="89e7e-149">Die `StringLength` Attribut bietet auch eine clientseitige und serverseitige Validierung.</span><span class="sxs-lookup"><span data-stu-id="89e7e-149">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="89e7e-150">Der minimale Wert hat keine Auswirkungen auf das Datenbankschema.</span><span class="sxs-lookup"><span data-stu-id="89e7e-150">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="89e7e-151">Update der `Student` Modell mit den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="89e7e-151">Update the `Student` model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="89e7e-152">Der vorangehende Code beschränkt die Objektnamen nicht mehr als 50 Zeichen.</span><span class="sxs-lookup"><span data-stu-id="89e7e-152">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="89e7e-153">Die `StringLength` Attribut nicht verhindern, dass einen Benutzer Leerzeichen für einen Namen eingeben.</span><span class="sxs-lookup"><span data-stu-id="89e7e-153">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="89e7e-154">Die [der reguläre Ausdruck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) Attribut wird verwendet, um Einschränkungen auf die Eingabe anwenden.</span><span class="sxs-lookup"><span data-stu-id="89e7e-154">The [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="89e7e-155">Beispielsweise erfordert, dass der folgende Code das erste Zeichen Großbuchstaben bestehen muss und die übrigen Zeichen ein Buchstabe sein:</span><span class="sxs-lookup"><span data-stu-id="89e7e-155">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

<span data-ttu-id="89e7e-156">Führen Sie die app ein:</span><span class="sxs-lookup"><span data-stu-id="89e7e-156">Run the app:</span></span>

* <span data-ttu-id="89e7e-157">Navigieren Sie zur Seite "Students".</span><span class="sxs-lookup"><span data-stu-id="89e7e-157">Navigate to the Students page.</span></span>
* <span data-ttu-id="89e7e-158">Wählen Sie **neu erstellen**, und geben Sie einen Namen, die mehr als 50 Zeichen.</span><span class="sxs-lookup"><span data-stu-id="89e7e-158">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="89e7e-159">Wählen Sie **erstellen**, clientseitige Validierung zeigt eine Fehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-159">Select **Create**, client-side validation shows an error message.</span></span>

![Studenten Indexseite mit Zeichenfolge LÄNGENFEHLER](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="89e7e-161">In **Objekt-Explorer von SQL Server** (SSOX), öffnen Sie die Student-Tabellen-Designer durch Doppelklicken auf die **Student** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="89e7e-161">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Students-Tabelle in SSOX vor der Migration](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="89e7e-163">Die vorhergehende Abbildung zeigt das Schema für die `Student` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="89e7e-163">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="89e7e-164">Die Namensfelder aufweisen `nvarchar(MAX)` da Migrationen auf die Datenbank nicht ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="89e7e-164">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="89e7e-165">Bei der Ausführung von Migrationen weiter unten in diesem Lernprogramm werden die Namensfelder werden `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="89e7e-165">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="89e7e-166">Das Spaltenattribut</span><span class="sxs-lookup"><span data-stu-id="89e7e-166">The Column attribute</span></span>

<span data-ttu-id="89e7e-167">Attribute können steuern, wie Klassen und Eigenschaften in der Datenbank zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="89e7e-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="89e7e-168">In diesem Abschnitt die `Column` Attribut wird verwendet, um den Namen der Zuordnung der `FirstMidName` -Eigenschaft auf "FirstName" in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="89e7e-168">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="89e7e-169">Wenn die Datenbank erstellt wird, werden Eigenschaftennamen für das Modell für Spaltennamen verwendet (außer bei der `Column` Attribut verwendet wird).</span><span class="sxs-lookup"><span data-stu-id="89e7e-169">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="89e7e-170">Die `Student` modellieren verwendet `FirstMidName` für den Vornamen-Feld verwendet wird, weil das Feld kann auch ein zweiter Vorname enthalten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="89e7e-171">Update der *Student.cs* -Datei mit den folgenden hervorgehobenen Code:</span><span class="sxs-lookup"><span data-stu-id="89e7e-171">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="89e7e-172">Mit der vorherigen Änderung `Student.FirstMidName` in der app ordnet die `FirstName` Spalte die `Student` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="89e7e-172">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="89e7e-173">Das Hinzufügen der `Column` Attribut ändert sich das Modell Sichern der `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="89e7e-173">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="89e7e-174">Sicherung des Modells die `SchoolContext` nicht mehr mit die Datenbank überein.</span><span class="sxs-lookup"><span data-stu-id="89e7e-174">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="89e7e-175">Wenn die Anwendung vor dem Anwenden von Migrationen ausgeführt wird, wird die folgende Ausnahme generiert:</span><span class="sxs-lookup"><span data-stu-id="89e7e-175">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```
<span data-ttu-id="89e7e-176">So aktualisieren Sie die Datenbank:</span><span class="sxs-lookup"><span data-stu-id="89e7e-176">To update the DB:</span></span>

* <span data-ttu-id="89e7e-177">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-177">Build the project.</span></span>
* <span data-ttu-id="89e7e-178">Öffnen Sie ein Befehlsfenster in den Projektordner ein.</span><span class="sxs-lookup"><span data-stu-id="89e7e-178">Open a command window in the project folder.</span></span> <span data-ttu-id="89e7e-179">Geben Sie die folgenden Befehle erstellen eine neue Migration und aktualisieren die Datenbank aus:</span><span class="sxs-lookup"><span data-stu-id="89e7e-179">Enter the following commands to create a new migration and update the DB:</span></span>

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

<span data-ttu-id="89e7e-180">Die `dotnet ef migrations add ColumnFirstName` Befehl wird die folgende Warnmeldung generiert:</span><span class="sxs-lookup"><span data-stu-id="89e7e-180">The `dotnet ef migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="89e7e-181">Die Warnung wird generiert, da nun die Felder sind auf 50 Zeichen begrenzt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-181">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="89e7e-182">Wenn Sie einen Namen in der Datenbank mehr als 50 Zeichen hätte, wäre die 51 bis zum letzten Zeichen verloren gehen.</span><span class="sxs-lookup"><span data-stu-id="89e7e-182">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="89e7e-183">Testen der app an.</span><span class="sxs-lookup"><span data-stu-id="89e7e-183">Test the app.</span></span>

<span data-ttu-id="89e7e-184">Öffnen Sie die Student-Tabelle in SSOX:</span><span class="sxs-lookup"><span data-stu-id="89e7e-184">Open the Student table in SSOX:</span></span>

![Students-Tabelle in SSOX nach der Migration](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="89e7e-186">Vor der Migration angewendet wurde, wurden die Spalten des Typs [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="89e7e-186">Before migration was applied, the name columns were of type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="89e7e-187">Die Namensspalten sind jetzt `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="89e7e-187">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="89e7e-188">Name der Spalte geändert hat `FirstMidName` auf `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="89e7e-188">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="89e7e-189">Im folgenden Abschnitt erstellen die app auf einigen Stufen werden Compilerfehler generiert.</span><span class="sxs-lookup"><span data-stu-id="89e7e-189">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="89e7e-190">Die Anweisungen geben, wann die app zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="89e7e-190">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="89e7e-191">Entitätsaktualisierung für Studenten</span><span class="sxs-lookup"><span data-stu-id="89e7e-191">Student entity update</span></span>

![Student-Entität](complex-data-model/_static/student-entity.png)

<span data-ttu-id="89e7e-193">Update *Models/Student.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="89e7e-193">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="89e7e-194">Das erforderliche Attribut</span><span class="sxs-lookup"><span data-stu-id="89e7e-194">The Required attribute</span></span>

<span data-ttu-id="89e7e-195">Die `Required` Attribut macht die erforderlichen Felder des Name-Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="89e7e-195">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="89e7e-196">Die `Required` Attribut ist nicht erforderlich, für NULL-Typen wie Werttypen (`DateTime`, `int`, `double`usw..).</span><span class="sxs-lookup"><span data-stu-id="89e7e-196">The `Required` attribute is not needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="89e7e-197">Typen, die nicht null sein können, werden automatisch als Pflichtfelder behandelt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-197">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="89e7e-198">Die `Required` Attribut konnten ersetzt werden, mit einem minimum Length-Parameter in der `StringLength` Attribut:</span><span class="sxs-lookup"><span data-stu-id="89e7e-198">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="89e7e-199">Das Attribut anzeigen</span><span class="sxs-lookup"><span data-stu-id="89e7e-199">The Display attribute</span></span>

<span data-ttu-id="89e7e-200">Die `Display` Attribut gibt an, dass die Beschriftung für die Textfelder werden soll, "Vorname", "Last Name", "Full Name" und "Registrierungsdatum".</span><span class="sxs-lookup"><span data-stu-id="89e7e-200">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="89e7e-201">Die Standard-Beschriftungen hatte kein Leerzeichen, unterteilen die Wörter ein, z. B. "Lastname".</span><span class="sxs-lookup"><span data-stu-id="89e7e-201">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="89e7e-202">Die berechnete FullName-Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="89e7e-202">The FullName calculated property</span></span>

<span data-ttu-id="89e7e-203">`FullName`ist eine berechnete Eigenschaft, die einen Wert zurückgibt, der durch Verketten von zwei weitere Eigenschaften erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="89e7e-203">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="89e7e-204">`FullName`nicht festgelegt ist, wird es nur einen Get-Accessor verfügt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-204">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="89e7e-205">Nicht `FullName` Spalte in der Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="89e7e-205">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="89e7e-206">Die Instructor-Entität erstellen</span><span class="sxs-lookup"><span data-stu-id="89e7e-206">Create the Instructor Entity</span></span>

![Instructor-Entität](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="89e7e-208">Erstellen Sie *Models/Instructor.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="89e7e-208">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="89e7e-209">Beachten Sie, dass mehrere Eigenschaften in werden der `Student` und `Instructor` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-209">Notice that several properties are the same in the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="89e7e-210">In diesem Lernprogramm Implementieren von Vererbung weiter unten in dieser Serie wird dieser Code umgestaltet, um Redundanzen zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="89e7e-210">In the Implementing Inheritance tutorial later in this series, this code is refactored to eliminate the redundancy.</span></span>

<span data-ttu-id="89e7e-211">Mehrere Attribute können in einer Zeile sein.</span><span class="sxs-lookup"><span data-stu-id="89e7e-211">Multiple attributes can be on one line.</span></span> <span data-ttu-id="89e7e-212">Die `HireDate` Attribute wie folgt geschrieben werden konnte:</span><span class="sxs-lookup"><span data-stu-id="89e7e-212">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="89e7e-213">Die Navigationseigenschaften CourseAssignments und OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="89e7e-213">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="89e7e-214">Die `CourseAssignments` und `OfficeAssignment` Eigenschaften sind Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="89e7e-214">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="89e7e-215">Ein Kursleiter kann eine beliebige Anzahl von Kurse werden folgende Themen behandelt, sodass `CourseAssignments` als eine Auflistung definiert ist.</span><span class="sxs-lookup"><span data-stu-id="89e7e-215">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="89e7e-216">Wenn eine Navigationseigenschaft über mehrere Entitäten enthält:</span><span class="sxs-lookup"><span data-stu-id="89e7e-216">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="89e7e-217">Es muss ein Listentyp, in dem die Einträge hinzugefügt, gelöscht und aktualisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="89e7e-217">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="89e7e-218">Navigation gehören:</span><span class="sxs-lookup"><span data-stu-id="89e7e-218">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="89e7e-219">Wenn `ICollection<T>` angegeben ist, erstellt von EF Core eine `HashSet<T>` Auflistung standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="89e7e-219">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="89e7e-220">Die `CourseAssignment` Entität wird im Abschnitt für die m: n-Beziehungen erläutert.</span><span class="sxs-lookup"><span data-stu-id="89e7e-220">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="89e7e-221">Contoso University Geschäftsregeln Zustand, der ein Kursleiter höchstens eine Niederlassung aufweisen kann.</span><span class="sxs-lookup"><span data-stu-id="89e7e-221">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="89e7e-222">Die `OfficeAssignment` Eigenschaft enthält eine einzelne `OfficeAssignment` Entität.</span><span class="sxs-lookup"><span data-stu-id="89e7e-222">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="89e7e-223">`OfficeAssignment`ist null, wenn keine Office zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="89e7e-223">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="89e7e-224">Erstellen Sie die Entität OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="89e7e-224">Create the OfficeAssignment entity</span></span>

![OfficeAssignment-Entität](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="89e7e-226">Erstellen Sie *Models/OfficeAssignment.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="89e7e-226">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="89e7e-227">Das Key-Attribut</span><span class="sxs-lookup"><span data-stu-id="89e7e-227">The Key attribute</span></span>

<span data-ttu-id="89e7e-228">Die `[Key]` Attribut dient zum Identifizieren einer Eigenschaft als Primärschlüssel (PS) Wenn der Eigenschaftsname etwas ist als ClassnameID oder -ID.</span><span class="sxs-lookup"><span data-stu-id="89e7e-228">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="89e7e-229">Es ist eine auf 0 (null) oder 1 eine Beziehung zwischen der `Instructor` und `OfficeAssignment` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-229">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="89e7e-230">Eine bürozuweisung existiert nur in Bezug auf den Dozenten, dem sie zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="89e7e-230">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="89e7e-231">Die `OfficeAssignment` PK entspricht auch der Fremdschlüssel (FS) die `Instructor` Entität.</span><span class="sxs-lookup"><span data-stu-id="89e7e-231">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="89e7e-232">EF Core nicht automatisch erkannt. `InstructorID` als der Primärschlüssel der `OfficeAssignment` da:</span><span class="sxs-lookup"><span data-stu-id="89e7e-232">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="89e7e-233">`InstructorID`nicht folgen die ID oder ClassnameID-Benennungskonvention.</span><span class="sxs-lookup"><span data-stu-id="89e7e-233">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="89e7e-234">Aus diesem Grund die `Key` Attribut wird verwendet, um zu identifizieren `InstructorID` als der PK:</span><span class="sxs-lookup"><span data-stu-id="89e7e-234">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="89e7e-235">Standardmäßig behandelt EF Core den Schlüssel als nicht-datenbankgeneriert, da die Spalte für eine identifizierende Beziehung ist.</span><span class="sxs-lookup"><span data-stu-id="89e7e-235">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="89e7e-236">Die Navigationseigenschaft für Dozenten</span><span class="sxs-lookup"><span data-stu-id="89e7e-236">The Instructor navigation property</span></span>

<span data-ttu-id="89e7e-237">Die `OfficeAssignment` Navigationseigenschaft für die `Instructor` Entität ist auf NULL festlegbar da:</span><span class="sxs-lookup"><span data-stu-id="89e7e-237">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="89e7e-238">Referenztypen (z. B. Klassen NULL sind).</span><span class="sxs-lookup"><span data-stu-id="89e7e-238">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="89e7e-239">Ein Kursleiter wurden möglicherweise nicht über eine bürozuweisung.</span><span class="sxs-lookup"><span data-stu-id="89e7e-239">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="89e7e-240">Die `OfficeAssignment` Entität hat eine NULL- `Instructor` Navigationseigenschaft da:</span><span class="sxs-lookup"><span data-stu-id="89e7e-240">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="89e7e-241">`InstructorID`ist NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-241">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="89e7e-242">Eine bürozuweisung kann ohne einen Kursleiter vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="89e7e-242">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="89e7e-243">Wenn ein `Instructor` Entität besitzt einen zugehörigen `OfficeAssignment` Entität, jede Entität enthält einen Verweis auf die andere in der Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="89e7e-243">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="89e7e-244">Die `[Required]` Attribut konnte angewendet werden, um die `Instructor` Navigationseigenschaft:</span><span class="sxs-lookup"><span data-stu-id="89e7e-244">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="89e7e-245">Der vorangehende Code gibt an, dass es muss eine verwandte Dozenten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-245">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="89e7e-246">Der vorhergehende Code ist nicht erforderlich, da die `InstructorID` Fremdschlüssel (also auch die PK) ist NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-246">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="89e7e-247">Ändern Sie die Kurs-Entität</span><span class="sxs-lookup"><span data-stu-id="89e7e-247">Modify the Course Entity</span></span>

![Kurs-Entität](complex-data-model/_static/course-entity.png)

<span data-ttu-id="89e7e-249">Update *Models/Course.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="89e7e-249">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="89e7e-250">Die `Course` Entität verfügt über eine Eigenschaft Fremdschlüssel (FS) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="89e7e-250">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="89e7e-251">`DepartmentID`verweist auf den zugehörigen `Department` Entität.</span><span class="sxs-lookup"><span data-stu-id="89e7e-251">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="89e7e-252">Die `Course` Entität verfügt über eine `Department` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="89e7e-252">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="89e7e-253">EF Core erfordert eine FK-Eigenschaft für ein Datenmodell, wenn das Modell eine Navigationseigenschaft für eine verwandte Entität ist.</span><span class="sxs-lookup"><span data-stu-id="89e7e-253">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="89e7e-254">EF Core erstellt FKs automatisch in der Datenbank, wo sie benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="89e7e-254">EF Core automatically creates FKs in the database wherever they are needed.</span></span> <span data-ttu-id="89e7e-255">EF Core erstellt [Shadowing von Eigenschaften](https://docs.microsoft.com/ef/core/modeling/shadow-properties) für automatisch erstellte FKs.</span><span class="sxs-lookup"><span data-stu-id="89e7e-255">EF Core creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="89e7e-256">Mit der FK im Datenmodell kann Updates einfacher und effizienter vornehmen.</span><span class="sxs-lookup"><span data-stu-id="89e7e-256">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="89e7e-257">Betrachten Sie beispielsweise ein Modell, in dem die Eigenschaft FK `DepartmentID` ist *nicht* enthalten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-257">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="89e7e-258">Wenn eine Entität Kurs abgerufen wird, um zu bearbeiten:</span><span class="sxs-lookup"><span data-stu-id="89e7e-258">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="89e7e-259">Die `Department` Entität ist null, wenn sie nicht explizit geladen wird.</span><span class="sxs-lookup"><span data-stu-id="89e7e-259">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="89e7e-260">Zum Aktualisieren der Entität Kurs der `Department` Entität muss zuerst abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="89e7e-260">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="89e7e-261">Wenn die FK-Eigenschaft `DepartmentID` enthalten ist im Datenmodell, besteht keine Notwendigkeit zum Abrufen der `Department` Entität vor einem Update.</span><span class="sxs-lookup"><span data-stu-id="89e7e-261">When the FK property `DepartmentID` is included in the data model, there is no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="89e7e-262">Das DatabaseGenerated-Attribut</span><span class="sxs-lookup"><span data-stu-id="89e7e-262">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="89e7e-263">Die `[DatabaseGenerated(DatabaseGeneratedOption.None)]` Attribut gibt an, dass der Primärschlüssel von der Anwendung bereitgestellt, sondern wird von der Datenbank generiert.</span><span class="sxs-lookup"><span data-stu-id="89e7e-263">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="89e7e-264">Standardmäßig nimmt EF Core PK Werte von der Datenbank generiert werden.</span><span class="sxs-lookup"><span data-stu-id="89e7e-264">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="89e7e-265">DB generiert PK Werte ist in der Regel die beste Herangehensweise.</span><span class="sxs-lookup"><span data-stu-id="89e7e-265">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="89e7e-266">Für `Course` Entitäten, die der Benutzer gibt die PK.</span><span class="sxs-lookup"><span data-stu-id="89e7e-266">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="89e7e-267">Z. B. eine Kurs Zahl wie z. B. eine Reihe 1000 für die Math-Abteilung, eine Reihe 2000 für die englischen Abteilung.</span><span class="sxs-lookup"><span data-stu-id="89e7e-267">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="89e7e-268">Die `DatabaseGenerated` Attribut kann auch zum Generieren der Standardwerte verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="89e7e-268">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="89e7e-269">Beispielsweise kann die Datenbank automatisch ein Datumsfeld so, dass das Datum aufgezeichnet wird, das eine Zeile erstellt oder aktualisiert wurde, generieren.</span><span class="sxs-lookup"><span data-stu-id="89e7e-269">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="89e7e-270">Weitere Informationen finden Sie unter [Eigenschaften generiert](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="89e7e-270">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="89e7e-271">Foreign Key- und Navigation-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="89e7e-271">Foreign key and navigation properties</span></span>

<span data-ttu-id="89e7e-272">Der Fremdschlüssel (FS) Eigenschaften und Navigationseigenschaften in den `Course` Entität wider, die folgenden Beziehungen:</span><span class="sxs-lookup"><span data-stu-id="89e7e-272">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="89e7e-273">Ein Kurs wird an eine Abteilung zugewiesen, daher besteht eine `DepartmentID` FK und ein `Department` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="89e7e-273">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="89e7e-274">Ein Kurs kann eine beliebige Anzahl von Lernende, haben also die `Enrollments` Navigationseigenschaft ist eine Sammlung:</span><span class="sxs-lookup"><span data-stu-id="89e7e-274">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="89e7e-275">Ein Kurs kann von mehreren Lehrkräfte vermittelten werden daher die `CourseAssignments` Navigationseigenschaft ist eine Sammlung:</span><span class="sxs-lookup"><span data-stu-id="89e7e-275">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="89e7e-276">`CourseAssignment`erläutert [später](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="89e7e-276">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="89e7e-277">Erstellen Sie die Entität Department</span><span class="sxs-lookup"><span data-stu-id="89e7e-277">Create the Department entity</span></span>

![Entität Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="89e7e-279">Erstellen Sie *Models/Department.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="89e7e-279">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="89e7e-280">Das Spaltenattribut</span><span class="sxs-lookup"><span data-stu-id="89e7e-280">The Column attribute</span></span>

<span data-ttu-id="89e7e-281">Zuvor die `Column` Attribut wurde verwendet, um die Zuordnung für die Namen von Spalten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="89e7e-281">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="89e7e-282">Im Code für die `Department` Entität, die `Column` Attribut wird verwendet, um die SQL-datentypzuordnung zu ändern.</span><span class="sxs-lookup"><span data-stu-id="89e7e-282">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="89e7e-283">Die `Budget` Spalte wird mit den SQL Server-Money-Datentyp in der DB definiert:</span><span class="sxs-lookup"><span data-stu-id="89e7e-283">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="89e7e-284">Die Zuordnung ist im Allgemeinen nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="89e7e-284">Column mapping is generally not required.</span></span> <span data-ttu-id="89e7e-285">EF Core wählt im Allgemeinen die entsprechenden SQL Server-Datentyp, der basierend auf den CLR-Typ für die Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="89e7e-285">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="89e7e-286">Die CLR `decimal` wird zugeordnet zu SQL Server `decimal` Typ.</span><span class="sxs-lookup"><span data-stu-id="89e7e-286">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="89e7e-287">`Budget`ist für die Währung, und der Money-Datentyp eignet sich besser für die Währung.</span><span class="sxs-lookup"><span data-stu-id="89e7e-287">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="89e7e-288">Foreign Key- und Navigation-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="89e7e-288">Foreign key and navigation properties</span></span>

<span data-ttu-id="89e7e-289">Die folgenden Beziehungen entsprechen den FK und Navigation-Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="89e7e-289">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="89e7e-290">Eine Abteilung möglicherweise nicht haben oder einen Administrator.</span><span class="sxs-lookup"><span data-stu-id="89e7e-290">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="89e7e-291">Ein Administrator ist immer einen Kursleiter.</span><span class="sxs-lookup"><span data-stu-id="89e7e-291">An administrator is always an instructor.</span></span> <span data-ttu-id="89e7e-292">Aus diesem Grund die `InstructorID` Eigenschaft ist als die FK, enthalten die `Instructor` Entität.</span><span class="sxs-lookup"><span data-stu-id="89e7e-292">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="89e7e-293">Die Navigationseigenschaft heißt `Administrator` jedoch enthält ein `Instructor` Entität:</span><span class="sxs-lookup"><span data-stu-id="89e7e-293">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="89e7e-294">Das Fragezeichen (?) im vorangehenden Code gibt an, dass die Eigenschaft auf NULL festlegbar ist.</span><span class="sxs-lookup"><span data-stu-id="89e7e-294">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="89e7e-295">Eine Abteilung möglicherweise viele Kurse, daher eine Navigationseigenschaft Kurse besteht:</span><span class="sxs-lookup"><span data-stu-id="89e7e-295">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="89e7e-296">Hinweis: EF Core können gemäß der Konvention kaskadierendes Delete für NULL--FKS. und für viele-zu-viele-Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="89e7e-296">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="89e7e-297">Löschweitergabe kann zirkuläre Cascade Delete Regeln führen.</span><span class="sxs-lookup"><span data-stu-id="89e7e-297">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="89e7e-298">Zirkuläre kaskadierte Löschung Regeln verursacht eine Ausnahme, wenn eine Migration hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="89e7e-298">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="89e7e-299">Beispielsweise, wenn die `Department.InstructorID` -Eigenschaft wurde nicht als auf NULL festlegbar definiert:</span><span class="sxs-lookup"><span data-stu-id="89e7e-299">For example, if the `Department.InstructorID` property was not defined as nullable:</span></span>

* <span data-ttu-id="89e7e-300">EF Core konfiguriert eine Cascade Delete-Regel klicken, um den Dozenten zu löschen, wenn die Abteilung gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="89e7e-300">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="89e7e-301">Den Dozenten löschen, wenn die Abteilung gelöscht wird, ist nicht das beabsichtigte Verhalten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-301">Deleting the instructor when the department is deleted is not the intended behavior.</span></span>

<span data-ttu-id="89e7e-302">Bei Bedarf von Geschäftsregeln die `InstructorID` Eigenschaft werden keine NULL-Werte zulässt, verwenden Sie die folgenden fluent-API-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="89e7e-302">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="89e7e-303">Der vorangehende Code deaktiviert kaskadierte Löschung auf der Abteilung Instructor-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="89e7e-303">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="89e7e-304">Aktualisieren Sie die Registrierung-Entität</span><span class="sxs-lookup"><span data-stu-id="89e7e-304">Update the Enrollment entity</span></span>

<span data-ttu-id="89e7e-305">Ein Anmeldungsdatensatz ist für einen einen Kurs von einem Studenten ausgeführte.</span><span class="sxs-lookup"><span data-stu-id="89e7e-305">An enrollment record is for a one course taken by one student.</span></span>

![Registrierung-Entität](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="89e7e-307">Update *Models/Enrollment.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="89e7e-307">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="89e7e-308">Foreign Key- und Navigation-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="89e7e-308">Foreign key and navigation properties</span></span>

<span data-ttu-id="89e7e-309">Die FK-Eigenschaften und Navigationseigenschaften reflektieren die folgenden Beziehungen:</span><span class="sxs-lookup"><span data-stu-id="89e7e-309">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="89e7e-310">Ein Anmeldungsdatensatz ist für einen Kurs, es ist also eine `CourseID` FK-Eigenschaft und eine `Course` Navigationseigenschaft:</span><span class="sxs-lookup"><span data-stu-id="89e7e-310">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="89e7e-311">Ein Anmeldungsdatensatz ist für ein Student, es ist also eine `StudentID` FK-Eigenschaft und eine `Student` Navigationseigenschaft:</span><span class="sxs-lookup"><span data-stu-id="89e7e-311">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="89e7e-312">n:n-Beziehungen</span><span class="sxs-lookup"><span data-stu-id="89e7e-312">Many-to-Many Relationships</span></span>

<span data-ttu-id="89e7e-313">Es ist eine m: n-Beziehung zwischen der `Student` und `Course` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-313">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="89e7e-314">Die `Enrollment` Entität als Jointabelle m: n-Funktionen *mit Nutzlast* in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="89e7e-314">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="89e7e-315">"Mit der Nutzlast" bedeutet, dass die `Enrollment` Tabelle enthält zusätzliche Daten außer FKs für den verknüpften Tabellen (in diesem Fall wird der Primärschlüssel und `Grade`).</span><span class="sxs-lookup"><span data-stu-id="89e7e-315">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="89e7e-316">Die folgende Abbildung zeigt, wie diese Beziehungen in einem Entitätsdiagramm aussieht.</span><span class="sxs-lookup"><span data-stu-id="89e7e-316">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="89e7e-317">(Dieses Diagramm generiert wurde, mithilfe von EF Power Tools für EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="89e7e-317">(This diagram was generated using EF Power Tools for EF 6.x.</span></span> <span data-ttu-id="89e7e-318">Erstellen das Diagramm ist nicht Teil des Lernprogramms.)</span><span class="sxs-lookup"><span data-stu-id="89e7e-318">Creating the diagram isn't part of the tutorial.)</span></span>

![Student-Kurs m: n-Beziehung](complex-data-model/_static/student-course.png)

<span data-ttu-id="89e7e-320">Jede Beziehungslinie hat eine 1 an einem Ende und ein Sternchen (*) am anderen Eintrag, der angibt, einer 1: n-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="89e7e-320">Each relationship line has a 1 at one end and an asterisk (*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="89e7e-321">Wenn die `Enrollment` Tabelle haben nicht Grade Informationen enthalten, er dürfte nur die zwei-FKS. enthalten (`CourseID` und `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="89e7e-321">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="89e7e-322">Eine m: n-Jointabelle ohne Nutzlast ist eine reine Jointabelle (PJT) bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="89e7e-322">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="89e7e-323">Die `Instructor` und `Course` Entitäten verfügen über eine m: n-Beziehung mit einer reinen Jointabelle.</span><span class="sxs-lookup"><span data-stu-id="89e7e-323">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="89e7e-324">Hinweis: EF 6.x unterstützt die implizite Verknüpfung von Tabellen für m: n-Beziehungen, aber EF Core nicht.</span><span class="sxs-lookup"><span data-stu-id="89e7e-324">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core does not.</span></span> <span data-ttu-id="89e7e-325">Weitere Informationen finden Sie unter [m: n-Beziehungen in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="89e7e-325">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="89e7e-326">Die CourseAssignment-Entität</span><span class="sxs-lookup"><span data-stu-id="89e7e-326">The CourseAssignment entity</span></span>

![CourseAssignment-Entität](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="89e7e-328">Erstellen Sie *Models/CourseAssignment.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="89e7e-328">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="89e7e-329">Instructor-zu-Kurse</span><span class="sxs-lookup"><span data-stu-id="89e7e-329">Instructor-to-Courses</span></span>

![Instructor-Kurse-m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="89e7e-331">Die Instructor-Kurse-viele-zu-viele-Beziehung:</span><span class="sxs-lookup"><span data-stu-id="89e7e-331">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="89e7e-332">Erfordert eine Jointabelle, die durch eine Entitätenmenge dargestellt werden muss.</span><span class="sxs-lookup"><span data-stu-id="89e7e-332">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="89e7e-333">Ist eine reine Jointabelle (Tabelle ohne Nutzlast).</span><span class="sxs-lookup"><span data-stu-id="89e7e-333">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="89e7e-334">Es ist üblich, benennen Sie eine Joinentität `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="89e7e-334">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="89e7e-335">Die Instructor-Kurse-Join-Tabelle, die mithilfe des folgenden Musters ist z. B. `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="89e7e-335">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="89e7e-336">Allerdings wird empfohlen, unter Verwendung eines Namens, der die Beziehung beschreibt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-336">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="89e7e-337">Datenmodelle fangen einfache und vergrößert.</span><span class="sxs-lookup"><span data-stu-id="89e7e-337">Data models start out simple and grow.</span></span> <span data-ttu-id="89e7e-338">No-Nutzlast Joins (PJTs) weiterentwickelt häufig um Nutzlast enthalten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-338">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="89e7e-339">Beginnen Sie mit einem aussagekräftigen Entitätsname, muss der Namen ändern, wenn die verknüpften Tabelle ändert.</span><span class="sxs-lookup"><span data-stu-id="89e7e-339">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="89e7e-340">Im Idealfall würden die Joinentität einen eigene natürlichen (möglicherweise einzelne Wort)-Namen in der Business-Domäne haben.</span><span class="sxs-lookup"><span data-stu-id="89e7e-340">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="89e7e-341">Beispielsweise konnte Büchern und Kunden mit einem Joinentität mit dem Namen Bewertungen verknüpft werden.</span><span class="sxs-lookup"><span data-stu-id="89e7e-341">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="89e7e-342">Für die Instructor-Kurse-viele-zu-viele-Beziehung `CourseAssignment` ist die bevorzugte über `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="89e7e-342">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="89e7e-343">Zusammengesetzter Schlüssel</span><span class="sxs-lookup"><span data-stu-id="89e7e-343">Composite key</span></span>

<span data-ttu-id="89e7e-344">FKs sind keine NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-344">FKs are not nullable.</span></span> <span data-ttu-id="89e7e-345">Die beiden-FKS. in `CourseAssignment` (`InstructorID` und `CourseID`) zusammen eindeutig identifizieren jede Zeile der `CourseAssignment` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="89e7e-345">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="89e7e-346">`CourseAssignment`erfordert eine dedizierte PK.</span><span class="sxs-lookup"><span data-stu-id="89e7e-346">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="89e7e-347">Die `InstructorID` und `CourseID` Eigenschaften Funktion, wie eine zusammengesetzte PK.</span><span class="sxs-lookup"><span data-stu-id="89e7e-347">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="89e7e-348">Die einzige Möglichkeit, geben Sie zusammengesetzte PKs EF-Kern ist mit der *fluent-API*.</span><span class="sxs-lookup"><span data-stu-id="89e7e-348">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="89e7e-349">Im nächste Abschnitt wird gezeigt, wie so konfigurieren Sie die zusammengesetzte PK.</span><span class="sxs-lookup"><span data-stu-id="89e7e-349">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="89e7e-350">Des zusammengesetzten Schlüssels wird sichergestellt, dass:</span><span class="sxs-lookup"><span data-stu-id="89e7e-350">The composite key ensures:</span></span>

* <span data-ttu-id="89e7e-351">Für einen Kurs sind mehrere Zeilen zulässig.</span><span class="sxs-lookup"><span data-stu-id="89e7e-351">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="89e7e-352">Für eine Instructor sind mehrere Zeilen zulässig.</span><span class="sxs-lookup"><span data-stu-id="89e7e-352">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="89e7e-353">Mehrere Zeilen für die gleiche Dozenten und Kurs ist nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="89e7e-353">Multiple rows for the same instructor and course is not allowed.</span></span>

<span data-ttu-id="89e7e-354">Die `Enrollment` Joinentität definiert seine eigenen PK Duplikate dieser Art möglich sind.</span><span class="sxs-lookup"><span data-stu-id="89e7e-354">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="89e7e-355">So verhindern Sie solche Duplikate:</span><span class="sxs-lookup"><span data-stu-id="89e7e-355">To prevent such duplicates:</span></span>

* <span data-ttu-id="89e7e-356">Fügen Sie einen eindeutigen Index für die Felder FK oder</span><span class="sxs-lookup"><span data-stu-id="89e7e-356">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="89e7e-357">Konfigurieren Sie `Enrollment` mit einem zusammengesetzten Primärschlüssel ähnelt `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="89e7e-357">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="89e7e-358">Weitere Informationen finden Sie unter [Indizes](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="89e7e-358">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="89e7e-359">Aktualisieren Sie den Datenbankkontext</span><span class="sxs-lookup"><span data-stu-id="89e7e-359">Update the DB context</span></span>

<span data-ttu-id="89e7e-360">Fügen Sie den folgenden hervorgehobenen Code hinzu *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="89e7e-360">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="89e7e-361">Der vorangehende Code fügt das neuen Entitäten und konfiguriert die `CourseAssignment` Entität zusammengesetzten PK.</span><span class="sxs-lookup"><span data-stu-id="89e7e-361">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="89e7e-362">Fluent-API-Alternative zu Attributen</span><span class="sxs-lookup"><span data-stu-id="89e7e-362">Fluent API alternative to attributes</span></span>

<span data-ttu-id="89e7e-363">Die `OnModelCreating` Methode im vorangehenden code verwendet die *fluent-API* EF Kernverhalten konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="89e7e-363">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="89e7e-364">Die API wird "fluent" bezeichnet, da es häufig von einer Reihe von Methodenaufrufen zusammen in einer einzigen Anweisung Vermittlungsstellen verwendet.</span><span class="sxs-lookup"><span data-stu-id="89e7e-364">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="89e7e-365">Die [folgenden Code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) ist ein Beispiel für die fluent-API:</span><span class="sxs-lookup"><span data-stu-id="89e7e-365">The [following code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="89e7e-366">In diesem Lernprogramm wird die fluent-API nur für DB-Zuordnung verwendet, die mit Attributen erfolgen kann.</span><span class="sxs-lookup"><span data-stu-id="89e7e-366">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="89e7e-367">Die fluent-API kann jedoch die meisten Formatierung, Überprüfung und Zuordnungsregeln an, die mit Attributen erfolgen angeben.</span><span class="sxs-lookup"><span data-stu-id="89e7e-367">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="89e7e-368">Einige Attribute wie z. B. `MinimumLength` kann nicht mit der fluent-API angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="89e7e-368">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="89e7e-369">`MinimumLength`Ändern Sie nicht das Schema gilt nur für eine Validierungsregel für die minimale Länge.</span><span class="sxs-lookup"><span data-stu-id="89e7e-369">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="89e7e-370">Einige Entwickler bevorzugen die fluent-API ausschließlich, damit sie ihre Entitätsklassen "bereinigen" bleibt</span><span class="sxs-lookup"><span data-stu-id="89e7e-370">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="89e7e-371">Attribute und die fluent-API können gemischt werden.</span><span class="sxs-lookup"><span data-stu-id="89e7e-371">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="89e7e-372">Es gibt einige Konfigurationen, die nur mit der fluent-API ausgeführt werden können (das Angeben einer zusammengesetzten PK).</span><span class="sxs-lookup"><span data-stu-id="89e7e-372">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="89e7e-373">Es gibt einige Konfigurationen, die mit Attributen nur ausgeführt werden können (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="89e7e-373">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="89e7e-374">Die empfohlene Vorgehensweise für die Verwendung von fluent-API oder Attribute:</span><span class="sxs-lookup"><span data-stu-id="89e7e-374">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="89e7e-375">Wählen Sie eine dieser beiden Ansätze.</span><span class="sxs-lookup"><span data-stu-id="89e7e-375">Choose one of these two approaches.</span></span>
* <span data-ttu-id="89e7e-376">Verwenden Sie die ausgewählten Ansatz einheitlich so weit wie möglich.</span><span class="sxs-lookup"><span data-stu-id="89e7e-376">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="89e7e-377">Einige der Attribute in diesem Lernprogramm werden zum:</span><span class="sxs-lookup"><span data-stu-id="89e7e-377">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="89e7e-378">Nur Überprüfung (z. B. `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="89e7e-378">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="89e7e-379">Nur EF-kernkonfiguration (z. B. `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="89e7e-379">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="89e7e-380">Überprüfung und EF-Core-Konfiguration (z. B. `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="89e7e-380">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="89e7e-381">Weitere Informationen zu Attributen im Vergleich zu fluent-API finden Sie unter [Methoden der Konfiguration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="89e7e-381">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="89e7e-382">Anzeigen von Entitätsbeziehungen-Diagramm</span><span class="sxs-lookup"><span data-stu-id="89e7e-382">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="89e7e-383">Die folgende Abbildung zeigt das Diagramm, das EF-Powertools für das abgeschlossene Modell "School" zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="89e7e-383">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Entitätsdiagramm](complex-data-model/_static/diagram.png)

<span data-ttu-id="89e7e-385">Die vorhergehende Abbildung zeigt:</span><span class="sxs-lookup"><span data-stu-id="89e7e-385">The preceding diagram shows:</span></span>

* <span data-ttu-id="89e7e-386">1: n-Beziehung Zeilenumbrüche (1 bis \*).</span><span class="sxs-lookup"><span data-stu-id="89e7e-386">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="89e7e-387">Zu 0 (null) oder eins eine Beziehungslinie (1 zu 0.. 1) zwischen den `Instructor` und `OfficeAssignment` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-387">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="89e7e-388">0 (null)-oder-1-n-Beziehung (0.. 1 auf *) zwischen den `Instructor` und `Department` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-388">The zero-or-one-to-many relationship line (0..1 to *) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="89e7e-389">Ausgangswert für die Datenbank mit der Testdaten</span><span class="sxs-lookup"><span data-stu-id="89e7e-389">Seed the DB with Test Data</span></span>

<span data-ttu-id="89e7e-390">Aktualisieren Sie den Code in *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="89e7e-390">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="89e7e-391">Der vorangehende Code stellt Seed-Daten für die neuen Entitäten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-391">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="89e7e-392">Die meisten dieser Code erstellt neue Entitätsobjekten und lädt Beispieldaten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-392">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="89e7e-393">Die Beispieldaten werden für Tests verwendet.</span><span class="sxs-lookup"><span data-stu-id="89e7e-393">The sample data is used for testing.</span></span> <span data-ttu-id="89e7e-394">Der vorangehende Code erstellt die folgenden viele-zu-viele-Beziehungen:</span><span class="sxs-lookup"><span data-stu-id="89e7e-394">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

<span data-ttu-id="89e7e-395">Hinweis: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) unterstützt [datenseeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span><span class="sxs-lookup"><span data-stu-id="89e7e-395">Note: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) will support [data seeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="89e7e-396">Fügen Sie eine migration</span><span class="sxs-lookup"><span data-stu-id="89e7e-396">Add a migration</span></span>

<span data-ttu-id="89e7e-397">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-397">Build the project.</span></span> <span data-ttu-id="89e7e-398">Öffnen Sie ein Befehlsfenster, das in den Projektordner, und geben Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="89e7e-398">Open a command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="89e7e-399">Dieser Befehl zeigt eine Warnung bezüglich eines möglichen Datenverlusts.</span><span class="sxs-lookup"><span data-stu-id="89e7e-399">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="89e7e-400">Wenn die `database update` Befehl ausgeführt wird, wird die folgende Fehlermeldung erzeugt:</span><span class="sxs-lookup"><span data-stu-id="89e7e-400">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="89e7e-401">Bei der Ausführung von Migrationen mit vorhandenen Daten werden möglicherweise FK-Einschränkungen, die nicht mit den vorhandenen Daten erfüllt werden.</span><span class="sxs-lookup"><span data-stu-id="89e7e-401">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="89e7e-402">In diesem Lernprogramm wird eine neue Datenbank erstellt, daher gibt es keine FK einschränkungsverletzungen.</span><span class="sxs-lookup"><span data-stu-id="89e7e-402">For this tutorial, a new DB is created, so there are no FK constraint violations.</span></span> <span data-ttu-id="89e7e-403">Finden Sie unter [korrigieren und foreign Key-Einschränkungen mit Legacydaten](#fk) Anleitungen Verstößen der FK auf der aktuellen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="89e7e-403">See [Fixing foreign key constraints with legacy data](#fk) for instructions on how to fix the FK violations on the current DB.</span></span>

## <a name="change-the-connection-string-and-update-the-db"></a><span data-ttu-id="89e7e-404">Ändern Sie die Verbindungszeichenfolge und aktualisieren die Datenbank</span><span class="sxs-lookup"><span data-stu-id="89e7e-404">Change the connection string and update the DB</span></span>

<span data-ttu-id="89e7e-405">Der Code in der aktualisierten `DbInitializer` Seed-Daten für die neue Entitäten hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-405">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="89e7e-406">So erzwingen Sie EF-Core, um eine neue leere Datenbank zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="89e7e-406">To force EF Core to create a new empty DB:</span></span>

* <span data-ttu-id="89e7e-407">Ändern der Name der Datenbankverbindungszeichenfolge in *appsettings.json* auf ContosoUniversity3.</span><span class="sxs-lookup"><span data-stu-id="89e7e-407">Change the DB connection string name in *appsettings.json* to ContosoUniversity3.</span></span> <span data-ttu-id="89e7e-408">Der neue Name muss ein Name sein, die auf dem Computer verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="89e7e-408">The new name must be a name that hasn't been used on the computer.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* <span data-ttu-id="89e7e-409">Löschen Sie alternativ die DB-verwenden:</span><span class="sxs-lookup"><span data-stu-id="89e7e-409">Alternatively, delete the DB using:</span></span>

    * <span data-ttu-id="89e7e-410">**Objekt-Explorer von SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="89e7e-410">**SQL Server Object Explorer** (SSOX).</span></span>
    * <span data-ttu-id="89e7e-411">Die `database drop` CLI-Befehl:</span><span class="sxs-lookup"><span data-stu-id="89e7e-411">The `database drop` CLI command:</span></span>

   ```console
   dotnet ef database drop
   ```

<span data-ttu-id="89e7e-412">Führen Sie `database update` im Befehlsfenster:</span><span class="sxs-lookup"><span data-stu-id="89e7e-412">Run `database update` in the command window:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="89e7e-413">Dieser Befehl wird bei allen Migrationen ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-413">The preceding command runs all the migrations.</span></span>

<span data-ttu-id="89e7e-414">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="89e7e-414">Run the app.</span></span> <span data-ttu-id="89e7e-415">Ausführen der app-Ausführung der `DbInitializer.Initialize` Methode.</span><span class="sxs-lookup"><span data-stu-id="89e7e-415">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="89e7e-416">Die `DbInitializer.Initialize` füllt die neue DB.</span><span class="sxs-lookup"><span data-stu-id="89e7e-416">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="89e7e-417">Öffnen Sie die Datenbank im SSOX:</span><span class="sxs-lookup"><span data-stu-id="89e7e-417">Open the DB in SSOX:</span></span>

* <span data-ttu-id="89e7e-418">Erweitern Sie die **Tabellen** Knoten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-418">Expand the **Tables** node.</span></span> <span data-ttu-id="89e7e-419">Die erstellten Tabellen werden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="89e7e-419">The created tables are displayed.</span></span>
* <span data-ttu-id="89e7e-420">Wenn zuvor SSOX geöffnet wurde, klicken Sie auf die **aktualisieren** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="89e7e-420">If SSOX was opened previously, click the **Refresh** button.</span></span>

![Tabellen in SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="89e7e-422">Überprüfen Sie die **CourseAssignment** Tabelle:</span><span class="sxs-lookup"><span data-stu-id="89e7e-422">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="89e7e-423">Mit der rechten Maustaste die **CourseAssignment** Tabelle, und wählen Sie **Ansichtsdaten**.</span><span class="sxs-lookup"><span data-stu-id="89e7e-423">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="89e7e-424">Überprüfen Sie die **CourseAssignment** Tabelle Daten enthält.</span><span class="sxs-lookup"><span data-stu-id="89e7e-424">Verify the **CourseAssignment** table contains data.</span></span>

![CourseAssignment Daten in SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a><span data-ttu-id="89e7e-426">Beheben von foreign Key-Einschränkungen mit älteren Daten</span><span class="sxs-lookup"><span data-stu-id="89e7e-426">Fixing foreign key constraints with legacy data</span></span>

<span data-ttu-id="89e7e-427">In diesem Abschnitt ist optional.</span><span class="sxs-lookup"><span data-stu-id="89e7e-427">This section is optional.</span></span>

<span data-ttu-id="89e7e-428">Bei der Ausführung von Migrationen mit vorhandenen Daten werden möglicherweise FK-Einschränkungen, die nicht mit den vorhandenen Daten erfüllt werden.</span><span class="sxs-lookup"><span data-stu-id="89e7e-428">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="89e7e-429">Mit Produktionsdaten müssen Schritte ausgeführt werden, um die vorhandenen Daten zu migrieren.</span><span class="sxs-lookup"><span data-stu-id="89e7e-429">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="89e7e-430">Dieser Abschnitt enthält ein Beispiel für FK einschränkungsverletzungen beheben.</span><span class="sxs-lookup"><span data-stu-id="89e7e-430">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="89e7e-431">Machen Sie keine dieser Änderungen am Code ohne Sicherung.</span><span class="sxs-lookup"><span data-stu-id="89e7e-431">Don't make these code changes without a backup.</span></span> <span data-ttu-id="89e7e-432">Machen Sie keine dieser Code ändert, wenn Sie im vorherigen Abschnitt abgeschlossen und die Datenbank aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="89e7e-432">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="89e7e-433">Die *{timestamp}_ComplexDataModel.cs* Datei enthält den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="89e7e-433">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="89e7e-434">Der vorangehende Code Fügt einen NULL- `DepartmentID` FK auf die `Course` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="89e7e-434">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="89e7e-435">Die Datenbank aus dem vorherigen Lernprogramm enthält Zeilen in `Course`, sodass dieser Tabelle durch Migrationen aktualisiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="89e7e-435">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="89e7e-436">Vornehmen der `ComplexDataModel` Migration mit vorhandenen Daten zu arbeiten:</span><span class="sxs-lookup"><span data-stu-id="89e7e-436">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="89e7e-437">Ändern Sie den Code, um die neue Spalte zu gewähren (`DepartmentID`) einen Standardwert.</span><span class="sxs-lookup"><span data-stu-id="89e7e-437">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="89e7e-438">Erstellen Sie eine gefälschte Abteilung, die mit dem Namen "Temp" als Standard-Abteilung zu agieren.</span><span class="sxs-lookup"><span data-stu-id="89e7e-438">Create a fake department named "Temp" to act as the default department.</span></span>

### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="89e7e-439">Korrigieren Sie die foreign Key-Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="89e7e-439">Fix the foreign key constraints</span></span>

<span data-ttu-id="89e7e-440">Update der `ComplexDataModel` Klassen `Up` Methode:</span><span class="sxs-lookup"><span data-stu-id="89e7e-440">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="89e7e-441">Öffnen der *{timestamp}_ComplexDataModel.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="89e7e-441">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="89e7e-442">Kommentieren Sie die Codezeile fügt die `DepartmentID` Spalte die `Course` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="89e7e-442">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="89e7e-443">Fügen Sie den folgenden hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="89e7e-443">Add the following highlighted code.</span></span> <span data-ttu-id="89e7e-444">Der neue Code wechselt nach der `.CreateTable( name: "Department"` blockieren:[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="89e7e-444">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="89e7e-445">Mit den vorhergehenden Änderungen, die vorhandenen `Course` Zeilen werden im Zusammenhang mit der Abteilung "Temp" nach der `ComplexDataModel` `Up` Methode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="89e7e-445">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="89e7e-446">Eine produktionsanwendung würde:</span><span class="sxs-lookup"><span data-stu-id="89e7e-446">A production app would:</span></span>

* <span data-ttu-id="89e7e-447">Einschließen von Code oder Skripts hinzufügen `Department` Zeilen und verwandte `Course` Zeilen mit dem neuen `Department` Zeilen.</span><span class="sxs-lookup"><span data-stu-id="89e7e-447">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="89e7e-448">Verwendet nicht die Abteilung "Temp" oder den Standardwert für `Course.DepartmentID `.</span><span class="sxs-lookup"><span data-stu-id="89e7e-448">Would not use the "Temp" department or the default value for `Course.DepartmentID `.</span></span>

<span data-ttu-id="89e7e-449">Das nächste Lernprogramm behandelt die verknüpfte Daten.</span><span class="sxs-lookup"><span data-stu-id="89e7e-449">The next tutorial covers related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="89e7e-450">[Zurück](xref:data/ef-rp/migrations)
[Weiter](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="89e7e-450">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>