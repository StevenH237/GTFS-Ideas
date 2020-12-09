This proposal aims to expand GTFS capabilities through the creation of timetables for viewing schedules.

A transit agency adopting this proposal could maintain multi-route timetables within the same dataset as the rest of the route and schedule information, without needing to update the layout every time one of those routes undergoes time changes.

# Terminology
As used in this document:

* A **table** is one of the files within a GTFS dataset, and the data it contains.
* A **record** is one row of a table within a GTFS dataset.
* A **field** is one column of a table within a GTFS dataset.
* A **timetable** is a schedule table created by this process.
* A **row** denotes a single row, which usually contains one trip on the timetable. For transposed timetables, the term "row" refers to a column, such that a single "row" spans part of the height and all of the width of a timetable.
* A **column** denotes a single column, which usually contains the information at one stop on the timetable. For transposed timetables, the term "column" refers to a row, such that a single "column" spans part of the width and all of the height of a timetable.

# Tables
This proposal contains the following tables.

(Note for "Is required?": Only refers to requirements in the course of using this proposal. Using this proposal is itself optional.)

Table name|Is required?
:-|:-
[**`timetables.txt`**](#timetablestxt)|Yes. This table must be specified or this proposal cannot be used.

## `timetables.txt`
This table defines the actual timetables that need to be defined.

It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
**`timetable_id`**|Required - ID|An ID that uniquely identifies a timetable.
**`timetable_name`**|Required - Text|A user-facing name for the timetable.
`column_order`|Enum|What order the columns should be in, as a value `0` to `3`. See [appendix A](#appendix-a) for details on what the values mean.
`row_order`|Enum|What order the rows should be in, as a alue `0` to `3`. See [appendix A](#appendix-a) for details on what the values mean.
`transposed`|Enum|Whether the table should be rotated (columns and rows swapped):<ul><li>`0` or empty: The table should not be transposed.</li><li>`1`: The table should be transposed.</li></ul>

## `timetable_columns.txt`
This file defines columns that can be used by timetables. Columns can be reused across multiple timetables.

It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
**`column_id`**|Required - ID|The ID of the column defined by this record.
`column_type`|Enum|What type of value this column displays:<ul><li>`0` or empty: The column displays times that vehicles serve a stop.</li><li>`1`: The column displays (in order of precedence) the `route_short_name` or `route_long_name` of the trip.</li><li>`2`: The column displays (in order of precedence) the `trip_short_name`, `route_short_name`, or `route_long_name` of the trip.</li><li>`3`: The column displays a note, which can be defined per-trip in `timetable_trip_notes` or per-column in the `column_value` field of this table. (If both are specified, the former overrides.)</li><li>`4`: The column displays what `route_short_name` (or `route_long_name` if `route_short_name` is unspecified) a vehicle was previously on (using the last trip with the same `block_id` that ends before this one starts), if any.</li><li>`5`: The column displays what `trip_short_name` a vehicle was previously on, if any. If unspecified, it displays `route_short_name` or `route_long_name`.</li><li>`6`: The column displays what route_short_name (or route_long_name if route_short_name is unspecified) a vehicle will next be on (using the next trip with the same `block_id` that starts after this trip ends), if any.</li><li>`7`: The column displays what `trip_short_name` a vehicle will next be on, if any. If unspecified, it displays `route_short_name` or `route_long_name`.</li></ul>
*`column_header`*|Conditionally required - Text|What should this column be called? If specified, it overrides the normal header. If not specified, the highest-precedence stop’s name is used instead. Required for any `column_type` except 0.
`column_prefix`|Text|The text that should be displayed before the value in this column.
`prefix_condition`|Enum|When should the column_prefix be displayed? <ul><li>`0`: The prefix should always be displayed.</li><li>`1`: The prefix should only be displayed if the column has a value.</li><li>`2` or empty: Like 1, but for stops, only if the stop isn’t a timepoint.</li><li>`3`: Like 1, but for stops, only if the stop is a timepoint.</li></ul>
*`column_value`*|Conditionally required - Text|The text that should be displayed if the column is otherwise empty. (Doesn’t trigger `prefix_condition` or `suffix_condition` of 1 or 2).
`column_suffix`|Text|The text that should be displayed after the value in this column.
`suffix_condition`|Enum|When should the column_suffix be displayed? <ul><li>`0`: The suffix should always be displayed.</li><li>`1`: The suffix should only be displayed if the column has a value.</li><li>`2` or empty: Like 1, but for stops, only if the stop isn’t a timepoint.</li><li>`3`: Like 1, but for stops, only if the stop is a timepoint.</li></ul>
`skip_if_empty`|Enum|If the column is empty (besides of `column_values`), should it be skipped? <ul><li>`0`: No, it should always be shown.</li><li>`1` or empty: Yes, it should be skipped if empty.</li></ul>

## `timetable_column_stops.txt`
This file defines which stops are contained in what columns of a timetable.

It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
**`column_id`**|Required - ID referencing [`timetable_columns.column_id`](#timetable_columnstxt)|Which column this record adds a stop to. This column must have a `column_type` of `0`.
**`stop_id`**|Required - ID referencing [`stops.stop_id`](https://developers.google.com/transit/gtfs/reference/#stopstxt)|Which stop this record defines a column for. If the referenced stop is a station, all of its child platforms are included.
**`stop_precedence`**|Required - Integer|If a trip would display multiple times for the `stop_id`s of this column, it will display the time for the lowest `stop_precedence`.
`around_stop_id`|ID referencing [`stops.stop_id`](https://developers.google.com/transit/gtfs/reference/#stopstxt)|Only show a time for this stop in this column if it has the relation to `around_stop_id` described by `around_direction`.
`around_direction`|Enum|Describes the relationship that `stop_id` and `around_stop_id` must have within the trip for the time at `stop_id` to be displayed in this column: <ul><li>`0` or empty: `around_stop_id` must either follow or precede `stop_id`.</li><li>`1`: `around_stop_id` must precede `stop_id`.</li><li>`2`: `around_stop_id` must precede `stop_id` or not be served in the same trip.</li><li>`3`: `around_stop_id` must not be served in the same trip.</li><li>`4`: `around_stop_id` must follow `stop_id` or not be served in the same trip.</li><li>`5`: `around_stop_id` must follow `stop_id` in the same trip.</li></ul>
`only_timepoint`|Enum|What to do if `stop_id` isn't a timepoint on the trip. <ul><li>`0` or empty: This stop's time (or `column_stop_value`, if applicable) is shown.</li><li>`1`: This stop's time is skipped. Lower-precedence stop times can be shown if applicable.</li></ul>
`column_stop_prefix`|Text|If this stop is served and shown, the time for it should be prefixed by this text.
`column_stop_value`|Text|If this stop is served as a non-timepoint, this text should be displayed instead of a time.
`column_stop_suffix`|Text|If this stop is served and shown, the time for it should be suffixed by this text.
`time_type`|Enum|<ul><li>`0`: Times departing from this stop are displayed.</li><li>`1`: Times arriving to this stop are displayed.</li><li>Empty: Times departing from this stop are displayed, except for the last stop of the trip, which uses the arrival time instead.</li></ul>

## `timetable_table_columns.txt`
This file defines which columns each table uses.

It contains the following fields:

Field name|Field type|Field detail
:-|:-|:-
**`timetable_id`**|Required - ID referencing [`timetables.timetable_id`](#timetablestxt)|Which timetable this record adds a column to.
**`column_id`**|Required - ID referencing [`timetable_columns.column_id`](#timetable_columnstxt)|Which column this record adds to a timetable.
**`column_index`**|Required - Integer|What position this column occupies within the timetable.

## `timetable_rows.txt`
This file defines which routes or trips are shown in the timetable.

Conditions in records are independent of each other - a trip that matches all the fields of any single record will be included, unless it matches an `except_trip_id`.

Trips that are matched multiple times are only included in the timetable once. If such trips are added by a `trip_id`, then that record's `sort_time` is used; otherwise, the lowest applicable sort time based on a `timed_stop_id` is used. Trips that don't have any specified `sort_time` or `timed_stop_id` are sorted using their start time.

It contains the following fields:

Field name|Field type|Field detail
:-|:-|:-
**`timetable_id`**|Required - ID referencing [`timetables.timetable_id`](#timetablestxt)|Which timetable this record adds rows to.
*`route_id`*|[Conditionally required¹](#condition-1) - ID referencing [`routes.route_id`](https://developers.google.com/transit/gtfs/reference#routestxt)|Which route’s trips should be added to this timetable.
*`direction_id`*|Conditionally required - Enum|Only trips in the given direction will be added to this timetable. Required iff `route_id` is specified. `direction_id` should match [`trips.direction_id`](https://developers.google.com/transit/gtfs/reference#tripstxt).
`through_stop_id`|ID referencing [`stops.stop_id`](https://developers.google.com/transit/gtfs/reference#stopstxt)|Only trips that serve this stop are added by this record. Only considered if `route_id` is specified.
`except_stop_id`|ID referencing [`stops.stop_id`](https://developers.google.com/transit/gtfs/reference#stopstxt)|Trips that serve this stop do not match this record. Only considered if `route_id` is specified.
`timed_stop_id`|Optional - ID referencing [`stops.stop_id`](https://developers.google.com/transit/gtfs/reference#stopstxt)|Sets the sort time of these trips to the time they serve `timed_stop_id`. Only considered if `route_id` is specified. Trips that don’t serve `timed_stop_id` are sorted based on other records’ `timed_stop_id`s or their start time if no other time is specified. If `timed_stop_id` is served multiple times, the first instance is used.
`timed_stop_offset`|Time preceded by a `+` or `-`|How much to add or subtract from the time `timed_stop_id` is served for sorting purposes.
`only_after_time`|Time|Only trips with a sort time after (or equal to) the time specified are added by this record.
`only_before_time`|Time|Only trips with a sort time before (or equal to) the time specified are added by this record.
*`trip_id`*|[Conditionally required¹](#condition-1) - ID referencing [`trips.trip_id`](https://developers.google.com/transit/gtfs/reference#tripstxt)|Which specific trip this record should add to the timetable.
`sort_time`|Time|A specific time this trip should be sorted at, which doesn’t need to be a time between the first and last stops. Only considered if `trip_id` is specified.
*`except_trip_id`*|[Conditionally required¹](#condition-1) - ID referencing [`trips.trip_id`](https://developers.google.com/transit/gtfs/reference#tripstxt)|Which specific trip this record should remove from the timetable.

<span id="condition-1">¹Each record must specify exactly one of route_id, trip_id, or except_trip_id.</span>

## `timetable_notes.txt`
This file defines notes to be inserted into or displayed alongside timetables.

It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
*`timetable_id`*|Conditionally required - ID referencing [`timetables.timetable_id`](#timetablestxt)|Which timetable this record adds a note to. Required unless the note is tied to a trip_id (in which case it’s optional - if unspecified, the note is inserted in all timetables in which the specified trip and column both appear).
**`note_id`**|Required - ID referencing timetable_note_text.note_id): The ID to the text of the note.
*`header_note_order`*|[Conditionally required²](#condition-2) - Integer|For notes at the top of a timetable, they should be displayed in this order. The column headers themselves occupy row 0.
*`note_time`*|[Conditionally required²](#condition-2) - Time|For notes interspersed within a schedule, they should be sorted among the trips as if they’re this time. (If a `note_time` matches a trip’s `sort_time`, the trip should come first.)
*`footer_note_order`*|[Conditionally required²](#condition-2) - Integer|For notes at the bottom of a timetable, they should be displayed in this order.
*`trip_id`*|[Conditionally required²](#condition-2) - ID referencing [`trips.trip_id`](https://developers.google.com/transit/gtfs/reference#tripstxt)|For the notes column of a time table, this field determines what trip the note is added to.
`column_index`|Integer|If specified, the note occupies only this column within the given row. Multiple notes can occupy the same row iff they each have a different `column_index`. Ignored if `column_id` is specified.
*`column_id`*|Conditionally required - ID referencing [`timetable_columns.column_id`](#timetable_columnstxt)|Same as `column_index`, except that a column is referenced by its ID instead. If this column isn’t in the timetable, this note gets skipped. The referenced column must have a `column_type` of 3. For notes tied to a specific `trip_id`, this field is required.
`service_id`|Optional - ID referencing [`calendar.service_id`](https://developers.google.com/transit/gtfs/reference#calendartxt) or [`calendar_dates.service_id`](https://developers.google.com/transit/gtfs/reference#calendar_datestxt)|If specified, the note is only displayed on days matching this service pattern. Ignored if the note is tied to a trip.

<span id="condition-2">²Each record must specify exactly one of `header_note_order`, `note_time`, `footer_note_order`, or `trip_id`.</span>

## `timetable_note_text.txt`
An extra table allowing the same note to be reused in multiple places and translated.

It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
**`note_id`**|Required - ID|The ID of the note.
**`note_text`**|Required - Text|The text of the note.

# Translations
The following change to `translations.txt` is proposed to account for additional tables from this proposal:

`table_name`|ID field for `record_id`|ID field for `record_sub_id`
:-|:-|:-
[`timetables`](#timetablestxt)|`timetable_id`|NONE
[`timetable_columns`](#timetable_columnstxt)|`timetable_column_id`|NONE
[`timetable_note_text`](#timetable_note_texttxt)|`note_id`|NONE

# Implementation notes
Date / service ID information isn’t part of a timetable. Instead, the information in these tables can be used to procedurally generate timetables for specific dates.

Routes not represented in a defined timetable can be in a default timetable instead.

# Appendices

## Appendix A
**Column and row ordering**

The values of [`timetables.column_order` and `timetables.row_order`](#timetablestxt) have the following meanings. "Reading order" refers to the order in which letters appear in the displayed language; that is, "left to right" for tables displayed in languages such as English, and "right to left" for tables displayed in languages such as Hebrew.

### `column_order`
Value|Non-transposed table|Transposed table
:-|:-|:-
`0` or empty|Reading order|Top to bottom
`1`|Reverse reading order|Bottom to top
`2`|Left to right|Top to bottom
`3`|Right to left|Bottom to top

### `row_order`
Note: The row order affects headers and footers too.

Value|Non-transposed table|Non-transposed table
:-|:-|:-
`0` or empty|Top to bottom|Reading order
`1`|Bottom to top|Reverse reading order
`2`|Top to bottom|Left to right
`3`|Bottom to top|Right to left
