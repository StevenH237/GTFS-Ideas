This proposal exists to document fares that are applicable only at certain times of day.

It is important to note that feed consumers that don't support this proposal will always display the cheaper fare of any of the timed options that apply to the same routes.

# Tables
## `fare_times.txt`
This table lists the times that fares are active. `fare_id`s not listed in this table are considered active 24/7; `fare_id`s listed in this table are active only during any of the times specified.

It contains the following fields:

Field name|Field type|Field detail
:-|:-|:-
**`fare_id`**|Required: ID referencing [`fare_attributes.fare_id`](https://developers.google.com/transit/gtfs/reference/#fare_attributestxt)|The fare that is modified by this rule.
*`period_id`*|Conditionally required: ID referencing [`periods.period_id`](../common/periods.md#periodstxt)|The time during which this modification is active. If left blank, this record describes the fare outside any defined period, and "ends" when a defined period begins.
**`adjusted_price`**|Required: Currency|The price paid for this fare during this period.
`continue_transfers`|Conditionally required: Enum|Whether or not fares paid while active but have transfer times exceeding the allotted time should continue to be active on later trips:<ul><li>`0`: No, when this period ends, any active transfers expire.</li><li>`1`: When this period ends, active transfers continue to be valid.</li><li>`2` or empty: When this period ends, if the fare becomes a higher price, the passenger must pay the difference. Then transfers continue to be valid.</li></ul>Required when describing the fare outside any period.