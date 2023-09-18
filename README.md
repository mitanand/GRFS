# rideshareapi - Mitanand
Connecting public transport and ridesharing

> Based on <a href="https://gtfs.org/de/">GTFS</a> <br>

> 🟥 = required <br>
> 🟦 = optional

# :minibus:  Aktueller Stand

## [agency.txt](#agency)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [agency_id](#agency_id) | Operator-ID | GOFLUX | 🟥 |
| [agency_name](#agency_name) | Operator-Name | goFlux | 🟥 |
| [agency_url](#agency_url) | Operator-URL | [https://www.goflux.de](https://www.goflux.de) | 🟦 |
| [agency_timezone](#agency_timezone) | Zeitzone | Europa/Berlin | 🟥 |

## [routes.txt](#routes)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [route_id](#route_id) |  |  | 🟥 |
| [agency_id](#agency_id) | Operator-ID | GOFLUX | 🟥 |
| [route_short_name](#route_short_name) |  | Berlin -> Munich  | 🟥 |
| [route_long_name](#route_long_name) |  | Alexanderplatz 7, 10178 Berlin -> Marienplatz 8, 80331 Munich | 🟥 |
| [route_type](#route_type) |  | 1551 | 🟥 |

## [trips.txt](#trips)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [route_id](#route_id) |  |  | 🟥 |
| [service_id](#service_id) |  |  | 🟥 |
| [trip_id](#trip_id) |  |  | 🟥 |
| [shape_id](#shape_id) |  |  | 🟥 |
| [trip_url](#trip_url) |  |  | 🟥 |

## [stop_times.txt](#stop_times)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [trip_id](#trip_id) |  |  |🟥 |
| [arrival_time](#arrival_time) |  |  | 🟥 |
| [departure_time](#departure_time) |  |  | 🟥 |
| [stop_id](#stop_id) |  |  | 🟥 |
| [stop_sequence](#stop_sequence) |  |  | 🟥 |

## [stops.txt](stops)


| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [stop_id](#stop_id) | PoI UUID | 30a9159b-2bcd-4763-b8c6-f13bde552fc1 | 🟥 |
| [stop_name](#stop_name) | PoI address |  | 🟥 |
| [stop_lat](#stop_lat) | latitude | 2.09 | 🟥 |
| [stop_lon](#stop_lon) | longitude | 4.38 | 🟥 |

## [calendar.txt](#calendar)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [service_id](#service_id) | UUID | 633dae2d-4879-4b56-b17a-5c9b90b219ab | 🟥 |
| [monday](#monday) | 1 if ride on weekday else 0 | 1 | 🟥 |
| [tuesday](#tuesday) | 1 if ride on weekday else 0 | 1 | 🟥 |
| [wednesday](#wednesday) | 1 if ride on weekday else 0 | 1 | 🟥 |
| [thursday](#thursday) | 1 if ride on weekday else 0 | 1 | 🟥 |
| [friday](#friday) | 1 if ride on weekday else 0 | 1 | 🟥 |
| [saturday](#saturday) | 1 if ride on weekday else 0 | 0 | 🟥 |
| [sunday](#sunday) | 1 if ride on weekday else 0 | 0 | 🟥 |
| [start_required](start_required) | day of the next upcoming ride | 20230801 | 🟥 |
| [end_date](end_date) |  | 20230804 | 🟥 |

# :rotating_light: Fehlend 
 // GTFS-RT einfügen <br>
 // Preise? <br>
 // Creation Date?
 // Beispieldatensatz neutralisieren Beispiel GmbH etc.
 // Language bei agency hinzufügen?
 // trip_url nicht in ursprünglicher Datei?

## [driver.txt](#driver)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [profile_picture](#profile_picture) |  |  | 🟦 |
| [driver_id](#driver_id) |  |  | 🟦 |
| [rating](#rating) |  |  | 🟦 |
    
## [additional_ridesharing_info.txt](additional_ridesharing_info)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [number_free_seats](#number_free_seats) |  |  | 🟥 |
| [same_gender](#same_gender) |  |  | 🟦 |
| [luggage_size](#luggage_size) |  |  | 🟦 |
| [animal_car](#animal_car) |  |  | 🟦 |
| [car_model](#car_model) |  |  | 🟦 |
| [car_brand](#car_brand) |  |  | 🟦 |
| [creation_date](#creation_date) |  |  | 🟦 |
| [smoking](#smoking) |  |  | 🟦 |
| [payment_method](#payment_method) |  |  | 🟦 |

## [fare_attributes.txt](#fare_attributes)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [fare_id](#fare_id) |  |  | 🟥 |
| [price](#price) |  |  | 🟥 |
| [currency_type](#currency_type) |  |  | 🟥 |




  
