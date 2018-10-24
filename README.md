# PyCarGr - Unofficial car.gr API

Searching for a car in Greece (and elsewhere probably) can be a boring process.
As my main search engine I use [car.gr](https:///www.car.gr)
which is the largest online marketplace for cars in Greece.
As I was clicking through checkboxes and dropdowns every now end then to check if there are any new entries
that fit my search criteria, it occurred to me that I'me a programmer so there must be a more efficient (and fun) way to
do my search.
My initial solution was a simple cron job that periodically scanned for new entries that fit my search criteria
and sended me an e-mail.
Later on I started playing around with the website's source and realized it was pretty simple to reverse-engineer
(almost everything is static html)
and thought that it'd make sense to scrap it and maybe do some aggregate statistics on the results and generally play with the data.
This required a more structured approach, hence I developed *PyCarGr*.
 
*PyCarGr* is an unofficial car.gr API backed-by flask.
It provides programmatic access to car data and search results.
Search results can also be exported as a nicely formatted csv file
Results are also stored in a SQLite Database.

## Installation
Before running *PyCarGr API*, you have to clone the code from this repository, install requirements at first.

```shell
$ git clone git@github.com:Florents-Tselai/PyCarGr.git
$ cd PyCarGr
$ pip install -r requirements.txt
```

## Usage
To run this API application, use the `flask` command as same as [Flask Quickstart](http://flask.pocoo.org/docs/0.12/quickstart/)

```shell
$ export FLASK_APP=./pycargr/api.py
$ export FLASK_DEBUG=1 ## if you run in debug mode.
$ flask run
 * Running on http://localhost:5000/
```

## Documentation

### Car
* `GET /api/car/{car_id}` - Get car data

#### Response
```json
{
    "bhp": 180,
    "car_id": "10068060",
    "city": "ΜΑΡΟΥΣΙ ΑΤΤΙΚΗΣ ",
    "color": "Ασπρο ",
    "description": "Δερμάτινο ηλεκτρικό σαλόνι 8 κινήσεων με μνήμες,Premium Leder,δερμάτινο ταμπλώ,ηλεκτρική κουκούλα,δερμάτινο τιμόνι πολλαπλών με αλλαγές ταχυτήτων,20άρες ζάντες,οθόνη Touchscreen,CD-DVD Player,Pdc με Camera Surround εμπρός και πίσω,αναδιπλούμενοι αντιθαμβωτικοί καθρέφτες,φιμέ τζάμια,Bluetooth,επένδυση Piano Black,Keyless-go,αισθητήρες βροχής και φώτων,έξοδος usb-i pod,έλεγχο πίεσης ελαστικών,αυτόματο-σειριακό 9άρι σασμάν,Bordcomputer,Cruise Control,Isofix,εργοστασιακό συναγερμό,Terrain Response,σύστημα αποφυγής σύγκρουσης,Start-Stop System,Skisack,έλεγχος αλλαγής λωρίδας,Meridian ηχοσύστημα,Dynamic Packet,1o χέρι,Ελληνικής αντιπροσωπείας με εργοστασιακή εγγύηση FUJI WHITE FINISHER-ETCHED ALUMINIUM 20 X 8 STYLE 16 SPARKLE SILVER 245/45R20 V RATED ALL TERRAIN PROGRESS CONTROL AMBIENT LIGHTING ANALOGUE AUDIO BROADCAST APPROACH LIGHTS AUTO DIMMING REAR VIEW MIRROR AUTOMATIC HEADLAMP ON/OFF BADGE-EVOQUE - RED BLUETOOTH-AUDIO STREAM BMPR LWR VLC-BLACK BRANDED SOUND SYS (L3) BUMPER APERTUR - BRUNEL STYLE2 BUMPERS-PLASTIC-SPORT CD/DVD PLAYER CONSOLE LID-LEATHER CONTINENTAL TYRE VENDOR CRUISE CONTROL DISC BRAKE SIZE 17 DOOR HANDLES-BODY DR ARMREST-LEATHER DR TRIM PNL-PVC/TAURUS DR/PAS ELEC FRT ST ADJ/MEMORY EBONY HEADLINER EFFICIENT DRIVELINE ELECTRONIC LOCKABLE GLOVEBOX "engine" POWER SOURCE - 180/430 EU6B + DPF EMISSION EXT MIRROR FINISH-BLACK FAMILY BADGE-LANDROVER ATLAS FLOOR MATS-CARPET FOOT PEDAL - METALIC FINISH FORWARD FACING CAMERA FRENCH LITERATURE PACK FRESH AIR HEATER FRONT BUMPER GUARDS - GREY FRONT FOG LAMPS FRONT PARKING AID FRONT SPRING LOAD CLASS-F FRT FENDER GILL-BRUNEL FRT LWR VALENCE - BLACK GREECE GRILLE-BRUNEL HALEWOOD JAGUAR HALOGEN HEADLAMPS HTD/P-FOLD/MEM/APP.LIGHTS I/P FAC CNTRST-EBONY/EBONY INDIVIDUAL 2ND ROW SEATS IP-PVC KEYLESS START LAMINATED WNDSCRN LAMP "color"-CLEAR LANE DEPARTURE WARNING SYSTEM LOCKING WHEEL LUG NUT MFD SINGLE VIEW SCREEN NGI HEAD UNIT - HIGH PERIMETER ANTI-THEFT PIANO BLACK APPLIQUE POLARIS WHITE/FUJI WHITE POLLUTION SENSOR POWER 8-WAY DRIVER SEAT ADJUST POWER 8-WAY PASS SEAT ADJUST RAIN SENSOR-FRONT W/WPR REAR BUMPER GUARDS - GREY REAR END CONSOLE WITH VENTS REAR SEAT CENTER ARMREST REAR SEAT SKI STOWAGE REAR SPRING LOAD CLASS-M REAR VIEW CAMERA ROTARY GEAR SHIFT KNOB SE DYNAMIC PACKAGE SIDE DR TRDPLT-PLASTIC SILL MLDG-EXT SPORT STABILIZER BAR REAR - CLASS A STARTER SYSTEM - STOP/START STNG WHL - S BRANDED TAILGATE APPLIQUE-BRUNEL TERRAIN RESPONSE TINTED GLASS-COMPLETE TIPTRONIC GEARSHIFT TYRE PRESSURE MONITORING SYSTM UREA EMISSION TANK ",
    "engine": 2000,
    "fueltype": "Πετρέλαιο",
    "images": [
        "https://static2.car.gr/10068060_f_a.jpg",
        "https://static1.car.gr/10068060_f_b.jpg"
    ],
    "km": 3000,
    "postal_code": 15122,
    "price": 71500,
    "region": " ΑΤΤΙΚΗΣ",
    "release_date": "Mar 2017",
    "title": "Land Rover Range Rover Evoque 2.0 DIESEL CABRIO DYNAMIC '17 - 71.500 EUR - Car.gr",
    "transmission": "Αυτόματο",
    "url": "https://www.car.gr/10068060"
}
```

### Search Results

* `GET /api/search?{search params}` - Get search results


#### Parameters
|Name   |Type   |Description   |
|---|---|---|
|format   |string   |Output format csv or json. Default is json  |

The `search params` are the same that one sees when does a search on the official: 
For example:
`GET /api/search?fs=1&condition=Μεταχειρισμένο&offer_type=sale&price-from=>60000&mileage-to=<5000&rg=3`
will return the following json which is backed by [these](https://www.car.gr/classifieds/cars/?fs=1&condition=%CE%9C%CE%B5%CF%84%CE%B1%CF%87%CE%B5%CE%B9%CF%81%CE%B9%CF%83%CE%BC%CE%AD%CE%BD%CE%BF&offer_type=sale&price-from=%3E60000&mileage-to=%3C5000&rg=3) search results.

#### Response

```json
[
    {
        "bhp": 163,
        "car_id": 8468307,
        "city": "ΒΟΥΛΑ ΑΤΤΙΚΗΣ ",
        "color": "Μαύρο (Μεταλλικό)",
        "description": "12ΘΕΣΙΟ, ΠΩΛΕΙΤΑΙ ΜΕ ΑΔΕΙΑ ΔΗΜΟΣΙΑΣ ΧΡΗΣΕΩΣ Ε.Ο.Τ. ΜΕ ΗΛΕΚΤΡΟΝΙΚΟ ΣΗΜΑ. ΟΛΟΚΑΙΝΟΥΡΙΟ ΣΕ ΑΡΙΣΤΗ ΚΑΤΑΣΤΑΣΗ, ΕΤΟΙΜΟ ΓΙΑ ΕΠΑΓΓΕΛΜΑΤΙΚΗ ΧΡΗΣΗ. ΣΥΣΤΗΜΑ ΠΑΡΑΚΟΛΟΥΘΗΣΗΣ ΟΔΗΓΟΥ ΜΕ ΚΑΜΕΡΑ + ΙΝΤΕΡΝΕΤ TV",
        "engine": 2143,
        "fueltype": "Πετρέλαιο",
        "images": [
        "https://static2.car.gr/8468307_u_b.jpg",
        "https://static1.car.gr/8468307_x_b.jpg",
        "https://static2.car.gr/8468307_y_b.jpg",
        "https://static1.car.gr/8468307_z_b.jpg"
        ],
        "km": 2000,
        "postal_code": 16673,
        "price": 64999,
        "region": null,
        "release_date": "Oct 2015",
        "title": "Mercedes-Benz Sprinter 316 BLUETEC VIP '15 - 64.999 EUR - Car.gr",
        "transmission": "Χειροκίνητο",
        "url": "https://www.car.gr/8468307"
    },
    
    {
        "bhp": 525,
        "car_id": 7278070,
        "city": "ΚΑΛΥΒΙΑ ΘΟΡΙΚΟΥ-ΛΑΓΟΝΗΣΙ ΑΤΤΙΚΗΣ ",
        "color": "Ασπρο ",
        "description": "*ΕΛΛΗΝΙΚΗΣ ΑΝΤΙΠΡΟΣΩΠΕΙΑΣ <31/07/2009> *ΕΝΑΣ ΙΔΙΟΚΤΗΤΗΣ *ΜΟΝΙΜΑ ΣΕ ΓΚΑΡΑΖ *ΟΧΙ ΚΑΠΝΙΣΤΗΣ *ΚΕΡΑΜΙΚΑ ΦΡΕΝΑ!!!!!!!!!!!! *CARBON PACKET *R-Tronic *SPOR ΚΑΘΙΣΜΑΤΑ *ΧΕΝΟΝ *ΝΑVIGATION SYSTEM *B&O SOUND SYSTEM *AΡΙΣΤΟ & ΑΒΑΦΟ !!! *ΒΙΒΛΙΟ ΣΕΡΒΙΣ *ΠΡΟΣΦΑΤΟ SERVICE *T.K.2017: 1380euro ΠΛΗΡΩΜΕΝΑ!!! *ΔΥΝΑΤΟΤΗΤΑ ΕΠΕΚΤΑΣΗ ΕΓΓΥΗΣΗΣ ΓΙΑ 1 ΧΡΟΝΟ!!!!!!! **ΤΙΜΗ ΓΕΡΜΑΝΙΑΣ ΧΩΡΙΣ ΚΕΡΑΜΙΚΑ ΦΡΕΝΑ &11,000χλμ 99.980 ευρω### & EURO 4..........### ###ΠΑΡΑΚΑΛΩ ΠΟΛΥ ΜΟΝΟ ΣΟΒΑΡΕΣ ΠΡΟΤΑΣΕΙΣ .!!##",
        "engine": 5204,
        "fueltype": "Βενζίνη",
        "images": [
        "https://static1.car.gr/7278070_8_a.jpg",
        "https://static2.car.gr/7278070_8_b.jpg",
        "https://static1.car.gr/7278070_9_b.jpg",
        "https://static1.car.gr/7278070_e_b.jpg"
        ],
        "km": 4798,
        "postal_code": 19010,
        "price": 164990,
        "region": " ΑΤΤΙΚΗΣ",
        "release_date": "Jul 2009",
        "title": "Audi R8 V10-R tronic KERAMIC-EΛΛΗΝΙΚΟ! '09 - 164.990 EUR - Car.gr",
        "transmission": "Αυτόματο",
        "url": "https://www.car.gr/7278070"
    },
    
    {
        "bhp": 180,
        "car_id": 9831555,
        "city": "ΑΓΙΟΣ ΔΗΜΗΤΡΙΟΣ ΑΤΤΙΚΗΣ ",
        "color": "Γκρι (Μεταλλικό)",
        "description": "ΤΙΜΗ ΓΕΡΜΑΝΙΑΣ-ΠΑΡΑΛΑΜΒΑΝΟΥΜΕ ΑΠΟ ΤΑ ΠΡΩΤΑ ΑΥΤ/ΤΑ ΠΑΡΑΓΩΓΗΣ ΠΕΡΙΠΟΥ ΙΟΥΝΙΟΣ",
        "engine": 2000,
        "fueltype": "Πετρέλαιο",
        "images": [
        "https://static1.car.gr/9831555_2_b.jpg",
        "https://static2.car.gr/9831555_3_b.jpg",
        "https://static1.car.gr/9831555_6_b.jpg",
        "https://static1.car.gr/9831555_8_b.jpg"
        ],
        "km": null,
        "postal_code": 17343,
        "price": 64000,
        "region": " ΑΤΤΙΚΗΣ",
        "release_date": "Jun 2017",
        "title": "Land Rover Range Rover velar '17 - 64.000 EUR - Car.gr",
        "transmission": "Αυτόματο",
        "url": "https://www.car.gr/9831555"
    },
    
    {
        "bhp": 215,
        "car_id": 9952220,
        "city": "ΜΑΡΟΥΣΙ ΑΤΤΙΚΗΣ ",
        "color": "Μαύρο (Μεταλλικό)",
        "description": "Οriginal m3 215hp matching number χωρίς καμία μετατροπή,μαύρο μεταλλικό με μαύρο δερμάτινο σαλόνι Bucket Recaro,δερμάτινο τιμόνι Μ Τech ΙΙ,5άρι σασμάν dog leg,abs,16άρες ζάντες,Clima,Βordcomputer,καθαριστήρες στα φανάρια,σε υπεράριστη κατάσταση απο γενικό Service με φρένα-λάδια-αμορτισέρ-φίλτρα-όλα τα υγρά-μπαταρία-silent blocks,με πάρα πολλά τιμόλόγια και αποδείξεις,χωρίς σκουριές,ένα απο τα 1519 m3 με 215hp, 181 Diamantschwarz Metallic 0203 Schwarz Leder 361 Ausstellfenster, Hinten 410 Window Lifts, Electric At Front 500 Headlight Washer Sys/intensive Cleaning 510 Headlight Beam-throw Contr. F Low Beam 530 Air Conditioning 551 On-board Computer Ii W Remote Control 813 France Version 854 Language Version French 925 Shipping Protection Package",
        "engine": 2300,
        "fueltype": "Βενζίνη",
        "images": [
        "https://static2.car.gr/9952220_1_a.jpg",
        "https://static1.car.gr/9952220_1_b.jpg",
        "https://static1.car.gr/9952220_3_b.jpg",
        "https://static1.car.gr/9952220_U_b.jpg"
        ],
        "km": null,
        "postal_code": 15122,
        "price": 75000,
        "region": " ΑΤΤΙΚΗΣ",
        "release_date": "Dec 1990",
        "title": "Bmw M3 Ε30 215HP ΓΝΗΣΙΟ '90 - 75.000 EUR - Car.gr",
        "transmission": "Χειροκίνητο",
        "url": "https://www.car.gr/9952220"
    },
    
    {
        "bhp": 190,
        "car_id": 8969973,
        "city": "ΜΑΡΟΥΣΙ ΑΤΤΙΚΗΣ ",
        "color": "Μαύρο ",
        "description": "Mπέζ δερμάτινο σαλόνι καινούριο,μπέζ μοκέτες και πατάκια,15άρες ζάντες αλουμινίου Campagnolo,5άρι σασμάν,δερμάτινο τιμόνι,ηχοσύστημα Blaupunkt με equalizer,ηλεκτρικά παράθυρα,φιμέ τζάμια,άψογα συντηρημένο,χωρίς σκουριές και διαρροές,Ital Design by Giugiaro,matching numbers,Ελληνικής αντιπροσωπείας",
        "engine": 3000,
        "fueltype": "Βενζίνη",
        "images": [
        "https://static1.car.gr/8969973_h_a.jpg",
        "https://static2.car.gr/8969973_h_b.jpg",
        "https://static1.car.gr/8969973_c_b.jpg",
        "https://static2.car.gr/8969973_d_b.jpg"
        ],
        "km": null,
        "postal_code": 15122,
        "price": 69900,
        "region": " ΑΤΤΙΚΗΣ",
        "release_date": "Jun 1975",
        "title": "Maserati Merak 3.0 V6 EΛΛΗΝΙΚO '75 - 69.900 EUR - Car.gr",
        "transmission": "Χειροκίνητο",
        "url": "https://www.car.gr/8969973"
    },
    
    {
        "bhp": 340,
        "car_id": 8602260,
        "city": "ΜΑΡΟΥΣΙ ΑΤΤΙΚΗΣ ",
        "color": "Γκρι (Μεταλλικό)",
        "description": "Stratus Grau με Camel Jet καφέ δερμάτινο ηλεκτρικό σαλόνι θερμαινόμενο Sport-Performance,καφέ ζώνες,20άρες ζάντες Turbinen Design,δερμάτινο ταμπλώ,ηλεκτρικό τιμόνι πολλαπλών θερμαινόμενο με αλλαγές ταχυτήτων,Navigation Touchscreen Europa,Bi-Xenon φώτα με Led προσαρμοζόμενα και πλυστικά φανών,αναδιπλούμενοι καθρέφτες,τηλέφωνο,esp,pdc μπροστά και πίσω,διζωνικός κλιματισμός,Premium Sound System,Chrom Roll Bars,ανεμοθραύστη,6a/b,cd,8άρι αυτόματο-σειριακό σασμάν,αυτόματο πιλότο με έλεγχο απόστασης,αισθητήρες βροχής και φώτων,6δισκο cd,Keyless-Go,Bordcomputer,επένδυση αλουμινίου Graphitgrau,Active Sport εξατμίσεις με ηλεκτρικά κλαπέτα,ηλεκτρική κουκούλα,εσωτερικό πακέτο φωτισμού Phosphor-Blue,Performance Packet,Ιntelligent Start-Stop Button,Blind Spot Assist,Performance Brake Programme,V6 Kompressor,1o xέρι,oλοκαίνουριο,Ελληνικό μενού 20X9 FR / 20X10.5 RR TURBINE 255/35 FRT/295/30 RR ZR20 ACTIVE SPORT EXHAUST AIR PURIFIER SENSOR AMBIENT LIGHTING ANALOGUE AUDIO BROADCAST AUTO DIMMING REAR VIEW MIRROR BLUETOOTH CONNECTIVITY BONNET LOUVRE - BLACK BRAKE CALIPERS-SILVER BREAKDOWN WARNING SIGN TYPE 1 BUMPERS-SPORTY (BDY COLOUR) CASTLE BROMWICH CCR CENTRE CONSOLE CCR COCKPIT CCR DOOR CASING CCR FRONT BUMPERS CCR FRONT DAMPER CCR HEADLINING CCR REAR BUMPERS CCR REAR DAMPER CCR SEATS CENTRE CONSOLE - BASE CUBBY BOX DRIVER HEATED SEAT DRIVERS SEAT - PERFORMANCE "engine" POWER SOURCE - 340/450 ENHANCED SOUND SYSTEM (L2) EXTENDED LEATHER DR TRIM PANEL FIRST AID KIT FRONT PARKING AID FRT FENDER GILL-BRIGHT GRILLE-BLK MESH/CHROME SRD HEATED PASSENGER SEAT I/P INSERT-LIGHT ALUMINIUM INSTR CLUS SURR-SATIN CHROME INT JET CARPET-LR INTERIOR GRAPHITE TRIM ACCENT INTERIOR SWITCH FINISH BLACK INTERIOR TRIM - MORZINE JAGUAR SEAT BELT - CAMEL JDO WINTER MODE/DYN MODE (DNU) JET HEADLINER KEYLESS START KIT TYRE REPAIR - HIGH LINE LANE CHANGE MERGE AID LEATHER CONSOLE MATERIAL LEATHER FACIA 6-WAY DRV SEAT ADJUSTER 6-WAY PASS SEAT ADJUST MID SERIES ANALOG CLUSTER NAVIGATION CENTER - EUROPE PADDLESHIFT - BLACK(DNU) PASS SEAT - PERFORMANCE PERFORMANCE BRAKE PROGRAMME RAIN SENSOR-FRONT W/WPR REVERSE PARKING AID ROLL BAR - SATIN CHROME RR BUMPER-SPORTY (BODY COLOUR) SIDE DR TRDPLT-METAL SPEED CONTROL SPORTS EXHAUST TONE STARTER - STOP/START SYSTEM STNG WHL-LEATHER WRAPPED STRG COLM-POWER TILT/RACK SUNVISOR,SINGLE-DRIVER VIN CODE F XENON HEADLAMPS",
        "engine": 3000,
        "fueltype": "Βενζίνη",
        "images": [
        "https://static2.car.gr/8602260_z_a.jpg",
        "https://static1.car.gr/8602260_j_b.jpg",
        "https://static1.car.gr/8602260_F_b.jpg",
        "https://static2.car.gr/8602260_S_b.jpg"
        ],
        "km": 4900,
        "postal_code": 15122,
        "price": 89000,
        "region": " ΑΤΤΙΚΗΣ",
        "release_date": "Oct 2014",
        "title": "Jaguar F-Type CABRIO 3.0 PERFORMANCE PACKET '14 - 89.000 EUR - Car.gr",
        "transmission": "Αυτόματο",
        "url": "https://www.car.gr/8602260"
    },
    
    {
        "bhp": 180,
        "car_id": 10068060,
        "city": "ΜΑΡΟΥΣΙ ΑΤΤΙΚΗΣ ",
        "color": "Ασπρο ",
        "description": "Δερμάτινο ηλεκτρικό σαλόνι 8 κινήσεων με μνήμες,Premium Leder,δερμάτινο ταμπλώ,ηλεκτρική κουκούλα,δερμάτινο τιμόνι πολλαπλών με αλλαγές ταχυτήτων,20άρες ζάντες,οθόνη Touchscreen,CD-DVD Player,Pdc με Camera Surround εμπρός και πίσω,αναδιπλούμενοι αντιθαμβωτικοί καθρέφτες,φιμέ τζάμια,Bluetooth,επένδυση Piano Black,Keyless-go,αισθητήρες βροχής και φώτων,έξοδος usb-i pod,έλεγχο πίεσης ελαστικών,αυτόματο-σειριακό 9άρι σασμάν,Bordcomputer,Cruise Control,Isofix,εργοστασιακό συναγερμό,Terrain Response,σύστημα αποφυγής σύγκρουσης,Start-Stop System,Skisack,έλεγχος αλλαγής λωρίδας,Meridian ηχοσύστημα,Dynamic Packet,1o χέρι,Ελληνικής αντιπροσωπείας με εργοστασιακή εγγύηση FUJI WHITE FINISHER-ETCHED ALUMINIUM 20 X 8 STYLE 16 SPARKLE SILVER 245/45R20 V RATED ALL TERRAIN PROGRESS CONTROL AMBIENT LIGHTING ANALOGUE AUDIO BROADCAST APPROACH LIGHTS AUTO DIMMING REAR VIEW MIRROR AUTOMATIC HEADLAMP ON/OFF BADGE-EVOQUE - RED BLUETOOTH-AUDIO STREAM BMPR LWR VLC-BLACK BRANDED SOUND SYS (L3) BUMPER APERTUR - BRUNEL STYLE2 BUMPERS-PLASTIC-SPORT CD/DVD PLAYER CONSOLE LID-LEATHER CONTINENTAL TYRE VENDOR CRUISE CONTROL DISC BRAKE SIZE 17 DOOR HANDLES-BODY "color" DR ARMREST-LEATHER DR TRIM PNL-PVC/TAURUS DR/PAS ELEC FRT ST ADJ/MEMORY EBONY HEADLINER EFFICIENT DRIVELINE ELECTRONIC LOCKABLE GLOVEBOX "engine" POWER SOURCE - 180/430 EU6B + DPF EMISSION EXT MIRROR FINISH-BLACK FAMILY BADGE-LANDROVER ATLAS FLOOR MATS-CARPET FOOT PEDAL - METALIC FINISH FORWARD FACING CAMERA FRENCH LITERATURE PACK FRESH AIR HEATER FRONT BUMPER GUARDS - GREY FRONT FOG LAMPS FRONT PARKING AID FRONT SPRING LOAD CLASS-F FRT FENDER GILL-BRUNEL FRT LWR VALENCE - BLACK GREECE GRILLE-BRUNEL HALEWOOD JAGUAR HALOGEN HEADLAMPS HTD/P-FOLD/MEM/APP.LIGHTS I/P FAC CNTRST-EBONY/EBONY INDIVIDUAL 2ND ROW SEATS IP-PVC KEYLESS START LAMINATED WNDSCRN LAMP "color"-CLEAR LANE DEPARTURE WARNING SYSTEM LOCKING WHEEL LUG NUT MFD SINGLE VIEW SCREEN NGI HEAD UNIT - HIGH PERIMETER ANTI-THEFT PIANO BLACK APPLIQUE POLARIS WHITE/FUJI WHITE POLLUTION SENSOR POWER 8-WAY DRIVER SEAT ADJUST POWER 8-WAY PASS SEAT ADJUST RAIN SENSOR-FRONT W/WPR REAR BUMPER GUARDS - GREY REAR END CONSOLE WITH VENTS REAR SEAT CENTER ARMREST REAR SEAT SKI STOWAGE REAR SPRING LOAD CLASS-M REAR VIEW CAMERA ROTARY GEAR SHIFT KNOB SE DYNAMIC PACKAGE SIDE DR TRDPLT-PLASTIC SILL MLDG-EXT SPORT STABILIZER BAR REAR - CLASS A STARTER SYSTEM - STOP/START STNG WHL - S BRANDED TAILGATE APPLIQUE-BRUNEL TERRAIN RESPONSE TINTED GLASS-COMPLETE TIPTRONIC GEARSHIFT TYRE PRESSURE MONITORING SYSTM UREA EMISSION TANK ",
        "engine": 2000,
        "fueltype": "Πετρέλαιο",
        "images": [
        "https://static2.car.gr/10068060_f_a.jpg",
        "https://static2.car.gr/10068060_k_b.jpg",
        "https://static1.car.gr/10068060_2_b.jpg",
        "https://static1.car.gr/10068060_d_b.jpg",
        "https://static2.car.gr/10068060_e_b.jpg",
        "https://static1.car.gr/10068060_l_b.jpg"
        ],
        "km": 3000,
        "postal_code": 15122,
        "price": 71500,
        "region": " ΑΤΤΙΚΗΣ",
        "release_date": "Mar 2017",
        "title": "Land Rover Range Rover Evoque 2.0 DIESEL CABRIO DYNAMIC '17 - 71.500 EUR - Car.gr",
        "transmission": "Αυτόματο",
        "url": "https://www.car.gr/10068060"
    },
    
    {
        "bhp": 218,
        "car_id": 8955547,
        "city": "ΑΘΗΝΑ ΑΤΤΙΚΗΣ ",
        "color": "Ασπρο (Μεταλλικό)",
        "description": "ΓΙΑ ΡΑΝΤΕΒΟΥ ΜΟΝΟ ΚΑΤΟΠΙΝ ΣΥΝΝΕΝΟΗΣΗΣ ΔΕΧΟΜΑΣΤΕ ΠΑΡΑΓΓΕΛΙΕΣ ΑΥΤΟΚΙΝΗΤΩΝ ΣΕ ΔΙΑΣΤΗΜΑ 7-10 ΗΜΕΡΩΝ ΠΑΡΑΔΙΔΟΥΜΕ ΟΠΟΙΟΔΗΠΟΤΕ ΜΟΝΤΕΛΟ BMW ΑΝΑΛΑΜΒΑΝΟΥΜΕ ΤΗΝ ΠΡΟΩΘΗΣΗ ΠΡΟΒΟΛΗ ΔΙΑΦΗΜΙΣΗ ΤΟΥ ΑΥΤΟΚΙΝΗΤΟΥ ΣΑΣ ΣΕ ΟΛΟ ΤΟΝ ΚΟΣΜΟ TΙΜΗ ΜΕΤΡΗΤΟΙΣ 69750 ΕΥΡΩ ΤΙΜΗ ΑΝΤΑΛΛΑΓΗΣ 74900 ΕΥΡΩ BMW X5 DIESEL 25D 1995 CC EURO 6 ΑΥΤΟΜΑΤΟ ΠΑΝΟΡΑΜΙΚΗ ΟΡΟΦΗ HEAD UP DISPLAY ΥΠΕΡΥΘΡΕΣ ΕΝΔΕΙΞΕΙΣ ΣΤΟ ΠΑΡΜΠΡΙΖ XENON NAVIGATION ΤΗΛΕΦΩΝΟ ΑΤΡΑΚΑΡΙΣΤΟ ΠΛΗΡΕΣ ΣΕΡΒΙΣ ΕΡΓΟΣΤΑΣΙΑΚΗ ΕΓΓΥΗΣΗ ΚΑΤΑΣΤΑΣΗ ΚΑΙΝΟΥΡΓΙΟΥ ΑΥΤΟΚΙΝΗΤΟΥ ",
        "engine": 1995,
        "fueltype": "Πετρέλαιο",
        "images": [
        "https://static1.car.gr/8955547_0_a.jpg",
        "https://static2.car.gr/8955547_0_b.jpg",
        "https://static2.car.gr/8955547_2_b.jpg",
        "https://static1.car.gr/8955547_m_b.jpg",
        "https://static1.car.gr/8955547_q_b.jpg"
        ],
        "km": null,
        "postal_code": 11636,
        "price": 69750,
        "region": " ΑΤΤΙΚΗΣ",
        "release_date": "May 2014",
        "title": "Bmw X5 1995 CC Diesel Bosganas '14 - 69.750 EUR - Car.gr",
        "transmission": "Αυτόματο",
        "url": "https://www.car.gr/8955547"
    },
    
    {
        "bhp": 256,
        "car_id": 7887040,
        "city": "ΑΛΙΜΟΣ ΑΤΤΙΚΗΣ ",
        "color": "Μαύρο (Μεταλλικό)",
        "description": "ΤΟ ΑΜΑΞΙ ΔΙΑΘΕΤΕΙ ΤΑ ΕΞΗΣ: MAYΡΟ ΜΕΤΑΛΙΚΟ ΧΡΩΜΑ FACE LIFT AYTOBIOGRAPHY SPORT ΗΛΕΚΤΡΙΚΗ ΟΡΟΦΗ ΑΥΤΟΜΑΤΟ ΚΙΒΩΤΙΟ ΤΑΧΥΤΗΤΩΝ 8 COMAMAND SHIFT 2 ΜΟΝΙΜΗ ΤΕΤΡΑΚΙΝΗΣΗ CORNERING BRAKE CONTROL (CBC) ΗΛΕΚΤΡΟΝΙΚΑ ΕΛΕΓΧΟΜΕΝΗ ΑΕΡΑΝΑΡΤΗΣΗ ΗΛΕΚΤΡΙΚΑ ΦΡΕΝΑ FORCE DISTRIBUTION (EBD) Hill Descent Control HDC ΗΛΕΚΤΡΟΝΙΚΟ ΣΥΣΤΗΜΑ ΕΛΕΓΧΟΥ ΠΡΟΣΦΥΣΗΣ(ETC) ΣΥΣΤΗΜΑ ΕΙΣΟΔΟΥ Keyless Entry ΣΥΣΤΗΜΑ ΠΛΟΗΓΗΣΗΣ ΜΕ ΟΘΟΝΗ ΑΦΗΣ Multi Media Packet ΜΕ ΟΘΟΝΕΣ 10,2'' ΚΑΙ TΡΕΙΑ ΑΣΥΡΜΑΤΑ ΑΚΟΥΣΤΙΚΑ ΨΗΦΙΑΚΗ ΤΗΛΕΟΡΑΣΗ Η Hybrid TV Infotainment ΟΘΟΝΗ ΑΦΗΣ ΜΕ ΤΕΧΝΟΛΟΓΙΑ DUAL VIEW ΗΧΟΣΥΣΤΗΜΑ ΗΧΟΥ HARMAN/KARDON & 825 watt ΗΛΕΚΤΡΙΚΑ ΚΑΘΙΣΜΑΤΑ ΜΕ ΜΝΗΜΗ 20 ΕΠΙΠΕΔΑ ΟΔΗΓΟΥ & ΣΥΝΟΔΗΓΟΥ ΘΕΡΜΑΙΝΟΜΕΝΑ EΜΠΡΟΣ ΠΙΣΩ ΑΛΚΑΝΤΑΡΑ ΕΠΕΝΔΥΣΗ ΟΥΡΑΝΟΥ ΧΕΙΡΙΣΤΗΡΙΑ ΑΛΛΑΓΗΣ ΤΑΧΥΤΗΤΩΝ ΣΤΟ ΤΙΜΟΝΙ ΔΕΡΜΑΤΙΝΟ ΤΙΜΟΝΙ & ΘΕΡΜΑΙΝΟΜΕΝΟ ΤΙΜΟΝΙ ΠΟΛΛΑΠΛΩΝ ΛΕΙΤΟΥΡΓΙΩΝ ΣΥΣΤΗΜΑ ΠΕΡΙΜΕΤΡΙΚΗΣ ΚΑΜΕΡΑΣ Surround ΚΑΜΕΡΑ ΟΠΙΣΘΟΠΟΡΕΙΑΣ ΑΙΣΘΗΤΗΡΕΣ ΠΑΡΚΑΡΙΣΜΑΤΟΣ ΕΜΠΡΟΣ ΠΙΣΩ Adaptive cruise control ΜΕ ΕΙΔΟΠΟΙΗΣΗ ΑΠΟΣΤΑΣΗΣ ΠΑΚΕΤΟ ΑΝΑΓΝΩΡΙΣΗΣ ΟΔΙΚΩΝ ΣΗΜΑΤΩΝ ΚΑΙ ΠΡΟΕΙΔΟΠΟΙΗΣΗ ΑΛΛΑΓΗΣ ΛΩΡΙΔΑΣ Soft-Close ΠΟΡΤΩΝ Adaptive ΠΡΟΒΟΛΕΙΣ Xenon ΜΕ LED ΥΠΟΓΡΑΦΗ ΦΩΤΑ ΗΜΕΡΑΣ ΦΩΤΑ ΕΚΤΑΚΤΟΥ ΑΝΑΓΚΗΣ ΕΣΩΤΕΡΙΚΟΣ ΑΤΜΟΣΦΑΙΡΙΚΟΣ ΦΩΤΙΣΜΟΣ ΗΛΕΚΤΡΙΚΟΙ ΚΑΘΡΕΦΤΕΣ & ΑΝΑΔΙΠΛΟΥΜΕΝΟΙ & ΘΕΡΜΑΙΝΟΜΕΝΟΙ & ΑΝΤΙΘΑΜΒΩΤΙΚΗ ΛΕΙΤΟΥΡΓΙΑ ΑΥΤΟΜΑΤΟ ΣΥΣΤΗΜΑ ΚΛΙΜΑΤΙΣΜΟΥ ΘΕΡΜΑΝΣΗ ΜΕ ΤΗΛΕΧΕΙΡΙΣΜΟ ΣΥΣΤΗΜΑ ΠΡΟΕΙΔΟΠΟΙΗΣΗΣ ΣΥΓΚΡΟΥΣΗΣ ΠΛΗΡΗΣ ΠΑΚΕΤΟ ΑΕΡΟΣΑΚΩΝ ΣΚΟΥΡΑ ΦΙΜΕ ΤΖΑΜΙΑ ΠΙΣΩ ΖΑΝΤΕΣ ΑΛΟΥΜΙΝΙΟΥ 20'' ΠΑΚΕΤΟ ΠΟΡΤ ΜΠΑΓΑΖ ΣΥΣΤΗΜΑ ΠΑΡΑΚΟΛΟΥΘΗΣΗΣ ΠΙΕΣΗΣ ΕΛΑΣΤΙΚΩΝ TPMS ΣΥΣΤΗΜΑ ΠΛΥΣΗΣ ΠΡΟΒΟΛΕΩΝ LED ΗΜΕΡΑΣ ΥΑΛΟΚΑΘΑΡΙΣΤΗΡΕΣ ΜΕ ΑΙΣΘΗΤΗΡΑ ΒΡΟΧΗΣ ΦΩΤΕΙΝΑ ΜΑΡΣΠΙΕ ΑΛΟΥΜΙΝΙΟΥ Range Rover ΕΛΕΓΧΟΥ ΕΥΣΤΑΘΕΙΑΣ Trailer ΦΡΕΝΑ BREBO DYNAMIC DRIVING ALARM ΦΡΕΝΩΝ ΕΚΤΑΚΤΗΣ ΑΝΑΓΚΗΣ ΚΑΛΥΜΜΑ ΚΑΘΡΕΠΤΩΝ ΜΑΥΡΟ ΜΕΤΑΛΛΙΚΟ ΠΛΗΡΗΣ ΙΣΤΟΡΙΚΟ ΣΥΝΤΗΡΗΣΗΣ ΑΝΑΛΑΜΒΑΝΟΥΜΕ ΓΙΑ ΟΛΑ ΤΑ ΕΙΔΗ ΑΥΤΟΚΙΝΗΤΩΝ ΠΑΡΑΓΓΕΛΙΕΣ ΣΤΙΣ ΚΑΛΥΤΕΡΕΣ ΤΙΜΕΣ ΤΗΣ ΑΓΟΡΑΣ ΜΕ ΠΑΡΑΔΟΣΗ ΕΝΤΟΣ ΠΕΝΤΕ ΗΜΕΡΩΝ ΑΠΟ ΤΙΣ ΕΓΚΑΤΑΣΤΑΣΕΙΣ ΜΑΣ ΣΤΗΝ ΓΕΡΜΑΝΙΑ ΣΤΗΝ ΝΥΡΕΜΒΕΡΓΗ",
        "engine": 3000,
        "fueltype": "Πετρέλαιο",
        "images": [
        "https://static2.car.gr/7887040_r_a.jpg",
        "https://static1.car.gr/7887040_0_b.jpg",
        "https://static1.car.gr/7887040_2_b.jpg",
        "https://static1.car.gr/7887040_X_b.jpg"
        ],
        "km": null,
        "postal_code": 17455,
        "price": 82500,
        "region": " ΑΤΤΙΚΗΣ",
        "release_date": "Jul 2012",
        "title": "Land Rover Range Rover Sport AUTOBIOGRAPHY TV SUNROOF '12 - 82.500 EUR - Car.gr",
        "transmission": "Αυτόματο",
        "url": "https://www.car.gr/7887040"
    },
    
    {
        "bhp": 380,
        "car_id": 10061412,
        "city": "ΑΘΗΝΑ ΑΤΤΙΚΗΣ ",
        "color": "Μαύρο (Μεταλλικό)",
        "description": "ΕΛΛΗΝΙΚΟ FULL EXTRA ΤΙΜΗ ΚΑΙΝΟΥΡΓΙΟΥ 87.714€",
        "engine": 3000,
        "fueltype": "Βενζίνη",
        "images": [
        "https://static2.car.gr/10061412_0_a.jpg",
        "https://static1.car.gr/10061412_2_b.jpg",
        "https://static2.car.gr/10061412_3_b.jpg",
        "https://static1.car.gr/10061412_4_b.jpg"
        ],
        "km": 2200,
        "postal_code": 10443,
        "price": 75000,
        "region": " ΑΤΤΙΚΗΣ",
        "release_date": "Oct 2016",
        "title": "Bmw M2 '16 - 75.000 EUR - Car.gr",
        "transmission": "Αυτόματο",
        "url": "https://www.car.gr/10061412"
    },
    
    {
        "bhp": null,
        "car_id": 9705399,
        "city": "ΚΗΦΙΣΙΑ ΑΤΤΙΚΗΣ ",
        "color": "Ασημί (Μεταλλικό)",
        "description": "Ατρακαριστο ! Ολα τα service στην Bentley ! ",
        "engine": 6000,
        "fueltype": "Βενζίνη",
        "images": [
        "https://static2.car.gr/9705399_0_a.jpg",
        "https://static1.car.gr/9705399_z_b.jpg",
        "https://static1.car.gr/9705399_h_b.jpg",
        "https://static2.car.gr/9705399_i_b.jpg",
        "https://static2.car.gr/9705399_C_b.jpg"
        ],
        "km": null,
        "postal_code": 14561,
        "price": 75000,
        "region": null,
        "release_date": "Nov 2007",
        "title": "Bentley Continental W12 twin turbo '07 - 75.000 EUR - Car.gr",
        "transmission": "Αυτόματο",
        "url": "https://www.car.gr/9705399"
    },
    
    {
        "bhp": 245,
        "car_id": 7734404,
        "city": "ΠΕΡΙΣΤΕΡΙ ΑΤΤΙΚΗΣ ",
        "color": "Ασπρο (Μεταλλικό)",
        "description": "# ΤΙΜΗ ΑΝΤΑΛΛΑΓΗΣ 69.000Ε # *DIESEL *ENAΣ ΙΔΙΟΚΤΗΤΗΣ *ΑΒΑΦΟ/ΑΤΡΑΚΑΡΙΣΤΟ *ΠΛΗΡΗΣ ΑΕΡΑΝΑΡΤΗΣΗ ΜΕ ΕΠΙΛΟΓΕΣ ΥΨΟΥΣ-ΧΑΜΗΛΩΜΑΤΟΣ ΚΑΙ ΕΠΙΛΟΓΕΣ normal,comfort,sport. *ΠΑΝΟΡΑΜΙΚΗ ΓΥΑΛΙΝΗ ΟΡΟΦΗ *ΕΠΙΛΟΓΕΑΣ ΔΙΑΦΟΡΙΚΩΝ ANΩΜΑΛΟΥ ΔΡΟΜΟΥ ΚΛΠ *ΑΙΣΘΗΤΗΡΕΣ ΒΡΟΧΗΣ-ΦΩΤΩΝ *ΟΘΟΝΗ PASM ΜΕ ΕΝΣΩΜΑΤΟΜΕΝΟ ΤΗΛΕΦΩΝΟ,NAVI,DVD,ΔΟΡΥΦΟΡΙΚΟ *ΗΧΟΣΥΣΤΗΜΑ BOSE ΜΕ 14 ΗΧΕΙΑ ΚΑΙ SUBWOOFER ΜΕ ΕΝΙΣΧΥΤΗ *ΕΡΓΟΣΤΑΣΙΑΚΗ ΚΑΜΕΡΑ ΟΠΙΣΘΟΠΟΡΕΙΑΣ *ΕΡΓΟΣΤΑΣΙΑΚΟ PARKTRONIC ΕΜΠΡΟΣ ΠΙΣΩ *ΕΡΓΟΣΤΑΣΙΑΚΟΣ ΗΛΕΚΤΡΙΚΟΣ ΚΟΤΣΑΔΟΡΟΣ ΑΝΑΔΙΠΛΟΥΜΕΝΟΣ *ΕΚΤΕΤΑΜΕΝΟ ΠΑΚΕΤΟ ΔΕΡΜΑΤΟΣ ( ΡΑΜΜΕΝΟ ΣΤΟ ΧΕΡΙ ) ΣΕ ΤΑΜΠΛΟ ΚΑΙ ΠΟΡΤΕΣ LEATHER PACKET PORSCHE *HΛΕΚΤΡΟΥΔΡΑΥΛΙΚΟ ΠΟΡΤ ΜΠΑΓΚΑΖ ( ΛΕΙΤΟΥΡΓΕΙ ΚΑΙ ΑΠΟ ΤΟ ΧΕΙΡΙΣΤΗΡΙΟ ) *ZANTEΣ ΑΛΛΟΥΜΙΝΙΟΥ ΕΛΑΦΡΟΥ ΚΡΑΜΑΤΟΣ 21'' ΚΑΙΝΟΥΡΙΕΣ ΑΠΟ ΤΗΝ PORSCHE ΜΕ ΟΛΟΚΑΙΝΟΥΡΙΑ ΕΛΑΣΤΙΚΑ ΧΡΗΣΗΣ 4000 ΧΛΜ *ΔΙΑΚΟΣΜΗΤΙΚΟ ΠΑΚΕΤΟ ΑΛΟΥΜΙΝΙΟΥ ΜΕΣΑ ΕΞΩ ΜΕ ΜΠΑΡΕΣ ΟΡΟΦΗΣ *ΤΙΜΟΝΙ ΠΟΛΛΑΠΛΩΝ ΛΕΙΤΟΥΡΓΙΩΝ ΚΑΙ ΧΡΗΣΗ F1 *ΤΕΤΡΑΖΩΝΙΚΟΣ ΚΛΙΜΑΤΙΣΜΟΣ THERMOTRONIC 4ZONES *HΛΕΚΤΡΙΚΑ ΚΑΘΙΣΜΑΤΑ ΜΕ ΟΛΕΣ ΤΙΣ ΛΕΙΤΟΥΡΓΙΕΣ *ΣΥΣΤΗΜΑ START-STOP ECONOMY *ΕΡΓΟΣΤΑΣΙΑΚΑ ΦΩΤΑ ΒΙ-XENON ADAPTIVE ΜΕ ΠΛΥΣΤΙΚΟ ΣΥΣΤΗΜΑ ΚΑΙ ΣΥΣΤΗΜΑ ΑΝΟΙΓΜΑ ΠΡΟΒΟΛΕΑ ΚΑΤΑ ΤΟ ΣΤΡΙΨΙΜΟ *ΕΡΓΟΣΤΑΣΙΑΚΟΣ ΣΥΝΑΓΕΡΜΟΣ KAI GPS PORSCHE *ΕΡΓΟΣΤΑΣΙΑΚΟΣ ΚΟΤΣΑΔΟΡΟΣ *ΠΑΚΕΤΟ ΦΩΤΙΣΜΟΥ ΜΕ ΔΙΑΚΟΣΜΗΤΙΚΟ ΚΡΥΦΟ ΦΩΤΙΣΜΟ ΚΑΤΑ ΤΗΝ ΟΔΗΓΗΣΗ *ΟΛΑ ΤΑ SERVICE ΜΕΣΑ ΣΕ ΑΝΤΙΠΡΟΣΩΠΕΙΑ ΜΕ ΠΛΗΡΕΣ ΙΣΤΟΡΙΚΟ ΣΥΝΤΗΡΗΣΗΣ * ΟΛΟΚΑΙΝΟΥΡΓΙΑ ΛΑΣΤΙΧΑ 2016 * ΜΕΓΑΛΟ ΣΕΡΒΙΣ 2016 # ΤΙΜΗ ΑΝΤΑΛΛΑΓΗΣ 69.000Ε #",
        "engine": 3000,
        "fueltype": "Πετρέλαιο",
        "images": [
        "https://static2.car.gr/7734404_3_a.jpg",
        "https://static1.car.gr/7734404_y_b.jpg",
        "https://static2.car.gr/7734404_z_b.jpg",
        "https://static1.car.gr/7734404_C_b.jpg",
        "https:https://storage.car.gr/userphotos/9018/c8b89.jpg"
        ],
        "km": 135,
        "postal_code": 12132,
        "price": 67500,
        "region": " ΑΤΤΙΚΗΣ",
        "release_date": "Jan 2011",
        "title": "Porsche Cayenne DIESEL - TIPTRONIC '11 - 67.500 EUR - Car.gr",
        "transmission": "Αυτόματο",
        "url": "https://www.car.gr/7734404"
    },
    
    {
        "bhp": 252,
        "car_id": 5583878,
        "city": "ΒΡΙΛΗΣΣΙΑ ΑΤΤΙΚΗΣ ",
        "color": "Ασπρο (Μεταλλικό)",
        "description": "ΣΕ ΚΑΤΑΣΤΑΣΗ ΒΙΤΡΙΝΑΣ *ΕΛΛΗΝΙΚΟ ΜΕ ΒΙΒΛΙΟ SERVICE ΑΝΤΙΠΡΟΣΩΠΕΙΑΣ *ΓΡΑΠΤΗ ΕΓΓΥΗΣΗ ΑΝΑΓΡΑΦΟΜΕΝΩΝ ΧΙΛΙΟΜΕΤΡΩΝ *ΓΡΑΠΤΗ ΕΓΓΥΗΣΗ ΑΤΡΑΚΑΡΙΣΤΟΥ ΑΥΤΟΚΙΝΗΤΟΥ *SERVICE ΜΕ ΤΗΝ ΠΑΡΑΔΟΣΗ ΤΟΥ ΑΥΤΟΚΙΝΗΤΟΥ *ΒΙΟΛΟΓΙΚΟΣ ΚΑΘΑΡΙΣΜΟΣ -ΓΥΑΛΙΣΜΑ -ΚΕΡΩΜΑ *ΘΕΡΜΑΙΝΟΜΕΝΟΥΣ ΕΞΩΤΕΡΙΚΟΥΣ ΚΑΘΡΕΠΤΕΣ *ΑΥΤΟΜΑΤΟ ΣΥΣΤΗΜΑ ΠΛΥΣΗΣ ΠΡΟΒΟΛΕΩΝ *ΑΥΤΟΜΑΤΩΣ ΔΙΖΩΝΙΚΟΣ ΚΛΙΜΑΤΙΣΜΟΣ *ΦΙΜΕ ΤΖΑΜΙΑ *ΠΟΛΥΛΕΙΤΟΥΡΓΙΚΟ ΤΙΜΟΝΙ *PARKTRONIC *CRUISE CONTROL *YΠΟΒΡΑΧΙΟΝΙΟ *TRIP CUMPUTER *ΕΡΓΟΣΤΑΣΙΑΚΟ ΗΧΟΣΥΣΤΗΜΑ ΜΕ NAVIGATION *USB-in , AUX-in , SC-Cards 2 Slots *BLUΕTOOTH *Sport Πακέτο AMG *Πακέτο Avantgarde *Navigation και σύστημα διασκέδασης πίσω επιβατών *Επένδυση Καρυδιάς *Θερμαινόμενα καθίσματα *Κάμερα οπισθοπορείας *Πολυλειτουργικό τιμόνι με Paddles αλλαγής ταχυτήτων *Πακέτο Μνημών στα καθίσματα *BlueTEC *Euro6 *ΠΡΟΣΒΑΣΗ ΜΕΣΩ ΑΤΤΙΚΗΣ ΟΔΟΥ Δυνατότητα χρηματοδότησης με επιτόκια από 8,50%, από 0% προκαταβολή και διάρκεια αποπληρωμής έως και 84 μήνες.",
        "engine": 3000,
        "fueltype": "Πετρέλαιο",
        "images": [
        "https://static1.car.gr/5583878_1_a.jpg",
        "https://static1.car.gr/5583878_b_b.jpg",
        "https://static2.car.gr/5583878_c_b.jpg",
        "https://static1.car.gr/5583878_d_b.jpg"
        ],
        "km": null,
        "postal_code": 15235,
        "price": 75000,
        "region": " ΑΤΤΙΚΗΣ",
        "release_date": "Jun 2014",
        "title": "Mercedes-Benz E 350 BlueTEC 4MATIC '14 - 75.000 EUR - Car.gr",
        "transmission": "Αυτόματο",
        "url": "https://www.car.gr/5583878"
    },
    
    {
        "bhp": 600,
        "car_id": 9484875,
        "city": "ΠΕΡΙΣΤΕΡΙ ΑΤΤΙΚΗΣ ",
        "color": "Κόκκινο ",
        "description": "ORIGINAL GTR600 GEMBALLA ΜΟΝΑΔΙΚΟ ΣΤΗΝ ΕΛΛΑΔΑ I804 GT2 Carbon exterior package IX26 Steering wheel airbag, full leather IX28 Steering wheel airbag, light burr walnut, leather IX30 Steering wheel airbag, dark burr walnut, leather IX45 Instrument dial in interior "color" IX46 Tiptronic selector lever in aluminium/leather IX47 Gear lever knob in Carbon/leather IX48 Tiptronic selector lever in Carbon/aluminium IX50 Powerkit 331/340 kW IX54 Stainless steel tailpipes IX58 Handbrake lever in Carbon/leather IX65 Tiptronic selector lever in light burr maple/aluminium IX66 Tiptronic selector lever in dark burr maple/aluminium IX69 Door entry guards in Carbon with logo IX70 Door entry guards in stainless steel with logo IX71 Instrument dial painted/Aluminium Look finish IX72 Gear lever knob aluminium/light burr maple/leather IX73 Turbo sports suspension IX74 Sports suspension (exclusive) IX75 Differential lock (exclusive) IX76 Inner door sill guard, left/right IX77 Steering wheel airbag, carbon/leather IX91 Handbrake lever aluminium/light burr maple/leather IX97 Gear lever knob in aluminium/leather IX98 Handbrake lever in aluminium/leather IX99 Natural leather IXAA Aerokit Cup IXAB Spyder rear in body colour IXAF Turbo aerokit IXAG Carrera - rear spoiler IXCA Dashboard trim strip (3-part) sports seats IXCC Side air vents sports seats IXCD Center air vent bracket sports seats IXCE Rear center console sports seats IXCG Sports seat backrest in Aluminium Look finish IXCZ Short shifter IXD9 Wheel painted in body colour IXE8 Gear lever knob aluminium/dark burr maple/leather IXE9 Handbrake lever aluminium/dark burr maple/leather IXEA Passive handset in leather IXJ4 Leather cover for ignition lock surround IXJB Rear section of centre console in Arctic Silver IXKB Side air vent in light burr maple IXKC Side air vent in dark burr maple IXKD Side air vent in Carbon IXKE Side air vent in Arctic Silver IXKG "Defroster trim in light burr maple; leather" IXKH "Defroster trim in dark burr maple; leather" IXKJ Defroster trim in Carbon/leather IXKL Leather speaker covers IX"km" Speaker covers in light burr maple IXKN Speaker covers in dark burr maple IXKP Carbon speaker covers IXKR Center air vent bracket in light burr maple IXKS Center air vent bracket in dark burr maple IXKW Center nozzle bracket arctic silver IXKX Instrument surround in Arctic Silver IXLA Boxster tailpipe IXLF Sports exhaust system IXMA Rooflining in leather IXME Rear section of centre console painted IXMF Front center console including 2 storage bins, leather IXMJ Rear section of centre console in Carbon IXMK Roll-over bar in exterior "color" IXML Rear center console in light burr maple IXMP Leather sun visor, 2 illuminated vanity mirrors * IXMR Sun visor in leather, 2 illuminated vanity mirrors, IXMY Roll-over bar in Arctic Silver IXMZ Rear section of centre console in leather IXN3 Dashboard side air vent in leather IXNB Rear center console in dark burr maple IXNG Instrument surround in leather IXNH Side vent, left/right defroster trim in leather IXNN Center air vent bracket in leather IXNR Carbon center air vent bracket IXNS Leather steering column trim, 4-part IXNU Leather trim strip (dashboard) IXNV Trim strip (dashboard) in light burr maple IXNW Trim strip (dashboard) in dark burr maple IXNX Carbon trim strip (dashboard) IXNY Trim strip in Arctic Silver IXPA 3-spoke sports steering wheel in leather interior co IXPB 3-spoke sports steering wheel, light burr maple, leather IXPC 3-spoke sports steering wheel, dark burr maple, leather IXPD 3-spoke sports steering wheel, Carbon/leather IXPG 3-spoke sports steering wheel, sports seats IXPP Loudspeaker finisher on switch panel in interior colour IXPW Instrument surround in light burr maple IXPX Instrument surround in dark burr maple * IXPY Carbon instrument surround IXRA 17-inch SPORTCLASSIC WHEEL IXRB 18-inch Sport Classic II wheel IXRC 18-inch SportTechno wheel IXRL 18-inch SportDesign wheel IXRN 17-mm spacers, rear IXRP 5-mm spacers, front/rear IXSA Painted sports seat backrest IXSB Leather sports seat backrest IXSC Porsche Crest embossed in headrest IXSD Front seat controls in leather IXSE Bucket seat, left IXSF Bucket seat, right IXSG Racing bucket seat, left IXSJ 6-point harness IXSL 6-point safety catch IXSM Racing safety cage IXSN Omission of rear seats IXSR Carbon back for bucket seat, right IXSS Carbon back for bucket seat, left IXSU Upholstered seat IXSW Seat belts in Maritime Blue IXSX Seat belts in Guards Red IXSY Seat belts in Speed Yellow IXTC Door panel parts in leather IXTE Door panel parts in carbon/leather IXTF Door panel parts in arctic silver/leather IXTG Inner door sills in leather IXTJ Door panel parts in light burr maple IXTK Door panel parts in dark burr maple IXTL Door panel parts in Carbon IXTR Door panel parts in Aluminium Look finish IXV1 Defroster trim in leather IXX1 Floor mats, logo, leather border IXX2 Illuminated footwell IXY5 Tiptronic gear selector gate in leather IXZD Interior light surround in leather M000 +-------------------------------+ ! Summary of optional extras "M"! +-------------------------------+ M003 Racing safety devices M014 Sport package 996 M024 Version for Greece M030 Sport-type chassis M032 Touring suspension M034 Version for Italy M058 Impact absorbers, front and rear M061 Version for Great Britain M062 Version for Sweden M063 Version Luxemburg M064 Version for Netherlands M065 Version for Denmark M066 Version for Norway M067 Version for Finland M068 Version for Thailand M069 Other country version M071 EU country version M072 Version for Mexico M073 Version for Russia M092 Special model Turbo S M101 "engine" parts 996 GT2 M111 Version for Austria M113 Version for Canada M114 Version for Taiwan M119 Version for Spain M124 Version for France M126 Control and indications in French lettering M127 Control and indications in Swedish M130 Control and indications in English lettering M139 Seat heating system, left seat M150 Operation with leaded gas M152 Fuel cooler M193 Version for Japan M197 Stronger battery M215 Version for Saudi Arabia M219 Differential M220 Locking differential M222 Anti-slip regulation (ASR) M224 Automatic limited slip differential M225 Version for Belgium M249 Tiptronic "transmission" M265 Automatic anti-dazzle interior mirror with rain sensor M266 Automatic anti-dazzle door mirror M270 Door mirror -flat- driver's side, electrically adjustable and heatable M271 Door mirror -aspherical- driver's side, electrically adjustable and heatable M273 Door mirrors, electrically adjustable and heatable M274 Vanity mirror illuminated M277 Version for Switzerland M288 Headlamp washer M320 Radio "Porsche CR-11" ROW M321 Radio "Porsche CR 22" M322 Radio "Porsche CR 220" M325 Version for South Africa/New Zealand M326 Radio "Porsche CR-21" ROW M327 Radio "Porsche CR 2200" M329 Radio "Porsche CR-210" USA M330 Radio "Porsche CR-31" ROW M335 Automatic seat belt, 3-point, rear M338 Rear-wheel drive M339 All-wheel drive M340 Seat heating system, right seat M342 Seat heating system, left/right seat M369 Standard seat, left M370 Standard seat, right M375 Sports seat, backrest shell, left M376 Sports seat, backrest shell, right M396 Cast wheel, 17-inch M399 17-inch Carrera-4 wheel M410 18-inch Turbo 2 wheel M413 18-inch Turbo-Look wheel M415 18-inch monobloc wheel Top M416 Wheel, GT3 18-inch M418 18-inch Turbo 2 wheel, GT Silver Metallic M421 Front cassette compartment M422 Rear cassette compartment M424 CD compartment M425 Rear window wiper M426 Without rear window wiper M432 Steering wheel with Tiptronic control M436 3-spoke airbag steering wheel M437 Comfort seat, left, electrically adjustable M438 Comfort seat, right, electrically adjustable M439 Electrical hood operation M440 Manual antenna, 4 loudspeakers --- MY 2002 MY 2003 --- antenna diversity M441 Radio preparation M446 Concave wheel centers with full-colour Porsche Crest M449 Cast iron brake GT2 M450 Ceramic brake (PCCB) M454 Automatic speed control M465 Rear foglamp, left M466 Rear foglamp, right M476 Porsche Stability Management (PSM) M479 Version for Australia M480 6-speed manual "transmission" M484 Version for USA M488 Inscriptions in German language M490 -MY 01 sound system MY 02 sound system Harmann analog M492 Headlamps for left-hand traffic M498 Deletion of model designation on rear end M499 Version for Germany M509 Fire extinguisher M513 Lumbar support, right seat M532 Alarm system remote control omitted M533 Radio convertible-top operation omitted M534 Theft security system M535 Anti-theft lock 315 MHz M536 Alarm siren and tilt sensor M537 Seating position control for comfort seat, left M538 Seating position control for comfort seat, right M539 Mechanical seat-height adjustment, left M540 Mechanical seat-height adjustment, right M549 Roof Transport System M550 Hard top M551 Wind deflector M553 Version for USA, Canada M562 Airbag, driver's side and front passenger's side M563 Side airbag M566 Front fog lights, white M567 Windscreen tinted, upper part darker coloured M568 Windscreen without green top-tint M571 Activated charcoal filter M573 Air conditioner M574 Without air conditioner M580 Non-smoker's package M581 Center console, front M586 Lumbar support, left seat M590 Power lid locking M601 Xenon lighting system M602 Raised stop lamp M605 Headlight levelling system M606 Daytime driving lights M614 Preparation for telephone installation Motorola 2200 M618 Preparation for telephone installation M620 Electronic accelerator M635 Parking assistent M650 Electrical sliding roof M651 Electric window opener M652 without electric sunroof M657 Power assisted steering M659 On-board computer M660 Obd 2 M661 Stricter emission-control concept M662 Info/navigation system M663 Passive handset M664 Orvr M665 PCM2 basic module including radio M666 PCM2 telephone (GSM) M668 PCM2 telephone handset M670 PCM2 navigation M680 -MY 01 digital sound package MY 02- sound system Bose analog M685 Rear seats, split M686 Radio "Porsche CDR-21" ROW M688 Radio "Porsche CDR-210" USA M689 Preperation of cd autochanger M690 CD player "CD-10" radio + ARI M692 CD autochanger "Porsche " M695 CD radio "Porsche CDR 22" M696 CD radio "Porsche CDR 220" M698 CD radio "Porsche CDR 32" M699 MD radio "Porsche MDR32" M703 Trunk entrapment M936 Seat covers, rear, leather M937 Seat covers, rear, leatherette M939 Seat covers, rear, draped leather M946 Seat covers, front, leather/leather/leatherette M971 Natural leather, Toro-embossed M981 Leather equipment without seat covers M982 Seat covers, front, draped leather/leather/leather M983 Seat covers, front, leather M990 Seat covers, front, cloth/cloth/leatherette M999 Passenger compartment monitoring sensor for leather colour of choice",
        "engine": 3600,
        "fueltype": "Βενζίνη",
        "images": [
        "https://static1.car.gr/9484875_f_a.jpg",
        "https://static2.car.gr/9484875_f_b.jpg",
        "https://static1.car.gr/9484875_u_b.jpg",
        "https://static2.car.gr/9484875_b_b.jpg"
        ],
        "km": null,
        "postal_code": 12131,
        "price": 74990,
        "region": " ΑΤΤΙΚΗΣ",
        "release_date": "Jan 2005",
        "title": "Porsche 911 996 TURBO GTR600 GEMBALLA '05 - 74.990 EUR - Car.gr",
        "transmission": "Χειροκίνητο",
        "url": "https://www.car.gr/9484875"
    },
    
    {
        "bhp": 230,
        "car_id": 8054974,
        "city": "ΒΟΥΛΑ ΑΤΤΙΚΗΣ ",
        "color": "Μπορντό (Μεταλλικό)",
        "description": "Concours condition. Factory sale receipt and service history from day one. Full matching numbers. Best Rhd in Europe in sensible "price" ONO τηλ¨0049 15163501314 0049 15171320376",
        "engine": 6250,
        "fueltype": "Βενζίνη",
        "images": [
        "https://static1.car.gr/8054974_8_a.jpg",
        "https://static2.car.gr/8054974_8_b.jpg",
        "https://static2.car.gr/8054974_0_b.jpg",
        "https://static2.car.gr/8054974_2_b.jpg"
        ],
        "km": null,
        "postal_code": 16673,
        "price": 100000,
        "region": " ΑΤΤΙΚΗΣ",
        "release_date": "Mar 1963",
        "title": "Rolls Royce Silver Cloud III '63 - 100.000 EUR - Car.gr",
        "transmission": "Αυτόματο",
        "url": "https://www.car.gr/8054974"
    }
]
```


License
-------

Copyright (c) 2017 [Florents Tselai](https://tselai.com)
Licensed under the [MIT license](http://opensource.org/licenses/MIT).
