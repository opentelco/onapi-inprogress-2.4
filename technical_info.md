# Technical Info API

### Description

The information communicated is intended for troubleshooting purposes and is sourced from network elements.

Technical decisions and data structure design are taken with regards to network element resource utilization, and to promote programmatic decision making. 

Information should be sanitized or formatted in regard to keep data as raw as possible, but enabling a standardized way to communicate this data. For example converting timestamps in an unified manner, or translating vlan tags to service names.

Data communicated should only be related to its troubleshooting purpose or context.

Example service types:

| Name | Description |
|--|--|
|bb| Internet |
|iptv| Tv |
|voip| Voip |
|unknown| Unknown service |


# Specification
## Reference index
### access
* GET [access/hardware](#get-access-hardware)
* GET [access/downlink/macaddresstable](#get-access-downlink-macaddresstable)
* GET [access/downlink/dhcpsnooping](#get-access-downlink-dhcpsnooping)
* GET [access/downlink/igmpsnooping](#get-access-downlink-igmpsnooping)
* GET [access/downlink/status](#get-access-downlink-status)

### cpe
* GET [cpe/status](#get-cpe-status)

## Requirements


**API Prefix:** `/tech` 
eg: `domain.local/api/2.4/tech/access/equipment/423323449`

Endpoints like dhcpsnooping should when empty return an empty array:
```http
HTTP/1.1 200 OK
Content-Type: application/json

[]
```




## Parameters

| Parameter  | Data format |
|--|--|
|accessId|accessId|

## Endpoints

<h3 id="get-access-hardware">GET access/hardware/{accessId}</h3>
Returns information about the customer-facing network element, eg. access switch.

Request:
```http
GET /api/2.4/tech/access/hardware/{accessId}
```

Response when the network element is working as intended:
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "up": true,
  "upSince": "2004-08-14T14:09:23Z",
  "vendor": "Huawei",
  "model": "S5320+"
}
```
Response when network element or eg. management interface is unavailable:
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "up": false
}
```
| Parameter | Type | Description |
|--|--|--|
|up|bool|Status of network element, eg. pingable/management is reachable|
|upSince|string (iso8601)|Timestamp when hardware was started|
|vendor|string|Hardware vendor|
|model|string|Hardware model|

<h3 id="get-access-downlink-macaddresstable">GET access/downlink/macaddresstable/{accessId}</h3>

Fetch mac address table data from only the downlink interface.

Request:
```http
GET /api/2.4/tech/access/downlink/macaddresstable/{accessId}
```


Response:
```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "mac": "00:11:22:aa:bb:cc",
    "service": "bb"
  },
  {
    "mac": "00:11:22:aa:bb:cc",
    "service": "iptv"
  }
]
```

| Parameter | Type | Description |
|--|--|--|
|mac|string (17)|Mac address of device|
|service|string|Which network service the mac originates from|

<h3 id="get-access-downlink-dhcpsnooping">GET access/downlink/dhcpsnooping/{accessId}</h3>

Fetch dhcp snooping data from only the downlink interface.

Request:
```http
GET /api/2.4/tech/access/downlink/dhcpsnooping/{accessId}
```

Response:
```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "ip:": "10.0.0.1",
    "mac": "00:11:22:aa:bb:cc",
    "service": "bb",
    "timeout": 600
  },
  {
    "ip:": "10.0.0.1",
    "mac": "00:11:22:aa:bb:cc",
    "service": "iptv",
    "timeout": 600
  },
]
```

| Parameter | Type | Description |
|--|--|--|
|ip|string (7-19)|IP lease|
|mac|string (17)|Mac address of device|
|service|string |Which network service the mac originates from|
|timeout|integer (1-10)|DHCP Lease timeout|


<h3 id="get-access-downlink-igmpsnooping">GET access/downlink/igmpsnooping/{accessId}</h3>

Fetch igmp snooping data from only the downlink interface.

Request:
```http
GET /api/2.4/tech/access/downlink/igmpsnooping/{accessId}
```
Response:
```http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "multicast": "233.10.10.100",
    "join": "2004-08-14T14:09:23Z",
    "counter": 231231
  },
  {
    "multicast": "233.10.10.100",
    "join": "2004-08-14T14:09:23Z",
    "counter": 231231
  }
]
```
| Parameter | Type | Description |
|--|--|--|
|multicast|string (7-19)|Multicast address|
|join|string (iso8601)|Timestamp when latest join was sent|
|counter|integer|Traffic counter|



<h3 id="get-access-downlink-status">GET access/downlink/status/{accessId}</h3>

Fetch access equipment downlink interface status.

Request:
```http
GET /api/2.4/tech/access/downlink/status/{accessId}
```
Response:
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "link": true,
  "fullDuplex": true,
  "linkSince": "2014-08-14T14:09:23Z",
  "speed": "auto",
  "linkSpeed": 1000,
  "configuredSpeed": [
    {
      "service": "bb",
      "queue": 1,
      "cir": 3000,
      "pir": 3000
    },
    {
      "service": "iptv",
      "queue": 0,
      "cir": 12000,
      "pir": 12000
    }
  ],
  "statistics": {
    "counterResetAt": "2014-08-14T14:09:23Z",
    "peakInputRate": {
      "rate": 30000,
      "at": "2014-08-14T14:09:23Z"
    },
    "peakOutputRate": {
      "rate": 30000,
      "at": "2014-08-14T14:09:23Z"
    },
    "lastFiveInput": { 
      "packets": 100,
      "bytes": 10000,
    },
    "lastFiveOutput": {
      "packets": 100,
      "bytes": 10000,
    },
    "input": {
      "packets": 100,
      "bytes": 1000,
      "unicast": 100,
      "broadcast": 100,
      "multicast": 100,
      "pauses": 0,
      "errors": 1337,
      "crcErrors": 0

    },
    "output": {
      "packets": 100,
      "bytes": 1000,
      "unicast": 100,
      "broadcast": 100,
      "multicast": 100,
      "pauses": 0,
      "errors": 1337,
      "crcErrors": 0
    }
  },
  "fiber": {
    "dbmRx": "-12",
    "dbmTx": "-8",
    "temp": "35"
  },
}
```
| Parameter | Type | Description |
|--|--|--|
|link|bool|Physical link status|
|fullDuplex|bool|If interface is set to full duplex|
|linkSince|string (iso8601)| Timestamp when port changed status|
|speed|string|Configured speed|
|linkSpeed|integer|Link speed|
|configuredSpeed|list|List of configured traffic shaping/policies|
|configuredSpeed.#.service|string|Service name|
|configuredSpeed.#.queue|string|Queue index|
|configuredSpeed.#.cir|string|Commited rate|
|configuredSpeed.#.pir|string|Peak rate|
|statistics.counterResetAt|string (iso8601)| Timestamp when statistics counter was started|
|statistics.peakInputRate.rate|integer| Bytes per second|
|statistics.peakInputRate.at|string (iso8601)| Timestamp of peak|
|statistics.peakOutputRate.rate|string|Bytes per second|
|statistics.peakOutputRate.at|string (iso8601)| Timestamp of peak|
|statistics.lastFiveInput.packets|integer|Ingress count of packets last five minutes|
|statistics.lastFiveInput.bytes|integer|Ingress count of bytes last five minutes|
|statistics.lastFiveOutput.packets|integer|Egress count of packets last five minutes|
|statistics.lastFiveOutput.bytes|integer|Egress count of byes last five minutes|
|statistics.input.packets|integer|Ingress count of packets|
|statistics.input.bytes|integer|Ingress count of bytes|
|statistics.input.unicast|integer|Ingress count of unicast|
|statistics.input.broadcast|integer|Ingress count of broadcast|
|statistics.input.multicast|integer|Ingress count of multicast|
|statistics.input.pauses|integer|Ingress count of pauses|
|statistics.input.errors|integer|Ingress count of errors|
|statistics.input.crcErrors|integer|Ingress count of crc errors|
|statistics.output.packets|integer|Egress count of packets|
|statistics.output.bytes|integer|Egress count of bytes|
|statistics.output.unicast|integer|Egress count of unicast|
|statistics.output.broadcast|integer|Egress count of broadcast|
|statistics.output.multicast|integer|Egress count of multicast|
|statistics.output.pauses|integer|Egress count of pauses|
|statistics.output.errors|integer|Egress count of errors|
|statistics.output.crcErrors|integer|Egress count of crc errors|
|fiber.dbmRx|string|Fiber recieve dampening|
|fiber.dbmTx|string|Fiber transcieve dampening| 
|fiber.temp|string|SFP temperature|


<h3 id="get-cpe-status">GET cpe/status/{accessId}</h3>

Fetch full cpe state.

Request:
```http
GET /api/2.4/tech/cpe/status/{accessId}
```
Response:
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "firmware": "abc123.123",
  "vendor": "inteno",
  "model": "xge365",
  "mac": "xx:xx:xx:xx:xx:xx",
  "up": true,
  "upSince": "2004-08-14T14:09:23Z",
  "ports": [
    {
      "index": 0,
      "ifDescription": "WAN",
      "name": "WAN sfp",
      "link": true,
      "fullDuplex": true,
      "changedAt": "2004-08-14T14:09:23Z",
      "loopDetectionStatus": "unlocked",
      "freeSeating": false,
      "configuredSpeed": "auto",
      "linkSpeed": 1000,
      "fiber": {
        "dbmRx": "-12",
        "dbmTx": "-8",
        "temp": "35"
      },
      "statistics": {
        "input": {
          "errors": 123,
          "crcErrors": 123,
          "bytes": 123,
          "multicast": 123,
          "unicast": 123,
        },
        "output": {
          "errors": 123,
          "crcErrors": 123,
          "bytes": 123,
          "multicast": 123,
          "unicast": 123,
        }
      }
    },
    {
      "index": 1,
      "ifDescription": "LAN1",
      "name": "LAN 1 (red)",
      "link": true,
      "fullDuplex": true,
      "changedAt": "2004-08-14T14:09:23Z",
      "loopDetectionStatus": "unlocked",
      "freeSeating": false,
      "configuredSpeed": "auto",
      "linkSpeed": 1000,
      "fiber": null,
      "statistics": {
        "input": {
          "errors": 123,
          "crcErrors": 123,
          "bytes": 123,
          "multicast": 123,
          "unicast": 123,
        },
        "output": {
          "errors": 123,
          "crcErrors": 123,
          "bytes": 123,
          "multicast": 123,
          "unicast": 123,
        }
      }
    }
  ],
  "macAddressTable": [
    {
      "mac": "xx:xx:xx:xx:xx:xx",
      "service": "bb",
      "port": 1
    },
    {
      "mac": "xx:xx:xx:xx:xx:xx",
      "service": "iptv",
      "port": 2
    }
  ],
  "dhcpSnooping": [
    {
      "ip:": "10.0.0.1",
      "mac": "00:11:22:aa:bb:cc",
      "service": "iptv",
      "timeout": "2004-08-14T14:09:23Z",
    },
    {
      "ip:": "10.0.0.1",
      "mac": "00:11:22:aa:bb:cc",
      "service": "iptv",
      "timeout": "2004-08-14T14:09:23Z",
    },
  ]
}
```
| Parameter | Type | Description |
|--|--|--|
|firmware|string|CPE firmware name|
|vendor|string|CPE vendor name|
|model|string|CPE model name|
|mac|string|CPE mgmt interface mac address|
|up|bool|Power status|
|upSince|string|Uptime of CPE|
|ports|list|List of port objects|
|ports.#.index|integer|Interface number|
|ports.#.ifDescription|string|Interface description|
|ports.#.name|string|Friendly interface name|
|ports.#.link|bool|duplex status|
|ports.#.fullDuplex|bool|duplex status|
|ports.#.changedAt|string (iso8601)| Timestamp when port changed state|
|ports.#.loopDetectionStatus|string| Loop detection status| 
|ports.#.freeSeating|bool| Free seating status| 
|ports.#.configuredSpeed|string|Configured speed|
|ports.#.linkSpeed|integer|Link speed|
|ports.#.fiber.dbmRx|string|Fiber recieve dampening|
|ports.#.fiber.dbmTx|string|Fiber transcieve dampening
|ports.#.fiber.temp|string|SFP temperature|
|ports.#.statistics.input.errors|integer|Ingress count of errors|
|ports.#.statistics.input.crcErrors|integer|Ingress count of crc errors|
|ports.#.statistics.input.bytes|integer|Ingress count of bytes|
|ports.#.statistics.input.multicast|integer|Ingress count of multicast|
|ports.#.statistics.input.unicast|integer|Ingress count of unicast|
|ports.#.statistics.output.errors|integer|Egress count of errors|
|ports.#.statistics.output.crcErrors|integer|Egress count of crc errors|
|ports.#.statistics.output.bytes|integer|Egress count of bytes|
|ports.#.statistics.output.multicast|integer|Egress count of multicast|
|ports.#.statistics.output.unicast|integer|Egress count of unicast|
|macAddressTable|list|List of mac address objects|
|macAddressTable.#.mac|string|Mac address of device|
|macAddressTable.#.service|string|Which network service the mac originates from|
|macAddressTable.#.port|string|Which CPE port index the mac originates from|
|dhcpSnooping|list|List of dhcp snooping objects|
|dhcpSnooping.#.ip|string|IP lease|
|dhcpSnooping.#.mac|string|Mac address of device|
|dhcpSnooping.#.service|string|Which network service the lease originates from|
|dhcpSnooping.#.timeout|string (iso8601)|DHCP Lease timeout|
  