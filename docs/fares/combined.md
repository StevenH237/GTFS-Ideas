With multiple proposals implemented together, extra tables can be made to connect them to each other.

## `pass_demographics.txt`
*Prerequisites: [passes](passes.md) and [demographics](demographics.md)*

This table documents how much different demographics pay for different passes, or different fares on different passes.

It contains the following fields:

Field name|Field type|Field details
:-|:-|:-
**`demographic_id`**|Required - ID referencing [`fare_demographics.demographic_id`](demographics.md#fare_demographicstxt)|The demographic that can use this price.
**`pass_id`**|Required - ID referencing [`passes.pass_id`](passes.md#passestxt)|The pass purchased or used by this demographic.
`fare_id`|Optional - ID referencing [`fare_attributes.fare_id`](https://developers.google.com/transit/gtfs/reference/#fare_attributestxt)|The fare covered by this pass for this demographic.
*`adjusted_price`*|Conditionally required - Decimal): Has three meanings:<ul><li>With no `fare_id`, the price this demographic pays for this pass.</li><li>With a `fare_id`, the `covered_amount` for that pass for that demographic.</li><li>If `availability` is 2, this field has no meaning and may be omitted.</li></ul>
`adjusted_limit`|Integer|The number of times the pass can be used for this fare by this demographic. If set, it overrides the pass's default `limit_group`. If empty, it’s the same as for the general-public demographic.
`limit_group_id`|ID referencing [`pass_limit_groups.limit_group_id`](passes.md#pass_limit_groupstxt)|The limit group for this pass when used by this demographic. If empty, it's the same as for the general-public demographic.
`adjusted_limit_uses`|Integer|The number of uses this pass takes from the limit group if used by this demographic. If empty, it's the same as for the general-public demographic.
`availability`|Enum|<ul><li>`0` or empty - This demographic has no bearing on the availability of this pass.</li><li>`1` - The pass can only be purchased by this demographic (or any demographic for which this pass's availability is `1`).</li><li>`2` - The pass cannot be purchased by this demographic. It may be purchased under the general-public demographic, but then the general-public rules apply when using it.</li></ul>
`pass_name`|Text|What the pass is called when it covers this demographic. If empty, the default name should be used.

# Translations
The following change to `translations.txt` is proposed to account for additional tables from this proposal:

`table_name`|ID field for `record_id`|ID field for `record_sub_id`
:-|:-|:-
`pass_demographics`|`pass_id`|`demographic_id`