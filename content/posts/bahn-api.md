---
title: "Bahn Api"
date: 2018-12-24T13:08:28+01:00
draft: true
featuredImg: ""
tags: 
  - hack
---

## 1. Request Location via official API: 

Registered User and API key needed (get it here: https://data.deutschebahn.com/ and register for the fahrplan-api). Authorization via Bearer-Token
Example: `http://api.deutschebahn.com/fahrplan-plus/v1/location/berlin` 
```json=
[
    {
        "name": "BERLIN",
        "lon": 13.386988,
        "lat": 52.520501,
        "id": 8096003
    },
    {
        "name": "Berlin Hbf",
        "lon": 13.369549,
        "lat": 52.525589,
        "id": 8011160
    }
]
```



## 2. Request Live Timetable with station from Locationrequest via not so official API:

`https://reiseauskunft.bahn.de/bin/bhftafel.exe/dn?L=vs_java&start=yes&boardType=arr&time=24:00&input=8096021`

Result:

- Firstline: Stationname
    - Arrivaltime
    - Name of the Connection
    - Delay:
        - `no` for no delay
        - `cancel` if the connection is canceled
        - `+ $int` Minutes of delay

Example:
``` 
8096021 FRANKFURT(MAIN)
00:00
S      5
no
00:01
S      7
no
00:02
S      9
cancel
00:02
S      3
no
00:04
ICE  520
+ 5
.
.
.
``` 

What worked on powershell:

```shell= 
httpie "https://reiseauskunft.bahn.de/bin/bhftafel.exe/dn?L=vs_java&start=yes&boardType=arr&time=24:00&input=8096021" | Select-String -Pattern "ICE .* 520" -Context 1
```

output: 

```
 00:04
> ICE  520
  no
```

# Request Traindetails: 
```
http://api.deutschebahn.com/fahrplan-plus/v1/journeyDetails/620715%252F218185%252F762102%252F174146%252F80%253Fstation_evaId%253D8000105
```
(hint: encode the already encoded detailsId from Timetable)
