# Laravel
### What is the Facade Pattern used for?
Facades provide a static interface to classes that are available in the application's service container. Laravel facades serve as static proxies to underlying classes in the service container, providing the benefit of a terse, expressive syntax while maintaining more testability and flexibility than traditional static methods.
All of Laravel's facades are defined in the Illuminate\Support\Facades namespace. Consider:

```bash
use Illuminate\Support\Facades\Cache;

Route::get('/cache', function () {
    return Cache::get('key');
});
