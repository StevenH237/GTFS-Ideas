Several proposals in this repo use "periods", which are - simply put - periods of time during which certain rules may change. This proposal is a common prerequisite for those.

# The table

## `periods.txt`
This table describes the actual periods of time in question.

It contains the following fields:
Field name|Field type|Field details
:-|:-|:-
**`period_id`**|Required: ID|The ID for the period of time.
**`service_id`**|Required: ID referencing [`calendar.service_id`](https://developers.google.com/transit/gtfs/reference/#calendartxt) or [`calendar_dates.service_id`](https://developers.google.com/transit/gtfs/reference/#calendardatestxt)|The days that this rule is active.
`start_time`|Conditionally required: Time|The time this period starts being active. If omitted, treated as start-of-service for strictly timed trips, and `00:00:00` for non-trips or non-strict periods.
`end_time`|Conditionally required: Time|The time this period stops being active. Not inclusive of that exact second. If omitted, treated as end-of-service for strictly timed trips, and `24:00:00` for non-trips or non-strict periods.
`strict_times`|Enum|If the period is used for trips, this field states whether times should be strictly applied to match the ones in [`stop_times.txt`](https://developers.google.com/transit/gtfs/reference/#stop_timestxt). For example, a value of `0` means that periods applicable at 03:00 Saturday can be applied to trips scheduled for 27:00 Friday, and vice versa.<ul><li>`0` or empty: Do not strictly apply times.</li><li>`1`: Strictly apply times.</li></ul>
`mid_trip_timing`|Enum|If the period is used for trips, this field states whether periods should start/stop applying mid-trip or not.<ul><li>`0`: Apply periods to whole trips that start within the given times.</li><li>`1` or empty: Apply periods to stops served at the given times.</li><li>`2`: Apply periods to whole trips that end within the given times.</li></ul>
`period_name`|Text|A human-readable name for this period, for example "AM peak hours".

# Translations
The following change to `translations.txt` is proposed to account for additional tables from this proposal:

`table_name`|ID field for `record_id`|ID field for `record_sub_id`
:-|:-|:-
[`periods`](#periodstxt)|`period_id`|NONE
