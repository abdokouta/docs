# introduction

Maginium Framework\Crud is an elegant and efficient filtering and sorting package for Maginium, designed to simplify complex data filtering and
sorting logic for eloquent queries. By simply adding `filter()` to your Eloquent query, you can add the ability for frontend users to apply filters
based on URL query string parameters like a breeze.

Features :

- Rename and restrict fields
- Various filter methods
- Simple installation and usage
- Filter by relation columns
- Custom filters
- Multi-column sort

Maginium Framework\Crud is not only developer-friendly but also front-end developer-friendly. Frontend developers can effortlessly filter and sort
APIs using the popular [JavaScript qs](https://www.npmjs.com/package/qs) package.

Strapi's filter and sort functionality inspires the way this package handles filters.

### How Does Framework\Crud Work?

Here is a basic usage example to clarify Framework\Crud's use case.

Add `filter()` to your query.

```php
$posts = Post::filter()->get();
```

That's it! Now you can filter your posts by adding query string parameters to the URL.

```
GET /api/posts?filters[title][$contains]=Framework\Crud
```
