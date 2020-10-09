This proposal exists to document fares that are applicable only at certain times of day.

It is important to note that feed consumers that don't support this proposal will always display the cheaper fare of any of the timed options that apply to the same routes.

# Tables
## `fare_times.txt`
This table lists the times that fares are active. `fare_id`s not listed in this table are considered active 24/7; `fare_id`s listed in this table are active only during any of the times specified.

It contains the following fields:

Field name|Field type|Field detail
:-|:-|:-
`fare_id`|ID referencing [`fare_attributes.fare_id`](https://developers.google.com/transit/gtfs/reference/#fare_attributestxt)|The fare that is modified by this rule.
`service_id`|ID referencing [`calendar.service_id`](https://developers.google.com/transit/gtfs/reference/#calendartxt) or [`calendar_dates.service_id`](https://developers.google.com/transit/gtfs/reference/#calendardatestxt)|The days that this rule is active.
`start_time`|Time|The time this fare starts being active.
`end_time`|Time|The time this fare stops being active. Does not include the exact second specified.
`strict_times`|Enum|Whether times should be strictly applied to match the ones in [`stop_times.txt`](https://developers.google.com/transit/gtfs/reference/#stop_timestxt). For example, a value of `0` means that fares applicable at 03:00 Saturday can be applied to trips scheduled for 27:00 Friday, and vice versa.<ul><li>`0` or empty: Do not strictly apply times.</li><li>`1`: Strictly apply times.</li></ul>
`mid_trip_timing`|Enum|Whether fares should apply mid-trip or not.<ul><li>`0`: Apply fares to trips that start within the given times.</li><li>`1` or empty: Apply fares to stops made at the given times.</li><li>`2`: Apply fares to trips that end within the given times.</li></ul>
`continue_transfers`|Enum|Whether or not fares paid while active but have transfer times exceeding the allotted time should continue to be active on later trips.