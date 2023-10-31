# GRFS General Ridesharing Feed Specification - Mitanand
Connecting public transport and ridesharing based on [GTFS](https://gtfs.org).
<!--
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
- [ ] Teilstrecken Buchung bzw. Matching -> trip_id Bezug bei fare_attributes fehlerhaft wegen Teilstrecke. Preis pro Kilometer? Teilstreckung Berechnung funktioniert nur beim dem der das Matching macht -> Cent pro Kilometer / Schwierigkeit bei Vollautomatisierten Systemen bspw. BlaBlaCar
- [ ] 🟥 Pflichtfeld / 🟦 Optional
- [ ] Vollautomatisierte Systeme direkt über GTFS da Stops vorher definiert sind.
- [ ] profile_picture -> url
- [ ] rating muss ein float sein
- [ ] routes.txt und trips.txt Beziehung 1:1
- [ ] Shapes.txt welches Koordinatensystem wird verwendet - WGS 84?!
-->
<br>

## :minibus: Specification

<details>
<summary><h3>agency.txt</h3><br>
</summary>

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
</details>


<details>
<summary><h3>routes.txt</h3><br>
</summary>
  

File: **Required**

All **Optional** attributes as in [GTFS routes.txt](https://gtfs.org/schedule/reference/#routestxt).

Primary key (`route_id`) - 1:1 Beziehung zwischen route_id und trip_id - Abweichung von GTFS.

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
</details>


<details>
<summary><h3>trips.txt</h3><br>
</summary>

File: **Required**

All **Optional** attributes as in [GTFS trips.txt](https://gtfs.org/schedule/reference/#tripstxt).

Primary key (`trip_id`) - 1:1 Beziehung zwischen route_id und trip_id - Abweichung von GTFS.

|  Field Name | Type | Presence | Description |
|  ------ | ------ | ------ | ------ |
|  `route_id` | Foreign ID referencing `routes.route_id` | **Required** | Identifies a route. |
|  `service_id` | Foreign ID referencing `calendar.service_id` or `calendar_dates.service_id` | **Required** | Identifies a set of dates when service is available for one or more routes. |
|  `trip_id` | Unique ID | **Required** | Identifies a trip. |
|  `shape_id` | Foreign ID referencing `shapes.shape_id` | **Required** | Identifies a geospatial shape describing the vehicle travel path for a trip. |

#### Example: trips.txt

```
route_id,trip_id,service_id,shape_id 
goflux:05558a29-7a0a-42fa-8162-501e3c7a024a_dfde43ae-7f38-4d6e-9951-bfd622e23c55,"EXAMPLE AG","https://www.example.com",Europe/Berlin
```

</details>

<details>
<summary><h3>stop_times.txt</h3><br>
</summary>

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

#### Example: stop_times.txt

```
trip_id,departure_time,arrival_time,stop_id,stop_sequence
fg:1,15:45:06,15:45:06,de:08436:8049,2
```
</details>

<details>
<summary><h3>stops.txt</h3><br>
</summary>

File: **Required**

All **Optional** attributes as in [GTFS stops.txt](https://gtfs.org/schedule/reference/#stopstxt).

Primary key (`stop_id`)

|  Field Name | Type | Presence | Description |
| :-------------: | :-------------: | :-------------: | :-------------: |
|  `stop_id` | Unique ID | **Required** | Identifies a location: stop/platform, station, entrance/exit, generic node or boarding area (see `location_type`). <br><br>Multiple routes may use the same `stop_id`. |
|  `stop_name` | Text | **Required** | Name of the location, e.g. {street}, {house_nr}, {zip}, {city}. |
|  `stop_lat` | Latitude | **Required** | Latitude of the location. |
|  `stop_lon` | Longitude | **Required** | Longitude of the location. |

#### Example: stops.txt

```
stop_id,stop_lat,stop_lon,stop_name
mfdz:Ang001,53.11901,14.015776,Mitfahrbank Biesenbrow
```

</details>

<details>
<summary><h3>calendar.txt.txt</h3><br>
</summary>

File: **Required**

**All** attributes as in [GTFS calendar.txt](https://gtfs.org/schedule/reference/#calendartxt).

Primary key (`service_id`)

| Field Name | Type | Presence | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| `service_id` | Unique ID | **Required** | Identifies a set of dates when service is available for one or more routes. Each service_id value must be unique in a calendar.txt file. |
| `monday` | ENUM | **Required** |  Indicates whether the service operates on all Mondays in the date range specified by the start_date and end_date fields. Note that exceptions for particular dates may be listed in calendar_dates.txt. Valid options are:<br>1 - Service is available for all Mondays in the date range.<br>0 - Service is not available for Mondays in the date range. |
| `tuesday` | ENUM | **Required** | Functions in the same way as monday except applies to Tuesdays |
| `wednesday` | ENUM | **Required** | Functions in the same way as monday except applies to Tuesdays |
| `thursday` | ENUM | **Required** | Functions in the same way as monday except applies to Tuesdays |
| `friday` | ENUM | **Required** | Functions in the same way as monday except applies to Tuesdays |
| `saturday` | ENUM | **Required** | Functions in the same way as monday except applies to Tuesdays |
| `sunday` | ENUM | **Required** | Functions in the same way as monday except applies to Tuesdays |
<!--
| `start_date` | ENUM | **Required** | Start service day for the service interval.  |
| `end_date` | ENUM | **Required** | End service day for the service interval. This service day is included in the interval. |
-->
#### Example: calendar.txt

```
service_id,start_date,end_date,monday,tuesday,wednesday,thursday,friday,saturday,sunday
fg:1,20220223,20220223,0,0,1,0,0,0,0
```
</details>

<details>
<summary><h3>shapes.txt</h3><br>
</summary>

File: **Required**

All **Optional** attributes as in [GTFS shapes.txt](https://gtfs.org/schedule/reference/#shapestxt).

Primary key (`shape_id`)

| Field Name | Type | Presence | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| `shape_id` | ID | **Required** | Identifies a shape. |
| `shape_pt_lon` | Latitude | **Required** | Latitude of a shape point. Each record in shapes.txt represents a shape point used to define the shape. |
| `shape_pt_lat` | Longitude | **Required** | Longitude of a shape point. |
| `shape_pt_sequence` | Non-negative integer | **Required** | Sequence in which the shape points connect to form the shape. Values must increase along the trip but do not need to be consecutive.Example: If the shape "A_shp" has three points in its definition, the shapes.txt file might contain these records to define the shape: <br> shape_id,shape_pt_lat,shape_pt_lon,shape_pt_sequence <br> A_shp,37.61956,-122.48161,0 <br> A_shp,37.64430,-122.41070,6 <br> A_shp,37.65863,-122.30839,11 |

#### Example: shapes.txt

```
shape_id,shape_pt_lon,shape_pt_lat,shape_pt_sequence
1,9.595989,47.753088,1
```

</details>

<details>
<summary><h3>fare_attributes.txt</h3><br>
</summary>

File: Optional

All **Optional** attributes as in [GTFS fare_attributes.txt](https://gtfs.org/schedule/reference/#fare_attributestxt).

Primary key (`trip_id`)

| Field Name | Type | Presence | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| `trip_id` | Unique ID | Optional | Identifies a trip. |
| `fare_id` | Unique ID | Optional | Identifies a fare class. |
| `price` | Non-negative float | Optional | Fare price, in the unit specified by currency_type. |
| `currency_type` | Currency code | Optional | Currency used to pay the fare.|
| `payment_method` | Enum | Optional | Gibt an, wann der Fahrpreis bezahlt werden muss. Gültige Optionen sind:<br> 0 - Fahrpreis wird an Bord bezahlt.<br> 1 - Der Fahrpreis muss vor dem Einsteigen bezahlt werden. |


#### Example: fare_attributes.txt

```
trip_id,fare_id,price,currency_type
e5cacd3-96de-4c40-9f4f-caf17b85619a,54asdasd8asd2asd,5.60,EUR
```

</details>

<details>
<summary><h3>driver.txt</h3><br>
</summary>

File: Optional

Extension of GRFS to the GTFS standard

Primary key (`trip_id`)

| Field Name | Type | Presence | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| `trip_id` | Text | Optional | Identifies a trip. |
| `profile_picture` | URL | Optional | URL contains the profile picture |
| `driver_id` | Unique ID | Optional | Identifies a driver. |
| `rating` | ENUM | Optional | Rating of the driver from 1 to 5. |


#### Example: driver.txt

```
trip_id,profile_picture,driver_id,rating
e5cacd3-96de-4c40-9f4f-caf17b85619a,"https://www.example.com",47753088,3,2
```
</details>

<details>
<summary><h3>additional_ridesharing_info.txt</h3><br>
</summary>

File: Optional

Extension of GRFS to the GTFS standard

Primary key (`trip_id`)

| Field Name | Type | Presence | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| `trip_id` | Unique ID | **Required** | Identifies a trip. |
| `number_free_seats` | ENUM | **Required** |  Number of free seats |
| `same_gender` | ENUM | Optional | Trip only for same gender:<br>1: Yes<br>2:No |
| `luggage_size` | ENUM | Optional | Size of the luggage: <br>1: Small<br>2: medium <br>3: large|
| `animal_car` | ENUM | Optional | Animals in Car allowed:<br>1: Yes<br>2:No |
| `car_model` | Text | Optional | Car model |
| `car_brand` | Text | Optional | Car brand |
| `creation_date` | DATE | **Required** | Date when trip was create - YYYYMMDD HH:MM:SS |
| `smoking` | ENUM | Optional | Smoking allowed:<br>1: Yes<br>2:No  |
| `payment_method` | Text | Optional | Method of payment |


#### Example: additional_ridesharing_info.txt

```
trip_id,number_free_seats,same_gender,luggage_size,animal_car,car_model,car_brand,creation_date,smoking,payment_method
e5cacd3-96de-4c40-9f4f-caf17b85619a,2,false,small,false,Golf,VW,20230820 12:10:10,false,PayPal
```
</details>
<!--
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
-->



  
