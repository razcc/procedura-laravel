## Seeder che non viene eseguito, come se non esistesse
- problema terminale: Target class [apartments_seeder] does not exist.
- Soluzione: composer dump-autoload

## Come prendere i dati dell'utente loggato dalla view

        {{ Auth::user()->name }}

## Come prendere i dati dell'utente loggato dalla Controller

        $user = Auth::user();
        use Illuminate\Support\Facades\Auth;


php artisan storage:link  
















