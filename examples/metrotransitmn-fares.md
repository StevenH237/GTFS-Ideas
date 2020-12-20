This file documents the fares of Metro Transit, MN - including how timing and demographics affect it. This file does *not* include passes.

# Disclaimer
This file was not created with any input, permission, or endorsement from Metro Transit of Minneapolis/St. Paul, MN. It has not been reviewed for accuracy and no guarantees are made of such. The author of this document has never even set foot in that state.

# The tables
Only relevant tables are shown here. IDs from other tables will be referenced using {} describing which item(s) the record is pointing to - for multiple IDs, assume multiple rows in the table.

Table name|From proposal
:-|:-
[periods.txt](#periodstxt)|[Periods](../docs/common/periods.md#periodstxt)
[fare_attributes.txt](#fare_attributestxt)|[GTFS Standard](https://developers.google.com/transit/gtfs/reference/#fare_attributestxt)
[fare_rules.txt](#fare_rulestxt)|[GTFS Standard](https://developers.google.com/transit/gtfs/reference/#fare_rulestxt)
[fare_times.txt](#fare_timestxt)|[Timed fares](../docs/fares/timed.md#fare_timestxt)
[fare_demographics.txt](#fare_demographicstxt)|[Fare demographics](../docs/fares/demographics.md#fare_demographicstxt)
[fare_demographic_prices.txt](#fare_demographic_pricestxt)|[Fare demographics](../docs/fares/demographics.md#fare_demographic_pricestxt)

## `periods.txt`
`period_id`|`service_id`|`start_time`|`end_time`|`period_name`
:-|:-|:-|:-|:-
`pd_am_rush`|{Weekdays}|06:00:00|09:00:00|AM Rush Hours
`pd_pm_rush`|{Weekdays}|15:00:00|18:30:00|PM Rush Hours

## `fare_attributes.txt`
`fare_id`|`price`|`currency_type`|`payment_method`|`transfers`|`transfer_duration`
:-|:-|:-|:-|:-|:-
`f_local`|2.50|USD|`0` (at stop)||9000
`f_express`|3.25|USD|`0` (at stop)||9000
`f_nicolet`|0.00|USD|`0` (at stop)|0|
`f_downtown`|0.50|USD|`0` (at stop)|0|

## `fare_rules.txt`
`fare_id`|`route_id`|`origin_id`|`destination_id`
:-|:-|:-|:-
`f_local`|{all Local routes and METRO}||
`f_express`|{all routes}||
`f_nicolet`|{Route 18}|{Northbound stops: Nicolet/Grant to Hennepin/Washington}
`f_nicolet`|{Routes 10 or 59}|{Southbound stops: Hennepin/Washington to Convention Center}
`f_downtown`||{Downtown Minneapolis}|{Downtown Minneapolis}
`f_downtown`||{Downtown St. Paul}|{Downtown St. Paul}

## `fare_times.txt`
`fare_id`|`period_id`|`adjusted_price`|`continue_transfers`
:-|:-|:-|:-
`f_local`|`pd_am_rush`|2.50|`1` (yes)
`f_local`|`pd_pm_rush`|2.50|`1` (yes)
`f_local`||2.00|`1` (yes)
`f_express`|`pd_am_rush`|3.25|`1` (yes)
`f_express`|`pd_pm_rush`|3.25|`1` (yes)
`f_express`||2.50|`1` (yes)
