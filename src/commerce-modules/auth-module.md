# Auth Module

Maginium’s **Auth Module** is a strong authentication system designed to handle user login, registration, and session management seamlessly. It provides a highly secure and configurable framework for building user authentication into your application. This guide will help you configure and utilize the Auth Module efficiently.

**Features of the Auth Module**

* **Secure Authentication:** Supports hashing and secure password storage.
* **User Registration:** Simplified user onboarding process.
* **Session Management:** Manage active user sessions effectively.
* **Password Reset:** Easy-to-use password recovery feature.
* **Extensibility:** Fully customizable to fit unique business requirements.

**Setting Up the Auth Module**

To enable authentication in your Maginium application:

**Step 1: Install the Auth Module**&#x20;

Ensure that the Auth module is included in your project. Run the following Artisan command:

```bash
php artisan auth:install
```

This will scaffold essential files, including controllers, views, and routes for authentication.

#### Step 2: Database Migration

Run the migrations to create the necessary tables:

```bash
bashCopyEditphp artisan migrate
```

The migration will create tables like `users`, `password_resets`, and others required for authentication.

***

### Authentication Workflow

Maginium’s Auth Module provides pre-built controllers to manage user authentication:

1. **Login:** Authenticates a user and starts a session.
2. **Registration:** Allows users to create new accounts.
3. **Logout:** Ends a user session securely.
4. **Password Reset:** Enables users to reset forgotten passwords.

#### Default Routes

The following routes are registered automatically:

* `GET /login` – Displays the login form.
* `POST /login` – Authenticates the user.
* `GET /register` – Displays the registration form.
* `POST /register` – Registers a new user.
* `POST /logout` – Logs out the current user.
* `GET /password/reset` – Displays the password reset request form.
* `POST /password/email` – Sends a password reset link.
* `POST /password/reset` – Resets the user’s password.

***

### Configuring Authentication

#### Modifying the `auth.php` Configuration

Maginium provides a configuration file at `config/auth.php` to customize the authentication behavior. Here’s an example:

```php
phpCopyEditreturn [
    'defaults' => [
        'guard' => 'web',
        'passwords' => 'users',
    ],
    'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],
    ],
    'providers' => [
        'users' => [
            'driver' => 'eloquent',
            'model' => App\Models\User::class,
        ],
    ],
    'passwords' => [
        'users' => [
            'provider' => 'users',
            'table' => 'password_resets',
            'expire' => 60,
        ],
    ],
];
```

#### Custom Guards and Providers

You can define custom guards and user providers to handle specific authentication scenarios, such as API authentication.

***

### Middleware for Authentication

Use the `auth` middleware to protect routes from unauthorized access. Example:

```php
phpCopyEditRoute::get('/dashboard', function () {
    return view('dashboard');
})->middleware('auth');
```

#### Redirecting Guests

To redirect unauthenticated users, apply the `guest` middleware:

```php
phpCopyEditRoute::get('/login', function () {
    return view('auth.login');
})->middleware('guest');
```

***

### Password Reset Functionality

Maginium's Auth Module includes built-in support for password resets:

1. Users request a password reset link by providing their email address.
2. A secure token is sent to the email, allowing users to reset their passwords.
3. The reset link redirects users to a form where they can set a new password.

#### Sending Password Reset Links

Ensure that your application is configured to send emails by updating the `.env` file:

```env
envCopyEditMAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="noreply@example.com"
MAIL_FROM_NAME="Maginium"
```

Run the following Artisan command to queue password reset emails:

```bash
bashCopyEditphp artisan queue:work
```

***

### Customizing Authentication

#### Modifying Views

Maginium provides default views for login, registration, and password reset. You can publish these views and customize them:

```bash
bashCopyEditphp artisan vendor:publish --tag=auth-views
```

#### Customizing Controllers

Override default controllers to implement custom logic. Example:

```php
phpCopyEditnamespace App\Http\Controllers\Auth;

use App\Http\Controllers\Controller;

class CustomLoginController extends Controller
{
    public function login(Request $request)
    {
        // Custom login logic
    }
}
```

***

### Securing Your Application

* **Encrypt User Data:** Use hashing for sensitive user information.
* **Rate Limiting:** Prevent brute force attacks with the `throttle` middleware.
* **Two-Factor Authentication (2FA):** Add 2FA for enhanced security.
* **Session Expiration:** Set short session lifetimes to minimize risk.

***

### Example: Protecting a Dashboard Route

Here’s an example of securing a dashboard route with authentication:

```php
phpCopyEditRoute::middleware('auth')->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'index']);
});
```

***

### Best Practices

* Use HTTPS to encrypt communication between clients and your server.
* Regularly update Maginium to the latest version for security patches.
* Limit access to sensitive routes using role-based authorization.

By leveraging Maginium’s Auth Module, you can build a secure and user-friendly authentication system tailored to your application’s needs.
