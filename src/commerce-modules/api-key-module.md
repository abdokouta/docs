# API Key Module

Maginium’s **API Key Module** provides a secure and flexible way to authenticate requests to your application’s API. This module is designed to ensure that only authorized clients can access your API endpoints. Follow this guide to configure and use API Key-based authentication in your Maginium-powered application.

***

### Features of the API Key Module

* **Secure Access Control:** Ensures only authorized clients can access APIs.
* **Key Management:** Generate, revoke, and manage API keys.
* **Rate Limiting:** Protect APIs from abuse by setting usage limits.
* **Custom Scopes:** Define fine-grained permissions for API keys.
* **Logging and Monitoring:** Track API key usage and detect anomalies.

***

### Setting Up the API Key Module

#### Step 1: Enable the API Key Module

Install the necessary components for API Key authentication using the Artisan command:

```bash
php artisan api-key:install
```

This will scaffold the required files and configurations for the module.

#### Step 2: Migrate the Database

Run the following migration command to create the `api_keys` table:

```bash
php artisan migrate
```

The `api_keys` table will store information about each API key, including the associated user, scopes, and expiration.

***

### Generating API Keys

Maginium provides a simple command to create new API keys:

```bash
php artisan api-key:generate --user=1 --scopes="read,write" --expires="2025-12-31"
```

#### Parameters:

* `--user`: The ID of the user the key is associated with.
* `--scopes`: A comma-separated list of scopes.
* `--expires`: Expiration date of the key (optional).

Example output:

```
API Key: abcd1234-ef56-7890-gh12-ijklmnopqrst
```

Store this key securely, as it won’t be shown again.

***

### Authenticating API Requests

Clients must include the API key in the request headers for authentication:

```http
GET /api/resource HTTP/1.1
Host: api.example.com
Authorization: Bearer abcd1234-ef56-7890-gh12-ijklmnopqrst
```

#### Middleware for API Key Validation

Protect API routes by applying the `api.key` middleware:

```php
Route::middleware('api.key')->group(function () {
    Route::get('/resource', [ResourceController::class, 'index']);
});
```

The `api.key` middleware will verify the provided key, including its validity, scopes, and expiration.

***

### Managing API Keys

#### Revoking API Keys

To revoke an API key, use the following command:

```bash
php artisan api-key:revoke abcd1234-ef56-7890-gh12-ijklmnopqrst
```

Revoked keys will no longer be valid for authentication.

#### Listing All API Keys

View all active API keys with:

```bash
php artisan api-key:list
```

Example output:

```
+----+--------------------------------------+--------+------------+---------+
| ID | Key                                  | User   | Scopes     | Expires |
+----+--------------------------------------+--------+------------+---------+
| 1  | abcd1234-ef56-7890-gh12-ijklmnopqrst | User 1 | read,write | 2025-12-31 |
+----+--------------------------------------+--------+------------+---------+
```

***

### Configuring API Key Settings

Edit the `config/api-key.php` file to customize settings like expiration defaults and key length. Example configuration:

```php
return [
    'key_length' => 32,
    'default_scopes' => ['read'],
    'expiration_days' => 30,
];
```

***

### Scopes and Permissions

Scopes allow fine-grained access control for API keys. Define available scopes in the `api-key` configuration file:

```php
return [
    'available_scopes' => [
        'read' => 'Read access to resources',
        'write' => 'Write access to resources',
        'delete' => 'Delete resources',
    ],
];
```

When generating API keys, assign scopes to limit their permissions. For example, a `read`-only key cannot modify resources.

***

### Logging and Monitoring

Track API key usage with built-in logging. Logs include details like:

* Key used for the request
* Endpoint accessed
* Timestamp

Example log entry:

```
[2025-01-04 12:34:56] INFO: API key "abcd1234-ef56-7890-gh12-ijklmnopqrst" accessed endpoint "/api/resource".
```

Use this data to monitor key usage patterns and detect unauthorized access.

***

### Best Practices for API Key Security

> **Important:** Always follow these best practices to secure your API keys:
>
> * **Use HTTPS** to encrypt communication.
> * **Regenerate keys periodically** and revoke old ones.
> * **Restrict scopes** to the minimum required for each key.
> * **Monitor usage** for anomalies.
> * **Store keys securely** and avoid exposing them in logs or URLs.

***

By implementing Maginium’s API Key Module, you can ensure secure and controlled access to your API endpoints, providing peace of mind for your application and its users.
