These tables expand representation of fare zones so that complex systems don't need hundreds of lines for representation.

While these tables aren't explicitly supported by the GTFS Standard, this proposal aims to be completely convertible into standards-compliant GTFS.

# Note
The types of the `zone_id` field of `stops`, and the `origin_id`, `destination_id`, and `contains_id` fields of `fare_rules`, are now "ID referencing `fare_zones.zone_id`". If the `fare_zones` table does not exist, it can be created by the consumer at runtime.

# The tables

## Table of Contents
Table name|Requirement details
:-|:-
**[`fare_zones`](#fare_zonestxt)**|Required to use this proposal whatsoever.
[`fare_zone_numbering`](#fare_zone_numberingtxt)|Optional.
[`fare_zone_numbering_rules`](#fare_zone_numbering_rulestxt)|Optional.
[`fare_zone_inheritance`](#fare_zone_inheritancetxt)|Optional.

## `fare_zones.txt`
This table creates the various zones that can be referenced within a file.

If this table does not exist, it can be created by taking the distinct values of `stops.zone_id` as the `fare_zones.zone_id`s, and leaving the other columns blank.

It contains the following fields:

Field name|Field type|Field detail
:-|:-|:-
**`zone_id`**|Required: ID|A dataset unique ID that represents this fare zone.
`zone_name`|Text|A human-readable name for this fare zone.

## `fare_zone_numbering.txt`
This table assigns numbers to fare zones. Multiple zones may occupy the same number, multiple numbers may be assigned to the same zone, and multiple distinct numbering schemes may exist.

This table contains the following fields:

Field name|Field type|Field detail
:-|:-|:-
**`zone_id`**|Required: ID referencing [`fare_zones.zone_id`](#fare_zonestxt)|The ID referenced by this record.
*`zone_scheme_id`*|Conditionally required: ID|The ID of this fare-zone numbering scheme. If specified for any zones, it's required for all. Otherwise, all zones are considered part of the same numbering system.
**`zone_number`**|Required: Integer|The number of this zone.

## `fare_zone_numbering_rules.txt`
This table can be used to assign fares to trips based on the numbers of zones in the trip. Trips may follow any single rule to qualify for that fare.

It contains the following fields:

Field name|Field type|Field detail
:-|:-|:-
**`fare_id`**|Required: ID referencing [`fare_attributes.fare_id`](https://developers.google.com/transit/gtfs/reference/#fare_attributestxt)|The fare that this rule applies to.
*`zone_scheme_id`*|Conditionally required: ID referencing [`fare_zone_numbering.zone_scheme_id`]|The numbering scheme that this rule applies to. Required iff the `zone_scheme_id` is specified for zones.
*`min_zone_number`*|Conditionally required: Integer|The lowest number zone the trip may contain while still following this rule.
*`max_zone_number`*|Conditionally required: Integer|The highest number zone the trip may contain while still following this rule.
*`zone_difference`*|Conditionally required: Integer|The exact difference between starting and ending zone numbers for the trip to follow this rule.
*`min_zone_difference`*|Conditionally required: Integer|The minimum difference between starting and ending zone numbers for the trip to follow this rule. Ignored if `zone_difference` is set.
*`max_zone_difference`*|Conditionally required: Integer|The maximum difference between starting and ending zone numbers for the trip to follow this rule. Ignored if `zone_difference` is set.
*`zone_count`*|Conditionally required: Integer|The exact number of unique zone numbers the trip passes through (including start and end) to follow this rule.
*`min_zone_count`*|Conditionally required: Integer|The minimum number of unique zone numbers the trip passes through (including start and end) to follow this rule. Ignored if `zone_count` is set.
*`max_zone_count`*|Conditionally required: Integer|The maximum number of unique zone numbers the trip passes through (including start and end) to follow this rule. Ignored if `zone_count` is set.
`zone_difference_absolute`|Enum|Whether or not the differences are absolute value differences:<ul><li>`0`: No, the sign of (start zone)-(end zone) must match the values provided in this record (trips may occur in one direction).</li><li>`1` or empty: Yes, take the absolute value (trips may occur in either direction).</li></ul>
`route_id`|ID referencing [`routes.route_id`](https://developers.google.com/transit/gtfs/reference/#routestxt)|The route to which this rule applies. If not specified, applies to a whole itinerary.

Conditionally required: At least one of `min_zone_number`, `max_zone_number`, `zone_difference`, `min_zone_difference`, `max_zone_difference`, `zone_count`, `min_zone_count`, or `max_zone_count` must be specified.