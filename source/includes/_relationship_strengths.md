# Relationship Strengths

Affinity calculates relationship strengths between internal and external people based on
previous interactions (emails, logged calls, calendar events).

If an internal and external person have no previous interactions, there may be no relation
strength resource for their relationship.

## The relationship strength resource

> Example Response

```json
{
  "external_person_id": 1234,
  "internal_person_id": 2345,
  "strength": 0.5,
}
```

Attribute | Type | Description
--------- | ------- | -----------
internal_person_id | integer | The internal person associated with this relationship strength.
external_person_id | integer | The external person associated with this relationship strength.
strength | float | The actual relationship strength. This is currently a number between 0 and 1, but may change in the future.


## Search for a relationship strength

Get the relationship strength between an internal person and an external person:


> Example Request

```shell
# Returns an array relationship strengths matching the criteria.
curl "https://api.affinity.co/relationships-strengths?external_person_id=1234&internal_person_id=2345" -u :<API-KEY>
```

> Example Response

```json
{
  "relationship_strengths": [
    {
      "internal_person_id": 1234,
      "external_person_id": 2345,
      "strength": 0.5,
    }
  ],
}
```

Get the relationship strength between all internal people and an external person:

> Example Request

```shell
# Returns an array relationship strengths matching the criteria.
curl "https://api.affinity.co/relationships-strengths?external_person_id=1234" -u :<API-KEY>
```

> Example Response

```json
{
  "relationship_strengths": [
    {
      "external_person_id": 1234,
      "internal_person_id": 2345,
      "strength": 0.5,
    },
    {
      "external_person_id": 1234,
      "internal_person_id": 3456,
      "strength": 0.9,
    },
    ...
  ],
}
```

### Parameters

Parameter | Type | Required | Description
--------- | ------- | ---------- | -----------
internal_person_id | integer | false | The internal person associated with this relationship strength.
external_person_id | integer | true | The external person associated with this relationship strength.

### Returns
An array of the relationship strengths matching the criteria.

If `internal_person_id` is provided, the list will contain either one item or no items (if
there is no relationship strength available).
