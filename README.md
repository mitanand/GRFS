# GRFS General Ridesharing Feed Specification - Mitanand
Connecting public transport and ridesharing based on [GTFS](https://gtfs.org).

## :rotating_light: ToDo 
- [ ] GTFS-RT zur Aktualisierung der Daten <br>
- [ ] Limit Beschreiben max. 256 Character etc.
- [ ] Links im Dokument setzen
- [ ] Ausgabe bei keiner Angabe im optionalen Feld - Default = none?
- [ ] Definition des Inputs von Anbietern / API-Key JSON/XML in welcher Form?
- [ ] Short Name rausnehmen ? Macht Sinn bei Buslinien X200 oder 135 etc.
- [ ] API-Key, Tokens, etc. Sicherheit?
- [ ] Integration via A) JSON/XML API-Key  B) rideshareapi/GTFS C) MFDZ Amarillo Github
- [ ] Zeichen an GTFS anlehnen
- [ ] Trip_url durch route_url ersetzten
- [ ] ID Semantik nicht zu hart angeben
- [ ] Stop_times - Haltestellen sehr nahe am Start und am Ziel -> nur als Einstieg bzw. Ausstieg definieren - Vermeidung von sehr kurzen Fahrten
- [ ] Teilstrecken Buchung bzw. Matching --> trip_id Bezug bei fare_attributes fehlerhaft wegen Teilstrecke. Preis pro Kilometer? Teilstreckung Berechnung funktioniert nur beim dem der das Matching macht --> Cent pro Kilometer / Schwierigkeit bei Vollautomatisierten Systemen bspw. BlaBlaCar
- [ ] 🟥 Pflichtfeld / 🟦 Optional
- [ ] Vollautomatisierte Systeme direkt über GTFS da Stops vorher definiert sind.
- [ ] profile_picture ==> url
- [ ] rating muss ein float sein

<br>

## :minibus: outbound rideshareapi

<details>
<summary><h3>agency.txt</h3> </summary>

Same as [GTFS agency.txt](https://gtfs.org/schedule/reference/#agencytxt).

File: **Required**

Primary key (`agency_id`)

|  Field Name | Type | Presence | Description |
|  ------ | ------ | ------ | ------ |
|  `agency_id` | Unique ID | **Conditionally Required** | Identifies a transit brand which is often synonymous with a transit agency. Note that in some cases, such as when a single agency operates multiple separate services, agencies and brands are distinct. This document uses the term "agency" in place of "brand". A dataset may contain data from multiple agencies. <br><br>Conditionally Required:<br>- **Required** when the dataset contains data for multiple transit agencies. <br>- Recommended otherwise. |
|  `agency_name` | Text | **Required** | Full name of the transit agency. |
|  `agency_url` | URL | **Required** | URL of the transit agency. |
|  `agency_timezone` | Timezone | **Required** | Timezone where the transit agency is located. If multiple agencies are specified in the dataset, each must have the same `agency_timezone`. |
|  `agency_lang` | Language code | Optional | Primary language used by this transit agency. Should be provided to help GTFS consumers choose capitalization rules and other language-specific settings for the dataset. |
|  `agency_phone` | Phone number | Optional | A voice telephone number for the specified agency. This field is a string value that presents the telephone number as typical for the agency's service area. It may contain punctuation marks to group the digits of the number. Dialable text (for example, TriMet's "503-238-RIDE") is permitted, but the field must not contain any other descriptive text. |
|  `agency_fare_url` | URL | Optional | URL of a web page that allows a rider to purchase tickets or other fare instruments for that agency online. |
|  `agency_email` | Email | Optional | Email address actively monitored by the agency’s customer service department. This email address should be a direct contact point where transit riders can reach a customer service representative at the agency. |

> #### Example: agency.txt
> agency_id,agency_name,agency_url,agency_timezone <br>
> "EXAMPLE AG",example,"https://www.example.com",Europa/Berlin
</details>

<details>
<summary><h3>routes.txt</h3> </summary>
  
Similar to [GTFS routes.txt](https://gtfs.org/schedule/reference/#routestxt), more strict.

File: **Required**

Primary key (`route_id`)

|  Field Name | Type | Presence | Description |
|  ------ | ------ | ------ | ------ |
|  `route_id` | Unique ID | **Required** | Identifies a route. Prefixed with agency_id and ":" if multiple agencies are defined in agency.txt, e.g. "goflux:1234" |
|  `agency_id` | Foreign ID referencing `agency.agency_id` | **Conditionally Required** | Agency for the specified route.<br><br>Conditionally Required:<br>- **Required** if multiple agencies are defined in [agency.txt](#agency). <br>- Recommended otherwise. |
|  `route_short_name` | Text | **Required** | Short name of a route departure_{city} -> {arrival_city}, e.g. Berlin - Munich. |
|  `route_long_name` | Text | **Required** | Full name of a route. This name is generally more descriptive than the `route_short_name` and often includes the route's destination or stop, {departure_address} - {arrival_address}, e.g. Alexanderplatz 7, 10178 Berlin - Marienplatz 8, 80331 Munich |
|  `route_type` | Enum | **Required** | 1551 | 1551 is the type supported by OpenTripPlanner |
|  `route_url` | URL | **Required** | URL of a web page about the particular route. Should be different from the `agency.agency_url` value, e.g. https://fahrgemeinschaft.de/?trip=322337 |

> #### Example: routes.txt
> route_id,agency_id,route_short_name,route_long_name, route_type,route_url<br>
> goflux:05558a29-7a0a-42fa-8162-501e3c7a024a_dfde43ae-7f38-4d6e-9951-bfd622e23c55,goflux,"Berlin - Munich","Alexanderplatz 7,10178 Berlin - Marienplatz 8, 80331 Munich",1551,https://goflux.de/?trip=322337

</details>

<details>
<summary><h3>trips.txt</h3> </summary>

| Feld | Typ | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| [route_id](#route_id) | string | vgl. [route_id](#route_id) | 05558a29-7a0a-42fa-8162-501e3c7a024a_dfde43ae-7f38-4d6e-9951-bfd622e23c55 | 🟥 |
| [service_id](#service_id) | string | vgl. [service_id](#service_id)  | 633dae2d-4879-4b56-b17a-5c9b90b219ab | 🟥 |
| [trip_id](#trip_id) | string | Kennzeichnet eine Fahrt | 3e5cacd3-96de-4c40-9f4f-caf17b85619a | 🟥 |
| [shape_id](#shape_id) | string | Kennzeichnet eine raumbezogene Form, die die Fahrstrecke des Fahrzeugs bei einer Fahrt beschreibt | fb65b6be-fcd6-48ce-a36d-b6ddee82212 | 🟥 |
| [trip_url](#trip_url) | string | Non standard field Deeplink prefix to find corresponding ride in the app | http://example.app/gtfs/3e5cacd3-96de-4c40-9f4f-caf17b85619a | 🟥 |
</details>

<details>
<summary><h3>stop_times.txt</h3> </summary>

| Feld | Typ | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| [trip_id](#trip_id) | string | vgl. [trip_id](#trip_id) | 3e5cacd3-96de-4c40-9f4f-caf17b85619a | 🟥 |
| [arrival_time](#arrival_time) | HH:MM:SS | Ankunftszeit an einer bestimmten Haltestelle bei einer bestimmten Fahrt auf einer Route. Wenn die Ankunfts- und die Abfahrtszeit an einer Haltestelle identisch ist, geben Sie für arrival_time und departure_time denselben Wert ein. Geben Sie für Zeiten nach Mitternacht am Betriebstag einen Wert größer als 24:00:00 in HH:MM:SS Ortszeit für den Tag ein, an dem der Fahrplan beginnt. | 13:45:35 | 🟥 |
| [departure_time](#departure_time) | HH:MM:SS | Abfahrtszeit an einer bestimmten Haltestelle bei einer bestimmten Fahrt auf einer Route. Geben Sie für Zeiten nach Mitternacht am Betriebstag einen Wert größer als 24:00:00 in HH:MM:SS Ortszeit für den Tag ein, an dem der Fahrplan beginnt. | 13:55:45 | 🟥 |
| [stop_id](#stop_id) | string | Kennzeichnet den angefahrenen Haltepunkt | 30a9159b-2bcd-4763-b8c6-f13bde552fc1 | 🟥 |
| [stop_sequence](#stop_sequence) | string | Reihenfolge der Haltestellen bei einer bestimmten Fahrt. Start: 0 | 0 | 🟥 |
</details>

<details>
<summary><h3>stops.txt</h3> </summary>

| Feld | Typ | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| [stop_id](#stop_id) | string | Kennzeichnet eine Haltestelle, eine Station oder einen Stationseingang. | 30a9159b-2bcd-4763-b8c6-f13bde552fc1 | 🟥 |
| [stop_name](#stop_name) | string | Name des Orts {Straße}, {Nr.}, {PLZ}, {Ort} | Tegernseerplatz 1, 81539 München | 🟥 |
| [stop_lat](#stop_lat) | float | Breitengrad des Orts in WGS84-Breitengrad im Dezimalformat | 2.09 | 🟥 |
| [stop_lon](#stop_lon) | float | Längengrad des Orts in WGS84-Längengrad im Dezimalformat. | 4.38 | 🟥 |
</details>

<details>
<summary><h3>calendar.txt</h3> </summary>

| Feld | Typ | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| [service_id](#service_id) | string | Kennzeichnet eindeutig eine Reihe von Tagen, an denen für eine oder mehrere Routen ein Fahrbetrieb stattfindet | 633dae2d-4879-4b56-b17a-5c9b90b219ab | 🟥 |
| [monday](#monday) | int | Gibt an, ob der Fahrbetrieb in dem in den Feldern start_date und end_date angegebenen Zeitraum an jedem Montag stattfindet. 1 if ride on weekday else 0 | 1 | 🟥 |
| [tuesday](#tuesday) | int | vgl. Montag | 1 | 🟥 |
| [wednesday](#wednesday) | int | vgl. Montag | 1 | 🟥 |
| [thursday](#thursday) | int | vgl. Montag | 1 | 🟥 |
| [friday](#friday) | int | vgl. Montag | 1 | 🟥 |
| [saturday](#saturday) | int | vgl. Montag | 0 | 🟥 |
| [sunday](#sunday) | int | vgl. Montag | 0 | 🟥 |
| [start_required](start_required) | YYYYMMDD | day of the next upcoming ride | 20230801 | 🟥 |
| [end_date](end_date) | YYYYMMDD | Letzter Betriebstag im Betriebsintervall. Dieser Betriebstag ist im Intervall enthalten | 20230804 | 🟥 |
</details>


<details>
<summary><h3>driver.txt</h3> </summary>

| Feld | Typ | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| [trip_id](#trip_id) | string | vgl. [trip_id](#trip_id) | 3e5cacd3-96de-4c40-9f4f-caf17b85619a | 🟥 |
| [profile_picture](#profile_picture) | |  |  | 🟦 |
| [driver_id](#driver_id) | string | Ein String aus UTF-8-Zeichen | 21321asd52a1sd58 | 🟦 |
| [rating](#rating) | int | {number} 1 low to 5 best | 5 | 🟦 |
</details>


<details>
<summary><h3>additional_ridesharing_info.txt</h3> </summary>

| Feld | Typ | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| [trip_id](#trip_id) | string | vgl. [trip_id](#trip_id) | 3e5cacd3-96de-4c40-9f4f-caf17b85619a | 🟥 |
| [number_free_seats](#number_free_seats) | int | {number} 0 to 40 best | 2 | 🟥 |
| [same_gender](#same_gender) | boolean | {Boolean} | true | 🟦 |
| [luggage_size](#luggage_size) | string | Ein String aus UTF-8-Zeichen klein, mittel, groß | klein | 🟦 |
| [animal_car](#animal_car) | boolean | {Boolean} | false | 🟦 |
| [car_model](#car_model) | string |Ein String aus UTF-8-Zeichen | Golf | 🟦 |
| [car_brand](#car_brand) | string |Ein String aus UTF-8-Zeichen | VW | 🟦 |
| [creation_date](#creation_date) | YYYYMMDD HH:MM:SS | {YYYYMMDD HH:MM:SS} | 20230820 12:10:10 | 🟦 |
| [smoking](#smoking) | boolean |{Boolean} | false | 🟦 |
| [payment_method](#payment_method) | string | Ein String aus UTF-8-Zeichen | PayPal | 🟦 |
</details>

<details>
<summary><h3>fare_attributes.txt</h3> </summary>

| Feld | Typ | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| [trip_id](#trip_id) | string | vgl. [trip_id](#trip_id) | 3e5cacd3-96de-4c40-9f4f-caf17b85619a | 🟦 |
| [fare_id](#fare_id) | string | Kennzeichnet eine Preisklasse | 54asdasd8asd2asd | 🟦 |
| [price](#price) | float |Fahrpreis in der in currency_type angegebenen Einheit. Ein Gleitkommawert größer oder gleich 0. In der Einheit € pro Kilometer | 2.30 | 🟦 |
| [currency_type](#currency_type) | string | Währung, in der der Fahrpreis bezahlt wird. Währungscode https://de.wikipedia.org/wiki/ISO_4217#Active_codes.| EUR | 🟦 |
</details>

<br>

## :hammer: inbound rideshareapi

### How to participate as a ridesharing provider?

<br>
<br>

![Alt Test](https://github.com/mitanand/rideshareapi/blob/4f4db82f9dd5d677ef3dd926dbc86237c6d5e45a/rideshareapi_how_to_participate.png)

<br>

<details>
<summary><h3>JSON/XML via API-KEY (Picutre version A)</h3> </summary>
</details>

<details>
<summary><h3>rideshareapi via ridesharing provider (Picutre version B)</h3> </summary>
</details>

<details>
<summary><h3>MFDZ Amarillo (Picutre version A)</h3> </summary>
</details>




  
