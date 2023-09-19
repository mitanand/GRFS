# rideshareapi - Mitanand
Connecting public transport and ridesharing based on <a href="https://gtfs.org/de/">GTFS</a>

## :rotating_light: ToDo 
- [ ] GTFS-RT zur Aktualisierung der Daten <br>
- [ ] Limit Beschreiben max. 256 Character etc.
- [ ] Links im Dokument setzen
- [ ] Ausgabe bei keiner Angabe im optionalen Feld - Default = none?
- [ ] Definition des Inputs von Anbietern / API-Key JSON/XML in welcher Form?
- [ ] Short Name rausnehmen ? Macht Sinn bei Buslinien X200 oder 135 etc.

<br>

## :raised_hand: How to participate as a ridesharing provider?

<br>
<br>

![Alt Test](https://github.com/mitanand/rideshareapi/blob/f6375cb9a11b0bc55cfa41e02dc4d1f11e788916/rideshareapi.png)

<br>

## :minibus: rideshareapi (🟥 = required fields, 🟦 = optional fields)

<details>
<summary><h3>agency.txt</h3> </summary>

| Feld | Typ | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| [agency_id](#agency_id) | string | Operator ID | EXAMPLE AG | 🟥 |
| [agency_name](#agency_name) | string | Operator Name | example | 🟥 |
| [agency_timezone](#agency_timezone) | string | Zeitzone | Europa/Berlin | 🟥 |
| [agency_url](#agency_url) | string | Operator URL | [https://www.example.com](https://www.example.com) | 🟦 |
| [agency_lang](#agency_lang) | string | Operator Language | de | 🟦 |

> #### Beispiel: agency.txt
> agency_id,agency_name,agency_url,agency_timezone <br>
> EXAMPLE AG, example, http://www.example.com, Europa/Berlin
</details>

<details>
<summary><h3>routes.txt</h3> </summary>

| Feld | Typ | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| [route_id](#route_id) | string | Kennzeichnet eine Route {origin_uuid}_{destination_uuid} | 05558a29-7a0a-42fa-8162-501e3c7a024a_dfde43ae-7f38-4d6e-9951-bfd622e23c55 | 🟥 |
| [agency_id](#agency_id) | string | Operator ID | EXAMPLE AG | 🟥 |
| [route_short_name](#route_short_name) | string | Kurzname einer Route {departure_city} → {arrival_city} | Berlin -> Munich  | 🟥 |
| [route_long_name](#route_long_name) | string | Vollständiger Name einer Route {departure_address} → {arrival_address} | Alexanderplatz 7, 10178 Berlin -> Marienplatz 8, 80331 Munich | 🟥 |
| [route_type](#route_type) | string | routeType OpenTripPlanner | 1551 | 🟥 |

> #### Beispiel: routes.txt
> route_id,agency_id,route_short_name,route_long_name, route_type <br>
> 05558a29-7a0a-42fa-8162-501e3c7a024a_dfde43ae-7f38-4d6e-9951-bfd622e23c55,EXAMPLE AG,Berlin -> Munich ,Alexanderplatz 7,10178 Berlin -> Marienplatz 8, 80331 Munich,1551
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
| [profile_picture](#profile_picture) | |  |  | 🟦 |
| [driver_id](#driver_id) | string | Ein String aus UTF-8-Zeichen | 21321asd52a1sd58 | 🟦 |
| [rating](#rating) | int | {number} 1 low to 5 best | 5 | 🟦 |
</details>


<details>
<summary><h3>additional_ridesharing_info.txt</h3> </summary>

| Feld | Typ | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
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
| [fare_id](#fare_id) | string | Kennzeichnet eine Preisklasse | 54asdasd8asd2asd | 🟥 |
| [price](#price) | float |Fahrpreis in der in currency_type angegebenen Einheit. Ein Gleitkommawert größer oder gleich 0 | 2.30 | 🟥 |
| [currency_type](#currency_type) | string | Währung, in der der Fahrpreis bezahlt wird. Währungscode https://de.wikipedia.org/wiki/ISO_4217#Active_codes.| EUR | 🟥 |
</details>


  
