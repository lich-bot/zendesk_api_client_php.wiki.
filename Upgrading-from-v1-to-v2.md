# V1 - V2 Upgrade Guide
The version 2 release of the PHP API resolved several issues we had in the version 1 and generally improved code quality. Listed below are the key changes done for version 2

* Full support for all endpoints in the core API
* A consistent interface for resources (i.e. the constructors, getters, setters, etc. should all work the same whether you're working with apps, tickets or anything else)
* Better handling of chaining.
* Better handling of routes.
* Better unit test coverage
* Tests run faster. Which means mocking the API calls, reducing the number of tests and making them easier to write.
* Improved maintainability.
* Better error and exception handling
* Examples to help developers understand how to use the client

## Namespacing
There is only one change in the namespacing.

Client -> HttpClient

## Authentication

```php
// v1
use Zendesk\API\Client as ZendeskAPI;
$client->setAuth('token', $token);
```
```php
// v2
use Zendesk\API\HttpClient as ZendeskAPI;
$client->setAuth('basic', ['username' => $username, 'token' => $token]);
```

## Basic operations

Most basic operations are very similar. We've tried to keep them as consistent as possible between v1 and v2, but there are some slight differences.

## Get & create a ticket
```php
// v1
$tickets = $client->tickets()->findAll();
$newTicket = $client->tickets()->create(array(
    'subject'  => 'The quick brown fox jumps over the lazy dog',
    'comment'  => array(
        'body' => 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.'
    ),
    'priority' => 'normal'
));
```
```php
// v2

$tickets = $client->tickets()->findAll();
$newTicket = $client->tickets()->create(array(
    'subject'  => 'The quick brown fox jumps over the lazy dog',
    'comment'  => array(
        'body' => 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.'
    ),
    'priority' => 'normal'
));
```
## Create a ticket with an attachment

```php
//v1
$attachmentData = array(
    'file' => getcwd() . '/vendor/zendesk/zendesk_api_client_php/tests/assets/UK.png',
    'name' => 'UK test non-alpha chars.png'
);

// Create a new ticket
$newTicket = $client->tickets()->attach($attachmentData)->create(array(
    'subject'  => 'The quick brown fox jumps over the lazy dog',
    'comment'  => array(
      'body' => 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.'
    ),
    'priority' => 'normal',
    'file' => $attachmentData['file']
));
```
```php
//v2
//upload an attachment
$attachmentData = array(
    'file' => getcwd() . '/vendor/zendesk/zendesk_api_client_php/tests/assets/UK.png',
    'name' => 'UK test non-alpha chars.png'
);

// Create a new ticket
$newTicket = $client->tickets()->attach($attachmentData)->create(array(
    'subject'  => 'The quick brown fox jumps over the lazy dog',
    'comment'  => array(
      'body' => 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.'
    ),
    'priority' => 'normal',
    'file' => $attachmentData['file']
));
```

## Get & create a user
```php
//v1
$listusers = $client->users()->findAll();
$newuser = $client->users()->create(array(
  'name' => 'foo bar',
  'email' => 'foo@bar.com'
));
```
```php
//v2
$listusers = $client->users()->findAll();
$newuser = $client->users()->create(array(
  'name' => 'foo bar',
  'email' => 'foo@bar.com'
));
```
## Get & Create Organizations
```php
//v1
$listOrganizations = $client->organizations()->findAll();


$newOrganzation = $client->organizations()->create(array(
  'name' => 'New World2',
  'url' => 'www.n3world2.com'

));

```
```php
//v2
$listOrganizations = $client->organizations()->findAll();


$newOrganzation = $client->organizations()->create(array(
  'name' => 'New World2',
  'url' => 'www.n3world2.com'

));

```

## List triggers
```php
//v1
$results = $client->triggers()->active();

```
```php
//v2
$results = $client->triggers()->findActive();

```

## Chaining
``` php
//v1
$results = $client->tickets(2)->comments()->findAll();

```
```php
//v2
$results = $client->tickets(2)->comments()->findAll();

```
## Sideloading
``` php
//v1
$results = $client->tickets()->find(['id' => [1], 'sideload' => ['users', 'groups']]);

```
```php
//v2
$results = $client->tickets()->find(1, ['sideload' => ['users', 'groups']]);

```
## Search
``` php
//v1
$query = 'sample';
$params = array(
    'query' => $query
);
$results = $client->search($params);

```
```php
//v2
$results = $client->search()->find('sample');

```

## Error handling

V1 of the PHP API client did not provide the actual error instead it throws an exception. V2 returns you the actual error. 

``` php
//example for searching for a ticket that doesn't exist
//v1
Fatal error: Uncaught exception 'Zendesk\API\ResponseException' with message 'Response to Zendesk\API\Tickets::find is not valid. Call $client->getDebug() for details' in /Users/idejesus/code/phptest v1/vendor/zendesk/zendesk_api_client_php/src/Zendesk/API/Tickets.php:118
Stack trace:
#0 /Users/idejesus/code/phptest v1/v1.php(18): Zendesk\API\Tickets->find()
#1 {main}
  thrown in /Users/idejesus/code/phptest v1/vendor/zendesk/zendesk_api_client_php/src/Zendesk/API/Tickets.php on line 118
```
```php
//v2
Fatal error: Uncaught exception 'Zendesk\API\Exceptions\ApiResponseException' with message 'Not Found [status code] 404 [details] {"error":"RecordNotFound","description":"Not found"}' in /Users/idejesus/code/phptest v2/vendor/zendesk/zendesk_api_client_php/src/Zendesk/API/Http.php:116
Stack trace:
#0 /Users/idejesus/code/phptest v2/vendor/zendesk/zendesk_api_client_php/src/Zendesk/API/HttpClient.php(365): Zendesk\API\Http::send(Object(Zendesk\API\HttpClient), 'tickets/20.json', Array)
#1 /Users/idejesus/code/phptest v2/vendor/zendesk/zendesk_api_client_php/src/Zendesk/API/Traits/Resource/Find.php(44): Zendesk\API\HttpClient->get('tickets/20.json', Array)
#2 /Users/idejesus/code/phptest v2/v2.php(77): Zendesk\API\Resources\Core\Tickets->find()
#3 {main}
  thrown in /Users/idejesus/code/phptest v2/vendor/zendesk/zendesk_api_client_php/src/Zendesk/API/Http.php on line 116