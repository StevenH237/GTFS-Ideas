This file documents the fares of Metro Transit, MN - including how timing and demographics affect it. This file does *not* include passes.

# Disclaimer
This file was not created with any input, permission, or endorsement from Metro Transit of Minneapolis/St. Paul, MN. It has not been reviewed for accuracy and no guarantees are made of such. The author of this document has never even set foot in that state.

# The tables
Only relevant tables are shown here. IDs from other tables will be referenced using {} describing which item(s) the record is pointing to - for multiple IDs, assume multiple rows in the table.

Table name|From proposal
:-|:-
[fare_attributes.txt](#fare_attributestxt)|[GTFS Standard](https://developers.google.com/transit/gtfs/reference/#fare_attributestxt)
[fare_rules.txt](#fare_rulestxt)|[GTFS Standard](https://developers.google.com/transit/gtfs/reference/#fare_rulestxt)
[fare_times.txt](#fare_timestxt)|[Timed fares](../fares/timed.md/#fare_timestxt)
[fare_demographics.txt](#fare_demographicstxt)|[Fare demographics](../fares/demographics.md#fare_demographicstxt)
[fare_demographic_prices.txt](#fare_demographic_pricestxt)|[Fare demographics](../fares/demographics.md#fare_demographic_pricestxt)

## `fare_attributes.txt`
`fare_id`|`price`|`currency_type`|`