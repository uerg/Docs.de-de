---
title: Razor-Seiten mit EF-Kern - Datenmodell - 5 8
author: rick-anderson
description: "In diesem Lernprogramm fügen Sie weitere Entitäten und Beziehungen und das Datenmodell anpassen, indem Sie Formatierung, Überprüfung und Zuordnungsregeln für die Datenbank angeben."
manager: wpickett
ms.author: riande
ms.date: 10/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 58bb773ba16314827da84909def05a8ef370479b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a><span data-ttu-id="adce0-103">Erstellen ein Modell mit komplexen Daten - EF-Core mit Razor-Seiten Lernprogramm (5 von 8)</span><span class="sxs-lookup"><span data-stu-id="adce0-103">Creating a complex data model - EF Core with Razor Pages tutorial (5 of 8)</span></span>

<span data-ttu-id="adce0-104">Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="adce0-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="adce0-105">In den vorherigen Lernprogrammen arbeitet mit einer einfachen Datenmodell, das von drei Entitäten verfasst wurde.</span><span class="sxs-lookup"><span data-stu-id="adce0-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="adce0-106">In diesem Lernprogramm:</span><span class="sxs-lookup"><span data-stu-id="adce0-106">In this tutorial:</span></span>

* <span data-ttu-id="adce0-107">Weitere Entitäten und Beziehungen werden hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="adce0-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="adce0-108">Das Datenmodell wird durch Angeben von Formatierung, Überprüfung und Zuordnungsregeln für die Datenbank angepasst.</span><span class="sxs-lookup"><span data-stu-id="adce0-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="adce0-109">Die Entitätsklassen für das vollständige Datenmodell ist in der folgenden Abbildung dargestellt:</span><span class="sxs-lookup"><span data-stu-id="adce0-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Entitätsdiagramm](complex-data-model/_static/diagram.png)

<span data-ttu-id="adce0-111">Wenn Probleme können nicht zu lösen auftreten, laden Sie die [abgeschlossene Anwendung für diese Stufe](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span><span class="sxs-lookup"><span data-stu-id="adce0-111">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="adce0-112">Das Datenmodell mit Attributen anpassen</span><span class="sxs-lookup"><span data-stu-id="adce0-112">Customize the data model with attributes</span></span>

<span data-ttu-id="adce0-113">In diesem Abschnitt wird das Datenmodell mithilfe von Attributen angepasst.</span><span class="sxs-lookup"><span data-stu-id="adce0-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="adce0-114">Das DataType-Attribut</span><span class="sxs-lookup"><span data-stu-id="adce0-114">The DataType attribute</span></span>

<span data-ttu-id="adce0-115">Die Student-Seiten derzeit zeigt an, der das Registrierungsdatum.</span><span class="sxs-lookup"><span data-stu-id="adce0-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="adce0-116">Date-Felder anzeigen in der Regel nur das Datum und nicht die Zeit.</span><span class="sxs-lookup"><span data-stu-id="adce0-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="adce0-117">Update *Models/Student.cs* mit den folgenden hervorgehobenen Code:</span><span class="sxs-lookup"><span data-stu-id="adce0-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="adce0-118">Die [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) Attribut gibt an, einen Datentyp, der als Typ der systeminternen genauer bestimmt ist.</span><span class="sxs-lookup"><span data-stu-id="adce0-118">The [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="adce0-119">In diesem Fall nur das Datum angezeigt werden soll, nicht das Datum und die Uhrzeit.</span><span class="sxs-lookup"><span data-stu-id="adce0-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="adce0-120">Die [DataType-Enumeration](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) stellt für viele Datentypen, z. B. Datum, Uhrzeit, PhoneNumber, Währung, EmailAddress usw. bereit. Die `DataType` Attribut kann auch aktivieren, die app-spezifische Funktionen automatisch bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="adce0-120">The [DataType Enumeration](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="adce0-121">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="adce0-121">For example:</span></span>

* <span data-ttu-id="adce0-122">Die `mailto:` Link wird automatisch erstellt, für die `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="adce0-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="adce0-123">Die Auswahl von Datum dient zur `DataType.Date` in den meisten Browsern.</span><span class="sxs-lookup"><span data-stu-id="adce0-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="adce0-124">Die `DataType` Attribut ausgibt HTML 5 `data-` (ausgesprochen als Daten Dash) Attribute, die HTML 5-Browser zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="adce0-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="adce0-125">Die `DataType` Attribute stellen keine Validierung.</span><span class="sxs-lookup"><span data-stu-id="adce0-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="adce0-126">`DataType.Date`das Format des Datums, das angezeigt wird, nicht angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="adce0-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="adce0-127">Standardmäßig wird die Date-Felds gemäß der Standardformate basierend auf dem Server angezeigt [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span><span class="sxs-lookup"><span data-stu-id="adce0-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="adce0-128">Das `DisplayFormat`-Attribut dient zum expliziten Angeben des Datumsformats:</span><span class="sxs-lookup"><span data-stu-id="adce0-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="adce0-129">Die `ApplyFormatInEditMode` Einstellung gibt an, dass die Formatierung auf die Bearbeitungsoberfläche auch angewendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="adce0-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="adce0-130">Einige Felder verwenden keine `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="adce0-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="adce0-131">Beispielsweise sollte das Währungssymbol im Allgemeinen nicht in einem Text-Bearbeitungsfeld angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="adce0-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="adce0-132">Die `DisplayFormat` -Attribut allein verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="adce0-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="adce0-133">Es wird im Allgemeinen empfiehlt sich, verwenden Sie die `DataType` -Attribut mit dem `DisplayFormat` Attribut.</span><span class="sxs-lookup"><span data-stu-id="adce0-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="adce0-134">Die `DataType` Attribut übermittelt, die die Semantik der Daten im Gegensatz zu wie auf dem Bildschirm gerendert werden soll.</span><span class="sxs-lookup"><span data-stu-id="adce0-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="adce0-135">Die `DataType` Attribut bietet die folgenden Vorteile, die in nicht verfügbar sind `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="adce0-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="adce0-136">Browser kann HTML5-Funktionen aktivieren.</span><span class="sxs-lookup"><span data-stu-id="adce0-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="adce0-137">Zeigen Sie z. B. ein Monatskalender-Steuerelement, das Gebietsschema entsprechende Währungssymbol, Links per e-Mail, die clientseitige Validierung von Benutzereingaben, usw. ein.</span><span class="sxs-lookup"><span data-stu-id="adce0-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="adce0-138">Standardmäßig wird der Browser Daten im richtigen Format basierend auf dem Gebietsschema gerendert.</span><span class="sxs-lookup"><span data-stu-id="adce0-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="adce0-139">Weitere Informationen finden Sie unter der [ \<input >-Tag-Helper-Dokumentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="adce0-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="adce0-140">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="adce0-140">Run the app.</span></span> <span data-ttu-id="adce0-141">Navigieren Sie zu der Studenten Indexseite.</span><span class="sxs-lookup"><span data-stu-id="adce0-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="adce0-142">Wie oft werden nicht mehr angezeigt.</span><span class="sxs-lookup"><span data-stu-id="adce0-142">Times are no longer displayed.</span></span> <span data-ttu-id="adce0-143">Jede Ansicht, verwendet die `Student` Modell zeigt das Datum ohne Uhrzeit an.</span><span class="sxs-lookup"><span data-stu-id="adce0-143">Every view that uses the `Student` model displays the date without time.</span></span>

![Studenten Indexseite mit Datumsangaben ohne Zeiten](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="adce0-145">Das StringLength-Attribut</span><span class="sxs-lookup"><span data-stu-id="adce0-145">The StringLength attribute</span></span>

<span data-ttu-id="adce0-146">Regeln für die Überprüfung von Daten und Überprüfungsfehlermeldungen können Attribute angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="adce0-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="adce0-147">Die [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) Attribut gibt an, die minimalen und maximalen Länge von Zeichen, die in einem Datenfeld zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="adce0-147">The [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="adce0-148">Die `StringLength` Attribut bietet auch eine clientseitige und serverseitige Validierung.</span><span class="sxs-lookup"><span data-stu-id="adce0-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="adce0-149">Der minimale Wert hat keine Auswirkungen auf das Datenbankschema.</span><span class="sxs-lookup"><span data-stu-id="adce0-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="adce0-150">Update der `Student` Modell mit den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="adce0-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="adce0-151">Der vorangehende Code beschränkt die Objektnamen nicht mehr als 50 Zeichen.</span><span class="sxs-lookup"><span data-stu-id="adce0-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="adce0-152">Die `StringLength` Attribut nicht verhindern, dass einen Benutzer Leerzeichen für einen Namen eingeben.</span><span class="sxs-lookup"><span data-stu-id="adce0-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="adce0-153">Die [der reguläre Ausdruck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) Attribut wird verwendet, um Einschränkungen auf die Eingabe anwenden.</span><span class="sxs-lookup"><span data-stu-id="adce0-153">The [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="adce0-154">Beispielsweise erfordert, dass der folgende Code das erste Zeichen Großbuchstaben bestehen muss und die übrigen Zeichen ein Buchstabe sein:</span><span class="sxs-lookup"><span data-stu-id="adce0-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="adce0-155">Führen Sie die app ein:</span><span class="sxs-lookup"><span data-stu-id="adce0-155">Run the app:</span></span>

* <span data-ttu-id="adce0-156">Navigieren Sie zur Seite "Students".</span><span class="sxs-lookup"><span data-stu-id="adce0-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="adce0-157">Wählen Sie **neu erstellen**, und geben Sie einen Namen, die mehr als 50 Zeichen.</span><span class="sxs-lookup"><span data-stu-id="adce0-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="adce0-158">Wählen Sie **erstellen**, clientseitige Validierung zeigt eine Fehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="adce0-158">Select **Create**, client-side validation shows an error message.</span></span>

![Studenten Indexseite mit Zeichenfolge LÄNGENFEHLER](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="adce0-160">In **Objekt-Explorer von SQL Server** (SSOX), öffnen Sie die Student-Tabellen-Designer durch Doppelklicken auf die **Student** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="adce0-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Students-Tabelle in SSOX vor der Migration](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="adce0-162">Die vorhergehende Abbildung zeigt das Schema für die `Student` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="adce0-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="adce0-163">Die Namensfelder aufweisen `nvarchar(MAX)` da Migrationen auf die Datenbank nicht ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="adce0-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="adce0-164">Bei der Ausführung von Migrationen weiter unten in diesem Lernprogramm werden die Namensfelder werden `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="adce0-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="adce0-165">Das Spaltenattribut</span><span class="sxs-lookup"><span data-stu-id="adce0-165">The Column attribute</span></span>

<span data-ttu-id="adce0-166">Attribute können steuern, wie Klassen und Eigenschaften in der Datenbank zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="adce0-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="adce0-167">In diesem Abschnitt die `Column` Attribut wird verwendet, um den Namen der Zuordnung der `FirstMidName` -Eigenschaft auf "FirstName" in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="adce0-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="adce0-168">Wenn die Datenbank erstellt wird, werden Eigenschaftennamen für das Modell für Spaltennamen verwendet (außer bei der `Column` Attribut verwendet wird).</span><span class="sxs-lookup"><span data-stu-id="adce0-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="adce0-169">Die `Student` modellieren verwendet `FirstMidName` für den Vornamen-Feld verwendet wird, weil das Feld kann auch ein zweiter Vorname enthalten.</span><span class="sxs-lookup"><span data-stu-id="adce0-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="adce0-170">Update der *Student.cs* -Datei mit den folgenden hervorgehobenen Code:</span><span class="sxs-lookup"><span data-stu-id="adce0-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="adce0-171">Mit der vorherigen Änderung `Student.FirstMidName` in der app ordnet die `FirstName` Spalte die `Student` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="adce0-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="adce0-172">Das Hinzufügen der `Column` Attribut ändert sich das Modell Sichern der `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="adce0-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="adce0-173">Sicherung des Modells die `SchoolContext` nicht mehr mit die Datenbank überein.</span><span class="sxs-lookup"><span data-stu-id="adce0-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="adce0-174">Wenn die Anwendung vor dem Anwenden von Migrationen ausgeführt wird, wird die folgende Ausnahme generiert:</span><span class="sxs-lookup"><span data-stu-id="adce0-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```
<span data-ttu-id="adce0-175">So aktualisieren Sie die Datenbank:</span><span class="sxs-lookup"><span data-stu-id="adce0-175">To update the DB:</span></span>

* <span data-ttu-id="adce0-176">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="adce0-176">Build the project.</span></span>
* <span data-ttu-id="adce0-177">Öffnen Sie ein Befehlsfenster in den Projektordner ein.</span><span class="sxs-lookup"><span data-stu-id="adce0-177">Open a command window in the project folder.</span></span> <span data-ttu-id="adce0-178">Geben Sie die folgenden Befehle erstellen eine neue Migration und aktualisieren die Datenbank aus:</span><span class="sxs-lookup"><span data-stu-id="adce0-178">Enter the following commands to create a new migration and update the DB:</span></span>

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

<span data-ttu-id="adce0-179">Die `dotnet ef migrations add ColumnFirstName` Befehl wird die folgende Warnmeldung generiert:</span><span class="sxs-lookup"><span data-stu-id="adce0-179">The `dotnet ef migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="adce0-180">Die Warnung wird generiert, da nun die Felder sind auf 50 Zeichen begrenzt.</span><span class="sxs-lookup"><span data-stu-id="adce0-180">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="adce0-181">Wenn Sie einen Namen in der Datenbank mehr als 50 Zeichen hätte, wäre die 51 bis zum letzten Zeichen verloren gehen.</span><span class="sxs-lookup"><span data-stu-id="adce0-181">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="adce0-182">Testen der app an.</span><span class="sxs-lookup"><span data-stu-id="adce0-182">Test the app.</span></span>

<span data-ttu-id="adce0-183">Öffnen Sie die Student-Tabelle in SSOX:</span><span class="sxs-lookup"><span data-stu-id="adce0-183">Open the Student table in SSOX:</span></span>

![Students-Tabelle in SSOX nach der Migration](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="adce0-185">Vor der Migration angewendet wurde, wurden die Spalten des Typs [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="adce0-185">Before migration was applied, the name columns were of type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="adce0-186">Die Namensspalten sind jetzt `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="adce0-186">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="adce0-187">Name der Spalte geändert hat `FirstMidName` auf `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="adce0-187">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="adce0-188">Im folgenden Abschnitt erstellen die app auf einigen Stufen werden Compilerfehler generiert.</span><span class="sxs-lookup"><span data-stu-id="adce0-188">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="adce0-189">Die Anweisungen geben, wann die app zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="adce0-189">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="adce0-190">Entitätsaktualisierung für Studenten</span><span class="sxs-lookup"><span data-stu-id="adce0-190">Student entity update</span></span>

![Student-Entität](complex-data-model/_static/student-entity.png)

<span data-ttu-id="adce0-192">Update *Models/Student.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="adce0-192">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="adce0-193">Das erforderliche Attribut</span><span class="sxs-lookup"><span data-stu-id="adce0-193">The Required attribute</span></span>

<span data-ttu-id="adce0-194">Die `Required` Attribut macht die erforderlichen Felder des Name-Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="adce0-194">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="adce0-195">Die `Required` Attribut ist nicht erforderlich, für NULL-Typen wie Werttypen (`DateTime`, `int`, `double`usw..).</span><span class="sxs-lookup"><span data-stu-id="adce0-195">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="adce0-196">Typen, die nicht null sein können, werden automatisch als Pflichtfelder behandelt.</span><span class="sxs-lookup"><span data-stu-id="adce0-196">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="adce0-197">Die `Required` Attribut konnten ersetzt werden, mit einem minimum Length-Parameter in der `StringLength` Attribut:</span><span class="sxs-lookup"><span data-stu-id="adce0-197">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="adce0-198">Das Attribut anzeigen</span><span class="sxs-lookup"><span data-stu-id="adce0-198">The Display attribute</span></span>

<span data-ttu-id="adce0-199">Die `Display` Attribut gibt an, dass die Beschriftung für die Textfelder werden soll, "Vorname", "Last Name", "Full Name" und "Registrierungsdatum".</span><span class="sxs-lookup"><span data-stu-id="adce0-199">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="adce0-200">Die Standard-Beschriftungen hatte kein Leerzeichen, unterteilen die Wörter ein, z. B. "Lastname".</span><span class="sxs-lookup"><span data-stu-id="adce0-200">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="adce0-201">Die berechnete FullName-Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="adce0-201">The FullName calculated property</span></span>

<span data-ttu-id="adce0-202">`FullName`ist eine berechnete Eigenschaft, die einen Wert zurückgibt, der durch Verketten von zwei weitere Eigenschaften erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="adce0-202">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="adce0-203">`FullName`nicht festgelegt ist, wird es nur einen Get-Accessor verfügt.</span><span class="sxs-lookup"><span data-stu-id="adce0-203">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="adce0-204">Nicht `FullName` Spalte in der Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="adce0-204">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="adce0-205">Die Instructor-Entität erstellen</span><span class="sxs-lookup"><span data-stu-id="adce0-205">Create the Instructor Entity</span></span>

![Instructor-Entität](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="adce0-207">Erstellen Sie *Models/Instructor.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="adce0-207">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="adce0-208">Beachten Sie, dass mehrere Eigenschaften in werden der `Student` und `Instructor` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="adce0-208">Notice that several properties are the same in the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="adce0-209">In diesem Lernprogramm Implementieren von Vererbung weiter unten in dieser Serie wird dieser Code umgestaltet, um Redundanzen zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="adce0-209">In the Implementing Inheritance tutorial later in this series, this code is refactored to eliminate the redundancy.</span></span>

<span data-ttu-id="adce0-210">Mehrere Attribute können in einer Zeile sein.</span><span class="sxs-lookup"><span data-stu-id="adce0-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="adce0-211">Die `HireDate` Attribute wie folgt geschrieben werden konnte:</span><span class="sxs-lookup"><span data-stu-id="adce0-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="adce0-212">Die Navigationseigenschaften CourseAssignments und OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="adce0-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="adce0-213">Die `CourseAssignments` und `OfficeAssignment` Eigenschaften sind Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="adce0-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="adce0-214">Ein Kursleiter kann eine beliebige Anzahl von Kurse werden folgende Themen behandelt, sodass `CourseAssignments` als eine Auflistung definiert ist.</span><span class="sxs-lookup"><span data-stu-id="adce0-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="adce0-215">Wenn eine Navigationseigenschaft über mehrere Entitäten enthält:</span><span class="sxs-lookup"><span data-stu-id="adce0-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="adce0-216">Es muss ein Listentyp, in dem die Einträge hinzugefügt, gelöscht und aktualisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="adce0-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="adce0-217">Navigation gehören:</span><span class="sxs-lookup"><span data-stu-id="adce0-217">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="adce0-218">Wenn `ICollection<T>` angegeben ist, erstellt von EF Core eine `HashSet<T>` Auflistung standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="adce0-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="adce0-219">Die `CourseAssignment` Entität wird im Abschnitt für die m: n-Beziehungen erläutert.</span><span class="sxs-lookup"><span data-stu-id="adce0-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="adce0-220">Contoso University Geschäftsregeln Zustand, der ein Kursleiter höchstens eine Niederlassung aufweisen kann.</span><span class="sxs-lookup"><span data-stu-id="adce0-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="adce0-221">Die `OfficeAssignment` Eigenschaft enthält eine einzelne `OfficeAssignment` Entität.</span><span class="sxs-lookup"><span data-stu-id="adce0-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="adce0-222">`OfficeAssignment`ist null, wenn keine Office zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="adce0-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="adce0-223">Erstellen Sie die Entität OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="adce0-223">Create the OfficeAssignment entity</span></span>

![OfficeAssignment-Entität](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="adce0-225">Erstellen Sie *Models/OfficeAssignment.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="adce0-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="adce0-226">Das Key-Attribut</span><span class="sxs-lookup"><span data-stu-id="adce0-226">The Key attribute</span></span>

<span data-ttu-id="adce0-227">Die `[Key]` Attribut dient zum Identifizieren einer Eigenschaft als Primärschlüssel (PS) Wenn der Eigenschaftsname etwas ist als ClassnameID oder -ID.</span><span class="sxs-lookup"><span data-stu-id="adce0-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="adce0-228">Es ist eine auf 0 (null) oder 1 eine Beziehung zwischen der `Instructor` und `OfficeAssignment` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="adce0-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="adce0-229">Eine bürozuweisung existiert nur in Bezug auf den Dozenten, dem sie zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="adce0-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="adce0-230">Die `OfficeAssignment` PK entspricht auch der Fremdschlüssel (FS) die `Instructor` Entität.</span><span class="sxs-lookup"><span data-stu-id="adce0-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="adce0-231">EF Core nicht automatisch erkannt. `InstructorID` als der Primärschlüssel der `OfficeAssignment` da:</span><span class="sxs-lookup"><span data-stu-id="adce0-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="adce0-232">`InstructorID`nicht folgen die ID oder ClassnameID-Benennungskonvention.</span><span class="sxs-lookup"><span data-stu-id="adce0-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="adce0-233">Aus diesem Grund die `Key` Attribut wird verwendet, um zu identifizieren `InstructorID` als der PK:</span><span class="sxs-lookup"><span data-stu-id="adce0-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="adce0-234">Standardmäßig behandelt EF Core den Schlüssel als nicht-datenbankgeneriert, da die Spalte für eine identifizierende Beziehung ist.</span><span class="sxs-lookup"><span data-stu-id="adce0-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="adce0-235">Die Navigationseigenschaft für Dozenten</span><span class="sxs-lookup"><span data-stu-id="adce0-235">The Instructor navigation property</span></span>

<span data-ttu-id="adce0-236">Die `OfficeAssignment` Navigationseigenschaft für die `Instructor` Entität ist auf NULL festlegbar da:</span><span class="sxs-lookup"><span data-stu-id="adce0-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="adce0-237">Referenztypen (z. B. Klassen NULL sind).</span><span class="sxs-lookup"><span data-stu-id="adce0-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="adce0-238">Ein Kursleiter wurden möglicherweise nicht über eine bürozuweisung.</span><span class="sxs-lookup"><span data-stu-id="adce0-238">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="adce0-239">Die `OfficeAssignment` Entität hat eine NULL- `Instructor` Navigationseigenschaft da:</span><span class="sxs-lookup"><span data-stu-id="adce0-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="adce0-240">`InstructorID`ist NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="adce0-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="adce0-241">Eine bürozuweisung kann ohne einen Kursleiter vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="adce0-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="adce0-242">Wenn ein `Instructor` Entität besitzt einen zugehörigen `OfficeAssignment` Entität, jede Entität enthält einen Verweis auf die andere in der Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="adce0-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="adce0-243">Die `[Required]` Attribut konnte angewendet werden, um die `Instructor` Navigationseigenschaft:</span><span class="sxs-lookup"><span data-stu-id="adce0-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="adce0-244">Der vorangehende Code gibt an, dass es muss eine verwandte Dozenten.</span><span class="sxs-lookup"><span data-stu-id="adce0-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="adce0-245">Der vorhergehende Code ist nicht erforderlich, da die `InstructorID` Fremdschlüssel (also auch die PK) ist NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="adce0-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="adce0-246">Ändern Sie die Kurs-Entität</span><span class="sxs-lookup"><span data-stu-id="adce0-246">Modify the Course Entity</span></span>

![Kurs-Entität](complex-data-model/_static/course-entity.png)

<span data-ttu-id="adce0-248">Update *Models/Course.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="adce0-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="adce0-249">Die `Course` Entität verfügt über eine Eigenschaft Fremdschlüssel (FS) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="adce0-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="adce0-250">`DepartmentID`verweist auf den zugehörigen `Department` Entität.</span><span class="sxs-lookup"><span data-stu-id="adce0-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="adce0-251">Die `Course` Entität verfügt über eine `Department` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="adce0-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="adce0-252">EF Core erfordert eine FK-Eigenschaft für ein Datenmodell, wenn das Modell eine Navigationseigenschaft für eine verwandte Entität ist.</span><span class="sxs-lookup"><span data-stu-id="adce0-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="adce0-253">EF Core erstellt FKs automatisch in der Datenbank, wo sie benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="adce0-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="adce0-254">EF Core erstellt [Shadowing von Eigenschaften](https://docs.microsoft.com/ef/core/modeling/shadow-properties) für automatisch erstellte FKs.</span><span class="sxs-lookup"><span data-stu-id="adce0-254">EF Core creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="adce0-255">Mit der FK im Datenmodell kann Updates einfacher und effizienter vornehmen.</span><span class="sxs-lookup"><span data-stu-id="adce0-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="adce0-256">Betrachten Sie beispielsweise ein Modell, in dem die Eigenschaft FK `DepartmentID` ist *nicht* enthalten.</span><span class="sxs-lookup"><span data-stu-id="adce0-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="adce0-257">Wenn eine Entität Kurs abgerufen wird, um zu bearbeiten:</span><span class="sxs-lookup"><span data-stu-id="adce0-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="adce0-258">Die `Department` Entität ist null, wenn sie nicht explizit geladen wird.</span><span class="sxs-lookup"><span data-stu-id="adce0-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="adce0-259">Zum Aktualisieren der Entität Kurs der `Department` Entität muss zuerst abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="adce0-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="adce0-260">Wenn die FK-Eigenschaft `DepartmentID` enthalten ist im Datenmodell, besteht keine Notwendigkeit zum Abrufen der `Department` Entität vor einem Update.</span><span class="sxs-lookup"><span data-stu-id="adce0-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="adce0-261">Das DatabaseGenerated-Attribut</span><span class="sxs-lookup"><span data-stu-id="adce0-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="adce0-262">Die `[DatabaseGenerated(DatabaseGeneratedOption.None)]` Attribut gibt an, dass der Primärschlüssel von der Anwendung bereitgestellt, sondern wird von der Datenbank generiert.</span><span class="sxs-lookup"><span data-stu-id="adce0-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="adce0-263">Standardmäßig nimmt EF Core PK Werte von der Datenbank generiert werden.</span><span class="sxs-lookup"><span data-stu-id="adce0-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="adce0-264">DB generiert PK Werte ist in der Regel die beste Herangehensweise.</span><span class="sxs-lookup"><span data-stu-id="adce0-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="adce0-265">Für `Course` Entitäten, die der Benutzer gibt die PK.</span><span class="sxs-lookup"><span data-stu-id="adce0-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="adce0-266">Z. B. eine Kurs Zahl wie z. B. eine Reihe 1000 für die Math-Abteilung, eine Reihe 2000 für die englischen Abteilung.</span><span class="sxs-lookup"><span data-stu-id="adce0-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="adce0-267">Die `DatabaseGenerated` Attribut kann auch zum Generieren der Standardwerte verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="adce0-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="adce0-268">Beispielsweise kann die Datenbank automatisch ein Datumsfeld so, dass das Datum aufgezeichnet wird, das eine Zeile erstellt oder aktualisiert wurde, generieren.</span><span class="sxs-lookup"><span data-stu-id="adce0-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="adce0-269">Weitere Informationen finden Sie unter [Eigenschaften generiert](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="adce0-269">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="adce0-270">Foreign Key- und Navigation-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="adce0-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="adce0-271">Der Fremdschlüssel (FS) Eigenschaften und Navigationseigenschaften in den `Course` Entität wider, die folgenden Beziehungen:</span><span class="sxs-lookup"><span data-stu-id="adce0-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="adce0-272">Ein Kurs wird an eine Abteilung zugewiesen, daher besteht eine `DepartmentID` FK und ein `Department` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="adce0-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="adce0-273">Ein Kurs kann eine beliebige Anzahl von Lernende, haben also die `Enrollments` Navigationseigenschaft ist eine Sammlung:</span><span class="sxs-lookup"><span data-stu-id="adce0-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="adce0-274">Ein Kurs kann von mehreren Lehrkräfte vermittelten werden daher die `CourseAssignments` Navigationseigenschaft ist eine Sammlung:</span><span class="sxs-lookup"><span data-stu-id="adce0-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="adce0-275">`CourseAssignment`erläutert [später](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="adce0-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="adce0-276">Erstellen Sie die Entität Department</span><span class="sxs-lookup"><span data-stu-id="adce0-276">Create the Department entity</span></span>

![Entität Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="adce0-278">Erstellen Sie *Models/Department.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="adce0-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="adce0-279">Das Spaltenattribut</span><span class="sxs-lookup"><span data-stu-id="adce0-279">The Column attribute</span></span>

<span data-ttu-id="adce0-280">Zuvor die `Column` Attribut wurde verwendet, um die Zuordnung für die Namen von Spalten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="adce0-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="adce0-281">Im Code für die `Department` Entität, die `Column` Attribut wird verwendet, um die SQL-datentypzuordnung zu ändern.</span><span class="sxs-lookup"><span data-stu-id="adce0-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="adce0-282">Die `Budget` Spalte wird mit den SQL Server-Money-Datentyp in der DB definiert:</span><span class="sxs-lookup"><span data-stu-id="adce0-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="adce0-283">Die Zuordnung ist im Allgemeinen nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="adce0-283">Column mapping is generally not required.</span></span> <span data-ttu-id="adce0-284">EF Core wählt im Allgemeinen die entsprechenden SQL Server-Datentyp, der basierend auf den CLR-Typ für die Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="adce0-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="adce0-285">Die CLR `decimal` wird zugeordnet zu SQL Server `decimal` Typ.</span><span class="sxs-lookup"><span data-stu-id="adce0-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="adce0-286">`Budget`ist für die Währung, und der Money-Datentyp eignet sich besser für die Währung.</span><span class="sxs-lookup"><span data-stu-id="adce0-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="adce0-287">Foreign Key- und Navigation-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="adce0-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="adce0-288">Die folgenden Beziehungen entsprechen den FK und Navigation-Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="adce0-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="adce0-289">Eine Abteilung möglicherweise nicht haben oder einen Administrator.</span><span class="sxs-lookup"><span data-stu-id="adce0-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="adce0-290">Ein Administrator ist immer einen Kursleiter.</span><span class="sxs-lookup"><span data-stu-id="adce0-290">An administrator is always an instructor.</span></span> <span data-ttu-id="adce0-291">Aus diesem Grund die `InstructorID` Eigenschaft ist als die FK, enthalten die `Instructor` Entität.</span><span class="sxs-lookup"><span data-stu-id="adce0-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="adce0-292">Die Navigationseigenschaft heißt `Administrator` jedoch enthält ein `Instructor` Entität:</span><span class="sxs-lookup"><span data-stu-id="adce0-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="adce0-293">Das Fragezeichen (?) im vorangehenden Code gibt an, dass die Eigenschaft auf NULL festlegbar ist.</span><span class="sxs-lookup"><span data-stu-id="adce0-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="adce0-294">Eine Abteilung möglicherweise viele Kurse, daher eine Navigationseigenschaft Kurse besteht:</span><span class="sxs-lookup"><span data-stu-id="adce0-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="adce0-295">Hinweis: EF Core können gemäß der Konvention kaskadierendes Delete für NULL--FKS. und für viele-zu-viele-Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="adce0-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="adce0-296">Löschweitergabe kann zirkuläre Cascade Delete Regeln führen.</span><span class="sxs-lookup"><span data-stu-id="adce0-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="adce0-297">Zirkuläre kaskadierte Löschung Regeln verursacht eine Ausnahme, wenn eine Migration hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="adce0-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="adce0-298">Beispielsweise, wenn die `Department.InstructorID` Eigenschaft wurde nicht als auf NULL festlegbar definiert:</span><span class="sxs-lookup"><span data-stu-id="adce0-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="adce0-299">EF Core konfiguriert eine Cascade Delete-Regel klicken, um den Dozenten zu löschen, wenn die Abteilung gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="adce0-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="adce0-300">Den Dozenten löschen, wenn die Abteilung gelöscht wird, ist das beabsichtigte Verhalten nicht.</span><span class="sxs-lookup"><span data-stu-id="adce0-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="adce0-301">Bei Bedarf von Geschäftsregeln die `InstructorID` Eigenschaft werden keine NULL-Werte zulässt, verwenden Sie die folgenden fluent-API-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="adce0-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="adce0-302">Der vorangehende Code deaktiviert kaskadierte Löschung auf der Abteilung Instructor-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="adce0-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="adce0-303">Aktualisieren Sie die Registrierung-Entität</span><span class="sxs-lookup"><span data-stu-id="adce0-303">Update the Enrollment entity</span></span>

<span data-ttu-id="adce0-304">Ein Anmeldungsdatensatz ist für einen einen Kurs von einem Studenten ausgeführte.</span><span class="sxs-lookup"><span data-stu-id="adce0-304">An enrollment record is for a one course taken by one student.</span></span>

![Registrierung-Entität](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="adce0-306">Update *Models/Enrollment.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="adce0-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="adce0-307">Foreign Key- und Navigation-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="adce0-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="adce0-308">Die FK-Eigenschaften und Navigationseigenschaften reflektieren die folgenden Beziehungen:</span><span class="sxs-lookup"><span data-stu-id="adce0-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="adce0-309">Ein Anmeldungsdatensatz ist für einen Kurs, es ist also eine `CourseID` FK-Eigenschaft und eine `Course` Navigationseigenschaft:</span><span class="sxs-lookup"><span data-stu-id="adce0-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="adce0-310">Ein Anmeldungsdatensatz ist für ein Student, es ist also eine `StudentID` FK-Eigenschaft und eine `Student` Navigationseigenschaft:</span><span class="sxs-lookup"><span data-stu-id="adce0-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="adce0-311">n:n-Beziehungen</span><span class="sxs-lookup"><span data-stu-id="adce0-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="adce0-312">Es ist eine m: n-Beziehung zwischen der `Student` und `Course` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="adce0-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="adce0-313">Die `Enrollment` Entität als Jointabelle m: n-Funktionen *mit Nutzlast* in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="adce0-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="adce0-314">"Mit der Nutzlast" bedeutet, dass die `Enrollment` Tabelle enthält zusätzliche Daten außer FKs für den verknüpften Tabellen (in diesem Fall wird der Primärschlüssel und `Grade`).</span><span class="sxs-lookup"><span data-stu-id="adce0-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="adce0-315">Die folgende Abbildung zeigt, wie diese Beziehungen in einem Entitätsdiagramm aussieht.</span><span class="sxs-lookup"><span data-stu-id="adce0-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="adce0-316">(Dieses Diagramm generiert wurde, mithilfe von EF Power Tools für EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="adce0-316">(This diagram was generated using EF Power Tools for EF 6.x.</span></span> <span data-ttu-id="adce0-317">Erstellen das Diagramm ist nicht Teil des Lernprogramms.)</span><span class="sxs-lookup"><span data-stu-id="adce0-317">Creating the diagram isn't part of the tutorial.)</span></span>

![Student-Kurs m: n-Beziehung](complex-data-model/_static/student-course.png)

<span data-ttu-id="adce0-319">Jede Beziehungslinie hat eine 1 an einem Ende und ein Sternchen (\*) am anderen Eintrag, der angibt, einer 1: n-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="adce0-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="adce0-320">Wenn die `Enrollment` Tabelle haben nicht Grade Informationen enthalten, er dürfte nur die zwei-FKS. enthalten (`CourseID` und `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="adce0-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="adce0-321">Eine m: n-Jointabelle ohne Nutzlast ist eine reine Jointabelle (PJT) bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="adce0-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="adce0-322">Die `Instructor` und `Course` Entitäten verfügen über eine m: n-Beziehung mit einer reinen Jointabelle.</span><span class="sxs-lookup"><span data-stu-id="adce0-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="adce0-323">Hinweis: EF 6.x unterstützt keine implizite Verknüpfung von Tabellen für m: n-Beziehungen, aber EF Core.</span><span class="sxs-lookup"><span data-stu-id="adce0-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="adce0-324">Weitere Informationen finden Sie unter [m: n-Beziehungen in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="adce0-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="adce0-325">Die CourseAssignment-Entität</span><span class="sxs-lookup"><span data-stu-id="adce0-325">The CourseAssignment entity</span></span>

![CourseAssignment-Entität](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="adce0-327">Erstellen Sie *Models/CourseAssignment.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="adce0-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="adce0-328">Instructor-zu-Kurse</span><span class="sxs-lookup"><span data-stu-id="adce0-328">Instructor-to-Courses</span></span>

![Instructor-Kurse-m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="adce0-330">Die Instructor-Kurse-viele-zu-viele-Beziehung:</span><span class="sxs-lookup"><span data-stu-id="adce0-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="adce0-331">Erfordert eine Jointabelle, die durch eine Entitätenmenge dargestellt werden muss.</span><span class="sxs-lookup"><span data-stu-id="adce0-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="adce0-332">Ist eine reine Jointabelle (Tabelle ohne Nutzlast).</span><span class="sxs-lookup"><span data-stu-id="adce0-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="adce0-333">Es ist üblich, benennen Sie eine Joinentität `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="adce0-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="adce0-334">Die Instructor-Kurse-Join-Tabelle, die mithilfe des folgenden Musters ist z. B. `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="adce0-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="adce0-335">Allerdings wird empfohlen, unter Verwendung eines Namens, der die Beziehung beschreibt.</span><span class="sxs-lookup"><span data-stu-id="adce0-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="adce0-336">Datenmodelle fangen einfache und vergrößert.</span><span class="sxs-lookup"><span data-stu-id="adce0-336">Data models start out simple and grow.</span></span> <span data-ttu-id="adce0-337">No-Nutzlast Joins (PJTs) weiterentwickelt häufig um Nutzlast enthalten.</span><span class="sxs-lookup"><span data-stu-id="adce0-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="adce0-338">Beginnen Sie mit einem aussagekräftigen Entitätsname, muss der Namen ändern, wenn die verknüpften Tabelle ändert.</span><span class="sxs-lookup"><span data-stu-id="adce0-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="adce0-339">Im Idealfall würden die Joinentität einen eigene natürlichen (möglicherweise einzelne Wort)-Namen in der Business-Domäne haben.</span><span class="sxs-lookup"><span data-stu-id="adce0-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="adce0-340">Beispielsweise konnte Büchern und Kunden mit einem Joinentität mit dem Namen Bewertungen verknüpft werden.</span><span class="sxs-lookup"><span data-stu-id="adce0-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="adce0-341">Für die Instructor-Kurse-viele-zu-viele-Beziehung `CourseAssignment` ist die bevorzugte über `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="adce0-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="adce0-342">Zusammengesetzter Schlüssel</span><span class="sxs-lookup"><span data-stu-id="adce0-342">Composite key</span></span>

<span data-ttu-id="adce0-343">FKs sind keine NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="adce0-343">FKs are not nullable.</span></span> <span data-ttu-id="adce0-344">Die beiden-FKS. in `CourseAssignment` (`InstructorID` und `CourseID`) zusammen eindeutig identifizieren jede Zeile der `CourseAssignment` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="adce0-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="adce0-345">`CourseAssignment`erfordert eine dedizierte PK.</span><span class="sxs-lookup"><span data-stu-id="adce0-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="adce0-346">Die `InstructorID` und `CourseID` Eigenschaften Funktion, wie eine zusammengesetzte PK.</span><span class="sxs-lookup"><span data-stu-id="adce0-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="adce0-347">Die einzige Möglichkeit, geben Sie zusammengesetzte PKs EF-Kern ist mit der *fluent-API*.</span><span class="sxs-lookup"><span data-stu-id="adce0-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="adce0-348">Im nächste Abschnitt wird gezeigt, wie so konfigurieren Sie die zusammengesetzte PK.</span><span class="sxs-lookup"><span data-stu-id="adce0-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="adce0-349">Des zusammengesetzten Schlüssels wird sichergestellt, dass:</span><span class="sxs-lookup"><span data-stu-id="adce0-349">The composite key ensures:</span></span>

* <span data-ttu-id="adce0-350">Für einen Kurs sind mehrere Zeilen zulässig.</span><span class="sxs-lookup"><span data-stu-id="adce0-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="adce0-351">Für eine Instructor sind mehrere Zeilen zulässig.</span><span class="sxs-lookup"><span data-stu-id="adce0-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="adce0-352">Mehrere Zeilen für die gleiche Dozenten und Kurs ist nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="adce0-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="adce0-353">Die `Enrollment` Joinentität definiert seine eigenen PK Duplikate dieser Art möglich sind.</span><span class="sxs-lookup"><span data-stu-id="adce0-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="adce0-354">So verhindern Sie solche Duplikate:</span><span class="sxs-lookup"><span data-stu-id="adce0-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="adce0-355">Fügen Sie einen eindeutigen Index für die Felder FK oder</span><span class="sxs-lookup"><span data-stu-id="adce0-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="adce0-356">Konfigurieren Sie `Enrollment` mit einem zusammengesetzten Primärschlüssel ähnelt `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="adce0-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="adce0-357">Weitere Informationen finden Sie unter [Indizes](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="adce0-357">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="adce0-358">Aktualisieren Sie den Datenbankkontext</span><span class="sxs-lookup"><span data-stu-id="adce0-358">Update the DB context</span></span>

<span data-ttu-id="adce0-359">Fügen Sie den folgenden hervorgehobenen Code hinzu *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="adce0-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="adce0-360">Der vorangehende Code fügt das neuen Entitäten und konfiguriert die `CourseAssignment` Entität zusammengesetzten PK.</span><span class="sxs-lookup"><span data-stu-id="adce0-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="adce0-361">Fluent-API-Alternative zu Attributen</span><span class="sxs-lookup"><span data-stu-id="adce0-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="adce0-362">Die `OnModelCreating` Methode im vorangehenden code verwendet die *fluent-API* EF Kernverhalten konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="adce0-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="adce0-363">Die API wird "fluent" bezeichnet, da es häufig von einer Reihe von Methodenaufrufen zusammen in einer einzigen Anweisung Vermittlungsstellen verwendet.</span><span class="sxs-lookup"><span data-stu-id="adce0-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="adce0-364">Die [folgenden Code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) ist ein Beispiel für die fluent-API:</span><span class="sxs-lookup"><span data-stu-id="adce0-364">The [following code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="adce0-365">In diesem Lernprogramm wird die fluent-API nur für DB-Zuordnung verwendet, die mit Attributen erfolgen kann.</span><span class="sxs-lookup"><span data-stu-id="adce0-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="adce0-366">Die fluent-API kann jedoch die meisten Formatierung, Überprüfung und Zuordnungsregeln an, die mit Attributen erfolgen angeben.</span><span class="sxs-lookup"><span data-stu-id="adce0-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="adce0-367">Einige Attribute wie z. B. `MinimumLength` kann nicht mit der fluent-API angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="adce0-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="adce0-368">`MinimumLength`Ändern Sie nicht das Schema gilt nur für eine Validierungsregel für die minimale Länge.</span><span class="sxs-lookup"><span data-stu-id="adce0-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="adce0-369">Einige Entwickler bevorzugen die fluent-API ausschließlich, damit sie ihre Entitätsklassen "bereinigen" bleibt</span><span class="sxs-lookup"><span data-stu-id="adce0-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="adce0-370">Attribute und die fluent-API können gemischt werden.</span><span class="sxs-lookup"><span data-stu-id="adce0-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="adce0-371">Es gibt einige Konfigurationen, die nur mit der fluent-API ausgeführt werden können (das Angeben einer zusammengesetzten PK).</span><span class="sxs-lookup"><span data-stu-id="adce0-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="adce0-372">Es gibt einige Konfigurationen, die mit Attributen nur ausgeführt werden können (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="adce0-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="adce0-373">Die empfohlene Vorgehensweise für die Verwendung von fluent-API oder Attribute:</span><span class="sxs-lookup"><span data-stu-id="adce0-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="adce0-374">Wählen Sie eine dieser beiden Ansätze.</span><span class="sxs-lookup"><span data-stu-id="adce0-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="adce0-375">Verwenden Sie die ausgewählten Ansatz einheitlich so weit wie möglich.</span><span class="sxs-lookup"><span data-stu-id="adce0-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="adce0-376">Einige der Attribute in diesem Lernprogramm werden zum:</span><span class="sxs-lookup"><span data-stu-id="adce0-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="adce0-377">Nur Überprüfung (z. B. `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="adce0-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="adce0-378">Nur EF-kernkonfiguration (z. B. `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="adce0-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="adce0-379">Überprüfung und EF-Core-Konfiguration (z. B. `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="adce0-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="adce0-380">Weitere Informationen zu Attributen im Vergleich zu fluent-API finden Sie unter [Methoden der Konfiguration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="adce0-380">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="adce0-381">Anzeigen von Entitätsbeziehungen-Diagramm</span><span class="sxs-lookup"><span data-stu-id="adce0-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="adce0-382">Die folgende Abbildung zeigt das Diagramm, das EF-Powertools für das abgeschlossene Modell "School" zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="adce0-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Entitätsdiagramm](complex-data-model/_static/diagram.png)

<span data-ttu-id="adce0-384">Die vorhergehende Abbildung zeigt:</span><span class="sxs-lookup"><span data-stu-id="adce0-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="adce0-385">1: n-Beziehung Zeilenumbrüche (1 bis \*).</span><span class="sxs-lookup"><span data-stu-id="adce0-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="adce0-386">Zu 0 (null) oder eins eine Beziehungslinie (1 zu 0.. 1) zwischen den `Instructor` und `OfficeAssignment` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="adce0-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="adce0-387">0 (null)-oder-1-n-Beziehung (0.. 1 auf \*) zwischen den `Instructor` und `Department` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="adce0-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="adce0-388">Ausgangswert für die Datenbank mit der Testdaten</span><span class="sxs-lookup"><span data-stu-id="adce0-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="adce0-389">Aktualisieren Sie den Code in *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="adce0-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="adce0-390">Der vorangehende Code stellt Seed-Daten für die neuen Entitäten.</span><span class="sxs-lookup"><span data-stu-id="adce0-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="adce0-391">Die meisten dieser Code erstellt neue Entitätsobjekten und lädt Beispieldaten.</span><span class="sxs-lookup"><span data-stu-id="adce0-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="adce0-392">Die Beispieldaten werden für Tests verwendet.</span><span class="sxs-lookup"><span data-stu-id="adce0-392">The sample data is used for testing.</span></span> <span data-ttu-id="adce0-393">Der vorangehende Code erstellt die folgenden viele-zu-viele-Beziehungen:</span><span class="sxs-lookup"><span data-stu-id="adce0-393">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

<span data-ttu-id="adce0-394">Hinweis: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) unterstützt [datenseeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span><span class="sxs-lookup"><span data-stu-id="adce0-394">Note: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) will support [data seeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="adce0-395">Fügen Sie eine migration</span><span class="sxs-lookup"><span data-stu-id="adce0-395">Add a migration</span></span>

<span data-ttu-id="adce0-396">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="adce0-396">Build the project.</span></span> <span data-ttu-id="adce0-397">Öffnen Sie ein Befehlsfenster, das in den Projektordner, und geben Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="adce0-397">Open a command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="adce0-398">Dieser Befehl zeigt eine Warnung bezüglich eines möglichen Datenverlusts.</span><span class="sxs-lookup"><span data-stu-id="adce0-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="adce0-399">Wenn die `database update` Befehl ausgeführt wird, wird die folgende Fehlermeldung erzeugt:</span><span class="sxs-lookup"><span data-stu-id="adce0-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="adce0-400">Bei der Ausführung von Migrationen mit vorhandenen Daten werden möglicherweise FK-Einschränkungen, die nicht mit den vorhandenen Daten erfüllt werden.</span><span class="sxs-lookup"><span data-stu-id="adce0-400">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="adce0-401">In diesem Lernprogramm wird eine neue Datenbank erstellt, daher gibt es keine FK einschränkungsverletzungen.</span><span class="sxs-lookup"><span data-stu-id="adce0-401">For this tutorial, a new DB is created, so there are no FK constraint violations.</span></span> <span data-ttu-id="adce0-402">Finden Sie unter [korrigieren und foreign Key-Einschränkungen mit Legacydaten](#fk) Anleitungen Verstößen der FK auf der aktuellen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="adce0-402">See [Fixing foreign key constraints with legacy data](#fk) for instructions on how to fix the FK violations on the current DB.</span></span>

## <a name="change-the-connection-string-and-update-the-db"></a><span data-ttu-id="adce0-403">Ändern Sie die Verbindungszeichenfolge und aktualisieren die Datenbank</span><span class="sxs-lookup"><span data-stu-id="adce0-403">Change the connection string and update the DB</span></span>

<span data-ttu-id="adce0-404">Der Code in der aktualisierten `DbInitializer` Seed-Daten für die neue Entitäten hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="adce0-404">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="adce0-405">So erzwingen Sie EF-Core, um eine neue leere Datenbank zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="adce0-405">To force EF Core to create a new empty DB:</span></span>

* <span data-ttu-id="adce0-406">Ändern der Name der Datenbankverbindungszeichenfolge in *appsettings.json* auf ContosoUniversity3.</span><span class="sxs-lookup"><span data-stu-id="adce0-406">Change the DB connection string name in *appsettings.json* to ContosoUniversity3.</span></span> <span data-ttu-id="adce0-407">Der neue Name muss ein Name sein, die auf dem Computer verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="adce0-407">The new name must be a name that hasn't been used on the computer.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* <span data-ttu-id="adce0-408">Löschen Sie alternativ die DB-verwenden:</span><span class="sxs-lookup"><span data-stu-id="adce0-408">Alternatively, delete the DB using:</span></span>

    * <span data-ttu-id="adce0-409">**Objekt-Explorer von SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="adce0-409">**SQL Server Object Explorer** (SSOX).</span></span>
    * <span data-ttu-id="adce0-410">Die `database drop` CLI-Befehl:</span><span class="sxs-lookup"><span data-stu-id="adce0-410">The `database drop` CLI command:</span></span>

   ```console
   dotnet ef database drop
   ```

<span data-ttu-id="adce0-411">Führen Sie `database update` im Befehlsfenster:</span><span class="sxs-lookup"><span data-stu-id="adce0-411">Run `database update` in the command window:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="adce0-412">Dieser Befehl wird bei allen Migrationen ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="adce0-412">The preceding command runs all the migrations.</span></span>

<span data-ttu-id="adce0-413">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="adce0-413">Run the app.</span></span> <span data-ttu-id="adce0-414">Ausführen der app-Ausführung der `DbInitializer.Initialize` Methode.</span><span class="sxs-lookup"><span data-stu-id="adce0-414">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="adce0-415">Die `DbInitializer.Initialize` füllt die neue DB.</span><span class="sxs-lookup"><span data-stu-id="adce0-415">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="adce0-416">Öffnen Sie die Datenbank im SSOX:</span><span class="sxs-lookup"><span data-stu-id="adce0-416">Open the DB in SSOX:</span></span>

* <span data-ttu-id="adce0-417">Erweitern Sie die **Tabellen** Knoten.</span><span class="sxs-lookup"><span data-stu-id="adce0-417">Expand the **Tables** node.</span></span> <span data-ttu-id="adce0-418">Die erstellten Tabellen werden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="adce0-418">The created tables are displayed.</span></span>
* <span data-ttu-id="adce0-419">Wenn zuvor SSOX geöffnet wurde, klicken Sie auf die **aktualisieren** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="adce0-419">If SSOX was opened previously, click the **Refresh** button.</span></span>

![Tabellen in SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="adce0-421">Überprüfen Sie die **CourseAssignment** Tabelle:</span><span class="sxs-lookup"><span data-stu-id="adce0-421">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="adce0-422">Mit der rechten Maustaste die **CourseAssignment** Tabelle, und wählen Sie **Ansichtsdaten**.</span><span class="sxs-lookup"><span data-stu-id="adce0-422">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="adce0-423">Überprüfen Sie die **CourseAssignment** Tabelle Daten enthält.</span><span class="sxs-lookup"><span data-stu-id="adce0-423">Verify the **CourseAssignment** table contains data.</span></span>

![CourseAssignment Daten in SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a><span data-ttu-id="adce0-425">Beheben von foreign Key-Einschränkungen mit älteren Daten</span><span class="sxs-lookup"><span data-stu-id="adce0-425">Fixing foreign key constraints with legacy data</span></span>

<span data-ttu-id="adce0-426">In diesem Abschnitt ist optional.</span><span class="sxs-lookup"><span data-stu-id="adce0-426">This section is optional.</span></span>

<span data-ttu-id="adce0-427">Bei der Ausführung von Migrationen mit vorhandenen Daten werden möglicherweise FK-Einschränkungen, die nicht mit den vorhandenen Daten erfüllt werden.</span><span class="sxs-lookup"><span data-stu-id="adce0-427">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="adce0-428">Mit Produktionsdaten müssen Schritte ausgeführt werden, um die vorhandenen Daten zu migrieren.</span><span class="sxs-lookup"><span data-stu-id="adce0-428">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="adce0-429">Dieser Abschnitt enthält ein Beispiel für FK einschränkungsverletzungen beheben.</span><span class="sxs-lookup"><span data-stu-id="adce0-429">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="adce0-430">Machen Sie keine dieser Änderungen am Code ohne Sicherung.</span><span class="sxs-lookup"><span data-stu-id="adce0-430">Don't make these code changes without a backup.</span></span> <span data-ttu-id="adce0-431">Machen Sie keine dieser Code ändert, wenn Sie im vorherigen Abschnitt abgeschlossen und die Datenbank aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="adce0-431">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="adce0-432">Die *{timestamp}_ComplexDataModel.cs* Datei enthält den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="adce0-432">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="adce0-433">Der vorangehende Code Fügt einen NULL- `DepartmentID` FK auf die `Course` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="adce0-433">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="adce0-434">Die Datenbank aus dem vorherigen Lernprogramm enthält Zeilen in `Course`, sodass dieser Tabelle durch Migrationen aktualisiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="adce0-434">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="adce0-435">Vornehmen der `ComplexDataModel` Migration mit vorhandenen Daten zu arbeiten:</span><span class="sxs-lookup"><span data-stu-id="adce0-435">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="adce0-436">Ändern Sie den Code, um die neue Spalte zu gewähren (`DepartmentID`) einen Standardwert.</span><span class="sxs-lookup"><span data-stu-id="adce0-436">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="adce0-437">Erstellen Sie eine gefälschte Abteilung, die mit dem Namen "Temp" als Standard-Abteilung zu agieren.</span><span class="sxs-lookup"><span data-stu-id="adce0-437">Create a fake department named "Temp" to act as the default department.</span></span>

### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="adce0-438">Korrigieren Sie die foreign Key-Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="adce0-438">Fix the foreign key constraints</span></span>

<span data-ttu-id="adce0-439">Update der `ComplexDataModel` Klassen `Up` Methode:</span><span class="sxs-lookup"><span data-stu-id="adce0-439">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="adce0-440">Öffnen der *{timestamp}_ComplexDataModel.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="adce0-440">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="adce0-441">Kommentieren Sie die Codezeile fügt die `DepartmentID` Spalte die `Course` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="adce0-441">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="adce0-442">Fügen Sie den folgenden hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="adce0-442">Add the following highlighted code.</span></span> <span data-ttu-id="adce0-443">Der neue Code wechselt nach der `.CreateTable( name: "Department"` blockieren:[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="adce0-443">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="adce0-444">Mit den vorhergehenden Änderungen, die vorhandenen `Course` Zeilen werden im Zusammenhang mit der Abteilung "Temp" nach der `ComplexDataModel` `Up` Methode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="adce0-444">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="adce0-445">Eine produktionsanwendung würde:</span><span class="sxs-lookup"><span data-stu-id="adce0-445">A production app would:</span></span>

* <span data-ttu-id="adce0-446">Einschließen von Code oder Skripts hinzufügen `Department` Zeilen und verwandte `Course` Zeilen mit dem neuen `Department` Zeilen.</span><span class="sxs-lookup"><span data-stu-id="adce0-446">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="adce0-447">Verwenden Sie nicht die Abteilung "Temp" oder der Standardwert für `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="adce0-447">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="adce0-448">Das nächste Lernprogramm behandelt die verknüpfte Daten.</span><span class="sxs-lookup"><span data-stu-id="adce0-448">The next tutorial covers related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="adce0-449">[Zurück](xref:data/ef-rp/migrations)
[Weiter](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="adce0-449">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
