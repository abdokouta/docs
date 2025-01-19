# Cart Module

The Cart Module in Maginium provides a flexible, efficient, and secure way to manage shopping carts in your application. Designed for scalability, it handles everything from simple cart operations to complex e-commerce workflows.

Whether you are building a small store or a large-scale platform, the Cart Module seamlessly integrates into your project, ensuring a smooth user experience.

***

### Key Features

* **Add and Remove Items:** Dynamically add or remove items to the cart with ease.
* **Item Quantity Management:** Adjust the quantity of items directly in the cart.
* **Session-Based Persistence:** Cart data is stored across user sessions.
* **Database Support:** Option to store cart information in your database for persistent carts.
* **Customisable:** Fully customisable to suit your application’s unique requirements.
* **Discounts and Coupons:** Apply discounts and coupons for promotions.
* **Tax and Shipping Calculations:** Automatically calculate applicable taxes and shipping costs.
* **Guest and Authenticated User Support:** Works seamlessly for both guest users and logged-in users.

***

### Getting Started

#### Installation

To enable the Cart Module in your Maginium application, ensure it is included in your configuration. Run the following command to install the required package:

```bash
composer require maginium/cart
```

Then, publish the configuration file:

```bash
php artisan vendor:publish --tag=cart-config
```

This command will publish a `cart.php` configuration file to your application’s `config` directory.

***

### Configuration

The `cart.php` file includes various options for customisation:

```php
return [
    'storage' => 'session', // Options: session, database
    'tax_rate' => 0.1, // Default tax rate (10%)
    'currency' => 'USD', // Default currency
];
```

* **Storage:** Defines where cart data will be stored. By default, it uses the session.
* **Tax Rate:** The default tax percentage applied to the cart.
* **Currency:** Sets the default currency for cart transactions.

***

### Using the Cart Module

#### Adding Items to the Cart

```php
use Maginium\Cart\Cart;

Cart::add([
    'id' => 101,
    'name' => 'Wireless Headphones',
    'quantity' => 1,
    'price' => 199.99,
    'attributes' => [
        'color' => 'black',
    ],
]);
```

#### Retrieving Cart Contents

```php
$cartContents = Cart::content();
foreach ($cartContents as $item) {
    echo $item->name . ' - ' . $item->quantity;
}
```

#### Updating Item Quantity

```php
Cart::update(101, [
    'quantity' => 2,
]);
```

#### Removing Items from the Cart

```php
Cart::remove(101);
```

***

### Handling Checkout

The Cart Module integrates with Maginium’s payment and order modules to facilitate a smooth checkout process. Use the following example to convert cart contents into an order:

```php
$order = Cart::checkout([
    'user_id' => Auth::id(),
    'payment_method' => 'credit_card',
    'shipping_address' => $address,
]);
```

#### Clearing the Cart

After a successful checkout, clear the cart:

```php
Cart::clear();
```

***

### Advanced Features

#### Persistent Cart for Authenticated Users

For authenticated users, you can enable persistent carts that save to the database:

```php
'middleware' => ['auth'],
```

#### Coupons and Discounts

```php
Cart::applyCoupon('SUMMER2025');
```

The `applyCoupon` method applies a discount to the cart, reducing the total price.

***

### Customisation

The Cart Module is fully extensible. Override core methods to add custom logic to your cart operations. For example, you can add a custom tax calculation:

```php
Cart::setTaxCalculation(function($subtotal) {
    return $subtotal * 0.15; // Custom 15% tax
});
```

***

### Best Practices

1. **Validate Inputs:** Always validate item data (ID, quantity, etc.) before adding it to the cart.
2. **Secure Data:** Use secure methods to store cart information, especially for guest users.
3. **Optimise Database Queries:** If using the database for storage, ensure queries are optimised for large-scale data.

***

### Conclusion

The Cart Module in Maginium is designed to simplify e-commerce development, providing robust features and flexibility. Start building powerful shopping experiences today with Maginium’s Cart Module.
