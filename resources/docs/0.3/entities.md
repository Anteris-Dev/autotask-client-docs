# Entities

---

- [Getting Started with Entities](#entities)
- [Collections](#collections)

<a name="entities"></a>
## Getting Started with Entities

This package relies on [Data Transfer Object](https://github.com/spatie/data-transfer-object) classes to represent Autotask resources. When creating or updating an Autotask resource, you must pass the service class an instance of that resource's entity.
<br><br>
These entities can be found under the API directory of this package and under the plural name of their endpoint. The class name is the singular form of the endpoint suffixed by "Entity".
<br><br>
When constructing an entity you may pass it an array of key / value pairs. If Autotask requires a field be set, it is required by the entity as well and will throw an exception if it is not present. Entities can be converted back to arrays by calling the `toArray()` method.

<br>
For example:

```php
$ticket = new Anteris\Autotask\API\Tickets\TicketEntity([...]);
```

<a name="collections"></a>
## Collections

If multiple entities are being handled (for example: the result of a `query()` request) they are put into a collection. Collections can be found under the API directory of this package and under the plural name of their endpoint. The class name is the singular form of the endpoint suffixed by "Collection". Collections are iterable.
<br><br>
When constructing a collection you may pass it an array of entities. A collection can be converted to an array by calling the `toArray()` method.

<br>
For example:

```php
use Anteris\Autotask\API\Tickets\TicketCollection;
use Anteris\Autotask\API\Tickets\TicketEntity;

$ticket1 = new TicketEntity([...]);
$ticket2 = new TicketEntity([...]);

$tickets = new TicketCollect([
    $ticket1,
    $ticket2
]);

foreach ($tickets as $ticket) {
    echo $ticket->ticketNumber . PHP_EOL;
}
```
