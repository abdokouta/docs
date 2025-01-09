# 🔢 Pagination

Pagination is a crucial feature in web applications, enabling you to divide large datasets into smaller, more manageable chunks. Maginium offers a
powerful and flexible pagination system that integrates seamlessly with the framework's data querying capabilities.

---

### Getting Started

Pagination in Maginium is implemented using the built-in paginator. The paginator can be used with database queries, collections, or any iterable
dataset to provide efficient navigation across multiple pages.

> **Important**: Maginium's paginator is optimised for both performance and user experience, offering intuitive methods for customization.

---

### Using Pagination with Query Builder

The query builder's `paginate` method simplifies database pagination:

```php
use Maginium\Framework\Database\Query;

$users = Query::table('users')->paginate(10);
```

This generates a paginated result set containing 10 records per page.

#### Accessing Pagination Data

Once you have a paginated result set, you can access additional pagination data:

```php
$users->currentPage(); // Current page number
$users->lastPage(); // Total number of pages
$users->perPage(); // Items per page
$users->total(); // Total items
$users->items(); // Data for the current page
```

---

### Pagination Links

The `links` method generates HTML for pagination controls:

```php
echo $users->links();
```

{% hint style="info" %} Pagination links are automatically styled using Maginium's default CSS classes. You can override these styles for custom
designs. {% endhint %}

---

### Customizing Pagination

Maginium's paginator allows customization of the number of items per page and query parameters:

```php
$users = Query::table('users')->paginate(15, 'page', ['*'], 'userPage');
```

#### Advanced Customization

You can modify the pagination view to meet your application's needs:

```php
$users->setPageName('custom_page');
$users->appends(['filter' => 'active']);
```

---

### Pagination with Collections

If you're working with collections, you can use the `forPage` method:

```php
use Maginium\Framework\Support\Collection;

$collection = new Collection(range(1, 100));
$paginated = $collection->forPage(2, 10);

// Returns items 11 to 20
```

---

### API Pagination

For API responses, Maginium provides JSON structures that include pagination metadata:

```php
return $users->toJson();
```

The response includes:

- **Data**: Current page's dataset
- **Meta**: Pagination information (e.g., total items, current page, last page)

#### Example Response

```json
{
  "data": [
    { "id": 1, "name": "John" },
    { "id": 2, "name": "Jane" }
  ],
  "meta": {
    "current_page": 1,
    "last_page": 10,
    "per_page": 10,
    "total": 100
  }
}
```

---
