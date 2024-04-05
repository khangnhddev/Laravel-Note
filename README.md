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


# Contracts
Laravel contracts are a set of interfaces that define the core services provided by the Laravel framework. These interfaces allow developers to define explicit dependencies for their classes, making it easier to understand the functionality being used. Furthermore, contracts serve as a method for loose coupling, enabling developers to use Laravel's features through interfaces rather than concrete implementations. This approach enhances the testability and flexibility of the application by allowing alternative implementations to be easily swapped without changing the contract.

Contracts are essentially a way to use Laravel's features in a more flexible and decoupled manner. They cover a wide range of functionalities, including caching, filesystem, queueing, and sending notifications, among others.

# Macros
Laravel's macroable feature allows for the runtime extension of the capabilities of various components within the framework, such as collections, responses, and the request object. This feature is incredibly useful for adding custom functionality to Laravel's classes without modifying their core code.

## How Macros Work
Macros are defined by calling the `macro` method on a macroable class with two arguments: the name of the macro and a Closure that encapsulates the macro's functionality. Once defined, these macros can be called as if they were native methods on the class.

### Defining a Macro
Here's how you can add a custom method to the `Illuminate\Support\Collection` class:

```php
use Illuminate\Support\Collection;

// Define a macro called 'toCustomFormat'
Collection::macro('toCustomFormat', function () {
    // Convert each item in the collection to a custom format...
    return $this->map(function ($item) {
        // Assume each item is an array with 'name' and 'value'
        return "{$item['name']}: {$item['value']}";
    })->implode(', ');
});

// Usage of the macro on a Collection instance
$collection = collect([
    ['name' => 'Apple', 'value' => 'Fruit'],
    ['name' => 'Carrot', 'value' => 'Vegetable'],
]);

echo $collection->toCustomFormat();
// Output: "Apple: Fruit, Carrot: Vegetable"
```

## Usage of Macros
Once a macro is defined, it can be utilized just like any regular method on the class, offering a flexible method for extending functionality.

## Macroable Classes in Laravel
Laravel enables several of its classes to be macroable, meaning you can extend them using macros. Commonly extended macroable classes include:

- `Illuminate\Support\Collection`
- `Illuminate\Http\Response`
- `Illuminate\Routing\Router`
- `Illuminate\Routing\UrlGenerator`

## Benefits and Caveats
While macros provide a potent tool for customizing Laravel's functionality, it's crucial to use them judiciously to prevent making your codebase hard to understand and maintain. They are best used for simple, reusable enhancements.

# API Resources
In the past, one of the first challenges we’d run into when developing APIs in Laravel was how to transform our data. The simplest APIs can just return Eloquent objects as JSON, but very quickly the needs of most APIs outgrow that structure. How should we convert our Eloquent results into the right format? What if we want to embed other resources or do so but only optionally, or add a computed field or hide some fields from APIs but not other JSON output? An API-specific transformer is the solution.

We now have access to a feature called Eloquent API resources, which are structures that define how to transform an Eloquent object (or a collection of Eloquent objects) of a given class to API results. For example, your Dog Eloquent model now has a Dog resource whose responsibility it is to translate each instance of Dog to the appropriate Dog-shaped API response object.

# API Authentication
Laravel provides two primary tools for authenticating API requests: Sanctum (most recommended) and Passport (powerful but very complex and usually overkill).

# Queues
Queues in programming are very similar. Your application adds a “job” to a queue, which is a chunk of code that tells the application how to perform a particular behavior. Then some other separate application structure, usually a “queue worker,” takes the responsibility for pulling jobs off of the queue one at a time and performing the appropriate behavior. Queue workers can delete the jobs, return them to the queue with a delay, or mark them as successfully processed.

Laravel makes it easy to serve your queues using Redis, beanstalkd, Amazon Simple Queue Service (SQS), or a database table. You can also choose the sync driver to have the jobs run right in your application without actually being queued, or the null driver for jobs to just be discarded; these two are usually used in local development or testing environments.
