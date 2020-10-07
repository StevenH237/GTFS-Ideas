These tables document the various passes, fare caps, and bulk discounts that are available for the transit agency(ies).

# The tables

## Table of Contents
<!-- The pun is entirely intended. -->
Table name|Requirement details
:-|:-
**[`passes.txt`](#passestxt)**|Required to use this proposal whatsoever.
[`pass_constraints.txt`](#pass_constraintstxt)|Optional, only needed if constraints on purchasing passes actually exist.
*[`pass_fares.txt`](#pass_farestxt)*|Required unless all passes fully cover all fares.
[`pass_limit_groups.txt`](#pass_limit_groupstxt)|Optional, only useful for advanced limits.
[`pass_locations.txt`](#pass_locationstxt)|Optional, used to indicate places passes may be purchased.
*[`pass_period_times.txt`](#pass_period_timestxt)*|Required unless all passes are quantity-based or completely unlimited.
*[`pass_periods.txt`](#pass_periodstxt)*|Required unless all passes are quantity-based or completely unlimited.
[`pass_vehicle_purchase.txt`](#pass_vehicle_purchasetxt)|Optional, only needed if there are multiple vehicle-purchase groups.

## `passes.txt`
This table defines the passes.

Field name|Field type|Field deails
:-|:-|:-
**`pass_id`**|Required: ID|The ID that refers to this pass.
**`pass_price`**|Required: Float|The cost of the pass, in a currency to be specified in the next field.
**`pass_currency_type`**|Required: Currency code|The currency in which the pass is purchased.
**`pass_name`**|Required: Text|The name of the pass.
`fares_to_pass`|Integer|Up to this many fares may be retroactively applied towards the purchase of the pass, reducing the cost of the pass by the sum of those fares. Defaults to `0`. Only counts fares that are covered by the pass, and only the portion of fare that it covers.
`fare_cap`|Enum|Whether or not the pass is a fare cap, which means that instead of being a pass purchased in advance, it is paid for by rides it covers and then takes full effect when its cost has been paid in rides within the time period. <ul><li>`0` or empty: The pass is not a fare cap.</li> <li>`1`: The pass is a fare cap.</li></ul>
`activation_time`|Enum|This field indicates when the timer of a pass is considered to be activated. <ul><li>`0` or empty: A pass’ timer is activated by the first fare it covers. For passes where previously purchased fares are applied, or for fare caps, this is the earliest fare used to purchase or build up to the pass. Fares that would make the pass expire before purchase cannot be used to purchase the pass.</li> <li>`1`: A pass’ timer is activated during a fixed period of time, usually set during its creation.</li> <li>`2`: A pass’ timer is activated at the time of purchase.</li> <li>`3`: A pass’ timer is activated by the first fare after its purchase.</li> <li>`4`: A pass’ timer is activated when any of its limited-quantity trips are exhausted.</li> <li>`5`: A pass’ activation time is set to the same as the pass used to purchase it, if you must already have a pass to get a new one. For passes not purchased with other passes, treated as `0`.</li>
`period_count`|Integer|How many periods (defined in `pass_period_times.txt`) the pass’s timer is valid for. Defaults to `1`.

## `pass_period_times.txt`
This file documents the various periods of validity for bulk passes.

Periods have two phases: An activation phase and a validity phase. The activation phase is the period of time during which a pass can be activated, and the validity phase is the period of time during which an already-active pass continues to be valid outside of the activation phase.

A pass can have multiple periods; if their activation phases overlap, then any overlap time is considered to be part of the most-recently-started period.

It contains the following fields:

Field name|Field type|Field description
:-|:-|:-
**`period_id`**|Required: ID|The ID of the period being described by this record.
`start_extended_day`|Enum|Whether or not trips after midnight following a given schedule day still count as that schedule day or their actual calendar date for pass activation:<ul><li>`0` or empty: They count as the calendar day.</li><li>`1`: They count as the schedule day.</li></ul>
`plus_month_end`|Enum|Whether or not validity phases extended by months should end at the start or the end time of the activation phase:<ul><li>`0` or empty: Validity phases extended by months end at the same time-of-day and day-of-month as the start of the activation phase.</li><li>`1`: Validity phases extended by months end at the same time-of-day and day-of-month as the end of the activation phase.</li></ul>
`plus_extended_day`|Enum|Whether or not passes that are valid at the end of a day are valid for all trips on that day's schedule (even those after midnight):<ul><li>`0` or empty: No, when the pass' validity phase ends, the time is up.</li><li>`1`: Yes, passes that are valid at the end of the day are valid on all trips scheduled that day.</li></ul>If `start_extended_day` is `1`, this value is ignored and treated as `1` as well.

### Validity start times
These fields are all optional text fields. They define the time at which a new period starts, but only times that match all of them count. These follow [cron](https://crontab.guru/) syntax, except that `,` is replaced with `;` - `*` means any value is valid, `/` means every nth value is valid, `-` indicates a range of values, and `;` separates multiple valid values. Note that the defaults cause a new period every minute.

Field name|Field description
:-|:-
`start_year`|The year during which a new period starts. Defaults to `*`. Note: A period that defines specific years happens only once!
`start_month`|The month during which a new period starts. Defaults to `*` if `start_year` is `*`; `1` otherwise.
`start_day`|The day of the month during which a new period starts. Defaults to `*` if `start_month` is `*`; `1` otherwise.
`start_week`|<span id="start-week"></span>The week during which a new period starts. Weeks are counted as the first Sunday of the year being the start of week `1`; anytime on/after January 1st but before the first Sunday is week `0`. Note that day-of-week `7` counts a Sunday as part of the same week as the preceding day (except that Sunday, January 1st as day-of-week `7` is part of week `0`). Defaults to `*`.
`start_day_of_week`|<span id="start-day-of-week"></span>The day of the week during which a new period starts. Defaults to `*` if `start_week` is `*`; `1` otherwise. `0` is Sunday, `1` is Monday, etc., `6` is Saturday, and `7` is the following Sunday.
`start_hour`|The hour during which a new period starts. Defaults to `*` if both `start_day` and `start_day_of_week` are `*`; `0` otherwise.
`start_minute`|The minute during which a new period starts. Defaults to `*` if `start_hour` is `*`; `0` otherwise.
`start_second`|The second during which a new period starts. Always defaults to `0`.

### Validity end times
These fields can be used to specify when the activation phases of periods end. All are optional text fields. A period's activation phase ends when either the next one begins or an end time passes, whichever comes first. Passes cannot be activated during times when an end time has happened more recently than a start.

Field name|Field description
:-|:-
`end_year`|The year during which an activation phase ends. Defaults to `*`.
`end_month`|The month during which an activation phase ends. Defaults to the value that `start_month` uses.
`end_day`|The day of the month during which an activation phase ends. Defaults to the value that `start_day` uses.
`end_week`|The week during which an activation phase starts. See [`start_week`](#start-week) for details on how weeks are defined. Defaults to the value that `start_week` uses.
`end_day_of_week`|The day of the week during which an activation phase ends. See [`start_day_of_week`](#start-day-of-week) for details on how days of the week are numbered. Defaults to the value that `start_day_of_week` uses.
`end_hour`|The hour during which an activation phase ends. Defaults to the value that `start_hour` uses.
`end_minute`|The minute during which an activation phase ends. Defaults to the value that `start_minute` uses.
`end_second`|The second during which an activation phase ends. Defaults to the value that `start_second` uses.

### Extended validity phases
These fields are all optional integers that specify the length of the validity phase, which is time outside the activation phase of a period that still counts as being within the same period. For example, if a pass starts an activation phase every midnight but has a validity phase of three hours, activating that pass at 01:05 January 23rd would result in it expiring 03:00 January 24th. All of these fields default to 0. Values are added top to bottom.

Field name|Field description
:-|:-
`plus_months`|Adds that many months, ending at the same time-of-day and day-of-month as the start (end, iff `plus_month_end` is `1`) of the activation phase. If that day of that month doesn’t exist, then it ends at 00:00 on the 1st of the next month instead.
`plus_days`|Adds that many days, ending at the same time-of-day.
`plus_minutes`|Adds that many minutes.

## `pass_periods.txt`
This table documents which periods apply to which passes.

Multiple periods can be defined for a single pass. In that case, the start of a new period instantly ends the last one, except for the final period for which a pass is valid.

Passes without any periods specified expire when limited-quantity trips are exhausted. If there are no limited-quantity trips, passes don't expire (i.e. )

Field name|Field type|Field description
:-|:-|:-
**`pass_id`**|Required: ID referencing [`passes.pass_id`](#passestxt)|The pass that this period applies to.
**`period_id`**|Required: ID referencing [`pass_period_times.period_id`](#pass_period_timestxt)|The period that applies to this pass.

## `pass_fares.txt`
This table documents how passes cover fares, including partial coverage, limited quantities, and transfers on a single use of the pass. If this table is not specified, all passes fully cover all fares in an unlimited quantity for their specified durations.

It contains the following fields:

Field name|Field type|Field description
:-|:-|:-
**`pass_id`**|Required: ID referencing [`passes.pass_id`](#passestxt)|The pass that covers this fare.
**`fare_id`**|Required: ID referencing [`fare_attributes.fare_id`](https://developers.google.com/transit/gtfs/reference/#fare_attributestxt)|The fare that is covered by this pass.
`covered_amount`|Float|One of the following:<ul><li>Empty, to indicate that the pass fully covers the fare (unless a value is provided for `covered_factor`).</li><li>A positive number, indicating that the pass covers that exact amount of the fare.</li><li>A negative number, indicating that the pass covers all but that amount of the fare.</li>
`covered_factor`|Float|A number between `0` and `1` indicating how much of the fare (as a factor of the original fare, e.g. `0.5` is one-half) is covered by the pass.
`limit`|Positive integer|The number of times this pass can be used for this fare. Applies only to this fare; if the same counter is used for multiple fares, a `limit_group_id` should be used instead. If both are empty, this pass covers this fare an unlimited number of times for its specified duration.
`limit_group_id`|ID referencing [`pass_limit_groups.limit_group_id`](#pass_limit_groupstxt)|Specifies the limit group this fare is deducted from.
`activates_timer`|Enum|Whether the pass covering this fare "activates" the pass if it wasn't already:<ul><li>`0`: This fare doesn’t activate the timer on this pass.</li><li>`1` or empty: This fare activates the timer on this pass.</li><li>`2`: This fare (or `limit_group_id`) activates the timer just for itself.
`transfers`|Integer|When using a pass for this fare, transfers are permitted this many times without counting as another use of the pass. If empty, defaults to the fare’s original value.

## `pass_limit_groups`
This table documents the shared limits on bulk pass usages for various fares.

It contains the following fields:

Field name|Field type|Field description
:-|:-|:-
**`limit_group_id`**|Required: ID|The identifier for this group.
**`limit`**|Required: Positive integer|How many times this pass can be used.
`overuse`|Enum|If a fare would use more uses out of this limit than are remaining to be used, this value specifies how the pass handles it:<ul><li>`0` or empty: The pass cannot be used.</li><li>`1`: The pass will cover its usual amount and use all remaining uses.</li><li>`2`: The pass will cover the fare fractionally based on how many uses are remaining.</li></ul>

## `pass_constraints.txt`
This table documents constraints on purchasing passes.

In order for a pass to be purchaseable, then all of the constraints within any `constraint_group_id` must be met.

Please note that constraints do not automatically imply their inverse; for example, if `p_pass_one` has a constraint of `purchase_pass_id`=`p_pass_two`, this does not mean that `p_pass_two` can't be purchased alone.

It contains the following fields:

Field name|Field type|Field description
:-|:-|:-
**`pass_id`**|Required: ID referencing [`passes.pass_id`](#passestxt)|The ID of the pass to which these constraints apply.
`constraint_group_id`|ID|Which set of constraints this record is part of. The empty ID is also allowed, and is a group not merged with another group.
*`hold_pass_id`*|Conditionally required: ID referencing [`passes.pass_id`](#passestxt)|The purchaser must possess `hold_pass_id` to purchase `pass_id`. If multiple are specified in one group, all must be held.
*`purchase_pass_id`*|Conditionally required: ID referencing [`passes.pass_id`](#passestxt)|The purchaser must purchase `purchase_pass_id` and `pass_id` at the same time in order to purchase `pass_id`.
*`purchase_fare_id`*|Conditionally required: ID referencing [`fare_attributes.fare_id`](https://developers.google.com/transit/gtfs/reference/#fare_attributestxt)|The purchaser must purchase `purchase_fare_id` to be able to purchase `pass_id`. This also means one of two things must be true:<ul><li>`purchase_fare_id` is a prepaid fare, and `pass_id` can be purchased at a stop where `purchase_fare_id` is valid.</li><li>`purchase_fare_id` is not a prepaid fare, and `pass_id` can be purchased onboard vehicles on which `purchase_fare_id` is valid.</li></ul>
`use_pass_id`|ID referencing [`passes.pass_id`](#passestxt)|When specified, `purchase_fare_id` must be purchased using `use_pass_id` to purchase `pass_id`. Overrides `without_passes` if set.
`without_pass_id`|ID referencing [`passes.pass_id`](#passestxt)|When specified, `purchase_fare_id` must not be purchased using any `without_pass_id`s.
`without_passes`|Enum|Defines whether `purchase_fare_id` must be purchased without passes, if specified.<ul><li>`0` or empty: `purchase_fare_id` may be purchased using other passes.</li><li>`1`: `purchase_Fare_id` must be purchased without any passes, except any `use_pass_id`s.</li></ul>

For the conditional requirement, exactly one of `hold_pass_id`, `purchase_pass_id`, or `purchase_fare_id` must be specified.

## `pass_locations.txt`
This table documents locations at which passes can be purchased.

It contains the following fields:

Field name|Field type|Field description
:-|:-|:-
**`location_id`**|Required: ID| A unique ID for the location in question.
*`location_name`*|Conditionally required: Text|The name of the location in question. Required unless `stop_id` is specified, in which case it's optional and defaults to the name of the stop (or its `parent_station`, recursing upwards until a name is found).
*`stop_id`*|Conditionally required: ID referencing [`stops.stop_id`](https://developers.google.com/transit/gtfs/reference/#stopstxt)|Which stop ID this point-of-sale is at. Required unless the `location_type` is `4`, `6`, or `7`.
`location_group_id`|ID (repeatable)|Which group of passes is sold at this location. If left blank, all passes are assumed to be sold here.
*`location_url`*|Conditionally required: URL|A URL referring to more information about this location. Required for `location_type`=`7`, where it’s the URL at which passes may be purchased online, or which points to further information to mail order passes.
`agency_id`|ID referencing [`agency.agency_id`](https://developers.google.com/transit/gtfs/reference/#agencytxt)|Which agency owns the location. If more than one system is represented in the file, this field is recommended; otherwise, it is presumed to be all the agencies at once.
**`location_type`**|Required: Enum|What kind of location the entry refers to:<ul><li>`0` or empty: This information is unavailable.</li><li>`1`: A building branded by the transit agency.</li><li>`2`: A space branded by the transit agency, but in a building that is not.</li><li>`3`: A space not branded by the transit agency, e.g. the customer service desk of a grocery store.</li><li>`4`: The transit agency’s mobile app.</li><li>`5`: A Ticket Vending Machine operated by the transit agency.</li><li>`6`: Vehicles operated by the transit agency.</li><li>`7`: By mail order.</li></ul>

The following fields are all optional enums with the following set of values:

* `0` or empty: The information is not available.
* `1`: The location accepts this method of payment.
* `2`: The location doesn't accept this method of payment.

Field name|Field description
:-|:-
`accepts_cash`|Whether or not cash is accepted for pass purchases at this location.
`accepts_credit`|Whether or not credit cards are accepted for pass purchases at this location.
`accepts_mobile_pay`|Whether or not mobile payments are accepted for pass purchases at this location.
`accepts_smart_card`|Whether or not payments are accepted through balance on the transit providers’ smart cards for pass purchases at this location.
`accepts_check`|Whether or not checks are accepted as payment for pass purchases at this location.

## `pass_location_groups.txt`
This table documents the groups that passes are sold in. It can be omitted (and, in fact, has no effect) if there are no `location_group_id`s specified in `pass_locations.txt`.

It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
**`pass_id`**|Required: ID referencing [`passes.pass_id`](#passestxt)|Which pass is part of the group.
**`location_group_id`**|Required: ID referencing [`pass_locations.location_group_id`](#pass_location_groupstxt)|Which group the pass is a part of.

## `pass_vehicle_purchase.txt`
This table documents when passes can be purchased aboard vehicles.

If this table is not specified, all passes that fall under `location_group_id`s sold in locations where `location_type` is `6` are presumed to be sold on all vehicles of that agency’s routes.

If this table *is* specified, it is assumed to have exhaustive information on buying passes on vehicles (routes and trips not specified are assumed to disallow purchases).

It contains the following fields:

Field name|Field type|Field description
:-|:-|:-
*`route_id`*|Conditionally required: ID referencing [`routes.route_id`](https://developers.google.com/transit/gtfs/reference/#routestxt)|Which route the passes may be purchased on.
*`trip_id`*|Conditionally required: ID referencing [`trips.trip_id`](https://developers.google.com/transit/gtfs/reference/#tripstxt)|Which trip the passes may be purchased on.
**`location_group_id`**|Required: ID referencing [`pass_locations.location_group_id`](#pass_locationstxt)|Which group of passes may be purchased on the route or trip.

Exactly one of `route_id` and `trip_id` must be specified. If any trip_ids are specified of route_ids also specified, then the trip_ids specified override their route_ids.

# Translations
The following change to `translations.txt` is proposed to account for additional tables from this proposal:

`table_name`|ID field for `record_id`|ID field for `record_sub_id`
:-|:-|:-
[`passes`](#passestxt)|`pass_id`|NONE
[`pass_locations`](#pass_locationstxt)|`location_id`|NONE
