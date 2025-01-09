---
title: Introduction
description: Laravel Framework\Crud is an elegant and efficient filtering and sorting package for Laravel, designed to simplify complex data filtering and sorting logic for eloquent queries.
---

Laravel Framework\Crud is an elegant and efficient filtering and sorting package for Laravel, designed to simplify complex data filtering and sorting logic for eloquent queries. By simply adding `filter()` to your Eloquent query, you can add the ability for frontend users to apply filters based on URL query string parameters like a breeze.

Features :
- Livewire support
- Rename and restrict fields
- Various filter methods
- Simple installation and usage
- Filter by relation columns
- Custom filters
- Multi-column sort

Laravel Framework\Crud is not only developer-friendly but also front-end developer-friendly. Frontend developers can effortlessly use filtering and sorting of the APIs by using the popular [JavaScript qs](https://www.npmjs.com/package/qs) package.

The way this package handles filters is inspired by strapi's [filter](https://docs.strapi.io/dev-docs/api/rest/filters-locale-publication#filtering) and [sort](https://docs.strapi.io/dev-docs/api/rest/sort-pagination#sorting) functionality.

## How Does Framework\Crud Work?

Here is a basic usage example to clarify Framework\Crud's use case.

Add `filter()` to your query.

```php
$posts = Post::filter()->get();
```

That's it!
Now you can filter your posts by adding query string parameters to the URL.

```
GET /api/posts?filters[title][$contains]=Framework\Crud
```