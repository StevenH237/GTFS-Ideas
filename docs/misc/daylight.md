This file is designed to allow GTFS consumers a more efficient way to define trips being affected by daylight saving time.

# The table
## `daylight_saving.txt`

Field name|Field type|Field details
:-|:-|:-
**`trip_id`**|Required: ID referencing `trips.trip_id`|The ID of the trip to which this rule applies.
**`clock_shift_direction`**|Required: Enum|The direction clocks are shifted to trigger this rule:<ul><li>`0` for forward shifts, where some times are skipped. Usually happens in the spring.</li><li>`1` for backward shifts, where times happen twice in a night. Usually happens in the fall.</li></ul>
`day_offset`|Integer|The changes described in this record occur on the day such that (the actual date of dst change) + (the value of this field) = (the date of the trip being changed).
`shift_trip`|Integer|This field indicates the number of seconds forward (backward if negative) the trip should change for the clock shift. Defaults to `0`.
`copy_trip`|Enum|Whether or not the trip should be copied for this clock shift (e.g. because the day is longer):<ul><li>`0` or empty: Do not copy trip.</li><li>`1`: Copy trip.</li></ul>
`copy_offset`|Integer|This field indicates the number of seconds forward (backward if negative) the copy of the trip should change for the clock shift. Times are relative to the trip's original time and default to the negative amount by which clocks change (usually, but not always, `-3600`).
`delete_trip`|Enum|Whether or not the trip should be deleted for this clock shift (e.g. because the day is shorter):<ul><li>`0` or empty: Do not delete trip.</li><li>`1`: Delete trip.</li></ul>
`retimed_scheduling`|Enum|Whether or not to retime the schedules for remaining stops based on the clock shift. Retiming means that a forward clock shift makes vehicles late, and a backward shift makes vehicles wait.<ul><li>`0` or empty: Do not retime the trip. The whole trip will operate based on the original utc offset from when it started.</li><li>`1`: Retime the trip based on agency timezone.</li><li>`2`: Retime the trip based on stop timezone.</li></ul>
