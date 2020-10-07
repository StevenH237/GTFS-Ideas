This proposal is for GTFS files to define places people can park for the purpose of riding public transit.

Note that the term "parking lot" can be used to refer to any of the following (non-exhaustive list) that are used to park vehicles to ride transit:

* A surface lot
* A parking garage
* A bicycle rack

# The tables

## Table of contents
Table name|Requirement details
:-|:-
[**`parking_lots`**](#parking_lotstxt)|Required in order to use this propsal.
[**`parking_hours`**](#parking_hourstxt)|Required unless all defined lots have 24/7 access.
[**`parking_periods`**](#parking_periodstxt)|Required in order to use this proposal, unless all defined lots are free with 24/7 access.
[*`parking_rates`*](#parking_ratestxt)|Required if any parking lots require a fee be paid to park.

## `parking_lots.txt`
This table defines the lots that passengers may park in to ride transit.

It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
**`parking_lot_id`**|Required: ID|The ID of this parking lot.
**`stop_id`**|Required: ID referencing [`stops.stop_id`](https://developers.google.com/transit/gtfs/reference/#stopstxt)|The stop - usually of `location_type` 3 - indicating where this parking lot is located.
`parking_type`|Enum|The type of this parking lot:<ul><li>`0` or empty: Parking for cars</li><li>`1`: Parking for bikes</li></ul>
`public_spaces`|Non-negative integer|How many spaces are available for parking, excluding accessibly-reserved spaces.
`accessible_spaces`|Non-negative integer|How many accessibly-reserved spaces are available for parking.
`covered_spaces`|Non-negative integer|How many of the spaces are covered. Covered spaces should still be counted under `public_spaces` or `accessible_spaces`.

## `parking_periods.txt`
This table defines the time periods that are available for parking lots.

It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
**`parking_period_id`**|Required: ID|The ID of this time period.
**`service_id`**|Required: ID referencing [`calendar.service_id`](https://developers.google.com/transit/gtfs/reference/#calendartxt) or [`calendar_dates.service_id`](https://developers.google.com/transit/gtfs/reference/#calendar_datestxt)|The service ID indicating the days to which this period applies.
**`start_time`**|Required: Time|The time at which this period starts.
**`end_time`**|Required: Time|The time at which this period ends.
`reset_daily`|Enum|If a period spans 24 hours, should it reset daily at the end of the time?<ul><li>`0`: No.</li><li>`1` or empty: Yes.</li></ul>

## `parking_hours.txt`
This table defines what hours lots are open.

Parking lots not mentioned in this table are assumed to be open 24/7 to both entrance and exit. Lots mentioned in this table are assumed to be closed, locked, and illegal to occupy except during the mentioned periods.

It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
**`parking_lot_id`**|Required: ID referencing [`parking_lots.parking_lot_id`](#parking_lotstxt)|The ID of the parking lot to which this period applies.
*`parking_period_id`*|Conditionally required: ID referencing [`parking_periods.parking_period_id`](#parking_periodstxt)|The ID of the time period to which this rule applies. If left blank, the rule applies to all times not inside a period.
**`opening_type`**|Required: Enum|How open or closed the lot is at this time:<ul><li>`0` or empty: The lot is open to vehicles entering or exiting.</li><li>`1`: The lot is not open to entry, but people may retrieve and exit with their vehicles.</li><li>`2`: The lot is not open to entry or exit, but vehicles may be stored during this time.</li><li>`3`: The lot is not open to entry or exit, and must not be occupied by any vehicles at this time.</li></ul>
`max_stay`|Positive integer|How long a vehicle may be kept at the lot during this period, expressed in seconds. Deemed to be unlimited (during opening hours) if empty.
`continue_timing`|Enum|Indicates whether the timing should take into account 

## `parking_rates.txt`
This table defines the rates to pay for parking.

It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
**`parking_lot_id`**|Required: ID referencing [`parking_lots.parking_lot_id`](#parking_lotstxt)|The ID of the parking lot to which this rate applies.
*`parking_period_id`*|Conditionally required: ID referencing [`parking_periods.parking_period_id`](#parking_periodstxt)|The ID of the time period to which this rate applies. If left blank, the rate applies to all times not inside a period.
**`currency`**|Required: Currency code|The currency in which the prices in this rule are paid.
`base_rate`|Non-negative float|The fee for parking immediately upon entering the lot. Defaults to `0.00`.
`base_time_secs`|Non-negative integer|The amount of time, in seconds, before the `periodic_rate` takes effect. Defaults to `0`.
`base_carryover`|Enum|Whether a vehicle already parked in the lot at the start of this period is subject to the `base_rate` and `base_time`:<ul><li>`0` or empty: No.</li><li>`1`: Yes.</li></ul>
`periodic_rate`|Non-negative float|The fee assessed periodically for continuing to park after the `base_time` has passed.
`periodic_time_secs`|Positive integer|The amount of time, in seconds, after which `periodic_rate` is charged repeatedly.
`max_rate`|Non-negative float|The maximum rate for this period.
`payment_type`|Enum|Whether the parking fee is paid on entry or exit:<ul><li>`0` or empty: On entry.</li><li>`1`: On exit.</li></ul>

## `parking_fares.txt`
This table defines fares that are offered for free or at a discount when parking at a lot. Multiple fares may be specified for the same lot/period combination.

It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
**`parking_lot_id`**|Required: ID referencing [`parking_lots.parking_lot_id`](#parking_lotstxt)|The ID of the parking lot to which this rule applies.
*`parking_period_id`*|Conditionally required: ID referencing [`parking_periods.parking_period_id`](#parking_periodstxt)|The ID of the time period to which this rule applies. If left blank, the rule applies to all times not inside a period.
**`fare_id`**|Required: ID referencing [`fare_attributes.fare_id`](https://developers.google.com/transit/gtfs/reference/#fare_attributestxt)|The fare given with this parking pass.