# Location-Based Service

## How to design Uber

or /Facebook nearby/Yelp/...\(any location based apps\)

### Scenario

#### Stage 1

* Drivers report locations
* Rider request Uber, match a driver with rider

#### Stage 2

* Driver deny/accept a request
* Driver cancels a matched request
* Rider cancels a request
* Driver picks up a rider and starts a trip
* Driver drops off a rider and ends a trip

#### Stage 3\*

* Uber pool
* Uber eats

#### Level of performance the system should be performing

* Assuming 200K rivers are online at the same time
  * Driver reports location every 4 seconds is a reasonable number
    * QPS = 200K / 4 = 50K
    * Peak QPS = 50K \* 3 = 150K
      * 150K writes is not a small number at all, it requires a write-bound, fast storage
  * Rider QPS can be ignored
    * Confidential considerations
    * Must be far less than drivers'
* Volumes estimation
  * If record each location info: 200K \* 86400 / 4 \* 100 bytes ~= 0.5T/day
  * If only record current location info: 200K \* 100 bytes = 20M/day

### Service

* Geoservice
  * Record vehicle's location
* DispatchService
  * Match request between driver and rider

#### Stage 1

![](../../.gitbook/assets/screen-shot-2019-12-23-at-9.15.15-pm.png)

#### Stage 2

The problem with above structure is it does not include how the driver receives the match request, the solution is when driver-client reports its location every 4 seconds, the response could includes a match request if there's a match

![](../../.gitbook/assets/screen-shot-2019-12-23-at-9.19.04-pm.png)

### Storage

* Trip table read more than write
* Location table write more than read

![](../../.gitbook/assets/screen-shot-2019-12-23-at-9.38.01-pm.png)

#### Schema

![](../../.gitbook/assets/screen-shot-2019-12-23-at-9.44.33-pm.png)

![](../../.gitbook/assets/screen-shot-2019-12-23-at-9.47.20-pm.png)

### How LBS stores geography information

![](../../.gitbook/assets/screen-shot-2019-12-23-at-10.07.52-pm.png)

