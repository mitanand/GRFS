# rideshareapi - Mitanand
Connecting public transport and ridesharing
Based on <a href="https://gtfs.org/de/">GTFS</a> (🟥 = required fields, 🟦 = optional fields)

## :rotating_light: ToDo 
- [ ] GTFS-RT <br>
- [x] Preise -> fare_attributes.txt <br>
- [ ] Creation Date? <br>
- [x] Language bei agency hinzufügen? --> Vorerst rausgelassen <br>
- [x] trip_url nicht in ursprünglicher Datei --> Deeplink zur Anwendung nicht Standard <br>
- [ ] Limit Beschreiben max. 256 Character etc.
- [ ] Links im Dokument setzen

## :minibus:  Aktueller Stand

### [agency.txt](#agency)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [agency_id](#agency_id) | Operator-ID | EXAMPLE AG | 🟥 |
| [agency_name](#agency_name) | Operator-Name | example | 🟥 |
| [agency_url](#agency_url) | Operator-URL | [https://www.example.com](https://www.example.com) | 🟦 |
| [agency_timezone](#agency_timezone) | Zeitzone | Europa/Berlin | 🟥 |

> #### Beispiel: agency.txt
> agency_id,agency_name,agency_url,agency_timezone <br>
> EXAMPLE AG,example,http://www.example.com,Europa/Berlin


### [routes.txt](#routes)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [route_id](#route_id) | {origin_uuid}_{destination_uuid} | 05558a29-7a0a-42fa-8162-501e3c7a024a_dfde43ae-7f38-4d6e-9951-bfd622e23c55 | 🟥 |
| [agency_id](#agency_id) | Operator-ID | EXAMPLE AG | 🟥 |
| [route_short_name](#route_short_name) | {departure_city} → {arrival_city} | Berlin -> Munich  | 🟥 |
| [route_long_name](#route_long_name) | {departure_address} → {arrival_address} | Alexanderplatz 7, 10178 Berlin -> Marienplatz 8, 80331 Munich | 🟥 |
| [route_type](#route_type) | routeType OpenTripPlanner | 1551 | 🟥 |

> #### Beispiel: routes.txt
> route_id,agency_id,route_short_name,route_long_name, route_type <br>
> ,,,,

### [trips.txt](#trips)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [route_id](#route_id) | vgl. [route_id](#route_id) | 05558a29-7a0a-42fa-8162-501e3c7a024a_dfde43ae-7f38-4d6e-9951-bfd622e23c55 | 🟥 |
| [service_id](#service_id) | vgl. [service_id](#service_id)  | 633dae2d-4879-4b56-b17a-5c9b90b219ab | 🟥 |
| [trip_id](#trip_id) | Ride UUID | 3e5cacd3-96de-4c40-9f4f-caf17b85619a | 🟥 |
| [shape_id](#shape_id) | vgl. [shape_id](#shape_id) | fb65b6be-fcd6-48ce-a36d-b6ddee82212 | 🟥 |
| [trip_url](#trip_url) | Non standard field Deeplink prefix to find corresponding ride in the app | http://example.app/gtfs/3e5cacd3-96de-4c40-9f4f-caf17b85619a | 🟥 |

### [stop_times.txt](#stop_times)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [trip_id](#trip_id) | vgl. [trip_id](#trip_id) | 3e5cacd3-96de-4c40-9f4f-caf17b85619a | 🟥 |
| [arrival_time](#arrival_time) | {HH:MM:SS} | 13:45:35 | 🟥 |
| [departure_time](#departure_time) | {HH:MM:SS} | 13:55:45 | 🟥 |
| [stop_id](#stop_id) | vgl. [stop_id](#stop_id) | 30a9159b-2bcd-4763-b8c6-f13bde552fc1 | 🟥 |
| [stop_sequence](#stop_sequence) | Order of stops for a particular trip. Start: 0 | 0 | 🟥 |

### [stops.txt](stops)


| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [stop_id](#stop_id) | PoI UUID | 30a9159b-2bcd-4763-b8c6-f13bde552fc1 | 🟥 |
| [stop_name](#stop_name) | {Straße}, {Nr.}, {PLZ}, {Ort} | Tegernseerplatz 1, 81539 München | 🟥 |
| [stop_lat](#stop_lat) | WGS84-Breitengrad im Dezimalformat | 2.09 | 🟥 |
| [stop_lon](#stop_lon) | WGS84-Längengrad im Dezimalformat. | 4.38 | 🟥 |

### [calendar.txt](#calendar)

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
| [end_date](end_date) | date of the finish ride | 20230804 | 🟥 |

### [driver.txt](#driver)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [profile_picture](#profile_picture) |  |  | 🟦 |
| [driver_id](#driver_id) | Ein String aus UTF-8-Zeichen | 21321asd52a1sd58 | 🟦 |
| [rating](#rating) | {number} 1 low to 5 best | 5 | 🟦 |
    
### [additional_ridesharing_info.txt](additional_ridesharing_info)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [number_free_seats](#number_free_seats) | {number} 0 to 40 best | 2 | 🟥 |
| [same_gender](#same_gender) | {Boolean} | true | 🟦 |
| [luggage_size](#luggage_size) | Ein String aus UTF-8-Zeichen klein, mittel, groß | klein | 🟦 |
| [animal_car](#animal_car) | {Boolean} | false | 🟦 |
| [car_model](#car_model) | Ein String aus UTF-8-Zeichen | Golf | 🟦 |
| [car_brand](#car_brand) | Ein String aus UTF-8-Zeichen | VW | 🟦 |
| [creation_date](#creation_date) | {YYYYMMDD HH:MM:SS} | 20230820 12:10:10 | 🟦 |
| [smoking](#smoking) | {Boolean} | false | 🟦 |
| [payment_method](#payment_method) | Ein String aus UTF-8-Zeichen | PayPal | 🟦 |

### [fare_attributes.txt](#fare_attributes)

| Feld | Beschreibung | Beispiel | Notwendigkeit |
| :-------------: | :-------------: | :-------------: | :-------------: |
| [fare_id](#fare_id) | Ein String aus UTF-8-Zeichen | 54asdasd8asd2asd | 🟥 |
| [price](#price) | Ein Gleitkommawert größer oder gleich 0 | 2.30 | 🟥 |
| [currency_type](#currency_type) | Währungscode https://de.wikipedia.org/wiki/ISO_4217#Active_codes.| EUR | 🟥 |




  
