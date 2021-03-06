# Java client

Client for the GOV.UK Notify API, built using Java 8.

## Installation

### Maven

The notifications-java-client has been deployed to [Bintray](https://bintray.com/gov-uk-notify/maven/notifications-java-client). Add the following snippet to your Maven `settings.xml` file.
```xml
<?xml version='1.0' encoding='UTF-8'?>
<settings xsi:schemaLocation='http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd' xmlns='http://maven.apache.org/SETTINGS/1.0.0' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
<profiles>
	<profile>
		<repositories>
			<repository>
				<snapshots>
					<enabled>false</enabled>
				</snapshots>
				<id>bintray-gov-uk-notify-maven</id>
				<name>bintray</name>
				<url>http://dl.bintray.com/gov-uk-notify/maven</url>
			</repository>
		</repositories>
		<pluginRepositories>
			<pluginRepository>
				<snapshots>
					<enabled>false</enabled>
				</snapshots>
				<id>bintray-gov-uk-notify-maven</id>
				<name>bintray-plugins</name>
				<url>http://dl.bintray.com/gov-uk-notify/maven</url>
			</pluginRepository>
		</pluginRepositories>
		<id>bintray</id>
	</profile>
</profiles>
<activeProfiles>
	<activeProfile>bintray</activeProfile>
</activeProfiles>
</settings>
```
Then you can add the Maven dependency to your project.
```xml
    <dependency>
        <groupId>uk.gov.service.notify</groupId>
        <artifactId>notifications-java-client</artifactId>
        <version>2.1.5-RELEASE</version>
    </dependency>

```

### Gradle
```
repositories {
    mavenCentral()
    maven {
        url  "http://dl.bintray.com/gov-uk-notify/maven"
    }
}

dependencies {
    compile('uk.gov.service.notify:notifications-java-client:2.1.5-RELEASE')
}
```

### Artifactory or Nexus

Click 'set me up!' on https://bintray.com/gov-uk-notify/maven/notifications-java-client for instructions.

## Getting started

**Import the `NotificationClient`**

```java
import uk.gov.service.notify.NotificationClient;
import uk.gov.service.notify.Notification;
import uk.gov.service.notify.NotificationList;
import uk.gov.service.notify.NotificationResponse;
```

**Create a new instance of `NotificationClient` and objects returned by the client**

```java
NotificationClient client = new NotificationClient(api_key, serviceId, "https://api.notifications.service.gov.uk");
```

## Send an email or text message

```java
NotificationResponse response = client.sendEmail(templateId, emailAddress, personalisation);
```

or

```java
NotificationResponse response = client.sendSms(templateId, mobileNumber, personalisation);
```

* `mobileNumber` is the mobile phone number for the notification
    * must be a UK mobile number
    * must start with +44
    * must not have a leading zero
    * must not have any whitespaces or punctuation
    * valid format is +447777111222
* `emailAddress` is the email address for the notification
* `templateId` is the template to send
    * must be a universally unique identifier (UUID) that identifies a valid template  - templates are created in the admin tools
* `personalisation` is the placeholders to send 
    * must be a HashMap<String, String> which contains the key value pairs for the placeholders. 

## Fetch notification by Id

`Notification notification = client.getNotificationById(notificationId);`

* `notificationId` is the Id of the notification - the Id is part of the notification object returned when `sendEmail` or `sendSms` is called
 
## Fetch all notifications for your service

`Notification notification = client.getNotification(status, notificationType);`

* `status` is a string that represents the status of the notifications you want returned; options for `status`:
    * `null` (for all status types)
    * `delivered` 
    * `failed`
    * `sending`
* `notificationType` is the type of notification to return; options for `notificaitonType`:
    * `null` (for both types)
    * `email` 
    * `sms`


## Testing

There is a main class that can be used to test the integration. It is also useful to read this class to see how to integrate with the notification client.

On a command line build the project with Maven:

`> mvn test`

Then run Java via the Maven exec command with the following arguments:

`> mvn exec:java -Dexec.mainClass=TestNotificationClient -Dexec.args="api-key-id service-id https://api.notifications.service.gov.uk"`

You will be prompted to select an option to submit the API call.
