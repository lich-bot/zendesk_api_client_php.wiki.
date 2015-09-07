# WIP Upgrade Guide

Intro to why we rewrote the API Client.

## Namespacing
Client -> HTTPClient

## Authentication

```php
// v1
use Zendesk\API\Client as ZendeskAPI;
$client->setAuth('token', $token);

// v2
use Zendesk\API\HTTPClient as ZendeskAPI;
$client->setAuth('basic', ['username' => $username, 'token' => $token]);
```

## Basic operations

Most basic operations are very similar. We've tried to keep them as consistent as possible between v1 and v2, but there are some slight differences.

Get & create a ticket
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

Get & create a user
```php
//v1
$listusers = $client->users()->findAll();
$newuser = $client->users()->create(array(
  'name' => 'foo bar',
  'email' => 'foo@bar.com'
));

//v2
$listusers = $client->users()->findAll();
$newuser = $client->users()->create(array(
  'name' => 'foo bar',
  'email' => 'foo@bar.com'
));
```
Get & create organizations
```php
//v1
$listOrganizations = $client->organizations()->findAll();
print_r($listOrganizations);
$newOrganzation = $client->organizations()->create(array(
  'name' => 'New World2',
  'url' => 'www.n3world2.com'

));
print_r($newOrganzation);

//v2
$listOrganizations = $client->organizations()->findAll();
print_r($listOrganizations);
$newOrganzation = $client->organizations()->create(array(
  'name' => 'New World2',
  'url' => 'www.n3world2.com'

));
print_r($newOrganzation);
```php

List triggers


## Chaining

## Sideloading

## Search

## Error handling

v1 there was nothing

v2 it's a LOT better