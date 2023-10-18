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

## :minibus: Specification

### agency.txt

File: **Required**

All **Optional** attributes as in [GTFS agency.txt](https://gtfs.org/schedule/reference/#agencytxt).

Primary key (`agency_id`)

|  Field Name | Type | Presence | Description |
|  ------ | ------ | ------ | ------ |
|  `agency_id` | Unique ID | **Required** | Identifies a transit brand which is often synonymous with a transit agency. Note that in some cases, such as when a single agency operates multiple separate services, agencies and brands are distinct. This document uses the term "agency" in place of "brand". A dataset may contain data from multiple agencies. |
|  `agency_name` | Text | **Required** | Full name of the transit agency. |
|  `agency_url` | URL | **Required** | URL of the transit agency. |
|  `agency_timezone` | Timezone | **Required** | Timezone where the transit agency is located. If multiple agencies are specified in the dataset, each must have the same `agency_timezone`. |

#### Example: agency.txt

```
agency_id,agency_name,agency_url,agency_timezone 
example,"EXAMPLE AG","https://www.example.com",Europe/Berlin
```


### routes.txt
  

File: **Required**

All **Optional** attributes as in [GTFS routes.txt](https://gtfs.org/schedule/reference/#routestxt).

Primary key (`route_id`)

|  Field Name | Type | Presence | Description |
|  ------ | ------ | ------ | ------ |
|  `route_id` | Unique ID | **Required** | Identifies a route. Prefixed with agency_id and ":" if multiple agencies are defined in agency.txt, e.g. "goflux:1234" |
|  `agency_id` | Foreign ID referencing `agency.agency_id` | **Required** | Agency for the specified route. |
|  `route_short_name` | Text | **Required** | Short name of a route departure_{city} -> {arrival_city}, e.g. Berlin - Munich. |
|  `route_long_name` | Text | **Required** | Full name of a route. This name is generally more descriptive than the `route_short_name` and often includes the route's destination or stop, {departure_address} - {arrival_address}, e.g. Alexanderplatz 7, 10178 Berlin - Marienplatz 8, 80331 Munich |
|  `route_type` | Enum | **Required** | 1551 | 1551 is the type supported by OpenTripPlanner |
|  `route_url` | URL | **Required** | URL of a web page about the particular route. Should be different from the `agency.agency_url` value, e.g. https://fahrgemeinschaft.de/?trip=322337 |

#### Example: routes.txt

```
route_id,agency_id,route_short_name,route_long_name, route_type,route_url
goflux:05558a29-7a0a-42fa-8162-501e3c7a024a_dfde43ae-7f38-4d6e-9951-bfd622e23c55,goflux,"Berlin - Munich","Alexanderplatz 7,10178 Berlin - Marienplatz 8, 80331 Munich",1551,https://goflux.de/?trip=322337
```

### trips.txt

File: **Required**

All **Optional** attributes as in [GTFS trips.txt](https://gtfs.org/schedule/reference/#tripstxt).

Primary key (`trip_id`)

|  Field Name | Type | Presence | Description |
|  ------ | ------ | ------ | ------ |
|  `route_id` | Foreign ID referencing `routes.route_id` | **Required** | Identifies a route. |
|  `service_id` | Foreign ID referencing `calendar.service_id` or `calendar_dates.service_id` | **Required** | Identifies a set of dates when service is available for one or more routes. |
|  `trip_id` | Unique ID | **Required** | Identifies a trip. |
|  `shape_id` | Foreign ID referencing `shapes.shape_id` | **Required** | Identifies a geospatial shape describing the vehicle travel path for a trip. |


### stop_times.txt

File: **Required**

All **Optional** attributes as in [GTFS stop_times.txt](https://gtfs.org/schedule/reference/#stop_timestxt).

Primary key (`trip_id`, `stop_sequence`)

|  Field Name | Type | Presence | Description |
|  ------ | ------ | ------ | ------ |
|  `trip_id` | Foreign ID referencing `trips.trip_id` | **Required** | Identifies a trip.  |
|  `arrival_time` | Time | **Conditionally Required** | Arrival time at the stop (defined by `stop_times.stop_id`) for a specific trip (defined by `stop_times.trip_id`) in the time zone specified by `agency.agency_timezone`, not `stops.stop_timezone`. <br><br>If there are not separate times for arrival and departure at a stop, `arrival_time` and `departure_time` should be the same. <br><br>For times occurring after midnight on the service day, enter the time as a value greater than 24:00:00 in HH:MM:SS.<br><br> If exact arrival and departure times (`timepoint=1` or empty) are not available, estimated or interpolated arrival and departure times (`timepoint=0`) should be provided.<br><br>Conditionally Required:<br>- **Required** for the first and last stop in a trip (defined by `stop_times.stop_sequence`). <br>- **Required** for `timepoint=1`.<br>- Optional otherwise. |
|  `departure_time` | Time | **Conditionally Required** | Departure time from the stop (defined by `stop_times.stop_id`) for a specific trip (defined by `stop_times.trip_id`) in the time zone specified by `agency.agency_timezone`, not `stops.stop_timezone`.<br><br>If there are not separate times for arrival and departure at a stop, `arrival_time` and `departure_time` should be the same. <br><br>For times occurring after midnight on the service day, enter the time as a value greater than 24:00:00 in HH:MM:SS.<br><br> If exact arrival and departure times (`timepoint=1` or empty) are not available, estimated or interpolated arrival and departure times (`timepoint=0`) should be provided. | |
|  `stop_id` | Foreign ID referencing `stops.stop_id` | **Required** | Identifies the serviced stop. All stops serviced during a trip must have a record in [stop_times.txt](#stop_timestxt). Referenced locations must be stops/platforms, i.e. their `stops.location_type` value must be `0` or empty. A stop may be serviced multiple times in the same trip, and multiple trips and routes may service the same stop. |
|  `stop_sequence` | Non-negative integer | **Required** | Order of stops for a particular trip. The values must increase along the trip but do not need to be consecutive.<hr>*Example: The first location on the trip could have a `stop_sequence`=`1`, the second location on the trip could have a `stop_sequence`=`23`, the third location could have a `stop_sequence`=`40`, and so on.* |


### stops.txt

File: **Required**

All **Optional** attributes as in [GTFS stops.txt](https://gtfs.org/schedule/reference/#stopstxt).

Primary key (`stop_id`)

|  Field Name | Type | Presence | Description |
|  ------ | ------ | ------ | ------ |
|  `stop_id` | Unique ID | **Required** | Identifies a location: stop/platform, station, entrance/exit, generic node or boarding area (see `location_type`). <br><br>Multiple routes may use the same `stop_id`. |
|  `stop_name` | Text | **Required** | Name of the location, e.g. {street}, {house_nr}, {zip}, {city}. |
|  `stop_lat` | Latitude | **Required** | Latitude of the location. |
|  `stop_lon` | Longitude | **Required** | Longitude of the location. |


### calendar.txt

File: **Required**

Primary key (`service_id`)

**All** attributes as in [GTFS calendar.txt](https://gtfs.org/schedule/reference/#calendartxt).

### driver.txt

| Feld | Typ | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| [trip_id](#trip_id) | string | vgl. [trip_id](#trip_id) | 3e5cacd3-96de-4c40-9f4f-caf17b85619a | 🟥 |
| [profile_picture](#profile_picture) | |  |  | 🟦 |
| [driver_id](#driver_id) | string | Ein String aus UTF-8-Zeichen | 21321asd52a1sd58 | 🟦 |
| [rating](#rating) | int | {number} 1 low to 5 best | 5 | 🟦 |


### additional_ridesharing_info.txt

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


### fare_attributes.txt

| Feld | Typ | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| [trip_id](#trip_id) | string | vgl. [trip_id](#trip_id) | 3e5cacd3-96de-4c40-9f4f-caf17b85619a | 🟦 |
| [fare_id](#fare_id) | string | Kennzeichnet eine Preisklasse | 54asdasd8asd2asd | 🟦 |
| [price](#price) | float |Fahrpreis in der in currency_type angegebenen Einheit. Ein Gleitkommawert größer oder gleich 0. In der Einheit € pro Kilometer | 2.30 | 🟦 |
| [currency_type](#currency_type) | string | Währung, in der der Fahrpreis bezahlt wird. Währungscode https://de.wikipedia.org/wiki/ISO_4217#Active_codes.| EUR | 🟦 |

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




  
