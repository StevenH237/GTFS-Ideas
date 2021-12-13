This file is designed to allow GTFS consumers to notify users when the multiple agencies in a file use different codes for the same stop.

# The table
## `stop_codes.txt`

Field name|Field type|Field details
:-|:-|:-
**`stop_id`**|Required: ID referencing `stops.stop_id`|The ID of the stop to which this record applies.
**`agency_id`**|Required: ID referencing `agency.agency_id`|The ID of the agency to which this record applies.
**`stop_code`**|Required: Text|The stop code which the specified agency uses for the specified stop.<br/><br/>Unlike most required values, this can be left empty. An empty value should be treated as an explicit declaration that the agency does not have a user-facing code for the stop. The field must still exist even if all values are empty.