# This is a simple note
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

### Custom Collection
Laravel's collection class is one of its most powerful features, providing a fluent, convenient wrapper for working with arrays of data. However, there might be cases where you want to extend the base collection class to include your custom methods or override existing ones. This can be particularly useful for encapsulating specific collection behaviors related to your application's domain logic.

### Custom Query Builder Class
Creating a custom query builder in Laravel allows you to extend the capabilities of the default Eloquent query builder with your own methods, providing a more fluent interface for specific database operations related to your application's domain logic. Here's how you can create and use a custom query builder in Laravel:

### What is the purpose of the Eloquent `cursor()` method in Laravel?

The Eloquent `cursor()` method in Laravel is intended for iterating through large datasets efficiently. It retrieves a `LazyCollection` instance that fetches one Eloquent model at a time from the database, using PHP generators. This approach greatly reduces memory usage when dealing with thousands or millions of records, as only the current model is held in memory during iteration, unlike `all()` or `get()` methods which load the entire result set at once.

# Scopes

Laravel scopes allow you to encapsulate query logic into methods on your Eloquent models. This can help keep your database queries clean and reusable across your application. Laravel provides two types of scopes: Local Scopes and Global Scopes.

## Local Scopes

Local scopes allow you to define query constraints that can be easily reused. They are defined directly on the Eloquent model and can be chained together with other query builder methods.

### Defining a Local Scope

You define a local scope by prefixing a method on the Eloquent model with `scope`. The scope method should accept an `\Illuminate\Database\Eloquent\Builder` instance as its first argument, along with any additional criteria:

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Builder;

class User extends Model
{
    /**
     * Scope a query to only include active users.
     *
     * @param \Illuminate\Database\Eloquent\Builder $query
     * @return \Illuminate\Database\Eloquent\Builder
     */
    public function scopeActive(Builder $query)
    {
        return $query->where('active', 1);
    }
}
```

## Global Scopes

Global Scopes allow you to add constraints to all queries for a specific model. This is particularly useful for enforcing a certain criterion across all queries for a model, like always filtering out soft-deleted records.

### Defining a Global Scope

To define a global scope, you'll create a class that implements the `Illuminate\Database\Eloquent\Scope` interface. This interface requires you to implement an `apply` method, which adds the scope to the given query builder instance.

```php
namespace App\Scopes;

use Illuminate\Database\Eloquent\Scope;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Builder;

class AgeScope implements Scope
{
    /**
     * Apply the scope to a given Eloquent query builder.
     *
     * @param \Illuminate\Database\Eloquent\Builder $builder
     * @param \Illuminate\Database\Eloquent\Model $model
     * @return void
     */
    public function apply(Builder $builder, Model $model)
    {
        $builder->where('age', '>', 20);
    }
}
```

# Middleware
Laravel Middleware provides a convenient mechanism for filtering HTTP requests entering your application. Middleware are essentially layers of code that can inspect and manipulate HTTP requests and responses. They can perform a range of functions, such as authenticating users, performing logging, CORS (Cross-Origin Resource Sharing), caching responses, and more. Middleware in Laravel is used to encapsulate this logic outside of your application's core, keeping your codebase clean and focusing on its primary responsibilities.

