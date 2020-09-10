# Query Builder

---

- [Introduction](#introduction)
- [Filtering](#filtering)
- [Specifying the Number of Records](#records)
- [Obtaining a record count](#count)
- [Executing your Query](#executing-query)
- [Pagination](#pagination)
- [Looping](#looping)

<a name="introduction"></a>
## Introduction
Autotask endpoints that can be queried have a `query()` method. This returns a query builder class that has several useful methods. These and examples of their use may be found below.

<a name="filtering"></a>
## Filtering
When making requests to Autotask, it is useful to make comparisons against the fields. The following comparisons can be used with the filter methods.

| Operator   | Definition                                                                                                                        |
|------------|-----------------------------------------------------------------------------------------------------------------------------------|
| eq         | Requires that the field value match the exact criteria provided.                                                                  |
| noteq      | Requires that the field value be anything other than the criteria provided.                                                       |
| gt         | Requires that the field value be greater than the criteria provided.                                                              |
| gte        | Requires that the field value be greater than or equal to the criteria provided.                                                  |
| lt         | Requires that the field value be less than the criteria provided.                                                                 |
| lte        | Requires that the field value be less than or equal to the criteria provided.                                                     |
| beginsWith | Requires that the field value begin with the defined criteria.                                                                    |
| endsWith   | Requires that the field value end with the defined criteria.                                                                      |
| contains   | Requires that the field value contains the defined criteria.                                                                      |
| exist      | Enter exist to query for field values that **are not** null.                                                                      |
| notExist   | Enter notExist to query for field values that **are** null.                                                                       |
| in         | With this value specified, the query will return only the values in the list array that match the field value you specify.        |
| notIn      | With this value specified, the query will only return the values in the list array that do not match the field value you specify. |

### Where Filter
The `where()` method adds a where filter to the query. You may pass a comparison to it or a callback to create a sub-query. Multiple where statements may be chained.<br><br>

For example:

```php
// Basic Comparison
$client->contacts()->query()->where('firstName', 'eq', 'Foo');

// Make sure the field exists
$client->contacts()->query()->where('emailAddress', 'exist'); // OR notexist

// Make a sub-query (these are grouped with the AND conjunction by default)
// This query means: firstName equals Foo AND emailAddress exists

$client->contacts()->query()->where(function ($q) {
    return $q->where('firstName', 'eq', 'Foo')
             ->where('emailAddress', 'exist');
});
```

<br>

Full options for the `where()` method are as shown below:

```php
where( $field, $operator = null, $value = null, $udf = false, $conjuction = 'AND' )
```

### Or Where
The `orWhere()` method acts as the `where()` method above, but uses an OR conjunction.<br><br>

For example:

```php
// Make a sub-query (these are grouped with the OR conjunction by default)
// This query means: firstName equals Foo OR emailAddress exists

$client->contacts()->query()->orWhere(function ($q) {
    return $q->where('firstName', 'eq', 'Foo')
             ->where('emailAddress', 'exist');
});
```

<br>

Full options for the `orWhere()` method are shown below:

```php
orWhere( $field, $operator = null, $value = null, $udf = false )
```

<br>

This method returns the following call:

```php
$this->where($field, $operator, $value, $udf, 'OR');
```

<a name="records"></a>
## Records
You may specify how many records you would like returned by the query by passing an integer to the `records()` method. The integer passed must be between 1 and 500.<br><br>

For example:

```php
$client->contacts()->query()->records(20);
```

<a name="count"></a>
## Count
Once you have defined all your filters, you may run the `count()` method to see how many records the request would return. This may be helpful for planning out pagination, etc.<br><br>

For example:

```php
// This returns an integer specifying how many records will be returned
$result = $client->contacts()->query()->where('firstName', 'contains', 'Foo')->count();

if ($result > 0) {
    echo 'Records exist!';
}
```

<a name="executing-query"></a>
## Executing your Query
Once you have defined all your filters, you may run the `get()` method to actually make the request against the Autotask API and return a collection of items containing the result.<br><br>

For example:

```php
$result = $client->contacts()->query()->where('firstName', 'contains', 'Foo')->get();

foreach ($result as $contact) {
    echo $contact->firstName . PHP_EOL;
}
```

<a name="pagination"></a>
## Pagination
The `get()` method is great, but what if I have over 500 results? What if I want to page the results? We have you covered! Use the `paginate()` method to return a Paginator object with helper properties and methods. These are explained below.

### Collection Property
When using a paginator object, you reference the collection of entities with the `collection` property.

### Has Next Page
The `hasNextPage()` method returns true if a next page exists. If not, it returns false.

### Has Previous Page
The `hasPrevPage()` method returns true if a previous page exists. If not, it returns false.

### Next Page
The `nextPage()` method attempts to retrieve the next page and returns it.

### Previous Page
The `prevPage()` method attempts to retrieve the previous page and returns it.

<br><br>
Let's take a look at an example of these helper methods in action.

```php
// Let's get the first page
$page = $client->contacts()->query()->where('firstName', 'exist')->paginate();

// We are going to do this until we run out of pages
while(true) {
    // For every contact, print their name and a new line
    foreach ($page->collection as $contact) {
        echo $contact->firstName . PHP_EOL;
    }

    // If a next page doesn't exist, stop the loop
    if(! $page->hasNextPage() ) {
        break;
    }

    // Otherwise retrieve the next page and do it again!
    $page = $page->nextPage();
}
```

<a name="looping"></a>
## Looping
Instead of iterating over all the records in Autotask via the paginate() method, you can very easily do this with the loop method. This method takes a callback function which will be executed for every record in Autotask that meets the criteria.

> {warning} The records iterated over here could be greater than 500, which is the Autotask return limit. Just be mindful of throttling.

For example:

```php
use Anteris\Autotask\API\Contacts\ContactEntity;

$query = $client->contacts()->query()->where('id', 'exist');

// Get all the contacts from Autotask and echo their name
$query->loop(function (ContactEntity $contact) {

    echo $contact->firstName . PHP_EOL;

});
```
