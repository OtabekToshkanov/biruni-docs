---
icon: memo-circle-check
---

# Pagination management in ORACLE

Effective data management and retrieval are crucial in any database-driven application. To ensure efficiency and performance, it's essential to implement pagination. Pagination not only improves response times but also prevents overloading the server and client with excessive data. This documentation outlines the guidelines and options available for implementing pagination in Oracle.

#### When to use Pagination?

Pagination should be used in any of the following scenarios:

* **External APIs** - All external APIs should return paginated datasets.
* **Large datasets** - All large datasets (more than 100 rows) should be served with pagination.

#### When NOT to use Pagination?

Pagination can be omitted in any of the following scenarios:

* **Small datasets** - When the dataset consistently contains a limited number of rows (less than 100 rows).
* **Reports** - When processing the entire dataset at once, such as for report generation.

## Pagination Options

Two options are provided:

* **Cursor-based pagination** - using `modified_id` column.
* **Offset-based pagination** - using `fazo_query` mechanism.

***

### Cursor-based pagination

This method uses the `modified_id` column as a cursor to fetch only modified rows.\
The `modified_id` column value is generated from the `biruni_modified_sq` global sequence.\
The `modified_id` column is automatically incremented by the `fazo_z` mechanism in each `INSERT` & `UPDATE` operation.

#### ✅When to use cursor-based pagination?

* **Fetching only updated values** - If you need statically ordered by earliest modified rows.
* **Fetching the full entity** - if you need efficient fetching of all rows starting from a specific point (modified\_id = some\_value) avoiding excessive offsetting.

#### ⚠️Downsides

* **No sorting (static ordering)** - This method statically orders by modified\_id only.
* **No filtering** - This method doesn't allow dynamic filtering.

#### 🛠️Implementation

Before implementing, ensure that entity tables have a `modified_id` column.

1. Call **`Uit_Mx.Prepare_Fetch_Params`** to get `start_id` and `limit`.
2. Query your data starting from `start_id` and fetch the first `limit` rows.
3. Return the result of the function **`Uit_Mx.Prepare_Api_Response`** to add `count` and `next_cursor` properties into your response data.

<figure><img src="../../../.gitbook/assets/advanced-topics/oracle-modules/extra-topics/pagination.png" alt=""><figcaption></figcaption></figure>

#### Example Structure of Result

```
{
  "meta": {
    "count": total_number_of_rows,
    "next_cursor": cursor_for_using_in_the_next_request
  },
  "data": [
    { "column1": value1, "column2": value2, ... },
    { "column1": value1, "column2": value2, ... },
    ...
  ]
}
```

***

### Offset-based pagination

This method uses the **`fazo_query`** mechanism under the hood. Which lets you **fetching only selected columns**, **filtering**, and **sorting**.

**`b.run_query()`** function executes the `fazo_query` engine and returns a **`gmap`** object containing the **`count`** and **`rows`** properties.

#### ✅When to use offset-based pagination?

* **Fetching filtered or sorted rows** - It allows dynamically ordering.
* **Fetching only initial pages of rows** - It efficiently fetches only initial pages.

#### ⚠️Downsides

* **Less efficient when full entity fetching** - It fetches all the rows until `limit`, only after that offsets. So, only the initial pages are efficient.

#### 🛠️Implementation

1. Run `b.run_query` to execute the `fazo_query`.

<figure><img src="../../../.gitbook/assets/advanced-topics/oracle-modules/extra-topics/query.png" alt=""><figcaption></figcaption></figure>

#### Example Structure of Result

```
{
  "count": total_number_of_rows,
  "data": [
    { "column1": value1, "column2": value2, ... },
    { "column1": value1, "column2": value2, ... },
    ...
  ]
}
```

## Conclusion

By implementing these pagination mechanisms, the database operations can be significantly improved. Choose the option that best suits your needs based on the nature of your data and the specific requirements of your application.
