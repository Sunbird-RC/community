# Notifications Configuration

### Registry

Sunbird RC Provides a way to send out notifications to the owner of the entity. Whenever the entity under a particular schema is Invited/Created/Modified/Deleted/Revoked, the notifications can be sent out to owner of that entity.&#x20;

The schema should be configured with the notification templates. [Here](../use/schema-configuration.md#notification-templates) is the way to configure the schema to send out notifications. The configuration shows an example for create notification templates which will be sent on creating an entity. Following are the other type of operations that are supported by Sunbird RC to send out notifications

1. update for Updating entity
2. invite for Inviting an Entity
3. delete for Deleting an Entity
4. revoke for Revoking an Entity

This notification template is configured for One97 Messaging API. Sunbird RC can send out multiple notifications too. Here if you see, create key's value is a list of json objects, where each object represents one notification template. This list can have multiple such templates

Sunbird RC supports sending out notifications for email and sms both. subject is sent only for email content and body is required for both the ways.

There are two ways that Sunbird RC sends out notification

1. Asynchronous - Via Kafka
2. Synchronous - Via APIs

Registry can be configured to send notifications either by Kafka or through APIs. Refer to this [link](configuration.md#registry-configurations)

**Asynchronous -**

Registry creates a kafka message in the required format and puts that in the configured topic of  messaging queue. Then, notification service, consumer of the topic, reads this message and then sents out notification to the phone number or the email address or both that is sent along with this kafka message

**Synchronous -**

Registry calls an API to the Notification Service. The controller in notification service then handles this request and sends out notification to the phone number or the email address or both that is sent in the request body.

### **Notification Service**

Notification service is the service responsible to send out notifications. The service expects configuration of SMS, SMTP Gateway and sends out notifications to the phone number or email address that it receives through kafka message or API request.

Here is the [link](configuration.md#notification-service) for configuration of notification service&#x20;
