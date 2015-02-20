API
=======

The Webhooks API is organized around REST and is designed to have predictable, resource-oriented URLs and to use HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which can be understood by off-the-shelf HTTP clients, and we support cross-origin resource sharing to allow you to interact securely with our API from a client-side web application (though you should remember to use a client token when using API calls in client-side code). JSON will be returned in all responses from the API, including errors.

Requests
------

All API requests must be send over SSL.  The base URL is https://api.webhooks.io which will hit our US East data center.  In the near future, we are looking to add region based endpoints as well.

Responses
------

Each API request will include a JSON response.

Authentication
------

You authenticate to the Webhooks API by providing one of your API keys in the request. You can manage your API keys from your account. You can have multiple API keys active at one time. Your API keys carry many privileges, so be sure to keep them secret!

Authentication to the API occurs via HTTP Basic Auth. Provide your API key as the basic auth username. You do not need to provide a password.

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. You must authenticate for all requests.

Versioning
------

When we make backwards-incompatible changes to the API, we release new versions. The current version is v1.  The version should be specified in the URL following the domain name, eg: https://api.webhooks.io/v1/

Errors
------

Content TBD...

METHODS
=======

Some intro text into the methods


Accounts
------

When you sign up with Webhooks.io you will be provided one account - your Master Account.  However, you have the ability to create more accounts called sub-accounts.  These sub-accounts are useful for segmenting your clients if you are using the platform to deliver webhooks for your application.

###Account Registration

_**POST** /v1/register_

Creates a new account.  This is the same call that is used when a user registers from webhooks.io.

#### POST Parameters

* ```name``` - Account/Company name (example: Sample Company, LLC)
* ```first_name``` (required) - First name of the primary user on the account. (example: Bob)
* ```last_name``` (required) - Last name of the primary user on the account. (example: Smith)
* ```email_address``` (required) - The primary email address for the user on the account. (example: bob.smith@example.com)
* ```password``` (required) - The password for the user on the account
* ```password_confirm``` (required) - The confirmation entry for the password. (example: Bob)
* ```plan_id``` (required) - The plan id selected for the account.  Use /plans resource for a list of all plans.
* ```card_number``` - The credit card number to be used for billing.
* ```card_month``` - The expiration month for the credit card.
* ```card_year``` - The expiration year for the credit card.
* ```card_cvc``` - The CVC on the credit card.
* ```coupon``` - A coupon code to be used.
* ```referrer``` - The location the user came from.
* ```email_verification_callback_url``` - The URL for where the user should be directed to upon verification of the email address.  A query param of ?status=[success,failure] will be appended to this URL.
* ```invite_code``` (required) - The invite code used to create account.

```js
{
	name: 'Sample Company, LLC'
	first_name: 'Bob'
	last_name: 'Smith'
	email_address: 'bob.smith@example.com'
	password_confirm: 'Bob'
}
```

###Create Sub Account

_**POST** /v1/accounts/:account_id/subaccounts_

Creates a sub account.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```name``` (required) - First name of the primary user on the account. (example: Bob)
* ```account_key``` - Identifier from another system. (example: acct123456789)

```js
{
	name: 'Bob'
	account_key: 'acct123456789'
}
```

###List Sub Accounts

_**GET** /v1/accounts/:account_id/subaccounts_

Lists all sub accounts user an account.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
#### Query Parameters

* ```account_key``` - Identifier from another system. (example: acct123456789)

###List Accounts

_**GET** /v1/accounts_

Lists all accounts.

#### Query Parameters

* ```account_key``` - Identifier from another system. (example: acct123456789)

###Get Account

_**GET** /v1/accounts/:account_id_

Returns the details of a specfic account.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
###Update Account

_**PUT** /v1/accounts/:account_id_

Updates the details on an account.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```name``` (required) - Friendly name for the account. (example: Bob)
* ```account_key``` - Identifier from another system. (example: acct123456789)

```js
{
	name: 'Bob'
	account_key: 'acct123456789'
}
```

###Delete Account

_**DELETE** /v1/accounts/:account_id_

Deletes an account or sub account.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)

Applications
------

Some intro into the API.

###Create Application

_**POST** /v1/accounts/:account_id/applications_

Adds an application to an account

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```name``` (required) - Name for the bucket. (example: My Awesome Application)
* ```categories``` (required) - The categories the application belongs to. (example: ecommerce,payment)
* ```overview``` (required) - A short description of the application - 255 characters or less. (example: This is the details of my awesome application.)
* ```description``` (required) - A full description of the application. (example: This is the details of my awesome application.)
* ```homepage_url``` (required) - The url of the application homepage. (example: http://mywebsite.com)
* ```api_url``` (required) - The url to the API documention for the application. (example: http://api.mywebsite.com)
* ```logo_url``` (required) - The url to the logo. (example: http://mywebsite.com/webhooksio/logo.jpg)
* ```active``` (required) - If the application should be active (viewable) or not.

```js
{
	name: 'My Awesome Application'
	categories: 'ecommerce,payment'
	overview: 'This is the details of my awesome application.'
	description: 'This is the details of my awesome application.'
	homepage_url: 'http://mywebsite.com'
	api_url: 'http://api.mywebsite.com'
	logo_url: 'http://mywebsite.com/webhooksio/logo.jpg'
}
```

###Update Application

_**PUT** /v1/accounts/:account_id/applications/:application_id_

Updates an Application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```name``` (required) - Name for the bucket. (example: My Awesome Application)
* ```categories``` (required) - The categories the application belongs to. (example: ecommerce,payment)
* ```overview``` (required) - A short description of the application - 255 characters or less. (example: This is the details of my awesome application.)
* ```description``` (required) - A full description of the application. (example: This is the details of my awesome application.)
* ```homepage_url``` (required) - The url of the application homepage. (example: http://mywebsite.com)
* ```api_url``` (required) - The url to the API documention for the application. (example: http://api.mywebsite.com)
* ```logo_url``` (required) - The url to the logo. (example: http://mywebsite.com/webhooksio/logo.jpg)
* ```active``` (required) - If the application should be active (viewable) or not.

```js
{
	name: 'My Awesome Application'
	categories: 'ecommerce,payment'
	overview: 'This is the details of my awesome application.'
	description: 'This is the details of my awesome application.'
	homepage_url: 'http://mywebsite.com'
	api_url: 'http://api.mywebsite.com'
	logo_url: 'http://mywebsite.com/webhooksio/logo.jpg'
}
```

###Get Application

_**GET** /v1/accounts/:account_id/applications/:application_id_

Returns the details for a specfic application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
###List Applications

_**GET** /v1/accounts/:account_id/applications_

Returns a collection of applications for an account.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
###Delete Application

_**DELETE** /v1/accounts/:account_id/applications/:application_id_

Deletes an application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
###Create Application Version

_**POST** /v1/accounts/:account_id/applications/:application_id/versions_

Adds a version to an application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```key``` (required) - The key/id for this version. (example: 1.1)
* ```release_date``` (required) - The date this version was released.
* ```version_json``` (required) - The complete JSON definition for the version.
* ```examples_json``` (required) - The complete JSON definition for the version examples/recipies
* ```active``` (required) - If the version should be active (viewable) or not.

```js
{
	key: '1.1'
}
```

###Update Application Version

_**PUT** /v1/accounts/:account_id/applications/:application_id/versions/:application_version_id_

Updates an application version.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```application_version_id``` -  (example: AVe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```key``` (required) - The key/id for this version. (example: 1.1)
* ```release_date``` (required) - The date this version was released.
* ```version_json``` (required) - The complete JSON definition for the version.
* ```examples_json``` (required) - The complete JSON definition for the version examples/recipies
* ```active``` (required) - If the version should be active (viewable) or not.

```js
{
	key: '1.1'
}
```

###Get Application Version

_**GET** /v1/accounts/:account_id/applications/:application_id/versions/:application_version_id_

Returns the details for a specfic application version.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```application_version_id``` -  (example: AVe987d754d82a419e8c54c2185ed0ef29)
###List Application Versions

_**GET** /v1/accounts/:account_id/applications/:application_id/versions_

Returns a collection of versions for an application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
###Delete Application Version

_**DELETE** /v1/accounts/:account_id/applications/:application_id/versions/:application_version_id_

Deletes a version for an application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```application_version_id``` -  (example: AVe987d754d82a419e8c54c2185ed0ef29)

Buckets
------

Some intro into the API.

###Create Bucket

_**POST** /v1/accounts/:account_id/buckets_

Adds a bucket to an account

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```name``` (required) - Name for the bucket. (example: My Bucket)
* ```key``` - The key for the bucket. (example: my-bucket)

```js
{
	name: 'My Bucket'
	key: 'my-bucket'
}
```

###Update Bucket

_**PUT** /v1/accounts/:account_id/buckets/:bucket_id_

Updates a bucket.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```bucket_id``` -  (example: BUe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```name``` (required) - Name for the bucket. (example: My Bucket)
* ```key``` - The key for the bucket. (example: my-bucket)

```js
{
	name: 'My Bucket'
	key: 'my-bucket'
}
```

###Get Bucket

_**GET** /v1/accounts/:account_id/buckets/:bucket_id_

Returns the details for a specfic bucket.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```bucket_id``` -  (example: BUe987d754d82a419e8c54c2185ed0ef29)
###List Buckets

_**GET** /v1/accounts/:account_id/buckets_

Returns a collection of buckets for an account.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
###Delete Bucket

_**DELETE** /v1/accounts/:account_id/buckets/:bucket_id_

Deletes a bucket.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```bucket_id``` -  (example: BUe987d754d82a419e8c54c2185ed0ef29)

Inputs
------

Some intro into the API.

###Create Input

_**POST** /v1/accounts/:account_id/inputs_

Adds an input to an account

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```bucket_id``` (required) - The bucket the input belongs to (example: BUe987d754d82a419e8c54c2185ed0ef29)
* ```name``` (required) - Name for the input. (example: My Bucket)
* ```status``` - The status of the bucket, defaults to active.
* ```event_location``` - The location of the event, header, payload, query param, etc (example: payload)
* ```event_path``` - The path to the value that specifies what type of event is coming in.  This starts with the value msg. (example: msg.event)
* ```event_filters``` - The events that this input should be triggerd for.  This can be a comma delimited list of events. (example: account.created,message.sent)
* ```delivery_mode``` - The mode the request should be made in.  Valid options include sync and async.
* ```response_code``` - HTTP Response code to provide upon hook receipt - defaults to 200
* ```response_content``` - Any content that should be provided upon hook receipt.
* ```response_content_type``` - The content type that should be returned upon hook receipt, this should mirror the data in the response_content variable. (example: application/json)
* ```authentication_failures``` - How to handle authentication failures.
* ```authentication_type``` - The type of authentication to apply to incoming requests.

```js
{
	bucket_id: 'BUe987d754d82a419e8c54c2185ed0ef29'
	name: 'My Bucket'
	event_location: 'payload'
	event_path: 'msg.event'
	event_filters: 'account.created,message.sent'
	response_content_type: 'application/json'
}
```

###Update Input

_**PUT** /v1/accounts/:account_id/inputs/:input_id_

Updates the details for an input.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```input_id``` -  (example: INe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```name``` (required) - Name for the input. (example: My Bucket)
* ```status``` - The status of the bucket, defaults to active.
* ```event_location``` - The location of the event, header, payload, query param, etc (example: payload)
* ```event_path``` - The path to the value that specifies what type of event is coming in.  This starts with the value msg. (example: msg.event)
* ```event_filters``` - The events that this input should be triggerd for.  This can be a comma delimited list of events. (example: account.created,message.sent)
* ```delivery_mode``` - The mode the request should be made in.  Valid options include sync and async.
* ```response_code``` - HTTP Response code to provide upon hook receipt - defaults to 200
* ```response_content``` - Any content that should be provided upon hook receipt.
* ```response_content_type``` - The content type that should be returned upon hook receipt, this should mirror the data in the response_content variable. (example: application/json)
* ```authentication_failures``` - How to handle authentication failures.
* ```authentication_type``` - The type of authentication to apply to incoming requests.

```js
{
	name: 'My Bucket'
	event_location: 'payload'
	event_path: 'msg.event'
	event_filters: 'account.created,message.sent'
	response_content_type: 'application/json'
}
```

###Get Input

_**GET** /v1/accounts/:account_id/inputs/:input_id_

Returns the details for a specfic input.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```input_id``` -  (example: INe987d754d82a419e8c54c2185ed0ef29)
###List Inputs

_**GET** /v1/accounts/:account_id/buckets/:bucket_id/inputs_

Returns a collection of inputs for an account.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```bucket_id``` -  (example: BUe987d754d82a419e8c54c2185ed0ef29)
#### Query Parameters

* ```key``` - Name for the bucket.
* ```event_filter``` - The event that should be filtered on.

###Delete Input

_**DELETE** /v1/accounts/:account_id/inputs/:input_id_

Deletes an input.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```input_id``` -  (example: INe987d754d82a419e8c54c2185ed0ef29)

Destinations
------

Some intro into the API.

###Create Destination

_**POST** /v1/accounts/:account_id/inputs/:input_id/destinations_

Adds an destination for an input.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```input_id``` -  (example: INe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```name``` (required) - Name for the input. (example: My Final Destination)
* ```endpoint_url``` (required) - The URL the messages should be sent to.
* ```delivery_order``` - How the deliveries should operate.  Valid options are random or fifo.  The default is random (example: random)
* ```status``` - The status of the bucket, defaults to active.
* ```message_method``` - The HTTP method the message will be sent with.  If null the method will pass through. (example: GET)
* ```event_filters``` - The events that this input should be triggerd for.  This can be a comma delimited list of events. (example: account.created,message.sent)
* ```authentication_type``` - The type of authentication to apply to incoming requests.
* ```retry_policy_id``` - The retry algorithm that will be used for failed attempts.
* ```retry_count``` - The number of times the hook will be retried.
* ```retry_interval``` - The interval for which the retries will be set.
* ```verify_ssl``` - Ensure the SSL certificate is trusted and valid.  If false, this will bypass this protection.
* ```headers_to_include``` - A comma delimited list of custom headers to include.
* ```header_prefix``` - The prefix of the custom headers that will be included.  The default is Webhooks (example: Webhooks)
* ```alert_on_failure``` - A comma delimited list of email addresses to alert when a webhook enters the failed status. (example: bob@mail.com,john@email.com)

```js
{
	name: 'My Final Destination'
	delivery_order: 'random'
	message_method: 'GET'
	event_filters: 'account.created,message.sent'
	header_prefix: 'Webhooks'
	alert_on_failure: 'bob@mail.com,john@email.com'
}
```

###Update Destination

_**PUT** /v1/accounts/:account_id/destinations/:destination_id_

Updates the details of an destination.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```destination_id``` -  (example: OUe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```name``` (required) - Name for the input. (example: My Final Destination)
* ```endpoint_url``` (required) - The URL the messages should be sent to.
* ```delivery_order``` - How the deliveries should operate.  Valid options are random or fifo.  The default is random (example: random)
* ```status``` - The status of the bucket, defaults to active.
* ```message_method``` - The HTTP method the message will be sent with.  If null the method will pass through. (example: GET)
* ```event_filters``` - The events that this input should be triggerd for.  This can be a comma delimited list of events. (example: account.created,message.sent)
* ```authentication_type``` - The type of authentication to apply to incoming requests.
* ```retry_policy_id``` - The retry algorithm that will be used for failed attempts.
* ```retry_count``` - The number of times the hook will be retried.
* ```retry_interval``` - The interval for which the retries will be set.
* ```verify_ssl``` - Ensure the SSL certificate is trusted and valid.  If false, this will bypass this protection.
* ```headers_to_include``` - A comma delimited list of custom headers to include.
* ```header_prefix``` - The prefix of the custom headers that will be included.  The default is Webhooks (example: Webhooks)
* ```alert_on_failure``` - A comma delimited list of email addresses to alert when a webhook enters the failed status. (example: bob@mail.com,john@email.com)

```js
{
	name: 'My Final Destination'
	delivery_order: 'random'
	message_method: 'GET'
	event_filters: 'account.created,message.sent'
	header_prefix: 'Webhooks'
	alert_on_failure: 'bob@mail.com,john@email.com'
}
```

###Get Destination

_**GET** /v1/accounts/:account_id/destinations/:destination_id_

Returns the details for a specfic destination.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```destination_id``` -  (example: OUe987d754d82a419e8c54c2185ed0ef29)
###List Destination

_**GET** /v1/accounts/:account_id/inputs/:input_id/destinations_

Returns a collection of destinations.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```input_id``` -  (example: INe987d754d82a419e8c54c2185ed0ef29)
#### Query Parameters

* ```destination_key``` - Name for the bucket.

###Delete Destination

_**DELETE** /v1/accounts/:account_id/destinations/:destination_id_

Deletes an destination.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```destination_id``` -  (example: OUe987d754d82a419e8c54c2185ed0ef29)

Providers
------

Some intro into the API.

###Create Consumer

_**POST** /v1/accounts/:account_id/applications/:application_id/consumers_

Creates a consumer for an application

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```consumer_id``` (required) - The id for the consumer of the application.  This id should be the unique id from the application provider that identifies this customer/consumer of the application.
* ```bucket_key``` (required) - The bucket key that identifies the container for this consumer, if this does not exist it will be created. Default is default. (example: default)
* ```name``` (required) - The name of the consumer.  This could be the account name within the provider application for example. (example: ACME Corp, Inc.)

```js
{
	bucket_key: 'default'
	name: 'ACME Corp, Inc.'
}
```

###Get Consumers

_**GET** /v1/accounts/:account_id/applications/:application_id/consumers_

Returns a list of all the consumers for a particular application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
###Update Consumer

_**PUT** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id_

Updates the details for a particular consumer.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)
#### POST Parameters

* ```name``` (required) - The name of the consumer.  This could be the account name within the provider application for example. (example: ACME Corp, Inc.)

```js
{
	name: 'ACME Corp, Inc.'
}
```

###Get Consumer

_**GET** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id_

Get the details for a particular consumer.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)
###Delete Consumer

_**DELETE** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id_

Removes a consumer from a particular application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)
###List Consumer destinations

_**GET** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id/destinations_

Returns all the destinations for the consumer of a given application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)
#### Query Parameters

* ```bucket_key``` (required) - The bucket key the destination shoud be created for. (example: default)
* ```destination_key``` - Name for the bucket.

###Create Consumer destination

_**POST** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id/destinations_

Adds an destination for the consumer of a given application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)
#### POST Parameters

* ```application_version_id``` (required) - The version of the application the destination should respond to. (example: Webhooks)
* ```name``` (required) - Name for the input. (example: My Bucket)
* ```bucket_key``` (required) - The bucket key the destination shoud be created for. (example: default)
* ```endpoint_url``` (required) - The status of the bucket, defaults to active.
* ```delivery_order``` - How the deliveries should operate.  Valid options are random or fifo.  The default is random (example: random)
* ```status``` - The status of the bucket, defaults to active.
* ```message_method``` - The HTTP method the message will be sent with.  If null the method will pass through. (example: GET)
* ```event_filters``` - The events that this input should be triggerd for.  This can be a comma delimited list of events. (example: account.created,message.sent)
* ```authentication_type``` - The type of authentication to apply to incoming requests.
* ```retry_policy_id``` - The retry algorithm that will be used for failed attempts.
* ```retry_count``` - The number of times the hook will be retried.
* ```retry_interval``` - The interval for which the retries will be set.
* ```verify_ssl``` - Ensure the SSL certificate is trusted and valid.  If false, this will bypass this protection.
* ```headers_to_include``` - A comma delimited list of custom headers to include.
* ```header_prefix``` - The prefix of the custom headers that will be included.  The default is Webhooks (example: Webhooks)
* ```alert_on_failure``` - A comma delimited list of email addresses to alert when a webhook enters the failed status. (example: bob@mail.com,john@email.com)

```js
{
	application_version_id: 'Webhooks'
	name: 'My Bucket'
	bucket_key: 'default'
	delivery_order: 'random'
	message_method: 'GET'
	event_filters: 'account.created,message.sent'
	header_prefix: 'Webhooks'
	alert_on_failure: 'bob@mail.com,john@email.com'
}
```

###Update Consumer destination

_**PUT** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id/destinations/:destination_id_

Updates an destination for the consumer of a given application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)
* ```destination_id``` -  (example: OUe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```application_version_id``` (required) - The version of the application the destination should respond to. (example: Webhooks)
* ```name``` (required) - Name for the input. (example: My Bucket)
* ```endpoint_url``` (required) - The status of the bucket, defaults to active.
* ```delivery_order``` - How the deliveries should operate.  Valid options are random or fifo.  The default is random (example: random)
* ```status``` - The status of the bucket, defaults to active.
* ```message_method``` - The HTTP method the message will be sent with.  If null the method will pass through. (example: GET)
* ```event_filters``` - The events that this input should be triggerd for.  This can be a comma delimited list of events. (example: account.created,message.sent)
* ```authentication_type``` - The type of authentication to apply to incoming requests.
* ```retry_policy_id``` - The retry algorithm that will be used for failed attempts.
* ```retry_count``` - The number of times the hook will be retried.
* ```retry_interval``` - The interval for which the retries will be set.
* ```verify_ssl``` - Ensure the SSL certificate is trusted and valid.  If false, this will bypass this protection.
* ```headers_to_include``` - A comma delimited list of custom headers to include.
* ```header_prefix``` - The prefix of the custom headers that will be included.  The default is Webhooks (example: Webhooks)
* ```alert_on_failure``` - A comma delimited list of email addresses to alert when a webhook enters the failed status. (example: bob@mail.com,john@email.com)

```js
{
	application_version_id: 'Webhooks'
	name: 'My Bucket'
	delivery_order: 'random'
	message_method: 'GET'
	event_filters: 'account.created,message.sent'
	header_prefix: 'Webhooks'
	alert_on_failure: 'bob@mail.com,john@email.com'
}
```

###Get Consumer destination

_**GET** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id/destinations/:destination_id_

Returns the details of an destination for the consumer of a given application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)
* ```destination_id``` -  (example: OUe987d754d82a419e8c54c2185ed0ef29)
###Delete Consumer destination

_**DELETE** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id/destinations/:destination_id_

Deletes an destination for the consumer of a given application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)
* ```destination_id``` -  (example: OUe987d754d82a419e8c54c2185ed0ef29)
###List Consumer Buckets

_**GET** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id/buckets_

Returns all the buckets for the consumer of a given application.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)

###Send webhook to consumer

_**POST** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id/send/:bucket_key_

Sends a webhook to a particular consumer of an application for the given bucket_key.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)
* ```bucket_key``` -  (example: development)

###Check consumer subscription

_**POST** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id/check_

Checks to see if the consumer is subscribed to a given event or set of events.  If the event query param is not passed the complete list of events will be returned.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)
#### POST Parameters

* ```bucket_key``` (required) - The bucket key the subscription should be checked for. (example: development)
* ```event_name``` (required) - The name of the event to check.
* ```include_destination_detail``` - If the details of each subscribed destination should be returned.

```js
{
	bucket_key: 'development'
}
```

###Consumer Request Log

_**GET** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id/log_

Returns a log of all messages for a given consumer.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)
#### Query Parameters

* ```start_date``` - The start date for the data.  This can be an exact UTC date or a texted based time period.  Valid text time periods can be found at: http://sugarjs.com/date_formats#text_formats
* ```end_date``` - The end date for the data.  This can be an exact UTC date or a texted based time period.  Valid text time periods can be found at: http://sugarjs.com/date_formats#text_formats
* ```destination_id``` - 
* ```http_status``` - 

###Create Client Token

_**POST** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id/client-token_

Generates a client token to be used with the embedded views.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)
#### POST Parameters

* ```bucket_key``` - The bucket key the client token should be generated for.  This can be an arbitrary value that maps back to your system. (example: development)
* ```paths``` - The permitted paths.

```js
{
	bucket_key: 'development'
}
```

###Get Embedded View HTML

_**POST** /v1/accounts/:account_id/applications/:application_id/consumers/:consumer_id/embedded-view-html_

Returns the HTML for the embedded view.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```application_id``` -  (example: APe987d754d82a419e8c54c2185ed0ef29)
* ```consumer_id``` -  (example: my_customer_id)
#### POST Parameters

* ```bucket_key``` - The bucket key the client token should be generated for.  This can be an arbitrary value that maps back to your system. (example: development)
* ```paths``` - The permitted paths.
* ```css_url``` - URL to a css file that will be applied to the application styles.

```js
{
	bucket_key: 'development'
}
```


Reporting
------

Some intro into the API.

###Overview Report

_**GET** /v1/accounts/:account_id/stats/overview_

Returns a general overview.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
#### Query Parameters

* ```start_date``` (required) - The start date for the data.
* ```end_date``` (required) - The end date for the data.
* ```precision``` (required) - The end date for the data.
* ```application_id``` - The application id the data should be filtered with.
* ```bucket_id``` - The end date for the data.
* ```destination_id``` - The end date for the data.
* ```input_id``` - The end date for the data.
* ```include_sub_accounts``` - If sub account data should be included.

###Summary Report

_**GET** /v1/accounts/:account_id/stats/summary_

Returns a general summary report.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
#### Query Parameters

* ```start_date``` - The start date for the data.
* ```end_date``` - The end date for the data.
* ```bucket_id``` - The end date for the data.
* ```destination_id``` - The end date for the data.
* ```input_id``` - The end date for the data.

###Request Log

_**GET** /v1/accounts/:account_id/log_

Returns a log of all messages.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
#### Query Parameters

* ```start_date``` - The start date for the data.  This can be an exact UTC date or a texted based time period.  Valid text time periods can be found at: http://sugarjs.com/date_formats#text_formats
* ```end_date``` - The end date for the data. This can be an exact UTC date or a texted based time period.  Valid text time periods can be found at: http://sugarjs.com/date_formats#text_formats
* ```input_id``` - 
* ```bucket_id``` - 
* ```http_status``` - 


Messages
------

Some intro into the API.

###Get Incoming Message

_**GET** /v1/accounts/:account_id/incoming/:incoming_message_id_

Returns the details regarding an incoming message.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```incoming_message_id``` -  (example: IMe987d754d82a419e8c54c2185ed0ef29)
#### Query Parameters

* ```include_outgoing_messages``` - If the outgoing messages should be included as well.

###Get Outgoing Message

_**GET** /v1/accounts/:account_id/outgoing/:outgoing_message_id_

Returns the details regarding an outgoing message, including all attempts

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```outgoing_message_id``` -  (example: OMe987d754d82a419e8c54c2185ed0ef29)
###Get Outgoing Message Status Details

_**GET** /v1/accounts/:account_id/outgoing/:outgoing_message_id/status_

Returns the basic information regarding the status of the outgoing request.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```outgoing_message_id``` -  (example: OMe987d754d82a419e8c54c2185ed0ef29)

Users
------

Some intro into the API.

###Login

_**PUT** /v1/authenticate_

Authenticates the users login credentials

#### POST Parameters

* ```email_address``` (required) - The user's email address.
* ```password``` (required) - The password supplied for login.

```js
{
}
```

###Change Password

_**PUT** /v1/accounts/:account_id/users/:user_id/change_password_

Allows a user to change their password.  Either the existing password or change key must be passed...and must match in order for this call to be successful.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```user_id``` -  (example: USe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```new_password``` (required) - The new password for the account.
* ```new_confirm_password``` (required) - A confirmation of the new password for their account.
* ```change_key``` - The code that was supplied in the password change email to allow them to change their email.
* ```existing_password``` - The current password on the user account.

```js
{
}
```

###Lookup API Token

_**GET** /v1/accounts/:account_id/users/:user_id/api-token_

Provides a user a way to lookup their own API token.  This is used when using ST or client-bearer-token authentication so the user can get a longer lasting API token.  This operation can only be carried out for the currently authenticated user.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```user_id``` -  (example: USe987d754d82a419e8c54c2185ed0ef29)
###Reset Password

_**POST** /v1/reset_password_

Allows the user to request their password to be emailed to them.  Really this provides them a link to the change password form.

#### POST Parameters

* ```email_address``` (required) - The primary email address for the user on the account. (example: bob.smith@example.com)

```js
{
	email_address: 'bob.smith@example.com'
}
```

###Lookup Password Change Key

_**GET** /v1/password_change_key/:password_change_key_

Looks up the meta data for the password change key.

#### URI Path Parameters

* ```password_change_key``` -  (example: CKe987d754d82a419e8c54c2185ed0ef29)
###Create User

_**POST** /v1/accounts/:account_id/users_

Adds a user to an account.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```first_name``` (required) - First name of the primary user on the account. (example: Bob)
* ```last_name``` (required) - Last name of the primary user on the account. (example: Smith)
* ```email_address``` (required) - The primary email address for the user on the account. (example: bob.smith@example.com)
* ```password``` (required) - The password for the user on the account
* ```permission_level``` (required) - The permission level for the user account.
* ```timezone``` (required) - The timezone the user is located in.  Default is Etc/GTM
* ```notify``` (required) - If the user should be notified that an account has been created for them.

```js
{
	first_name: 'Bob'
	last_name: 'Smith'
	email_address: 'bob.smith@example.com'
}
```

###Update User

_**PUT** /v1/accounts/:account_id/users/:user_id_

Updates a users account information.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```user_id``` -  (example: USe987d754d82a419e8c54c2185ed0ef29)
#### POST Parameters

* ```first_name``` (required) - First name of the primary user on the account. (example: Bob)
* ```last_name``` (required) - Last name of the primary user on the account. (example: Smith)
* ```email_address``` (required) - The primary email address for the user on the account. (example: bob.smith@example.com)
* ```password``` - The password for the user on the account
* ```timezone``` (required) - The timezone the user is located in.  Default is Etc/GMT
* ```permission_level``` - The permission level for the user account.

```js
{
	first_name: 'Bob'
	last_name: 'Smith'
	email_address: 'bob.smith@example.com'
}
```

###Get User

_**GET** /v1/accounts/:account_id/users/:user_id_

Returns the details for a specfic user.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```user_id``` -  (example: USe987d754d82a419e8c54c2185ed0ef29)
###List Users

_**GET** /v1/accounts/:account_id/users_

Returns a collection of users.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```user_id``` -  (example: USe987d754d82a419e8c54c2185ed0ef29)
###Delete User

_**DELETE** /v1/accounts/:account_id/users/:user_id_

Deletes a user.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```user_id``` -  (example: USe987d754d82a419e8c54c2185ed0ef29)
###Verify Email Address

_**GET** /v1/verify/:email_verification_key_

Handles validating the email address once the user has clicked the validation link in an email.

#### URI Path Parameters

* ```email_verification_key``` -  (example: EV4d3dc5927f304df08ad36c5a3a893c52)
###Resend Verification Email

_**GET** /v1/accounts/:account_id/users/:user_id/resend_verification_

Resends a verification email for a user.

#### URI Path Parameters

* ```account_id``` -  (example: ACe987d754d82a419e8c54c2185ed0ef29)
* ```user_id``` -  (example: USe987d754d82a419e8c54c2185ed0ef29)

Utils
------

Some intro into the API.

###Health Check

_**GET** /v1/health_

System health check

###Gets Plans

_**GET** /v1/plans_

Returns all the possible public plans.

###Get Plan

_**GET** /v1/plans/:plan_id_

Returns the details of a specific plan.

#### URI Path Parameters

* ```plan_id``` -  (example: starter)
###Get Timezones

_**GET** /v1/util/timezones_

Returns all valid timezones.

###Gets Retry Policies

_**GET** /v1/retry_policies_

Returns the possible retry policies along with the system default policy.

###Gets Retry Policy

_**GET** /v1/retry_policies/:policy_id_

Returns the details of a specific retry policy.

#### URI Path Parameters

* ```policy_id``` -  (example: linear)
