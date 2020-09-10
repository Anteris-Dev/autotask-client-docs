# Getting Started

---

- [The Client Wrapper](#client-wrapper)
- [Bypassing the Client Wrapper](#bypassing-client-wrapper)

<a name="client-wrapper"></a>
## The Client Wrapper

For the purpose of ease, there is a client wrapper that allows you to easily interact with every Autotask service. This can be created as shown below.

```php
$client = new Anteris\Autotask\Client($apiUser, $apiSecret, $integrationCode, $baseUrl);
```

From here, you can use any service by referencing its class method as shown below. The class wrapper caches each instantiation of the service making it efficient.

> {success} Class methods are named the camel case version of their endpoint name (e.g. Contacts is `contacts()`, ContactGroups is `contactGroups`).

For example:

```php
$client->contacts();
```

<br>

To find the base URL for your company, go to [https://webservices.autotask.net/atservicesrest/v1.0/zoneInformation?user=email@example.com](https://webservices.autotask.net/atservicesrest/v1.0/zoneInformation?user=) where `email@example.com` equals the username of your API user.

<a name="bypassing-client-wrapper"></a>
## Bypassing the Client Wrapper

If you are only interacting with one or two Autotask endpoints, you may wish to bypass the client wrapper. You can do this as shown below.

```php
// Note that we are using the HttpClient, not the wrapper Client.
$client = new Anteris\Autotask\HttpClient($apiUser, $apiSecret, $integrationCode, $baseUrl);

$contactEndpoint = new Anteris\Autotask\API\Contacts\ContactService($client);
$ticketEndpoint  = new Anteris\Autotask\API\Tickets\TicketService($client);
```
