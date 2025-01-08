---
icon: info
---

# Helpers

Maginium provides a collection of helper functions that make everyday tasks easier and more intuitive for developers. These helpers are globally available and can save significant development time by simplifying common operations. Let's explore these helpers in detail.

***

### String Helpers

\### Str::plural()

The `Str::plural()` helper converts a given word to its plural form. This is particularly useful when dynamically generating text in your application.

```php
use Maginium\Framework\Support\Str;

$plural = Str::plural('car'); // Returns 'cars'
$plural = Str::plural('child'); // Returns 'children'
```

\### Str::singular()

Converts a plural word to its singular form:

```php
$singular = Str::singular('cars'); // Returns 'car'
$singular = Str::singular('children'); // Returns 'child'
```

> **Important**: Maginium's string helpers intelligently handle irregular cases, making them ideal for content-rich applications.

***

### Array Helpers

\### Arr::except()

Removes specified keys from an array.

```php
use Maginium\Framework\Support\Arr;

$array = ['name' => 'John', 'age' => 30];
$result = Arr::except($array, ['age']);
// Returns ['name' => 'John']
```

\### Arr::only()

Extracts only the specified keys from an array.

```php
$result = Arr::only($array, ['name']);
// Returns ['name' => 'John']
```

\### Arr::has()

Checks if a given key exists in an array.

```php
$exists = Arr::has($array, 'name'); // Returns true
$exists = Arr::has($array, 'address'); // Returns false
```

> **Tip**: Use `Arr` helpers to manipulate arrays efficiently in your controllers or utility classes.

***

### Path Helpers

\### base\_path()

Returns the fully qualified path to the root of the Maginium application.

```php
$path = base_path('config/app.php');
// Returns '/var/www/maginium/config/app.php'
```

\### app\_path()

Gets the path to the `app` directory:

```php
$path = app_path('Http/Controllers');
// Returns '/var/www/maginium/app/Http/Controllers'
```

> **Important**: Path helpers ensure portability and prevent hardcoding paths, which can lead to errors during deployment.

***

### URL Helpers

\### url()

Generates a fully qualified URL for a given path.

```php
$url = url('user/profile');
// Returns 'http://your-app.com/user/profile'
```

\### route()

Generates a URL for a named route.

```php
$url = route('profile', ['id' => 1]);
// Returns 'http://your-app.com/profile/1'
```

> **Note**: Use URL helpers for clean and reliable link generation within your application.

***

### Validation Helpers

\### validator()

Creates a new validator instance for validating input data.

```php
use Maginium\Framework\Validation\Validator;

$validator = validator(['email' => 'invalid'], ['email' => 'required|email']);

if ($validator->fails()) {
    echo 'Validation failed';
}
```

\### old()

Retrieves old input data for pre-filling forms.

```php
$oldValue = old('email', 'default@example.com');
// Returns the previously submitted value for 'email' or the default value.
```

***

