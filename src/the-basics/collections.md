# 🗂️ Collections

- [Introduction](collections.md#introduction)
  - [Creating Collections](collections.md#creating-collections)
  - [Extending Collections](collections.md#extending-collections)
- [Available Methods](collections.md#available-methods)
- [Higher Order Messages](collections.md#higher-order-messages)
- [Lazy Collections](collections.md#lazy-collections)
  - [Introduction](collections.md#lazy-collection-introduction)
  - [Creating Lazy Collections](collections.md#creating-lazy-collections)
  - [The Enumerable Contract](collections.md#the-enumerable-contract)
  - [Lazy Collection Methods](collections.md#lazy-collection-methods)

## [Introduction](collections.md#introduction) <a href="#introduction" id="introduction"></a>

The `Maginium\Framework\Support\Collection` class provides a fluent, convenient wrapper for working with arrays of data. For example, check out the
following code. We'll use the `collect` helper to create a new collection instance from the array, run the `strtoupper` function on each element, and
then remove all empty elements:

```
$collection = collect(['taylor', 'abigail', null])->map(function (?string $name) {
    return strtoupper($name);
})->reject(function (string $name) {
    return empty($name);
});
```

As you can see, the `Collection` class allows you to chain its methods to perform fluent mapping and reducing of the underlying array. In general,
collections are immutable, meaning every `Collection` method returns an entirely new `Collection` instance.

### [Creating Collections](collections.md#creating-collections) <a href="#creating-collections" id="creating-collections"></a>

As mentioned above, the `collect` helper returns a new `Maginium\Framework\Support\Collection` instance for the given array. So, creating a collection
is as simple as:

```
$collection = collect([1, 2, 3]);
```

{% hint style="info" %} The results of [Eloquent](../docs/%7B%7Bversion%7D%7D/eloquent/) queries are always returned as `Collection` instances.
{% endhint %}

### [Extending Collections](collections.md#extending-collections) <a href="#extending-collections" id="extending-collections"></a>

Collections are "macroable", which allows you to add additional methods to the `Collection` class at run time. The
`Maginium\Framework\Support\Collection` class' `macro` method accepts a closure that will be executed when your macro is called. The macro closure may
access the collection's other methods via `$this`, just as if it were a real method of the collection class. For example, the following code adds a
`toUpper` method to the `Collection` class:

```
use Maginium\Framework\Support\Collection;
use Maginium\Framework\Support\Str;

Collection::macro('toUpper', function () {
    return $this->map(function (string $value) {
        return Str::upper($value);
    });
});

$collection = collect(['first', 'second']);

$upper = $collection->toUpper();

// ['FIRST', 'SECOND']
```

Typically, you should declare collection macros in the `boot` method of a [service provider](../docs/%7B%7Bversion%7D%7D/providers/).

#### [Macro Arguments](collections.md#macro-arguments) <a href="#macro-arguments" id="macro-arguments"></a>

If necessary, you may define macros that accept additional arguments:

```
use Maginium\Framework\Support\Collection;
use Maginium\Framework\Support\Facades\Lang;

Collection::macro('toLocale', function (string $locale) {
    return $this->map(function (string $value) use ($locale) {
        return Lang::get($value, [], $locale);
    });
});

$collection = collect(['first', 'second']);

$translated = $collection->toLocale('es');
```

## [Available Methods](collections.md#available-methods) <a href="#available-methods" id="available-methods"></a>

For the majority of the remaining collection documentation, we'll discuss each method available in the `Collection` class. Remember, all of these
methods may be chained to fluently manipulate the underlying array. Furthermore, almost every method returns a new `Collection` instance, allowing you
to preserve the original copy of the collection when necessary:

| [after](collections.md#method-after)                       | [all](collections.md#method-all)                       | [average](collections.md#method-average)                   |
| ---------------------------------------------------------- | ------------------------------------------------------ | ---------------------------------------------------------- |
| [avg](collections.md#method-avg)                           | [before](collections.md#method-before)                 | [chunk](collections.md#method-chunk)                       |
| [chunkWhile](collections.md#method-chunkwhile)             | [collapse](collections.md#method-collapse)             | [collect](collections.md#method-collect)                   |
| [combine](collections.md#method-combine)                   | [concat](collections.md#method-concat)                 | [contains](collections.md#method-contains)                 |
| [containsOneItem](collections.md#method-containsoneitem)   | [containsStrict](collections.md#method-containsstrict) | [count](collections.md#method-count)                       |
| [countBy](collections.md#method-countBy)                   | [crossJoin](collections.md#method-crossjoin)           | [dd](collections.md#method-dd)                             |
| [diff](collections.md#method-diff)                         | [diffAssoc](collections.md#method-diffassoc)           | [diffAssocUsing](collections.md#method-diffassocusing)     |
| [diffKeys](collections.md#method-diffkeys)                 | [doesntContain](collections.md#method-doesntcontain)   | [dot](collections.md#method-dot)                           |
| [dump](collections.md#method-dump)                         | [duplicates](collections.md#method-duplicates)         | [duplicatesStrict](collections.md#method-duplicatesstrict) |
| [each](collections.md#method-each)                         | [eachSpread](collections.md#method-eachspread)         | [ensure](collections.md#method-ensure)                     |
| [every](collections.md#method-every)                       | [except](collections.md#method-except)                 | [filter](collections.md#method-filter)                     |
| [first](collections.md#method-first)                       | [firstOrFail](collections.md#method-first-or-fail)     | [firstWhere](collections.md#method-first-where)            |
| [flatMap](collections.md#method-flatmap)                   | [flatten](collections.md#method-flatten)               | [flip](collections.md#method-flip)                         |
| [forget](collections.md#method-forget)                     | [forPage](collections.md#method-forpage)               | [get](collections.md#method-get)                           |
| [groupBy](collections.md#method-groupby)                   | [has](collections.md#method-has)                       | [hasAny](collections.md#method-hasany)                     |
| [implode](collections.md#method-implode)                   | [intersect](collections.md#method-intersect)           | [intersectAssoc](collections.md#method-intersectAssoc)     |
| [intersectByKeys](collections.md#method-intersectbykeys)   | [isEmpty](collections.md#method-isempty)               | [isNotEmpty](collections.md#method-isnotempty)             |
| [join](collections.md#method-join)                         | [keyBy](collections.md#method-keyby)                   | [keys](collections.md#method-keys)                         |
| [last](collections.md#method-last)                         | [lazy](collections.md#method-lazy)                     | [macro](collections.md#method-macro)                       |
| [make](collections.md#method-make)                         | [map](collections.md#method-map)                       | [mapInto](collections.md#method-mapinto)                   |
| [mapSpread](collections.md#method-mapspread)               | [mapToGroups](collections.md#method-maptogroups)       | [mapWithKeys](collections.md#method-mapwithkeys)           |
| [max](collections.md#method-max)                           | [median](collections.md#method-median)                 | [merge](collections.md#method-merge)                       |
| [mergeRecursive](collections.md#method-mergerecursive)     | [min](collections.md#method-min)                       | [mode](collections.md#method-mode)                         |
| [multiply](collections.md#method-multiply)                 | [nth](collections.md#method-nth)                       | [only](collections.md#method-only)                         |
| [pad](collections.md#method-pad)                           | [partition](collections.md#method-partition)           | [percentage](collections.md#method-percentage)             |
| [pipe](collections.md#method-pipe)                         | [pipeInto](collections.md#method-pipeinto)             | [pipeThrough](collections.md#method-pipethrough)           |
| [pluck](collections.md#method-pluck)                       | [pop](collections.md#method-pop)                       | [prepend](collections.md#method-prepend)                   |
| [pull](collections.md#method-pull)                         | [push](collections.md#method-push)                     | [put](collections.md#method-put)                           |
| [random](collections.md#method-random)                     | [range](collections.md#method-range)                   | [reduce](collections.md#method-reduce)                     |
| [reduceSpread](collections.md#method-reduce-spread)        | [reject](collections.md#method-reject)                 | [replace](collections.md#method-replace)                   |
| [replaceRecursive](collections.md#method-replacerecursive) | [reverse](collections.md#method-reverse)               | [search](collections.md#method-search)                     |
| [select](collections.md#method-select)                     | [shift](collections.md#method-shift)                   | [shuffle](collections.md#method-shuffle)                   |
| [skip](collections.md#method-skip)                         | [skipUntil](collections.md#method-skipuntil)           | [skipWhile](collections.md#method-skipwhile)               |
| [slice](collections.md#method-slice)                       | [sliding](collections.md#method-sliding)               | [sole](collections.md#method-sole)                         |
| [some](collections.md#method-some)                         | [sort](collections.md#method-sort)                     | [sortBy](collections.md#method-sortby)                     |
| [sortByDesc](collections.md#method-sortbydesc)             | [sortDesc](collections.md#method-sortdesc)             | [sortKeys](collections.md#method-sortkeys)                 |
| [sortKeysDesc](collections.md#method-sortkeysdesc)         | [sortKeysUsing](collections.md#method-sortkeysusing)   | [splice](collections.md#method-splice)                     |
| [split](collections.md#method-split)                       | [splitIn](collections.md#method-splitin)               | [sum](collections.md#method-sum)                           |
| [take](collections.md#method-take)                         | [takeUntil](collections.md#method-takeuntil)           | [takeWhile](collections.md#method-takewhile)               |
| [tap](collections.md#method-tap)                           | [times](collections.md#method-times)                   | [toArray](collections.md#method-toarray)                   |
| [toJson](collections.md#method-tojson)                     | [transform](collections.md#method-transform)           | [undot](collections.md#method-undot)                       |
| [union](collections.md#method-union)                       | [unique](collections.md#method-unique)                 | [uniqueStrict](collections.md#method-uniquestrict)         |
| [unless](collections.md#method-unless)                     | [unlessEmpty](collections.md#method-unlessempty)       | [unlessNotEmpty](collections.md#method-unlessnotempty)     |
| [unwrap](collections.md#method-unwrap)                     | [value](collections.md#method-value)                   | [values](collections.md#method-values)                     |
| [when](collections.md#method-when)                         | [whenEmpty](collections.md#method-whenempty)           | [whenNotEmpty](collections.md#method-whennotempty)         |
| [where](collections.md#method-where)                       | [whereStrict](collections.md#method-wherestrict)       | [whereBetween](collections.md#method-wherebetween)         |
| [whereIn](collections.md#method-wherein)                   | [whereInStrict](collections.md#method-whereinstrict)   | [whereInstanceOf](collections.md#method-whereinstanceof)   |
| [whereNotBetween](collections.md#method-wherenotbetween)   | [whereNotIn](collections.md#method-wherenotin)         | [whereNotInStrict](collections.md#method-wherenotinstrict) |
| [whereNotNull](collections.md#method-wherenotnull)         | [whereNull](collections.md#method-wherenull)           | [wrap](collections.md#method-wrap)                         |
| [zip](collections.md#method-zip)                           |                                                        |                                                            |

## [Method Listing](collections.md#method-listing) <a href="#method-listing" id="method-listing"></a>

#### [`after()` {.collection-method .first-collection-method}](collections.md#method-after) <a href="#method-after" id="method-after"></a>

The `after` method returns the item after the given item. `null` is returned if the given item is not found or is the last item:

```
$collection = collect([1, 2, 3, 4, 5]);

$collection->after(3);

// 4

$collection->after(5);

// null
```

This method searches for the given item using "loose" comparison, meaning a string containing an integer value will be considered equal to an integer
of the same value. To use "strict" comparison, you may provide the `strict` argument to the method:

```
collect([2, 4, 6, 8])->after('4', strict: true);

// null
```

Alternatively, you may provide your own closure to search for the first item that passes a given truth test:

```
collect([2, 4, 6, 8])->after(function (int $item, int $key) {
    return $item > 5;
});

// 8
```

#### [`all()` {.collection-method}](collections.md#method-all) <a href="#method-all" id="method-all"></a>

The `all` method returns the underlying array represented by the collection:

```
collect([1, 2, 3])->all();

// [1, 2, 3]
```

#### [`average()` {.collection-method}](collections.md#method-average) <a href="#method-average" id="method-average"></a>

Alias for the [`avg`](collections.md#method-avg) method.

#### [`avg()` {.collection-method}](collections.md#method-avg) <a href="#method-avg" id="method-avg"></a>

The `avg` method returns the [average value](https://en.wikipedia.org/wiki/Average) of a given key:

```
$average = collect([
    ['foo' => 10],
    ['foo' => 10],
    ['foo' => 20],
    ['foo' => 40]
])->avg('foo');

// 20

$average = collect([1, 1, 2, 4])->avg();

// 2
```

#### [`before()` {.collection-method}](collections.md#method-before) <a href="#method-before" id="method-before"></a>

The `before` method is the opposite of the [`after`](collections.md#method-after) method. It returns the item before the given item. `null` is
returned if the given item is not found or is the first item:

```
$collection = collect([1, 2, 3, 4, 5]);

$collection->before(3);

// 2

$collection->before(1);

// null

collect([2, 4, 6, 8])->before('4', strict: true);

// null

collect([2, 4, 6, 8])->before(function (int $item, int $key) {
    return $item > 5;
});

// 4
```

#### [`chunk()` {.collection-method}](collections.md#method-chunk) <a href="#method-chunk" id="method-chunk"></a>

The `chunk` method breaks the collection into multiple, smaller collections of a given size:

```
$collection = collect([1, 2, 3, 4, 5, 6, 7]);

$chunks = $collection->chunk(4);

$chunks->all();

// [[1, 2, 3, 4], [5, 6, 7]]
```

This method is especially useful in [views](../docs/%7B%7Bversion%7D%7D/views/) when working with a grid system such as
[Bootstrap](https://getbootstrap.com/docs/5.3/layout/grid/). For example, imagine you have a collection of
[Eloquent](../docs/%7B%7Bversion%7D%7D/eloquent/) models you want to display in a grid:

```blade
@foreach ($products->chunk(3) as $chunk)
    <div class="row">
        @foreach ($chunk as $product)
            <div class="col-xs-4">{{ $product->name }}</div>
        @endforeach
    </div>
@endforeach
```

#### [`chunkWhile()` {.collection-method}](collections.md#method-chunkwhile) <a href="#method-chunkwhile" id="method-chunkwhile"></a>

The `chunkWhile` method breaks the collection into multiple, smaller collections based on the evaluation of the given callback. The `$chunk` variable
passed to the closure may be used to inspect the previous element:

```
$collection = collect(str_split('AABBCCCD'));

$chunks = $collection->chunkWhile(function (string $value, int $key, Collection $chunk) {
    return $value === $chunk->last();
});

$chunks->all();

// [['A', 'A'], ['B', 'B'], ['C', 'C', 'C'], ['D']]
```

#### [`collapse()` {.collection-method}](collections.md#method-collapse) <a href="#method-collapse" id="method-collapse"></a>

The `collapse` method collapses a collection of arrays into a single, flat collection:

```
$collection = collect([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]);

$collapsed = $collection->collapse();

$collapsed->all();

// [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

#### [`collect()` {.collection-method}](collections.md#method-collect) <a href="#method-collect" id="method-collect"></a>

The `collect` method returns a new `Collection` instance with the items currently in the collection:

```
$collectionA = collect([1, 2, 3]);

$collectionB = $collectionA->collect();

$collectionB->all();

// [1, 2, 3]
```

The `collect` method is primarily useful for converting [lazy collections](collections.md#lazy-collections) into standard `Collection` instances:

```
$lazyCollection = LazyCollection::make(function () {
    yield 1;
    yield 2;
    yield 3;
});

$collection = $lazyCollection->collect();

$collection::class;

// 'Maginium\Framework\Support\Collection'

$collection->all();

// [1, 2, 3]
```

{% hint style="info" %} The `collect` method is especially useful when you have an instance of `Enumerable` and need a non-lazy collection instance.
Since `collect()` is part of the `Enumerable` contract, you can safely use it to get a `Collection` instance. {% endhint %}

#### [`combine()` {.collection-method}](collections.md#method-combine) <a href="#method-combine" id="method-combine"></a>

The `combine` method combines the values of the collection, as keys, with the values of another array or collection:

```
$collection = collect(['name', 'age']);

$combined = $collection->combine(['George', 29]);

$combined->all();

// ['name' => 'George', 'age' => 29]
```

#### [`concat()` {.collection-method}](collections.md#method-concat) <a href="#method-concat" id="method-concat"></a>

The `concat` method appends the given `array` or collection's values onto the end of another collection:

```
$collection = collect(['John Doe']);

$concatenated = $collection->concat(['Jane Doe'])->concat(['name' => 'Johnny Doe']);

$concatenated->all();

// ['John Doe', 'Jane Doe', 'Johnny Doe']
```

The `concat` method numerically reindexes keys for items concatenated onto the original collection. To maintain keys in associative collections, see
the [merge](collections.md#method-merge) method.

#### [`contains()` {.collection-method}](collections.md#method-contains) <a href="#method-contains" id="method-contains"></a>

The `contains` method determines whether the collection contains a given item. You may pass a closure to the `contains` method to determine if an
element exists in the collection matching a given truth test:

```
$collection = collect([1, 2, 3, 4, 5]);

$collection->contains(function (int $value, int $key) {
    return $value > 5;
});

// false
```

Alternatively, you may pass a string to the `contains` method to determine whether the collection contains a given item value:

```
$collection = collect(['name' => 'Desk', 'price' => 100]);

$collection->contains('Desk');

// true

$collection->contains('New York');

// false
```

You may also pass a key / value pair to the `contains` method, which will determine if the given pair exists in the collection:

```
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
]);

$collection->contains('product', 'Bookcase');

// false
```

The `contains` method uses "loose" comparisons when checking item values, meaning a string with an integer value will be considered equal to an
integer of the same value. Use the [`containsStrict`](collections.md#method-containsstrict) method to filter using "strict" comparisons.

For the inverse of `contains`, see the [doesntContain](collections.md#method-doesntcontain) method.

#### [`containsOneItem()` {.collection-method}](collections.md#method-containsoneitem) <a href="#method-containsoneitem" id="method-containsoneitem"></a>

The `containsOneItem` method determines whether the collection contains a single item:

```
collect([])->containsOneItem();

// false

collect(['1'])->containsOneItem();

// true

collect(['1', '2'])->containsOneItem();

// false
```

#### [`containsStrict()` {.collection-method}](collections.md#method-containsstrict) <a href="#method-containsstrict" id="method-containsstrict"></a>

This method has the same signature as the [`contains`](collections.md#method-contains) method; however, all values are compared using "strict"
comparisons.

{% hint style="info" %} This method's behavior is modified when using
[Eloquent Collections](../docs/%7B%7Bversion%7D%7D/eloquent-collections/#method-contains). {% endhint %}

#### [`count()` {.collection-method}](collections.md#method-count) <a href="#method-count" id="method-count"></a>

The `count` method returns the total number of items in the collection:

```
$collection = collect([1, 2, 3, 4]);

$collection->count();

// 4
```

#### [`countBy()` {.collection-method}](collections.md#method-countBy) <a href="#method-countby" id="method-countby"></a>

The `countBy` method counts the occurrences of values in the collection. By default, the method counts the occurrences of every element, allowing you
to count certain "types" of elements in the collection:

```
$collection = collect([1, 2, 2, 2, 3]);

$counted = $collection->countBy();

$counted->all();

// [1 => 1, 2 => 3, 3 => 1]
```

You pass a closure to the `countBy` method to count all items by a custom value:

```
$collection = collect(['alice@gmail.com', 'bob@yahoo.com', 'carlos@gmail.com']);

$counted = $collection->countBy(function (string $email) {
    return substr(strrchr($email, "@"), 1);
});

$counted->all();

// ['gmail.com' => 2, 'yahoo.com' => 1]
```

#### [`crossJoin()` {.collection-method}](collections.md#method-crossjoin) <a href="#method-crossjoin" id="method-crossjoin"></a>

The `crossJoin` method cross joins the collection's values among the given arrays or collections, returning a Cartesian product with all possible
permutations:

```
$collection = collect([1, 2]);

$matrix = $collection->crossJoin(['a', 'b']);

$matrix->all();

/*
    [
        [1, 'a'],
        [1, 'b'],
        [2, 'a'],
        [2, 'b'],
    ]
*/

$collection = collect([1, 2]);

$matrix = $collection->crossJoin(['a', 'b'], ['I', 'II']);

$matrix->all();

/*
    [
        [1, 'a', 'I'],
        [1, 'a', 'II'],
        [1, 'b', 'I'],
        [1, 'b', 'II'],
        [2, 'a', 'I'],
        [2, 'a', 'II'],
        [2, 'b', 'I'],
        [2, 'b', 'II'],
    ]
*/
```

#### [`dd()` {.collection-method}](collections.md#method-dd) <a href="#method-dd" id="method-dd"></a>

The `dd` method dumps the collection's items and ends execution of the script:

```
$collection = collect(['John Doe', 'Jane Doe']);

$collection->dd();

/*
    Collection {
        #items: array:2 [
            0 => "John Doe"
            1 => "Jane Doe"
        ]
    }
*/
```

If you do not want to stop executing the script, use the [`dump`](collections.md#method-dump) method instead.

#### [`diff()` {.collection-method}](collections.md#method-diff) <a href="#method-diff" id="method-diff"></a>

The `diff` method compares the collection against another collection or a plain PHP `array` based on its values. This method will return the values in
the original collection that are not present in the given collection:

```
$collection = collect([1, 2, 3, 4, 5]);

$diff = $collection->diff([2, 4, 6, 8]);

$diff->all();

// [1, 3, 5]
```

{% hint style="info" %} This method's behavior is modified when using
[Eloquent Collections](../docs/%7B%7Bversion%7D%7D/eloquent-collections/#method-diff). {% endhint %}

#### [`diffAssoc()` {.collection-method}](collections.md#method-diffassoc) <a href="#method-diffassoc" id="method-diffassoc"></a>

The `diffAssoc` method compares the collection against another collection or a plain PHP `array` based on its keys and values. This method will return
the key / value pairs in the original collection that are not present in the given collection:

```
$collection = collect([
    'color' => 'orange',
    'type' => 'fruit',
    'remain' => 6,
]);

$diff = $collection->diffAssoc([
    'color' => 'yellow',
    'type' => 'fruit',
    'remain' => 3,
    'used' => 6,
]);

$diff->all();

// ['color' => 'orange', 'remain' => 6]
```

#### [`diffAssocUsing()` {.collection-method}](collections.md#method-diffassocusing) <a href="#method-diffassocusing" id="method-diffassocusing"></a>

Unlike `diffAssoc`, `diffAssocUsing` accepts a user supplied callback function for the indices comparison:

```
$collection = collect([
    'color' => 'orange',
    'type' => 'fruit',
    'remain' => 6,
]);

$diff = $collection->diffAssocUsing([
    'Color' => 'yellow',
    'Type' => 'fruit',
    'Remain' => 3,
], 'strnatcasecmp');

$diff->all();

// ['color' => 'orange', 'remain' => 6]
```

The callback must be a comparison function that returns an integer less than, equal to, or greater than zero. For more information, refer to the PHP
documentation on [`array_diff_uassoc`](https://www.php.net/array_diff_uassoc#refsect1-function.array-diff-uassoc-parameters), which is the PHP
function that the `diffAssocUsing` method utilizes internally.

#### [`diffKeys()` {.collection-method}](collections.md#method-diffkeys) <a href="#method-diffkeys" id="method-diffkeys"></a>

The `diffKeys` method compares the collection against another collection or a plain PHP `array` based on its keys. This method will return the key /
value pairs in the original collection that are not present in the given collection:

```
$collection = collect([
    'one' => 10,
    'two' => 20,
    'three' => 30,
    'four' => 40,
    'five' => 50,
]);

$diff = $collection->diffKeys([
    'two' => 2,
    'four' => 4,
    'six' => 6,
    'eight' => 8,
]);

$diff->all();

// ['one' => 10, 'three' => 30, 'five' => 50]
```

#### [`doesntContain()` {.collection-method}](collections.md#method-doesntcontain) <a href="#method-doesntcontain" id="method-doesntcontain"></a>

The `doesntContain` method determines whether the collection does not contain a given item. You may pass a closure to the `doesntContain` method to
determine if an element does not exist in the collection matching a given truth test:

```
$collection = collect([1, 2, 3, 4, 5]);

$collection->doesntContain(function (int $value, int $key) {
    return $value < 5;
});

// false
```

Alternatively, you may pass a string to the `doesntContain` method to determine whether the collection does not contain a given item value:

```
$collection = collect(['name' => 'Desk', 'price' => 100]);

$collection->doesntContain('Table');

// true

$collection->doesntContain('Desk');

// false
```

You may also pass a key / value pair to the `doesntContain` method, which will determine if the given pair does not exist in the collection:

```
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
]);

$collection->doesntContain('product', 'Bookcase');

// true
```

The `doesntContain` method uses "loose" comparisons when checking item values, meaning a string with an integer value will be considered equal to an
integer of the same value.

#### [`dot()` {.collection-method}](collections.md#method-dot) <a href="#method-dot" id="method-dot"></a>

The `dot` method flattens a multi-dimensional collection into a single level collection that uses "dot" notation to indicate depth:

```
$collection = collect(['products' => ['desk' => ['price' => 100]]]);

$flattened = $collection->dot();

$flattened->all();

// ['products.desk.price' => 100]
```

#### [`dump()` {.collection-method}](collections.md#method-dump) <a href="#method-dump" id="method-dump"></a>

The `dump` method dumps the collection's items:

```
$collection = collect(['John Doe', 'Jane Doe']);

$collection->dump();

/*
    Collection {
        #items: array:2 [
            0 => "John Doe"
            1 => "Jane Doe"
        ]
    }
*/
```

If you want to stop executing the script after dumping the collection, use the [`dd`](collections.md#method-dd) method instead.

#### [`duplicates()` {.collection-method}](collections.md#method-duplicates) <a href="#method-duplicates" id="method-duplicates"></a>

The `duplicates` method retrieves and returns duplicate values from the collection:

```
$collection = collect(['a', 'b', 'a', 'c', 'b']);

$collection->duplicates();

// [2 => 'a', 4 => 'b']
```

If the collection contains arrays or objects, you can pass the key of the attributes that you wish to check for duplicate values:

```
$employees = collect([
    ['email' => 'abigail@example.com', 'position' => 'Developer'],
    ['email' => 'james@example.com', 'position' => 'Designer'],
    ['email' => 'victoria@example.com', 'position' => 'Developer'],
]);

$employees->duplicates('position');

// [2 => 'Developer']
```

#### [`duplicatesStrict()` {.collection-method}](collections.md#method-duplicatesstrict) <a href="#method-duplicatesstrict" id="method-duplicatesstrict"></a>

This method has the same signature as the [`duplicates`](collections.md#method-duplicates) method; however, all values are compared using "strict"
comparisons.

#### [`each()` {.collection-method}](collections.md#method-each) <a href="#method-each" id="method-each"></a>

The `each` method iterates over the items in the collection and passes each item to a closure:

```
$collection = collect([1, 2, 3, 4]);

$collection->each(function (int $item, int $key) {
    // ...
});
```

If you would like to stop iterating through the items, you may return `false` from your closure:

```
$collection->each(function (int $item, int $key) {
    if (/* condition */) {
        return false;
    }
});
```

#### [`eachSpread()` {.collection-method}](collections.md#method-eachspread) <a href="#method-eachspread" id="method-eachspread"></a>

The `eachSpread` method iterates over the collection's items, passing each nested item value into the given callback:

```
$collection = collect([['John Doe', 35], ['Jane Doe', 33]]);

$collection->eachSpread(function (string $name, int $age) {
    // ...
});
```

You may stop iterating through the items by returning `false` from the callback:

```
$collection->eachSpread(function (string $name, int $age) {
    return false;
});
```

#### [`ensure()` {.collection-method}](collections.md#method-ensure) <a href="#method-ensure" id="method-ensure"></a>

The `ensure` method may be used to verify that all elements of a collection are of a given type or list of types. Otherwise, an
`UnexpectedValueException` will be thrown:

```
return $collection->ensure(User::class);

return $collection->ensure([User::class, Customer::class]);
```

Primitive types such as `string`, `int`, `float`, `bool`, and `array` may also be specified:

```
return $collection->ensure('int');
```

> \[!WARNING]\
> The `ensure` method does not guarantee that elements of different types will not be added to the collection at a later time.

#### [`every()` {.collection-method}](collections.md#method-every) <a href="#method-every" id="method-every"></a>

The `every` method may be used to verify that all elements of a collection pass a given truth test:

```
collect([1, 2, 3, 4])->every(function (int $value, int $key) {
    return $value > 2;
});

// false
```

If the collection is empty, the `every` method will return true:

```
$collection = collect([]);

$collection->every(function (int $value, int $key) {
    return $value > 2;
});

// true
```

#### [`except()` {.collection-method}](collections.md#method-except) <a href="#method-except" id="method-except"></a>

The `except` method returns all items in the collection except for those with the specified keys:

```
$collection = collect(['product_id' => 1, 'price' => 100, 'discount' => false]);

$filtered = $collection->except(['price', 'discount']);

$filtered->all();

// ['product_id' => 1]
```

For the inverse of `except`, see the [only](collections.md#method-only) method.

{% hint style="info" %} This method's behavior is modified when using
[Eloquent Collections](../docs/%7B%7Bversion%7D%7D/eloquent-collections/#method-except). {% endhint %}

#### [`filter()` {.collection-method}](collections.md#method-filter) <a href="#method-filter" id="method-filter"></a>

The `filter` method filters the collection using the given callback, keeping only those items that pass a given truth test:

```
$collection = collect([1, 2, 3, 4]);

$filtered = $collection->filter(function (int $value, int $key) {
    return $value > 2;
});

$filtered->all();

// [3, 4]
```

If no callback is supplied, all entries of the collection that are equivalent to `false` will be removed:

```
$collection = collect([1, 2, 3, null, false, '', 0, []]);

$collection->filter()->all();

// [1, 2, 3]
```

For the inverse of `filter`, see the [reject](collections.md#method-reject) method.

#### [`first()` {.collection-method}](collections.md#method-first) <a href="#method-first" id="method-first"></a>

The `first` method returns the first element in the collection that passes a given truth test:

```
collect([1, 2, 3, 4])->first(function (int $value, int $key) {
    return $value > 2;
});

// 3
```

You may also call the `first` method with no arguments to get the first element in the collection. If the collection is empty, `null` is returned:

```
collect([1, 2, 3, 4])->first();

// 1
```

#### [`firstOrFail()` {.collection-method}](collections.md#method-first-or-fail) <a href="#method-first-or-fail" id="method-first-or-fail"></a>

The `firstOrFail` method is identical to the `first` method; however, if no result is found, an `Maginium\Framework\Support\ItemNotFoundException`
exception will be thrown:

```
collect([1, 2, 3, 4])->firstOrFail(function (int $value, int $key) {
    return $value > 5;
});

// Throws ItemNotFoundException...
```

You may also call the `firstOrFail` method with no arguments to get the first element in the collection. If the collection is empty, an
`Maginium\Framework\Support\ItemNotFoundException` exception will be thrown:

```
collect([])->firstOrFail();

// Throws ItemNotFoundException...
```

#### [`firstWhere()` {.collection-method}](collections.md#method-first-where) <a href="#method-first-where" id="method-first-where"></a>

The `firstWhere` method returns the first element in the collection with the given key / value pair:

```
$collection = collect([
    ['name' => 'Regena', 'age' => null],
    ['name' => 'Linda', 'age' => 14],
    ['name' => 'Diego', 'age' => 23],
    ['name' => 'Linda', 'age' => 84],
]);

$collection->firstWhere('name', 'Linda');

// ['name' => 'Linda', 'age' => 14]
```

You may also call the `firstWhere` method with a comparison operator:

```
$collection->firstWhere('age', '>=', 18);

// ['name' => 'Diego', 'age' => 23]
```

Like the [where](collections.md#method-where) method, you may pass one argument to the `firstWhere` method. In this scenario, the `firstWhere` method
will return the first item where the given item key's value is "truthy":

```
$collection->firstWhere('age');

// ['name' => 'Linda', 'age' => 14]
```

#### [`flatMap()` {.collection-method}](collections.md#method-flatmap) <a href="#method-flatmap" id="method-flatmap"></a>

The `flatMap` method iterates through the collection and passes each value to the given closure. The closure is free to modify the item and return it,
thus forming a new collection of modified items. Then, the array is flattened by one level:

```
$collection = collect([
    ['name' => 'Sally'],
    ['school' => 'Arkansas'],
    ['age' => 28]
]);

$flattened = $collection->flatMap(function (array $values) {
    return array_map('strtoupper', $values);
});

$flattened->all();

// ['name' => 'SALLY', 'school' => 'ARKANSAS', 'age' => '28'];
```

#### [`flatten()` {.collection-method}](collections.md#method-flatten) <a href="#method-flatten" id="method-flatten"></a>

The `flatten` method flattens a multi-dimensional collection into a single dimension:

```
$collection = collect([
    'name' => 'taylor',
    'languages' => [
        'php', 'javascript'
    ]
]);

$flattened = $collection->flatten();

$flattened->all();

// ['taylor', 'php', 'javascript'];
```

If necessary, you may pass the `flatten` method a "depth" argument:

```
$collection = collect([
    'Apple' => [
        [
            'name' => 'iPhone 6S',
            'brand' => 'Apple'
        ],
    ],
    'Samsung' => [
        [
            'name' => 'Galaxy S7',
            'brand' => 'Samsung'
        ],
    ],
]);

$products = $collection->flatten(1);

$products->values()->all();

/*
    [
        ['name' => 'iPhone 6S', 'brand' => 'Apple'],
        ['name' => 'Galaxy S7', 'brand' => 'Samsung'],
    ]
*/
```

In this example, calling `flatten` without providing the depth would have also flattened the nested arrays, resulting in
`['iPhone 6S', 'Apple', 'Galaxy S7', 'Samsung']`. Providing a depth allows you to specify the number of levels nested arrays will be flattened.

#### [`flip()` {.collection-method}](collections.md#method-flip) <a href="#method-flip" id="method-flip"></a>

The `flip` method swaps the collection's keys with their corresponding values:

```
$collection = collect(['name' => 'taylor', 'framework' => 'maginium']);

$flipped = $collection->flip();

$flipped->all();

// ['taylor' => 'name', 'maginium' => 'framework']
```

#### [`forget()` {.collection-method}](collections.md#method-forget) <a href="#method-forget" id="method-forget"></a>

The `forget` method removes an item from the collection by its key:

```
$collection = collect(['name' => 'taylor', 'framework' => 'maginium']);

// Forget a single key...
$collection->forget('name');

// ['framework' => 'maginium']

// Forget multiple keys...
$collection->forget(['name', 'framework']);

// []
```

> \[!WARNING]\
> Unlike most other collection methods, `forget` does not return a new modified collection; it modifies and returns the collection it is called on.

#### [`forPage()` {.collection-method}](collections.md#method-forpage) <a href="#method-forpage" id="method-forpage"></a>

The `forPage` method returns a new collection containing the items that would be present on a given page number. The method accepts the page number as
its first argument and the number of items to show per page as its second argument:

```
$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);

$chunk = $collection->forPage(2, 3);

$chunk->all();

// [4, 5, 6]
```

#### [`get()` {.collection-method}](collections.md#method-get) <a href="#method-get" id="method-get"></a>

The `get` method returns the item at a given key. If the key does not exist, `null` is returned:

```
$collection = collect(['name' => 'taylor', 'framework' => 'maginium']);

$value = $collection->get('name');

// taylor
```

You may optionally pass a default value as the second argument:

```
$collection = collect(['name' => 'taylor', 'framework' => 'maginium']);

$value = $collection->get('age', 34);

// 34
```

You may even pass a callback as the method's default value. The result of the callback will be returned if the specified key does not exist:

```
$collection->get('email', function () {
    return 'taylor@example.com';
});

// taylor@example.com
```

#### [`groupBy()` {.collection-method}](collections.md#method-groupby) <a href="#method-groupby" id="method-groupby"></a>

The `groupBy` method groups the collection's items by a given key:

```
$collection = collect([
    ['account_id' => 'account-x10', 'product' => 'Chair'],
    ['account_id' => 'account-x10', 'product' => 'Bookcase'],
    ['account_id' => 'account-x11', 'product' => 'Desk'],
]);

$grouped = $collection->groupBy('account_id');

$grouped->all();

/*
    [
        'account-x10' => [
            ['account_id' => 'account-x10', 'product' => 'Chair'],
            ['account_id' => 'account-x10', 'product' => 'Bookcase'],
        ],
        'account-x11' => [
            ['account_id' => 'account-x11', 'product' => 'Desk'],
        ],
    ]
*/
```

Instead of passing a string `key`, you may pass a callback. The callback should return the value you wish to key the group by:

```
$grouped = $collection->groupBy(function (array $item, int $key) {
    return substr($item['account_id'], -3);
});

$grouped->all();

/*
    [
        'x10' => [
            ['account_id' => 'account-x10', 'product' => 'Chair'],
            ['account_id' => 'account-x10', 'product' => 'Bookcase'],
        ],
        'x11' => [
            ['account_id' => 'account-x11', 'product' => 'Desk'],
        ],
    ]
*/
```

Multiple grouping criteria may be passed as an array. Each array element will be applied to the corresponding level within a multi-dimensional array:

```
$data = new Collection([
    10 => ['user' => 1, 'skill' => 1, 'roles' => ['Role_1', 'Role_3']],
    20 => ['user' => 2, 'skill' => 1, 'roles' => ['Role_1', 'Role_2']],
    30 => ['user' => 3, 'skill' => 2, 'roles' => ['Role_1']],
    40 => ['user' => 4, 'skill' => 2, 'roles' => ['Role_2']],
]);

$result = $data->groupBy(['skill', function (array $item) {
    return $item['roles'];
}], preserveKeys: true);

/*
[
    1 => [
        'Role_1' => [
            10 => ['user' => 1, 'skill' => 1, 'roles' => ['Role_1', 'Role_3']],
            20 => ['user' => 2, 'skill' => 1, 'roles' => ['Role_1', 'Role_2']],
        ],
        'Role_2' => [
            20 => ['user' => 2, 'skill' => 1, 'roles' => ['Role_1', 'Role_2']],
        ],
        'Role_3' => [
            10 => ['user' => 1, 'skill' => 1, 'roles' => ['Role_1', 'Role_3']],
        ],
    ],
    2 => [
        'Role_1' => [
            30 => ['user' => 3, 'skill' => 2, 'roles' => ['Role_1']],
        ],
        'Role_2' => [
            40 => ['user' => 4, 'skill' => 2, 'roles' => ['Role_2']],
        ],
    ],
];
*/
```

#### [`has()` {.collection-method}](collections.md#method-has) <a href="#method-has" id="method-has"></a>

The `has` method determines if a given key exists in the collection:

```
$collection = collect(['account_id' => 1, 'product' => 'Desk', 'amount' => 5]);

$collection->has('product');

// true

$collection->has(['product', 'amount']);

// true

$collection->has(['amount', 'price']);

// false
```

#### [`hasAny()` {.collection-method}](collections.md#method-hasany) <a href="#method-hasany" id="method-hasany"></a>

The `hasAny` method determines whether any of the given keys exist in the collection:

```
$collection = collect(['account_id' => 1, 'product' => 'Desk', 'amount' => 5]);

$collection->hasAny(['product', 'price']);

// true

$collection->hasAny(['name', 'price']);

// false
```

#### [`implode()` {.collection-method}](collections.md#method-implode) <a href="#method-implode" id="method-implode"></a>

The `implode` method joins items in a collection. Its arguments depend on the type of items in the collection. If the collection contains arrays or
objects, you should pass the key of the attributes you wish to join, and the "glue" string you wish to place between the values:

```
$collection = collect([
    ['account_id' => 1, 'product' => 'Desk'],
    ['account_id' => 2, 'product' => 'Chair'],
]);

$collection->implode('product', ', ');

// Desk, Chair
```

If the collection contains simple strings or numeric values, you should pass the "glue" as the only argument to the method:

```
collect([1, 2, 3, 4, 5])->implode('-');

// '1-2-3-4-5'
```

You may pass a closure to the `implode` method if you would like to format the values being imploded:

```
$collection->implode(function (array $item, int $key) {
    return strtoupper($item['product']);
}, ', ');

// DESK, CHAIR
```

#### [`intersect()` {.collection-method}](collections.md#method-intersect) <a href="#method-intersect" id="method-intersect"></a>

The `intersect` method removes any values from the original collection that are not present in the given `array` or collection. The resulting
collection will preserve the original collection's keys:

```
$collection = collect(['Desk', 'Sofa', 'Chair']);

$intersect = $collection->intersect(['Desk', 'Chair', 'Bookcase']);

$intersect->all();

// [0 => 'Desk', 2 => 'Chair']
```

{% hint style="info" %} This method's behavior is modified when using
[Eloquent Collections](../docs/%7B%7Bversion%7D%7D/eloquent-collections/#method-intersect). {% endhint %}

#### [`intersectAssoc()` {.collection-method}](collections.md#method-intersectAssoc) <a href="#method-intersectassoc" id="method-intersectassoc"></a>

The `intersectAssoc` method compares the original collection against another collection or `array`, returning the key / value pairs that are present
in all of the given collections:

```
$collection = collect([
    'color' => 'red',
    'size' => 'M',
    'material' => 'cotton'
]);

$intersect = $collection->intersectAssoc([
    'color' => 'blue',
    'size' => 'M',
    'material' => 'polyester'
]);

$intersect->all();

// ['size' => 'M']
```

#### [`intersectByKeys()` {.collection-method}](collections.md#method-intersectbykeys) <a href="#method-intersectbykeys" id="method-intersectbykeys"></a>

The `intersectByKeys` method removes any keys and their corresponding values from the original collection that are not present in the given `array` or
collection:

```
$collection = collect([
    'serial' => 'UX301', 'type' => 'screen', 'year' => 2009,
]);

$intersect = $collection->intersectByKeys([
    'reference' => 'UX404', 'type' => 'tab', 'year' => 2011,
]);

$intersect->all();

// ['type' => 'screen', 'year' => 2009]
```

#### [`isEmpty()` {.collection-method}](collections.md#method-isempty) <a href="#method-isempty" id="method-isempty"></a>

The `isEmpty` method returns `true` if the collection is empty; otherwise, `false` is returned:

```
collect([])->isEmpty();

// true
```

#### [`isNotEmpty()` {.collection-method}](collections.md#method-isnotempty) <a href="#method-isnotempty" id="method-isnotempty"></a>

The `isNotEmpty` method returns `true` if the collection is not empty; otherwise, `false` is returned:

```
collect([])->isNotEmpty();

// false
```

#### [`join()` {.collection-method}](collections.md#method-join) <a href="#method-join" id="method-join"></a>

The `join` method joins the collection's values with a string. Using this method's second argument, you may also specify how the final element should
be appended to the string:

```
collect(['a', 'b', 'c'])->join(', '); // 'a, b, c'
collect(['a', 'b', 'c'])->join(', ', ', and '); // 'a, b, and c'
collect(['a', 'b'])->join(', ', ' and '); // 'a and b'
collect(['a'])->join(', ', ' and '); // 'a'
collect([])->join(', ', ' and '); // ''
```

#### [`keyBy()` {.collection-method}](collections.md#method-keyby) <a href="#method-keyby" id="method-keyby"></a>

The `keyBy` method keys the collection by the given key. If multiple items have the same key, only the last one will appear in the new collection:

```
$collection = collect([
    ['product_id' => 'prod-100', 'name' => 'Desk'],
    ['product_id' => 'prod-200', 'name' => 'Chair'],
]);

$keyed = $collection->keyBy('product_id');

$keyed->all();

/*
    [
        'prod-100' => ['product_id' => 'prod-100', 'name' => 'Desk'],
        'prod-200' => ['product_id' => 'prod-200', 'name' => 'Chair'],
    ]
*/
```

You may also pass a callback to the method. The callback should return the value to key the collection by:

```
$keyed = $collection->keyBy(function (array $item, int $key) {
    return strtoupper($item['product_id']);
});

$keyed->all();

/*
    [
        'PROD-100' => ['product_id' => 'prod-100', 'name' => 'Desk'],
        'PROD-200' => ['product_id' => 'prod-200', 'name' => 'Chair'],
    ]
*/
```

#### [`keys()` {.collection-method}](collections.md#method-keys) <a href="#method-keys" id="method-keys"></a>

The `keys` method returns all of the collection's keys:

```
$collection = collect([
    'prod-100' => ['product_id' => 'prod-100', 'name' => 'Desk'],
    'prod-200' => ['product_id' => 'prod-200', 'name' => 'Chair'],
]);

$keys = $collection->keys();

$keys->all();

// ['prod-100', 'prod-200']
```

#### [`last()` {.collection-method}](collections.md#method-last) <a href="#method-last" id="method-last"></a>

The `last` method returns the last element in the collection that passes a given truth test:

```
collect([1, 2, 3, 4])->last(function (int $value, int $key) {
    return $value < 3;
});

// 2
```

You may also call the `last` method with no arguments to get the last element in the collection. If the collection is empty, `null` is returned:

```
collect([1, 2, 3, 4])->last();

// 4
```

#### [`lazy()` {.collection-method}](collections.md#method-lazy) <a href="#method-lazy" id="method-lazy"></a>

The `lazy` method returns a new [`LazyCollection`](collections.md#lazy-collections) instance from the underlying array of items:

```
$lazyCollection = collect([1, 2, 3, 4])->lazy();

$lazyCollection::class;

// Maginium\Framework\Support\LazyCollection

$lazyCollection->all();

// [1, 2, 3, 4]
```

This is especially useful when you need to perform transformations on a huge `Collection` that contains many items:

```
$count = $hugeCollection
    ->lazy()
    ->where('country', 'FR')
    ->where('balance', '>', '100')
    ->count();
```

By converting the collection to a `LazyCollection`, we avoid having to allocate a ton of additional memory. Though the original collection still keeps
_its_ values in memory, the subsequent filters will not. Therefore, virtually no additional memory will be allocated when filtering the collection's
results.

#### [`macro()` {.collection-method}](collections.md#method-macro) <a href="#method-macro" id="method-macro"></a>

The static `macro` method allows you to add methods to the `Collection` class at run time. Refer to the documentation on
[extending collections](collections.md#extending-collections) for more information.

#### [`make()` {.collection-method}](collections.md#method-make) <a href="#method-make" id="method-make"></a>

The static `make` method creates a new collection instance. See the [Creating Collections](collections.md#creating-collections) section.

#### [`map()` {.collection-method}](collections.md#method-map) <a href="#method-map" id="method-map"></a>

The `map` method iterates through the collection and passes each value to the given callback. The callback is free to modify the item and return it,
thus forming a new collection of modified items:

```
$collection = collect([1, 2, 3, 4, 5]);

$multiplied = $collection->map(function (int $item, int $key) {
    return $item * 2;
});

$multiplied->all();

// [2, 4, 6, 8, 10]
```

> \[!WARNING]\
> Like most other collection methods, `map` returns a new collection instance; it does not modify the collection it is called on. If you want to transform
> the original collection, use the [`transform`](collections.md#method-transform) method.

#### [`mapInto()` {.collection-method}](collections.md#method-mapinto) <a href="#method-mapinto" id="method-mapinto"></a>

The `mapInto()` method iterates over the collection, creating a new instance of the given class by passing the value into the constructor:

```
class Currency
{
    /**
     * Create a new currency instance.
     */
    function __construct(
        public string $code,
    ) {}
}

$collection = collect(['USD', 'EUR', 'GBP']);

$currencies = $collection->mapInto(Currency::class);

$currencies->all();

// [Currency('USD'), Currency('EUR'), Currency('GBP')]
```

#### [`mapSpread()` {.collection-method}](collections.md#method-mapspread) <a href="#method-mapspread" id="method-mapspread"></a>

The `mapSpread` method iterates over the collection's items, passing each nested item value into the given closure. The closure is free to modify the
item and return it, thus forming a new collection of modified items:

```
$collection = collect([0, 1, 2, 3, 4, 5, 6, 7, 8, 9]);

$chunks = $collection->chunk(2);

$sequence = $chunks->mapSpread(function (int $even, int $odd) {
    return $even + $odd;
});

$sequence->all();

// [1, 5, 9, 13, 17]
```

#### [`mapToGroups()` {.collection-method}](collections.md#method-maptogroups) <a href="#method-maptogroups" id="method-maptogroups"></a>

The `mapToGroups` method groups the collection's items by the given closure. The closure should return an associative array containing a single key /
value pair, thus forming a new collection of grouped values:

```
$collection = collect([
    [
        'name' => 'John Doe',
        'department' => 'Sales',
    ],
    [
        'name' => 'Jane Doe',
        'department' => 'Sales',
    ],
    [
        'name' => 'Johnny Doe',
        'department' => 'Marketing',
    ]
]);

$grouped = $collection->mapToGroups(function (array $item, int $key) {
    return [$item['department'] => $item['name']];
});

$grouped->all();

/*
    [
        'Sales' => ['John Doe', 'Jane Doe'],
        'Marketing' => ['Johnny Doe'],
    ]
*/

$grouped->get('Sales')->all();

// ['John Doe', 'Jane Doe']
```

#### [`mapWithKeys()` {.collection-method}](collections.md#method-mapwithkeys) <a href="#method-mapwithkeys" id="method-mapwithkeys"></a>

The `mapWithKeys` method iterates through the collection and passes each value to the given callback. The callback should return an associative array
containing a single key / value pair:

```
$collection = collect([
    [
        'name' => 'John',
        'department' => 'Sales',
        'email' => 'john@example.com',
    ],
    [
        'name' => 'Jane',
        'department' => 'Marketing',
        'email' => 'jane@example.com',
    ]
]);

$keyed = $collection->mapWithKeys(function (array $item, int $key) {
    return [$item['email'] => $item['name']];
});

$keyed->all();

/*
    [
        'john@example.com' => 'John',
        'jane@example.com' => 'Jane',
    ]
*/
```

#### [`max()` {.collection-method}](collections.md#method-max) <a href="#method-max" id="method-max"></a>

The `max` method returns the maximum value of a given key:

```
$max = collect([
    ['foo' => 10],
    ['foo' => 20]
])->max('foo');

// 20

$max = collect([1, 2, 3, 4, 5])->max();

// 5
```

#### [`median()` {.collection-method}](collections.md#method-median) <a href="#method-median" id="method-median"></a>

The `median` method returns the [median value](https://en.wikipedia.org/wiki/Median) of a given key:

```
$median = collect([
    ['foo' => 10],
    ['foo' => 10],
    ['foo' => 20],
    ['foo' => 40]
])->median('foo');

// 15

$median = collect([1, 1, 2, 4])->median();

// 1.5
```

#### [`merge()` {.collection-method}](collections.md#method-merge) <a href="#method-merge" id="method-merge"></a>

The `merge` method merges the given array or collection with the original collection. If a string key in the given items matches a string key in the
original collection, the given item's value will overwrite the value in the original collection:

```
$collection = collect(['product_id' => 1, 'price' => 100]);

$merged = $collection->merge(['price' => 200, 'discount' => false]);

$merged->all();

// ['product_id' => 1, 'price' => 200, 'discount' => false]
```

If the given item's keys are numeric, the values will be appended to the end of the collection:

```
$collection = collect(['Desk', 'Chair']);

$merged = $collection->merge(['Bookcase', 'Door']);

$merged->all();

// ['Desk', 'Chair', 'Bookcase', 'Door']
```

#### [`mergeRecursive()` {.collection-method}](collections.md#method-mergerecursive) <a href="#method-mergerecursive" id="method-mergerecursive"></a>

The `mergeRecursive` method merges the given array or collection recursively with the original collection. If a string key in the given items matches
a string key in the original collection, then the values for these keys are merged together into an array, and this is done recursively:

```
$collection = collect(['product_id' => 1, 'price' => 100]);

$merged = $collection->mergeRecursive([
    'product_id' => 2,
    'price' => 200,
    'discount' => false
]);

$merged->all();

// ['product_id' => [1, 2], 'price' => [100, 200], 'discount' => false]
```

#### [`min()` {.collection-method}](collections.md#method-min) <a href="#method-min" id="method-min"></a>

The `min` method returns the minimum value of a given key:

```
$min = collect([['foo' => 10], ['foo' => 20]])->min('foo');

// 10

$min = collect([1, 2, 3, 4, 5])->min();

// 1
```

#### [`mode()` {.collection-method}](collections.md#method-mode) <a href="#method-mode" id="method-mode"></a>

The `mode` method returns the [mode value](<https://en.wikipedia.org/wiki/Mode_(statistics)>) of a given key:

```
$mode = collect([
    ['foo' => 10],
    ['foo' => 10],
    ['foo' => 20],
    ['foo' => 40]
])->mode('foo');

// [10]

$mode = collect([1, 1, 2, 4])->mode();

// [1]

$mode = collect([1, 1, 2, 2])->mode();

// [1, 2]
```

#### [`multiply()` {.collection-method}](collections.md#method-multiply) <a href="#method-multiply" id="method-multiply"></a>

The `multiply` method creates the specified number of copies of all items in the collection:

```php
$users = collect([
    ['name' => 'User #1', 'email' => 'user1@example.com'],
    ['name' => 'User #2', 'email' => 'user2@example.com'],
])->multiply(3);

/*
    [
        ['name' => 'User #1', 'email' => 'user1@example.com'],
        ['name' => 'User #2', 'email' => 'user2@example.com'],
        ['name' => 'User #1', 'email' => 'user1@example.com'],
        ['name' => 'User #2', 'email' => 'user2@example.com'],
        ['name' => 'User #1', 'email' => 'user1@example.com'],
        ['name' => 'User #2', 'email' => 'user2@example.com'],
    ]
*/
```

#### [`nth()` {.collection-method}](collections.md#method-nth) <a href="#method-nth" id="method-nth"></a>

The `nth` method creates a new collection consisting of every n-th element:

```
$collection = collect(['a', 'b', 'c', 'd', 'e', 'f']);

$collection->nth(4);

// ['a', 'e']
```

You may optionally pass a starting offset as the second argument:

```
$collection->nth(4, 1);

// ['b', 'f']
```

#### [`only()` {.collection-method}](collections.md#method-only) <a href="#method-only" id="method-only"></a>

The `only` method returns the items in the collection with the specified keys:

```
$collection = collect([
    'product_id' => 1,
    'name' => 'Desk',
    'price' => 100,
    'discount' => false
]);

$filtered = $collection->only(['product_id', 'name']);

$filtered->all();

// ['product_id' => 1, 'name' => 'Desk']
```

For the inverse of `only`, see the [except](collections.md#method-except) method.

{% hint style="info" %} This method's behavior is modified when using
[Eloquent Collections](../docs/%7B%7Bversion%7D%7D/eloquent-collections/#method-only). {% endhint %}

#### [`pad()` {.collection-method}](collections.md#method-pad) <a href="#method-pad" id="method-pad"></a>

The `pad` method will fill the array with the given value until the array reaches the specified size. This method behaves like the
[array_pad](https://secure.php.net/manual/en/function.array-pad.php) PHP function.

To pad to the left, you should specify a negative size. No padding will take place if the absolute value of the given size is less than or equal to
the length of the array:

```
$collection = collect(['A', 'B', 'C']);

$filtered = $collection->pad(5, 0);

$filtered->all();

// ['A', 'B', 'C', 0, 0]

$filtered = $collection->pad(-5, 0);

$filtered->all();

// [0, 0, 'A', 'B', 'C']
```

#### [`partition()` {.collection-method}](collections.md#method-partition) <a href="#method-partition" id="method-partition"></a>

The `partition` method may be combined with PHP array destructuring to separate elements that pass a given truth test from those that do not:

```
$collection = collect([1, 2, 3, 4, 5, 6]);

[$underThree, $equalOrAboveThree] = $collection->partition(function (int $i) {
    return $i < 3;
});

$underThree->all();

// [1, 2]

$equalOrAboveThree->all();

// [3, 4, 5, 6]
```

#### [`percentage()` {.collection-method}](collections.md#method-percentage) <a href="#method-percentage" id="method-percentage"></a>

The `percentage` method may be used to quickly determine the percentage of items in the collection that pass a given truth test:

```php
$collection = collect([1, 1, 2, 2, 2, 3]);

$percentage = $collection->percentage(fn ($value) => $value === 1);

// 33.33
```

By default, the percentage will be rounded to two decimal places. However, you may customize this behavior by providing a second argument to the
method:

```php
$percentage = $collection->percentage(fn ($value) => $value === 1, precision: 3);

// 33.333
```

#### [`pipe()` {.collection-method}](collections.md#method-pipe) <a href="#method-pipe" id="method-pipe"></a>

The `pipe` method passes the collection to the given closure and returns the result of the executed closure:

```
$collection = collect([1, 2, 3]);

$piped = $collection->pipe(function (Collection $collection) {
    return $collection->sum();
});

// 6
```

#### [`pipeInto()` {.collection-method}](collections.md#method-pipeinto) <a href="#method-pipeinto" id="method-pipeinto"></a>

The `pipeInto` method creates a new instance of the given class and passes the collection into the constructor:

```
class ResourceCollection
{
    /**
     * Create a new ResourceCollection instance.
     */
    public function __construct(
        public Collection $collection,
    ) {}
}

$collection = collect([1, 2, 3]);

$resource = $collection->pipeInto(ResourceCollection::class);

$resource->collection->all();

// [1, 2, 3]
```

#### [`pipeThrough()` {.collection-method}](collections.md#method-pipethrough) <a href="#method-pipethrough" id="method-pipethrough"></a>

The `pipeThrough` method passes the collection to the given array of closures and returns the result of the executed closures:

```
use Maginium\Framework\Support\Collection;

$collection = collect([1, 2, 3]);

$result = $collection->pipeThrough([
    function (Collection $collection) {
        return $collection->merge([4, 5]);
    },
    function (Collection $collection) {
        return $collection->sum();
    },
]);

// 15
```

#### [`pluck()` {.collection-method}](collections.md#method-pluck) <a href="#method-pluck" id="method-pluck"></a>

The `pluck` method retrieves all of the values for a given key:

```
$collection = collect([
    ['product_id' => 'prod-100', 'name' => 'Desk'],
    ['product_id' => 'prod-200', 'name' => 'Chair'],
]);

$plucked = $collection->pluck('name');

$plucked->all();

// ['Desk', 'Chair']
```

You may also specify how you wish the resulting collection to be keyed:

```
$plucked = $collection->pluck('name', 'product_id');

$plucked->all();

// ['prod-100' => 'Desk', 'prod-200' => 'Chair']
```

The `pluck` method also supports retrieving nested values using "dot" notation:

```
$collection = collect([
    [
        'name' => 'Laracon',
        'speakers' => [
            'first_day' => ['Rosa', 'Judith'],
        ],
    ],
    [
        'name' => 'VueConf',
        'speakers' => [
            'first_day' => ['Abigail', 'Joey'],
        ],
    ],
]);

$plucked = $collection->pluck('speakers.first_day');

$plucked->all();

// [['Rosa', 'Judith'], ['Abigail', 'Joey']]
```

If duplicate keys exist, the last matching element will be inserted into the plucked collection:

```
$collection = collect([
    ['brand' => 'Tesla',  'color' => 'red'],
    ['brand' => 'Pagani', 'color' => 'white'],
    ['brand' => 'Tesla',  'color' => 'black'],
    ['brand' => 'Pagani', 'color' => 'orange'],
]);

$plucked = $collection->pluck('color', 'brand');

$plucked->all();

// ['Tesla' => 'black', 'Pagani' => 'orange']
```

#### [`pop()` {.collection-method}](collections.md#method-pop) <a href="#method-pop" id="method-pop"></a>

The `pop` method removes and returns the last item from the collection:

```
$collection = collect([1, 2, 3, 4, 5]);

$collection->pop();

// 5

$collection->all();

// [1, 2, 3, 4]
```

You may pass an integer to the `pop` method to remove and return multiple items from the end of a collection:

```
$collection = collect([1, 2, 3, 4, 5]);

$collection->pop(3);

// collect([5, 4, 3])

$collection->all();

// [1, 2]
```

#### [`prepend()` {.collection-method}](collections.md#method-prepend) <a href="#method-prepend" id="method-prepend"></a>

The `prepend` method adds an item to the beginning of the collection:

```
$collection = collect([1, 2, 3, 4, 5]);

$collection->prepend(0);

$collection->all();

// [0, 1, 2, 3, 4, 5]
```

You may also pass a second argument to specify the key of the prepended item:

```
$collection = collect(['one' => 1, 'two' => 2]);

$collection->prepend(0, 'zero');

$collection->all();

// ['zero' => 0, 'one' => 1, 'two' => 2]
```

#### [`pull()` {.collection-method}](collections.md#method-pull) <a href="#method-pull" id="method-pull"></a>

The `pull` method removes and returns an item from the collection by its key:

```
$collection = collect(['product_id' => 'prod-100', 'name' => 'Desk']);

$collection->pull('name');

// 'Desk'

$collection->all();

// ['product_id' => 'prod-100']
```

#### [`push()` {.collection-method}](collections.md#method-push) <a href="#method-push" id="method-push"></a>

The `push` method appends an item to the end of the collection:

```
$collection = collect([1, 2, 3, 4]);

$collection->push(5);

$collection->all();

// [1, 2, 3, 4, 5]
```

#### [`put()` {.collection-method}](collections.md#method-put) <a href="#method-put" id="method-put"></a>

The `put` method sets the given key and value in the collection:

```
$collection = collect(['product_id' => 1, 'name' => 'Desk']);

$collection->put('price', 100);

$collection->all();

// ['product_id' => 1, 'name' => 'Desk', 'price' => 100]
```

#### [`random()` {.collection-method}](collections.md#method-random) <a href="#method-random" id="method-random"></a>

The `random` method returns a random item from the collection:

```
$collection = collect([1, 2, 3, 4, 5]);

$collection->random();

// 4 - (retrieved randomly)
```

You may pass an integer to `random` to specify how many items you would like to randomly retrieve. A collection of items is always returned when
explicitly passing the number of items you wish to receive:

```
$random = $collection->random(3);

$random->all();

// [2, 4, 5] - (retrieved randomly)
```

If the collection instance has fewer items than requested, the `random` method will throw an `InvalidArgumentException`.

The `random` method also accepts a closure, which will receive the current collection instance:

```
use Maginium\Framework\Support\Collection;

$random = $collection->random(fn (Collection $items) => min(10, count($items)));

$random->all();

// [1, 2, 3, 4, 5] - (retrieved randomly)
```

#### [`range()` {.collection-method}](collections.md#method-range) <a href="#method-range" id="method-range"></a>

The `range` method returns a collection containing integers between the specified range:

```
$collection = collect()->range(3, 6);

$collection->all();

// [3, 4, 5, 6]
```

#### [`reduce()` {.collection-method}](collections.md#method-reduce) <a href="#method-reduce" id="method-reduce"></a>

The `reduce` method reduces the collection to a single value, passing the result of each iteration into the subsequent iteration:

```
$collection = collect([1, 2, 3]);

$total = $collection->reduce(function (?int $carry, int $item) {
    return $carry + $item;
});

// 6
```

The value for `$carry` on the first iteration is `null`; however, you may specify its initial value by passing a second argument to `reduce`:

```
$collection->reduce(function (int $carry, int $item) {
    return $carry + $item;
}, 4);

// 10
```

The `reduce` method also passes array keys in associative collections to the given callback:

```
$collection = collect([
    'usd' => 1400,
    'gbp' => 1200,
    'eur' => 1000,
]);

$ratio = [
    'usd' => 1,
    'gbp' => 1.37,
    'eur' => 1.22,
];

$collection->reduce(function (int $carry, int $value, int $key) use ($ratio) {
    return $carry + ($value * $ratio[$key]);
});

// 4264
```

#### [`reduceSpread()` {.collection-method}](collections.md#method-reduce-spread) <a href="#method-reduce-spread" id="method-reduce-spread"></a>

The `reduceSpread` method reduces the collection to an array of values, passing the results of each iteration into the subsequent iteration. This
method is similar to the `reduce` method; however, it can accept multiple initial values:

```
[$creditsRemaining, $batch] = Image::where('status', 'unprocessed')
    ->get()
    ->reduceSpread(function (int $creditsRemaining, Collection $batch, Image $image) {
        if ($creditsRemaining >= $image->creditsRequired()) {
            $batch->push($image);

            $creditsRemaining -= $image->creditsRequired();
        }

        return [$creditsRemaining, $batch];
    }, $creditsAvailable, collect());
```

#### [`reject()` {.collection-method}](collections.md#method-reject) <a href="#method-reject" id="method-reject"></a>

The `reject` method filters the collection using the given closure. The closure should return `true` if the item should be removed from the resulting
collection:

```
$collection = collect([1, 2, 3, 4]);

$filtered = $collection->reject(function (int $value, int $key) {
    return $value > 2;
});

$filtered->all();

// [1, 2]
```

For the inverse of the `reject` method, see the [`filter`](collections.md#method-filter) method.

#### [`replace()` {.collection-method}](collections.md#method-replace) <a href="#method-replace" id="method-replace"></a>

The `replace` method behaves similarly to `merge`; however, in addition to overwriting matching items that have string keys, the `replace` method will
also overwrite items in the collection that have matching numeric keys:

```
$collection = collect(['Taylor', 'Abigail', 'James']);

$replaced = $collection->replace([1 => 'Victoria', 3 => 'Finn']);

$replaced->all();

// ['Taylor', 'Victoria', 'James', 'Finn']
```

#### [`replaceRecursive()` {.collection-method}](collections.md#method-replacerecursive) <a href="#method-replacerecursive" id="method-replacerecursive"></a>

This method works like `replace`, but it will recur into arrays and apply the same replacement process to the inner values:

```
$collection = collect([
    'Taylor',
    'Abigail',
    [
        'James',
        'Victoria',
        'Finn'
    ]
]);

$replaced = $collection->replaceRecursive([
    'Charlie',
    2 => [1 => 'King']
]);

$replaced->all();

// ['Charlie', 'Abigail', ['James', 'King', 'Finn']]
```

#### [`reverse()` {.collection-method}](collections.md#method-reverse) <a href="#method-reverse" id="method-reverse"></a>

The `reverse` method reverses the order of the collection's items, preserving the original keys:

```
$collection = collect(['a', 'b', 'c', 'd', 'e']);

$reversed = $collection->reverse();

$reversed->all();

/*
    [
        4 => 'e',
        3 => 'd',
        2 => 'c',
        1 => 'b',
        0 => 'a',
    ]
*/
```

#### [`search()` {.collection-method}](collections.md#method-search) <a href="#method-search" id="method-search"></a>

The `search` method searches the collection for the given value and returns its key if found. If the item is not found, `false` is returned:

```
$collection = collect([2, 4, 6, 8]);

$collection->search(4);

// 1
```

The search is done using a "loose" comparison, meaning a string with an integer value will be considered equal to an integer of the same value. To use
"strict" comparison, pass `true` as the second argument to the method:

```
collect([2, 4, 6, 8])->search('4', strict: true);

// false
```

Alternatively, you may provide your own closure to search for the first item that passes a given truth test:

```
collect([2, 4, 6, 8])->search(function (int $item, int $key) {
    return $item > 5;
});

// 2
```

#### [`select()` {.collection-method}](collections.md#method-select) <a href="#method-select" id="method-select"></a>

The `select` method selects the given keys from the collection, similar to an SQL `SELECT` statement:

```php
$users = collect([
    ['name' => 'Taylor Otwell', 'role' => 'Developer', 'status' => 'active'],
    ['name' => 'Victoria Faith', 'role' => 'Researcher', 'status' => 'active'],
]);

$users->select(['name', 'role']);

/*
    [
        ['name' => 'Taylor Otwell', 'role' => 'Developer'],
        ['name' => 'Victoria Faith', 'role' => 'Researcher'],
    ],
*/
```

#### [`shift()` {.collection-method}](collections.md#method-shift) <a href="#method-shift" id="method-shift"></a>

The `shift` method removes and returns the first item from the collection:

```
$collection = collect([1, 2, 3, 4, 5]);

$collection->shift();

// 1

$collection->all();

// [2, 3, 4, 5]
```

You may pass an integer to the `shift` method to remove and return multiple items from the beginning of a collection:

```
$collection = collect([1, 2, 3, 4, 5]);

$collection->shift(3);

// collect([1, 2, 3])

$collection->all();

// [4, 5]
```

#### [`shuffle()` {.collection-method}](collections.md#method-shuffle) <a href="#method-shuffle" id="method-shuffle"></a>

The `shuffle` method randomly shuffles the items in the collection:

```
$collection = collect([1, 2, 3, 4, 5]);

$shuffled = $collection->shuffle();

$shuffled->all();

// [3, 2, 5, 1, 4] - (generated randomly)
```

#### [`skip()` {.collection-method}](collections.md#method-skip) <a href="#method-skip" id="method-skip"></a>

The `skip` method returns a new collection, with the given number of elements removed from the beginning of the collection:

```
$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]);

$collection = $collection->skip(4);

$collection->all();

// [5, 6, 7, 8, 9, 10]
```

#### [`skipUntil()` {.collection-method}](collections.md#method-skipuntil) <a href="#method-skipuntil" id="method-skipuntil"></a>

The `skipUntil` method skips over items from the collection while the given callback returns `false`. Once the callback returns `true` all of the
remaining items in the collection will be returned as a new collection:

```
$collection = collect([1, 2, 3, 4]);

$subset = $collection->skipUntil(function (int $item) {
    return $item >= 3;
});

$subset->all();

// [3, 4]
```

You may also pass a simple value to the `skipUntil` method to skip all items until the given value is found:

```
$collection = collect([1, 2, 3, 4]);

$subset = $collection->skipUntil(3);

$subset->all();

// [3, 4]
```

> \[!WARNING]\
> If the given value is not found or the callback never returns `true`, the `skipUntil` method will return an empty collection.

#### [`skipWhile()` {.collection-method}](collections.md#method-skipwhile) <a href="#method-skipwhile" id="method-skipwhile"></a>

The `skipWhile` method skips over items from the collection while the given callback returns `true`. Once the callback returns `false` all of the
remaining items in the collection will be returned as a new collection:

```
$collection = collect([1, 2, 3, 4]);

$subset = $collection->skipWhile(function (int $item) {
    return $item <= 3;
});

$subset->all();

// [4]
```

> \[!WARNING]\
> If the callback never returns `false`, the `skipWhile` method will return an empty collection.

#### [`slice()` {.collection-method}](collections.md#method-slice) <a href="#method-slice" id="method-slice"></a>

The `slice` method returns a slice of the collection starting at the given index:

```
$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]);

$slice = $collection->slice(4);

$slice->all();

// [5, 6, 7, 8, 9, 10]
```

If you would like to limit the size of the returned slice, pass the desired size as the second argument to the method:

```
$slice = $collection->slice(4, 2);

$slice->all();

// [5, 6]
```

The returned slice will preserve keys by default. If you do not wish to preserve the original keys, you can use the
[`values`](collections.md#method-values) method to reindex them.

#### [`sliding()` {.collection-method}](collections.md#method-sliding) <a href="#method-sliding" id="method-sliding"></a>

The `sliding` method returns a new collection of chunks representing a "sliding window" view of the items in the collection:

```
$collection = collect([1, 2, 3, 4, 5]);

$chunks = $collection->sliding(2);

$chunks->toArray();

// [[1, 2], [2, 3], [3, 4], [4, 5]]
```

This is especially useful in conjunction with the [`eachSpread`](collections.md#method-eachspread) method:

```
$transactions->sliding(2)->eachSpread(function (Collection $previous, Collection $current) {
    $current->total = $previous->total + $current->amount;
});
```

You may optionally pass a second "step" value, which determines the distance between the first item of every chunk:

```
$collection = collect([1, 2, 3, 4, 5]);

$chunks = $collection->sliding(3, step: 2);

$chunks->toArray();

// [[1, 2, 3], [3, 4, 5]]
```

#### [`sole()` {.collection-method}](collections.md#method-sole) <a href="#method-sole" id="method-sole"></a>

The `sole` method returns the first element in the collection that passes a given truth test, but only if the truth test matches exactly one element:

```
collect([1, 2, 3, 4])->sole(function (int $value, int $key) {
    return $value === 2;
});

// 2
```

You may also pass a key / value pair to the `sole` method, which will return the first element in the collection that matches the given pair, but only
if it exactly one element matches:

```
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
]);

$collection->sole('product', 'Chair');

// ['product' => 'Chair', 'price' => 100]
```

Alternatively, you may also call the `sole` method with no argument to get the first element in the collection if there is only one element:

```
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
]);

$collection->sole();

// ['product' => 'Desk', 'price' => 200]
```

If there are no elements in the collection that should be returned by the `sole` method, an `\Illuminate\Collections\ItemNotFoundException` exception
will be thrown. If there is more than one element that should be returned, an `\Illuminate\Collections\MultipleItemsFoundException` will be thrown.

#### [`some()` {.collection-method}](collections.md#method-some) <a href="#method-some" id="method-some"></a>

Alias for the [`contains`](collections.md#method-contains) method.

#### [`sort()` {.collection-method}](collections.md#method-sort) <a href="#method-sort" id="method-sort"></a>

The `sort` method sorts the collection. The sorted collection keeps the original array keys, so in the following example we will use the
[`values`](collections.md#method-values) method to reset the keys to consecutively numbered indexes:

```
$collection = collect([5, 3, 1, 2, 4]);

$sorted = $collection->sort();

$sorted->values()->all();

// [1, 2, 3, 4, 5]
```

If your sorting needs are more advanced, you may pass a callback to `sort` with your own algorithm. Refer to the PHP documentation on
[`uasort`](https://secure.php.net/manual/en/function.uasort.php#refsect1-function.uasort-parameters), which is what the collection's `sort` method
calls utilizes internally.

{% hint style="info" %} If you need to sort a collection of nested arrays or objects, see the [`sortBy`](collections.md#method-sortby) and
[`sortByDesc`](collections.md#method-sortbydesc) methods. {% endhint %}

#### [`sortBy()` {.collection-method}](collections.md#method-sortby) <a href="#method-sortby" id="method-sortby"></a>

The `sortBy` method sorts the collection by the given key. The sorted collection keeps the original array keys, so in the following example we will
use the [`values`](collections.md#method-values) method to reset the keys to consecutively numbered indexes:

```
$collection = collect([
    ['name' => 'Desk', 'price' => 200],
    ['name' => 'Chair', 'price' => 100],
    ['name' => 'Bookcase', 'price' => 150],
]);

$sorted = $collection->sortBy('price');

$sorted->values()->all();

/*
    [
        ['name' => 'Chair', 'price' => 100],
        ['name' => 'Bookcase', 'price' => 150],
        ['name' => 'Desk', 'price' => 200],
    ]
*/
```

The `sortBy` method accepts [sort flags](https://www.php.net/manual/en/function.sort.php) as its second argument:

```
$collection = collect([
    ['title' => 'Item 1'],
    ['title' => 'Item 12'],
    ['title' => 'Item 3'],
]);

$sorted = $collection->sortBy('title', SORT_NATURAL);

$sorted->values()->all();

/*
    [
        ['title' => 'Item 1'],
        ['title' => 'Item 3'],
        ['title' => 'Item 12'],
    ]
*/
```

Alternatively, you may pass your own closure to determine how to sort the collection's values:

```
$collection = collect([
    ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
    ['name' => 'Chair', 'colors' => ['Black']],
    ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
]);

$sorted = $collection->sortBy(function (array $product, int $key) {
    return count($product['colors']);
});

$sorted->values()->all();

/*
    [
        ['name' => 'Chair', 'colors' => ['Black']],
        ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
        ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
    ]
*/
```

If you would like to sort your collection by multiple attributes, you may pass an array of sort operations to the `sortBy` method. Each sort operation
should be an array consisting of the attribute that you wish to sort by and the direction of the desired sort:

```
$collection = collect([
    ['name' => 'Taylor Otwell', 'age' => 34],
    ['name' => 'Abigail Otwell', 'age' => 30],
    ['name' => 'Taylor Otwell', 'age' => 36],
    ['name' => 'Abigail Otwell', 'age' => 32],
]);

$sorted = $collection->sortBy([
    ['name', 'asc'],
    ['age', 'desc'],
]);

$sorted->values()->all();

/*
    [
        ['name' => 'Abigail Otwell', 'age' => 32],
        ['name' => 'Abigail Otwell', 'age' => 30],
        ['name' => 'Taylor Otwell', 'age' => 36],
        ['name' => 'Taylor Otwell', 'age' => 34],
    ]
*/
```

When sorting a collection by multiple attributes, you may also provide closures that define each sort operation:

```
$collection = collect([
    ['name' => 'Taylor Otwell', 'age' => 34],
    ['name' => 'Abigail Otwell', 'age' => 30],
    ['name' => 'Taylor Otwell', 'age' => 36],
    ['name' => 'Abigail Otwell', 'age' => 32],
]);

$sorted = $collection->sortBy([
    fn (array $a, array $b) => $a['name'] <=> $b['name'],
    fn (array $a, array $b) => $b['age'] <=> $a['age'],
]);

$sorted->values()->all();

/*
    [
        ['name' => 'Abigail Otwell', 'age' => 32],
        ['name' => 'Abigail Otwell', 'age' => 30],
        ['name' => 'Taylor Otwell', 'age' => 36],
        ['name' => 'Taylor Otwell', 'age' => 34],
    ]
*/
```

#### [`sortByDesc()` {.collection-method}](collections.md#method-sortbydesc) <a href="#method-sortbydesc" id="method-sortbydesc"></a>

This method has the same signature as the [`sortBy`](collections.md#method-sortby) method, but will sort the collection in the opposite order.

#### [`sortDesc()` {.collection-method}](collections.md#method-sortdesc) <a href="#method-sortdesc" id="method-sortdesc"></a>

This method will sort the collection in the opposite order as the [`sort`](collections.md#method-sort) method:

```
$collection = collect([5, 3, 1, 2, 4]);

$sorted = $collection->sortDesc();

$sorted->values()->all();

// [5, 4, 3, 2, 1]
```

Unlike `sort`, you may not pass a closure to `sortDesc`. Instead, you should use the [`sort`](collections.md#method-sort) method and invert your
comparison.

#### [`sortKeys()` {.collection-method}](collections.md#method-sortkeys) <a href="#method-sortkeys" id="method-sortkeys"></a>

The `sortKeys` method sorts the collection by the keys of the underlying associative array:

```
$collection = collect([
    'id' => 22345,
    'first' => 'John',
    'last' => 'Doe',
]);

$sorted = $collection->sortKeys();

$sorted->all();

/*
    [
        'first' => 'John',
        'id' => 22345,
        'last' => 'Doe',
    ]
*/
```

#### [`sortKeysDesc()` {.collection-method}](collections.md#method-sortkeysdesc) <a href="#method-sortkeysdesc" id="method-sortkeysdesc"></a>

This method has the same signature as the [`sortKeys`](collections.md#method-sortkeys) method, but will sort the collection in the opposite order.

#### [`sortKeysUsing()` {.collection-method}](collections.md#method-sortkeysusing) <a href="#method-sortkeysusing" id="method-sortkeysusing"></a>

The `sortKeysUsing` method sorts the collection by the keys of the underlying associative array using a callback:

```
$collection = collect([
    'ID' => 22345,
    'first' => 'John',
    'last' => 'Doe',
]);

$sorted = $collection->sortKeysUsing('strnatcasecmp');

$sorted->all();

/*
    [
        'first' => 'John',
        'ID' => 22345,
        'last' => 'Doe',
    ]
*/
```

The callback must be a comparison function that returns an integer less than, equal to, or greater than zero. For more information, refer to the PHP
documentation on [`uksort`](https://www.php.net/manual/en/function.uksort.php#refsect1-function.uksort-parameters), which is the PHP function that
`sortKeysUsing` method utilizes internally.

#### [`splice()` {.collection-method}](collections.md#method-splice) <a href="#method-splice" id="method-splice"></a>

The `splice` method removes and returns a slice of items starting at the specified index:

```
$collection = collect([1, 2, 3, 4, 5]);

$chunk = $collection->splice(2);

$chunk->all();

// [3, 4, 5]

$collection->all();

// [1, 2]
```

You may pass a second argument to limit the size of the resulting collection:

```
$collection = collect([1, 2, 3, 4, 5]);

$chunk = $collection->splice(2, 1);

$chunk->all();

// [3]

$collection->all();

// [1, 2, 4, 5]
```

In addition, you may pass a third argument containing the new items to replace the items removed from the collection:

```
$collection = collect([1, 2, 3, 4, 5]);

$chunk = $collection->splice(2, 1, [10, 11]);

$chunk->all();

// [3]

$collection->all();

// [1, 2, 10, 11, 4, 5]
```

#### [`split()` {.collection-method}](collections.md#method-split) <a href="#method-split" id="method-split"></a>

The `split` method breaks a collection into the given number of groups:

```
$collection = collect([1, 2, 3, 4, 5]);

$groups = $collection->split(3);

$groups->all();

// [[1, 2], [3, 4], [5]]
```

#### [`splitIn()` {.collection-method}](collections.md#method-splitin) <a href="#method-splitin" id="method-splitin"></a>

The `splitIn` method breaks a collection into the given number of groups, filling non-terminal groups completely before allocating the remainder to
the final group:

```
$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]);

$groups = $collection->splitIn(3);

$groups->all();

// [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10]]
```

#### [`sum()` {.collection-method}](collections.md#method-sum) <a href="#method-sum" id="method-sum"></a>

The `sum` method returns the sum of all items in the collection:

```
collect([1, 2, 3, 4, 5])->sum();

// 15
```

If the collection contains nested arrays or objects, you should pass a key that will be used to determine which values to sum:

```
$collection = collect([
    ['name' => 'JavaScript: The Good Parts', 'pages' => 176],
    ['name' => 'JavaScript: The Definitive Guide', 'pages' => 1096],
]);

$collection->sum('pages');

// 1272
```

In addition, you may pass your own closure to determine which values of the collection to sum:

```
$collection = collect([
    ['name' => 'Chair', 'colors' => ['Black']],
    ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
    ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
]);

$collection->sum(function (array $product) {
    return count($product['colors']);
});

// 6
```

#### [`take()` {.collection-method}](collections.md#method-take) <a href="#method-take" id="method-take"></a>

The `take` method returns a new collection with the specified number of items:

```
$collection = collect([0, 1, 2, 3, 4, 5]);

$chunk = $collection->take(3);

$chunk->all();

// [0, 1, 2]
```

You may also pass a negative integer to take the specified number of items from the end of the collection:

```
$collection = collect([0, 1, 2, 3, 4, 5]);

$chunk = $collection->take(-2);

$chunk->all();

// [4, 5]
```

#### [`takeUntil()` {.collection-method}](collections.md#method-takeuntil) <a href="#method-takeuntil" id="method-takeuntil"></a>

The `takeUntil` method returns items in the collection until the given callback returns `true`:

```
$collection = collect([1, 2, 3, 4]);

$subset = $collection->takeUntil(function (int $item) {
    return $item >= 3;
});

$subset->all();

// [1, 2]
```

You may also pass a simple value to the `takeUntil` method to get the items until the given value is found:

```
$collection = collect([1, 2, 3, 4]);

$subset = $collection->takeUntil(3);

$subset->all();

// [1, 2]
```

> \[!WARNING]\
> If the given value is not found or the callback never returns `true`, the `takeUntil` method will return all items in the collection.

#### [`takeWhile()` {.collection-method}](collections.md#method-takewhile) <a href="#method-takewhile" id="method-takewhile"></a>

The `takeWhile` method returns items in the collection until the given callback returns `false`:

```
$collection = collect([1, 2, 3, 4]);

$subset = $collection->takeWhile(function (int $item) {
    return $item < 3;
});

$subset->all();

// [1, 2]
```

> \[!WARNING]\
> If the callback never returns `false`, the `takeWhile` method will return all items in the collection.

#### [`tap()` {.collection-method}](collections.md#method-tap) <a href="#method-tap" id="method-tap"></a>

The `tap` method passes the collection to the given callback, allowing you to "tap" into the collection at a specific point and do something with the
items while not affecting the collection itself. The collection is then returned by the `tap` method:

```
collect([2, 4, 3, 1, 5])
    ->sort()
    ->tap(function (Collection $collection) {
        Log::debug('Values after sorting', $collection->values()->all());
    })
    ->shift();

// 1
```

#### [`times()` {.collection-method}](collections.md#method-times) <a href="#method-times" id="method-times"></a>

The static `times` method creates a new collection by invoking the given closure a specified number of times:

```
$collection = Collection::times(10, function (int $number) {
    return $number * 9;
});

$collection->all();

// [9, 18, 27, 36, 45, 54, 63, 72, 81, 90]
```

#### [`toArray()` {.collection-method}](collections.md#method-toarray) <a href="#method-toarray" id="method-toarray"></a>

The `toArray` method converts the collection into a plain PHP `array`. If the collection's values are
[Eloquent](../docs/%7B%7Bversion%7D%7D/eloquent/) models, the models will also be converted to arrays:

```
$collection = collect(['name' => 'Desk', 'price' => 200]);

$collection->toArray();

/*
    [
        ['name' => 'Desk', 'price' => 200],
    ]
*/
```

> \[!WARNING]\
> `toArray` also converts all of the collection's nested objects that are an instance of `Arrayable` to an array. If you want to get the raw array underlying
> the collection, use the [`all`](collections.md#method-all) method instead.

#### [`toJson()` {.collection-method}](collections.md#method-tojson) <a href="#method-tojson" id="method-tojson"></a>

The `toJson` method converts the collection into a JSON serialized string:

```
$collection = collect(['name' => 'Desk', 'price' => 200]);

$collection->toJson();

// '{"name":"Desk", "price":200}'
```

#### [`transform()` {.collection-method}](collections.md#method-transform) <a href="#method-transform" id="method-transform"></a>

The `transform` method iterates over the collection and calls the given callback with each item in the collection. The items in the collection will be
replaced by the values returned by the callback:

```
$collection = collect([1, 2, 3, 4, 5]);

$collection->transform(function (int $item, int $key) {
    return $item * 2;
});

$collection->all();

// [2, 4, 6, 8, 10]
```

> \[!WARNING]\
> Unlike most other collection methods, `transform` modifies the collection itself. If you wish to create a new collection instead, use the [`map`](collections.md#method-map)
> method.

#### [`undot()` {.collection-method}](collections.md#method-undot) <a href="#method-undot" id="method-undot"></a>

The `undot` method expands a single-dimensional collection that uses "dot" notation into a multi-dimensional collection:

```
$person = collect([
    'name.first_name' => 'Marie',
    'name.last_name' => 'Valentine',
    'address.line_1' => '2992 Eagle Drive',
    'address.line_2' => '',
    'address.suburb' => 'Detroit',
    'address.state' => 'MI',
    'address.postcode' => '48219'
]);

$person = $person->undot();

$person->toArray();

/*
    [
        "name" => [
            "first_name" => "Marie",
            "last_name" => "Valentine",
        ],
        "address" => [
            "line_1" => "2992 Eagle Drive",
            "line_2" => "",
            "suburb" => "Detroit",
            "state" => "MI",
            "postcode" => "48219",
        ],
    ]
*/
```

#### [`union()` {.collection-method}](collections.md#method-union) <a href="#method-union" id="method-union"></a>

The `union` method adds the given array to the collection. If the given array contains keys that are already in the original collection, the original
collection's values will be preferred:

```
$collection = collect([1 => ['a'], 2 => ['b']]);

$union = $collection->union([3 => ['c'], 1 => ['d']]);

$union->all();

// [1 => ['a'], 2 => ['b'], 3 => ['c']]
```

#### [`unique()` {.collection-method}](collections.md#method-unique) <a href="#method-unique" id="method-unique"></a>

The `unique` method returns all of the unique items in the collection. The returned collection keeps the original array keys, so in the following
example we will use the [`values`](collections.md#method-values) method to reset the keys to consecutively numbered indexes:

```
$collection = collect([1, 1, 2, 2, 3, 4, 2]);

$unique = $collection->unique();

$unique->values()->all();

// [1, 2, 3, 4]
```

When dealing with nested arrays or objects, you may specify the key used to determine uniqueness:

```
$collection = collect([
    ['name' => 'iPhone 6', 'brand' => 'Apple', 'type' => 'phone'],
    ['name' => 'iPhone 5', 'brand' => 'Apple', 'type' => 'phone'],
    ['name' => 'Apple Watch', 'brand' => 'Apple', 'type' => 'watch'],
    ['name' => 'Galaxy S6', 'brand' => 'Samsung', 'type' => 'phone'],
    ['name' => 'Galaxy Gear', 'brand' => 'Samsung', 'type' => 'watch'],
]);

$unique = $collection->unique('brand');

$unique->values()->all();

/*
    [
        ['name' => 'iPhone 6', 'brand' => 'Apple', 'type' => 'phone'],
        ['name' => 'Galaxy S6', 'brand' => 'Samsung', 'type' => 'phone'],
    ]
*/
```

Finally, you may also pass your own closure to the `unique` method to specify which value should determine an item's uniqueness:

```
$unique = $collection->unique(function (array $item) {
    return $item['brand'].$item['type'];
});

$unique->values()->all();

/*
    [
        ['name' => 'iPhone 6', 'brand' => 'Apple', 'type' => 'phone'],
        ['name' => 'Apple Watch', 'brand' => 'Apple', 'type' => 'watch'],
        ['name' => 'Galaxy S6', 'brand' => 'Samsung', 'type' => 'phone'],
        ['name' => 'Galaxy Gear', 'brand' => 'Samsung', 'type' => 'watch'],
    ]
*/
```

The `unique` method uses "loose" comparisons when checking item values, meaning a string with an integer value will be considered equal to an integer
of the same value. Use the [`uniqueStrict`](collections.md#method-uniquestrict) method to filter using "strict" comparisons.

{% hint style="info" %} This method's behavior is modified when using
[Eloquent Collections](../docs/%7B%7Bversion%7D%7D/eloquent-collections/#method-unique). {% endhint %}

#### [`uniqueStrict()` {.collection-method}](collections.md#method-uniquestrict) <a href="#method-uniquestrict" id="method-uniquestrict"></a>

This method has the same signature as the [`unique`](collections.md#method-unique) method; however, all values are compared using "strict"
comparisons.

#### [`unless()` {.collection-method}](collections.md#method-unless) <a href="#method-unless" id="method-unless"></a>

The `unless` method will execute the given callback unless the first argument given to the method evaluates to `true`:

```
$collection = collect([1, 2, 3]);

$collection->unless(true, function (Collection $collection) {
    return $collection->push(4);
});

$collection->unless(false, function (Collection $collection) {
    return $collection->push(5);
});

$collection->all();

// [1, 2, 3, 5]
```

A second callback may be passed to the `unless` method. The second callback will be executed when the first argument given to the `unless` method
evaluates to `true`:

```
$collection = collect([1, 2, 3]);

$collection->unless(true, function (Collection $collection) {
    return $collection->push(4);
}, function (Collection $collection) {
    return $collection->push(5);
});

$collection->all();

// [1, 2, 3, 5]
```

For the inverse of `unless`, see the [`when`](collections.md#method-when) method.

#### [`unlessEmpty()` {.collection-method}](collections.md#method-unlessempty) <a href="#method-unlessempty" id="method-unlessempty"></a>

Alias for the [`whenNotEmpty`](collections.md#method-whennotempty) method.

#### [`unlessNotEmpty()` {.collection-method}](collections.md#method-unlessnotempty) <a href="#method-unlessnotempty" id="method-unlessnotempty"></a>

Alias for the [`whenEmpty`](collections.md#method-whenempty) method.

#### [`unwrap()` {.collection-method}](collections.md#method-unwrap) <a href="#method-unwrap" id="method-unwrap"></a>

The static `unwrap` method returns the collection's underlying items from the given value when applicable:

```
Collection::unwrap(collect('John Doe'));

// ['John Doe']

Collection::unwrap(['John Doe']);

// ['John Doe']

Collection::unwrap('John Doe');

// 'John Doe'
```

#### [`value()` {.collection-method}](collections.md#method-value) <a href="#method-value" id="method-value"></a>

The `value` method retrieves a given value from the first element of the collection:

```
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Speaker', 'price' => 400],
]);

$value = $collection->value('price');

// 200
```

#### [`values()` {.collection-method}](collections.md#method-values) <a href="#method-values" id="method-values"></a>

The `values` method returns a new collection with the keys reset to consecutive integers:

```
$collection = collect([
    10 => ['product' => 'Desk', 'price' => 200],
    11 => ['product' => 'Desk', 'price' => 200],
]);

$values = $collection->values();

$values->all();

/*
    [
        0 => ['product' => 'Desk', 'price' => 200],
        1 => ['product' => 'Desk', 'price' => 200],
    ]
*/
```

#### [`when()` {.collection-method}](collections.md#method-when) <a href="#method-when" id="method-when"></a>

The `when` method will execute the given callback when the first argument given to the method evaluates to `true`. The collection instance and the
first argument given to the `when` method will be provided to the closure:

```
$collection = collect([1, 2, 3]);

$collection->when(true, function (Collection $collection, int $value) {
    return $collection->push(4);
});

$collection->when(false, function (Collection $collection, int $value) {
    return $collection->push(5);
});

$collection->all();

// [1, 2, 3, 4]
```

A second callback may be passed to the `when` method. The second callback will be executed when the first argument given to the `when` method
evaluates to `false`:

```
$collection = collect([1, 2, 3]);

$collection->when(false, function (Collection $collection, int $value) {
    return $collection->push(4);
}, function (Collection $collection) {
    return $collection->push(5);
});

$collection->all();

// [1, 2, 3, 5]
```

For the inverse of `when`, see the [`unless`](collections.md#method-unless) method.

#### [`whenEmpty()` {.collection-method}](collections.md#method-whenempty) <a href="#method-whenempty" id="method-whenempty"></a>

The `whenEmpty` method will execute the given callback when the collection is empty:

```
$collection = collect(['Michael', 'Tom']);

$collection->whenEmpty(function (Collection $collection) {
    return $collection->push('Adam');
});

$collection->all();

// ['Michael', 'Tom']


$collection = collect();

$collection->whenEmpty(function (Collection $collection) {
    return $collection->push('Adam');
});

$collection->all();

// ['Adam']
```

A second closure may be passed to the `whenEmpty` method that will be executed when the collection is not empty:

```
$collection = collect(['Michael', 'Tom']);

$collection->whenEmpty(function (Collection $collection) {
    return $collection->push('Adam');
}, function (Collection $collection) {
    return $collection->push('Taylor');
});

$collection->all();

// ['Michael', 'Tom', 'Taylor']
```

For the inverse of `whenEmpty`, see the [`whenNotEmpty`](collections.md#method-whennotempty) method.

#### [`whenNotEmpty()` {.collection-method}](collections.md#method-whennotempty) <a href="#method-whennotempty" id="method-whennotempty"></a>

The `whenNotEmpty` method will execute the given callback when the collection is not empty:

```
$collection = collect(['michael', 'tom']);

$collection->whenNotEmpty(function (Collection $collection) {
    return $collection->push('adam');
});

$collection->all();

// ['michael', 'tom', 'adam']


$collection = collect();

$collection->whenNotEmpty(function (Collection $collection) {
    return $collection->push('adam');
});

$collection->all();

// []
```

A second closure may be passed to the `whenNotEmpty` method that will be executed when the collection is empty:

```
$collection = collect();

$collection->whenNotEmpty(function (Collection $collection) {
    return $collection->push('adam');
}, function (Collection $collection) {
    return $collection->push('taylor');
});

$collection->all();

// ['taylor']
```

For the inverse of `whenNotEmpty`, see the [`whenEmpty`](collections.md#method-whenempty) method.

#### [`where()` {.collection-method}](collections.md#method-where) <a href="#method-where" id="method-where"></a>

The `where` method filters the collection by a given key / value pair:

```
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
    ['product' => 'Bookcase', 'price' => 150],
    ['product' => 'Door', 'price' => 100],
]);

$filtered = $collection->where('price', 100);

$filtered->all();

/*
    [
        ['product' => 'Chair', 'price' => 100],
        ['product' => 'Door', 'price' => 100],
    ]
*/
```

The `where` method uses "loose" comparisons when checking item values, meaning a string with an integer value will be considered equal to an integer
of the same value. Use the [`whereStrict`](collections.md#method-wherestrict) method to filter using "strict" comparisons.

Optionally, you may pass a comparison operator as the second parameter. Supported operators are: '===', '!==', '!=', '==', '=', '<>', '>', '<', '>=',
and '<=':

```
$collection = collect([
    ['name' => 'Jim', 'deleted_at' => '2019-01-01 00:00:00'],
    ['name' => 'Sally', 'deleted_at' => '2019-01-02 00:00:00'],
    ['name' => 'Sue', 'deleted_at' => null],
]);

$filtered = $collection->where('deleted_at', '!=', null);

$filtered->all();

/*
    [
        ['name' => 'Jim', 'deleted_at' => '2019-01-01 00:00:00'],
        ['name' => 'Sally', 'deleted_at' => '2019-01-02 00:00:00'],
    ]
*/
```

#### [`whereStrict()` {.collection-method}](collections.md#method-wherestrict) <a href="#method-wherestrict" id="method-wherestrict"></a>

This method has the same signature as the [`where`](collections.md#method-where) method; however, all values are compared using "strict" comparisons.

#### [`whereBetween()` {.collection-method}](collections.md#method-wherebetween) <a href="#method-wherebetween" id="method-wherebetween"></a>

The `whereBetween` method filters the collection by determining if a specified item value is within a given range:

```
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 80],
    ['product' => 'Bookcase', 'price' => 150],
    ['product' => 'Pencil', 'price' => 30],
    ['product' => 'Door', 'price' => 100],
]);

$filtered = $collection->whereBetween('price', [100, 200]);

$filtered->all();

/*
    [
        ['product' => 'Desk', 'price' => 200],
        ['product' => 'Bookcase', 'price' => 150],
        ['product' => 'Door', 'price' => 100],
    ]
*/
```

#### [`whereIn()` {.collection-method}](collections.md#method-wherein) <a href="#method-wherein" id="method-wherein"></a>

The `whereIn` method removes elements from the collection that do not have a specified item value that is contained within the given array:

```
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
    ['product' => 'Bookcase', 'price' => 150],
    ['product' => 'Door', 'price' => 100],
]);

$filtered = $collection->whereIn('price', [150, 200]);

$filtered->all();

/*
    [
        ['product' => 'Desk', 'price' => 200],
        ['product' => 'Bookcase', 'price' => 150],
    ]
*/
```

The `whereIn` method uses "loose" comparisons when checking item values, meaning a string with an integer value will be considered equal to an integer
of the same value. Use the [`whereInStrict`](collections.md#method-whereinstrict) method to filter using "strict" comparisons.

#### [`whereInStrict()` {.collection-method}](collections.md#method-whereinstrict) <a href="#method-whereinstrict" id="method-whereinstrict"></a>

This method has the same signature as the [`whereIn`](collections.md#method-wherein) method; however, all values are compared using "strict"
comparisons.

#### [`whereInstanceOf()` {.collection-method}](collections.md#method-whereinstanceof) <a href="#method-whereinstanceof" id="method-whereinstanceof"></a>

The `whereInstanceOf` method filters the collection by a given class type:

```
use App\Models\User;
use App\Models\Post;

$collection = collect([
    new User,
    new User,
    new Post,
]);

$filtered = $collection->whereInstanceOf(User::class);

$filtered->all();

// [App\Models\User, App\Models\User]
```

#### [`whereNotBetween()` {.collection-method}](collections.md#method-wherenotbetween) <a href="#method-wherenotbetween" id="method-wherenotbetween"></a>

The `whereNotBetween` method filters the collection by determining if a specified item value is outside of a given range:

```
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 80],
    ['product' => 'Bookcase', 'price' => 150],
    ['product' => 'Pencil', 'price' => 30],
    ['product' => 'Door', 'price' => 100],
]);

$filtered = $collection->whereNotBetween('price', [100, 200]);

$filtered->all();

/*
    [
        ['product' => 'Chair', 'price' => 80],
        ['product' => 'Pencil', 'price' => 30],
    ]
*/
```

#### [`whereNotIn()` {.collection-method}](collections.md#method-wherenotin) <a href="#method-wherenotin" id="method-wherenotin"></a>

The `whereNotIn` method removes elements from the collection that have a specified item value that is contained within the given array:

```
$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
    ['product' => 'Bookcase', 'price' => 150],
    ['product' => 'Door', 'price' => 100],
]);

$filtered = $collection->whereNotIn('price', [150, 200]);

$filtered->all();

/*
    [
        ['product' => 'Chair', 'price' => 100],
        ['product' => 'Door', 'price' => 100],
    ]
*/
```

The `whereNotIn` method uses "loose" comparisons when checking item values, meaning a string with an integer value will be considered equal to an
integer of the same value. Use the [`whereNotInStrict`](collections.md#method-wherenotinstrict) method to filter using "strict" comparisons.

#### [`whereNotInStrict()` {.collection-method}](collections.md#method-wherenotinstrict) <a href="#method-wherenotinstrict" id="method-wherenotinstrict"></a>

This method has the same signature as the [`whereNotIn`](collections.md#method-wherenotin) method; however, all values are compared using "strict"
comparisons.

#### [`whereNotNull()` {.collection-method}](collections.md#method-wherenotnull) <a href="#method-wherenotnull" id="method-wherenotnull"></a>

The `whereNotNull` method returns items from the collection where the given key is not `null`:

```
$collection = collect([
    ['name' => 'Desk'],
    ['name' => null],
    ['name' => 'Bookcase'],
]);

$filtered = $collection->whereNotNull('name');

$filtered->all();

/*
    [
        ['name' => 'Desk'],
        ['name' => 'Bookcase'],
    ]
*/
```

#### [`whereNull()` {.collection-method}](collections.md#method-wherenull) <a href="#method-wherenull" id="method-wherenull"></a>

The `whereNull` method returns items from the collection where the given key is `null`:

```
$collection = collect([
    ['name' => 'Desk'],
    ['name' => null],
    ['name' => 'Bookcase'],
]);

$filtered = $collection->whereNull('name');

$filtered->all();

/*
    [
        ['name' => null],
    ]
*/
```

#### [`wrap()` {.collection-method}](collections.md#method-wrap) <a href="#method-wrap" id="method-wrap"></a>

The static `wrap` method wraps the given value in a collection when applicable:

```
use Maginium\Framework\Support\Collection;

$collection = Collection::wrap('John Doe');

$collection->all();

// ['John Doe']

$collection = Collection::wrap(['John Doe']);

$collection->all();

// ['John Doe']

$collection = Collection::wrap(collect('John Doe'));

$collection->all();

// ['John Doe']
```

#### [`zip()` {.collection-method}](collections.md#method-zip) <a href="#method-zip" id="method-zip"></a>

The `zip` method merges together the values of the given array with the values of the original collection at their corresponding index:

```
$collection = collect(['Chair', 'Desk']);

$zipped = $collection->zip([100, 200]);

$zipped->all();

// [['Chair', 100], ['Desk', 200]]
```

## [Higher Order Messages](collections.md#higher-order-messages) <a href="#higher-order-messages" id="higher-order-messages"></a>

Collections also provide support for "higher order messages", which are short-cuts for performing common actions on collections. The collection
methods that provide higher order messages are: [`average`](collections.md#method-average), [`avg`](collections.md#method-avg),
[`contains`](collections.md#method-contains), [`each`](collections.md#method-each), [`every`](collections.md#method-every),
[`filter`](collections.md#method-filter), [`first`](collections.md#method-first), [`flatMap`](collections.md#method-flatmap),
[`groupBy`](collections.md#method-groupby), [`keyBy`](collections.md#method-keyby), [`map`](collections.md#method-map),
[`max`](collections.md#method-max), [`min`](collections.md#method-min), [`partition`](collections.md#method-partition),
[`reject`](collections.md#method-reject), [`skipUntil`](collections.md#method-skipuntil), [`skipWhile`](collections.md#method-skipwhile),
[`some`](collections.md#method-some), [`sortBy`](collections.md#method-sortby), [`sortByDesc`](collections.md#method-sortbydesc),
[`sum`](collections.md#method-sum), [`takeUntil`](collections.md#method-takeuntil), [`takeWhile`](collections.md#method-takewhile), and
[`unique`](collections.md#method-unique).

Each higher order message can be accessed as a dynamic property on a collection instance. For instance, let's use the `each` higher order message to
call a method on each object within a collection:

```
use App\Models\User;

$users = User::where('votes', '>', 500)->get();

$users->each->markAsVip();
```

Likewise, we can use the `sum` higher order message to gather the total number of "votes" for a collection of users:

```
$users = User::where('group', 'Development')->get();

return $users->sum->votes;
```

## [Lazy Collections](collections.md#lazy-collections) <a href="#lazy-collections" id="lazy-collections"></a>

### [Introduction](collections.md#lazy-collection-introduction) <a href="#lazy-collection-introduction" id="lazy-collection-introduction"></a>

> \[!WARNING]\
> Before learning more about Maginium's lazy collections, take some time to familiarize yourself with [PHP generators](https://www.php.net/manual/en/language.generators.overview.php).

To supplement the already powerful `Collection` class, the `LazyCollection` class leverages PHP's
[generators](https://www.php.net/manual/en/language.generators.overview.php) to allow you to work with very large datasets while keeping memory usage
low.

For example, imagine your application needs to process a multi-gigabyte log file while taking advantage of Maginium's collection methods to parse the
logs. Instead of reading the entire file into memory at once, lazy collections may be used to keep only a small part of the file in memory at a given
time:

```
use App\Models\LogEntry;
use Maginium\Framework\Support\LazyCollection;

LazyCollection::make(function () {
    $handle = fopen('log.txt', 'r');

    while (($line = fgets($handle)) !== false) {
        yield $line;
    }
})->chunk(4)->map(function (array $lines) {
    return LogEntry::fromLines($lines);
})->each(function (LogEntry $logEntry) {
    // Process the log entry...
});
```

Or, imagine you need to iterate through 10,000 Eloquent models. When using traditional Maginium collections, all 10,000 Eloquent models must be loaded
into memory at the same time:

```
use App\Models\User;

$users = User::all()->filter(function (User $user) {
    return $user->id > 500;
});
```

However, the query builder's `cursor` method returns a `LazyCollection` instance. This allows you to still only run a single query against the
database but also only keep one Eloquent model loaded in memory at a time. In this example, the `filter` callback is not executed until we actually
iterate over each user individually, allowing for a drastic reduction in memory usage:

```
use App\Models\User;

$users = User::cursor()->filter(function (User $user) {
    return $user->id > 500;
});

foreach ($users as $user) {
    echo $user->id;
}
```

### [Creating Lazy Collections](collections.md#creating-lazy-collections) <a href="#creating-lazy-collections" id="creating-lazy-collections"></a>

To create a lazy collection instance, you should pass a PHP generator function to the collection's `make` method:

```
use Maginium\Framework\Support\LazyCollection;

LazyCollection::make(function () {
    $handle = fopen('log.txt', 'r');

    while (($line = fgets($handle)) !== false) {
        yield $line;
    }
});
```

### [The Enumerable Contract](collections.md#the-enumerable-contract) <a href="#the-enumerable-contract" id="the-enumerable-contract"></a>

Almost all methods available on the `Collection` class are also available on the `LazyCollection` class. Both of these classes implement the
`Maginium\Framework\Support\Enumerable` contract, which defines the following methods:

[all](collections.md#method-all) [average](collections.md#method-average) [avg](collections.md#method-avg) [chunk](collections.md#method-chunk)
[chunkWhile](collections.md#method-chunkwhile) [collapse](collections.md#method-collapse) [collect](collections.md#method-collect)
[combine](collections.md#method-combine) [concat](collections.md#method-concat) [contains](collections.md#method-contains)
[containsStrict](collections.md#method-containsstrict) [count](collections.md#method-count) [countBy](collections.md#method-countBy)
[crossJoin](collections.md#method-crossjoin) [dd](collections.md#method-dd) [diff](collections.md#method-diff)
[diffAssoc](collections.md#method-diffassoc) [diffKeys](collections.md#method-diffkeys) [dump](collections.md#method-dump)
[duplicates](collections.md#method-duplicates) [duplicatesStrict](collections.md#method-duplicatesstrict) [each](collections.md#method-each)
[eachSpread](collections.md#method-eachspread) [every](collections.md#method-every) [except](collections.md#method-except)
[filter](collections.md#method-filter) [first](collections.md#method-first) [firstOrFail](collections.md#method-first-or-fail)
[firstWhere](collections.md#method-first-where) [flatMap](collections.md#method-flatmap) [flatten](collections.md#method-flatten)
[flip](collections.md#method-flip) [forPage](collections.md#method-forpage) [get](collections.md#method-get) [groupBy](collections.md#method-groupby)
[has](collections.md#method-has) [implode](collections.md#method-implode) [intersect](collections.md#method-intersect)
[intersectAssoc](collections.md#method-intersectAssoc) [intersectByKeys](collections.md#method-intersectbykeys)
[isEmpty](collections.md#method-isempty) [isNotEmpty](collections.md#method-isnotempty) [join](collections.md#method-join)
[keyBy](collections.md#method-keyby) [keys](collections.md#method-keys) [last](collections.md#method-last) [macro](collections.md#method-macro)
[make](collections.md#method-make) [map](collections.md#method-map) [mapInto](collections.md#method-mapinto)
[mapSpread](collections.md#method-mapspread) [mapToGroups](collections.md#method-maptogroups) [mapWithKeys](collections.md#method-mapwithkeys)
[max](collections.md#method-max) [median](collections.md#method-median) [merge](collections.md#method-merge)
[mergeRecursive](collections.md#method-mergerecursive) [min](collections.md#method-min) [mode](collections.md#method-mode)
[nth](collections.md#method-nth) [only](collections.md#method-only) [pad](collections.md#method-pad) [partition](collections.md#method-partition)
[pipe](collections.md#method-pipe) [pluck](collections.md#method-pluck) [random](collections.md#method-random) [reduce](collections.md#method-reduce)
[reject](collections.md#method-reject) [replace](collections.md#method-replace) [replaceRecursive](collections.md#method-replacerecursive)
[reverse](collections.md#method-reverse) [search](collections.md#method-search) [shuffle](collections.md#method-shuffle)
[skip](collections.md#method-skip) [slice](collections.md#method-slice) [sole](collections.md#method-sole) [some](collections.md#method-some)
[sort](collections.md#method-sort) [sortBy](collections.md#method-sortby) [sortByDesc](collections.md#method-sortbydesc)
[sortKeys](collections.md#method-sortkeys) [sortKeysDesc](collections.md#method-sortkeysdesc) [split](collections.md#method-split)
[sum](collections.md#method-sum) [take](collections.md#method-take) [tap](collections.md#method-tap) [times](collections.md#method-times)
[toArray](collections.md#method-toarray) [toJson](collections.md#method-tojson) [union](collections.md#method-union)
[unique](collections.md#method-unique) [uniqueStrict](collections.md#method-uniquestrict) [unless](collections.md#method-unless)
[unlessEmpty](collections.md#method-unlessempty) [unlessNotEmpty](collections.md#method-unlessnotempty) [unwrap](collections.md#method-unwrap)
[values](collections.md#method-values) [when](collections.md#method-when) [whenEmpty](collections.md#method-whenempty)
[whenNotEmpty](collections.md#method-whennotempty) [where](collections.md#method-where) [whereStrict](collections.md#method-wherestrict)
[whereBetween](collections.md#method-wherebetween) [whereIn](collections.md#method-wherein) [whereInStrict](collections.md#method-whereinstrict)
[whereInstanceOf](collections.md#method-whereinstanceof) [whereNotBetween](collections.md#method-wherenotbetween)
[whereNotIn](collections.md#method-wherenotin) [whereNotInStrict](collections.md#method-wherenotinstrict) [wrap](collections.md#method-wrap)
[zip](collections.md#method-zip)

> \[!WARNING]\
> Methods that mutate the collection (such as `shift`, `pop`, `prepend` etc.) are **not** available on the `LazyCollection` class.

### [Lazy Collection Methods](collections.md#lazy-collection-methods) <a href="#lazy-collection-methods" id="lazy-collection-methods"></a>

In addition to the methods defined in the `Enumerable` contract, the `LazyCollection` class contains the following methods:

#### [`takeUntilTimeout()` {.collection-method}](collections.md#method-takeUntilTimeout) <a href="#method-takeuntiltimeout" id="method-takeuntiltimeout"></a>

The `takeUntilTimeout` method returns a new lazy collection that will enumerate values until the specified time. After that time, the collection will
then stop enumerating:

```
$lazyCollection = LazyCollection::times(INF)
    ->takeUntilTimeout(now()->addMinute());

$lazyCollection->each(function (int $number) {
    dump($number);

    sleep(1);
});

// 1
// 2
// ...
// 58
// 59
```

To illustrate the usage of this method, imagine an application that submits invoices from the database using a cursor. You could define a
[scheduled task](../docs/%7B%7Bversion%7D%7D/scheduling/) that runs every 15 minutes and only processes invoices for a maximum of 14 minutes:

```
use App\Models\Invoice;
use Maginium\Framework\Support\Carbon;

Invoice::pending()->cursor()
    ->takeUntilTimeout(
        Carbon::createFromTimestamp(MAGINIUM_START)->add(14, 'minutes')
    )
    ->each(fn (Invoice $invoice) => $invoice->submit());
```

#### [`tapEach()` {.collection-method}](collections.md#method-tapEach) <a href="#method-tapeach" id="method-tapeach"></a>

While the `each` method calls the given callback for each item in the collection right away, the `tapEach` method only calls the given callback as the
items are being pulled out of the list one by one:

```
// Nothing has been dumped so far...
$lazyCollection = LazyCollection::times(INF)->tapEach(function (int $value) {
    dump($value);
});

// Three items are dumped...
$array = $lazyCollection->take(3)->all();

// 1
// 2
// 3
```

#### [`throttle()` {.collection-method}](collections.md#method-throttle) <a href="#method-throttle" id="method-throttle"></a>

The `throttle` method will throttle the lazy collection such that each value is returned after the specified number of seconds. This method is
especially useful for situations where you may be interacting with external APIs that rate limit incoming requests:

```php
use App\Models\User;

User::where('vip', true)
    ->cursor()
    ->throttle(seconds: 1)
    ->each(function (User $user) {
        // Call external API...
    });
```

#### [`remember()` {.collection-method}](collections.md#method-remember) <a href="#method-remember" id="method-remember"></a>

The `remember` method returns a new lazy collection that will remember any values that have already been enumerated and will not retrieve them again
on subsequent collection enumerations:

```
// No query has been executed yet...
$users = User::cursor()->remember();

// The query is executed...
// The first 5 users are hydrated from the database...
$users->take(5)->all();

// First 5 users come from the collection's cache...
// The rest are hydrated from the database...
$users->take(20)->all();
```
