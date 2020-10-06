This table can be used to indicate the rules of using transit with a bicycle.

# The table

## `bicycle_rules.txt`
This table defines the actual rules.

It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
**`rule_sort_order`**|Required: Integer|The order in which to apply the rules. Applicable rules are applied to a trip in order from lowest to highest, and override the trip's own `bikes_allowed` value. These values must be unique.
`agency_id`|ID referencing `agency.agency_id`|The ID of the agency whose routes to which a rule applies.
`route_type`|Enum, same as `routes.route_type`|The route type to which a rule applies.
`route_id`|ID referencing `routes.route_id`|The ID of the route to which a rule applies. If set, `agency_id` and `route_type` are ignored, whether or not they match this route.
`direction_id`|Enum, same as `trips.direction_id`|The route direction to which a rule applies.
`trip_id`|ID referencing `trips.trip_id`|The ID of the trip to which a rule applies. If set, `agency_id`, `route_id`, `route_type`, and `direction_id` are ignored, whether or not they match this trip.
`service_id`|ID referencing `calendar.service_id` or `calendar_dates.service_id`|The set of days to which a rule applies. Ignored if `trip_id` is set.
`strict_service`|Enum|Whether or not the `service_id` should be taken strictly:<ul><li>`0` or empty: This rule applies to any trip on the days of `service_id`.</li><li>`1`: This rule applies only to trips with the same `service_id` as specified.</li></ul>
`after_time`|Time|The time after which this rule is applied.
`before_time`|Time|The time before which this rule is applied. If this falls before `after_time`, then the rule is applied outside of the range between the two.
`strict_time`|Enum|Whether or not the times should be taken strictly:<ul><li>Empty: Follows the same value as `strict_service`.</li><li>`0`: Applies to the same time of day (i.e. a trip scheduled for `28:00:00` happens between `03:00:00` and `05:00:00`).</li><li>`1`: Applies only to trips specifying `stop_times` matching `after_time`/`before_time` (i.e. `28:00:00` isn't between `03:00:00` and `05:00:00`).
`trip_after_alignment`|Enum|How the `after_time` rule should handle trips during the starting time:<ul><li>`0` or empty: Trips that are running during the `after_time` will start enforcing the rule mid-trip at that time.</li><li>`1`: The rule applies to trips that start at or after the `after_time`.</li><li>`2`: The rule applies to trips that end at or after the `after_time`.</li></ul>
`trip_before_alignment`|Enum|How the `before_time` rule should handle trips during the ending time:<ul><li>`0` or empty: Trips that are running during the `before_time` will stop enforcing the rule mid-trip at that time.</li><li>`1`: The rule applies to trips that end at or before the `before_time`.</li><li>`2`: The rule applies to trips that start at or before the `before_time`.</li></ul>
`after_stop_id`|ID referencing `stops.stop_id`|The ID of the stop after which this rule is applied. The rule includes the specified stop.
`before_stop_id`|ID referencing `stops.stop_id`|The ID of the stop before which this rule is applied. The rule includes the specified stop.
`at_stop_id`|ID referencing `stops.stop_id`|The ID of the stop at which this rule is exclusively applied. Can also be used by itself when indicating a station rule.
`bikes_allowed`|Enum|For a route:<ul><li>`0`: Bikes may not be carried on the vehicle.</li><li>`1`: At least one bike may be carried on the vehicle.</li></ul> For a station:<ul><li>`0`: Bikes may not be present within the station.</li><li>`1`: Bikes may be present within the station.</li></ul>
`bike_capacity`|Integer|The maximum number of bikes that can be carried by the vehicle at this time.
`bike_boarding`|Enum|For a route:<ul><li>`0`: Bikes may not board the vehicle here.</li><li>`1`: Bikes may board the vehicle here.</li></ul> For a station:<ul><li>`0`: Bikes may not board vehicles from this station.</li><li>`1`: Bikes may board vehicles from this station.</li></ul>
`bike_deboarding`|Enum|For a route:<ul><li>`0`: Bikes may not deboard the vehicle here.</li><li>`1`: Bikes may deboard the vehicle here.</li><li>`2`: Bikes *must* deboard the vehicle here.</li></ul> For a station:<ul><li>`0`: Bikes may not deboard vehicles into this station.</li><li>`1`: Bikes may deboard vehicles into this station.</li></ul>Please be aware of local laws that may make this unenforceable.
`bike_entry`|Enum|Ignored for routes. For stations:<ul><li>`0`: Bikes may not enter the station from the street.</li><li>`1`: Bikes may enter the station from the street.</li></ul>
`bike_exit`|Enum|Ignored for routes. For stations:<ul><li>`0`: Bikes may not exit the station to the street.</li><li>`1`: Bikes may exit the station to the street.</li><li>`2`: Bikes *must* exit the station to the street.</li></ul>

Be sure to specify all the rules that change as each individual rule will inherit its last value.