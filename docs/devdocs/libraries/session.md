---
sidebar_position: 6
---

# Session

**Table** is a _**Wanuau**_ built in library that can be used to show records in a table.

A table used to show records is a structured way to organize and display data. Think of it like a grid made up of rows and columns, where each row is a single record (or entry), and each column holds a specific type of information (or attribute) about that record.

<!-- markdownlint-disable MD041 -->

## Table APIs

The following are the APIs that can be used to configure our **Table**.

- [Create instance of **Table**](#create-instance-of-table)
- [Get sorting column](#get_sorting_column)
- [Get sorting direction](#get_sorting_direction)
- [Get table ID](#get_table_id)
- [Set table header](#set_header)
- [Set table column](#set_column)
- [Set table column link](#set_column_link)
- [Set table column download link](#set_column_download_link)
- [Set table with pagination](#with_pagination)
- [Set table to be sortable](#sort)
- [Set table with search](#with_search)
- [Set table with filter](#with_filter)
- [Set table with checkbox](#with_checkbox)
- [Set table with entry](#with_entry)
- [Set table data](#set_data)
- [Display the table](#render_table)

### Create instance of **Table**

To create a new instance of **Table**, it takes one parameter which is:

- Param `tableId`: Table unique ID `string` (Required)

```php
$table = new Table('user-table');
```

### get_sorting_column

This API does not accept any parameters and returns default/selected/from-session sorting `column`.

**Calling the API:**

```php
$table->get_sorting_column();
```

### get_sorting_direction

This API does not accept any parameters and returns default/selected/from-session sorting `direction`.

**Calling the API:**

```php
$table->get_sorting_direction();
```

### get_table_id

This API does not accept any parameters and returns table id defined when creating an instance of **Table**.

**Calling the API:**

```php
$table->get_table_id();
```

### set_header

This API is used to set table's header labels and accepts one array parameter contains header labels, for example: `#`, `Name`, `Avatar`, `Suspended` and etc.
:::warning Note
Number of headers should match number of columns.
:::

**Calling the API:**

```php
$table->set_header(['#', 'Name', 'Avatar', 'Suspended']);
```

### set_column

This API is used to set table's column using database column names and accepts one array parameter contains database column name, for example: `id`, `name`, `avatar`, `suspended` and etc.
:::warning Note
Number of columns should match number of headers.
:::

**Calling the API:**

```php
$table->set_column(['id', 'name', 'avatar', 'suspended']);
```

### set_column_link

This API is used to set a column to be/have a link and accepts one array parameter contains target column_name as `key` and link as the `value`.

Bellow example is adding a link to column `name` and `name_link` is added to the data that will be sent to Table.
:::warning Note
Column name `name` should match the column set in **[set_column](#set_column)**.
:::

To generate column link you can add it to each record like in **[set_data](#4-adding-additional-data-to-each-record)**.

**Calling the API:**

```php
$table->set_column_link(['name' => 'name_link']);
```

### set_column_download_link

This API is used to set a column to be/have a download link and accepts one array parameter contains target column_name as `key` and download link as the `value`.

Bellow example is adding a link to column `avatar` and `avatar_download_link` is added to the data that will be sent to Table.
:::warning Note
Column name `avatar` should match the column set in **[set_column](#set_column)**.
:::

To generate column download link you can add it to each record like in **[set_data](#4-adding-additional-data-to-each-record)**.

**Calling the API:**

```php
$table->set_column_download_link(['avatar' => 'avatar_download_link']);
```

### with_pagination

This API is used to use pagination in table and accepts two parameters, which are:

- Param `numberOfRecords` is a number of records - `int` (Required).
- Param `paginationElementId` is an element in which the pagination is wrapped/located `string` (Required).

:::warning Note
This API must be called before calling:

- `$table->pagination->get_page_number()`.
- `$table->pagination->get_result_per_page()`.
:::

**Calling the API:**

```php
// Get number of users from UserModel.
$numberOfUsers = UserModel::all->get('numrows');

$table->with_pagination($numberOfUsers, 'user-table');
```

### sort

This API is used to set a table to be sortable and accepts two parameters, which are:

- Param `sortable`: set a table to be sortable - `bool` (Optional).
- Param `defaultSortColumn`: set a default sorting column `string` (Optional).

**Calling the API:**

```php
// Set table to be sortable and default sorting column will be defaulted to "1",
// which is first column of the table.
$table->sort(true);

// Set table to be sortable and default sorting column is "name".
$table->sort(true, 'name');
```

### with_search

This API is used to set to use search in table and accepts two parameters, which are:

- Param `searchKeyword`: search keyword used by the user - `string` (Optional).
- Param `searchPlaceholder`: search input placeholder - `string` (Optional).

:::tip Tip
To get user search from input or session, call `$prop->http_request->get_wml_table_search()`.

API `get_wml_table_search` accepts one parameter:

- Param `tableId` is id used to create an instance of **Table** - `string` (Optional, but `required` when calling outside **Table**)
:::

**Calling the API:**

```php
// Get search from user input/session
$tableSearch = $prop->http_request->get_wml_table_search('user-table');

// Without user's defined placeholder.
$table->with_search($tableSearch);

// With user's defined placeholder.
$table->with_search($tableSearch, 'Search name...');
```

### with_filter

This API is used set to use filter in table and accepts one parameter which is:

- Param `filterColumn`: is array of database columns based on the fetched records - `array` (Optional, but required to show the filter)

:::note Note
The filter should at least have `'label'`, `'column'` and `'input'` keys in each array filter. If the `'input'` = `'select'` then key `'options'` is required and should be defined.

There is a `'type'` key for `'field'` input type. If `'type'` is defined the input type will be set to that, otherwise it will be set to `'text'`.
:::

Check the following API call:

```php
// Input 'filed' type will be 'text` due to not being set explicitly.
$table->with_filter([
    [
        'label'  => 'Name',
        'column' => 'name',
        'input'  => 'field', // It will generate input field
    ],
    [
        'label'  => 'Suspended',
        'column' => 'suspended',
        'input'  => 'select',
        'options' => [
            [
                'label' => 'Not selected',
            ],
            [
                'label' => 'Yes',
                'value' => 1,
            ],
            [
                'label' => 'No',
                'value' => 0,
            ],
        ]
    ],
]);
```

### with_checkbox

This API is used set to use checkbox in table and accepts two parameters which are:

- Param `bulkActionButtons`: checkbox bulk action button - `array` (Required)
    - Group key should be unique to two actions for example: `'suspend:restore'`, the action must always be paired with `restore`.
    - Value should have two array of actions for example: `'suspend'` and `'restore'`. Inside each action array, there must be the following `keys` set.
        - Set `'text'` => `'Suspend'`: Used as a label (Required).
        - Set `'class'` => `'btn btn-warning'`: Used as the action button style (Required).
        - Set `'icon'` => `'fa fa-user-slash'`: Used as the action button icon (Required).
        - Set `'action'` => `'suspend'`: Action used by the action button (Required).
        - Set `'route'` => `'setting/users/suspend'`: Action used by the action button (Required).
        - Set `'method'` => `Route::POST`: Action used by the action button (Required).
        - Set `'confirm'` => `true`: Action used by the action button (Optional, defaulted to `false`).
- Param `style`: use to style how the action buttons are displayed - `string` (Optional: `'group-regular'`, `'group-advanced'`, `'individual'`; Default: `'group-advanced'`)

**Calling the API:**

```php
$table->with_checkbox([
    // Suspend/restore bulk actions.
    'suspend:restore' => [
        'suspend' => [
            'text' => 'Suspend',
            'class' => 'btn btn-warning',
            'icon' => 'fa fa-user-slash',
            'action' => 'suspend',
            'route' => 'setting/users/suspend',
            'method' => Route::POST,
            'confirm' => true,
        ],
        'restore' => [
            'text' => 'Restore',
            'class' => 'btn btn-success',
            'icon' => 'fa fa-undo',
            'action' => 'restore',
            'route' => 'setting/users/suspend',
            'method' => Route::POST,
            'confirm' => true,
        ],
    ],
]);
```

### with_entry

This API is used set to use entry selection in table.

**Calling the API:**

```php
$table->with_entry();
```

### set_data

This API is used set data that will be used in **Table**.

#### 1. Get records from table

```php
// Get user records.
$users = UserModel::all->get();
```

#### 2. Get records from table with search and filter

```php
// Get user search input/session.
$tableSearch = $prop->http_request->get_wml_table_search('user-table');

// Get user filter input/session.
$tableFilter = $prop->http_request->get_wml_table_filter('user-table');

// Get user records based on search (on "username") or filter.
$users = UserModel::append($tableFilter ?? DML::where_like('username', $tableSearch))->get();
```

#### 3. Get records from table with [search](#with_search), [filter](#with_filter), [sorting column](#get_sorting_column), [sorting direction](#get_sorting_direction) and [pagination](#with_pagination)

Let's use `$tableFilter` and `$tableSearch` from the [#2](#2-get-records-from-table-with-search-and-filter).

```php
// Set to use pagination.
$table->with_pagination(
    UserModel::append($tableFilter ?? DML::where_like('username', $tableSearch))->get('numrows'),
    'user-table',
);

// Get user records based on search (on "username") or filter.
$users = (new UserModel)
      ->append($tableFilter ?? DML::where_like('username', $tableSearch))
    ->order_by($table->get_sorting_column(), $table->get_sorting_direction())
       ->limit($table->pagination->get_page_number(), $table->pagination->get_result_per_page())
->get();
```

#### 4. Adding additional data to each record

If we want to add more data to each record like `column_link`, `column_download_link` or `toggle_switch`, we can see how they are achieved.

:::note Note
If adding a `toggle_switch`, the key name must be set to database `'column_name'` + `'_switch'`. For example, `'suspended_switch'` like in the following example where `'suspended'` is a database column name and `'_switch'` is appended to it.
:::

```php
foreach ($users as $user) {
    // Add suspended_switch key/value to UserModel.
    $user->set('suspended_switch', SwitchToggle::render(
        id          : 'MUST BE UNIQUE' => $user->get_username().'-suspend-'.$user->get_id(),
        value       : (string) 'MUST BE A RECORD UNIQUE ID (PK)' => $user->get_id(),
        action      : 'ACTION:RESTORE' => 'suspend:restore',
        checkboxId  : 'MUST BE TABLE ID-cb-RECORD UNIQUE ID (PK)' => $table->get_table_id()."-cb-" . $user->get_id(),
        route       : 'MUST A VALID ROUTE' => 'setting/users/suspend',
        enabled     : (bool) $user->get_suspended(),
    ));

    // Set column link.
    $user->set('name_link', 'setting/users/' . $user->get_id());

    // Set column download link.
    $user->set('avatar_download_link', 'setting/users/' . $user->get_avatar());
}
```

#### 5. Convert array of DML records to array of array records

:::note Note
Result of `$user` records are in a form of arrays of DML, so we need to convert them into array of array record before sending them to **Table**.
:::

**Calling the API:**

```php
// We need to convert them to array records using "dml_to_array_record" function.
$users = dml_to_array_record($users);
```

### render_table

This API is used to render the **Table** based on the all the configurations.

**Calling the API:**

```php
// Set table data.
$table->render_table();
```
