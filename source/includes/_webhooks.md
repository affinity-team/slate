# Webhooks

Webhooks allow you to be notified of events that happen on your Affinity instance. For example, your app can be notified when a list is created, a field value is updated, a person is deleted, and more.

## Create a Webhook Subscription



## Supported Webhook Events

List Events
'list.created'
'list.updated'
'list.deleted'
'list_entry.created'
'list_entry.deleted'

Note Events
'note.created'
'note.updated'
'note.deleted'

Field Events
'field.created'
'field.updated'
'field.deleted'
'field_value.created'
'field_value.updated'
'field_value.deleted'

Person Events
'person.created'
'person.updated'
'person.deleted'

Organization Events
'organization.created'
'organization.updated'
'organization.deleted'

Opportunity Events
'opportunity.created'
'opportunity.updated'
'opportunity.deleted'

File Events
'file.created'
'file.deleted'

## Limitations

We don't yet have a way for you to only subscribe to certain events - it's all or nothing.
There is no UI to entire the URL that we'll POST to. You'll just have to give them to me via email.
There are some conditions that won't trigger webhooks just yet: Data/Note Imports, EmailBot related events, Automated organization creation and Automated global organization field updates (crunchbase fields like last round raised, location, etc.).