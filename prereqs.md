Any proposals in this file are required for the other proposals.

## `stops.txt`
For stops with `location_type` of 3, the following changes are proposed:

* The `parent_station` can have any `location_type` or be null. Circular references are not allowed.
* Stops without a `parent_station` must specify `stop_lat` and `stop_lon`.
