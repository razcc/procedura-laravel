## Inizio Nuovo progetto

1. ``` composer create-project --prefer-dist laravel/laravel:^7.0 "Nome progetto" ```

2. ``` composer require laravel/ui:^2.4 ```

3. ``` php artisan ui vue --auth ```

4. ``` npm i ```

5. Modifichiamo il file 📁.env

6. ``` php -S localhost:8000 -t public ```

7. Su un altro terminale: 
    ``` npm run dev && npm run watch ```

8. Cancello il HomeController creato in automatico e creo il nuovo controller Admin/HomeController per le pagine di backOffice:
    ``` php artisan make:controller Admin/HomeController ```

9.  Modifico web.php con il gruppo rotte per il backOffice e le rotte per il frontOffice
    
        Route::middleware('auth')->namespace('Admin')->prefix('admin')->name('admin.')->group(function () {
            Route::get('/', 'HomeController@index')->name('index');
            Route::resource('/posts', 'PostController');
        });

            //Gestione rotte FRONT end
         Route::get('{any?}', function(){
            return view('guest.home');
        })->where("any", ".*");

10. Modifico il file: 📁RouteServiceProvider.php in: public const HOME = '/admin';

11. Modifico il nuovo home controller, percorso 📁Admin/HomeController, in:

    ```
    class HomeController extends Controller
    {
        // pagina di atterraggio dopo il login
        public function index()
        {
            return view('admin.home');
        }
    }

12. Creiamo il file home.blade per la pagina di frontoffice in questo percorso: 📁"resources/views/guest/home.blade.php"
    struttura tipo:
    ```
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Guest Home</title>
    </head>

    <body>
        <!-- inseriamo un unico div #root a cui si aggancerà vue -->
        <div id="root"> </div>

        <!-- colleghiamo il file js -->
        <script src="{{ asset('js/app.js') }}" charset="utf-8"></script>
    </body>

    </html>

13. Creiamo la views App.vue nel percorso: 📁"resources/js/views/App.vue" 

    struttura tipo:
    ```
    <template>
        <div>

        </div>
    </template>

    <script>
        export default {
            name:"App",
        }
    </script>

    <style lang="scss" scoped>

    </style>

14. Attiviamo il rendering del file App.vue in questo percorso: 📁"resources/js/app.js"
    struttura tipo:
    ```
    require('./bootstrap');
    window.Vue = require('vue');

    // importiamo il componente App
    import App from './views/App';

    const app = new Vue({
        el: '#root',
        render: h => h(App), // renderizziamo App all'avvio di Vue
    });

15. Da qui in poi possiamo tornare a dedicarci al backend come abbiamo sempre fatto e sucessivamente gestire qua le pagine
che non richiedono il login, con la solita struttura a componenti di vue

