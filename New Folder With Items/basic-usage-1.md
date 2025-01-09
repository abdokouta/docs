# Basic Usage

## Filters

Add `Filterable` trait to your model to get filters functionalities.

```php
use Maginium\Framework\Crud\Traits\Filterable;

class Post extends Model
{
    use Filterable;
    
    //
}
```

Now add `filter()` to your model eloquent query in the controller.

```php
use App\Models\Post;

class PostController extends Controller
{
    public function index()
    {
        return Post::filter()->get();
    }
}
```

By default, it gives access to all filters available. here is the list of [available filters](broken-reference). if you want to explicitly specify which filters to use in this call head to restrict filters section.

## Sort

### Sort Basics

Add `Sortable` trait to your model to get sorts functionalities.

```php
use Maginium\Framework\Crud\Traits\Sortable;

class Post extends Model
{
    use Sortable;
    
    //
}
```

Now add `sort()` to your eloquent query in the controller.

```php
use App\Models\Post;

class PostController extends Controller
{
    public function index()
    {
        return Post::sort()->get();
    }
}
```

Now sort can be applied as instructed in [apply sort](broken-reference).
