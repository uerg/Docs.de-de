---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: ASP.NET Web Pages (Razor), API-Kurzreferenz | Microsoft-Dokumentation
author: tfitzmac
description: Diese Seite enthält eine Liste mit kurze Beispiele für die am häufigsten verwendeten Objekten, Eigenschaften und Methoden für die Programmierung von ASP.NET Web Pages mit Razor-Syntax.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: a10446eb621feb26c485384700ad9980cccf5d8b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827074"
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a>ASP.NET Web Pages (Razor), API-Kurzreferenz
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese Seite enthält eine Liste mit kurze Beispiele für die am häufigsten verwendeten Objekten, Eigenschaften und Methoden für die Programmierung von ASP.NET Web Pages mit Razor-Syntax.
> 
> Beschreibungen, die mit "(v2)" gekennzeichnet, die in ASP.NET Web Pages 2-Version eingeführt wurden.
> 
> API-Referenzdokumentation, finden Sie unter den [ASP.NET Web Pages-Referenzdokumentation](https://go.microsoft.com/fwlink/?LinkId=208659) auf MSDN.
> 
> ## <a name="software-versions"></a>Software-Versionen
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2 und ASP.NET Web Pages-1.0 (mit Ausnahme von Features, die markiert v2).


Diese Seite enthält Referenzinformationen für Folgendes:

- [Klassen](#Classes)
- [Data](#Data)
- [Hilfsmethoden](#Helpers)
- [Validierung](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Klassen

### `AppState[key], AppState[index],App`

Enthält Daten, die von der alle Seiten, die in der Anwendung gemeinsam genutzt werden können. Sie können die dynamische `App` Eigenschaft Zugriff auf die gleichen Daten, wie im folgenden Beispiel gezeigt:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Konvertiert einen Zeichenfolgenwert in einen booleschen Wert (True/False). Der angegebene Wert, wenn die Zeichenfolge nicht "true" / "false" darstellt, oder gibt "false".

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Konvertiert einen Zeichenfolgenwert, der Datum/Uhrzeit. Gibt `DateTime.MinValue` oder den angegebenen Wert, wenn die Zeichenfolge keinen Datums-/Uhrzeitwert darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Konvertiert einen Zeichenfolgenwert in einen Dezimalwert. Gibt 0,0 oder den angegebenen Wert, wenn die Zeichenfolge keinen decimal-Wert darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Konvertiert einen Zeichenfolgenwert in einen Gleitkommawert an. Gibt 0,0 oder den angegebenen Wert, wenn die Zeichenfolge keinen decimal-Wert darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Konvertiert einen Zeichenfolgenwert in eine ganze Zahl. Gibt 0 zurück, oder der angegebene Wert, wenn die Zeichenfolge keine ganze Zahl darstellt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Erstellt einen kompatiblen Browser-URL aus einem lokalen Dateipfad, mit optionalen zusätzlichen Pfad teilen.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Rendert *Wert* als HTML-Markup und nicht als HTML-codierte Ausgabe rendern.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Gibt true zurück, wenn der Wert aus einer Zeichenfolge in den angegebenen Typ konvertiert werden kann.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Gibt "true" zurück, wenn das Objekt oder eine Variable keinen Wert besitzt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Gibt true zurück, wenn die Anforderung einen Beitrag ist. (Erste Anforderungen sind in der Regel GET).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Gibt den Pfad einer Layoutseite zu dieser Seite anwenden.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Enthält Daten, die die Seite, Layoutseiten und Teilseiten gemeinsam, in der aktuellen Anforderung. Sie können die dynamische `Page` Eigenschaft Zugriff auf die gleichen Daten, wie im folgenden Beispiel gezeigt:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Layoutseiten) Rendert den Inhalt der Inhaltsseite, die nicht in eine beliebige benannte Abschnitte enthalten ist.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Rendert eine Inhaltsseite, die mit dem angegebenen Pfad und optional zusätzliche Daten. Erhalten Sie die Werte der zusätzlichen Parameter aus `PageData` durch Position (z. B. 1) oder Schlüssel (z. B. 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Layoutseiten) Rendert einen Content-Abschnitt, der einen Namen aufweist. Legen Sie *erforderlichen* auf "false", um einen Abschnitt optional zu machen.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Ruft ab oder legt den Wert eines HTTP-Cookies.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Ruft ab, die Dateien, die in der aktuellen Anforderung hochgeladen wurden.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Ruft ab, in einem Formular (als Zeichenfolgen) gesendet wurde. `Request[key]` überprüft, ob sowohl der `Request.Form` und `Request.QueryString` Sammlungen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Ruft die Daten, die in der URL-Abfragezeichenfolge angegeben wurde. `Request[key]` überprüft, ob sowohl der `Request.Form` und `Request.QueryString` Sammlungen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Selektiv die Anforderungsvalidierung deaktiviert für ein Formularelement, Abfragezeichenfolgen-Wert, ein Cookie oder Headerwert. Anforderungsvalidierung ist standardmäßig aktiviert und verhindert, dass Benutzer aus der Buchung Markup oder andere potenziell gefährliche Inhalte.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Fügt einen HTTP-Server-Header der Antwort hinzu.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Werden die Seitenausgabe für einen angegebenen Zeitraum zwischengespeichert. Legen Sie optional *gleitend* zum Zurücksetzen des Timeouts für jede Seitenzugriff und *VaryByParams* verschiedene Versionen der Seite für jede andere Abfrage-Zeichenfolge in die Seitenanforderung zwischenzuspeichern.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Leitet den Browseranforderung an einen neuen Speicherort.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Legt fest, an den Browser gesendeten HTTP-Statuscode.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Schreibt den Inhalt der *Daten* der Antwort mit einem optionalen MIME-Typ.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Schreibt den Inhalt einer Datei in die Antwort an.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Layoutseiten) Definiert einen Inhaltsbereich, deren Name.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Decodiert eine Zeichenfolge, die HTML-codiert ist.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Codiert eine Zeichenfolge für das Rendern im HTML-Markup.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Gibt den physischen Pfad der Server für den angegebenen virtuellen Pfad zurück.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Decodiert Text aus einer URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Codiert Text, der in einer URL einfügen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Ruft ab oder legt einen Wert, der vorhanden ist, bis der Benutzer den Browser schließt.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Zeigt eine Zeichenfolgendarstellung für den Wert des Objekts an.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Ruft zusätzliche Daten aus der URL (z. B. */MyPage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Ändert das Kennwort für den angegebenen Benutzer.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Wird ein Konto mit das bestätigungstoken Konto bestätigt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Erstellt ein neues Benutzerkonto mit dem angegebenen Benutzernamen und Kennwort an. Um ein Zugriffstoken für die Bestätigung erforderlich ist, übergeben Sie für "true" *RequireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Ruft den ganzzahligen Bezeichner für den aktuell angemeldeten Benutzer ab.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Ruft den Namen des aktuell angemeldeten Benutzers.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Generiert ein Zurücksetzen des Kennworts-Token, die in e-Mail-Adresse für einem Benutzer gesendet werden kann, damit der Benutzer das Kennwort zurücksetzen kann.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Gibt die Benutzer-ID aus dem Benutzernamen zurück.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Gibt true zurück, wenn der aktuelle Benutzer angemeldet ist.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Gibt "true" zurück, wenn der Benutzer (z. B. über eine Bestätigung per e-Mail) bestätigt wurde.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Gibt True zurück, wenn der aktuelle Benutzername den angegebenen Benutzernamen übereinstimmt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Meldet sich der Benutzer in ein Authentifizierungstoken im Cookie festlegen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Meldet den Benutzer, durch das Entfernen des Authentifizierungscookies token aus.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Wenn der Benutzer nicht authentifiziert ist, legt den HTTP-Status auf 401 (nicht autorisiert) fest.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Wenn der aktuelle Benutzer nicht Mitglied einer der angegebenen Rollen ist, legt den HTTP-Status auf 401 (nicht autorisiert) fest.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Wenn der aktuelle Benutzer nicht von angegebene Benutzer ist *Benutzername*, legt den HTTP-Status auf 401 (nicht autorisiert) fest.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Wenn das kennwortzurücksetzungstoken gültig ist, ändert das Kennwort des Benutzers, für das neue Kennwort.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Daten

### `Database.Execute(SQLstatement [,parameters]`

Führt *SQLstatement* (mit optionalen Parametern) wie z. B. INSERT, DELETE oder UPDATE und die Anzahl der betroffenen Datensätze zurückgibt.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Gibt die Identitätsspalte der zuletzt eingefügten Zeile zurück.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Öffnet entweder die angegebene Datenbankdatei oder die Datenbank angegeben, unter Verwendung einer benannten Verbindungszeichenfolge aus der *"Web.config"* Datei.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Öffnet eine Datenbank mithilfe der Verbindungszeichenfolge. (Dies steht im Gegensatz zu `Database.Open`, die Namen einer Verbindungszeichenfolge verwendet.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Abfragen der Datenbank mit *SQLstatement* (wahlweise können Parameter übergeben) und gibt die Ergebnisse als Auflistung zurück.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Führt *SQLstatement* (mit optionalen Parametern) und gibt einen einzelnen Datensatz zurück.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Führt *SQLstatement* (mit optionalen Parametern) und einen einzelnen Wert zurückgibt.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Hilfsmethoden

### `Analytics.GetGoogleHtml(webPropertyId)`

Rendert den Google Analytics-JavaScript-Code für die angegebene ID.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Rendert den StatCounter Analytics-JavaScript-Code für das angegebene Projekt.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Rendert den Yahoo-Analytics-JavaScript-Code für das angegebene Konto an.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Übergibt eine Suche mit Bing. Um den Standort, zu suchen und einen Titel für das Suchfeld angeben, Sie können festlegen, die `Bing.SiteUrl` und `Bing.SiteTitle` Eigenschaften. Normalerweise legen Sie diese Eigenschaften der  *\_AppStart* Seite.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Initialisiert ein Diagramm an.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Fügt ein Diagramm eine Legende hinzu.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Fügt eine Reihe von Werten für das Diagramm an.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Gibt einen Hash für die angegebenen Daten zurück. Standardmäßig wird der Algorithmus `sha256`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Ermöglicht Benutzern das Herstellen einer Verbindung mit Seiten Facebook.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Rendert Benutzeroberfläche zum Hochladen von Dateien.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Rendert das angegebene Spiele Xbox-Tag.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Rendert das Gravatar-Image für die angegebene e-Mail-Adresse an.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Konvertiert ein Datenobjekt in eine Zeichenfolge im JavaScript Object Notation (JSON) Format.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Konvertiert eine JSON-codierten Zeichenfolge auf ein Objekt, das Sie durchlaufen können, oder fügen Sie in einer Datenbank an.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Rendert die Links zu sozialen Netzwerken mit dem angegebenen Titel und eine optionale URL.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Ordnet eine Fehlermeldung eines Formularfelds. Verwenden der `ModelState` Helper auf diesen Member zuzugreifen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Ordnet eine Fehlermeldung mit einem Formular. Verwenden der `ModelState` Helper auf diesen Member zuzugreifen.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Gibt "true" zurück, wenn keine Validierungsfehler vorhanden sind. Verwenden der `ModelState` Helper auf diesen Member zuzugreifen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Rendert die Eigenschaften und Werte eines Objekts und alle untergeordneten Objekte.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Rendert, der die ReCAPTCHA-Überprüfung.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Legt fest, öffentliche und private Schlüssel für den ReCAPTCHA-Dienst. Normalerweise legen Sie diese Eigenschaften der  *\_AppStart* Seite.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Das Ergebnis des Tests ReCAPTCHA zurückgegeben.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Rendert die Statusinformationen zu ASP.NET Web Pages.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Rendert einen Twitter-Datenstrom für den angegebenen Benutzer.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Rendert einen Twitter-Datenstrom für den angegebenen Suchtext.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Rendert einen Flash-video-Player für die angegebene Datei mit optionalen Breite und Höhe.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Rendert einen Windows Media Player für die angegebene Datei mit optionalen Breite und Höhe.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Rendert eine Silverlight-Players für die angegebene *XAP* -Datei mit erforderliche Breite und Höhe.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Gibt das Objekt anhand des *Schlüssel*, oder null, wenn das Objekt nicht gefunden wird.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Entfernt das Objekt anhand des *Schlüssel* aus dem Cache.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Setzt *Wert* in den Cache unter dem Namen gemäß *Schlüssel*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Erstellt ein neues `WebGrid` -Objekt mithilfe der Daten aus einer Abfrage.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Rendert Markup zum Anzeigen von Daten in eine HTML-Tabelle.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Rendert einen Pager, der für die `WebGrid` Objekt.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Lädt ein Bild aus dem angegebenen Pfad.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Fügt das angegebene Bild als Wasserzeichen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Fügt den angegebenen Text in der Abbildung.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Kippt das Bild, horizontal oder vertikal.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Lädt ein Bild aus, wenn ein Bild auf eine Seite, während eine Datei hoch gesendet wird.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Ändert die Größe ein Bild.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Dreht das Bild nach links oder rechts.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Speichert das Bild in den angegebenen Pfad.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Legt das Kennwort für den SMTP-Server an. Normalerweise legen Sie diese Eigenschaft der  *\_AppStart* Seite.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Sendet eine E-Mail.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Legt den Namen des SMTP-Server. Normalerweise legen Sie diese Eigenschaft der<em>\_AppStart</em> Seite.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Legt den Benutzernamen für den SMTP-Server fest. Normalerweise sollten Sie diese Eigenschaft festlegen, der  *\_AppStart* Seite.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Validierung

### `Html.ValidationMessage(field)`

(v2) Rendert eine Überprüfungsfehlermeldung für das angegebene Feld an.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

(v2) Zeigt eine Liste aller Validierungsfehler.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

(v2) Registriert ein benutzereingabeelement für den angegebenen Typ der Überprüfung.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

(v2) Rendert dynamisch CSS-Klassenattribute für die clientseitige Validierung auf, sodass Sie Meldungen für Validierungsfehler formatieren können. (Erfordert, dass Sie die entsprechenden Client-Script-Bibliotheken verweisen und, dass Sie CSS-Klassen definieren.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

(v2) Ermöglicht die clientseitige Validierung für das Eingabefeld für die Benutzer an. (Erfordert, dass Sie die entsprechenden Client-Script-Bibliotheken verweisen.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

(v2) Gibt "true" zurück, wenn alle benutzereingabeelemente, die Registred für die Überprüfung auf gültige Werte enthalten.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

(v2) Gibt an, dass Benutzer einen Wert für das benutzereingabeelement bereitstellen müssen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

(v2) Gibt an, dass Benutzer Werte für die einzelnen die benutzereingabeelemente angeben müssen. Diese Methode können Geben Sie eine benutzerdefinierte Fehlermeldung Sie nicht.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

(v2) Gibt einen Überprüfungstest, bei der Verwendung der `Validation.Add` Methode.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
