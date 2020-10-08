This file is designed to allow GTFS consumers a more efficient way to define trips being affected by daylight saving time.

# The table
## `daylight_saving.txt`

Field name|Field type|Field details
:-|:-|:-
**`trip_id`**|Required: ID referencing `trips.trip_id`|The ID of the trip to which this rule applies.
**`clock_shift_direction`**|Required: Enum|The direction clocks are shifted to trigger this rule:<ul><li>`0` for forward shifts, where some times are skipped. Usually happens in the spring.</li><li>`1` for backward shifts, where times happen twice in a night. Usually happens in the fall.</li></ul>
`copy_trip`|Enum|Whether or not the trip should be copied for this clock shift:<ul><li>`0` or empty: Do not copy trip.</li><li>`1`: Copy trip.</li></ul>