# 📦 Event

Maginium's event system provides a robust way to decouple various parts of your application by allowing you to listen for and dispatch events. Events
provide a flexible mechanism to execute code based on specific actions or occurrences within the application.

---

### Defining Events

To define a new event in Maginium, create a class that implements the `Maginium\Framework\Contracts\Event` interface. This class serves as the
blueprint for your event.

\### Example: Creating an Event

```php
namespace App\Events;

use Maginium\Framework\Contracts\Event;

class UserRegistered implements Event
{
    public $user;

    public function __construct($user)
    {
        $this->user = $user;
    }
}
```

> **Important**: Each event should encapsulate all the data required to handle the occurrence effectively.

---

### Dispatching Events

Events can be dispatched using the `dispatch` helper function or the `Maginium\Framework\Events\Dispatcher` service. Dispatching an event triggers all
registered listeners for that event.

\### Example: Dispatching an Event

```php
use App\Events\UserRegistered;

dispatch(new UserRegistered($user));
```

{% hint style="info" %} Dispatching events allows other parts of your application to react to changes or actions without tightly coupling them.
{% endhint %}

---

### Event Listeners

Listeners respond to dispatched events and execute specific logic. To define a listener, create a class that implements the
`Maginium\Framework\Contracts\Listener` interface.

\### Example: Creating a Listener

```php
namespace App\Listeners;

use App\Events\UserRegistered;

class SendWelcomeEmail
{
    public function handle(UserRegistered $event)
    {
        // Send email to $event->user
        mail($event->user->email, "Welcome!", "Thank you for registering.");
    }
}
```

---

### Registering Events and Listeners

Events and their listeners can be registered in the `App\Providers\EventServiceProvider`. This class maps events to their respective listeners.

\### Example: EventServiceProvider

```php
namespace App\Providers;

use Maginium\Framework\Providers\EventServiceProvider as ServiceProvider;

class EventServiceProvider extends ServiceProvider
{
    protected $listen = [
        \App\Events\UserRegistered::class => [
            \App\Listeners\SendWelcomeEmail::class,
        ],
    ];
}
```

> **Tip**: Use the `listen` property to easily manage event-listener relationships.

---

### Event Subscribers

For more complex scenarios, you can use event subscribers. Subscribers are classes that can subscribe to multiple events, centralizing the logic in a
single location.

\### Example: Creating an Event Subscriber

```php
namespace App\Subscribers;

use Maginium\Framework\Contracts\Subscriber;

class UserEventSubscriber implements Subscriber
{
    public function subscribe($events)
    {
        $events->listen(
            \App\Events\UserRegistered::class,
            [\App\Listeners\SendWelcomeEmail::class, 'handle']
        );

        $events->listen(
            \App\Events\UserDeleted::class,
            [\App\Listeners\RemoveUserData::class, 'handle']
        );
    }
}
```

{% hint style="info" %} Subscribers simplify the management of multiple event-listener mappings. {% endhint %}

---

### Queued Listeners

Listeners can be queued to improve performance by delaying execution until a later time. To enable this, mark the listener class with the
`ShouldQueue` interface.

\### Example: Queued Listener

```php
namespace App\Listeners;

use Maginium\Framework\Contracts\ShouldQueue;

class SendNotification implements ShouldQueue
{
    public function handle($event)
    {
        // Perform queued operations
    }
}
```

> **Important**: Queued listeners require the queue system to be properly configured in Maginium.

---
