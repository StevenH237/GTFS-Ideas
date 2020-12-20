This file documents the fares of the Regional Transit of Metro Detroit ("RTMD") systems, including how demographics and passes affect it.

# Disclaimer
This file *is* fully endorsed by the RTMD. That's because RTMD is a fantasy system created by the same person as this file.

This file is *not* referring to the Regional Transit Authority ("RTA") of Southeast Michigan. The RTA is not related to, does not endorse or approve of, and is not endorsed or approved by, the RTMD or its creator.

# The tables
Only relevant tables are shown here. IDs from other tables will be referenced using {} describing which item(s) the record is pointing to - for multiple IDs, assume multiple rows in the table.

Table name|From proposal
:-|:-
[fare_attributes.txt](#fare_attributestxt)|[GTFS Standard](https://developers.google.com/transit/gtfs/reference/#fare_attributestxt)
[fare_rules.txt](#fare_rulestxt)|[GTFS Standard](https://developers.google.com/transit/gtfs/reference/#fare_rulestxt)
[fare_demographics.txt](#fare_demographicstxt)|[Fare demographics](../docs/fares/demographics.md#fare_demographicstxt)
[fare_demographic_prices.txt](#fare_demographic_pricestxt)|[Fare demographics](../docs/fares/demographics.md#fare_demographic_pricestxt)
[passes.txt](#passestxt)|[Passes](../docs/fares/passes.md#passestxt)
[pass_fares.txt](#pass_farestxt)|[Passes](../docs/fares/passes.md#pass_farestxt)
[pass_period_times.txt](#pass_period_timestxt)|[Passes](../docs/fares/passes.md#pass_period_timestxt)
[pass_periods.txt](#pass_periodstxt)|[Passes](../docs/fares/passes.md#pass_periodstxt)

## `fare_attributes.txt`
`fare_id`|`price`|`currency_type`|`payment_method`|`transfers`|`agency_id`
:-|:-|:-|:-|:-|:-
`f_free`|0.00|USD|`0`|0|RTMD
`f_dtwn`|2.00|USD|`0`|0|RTMD
`f_nxs`|2.00|USD|`0`|0|RTMD
`f_dex_1z`|2.50|USD|`0`|0|RTMD
`f_dex`|2.50|USD|`0`|0|RTMD
`f_mower_std`|3.00|USD|`0`|0|RTMD
`f_mower_exu`|3.00|USD|`0`|0|RTMD
`f_mower`|3.00|USD|`0`|0|RTMD
`f_wex`|5.00|USD|`0`|0|RTMD

## `fare_rules.txt`
`fare_id`|`route_id`|`origin_id`|`destination_id`
:-|:-|:-|:-
`f_free`||`airport`|`airport`
`f_free`||`orchard_ridge`|`orchard_ridge`
`f_dtwn`||`downtown`|`downtown`
`f_nxs`|NXS routes: 1 to 750||
`f_dex`|DEX routes: 800 to 896||
`f_mower`|MOWER routes: 900 to 944||
`f_wex`|WEX routes: 950 to 952||
`f_dex_1z`|DEX routes|Any zone in [sub-table 1](#sub-table-1)|The same zone
`f_dex_1z`|DEX routes|Any zone in [sub-table 2](#sub-table-2)|An immediately adjacent zone
`f_mower_std`|MOWER routes|`standard`|`standard`
`f_mower_exu`|MOWER routes|`standard`|`exurban`
`f_mower_exu`|MOWER routes|`exurban`|`standard`
`f_mower_exu`|MOWER routes|`exurban`|`exurban`
`f_mower`|MOWER routes|`standard`|`regional`
`f_mower`|MOWER routes|`exurban`|`regional`
`f_mower`|MOWER routes|`regional`|`regional`
`f_mower`|MOWER routes|`regional`|`exurban`
`f_mower`|MOWER routes|`regional`|`standard`
`f_mower_exu`|MOWER routes|`standard`|`flat_rock`
`f_mower_exu`|MOWER routes|`flat_rock`|`standard`
`f_mower_exu`|MOWER routes|`flat_rock`|`flat_rock`
`f_mower_exu`|MOWER routes|`flat_rock`|`exurban`
`f_mower_exu`|MOWER routes|`exurban`|`flat_rock`
`f_mower`|MOWER routes|`flat_rock`|`regional`
`f_mower`|MOWER routes|`regional`|`flat_rock`

### Sub-table 1
The listing of same zones to which `f_dex_1z` applies:

Zone|Zone|Zone|Zone
:-|:-|:-|:-
`ann_arbor`|`auburn_hills`|`chesterfield`|`dearborn`
`detroit_dearborn`|`eastpointe`|`exurban`|`fairlane_green`
`farmington`|`five_points`|`greenfield_village`|`grosse_pointe`
`highland_park`|`midtown`|`millennium_park`|`mt_clemens`
`plymouth`|`pontiac`|`royal_oak`|`southfield_civic_center`
`southgate`|`suburban`|`troy`|`wixom`
`ypsilanti`|&nbsp;|&nbsp;|&nbsp;

### Sub-table 2
The listing of different zones to which `f_dex_1z` applies - entries are two consecutive items in this list, or two consecutive odd-numbered items in this list, in both orders:

1. `downtown`
2. `detroit`
3. `detroit_dearborn`
4. `dearborn`
5. `dearborn_suburban`
6. `suburban`
7. `suburban_exurban`
8. `exurban`
9. `exurban_ypsilanti`
10. `ypsilanti`

# zone listing
* 12_mile_van_dyke
* airport
* ann_arbor
* auburn_hills
* chesterfield
* dearborn
* dearborn_suburban
* detroit
* detroit_dearborn
* downtown
* eastpointe
* exurban
* exurban_ypsilanti
* fairlane_green
* farmington
* five_points
* flat_rock
* greenfield_village
* grosse_point
* grosse_pointe
* highland_park
* lakeland
* madonna_university
* midtown
* millennium_park
* mt_clemens
* northville
* novi
* oakland_mall
* orchard_ridge
* outer_drive_wcccd
* plymouth
* pontiac
* roseville
* royal_oak
* southfield
* southfield_civic_center
* southfield_grand_river
* southfield_greyhound
* southgate
* standard
* suburban
* suburban_exurban
* troy
* windsor
* wixom
* woodhaven
* ypsilanti
