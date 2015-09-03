# WIP Upgrade Guide

Intro to why we rewrote the API Client.

## Namespacing
Client -> HTTPClient

## Authentication

```php
// v1
use Zendesk\API\Client as ZendeskAPI;

// v2
use Zendesk\API\HTTPClient as ZendeskAPI;
```

## Basic operations

Most basic operations are very similar. We've tried to keep them as consistent as possible between v1 and v2, but there are some slight differences.

Get & create a ticket


Get & create a user
Get & create organizations
List triggers


## Chaining

## Sideloading

## Search

## Error handling

v1 there was nothing

v2 it's a LOT better
