# Relation Fields

In this section, we're going to talk about how Framework\Crud can help you handle relation fields.

## Filtering by Relation

First, you need to define the relation in your model:

```php
use Illuminate\Database\Eloquent\Relations\HasMany;

class Post extends Model
{
use Filterable;

    public function tags(): HasMany
    {
        return $this->hasMany(Tag::class);
    }
}
```

Note that Framework\Crud read available fields from related models, you'll want to edit the `$filterFields` property in the related model. read more about `$filterFields` in [allowed fields](broken-reference) section.

```php
class Tags extends Model
{
use Filterable;

    protected $filterFields = [
        'title',
    ];
}
```

Now you're all set to apply relation filtering! You can find some examples on the filter examples page.

## Sorting by Relation

Framework\Crud supports sorting by the following relationship types:

* One to One
* One to Many
* Many to Many

Just like with filtering, you'll need to define the return type of the relations in your model:

```php
use Illuminate\Database\Eloquent\Relations\HasMany;

class Post extends Model
{
use Sortable;

    public function tags(): HasMany
    {
        return $this->hasMany(Tag::class);
    }
}
```

Check out the sort examples page for some examples.
