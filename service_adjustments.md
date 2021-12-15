# WORKING DRAFT Service Adjustments API Design

## Design
- TM service adjustment contains all routes with identical detours
- both directions packaged into a single detour

## Adjusted Schedule Data Loader

### TODO
- **Loops** (when a stop\_id is repeated in a trip): GTFS stop\_times.txt is keyed on trip\_id, stop\_sequence, but that makes adjustment applicable to specific FromToVia, not just route\_id-direction\_id combinations. Shapes in ScheduleDB are constructed from OriginStop-DestinationStop pairs, which can be derived from the stop-sequence.
- **Metadata**: do we want information about who created the adjustment, notes, other information about the process of making the detour rather than the actual detour itself?
- **Recurrence**: is the existing recurrence framework sufficient? Would we ever want to schedule a detour that happens every 3rd Thursday of the month? This isn't possible with the current options in Transit Master, would need to add day-of-month, week-of-month fields. What scheduled but not repeating detours, for example, suppose we detour routes around some stadium during a sports team's home game schedule. We'd know which dates the detour applies to, but they wouldn't fit into a repeating framework without relying heavily on the `excluded_dates` field. Maybe a `specified_dates` field would work?

### Design decisions
- Detours start and end at the last regular service stop and first resumed stop.
- Times are local date (`%Y-%m-%d`) and integer time, e.g. `{"date": "2021-12-15", "time": 9000}` for "2021-12-15 02:30:00 America/Chicago".
- Transit Master Recurrence
	- Start date
	- End date
	- Start time
	- End time
	- Recurs every X {days, weeks}
	- Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday
	- Excluded dates

 

### Example: NB Nicollet Mall detour
Detour:

- 19333 (entry stop) to 53311
- 53311 to 53312
- 53312 to 53313
- 53313 to 53314
- 53314 to 17950 (exit stop)

#### sample
*new_shape_id 1 isn't correct, but accurately portrays the data layout*
```json
{
  "service_adjustment_id": "129861r09ruqw0",
  "detour_name": "Nicollet Mall Farmer's Market NB routes 4, 6, 12",
  "previous_adjustment_id": null,
  "adjustment_types": [
    "detour"
  ],
  "skipped_stops": [
    "17990",
    "17992",
    "17994",
    "17996",
    "17998"
  ],
  "new_shapes": [
    {
      "new_shape_id": "0",
      "shape_segment": [
        {
          "from_stop_id": "19333",
          "to_stop_id": "53311",
          "lat": [
            44.9731,
            44.9729,
            44.9728,
            44.9728,
            44.9727,
            44.9726,
            44.972,
            44.9714,
            44.9709,
            44.9717
          ],
          "lon": [
            -93.2789,
            -93.2788,
            -93.2786,
            -93.2783,
            -93.278,
            -93.2778,
            -93.2763,
            -93.2749,
            -93.2735,
            -93.2728
          ]
        },
        {
          "from_stop_id": "53311",
          "to_stop_id": "53312",
          "lat": [
            44.9717,
            44.9718,
            44.9728,
            44.9737
          ],
          "lon": [
            -93.2728,
            -93.2727,
            -93.272,
            -93.2713
          ]
        },
        {
          "from_stop_id": "53312",
          "to_stop_id": "53313",
          "lat": [
            44.9737,
            44.9738,
            44.9748,
            44.9756
          ],
          "lon": [
            -93.2713,
            -93.2711,
            -93.2703,
            -93.2696
          ]
        },
        {
          "from_stop_id": "53313",
          "to_stop_id": "53314",
          "lat": [
            44.9756,
            44.9757,
            44.9767,
            44.9776
          ],
          "lon": [
            -93.2696,
            -93.2695,
            -93.2687,
            -93.268
          ]
        },
        {
          "from_stop_id": "53314",
          "to_stop_id": "17950",
          "lat": [
            44.9775,
            44.9776,
            44.9786,
            44.9796,
            44.9805,
            44.9807,
            44.9812,
            44.9819,
            44.9822,
            44.9827
          ],
          "lon": [
            -93.268,
            -93.2679,
            -93.2671,
            -93.2663,
            -93.2654,
            -93.2653,
            -93.2667,
            -93.2681,
            -93.2689,
            -93.2683
          ]
        }
      ]
    },
    {
      "new_shape_id": "1",
      "shape_segment": [
        {
          "from_stop_id": "19333",
          "to_stop_id": "53311",
          "lat": [
            44.9731,
            44.9729,
            44.9728,
            44.9728,
            44.9727,
            44.9726,
            44.972,
            44.9714,
            44.9709,
            44.9717
          ],
          "lon": [
            -93.2789,
            -93.2788,
            -93.2786,
            -93.2783,
            -93.278,
            -93.2778,
            -93.2763,
            -93.2749,
            -93.2735,
            -93.2728
          ]
        },
        {
          "from_stop_id": "53311",
          "to_stop_id": "53312",
          "lat": [
            44.9717,
            44.9718,
            44.9728,
            44.9737
          ],
          "lon": [
            -93.2728,
            -93.2727,
            -93.272,
            -93.2713
          ]
        },
        {
          "from_stop_id": "53312",
          "to_stop_id": "53313",
          "lat": [
            44.9737,
            44.9738,
            44.9748,
            44.9756
          ],
          "lon": [
            -93.2713,
            -93.2711,
            -93.2703,
            -93.2696
          ]
        },
        {
          "from_stop_id": "53313",
          "to_stop_id": "53314",
          "lat": [
            44.9756,
            44.9757,
            44.9767,
            44.9776
          ],
          "lon": [
            -93.2696,
            -93.2695,
            -93.2687,
            -93.268
          ]
        },
        {
          "from_stop_id": "53314",
          "to_stop_id": "17950",
          "lat": [
            44.9775,
            44.9776,
            44.9786,
            44.9796,
            44.9805,
            44.9807,
            44.9812,
            44.9819,
            44.9822,
            44.9827
          ],
          "lon": [
            -93.268,
            -93.2679,
            -93.2671,
            -93.2663,
            -93.2654,
            -93.2653,
            -93.2667,
            -93.2681,
            -93.2689,
            -93.2683
          ]
        }
      ]
    }
  ],
  "new_stops": [
    {
      "stop_id": "53311"
    },
    {
      "stop_id": "53312"
    },
    {
      "stop_id": "53313"
    },
    {
      "stop_id": "53314"
    }
  ],
  "entity_selector": [
    {
      "route_id": "6",
      "direction_id": 0,
      "new_shape_id": 1,
      "FromToVia": null
    },
    {
      "route_id": "4",
      "direction_id": 0,
      "new_shape_id": 1,
      "FromToVia": null
    },
    {
      "route_id": "6",
      "direction_id": 1,
      "new_shape_id": 1,
      "FromToVia": null
    },
    {
      "route_id": "4",
      "direction_id": 1,
      "new_shape_id": 1,
      "FromToVia": null
    }
  ],
  "start_datetime": {
    "date": "2021-09-23",
    "time": 17100
  },
  "end_datetime": {
    "date": "2021-09-23",
    "time": 57600
  },
  "recurs_every": {
    "days": null,
    "weeks": 1,
    "weekday": [
      4
    ],
    "ends_on": {
      "date": "2021-12-23",
      "time": 57600
    },
    "excluded_dates": null
  }
}
```

### Example: EB Route 23 Detour Minnehaha to 42nd Ave
Detour: Right on Minnehaha, left on 39th, left on 42nd, right on Minnehaha

#### sample
```json
{
  "service_adjustment_id": "3281yhfljksdh",
  "detour_name": "EB Route 23 Detour Minnehaha to 42nd Ave",
  "previous_adjustment_id": null,
  "adjustment_types": [
    "detour"
  ],
  "skipped_stops": [
    "12549",
    "12545",
    "12541",
    "12537",
    "12533"
  ],
  "new_shapes": [
    {
      "new_shape_id": "0",
      "shape_segment": [
        {
          "from_stop_id": "12551",
          "to_stop_id": "100001",
          "lat": [
            44.933815,
            44.932398,
            44.932372
          ],
          "lon": [
            -93.223789,
            -93.219238,
            -93.222607
          ]
        },
        {
          "from_stop_id": "100001",
          "to_stop_id": "100002",
          "lat": [
            44.932398,
            44.93243
          ],
          "lon": [
            -93.219238,
            -93.216705
          ]
        },
        {
          "from_stop_id": "100002",
          "to_stop_id": "12529",
          "lat": [
            44.93243,
            44.932458,
            44.934266,
            44.93423
          ],
          "lon": [
            -93.216705,
            -93.212783,
            -93.212783,
            -93.210365
          ]
        }
      ]
    }
  ],
  "new_stops": [
    {
      "stop_id": "12550"
    },
    {
      "stop_id": "100001",
      "stop_lat": 44.932398,
      "stop_lon": -93.219238,
      "stop_name": "39th St & 37th Ave S"
    },
    {
      "stop_id": "100002",
      "stop_lat": 44.93243,
      "stop_lon": -93.216705,
      "stop_name": "39th St & 39th Ave S"
    }
  ],
  "entity_selector": [
    {
      "route_id": "23",
      "direction_id": 0,
      "new_shape_id": 0
    }
  ],
  "start_date": {
    "date": "2021-10-13",
    "time": 57600
  },
  "end_date": null,
  "estimated_end_date": {
    "date": "2022-01-30",
    "time": 57600
  },
  "recurs_every": null
}
```

### Example: Concord Closed Hardman to Armour Phase 2
Rt. 71 #32962 Start: 7/31/21 @345p End: Indef

Due to Concord St improvement project. Starting 7/31/21

**Buses continuing past CXCO**:
- Route 71 A/D
- SB (new_shape_id 0): Reg Concord to R Central (next street after Bryant), L-10th Ave N, R-Thompson, L-12th Ave N, L- Southview, L- 3rd Ave S onto Grand Ave, R- Hardman, R- Armour, L- Concord & reg.
- NB: Concord to R- Armour, L- Hardman, R- Concord & Reg
 
**Buses Laying over at Concord Exchange**:

- 71 B/E
- SB (new_shape_id 2): Reg Concord to R Central (next street after Bryant), L-10th Ave N, R-Thompson, L-12th Ave N, L- Southview, L- 3rd Ave S onto Grand Ave, R- Hardman, R - Villaume, R - Farwell, Layover on Farwell at Armour
- NB (new_shape_id 3): From Layover R- Armour, L-Hardman, R- Concord & Reg.

**P/I & Deadhead buses ending at Concord Exchange**:

- 71 ??
- SB (new_shape_id 4): Reg Concord to R Central (next street after Bryant), L-10th Ave N, R-Thompson, L-12th Ave N, L- Southview, L- 3rd Ave S onto Grand Ave, R- Veteran's Memorial DR, L- Concord Exchange, R- Grand, R- Hardman, R - Armour, L - Concord, to 494 and reg

Passengers directed to:

- NB:
  - On Armour @ Concord #41155
  - Temp stop on Hardman @ Grand #80310
- SB:
  - On Armour @ Concord #41144
  - Temp stop on Hardman @ Grand #80311
  - On Concord north of Bryant Ave #10062

#### sample
```json
{
  "service_adjustment_id": "32962",
  "detour_name": "Concord Closed Hardman to Armour Phase 2",
  "previous_adjustment_id": null,
  "adjustment_types": [
    "detour"
  ],
  "skipped_stops": [
    "10064",
    "10065",
    "48729"
  ],
  "new_shapes": [
    {
      "new_shape_id": "0",
      "shape_segment": [
        {
          "from_stop_id": "10062",
          "to_stop_id": "10066",
          "lat": [
            44.933815,
            44.932398,
            44.932372
          ],
          "lon": [
            -93.223789,
            -93.219238,
            -93.222607
          ]
        }
      ]
    },
    {
      "new_shape_id": "1",
      "shape_segment": [
        {
          "from_stop_id": "10062",
          "to_stop_id": "41150",
          "lat": [
            44.933815,
            44.932398,
            44.932372
          ],
          "lon": [
            -93.223789,
            -93.219238,
            -93.222607
          ]
        }
      ]
    },
    {
      "new_shape_id": "2",
      "shape_segment": [
        {
          "from_stop_id": "10062",
          "to_stop_id": "80305",
          "lat": [
            44.933815,
            44.932398,
            44.932372
          ],
          "lon": [
            -93.223789,
            -93.219238,
            -93.222607
          ]
        }
      ]
    },
    {
      "new_shape_id": "3",
      "shape_segment": [
        {
          "from_stop_id": "80305",
          "to_stop_id": "??Concord & Hardman",
          "lat": [
            44.933815,
            44.932398,
            44.932372
          ],
          "lon": [
            -93.223789,
            -93.219238,
            -93.222607
          ]
        }
      ]
    }
  ],
  "new_stops": [],
  "entity_selector": [
    {
      "new_shape_id": 0,
      "route_id": "71",
      "direction_id": 1,
      "FromToVia": [
        "LCTCCHEL57",
        "SLROCHEL57",
        "SLROINCO70"
      ]
    },
    {
      "new_shape_id": 1,
      "route_id": "71",
      "direction_id": 1,
      "FromToVia": [
        "SLMCINCO70",
        "SLROINCO70"
      ]
    },
    {
      "new_shape_id": 2,
      "route_id": "71",
      "direction_id": 1,
      "FromToVia": [
        "SLROCXCO61",
        "LCTCCXCO61",
        "SLROCXCO54",
        "LREMCXCO54",
        "SLROCXCO59",
        "SLROCXCO53"
      ]
    },
    {
      "new_shape_id": 3,
      "route_id": "71",
      "direction_id": 0,
      "FromToVia": [
        "CXCOLCTC12",
        "CXCOLCTC13",
        "CXCOSLRO12",
        "CXCOSLRO13",
        "CXCOSLRO16",
        "CXCOSLRO40"
      ]
    }
  ],
  "start_date": {
    "date": "2021-07-31",
    "time": 56700
  },
  "end_date": null,
  "estimated_end_date": {
    "date": "2022-07-31",
    "time": 52200
  },
  "recurs_every": null
}
```

## GTFS-realtime Service Changes v3

### TripUpdates
api path: `adjustments/v1/tripupdate`

#### sample
```json
{
	"trip_update":
	[
		{
			"trip": {"trip_id": "19080619", "schedule_relationship": 2},
			"trip_properties": {
				"trip_id": "19080619-001", 
				"start_date": "",
				"start_time": "12:40:00",
				"trip_headsign": "Selby-Lake/Uptown",
				"trip_short_name": null,
				"block_id": "1214-OL1",
				"shape_id": "210014"
			},
			"vehicle_properties": {
				"wheelchair_accessible": 1,
				"bikes_allowed": 0
			},
			"stop_time_update": [
				{
					"stop_sequence": 1,
					"arrival": {"schedule_time": },
					"departure": {"schedule_time": },
					"schedule_relationship": 0,
					"stop_time_properties":{
						"pickup_type": 1
					}
				}
			]
		}
	]
}
```

### NewShapes
api path: `adjustments/v1/newshapes`

Shape

 - shape\_id
 - encoded\_polyline

Implementation 

 - apply detours to path
 - ordered lat-lon points to encoded polyline

#### sample
```json
{
	"shape":
	[
		{"shape_id": "20001-13k4jnf434t", "encoded_polyline": "sdasldk3if"}
	]
}
```

### NewTrips
api path: `adjustments/v1/newtrips`

 - trip
   - trip\_id (req)
   - route\_id
   - trip\_headsign (req)
   - trip\_short_name (req)
   - direction\_id
   - block\_id
   - shape\_id
   - wheelchair\_accessible
   - bikes\_allowed
   - start\_date (required many)
   - stop\_time (required many)
     - stop\_sequence (required)
     - stop\_id (required)
     - arrival\_time (required)
     - departure\_time (required)
     - stop\_headsign
     - pickup\_type
     - drop\_off\_type
     - shape\_dist\_traveled

#### sample
```json
{
	"trip":
		[
			{
				"trip_id": "19080619-001", "route_id": "21",
				"trip_headsign": "Selby-Lake/Uptown", "trip_short_name": null,
				"direction_id": 1, "block_id": "1214", "shape_id": "210014",
				"wheelchair_accessible": 1, "bikes_allowed": null,
				"start_date": ["20211101", "20211102"],
				"stop_time": [
					{
						"stop_sequence": 1, "stop_id": "56100",
						"arrival_time": "12:36:00", "departure_time": "12:36:00",
						"stop_headsign": null,
						"pickup_type": 0, "drop_off_type": 1, "shape_dist_traveled": 0.0
					},
				]
			},
		]
}
```

### NewRoutes
api path: `adjustments/v1/newroutes`

- route
	- route\_id (req)
	- agency\_id (req)
	- route\_short\_name
	- route\_long\_name
	- route\_desc
	- route\_type (req)
	- route\_url
	- route\_color
	- route\_text\_color
	- route\_sort\_order

#### sample
```json
{
	"route":
	[
		{
			"route_id": "992", "agency_id": "0", "route_short_name": "Green Line Shuttle",
			"route_long_name": null,
			"route_desc": "Bus service replacing Green Line trains",
			"route_type": 3, "route_url": "https://www.metrotransit.org/route/992",
			"route_color": "00B100", "route_text_color": "ffffff", "route_sort_order": 3
		}
	]
}
```

### NewStops
api path: `adjustments/v1/newstops`

 - stop
   - stop_id (req)
   - stop_code
   - stop_name (req)
   - stop_desc
   - stop_lat (req)
   - stop_lon (req)
   - zone_id
   - stop_url
   - parent_station
   - stop_timezone
   - wheelchair_boarding
   - level_id
   - platform_code

#### sample
```json
{
	"stop":
		[
			{"stop_id": "100001", "stop_name": "39th St at 37th Ave", "stop_lat": 44.24, "stop_lon": -93.5},
			{"stop_id": "100002", "stop_code": "100002", "stop_name": "39th St at 39th Ave", "stop_desc": "Nearside", "stop_lat": 44.24, "stop_lon": -93.5, "zone_id": "1", "stop_url": "https://www.metrotransit.org/nextrip/100002", "parent_station": null, "stop_timezome": null, "wheelchair_boarding": null, "level_id": null, "platform_code": null}
		]
}
```

### ServiceUpdates
api path: `adjustments/v1/serviceupdates`

 - service_update (many)
   - service_id (req)
   - date (req, many)
   - exception_type (req)

#### sample
```json
{
	"service_update":
	[
		{
			"service_id": "AUG21-MVS-BUS-Weekday-01",
			"date": ["20211105", "20211119"],
			"exception_type": 2
		},
		{
			"service_id": "AUG21-MVS-NS-Weekday-01",
			"date": ["20211125"],
			"exception_type": 2
		}
	]
}
```
