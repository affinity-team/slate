# Relationship Strengths

Affinity calculates relationship strengths between internal and external people based on
previous interactions (emails, logged calls, calendar events).

A higher numeric value means that the relationship strength between the two people is
higher. Emails, calls, and meetings don't tell the whole story of a relationship, so treat
the strength as an estimate. If an internal and external person have no previous
interactions, there may be no relation strength resource for their relationship.

Relationship strengths are usually recalculated daily.

## The relationship strength resource

> Example Response

```json
{
  "external_id": 1234,
  "internal_id": 2345,
  "strength": 0.5,
}
```

The relationship strength resource specifies the two [Persons](#persons) the relationship
strength is about, along with the actual value. There may be at most one resource for
every (internal, external) pair.

Attribute | Type | Description
--------- | ------- | -----------
internal_id | integer | The internal person associated with this relationship strength.
external_id | integer | The external person associated with this relationship strength.
strength | float | The actual relationship strength. This is currently a number between 0 and 1, but may change in the future.


## Fetch a relationship strength

Get the relationship strength between an internal person and an external person:


> Example Request

```shell
# Returns an array relationship strengths matching the criteria.
curl "https://api.affinity.co/relationships-strengths?external_id=1234&internal_id=2345" -u :<API-KEY>
```

> Example Response

```json
[
  {
    "internal_id": 1234,
    "external_id": 2345,
    "strength": 0.5,
  }
]
```

Get the relationship strength between all internal people and an external person:

> Example Request

```shell
# Returns an array relationship strengths matching the criteria.
curl "https://api.affinity.co/relationships-strengths?external_id=1234" -u :<API-KEY>
```

> Example Response

```json
[
  {
    "external_id": 1234,
    "internal_id": 2345,
    "strength": 0.5,
  },
  {
    "external_id": 1234,
    "internal_id": 3456,
    "strength": 0.9,
  },
  ...
]
```

Note: The results are not guaranteed to be sorted in any way.

### Parameters

Parameter | Type | Required | Description
--------- | ------- | ---------- | -----------
internal_id | integer | false | The internal person associated with this relationship strength.
external_id | integer | true | The external person associated with this relationship strength.

### Returns
An array of the relationship strengths matching the criteria.

If `internal_id` is provided, the list will contain either one item or no items (if
there is no relationship strength available).
