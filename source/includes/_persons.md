# Persons
The persons API allows you to manage all the contacts of your organization. These people
include anyone your team has ever been in email communications or meetings with, and all
the people that your team has added to Affinity either manually or through the API.
Affinity's data model also guarantees that only one person in your team's shared contact
list has a given email address.

**Note:**

1. If you are looking to add or remove a person from a list, please check out the
[List Entries](#list-entries) section of the API.
2. If you are looking to modify a person's entity values (one of the cells on Affinity's
spreadsheet), please check out the [Entity Values](#entity-values) section of the API.

## The person resource
> Example Response

```json
{
  "id": 38706,
  "type": 0,
  "first_name": "John",
  "last_name": "Smith",
  "phone_numbers": ["123-456-7890"],
  "primary_email": "john@affinity.co",
  "emails": [
    "john@affinity.co",
    "john@affinity.vc",
    "jsmith@alumni.stanford.edu",
    "johnjsmith@gmail.com",
  ],
  "organization_ids": [1687449],
},
```

Each person resource is assigned a unique `id` and stores the name, phone numbers, and
email addresses of the person. A person resource also has access to a smart attribute
called `primary_email`. The value of `primary_email` is automatically computed by
Affinity's proprietary algorithms and refers to the email that is most likely to be the
current active email address of a person.

The person resource `organization_ids` is a collection of unique identifiers to the
person's associated organizations. Note that a person can be associated with multiple
organizations. For example, say your team has talked with organizations A and B. Person X
used to work at A and was your point of contact, but then changed jobs and started emailing
you from a new email address (corresponding to organization B).
In this case, Affinity will automatically associate person X with both
organization A and organization B.

Finally, the person resource `type` indicates whether a person is internal or
external to your team. Every internal person is a user of Affinity on your team, and all other
people are externals.

Attribute | Type | Description
--------- | ------- | -----------
id | integer | The unique identifier of the person object.
type | integer | The type of person (see below).
first_name | string | The first name of the person.
last_name | string | The last name of the person.
emails | string[] | The email addresses of the person.
phone_numbers | string[] | The phone numbers of the person.
primary_email | string | The email (automatically computed) that is most likely to the current active email address of the person.
organization_ids | integer[] | An array of unique identifiers of organizations that the person is associated with.

### Person types

Type | Value | Description
--------- | ------- | -----------
external | 0 | Default value. All people that your team has spoken with externally have this type.
internal | 1 | All people on your team that have Affinity accounts will have this type.

## Search for persons
`GET /persons`

Searches your teams data and fetches all the persons that meet the search criteria.
The search term can be part of an email address, a first name or a last name.

> Example Request

```shell
curl "https://api.affinity.vc/persons?key=<API-KEY>"
```
> Example Response

```json
[
  {
    "id": 38706,
    "type": 0,
    "first_name": "John",
    "last_name": "Smith",
    "phone_numbers": ["123-456-7890"],
    "primary_email": "john@affinity.co",
    "emails": [
      "john@affinity.co",
      "john@affinity.vc",
      "jsmith@alumni.stanford.edu",
      "johnjsmith@gmail.com",
    ]
  },
  {
    "id": 624289,
    "type": 1,
    "first_name": "Jane",
    "last_name": "Doe",
    "phone_numbers": ["098-765-4321"],
    "primary_email": "jane@gmail.com",
    "emails": [
      "jane@gmail.com"
    ]
  },
  ...
]
```

### Parameters

Parameter | Type | Required | Description
--------- | ------- | ---------- | -----------
term | string | true | A string used to search all the persons in your team's address book. This could be an email address, a first name or a last name.

### Returns
An array of all the person resources that match the search criteria. Does not include the associated `organization_ids`.

## Get a specific person

> Example Request

```shell
curl "https://api.affinity.vc/persons/38706?key=<API-KEY>"
```

> Example Response

```json
{
  "id": 38706,
  "type": 0,
  "first_name": "John",
  "last_name": "Doe",
  "phone_numbers": ["123-456-7890"],
  "primary_email": "john@affinity.co",
  "emails": [
    "john@affinity.co",
    "john@affinity.vc",
    "jdoe@alumni.stanford.edu",
    "johndoe@gmail.com",
  ],
  "organization_ids": [1687449],
}
```

`GET /persons/{person_id}`

Fetches a person with a specified `person_id`.

### Parameters

Parameter | Type | Required | Description
--------- | ------- | ---------- | -----------
person_id | integer | true | The unique id of the person that needs to be retrieved.

### Returns
The person resource corresponding to the `person_id`.

## Create a new person

> Example Request

```shell
curl "https://api.affinity.vc/persons?key=<API-KEY>" \
  -d first_name="Alice" \
  -d last_name="Doe" \
  -d emails[]="alice@affinity.co" \
  -d phone_numbers[]="123-123-123" \
  -d organization_ids[]=1687449
```

> Example Response

```json
{
  "id":860197,
  "type":0,
  "first_name":"Alice",
  "last_name":"Doe",
  "phone_numbers":["123-123-123"],
  "primary_email":"alice@affinity.co",
  "emails":["alice@affinity.co"],
  "organization_ids":[1687449]
}
```

`POST /persons`

Creates a new person with the supplied parameters.

**Note:**

1. If one of the supplied email addresses conflicts with another person object, this
request will fail and an appropriate error will be returned.
2. If you are looking to add an existing person to a list, please check the `List Entry` section
of the API.

### Parameters

Parameter | Type | Required | Description
--------- | ------- | ---------- | -----------
first_name | string | true | The first name of the person.
last_name | string | true | The last name of the person.
emails | string[] | true | The email addresses of the person. If there are no email addresses, please specify an empty array.
phone_numbers | string[] | true | The phone numbers of the person. If there are no phone numbers, please specify an empty array.
organization_ids | integer[] | false | An array of unique identifiers of organizations that the person is associated with.

### Returns
The person resource was newly created from this successful request.

## Update a person

> Example Request

```shell
curl "https://api.affinity.vc/860197?key=<API-KEY>" \
  -d phone_numbers[]="123-123-123" \
  -d phone_numbers[]="234-234-234" \
  -d first_name="Allison" \
  -X "PUT"
```

> Example Response

```json
{
  "id":860197,
  "type":0,
  "first_name":"Allison",
  "last_name":"Doe",
  "phone_numbers":["123-123-123","234-234-234"],
  "primary_email":"alice@affinity.co",
  "emails":["alice@affinity.co"],
  "organization_ids":[1687449]
}
```

`PUT /persons/{person_id}`

Updates an existing person with `person_id` with the supplied parameters. Only attributes
that need to be changed must be passed in.

**Note:**

If you are looking to add an existing person to a list, please check the `List Entry` section
of the API.

If you are trying to add a new phone number, email, or organization to a person, the existing
values for `phone_numbers`, `emails` and `organization_ids` must also be supplied as
parameters.

### Parameters

Parameter | Type | Required | Description
--------- | ------- | ---------- | -----------
first_name | string | false | The first name of the person.
last_name | string | false | The last name of the person.
emails | string[] | false | The email addresses of the person. If there are no email addresses, please specify an empty array.
phone_numbers | string[] | false | The phone numbers of the person. If there are no phone numbers, please specify an empty array.
organization_ids | integer[] | false | An array of unique identifiers of organizations that the person is associated with

### Returns
The person object that was just updated through this request.

## Delete a person

> Example Request

```shell
curl "https://api.affinity.vc/persons/860197?key=<API-KEY>" \
  -X "DELETE"
```

> Example Response

```json
{ "success": true }
```

`DELETE /persons/{person_id}`

Deletes a person with a specified `person_id`.

**Note:** This will also delete all the entity values, if any, associated with the person.
Such entity values exist linked to either global or list-specific entity attributes.

### Parameters

Parameter | Type | Required | Description
--------- | ------- | ---------- | -----------
person_id | integer | true | The unique id of the person that needs to be deleted.

### Returns
`{success: true}`.

## Get global entity attributes

`GET /persons/entity-attributes`

Fetches an array of all the global entity attributes that exist on people. If you aren't sure
about what entity attributes are, please read the [entity attributes](#entity-attributes) 
section first.

> Example Request

```shell
curl "https://api.affinity.vc/persons/entity-attributes?key=<API-KEY>"
```

> Example Response

```json
[
  {
    "id":125,
    "name":"Use Case",
    "value_type":2,
    "allows_multiple":true,
    "dropdown_options":[]
  },
  {
    "id":198,
    "name":"Referrers",
    "value_type":0,
    "allows_multiple":true,
    "dropdown_options":[]
  },
  {
    "id":1615,
    "name":"Address",
    "value_type":5,
    "allows_multiple":false,
    "dropdown_options":[]
  },
  ...
]
```

### Parameters
None.

### Returns
An array of the global entity attributes that exist on people for your team.
