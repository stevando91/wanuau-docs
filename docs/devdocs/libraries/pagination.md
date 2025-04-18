---
sidebar_position: 6
---

# Pagination

**Pagination** is a _**Wanuau**_ built in library that is used to split up large amounts of content into separate pages. Instead of loading everything at once—which can slow down a site or overwhelm users—pagination helps break it up into smaller, more manageable chunks.

## Why use pagination?

- **Performance**: Loading hundreds or thousands of items (e.g., products, blog posts) at once can slow down a webpage.
- **User Experience**: Easier for users to browse through smaller sets of content.
- **Organization**: Helps categorize or structure content clearly.

## Setting up the route

When using **Pagination**, the route has to include an extra uri of `Route::PAGINATION_PAGE` and it has to be a POST http request method.

Please click **[here](../route)** to learn how to create a route.

```php
Route::POST => [
    '/user-list/'.Route::PAGINATION_PAGE => [
        Route::CALLBACK => [UserListControllerAPI::class, 'user_list'],
    ],
],
```

## Using the namespace

```php
use Wanuau\Lib\Pagination\Pagination;
```

## Create instance of **Pagination**

To create a new instance of **Pagination**, it takes two parameter which are:

- Param `numberOfRecords` is a number of records - `int` <span style={{ color: 'red' }}>(Required)</span>.
- Param `paginationElementId` is an element in which the **Pagination** is wrapped/located `string` <span style={{ color: 'red' }}>(Required)</span>.

```php
$pagination = new Pagination(100, 'user-list');
```

## API

The following are the APIs that can be used to configure our **Pagination**.

- [Get current page number](#get_page_number)
- [Get number of results shown per page](#get_result_per_page)
- [Render the **Pagination** links](#links)

### get_page_number

This API accepts no parameters and returns current page number when using **Pagination**.

This is usually used inside a query.

**Calling the API:**

```php
$pagination->get_page_number();
```

### get_result_per_page

This API accepts no parameters and returns number of results that will be shown when using **Pagination**.

This is usually used inside a query.

**Calling the API:**

```php
$pagination->get_result_per_page();
```

### links

This API accepts no parameters and renders the **Pagination** links.

**Calling the API:**

```php
$pagination->links();
```
