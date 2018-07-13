# Entity Files

> Example Response

```json
[
  {
    "id":3124,
    "name":"Pitch.pdf",
    "type":"application/pdf",
    "size":51073,
    "entity_id":4113456,
    "uploader_id":860197,
    "created_at":"2018-07-12 02:11:06 -0700"
  },
  {
    "id":3125,
    "name":"10_01_18_meeting.txt",
    "type":"text/plain",
    "size":337,
    "entity_id":4113456,
    "uploader_id":860197,
    "created_at":"2018-07-12 02:11:08 -0700"
  },
]
```

Entity files are files uploaded to Affinity that are attached to a `person`, `company`, or `opportunity`. Possible files could be a pitch deck for an opportunity or a physical mail correspondence for a person.

## The entity file resource

A single entity file must be attached to a single entity whose unique identifier is represented by `entity_id`.

Provided that the entity is not a person internal to the user's organization, the file will then display on the entity's profile.

Attribute | Type | Description
--------- | ------- | -----------
id | integer | The unique identifier of the entity file object.
name | string | The name of the file.
type | string | The MIME type of the file.
size | integer | The size of the file.
entity_id | integer | The unique identifier of the person, company, or opportunity object that this file was attached to.
uploader_id | integer | The unique identifier of the person object who uploaded the file.
created_at | datetime | The string representing the time when the note was created.

## Upload files

> Example Request

```shell
curl "https://api.affinity.co/entity-files" \
  -u :<API-KEY> \
  -F files[]=@Pitch.pdf \
  -F files[]=@10_01_18_meeting.txt \
  -F person_id=4113456
```
> Example Response

```json
[
  {
    "id":3124,
    "name":"Pitch.pdf",
    "type":"application/pdf",
    "size":51073,
    "entity_id":4113456,
    "uploader_id":860197,
    "created_at":"2018-07-12 02:11:06 -0700"
  },
  {
    "id":3125,
    "name":"10_01_18_meeting.txt",
    "type":"text/plain",
    "size":337,
    "entity_id":4113456,
    "uploader_id":860197,
    "created_at":"2018-07-12 02:11:08 -0700"
  },
]
```

`POST /entity-files`

Uploads files that are attached to the entity with the given id.

### Path Parameters

Parameter | Type | Required | Description
--------- | ------- | ---------- | -----------
file | File | false | A singular file to be uploaded, formatted as form data (multipart/form-data).
files | File[] | false | An array of files to be uploaded, formatted as form data (multipart/form-data).
person_id | integer | false | The unique identifier of the person object to attach the
file(s) to.
company_id | integer | false | The unique identifier of the company object to attach the
file(s) to.
opp_id | integer | false | The unique identifier of the opportunity object to attach the
file(s) to.

**Note:**

1. One of the three entity id parameters (`person_id`, `company_id`, `opp_id`)
must be specified.
2. Either `file` or `files` must be specified.

### Returns
The entity file resource(s) created through this request.
