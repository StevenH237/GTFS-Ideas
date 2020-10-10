## `prepaid_fares.txt`
This table defines station-, route-, or trip-level overrides on whether a fare is paid aboard the vehicle or before boarding, as well as offering additional options. It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
**`fare_id`**|Required: ID referencing [`fare_attributes.fare_id`](https://developers.google.com/transit/gtfs/reference/#fare_attributestxt)|Defines which fare this override applies to.
**`exception_order`**|Required: Integer|Defines the order in which to apply the exception rules.
*`stop_id`*|Conditionally required: ID referencing [`stops.stop_id`](https://developers.google.com/transit/gtfs/reference/#stopstxt)|Defines which stop this override applies to.
*`route_id`*|Conditionally required: ID referencing [`routes.route_id`](https://developers.google.com/transit/gtfs/reference/#routestxt)|Defines which route this override applies to.
*`trip_id`*|Conditionally required: ID referencing [`trips.trip_id`](https://developers.google.com/transit/gtfs/reference/#tripstxt)|Defines which trip this override applies to.
**`exception_type`**|Required: Enum|<ul><li>`0`: The fare must be paid onboard.</li><li>`1`: The fare must be paid in advance of boarding.</li><li>`2`: The fare must be paid at a later stop on the route.</li><li>`3`: The fare does not apply (another fare must be chosen if applicable).</li><li>`4`: The fare need not be paid.

At least one of `stop_id`, `route_id`, or `trip_id` must be specified. Rules are applied to an itinerary from lowest to highest `exception_order` within a document when all IDs match, with the fares’ own definitions assumed to be at 0. For example, the following…

fare_id|exception_order|stop_id|route_id|exception_type
:-|:-|:-|:-|:-
`basefare`|1||`silverline`|1
`basefare`|2|`40thst`||0

… defines that although basefare is normally paid onboard a vehicle, for all `silverline` vehicles it must be paid in advance - except at the `40thst` stop, which doesn’t support advance payments, so the fare reverts to being paid on board even on silverline.