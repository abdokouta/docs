---
icon: folder-tree
---

# Structure

## Application Structure

Maginium's application structure is crafted to offer a clean and organised way to build applications, ensuring scalability, maintainability, and ease
of development. Each component of the structure is designed to serve a specific purpose, enabling developers to locate and modify code efficiently.

### Directory Overview

Below is a quick overview of the primary directories included in a typical Maginium application:

```plaintext
app/            Application code (Controllers, Models, Middleware, etc.)
bootstrap/      Application bootstrapping and autoload setup
config/         Configuration files for the application
database/       Database-related files (migrations, factories, seeders)
lang/           Localization files
public/         Entry point for HTTP requests and public assets
resources/      Frontend assets like views, CSS, and JavaScript
routes/         Application route definitions
storage/        File storage, logs, and compiled views
tests/          Automated tests for the application
vendor/         Composer dependencies
```

> **Important** The `public/` directory should be set as the document root in your web server to ensure security.

---

### The `app` Directory

This directory contains the core logic of your application. By default, it is divided into subdirectories like `Console`, `Http`, and `Models`:

- **Console**: Houses all console commands.
- **Http**: Contains controllers, middleware, and form requests.
- **Models**: Holds your application's Eloquent models.

You are free to create additional directories within `app/` to better organise your application logic.

```plaintext
app/
├── Console/
├── Http/
│   ├── Controllers/
│   ├── Middleware/
├── Models/
```

---

### The `bootstrap` Directory

The `bootstrap/` directory contains the `app.php` file, which bootstraps the framework. This directory also holds a `cache/` directory for performance
optimizations, such as compiled files and route caching.

---

### The `config` Directory

All of your application's configuration files are stored in the `config/` directory. These files provide a clean and intuitive way to configure your
application.

---

### The `database` Directory

This directory includes:

- **Migrations**: Version control for your database schema.
- **Factories**: Generate fake data for testing.
- **Seeders**: Populate your database with initial data.

```plaintext
database/
├── migrations/
├── factories/
├── seeders/
```

---

### The `public` Directory

This is the entry point for all HTTP requests. It contains:

- `index.php`: The main entry script.
- Static assets like images, JavaScript, and CSS files.

> **Important** The `public/` directory should be the only directory exposed to the web.

---

### The `resources` Directory

The `resources/` directory contains your application's frontend assets:

- **Views**: Blade templates for rendering HTML.
- **CSS & JS**: Stylesheets and JavaScript files.
- **Lang**: Localization files for supporting multiple languages.

```plaintext
resources/
├── views/
├── css/
├── js/
├── lang/
```

---

### The `routes` Directory

This directory contains route definitions for your application. By default, it includes files like:

- `web.php`: Routes for web interfaces.
- `api.php`: Routes for APIs.
- `console.php`: Routes for console-based commands.

```plaintext
routes/
├── web.php
├── api.php
├── console.php
```

---

### The `storage` Directory

The `storage/` directory stores:

- **Logs**: Application log files.
- **Compiled Views**: Precompiled Blade templates.
- **File Storage**: Uploaded files and other user-generated content.

```plaintext
storage/
├── logs/
├── framework/
│   ├── cache/
│   ├── sessions/
├── app/
```

> **Attention** Ensure this directory is **not publicly accessible**. Instead, use symbolic links for publicly accessible files.

---

### The `tests` Directory

This directory contains automated tests for your application. By default, it includes `Feature` and `Unit` subdirectories:

- **Feature**: High-level tests covering multiple parts of your application.
- **Unit**: Low-level tests focused on specific methods or functions.

---

### The `vendor` Directory

The `vendor/` directory is where Composer stores all your application's dependencies. You should not modify its contents directly.

---

### Customising the Structure

Maginium gives you the freedom to customise the directory structure according to your needs. You can even configure custom namespaces using Composer's
autoload feature.

```json
{
  "autoload": {
    "psr-4": {
      "App\\": "app/",
      "Custom\\": "custom-directory/"
    }
  }
}
```

Run `composer dump-autoload` after making changes to your `composer.json` file.

> **Important** Customising the structure is optional and should only be done when necessary to maintain clarity.

---

By understanding and leveraging Maginium's application structure, you can create powerful and maintainable applications with ease. Each component is
carefully designed to provide both flexibility and simplicity for developers.
