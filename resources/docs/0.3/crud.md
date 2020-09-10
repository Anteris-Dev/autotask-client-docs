# CRUD Actions

---

- [Introduction](#introduction)
- [Create](#create)
- [Read](#read)
- [Update](#update)
- [Delete](#delete)

<a name="introduction"></a>
## Introduction
**C**reate **R**ead **U**pdate and **D**elete. These are the important actions you will need to make against the Autotask API and we've got you covered. Below we will describe the ways you can interact with Autotask resources.

<br>

These actions are performed by methods that exist on the service class. This can be easily accessed through the wrapper client, or created separately. To learn about bypassing the wrapper class, look [here](overview#bypassing-client-wrapper).

> {danger} If Autotask does not support one of these actions on the entity, its method will not exist on the service. To check one of these actions on the entity you are curious about, check out the [Autotask documentation](https://autotask.net/help/DeveloperHelp/Content/AdminSetup/2ExtensionsIntegrations/APIs/REST/Entities/_EntitiesOverview.htm).

<a name="create"></a>
## Create
To create a new resource, you can use the `create()` method on the resource service. The `create()` method takes a single entity that represents the resource. The client will then make a request against the Autotask API. If any errors are encountered along the way, an exception is thrown.

> {warning} Autotask requires that new entities have an ID of 0. Don't forget this as you build your entity!

For example:

```php
// Create a contact entity representing the contact I want to create...
$contact = new Anteris\Autotask\API\Contacts\ContactEntity([
    'id'        => 0,
    'companyID' => 123,
    'firstName' => 'New',
    'lastName'  => 'User',
    'isActive'  => 1,
]);

// Actually create the contact
try {

    $client->contacts()->create($contact);

} catch ( Throwable $error ) {
    
    echo "There was an error creating that ticket: {$error->getMessage()}";

}
```

<a name="read"></a>
## Read
There are several ways to read resources from Autotask. If you are looking to retrieve a single resource, our recommendation is the `findById()` method on the resource service. The `findById()` method takes a single integer that represents the resource ID. The client will then make a request against the Autotask API. If any errors are encountered along the way, an exception is thrown.

<br>

For example:

```php
try {

    $client->contacts()->findById(1);

} catch ( Throwable $error ) {
    
    echo "That contact was not found!";

}
```

<br>

Another way to read resources is by creating a query to retrieve multiple assets. This can be done by using the `query()` method which returns a new QueryBuilder. To learn more about query builders, see the [query builder](/{{route}}/{{version}}/query-builder) section.


<a name="update"></a>
## Update
To update resources in Autotask, you can use the `update()` method on the resource service. This method takes a single entity which will then be updated using the Autotask API.

> {warning} Currently the `update()` method updates the resource using a **PUT** request. Because of this, you need to be careful to ensure the entity looks **exactly** like you want it to in the Autotask database. To accomplish this, we recommend executing a `findById()` request and making your updates to the result before sending the update.

For example:

```php

// Retrieve the contact
try {

    $contact = $client->contacts()->findById(1);

} catch ( Throwable $error ) {

    die("Unable to retrieve contact: {$error->getMessage()}");

}

// Make the changes
$contact->firstName = 'Aidan';

// Perform the update
try {

    $client->contacts()->update($contact);

} catch ( Throwable $error ) {

    echo "Unable to update the contact: {$error->getMessage()}";

}

```

<a name="delete"></a>
## Delete
To delete resources in Autotask, you can use the `deleteById()` method on the resource service. The `deleteById()` method takes a single integer that represents the resource ID. The client will then make a request against the Autotask API. If any errors are encountered along the way, an exception is thrown.

<br>

For example:

```php

try {

    $client->contacts()->deleteById(1);

} catch ( Throwable $error ) {

    echo "Unable to delete the resouse: {$error->getMessage()}";

}

```
