
### What is the Facade Pattern used for?
Facades provide a static interface to classes that are available in the application's service container. Laravel facades serve as static proxies to underlying classes in the service container, providing the benefit of a terse, expressive syntax while maintaining more testability and flexibility than traditional static methods.
All of Laravel's facades are defined in the Illuminate\Support\Facades namespace. Consider:

```bash
use Illuminate\Support\Facades\Cache;

Route::get('/cache', function () {
    return Cache::get('key');
});

```

### What are Laravel events?
In Laravel, events are a way of decoupling various parts of your application by using a simple observer implementation. Events serve as a means to trigger actions in your application. When a specific event occurs, Laravel allows you to hook into that event and run associated listeners. These listeners are responsible for executing the code in response to the event being fired.

Events can be used for a wide range of purposes, such as sending notifications, changing the state of an application, or handling business logic after a particular action is performed.

### How do you do soft deletes?

Laravel's soft delete is a feature that allows you to "delete" records from your database without actually removing them. When models are soft deleted, they are not permanently removed from your database. Instead, a deleted_at column in the database table is set to the current timestamp, indicating the model has been deleted. Queries retrieving the models automatically ignore these soft deleted records, making them invisible to your application until you explicitly ask for them.
