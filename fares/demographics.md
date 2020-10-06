These tables describe the various demographics that are eligible for altered (usually reduced) fares for trips, as well as what those fares actually are.

# The tables
All tables in this proposal are required in order for this proposal to be useful.

## `fare_demographics.txt`
This table establishes the actual demographics.

Demographic records usually contain one or more categories to include or exclude. Despite this, feed consumers should not make automatic choices as to what demographics a user is part of; the choices should be presented to the user for self-identification.

It should be noted that the transit agency always has the final say on who is a member of what demographics.

Feed producers should not create a “general public” demographic, but instead use the default values specified in `fare_attributes.txt` as “general public” fares.

This table must contain the following fields:

Field name|Field type|Field details
:-|:-|:-
**`demographic_id`**|Required: ID|The ID that refers to this demographic.
**`demographic_name`**|Required: Text|A short name for this demographic.
**`demographic_detail`**|Required: Text|Details about the people who qualify for this demographic.

It should also contain fields to describe the categories of this demographic. This list is not exhaustive - GTFS consumers should support at least all of the following fields and also provide support for custom fields.

All of these fields are optional enums, with the following values:

* `0`, empty, or omitted: This category has no bearing on this demographic.
* `1`: The individual must fall into at least one of these categories to be part of this demographic.
* `2`: The individual must fall into all of these categories to be part of this demographic.
* `3`: The individual must not fall into this category to be part of this demographic.
* If values of 1 and 2 are both present, the individual must be a member of all 2 categories and at least one 1 category.

Field name|Field details
:-|:-
`senior`|Individuals that meet general criteria to be conidered senior citizens.
`student`|Individuals that are a student at an educational institution.
`minor`|Individuals that are below a certain age, usually that of majority.
`accompanied_minor`|Individuals that are below a certain age, traveling with an adult. (Fares for this category should only include the minor's fare.)
`unaccompanied_minor`|Individuals that are below a certain age, not traveling with an adult.
`disabled`|Individuals with a disability that is recognized by the agency.
`resident`|Individuals residing within an area defined by the agency.
`employee`|Employees of the agency.
`military`|Active members of the military.
`veteran`|Former members of the military.
`local_official`|Local officials in the agency's area.
`police`|Police officers in the agency's area.
`first_responder`|Non-police first responders in the agency's area.

## `fare_demographic_prices.txt`
This table overrides the prices of specific fare classes for specific demographics.

Note that only the specific combinations shown are overridden. If a fare class-demographic combination exists but isn’t documented in this table, that demographic pays that fare’s normal price. Currency remains the same as the one specified in `fare_attributes.txt`.

Field name|Field type|Field details
:-|:-|:-
**`demographic_id`**|Required: ID referencing `fare_demographics.demographic_id`| The demographic for which this fare adjustment applies.
**`fare_id`**|Required: ID referencing `fare_attributes.fare_id`|The fare class for which this adjustment applies.
**`adjusted_price`**|Required: Float|The adjusted price the given demographic pays for the given fare.

# Translating these tables
The following change to `translations.txt` is proposed to account for additional tables from this proposal:

`table_name`|ID field for `record_id`|ID field for `record_sub_id`
:-|:-|:-
`fare_demographics`|`demographic_id`|NONE