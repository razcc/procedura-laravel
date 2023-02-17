# Iniziare progetto laravel 7 da zero

## GIT
1. ``` composer create-project --prefer-dist laravel/laravel:^7.0 [NOME PROGETTO] ```
2. Collegamento a GIT
    
        git add -A 
        git commit -m " Testo del commit " 
        git branch -M main
        git remote add origin .........URL DELLA REPO........
        git push -u origin main
    
## SASS
1. ``` npm i ```
2. ``` npm run dev ```
3. ``` npm run watch ```
   
4. Per gestire gli url delle immagini caricate in sass, modificare il file 📃 webpack.mix.js aggiungendo le options i1. questo modo:
    
        mix.js('resources/js/app.js', 'public/js')
            .sass('resources/sass/app.scss', 'public/css')
            .options({
            processCssUrls: false});

5. ``` php artisan serve ```


## Clonazione da GIT un Progetto Laravel Già avviato
1. Apriamo il progetto con VS Code
2. Creiamo dentro il progetto un nuovo file 📃 .env
3. Copiamo e incolliamo dentro il file 📃 .env il contenuto di .env.example

4. Apriamo il terminale nel progetto e lanciamo il comando:
    ``` composer install ``` <br>
     // Se escono errori passiamo al comando: <br>
     ``` composer update ``` 
5. ``` php artisan key:generate ```
6. ``` npm install ```
7. ``` php artisan serve ```


## Bootstrap
1. Lanciare il comando se non ancora fatto: 
    ```npm i
2. Poi:
    ```npm i npm install bootstrap
    ``` npm i @popperjs/core
3. Aprire il file 📃app.scss e inserire:
    ```
    @import '~bootstrap/dist/css/bootstrap.min.css';

4. Andare nel file app.js nella cartella resources e inserire:
    ```
    import '../../node_modules/@popperjs/core/dist/umd/popper.min.js';
    import 'bootstrap/js/dist/dropdown';

5. Lanciare da terminale il comando (per generare files css e js nella cartella public): 
    ``` npm run dev
6. Creare nella view del layout il collegamento ai file:

    ```
    <link rel="stylesheet" href=" {{ asset('css/app.css') }} ">
    <script src=" {{ asset('js/app.js') }} "></script>

7. Rilanciare d azero il comando da terminale: 
    ``` npm run watch
8. Usare le classi di bootstrap 5 nelle views


## Node Modules
1. ``` npm install


## Route List 
1. ```  php artisan route:list


## MODEL
1. Deve essere scritto in PascalCase e al singolare e deve essere la versione singolare del nome della tabella del DB in inglese
2. ``` php artisan make:model Models/NomeModello


## CONTROLLER
1. Deve essere scritto in PascalCase e al singolare
2. ``` php artisan make:controller NomeController

## Controller RISORSA
1. ``` php artisan make:controller PastaController --resource
2. scrittura in web.php di un controller risorsa
    ``` Route::resource('/pastas', PastaController::class);


## QUERY
1. ALL:

    ```
    public function index(){

        // 'select * from books'
        $var = NomeModello::all();

        return view('welcome', compact('var') );
    }

2. WHERE:

    ```
    public function index(){

        //filtraggio elementi
        $valori_filtrati = NomeModello::where('title', 'Like', 'L%')->get();

        return view('welcome', compact('valori_filtrati') );
    }


## ACTIVE MENU
1. Nelle nav bisogna realizzare un ternario che legge il "name" delle diverse rotte associate alle voci del menu:
    ```
    <header>
            <ul>
                <li>
                    <a class="{{ Request::route()->getName() == 'all_books' ? 'active' : '' }}" href="{{route('all_books')}}">Home</a>
                </li>
                <li>
                    <a class="{{ Request::route()->getName() == 'about' ? 'active' : '' }}" href="{{route('about')}}">About</a>
                </li>
            </ul>
    </header>


## SHOW BASE (NO Crud, NO Controller)
1. Creo un rotta in web.php dedicata:
    ```
    Route::get('/prodotti/{key}', function($key){

        //Prendo i dati da config
        $pasta = config('pasta');

        if( is_numeric($key) && $key >= 0 && $key < count($pasta) ){
            $prodotto_singolo = $pasta[$key];
        } else {
            abort(404);
        }

        // dd($prodotto_singolo);

        return view('pages.show', compact('prodotto_singolo'));
    })->name('show.pasta');

2. Per ogni elemento stampato dal ciclo foreach, creo un link che richiami la rotta della singola pagina e che passi il dato univoco che permetterà di recuperare il record:
    ```
    <a href="{{ route('show.pasta', compact('key') ) }}">

3. Creo la view che stamperà i dati del singolo record creato: "views/pages/show.blade.php"
    ```
    @extends('layouts.app')

    @section('page-title', "Title")

    @section('main-content')
        <h2>Prodotto: {{ $prodotto_singolo['titolo'] }}</h2>
    @endsection



## MIGRATION
1. ``` php artisan make:migration create_nomeTabellaPlurale_table (nome tabella deve essere in minuscolo)
2. Compilo il file generato nella cartella 📁database>migrations con le rispettive colonne di cui avrò bisogno al suo interno,
   facendo attenzione al tipo di dato che sceglierò di utilizzare in fase di riempimento
3. Terminata la compilazione lancio il comando:
    ``` php artisan migrate

4. Tornare indietro di un passaggio con le migration
    ``` php artisan migrate:rollback

5. Ricarico da zero tutte le migration svuotandole
    ``` php artisan migrate:refresh


## SEEDER
1. ``` php artisan make:seeder NomeSeeder
2. Compila la funzione interna "run" con ad esempio un array multidimensionale che ciclerò per creare diverse istanze in base a quanti dati fittizi ho creato all'interno dell'array multidimensionale

3. Lanciare il seeder
    ``` php artisan db:seed --class=HousesTableSeeder
    ``` Metodo 2: Compilo il file DatabaseSeeder con il nome del file seeder che ho creato
        e compilato e poi lancio il comando da terminale: 
    ``` php artisan db:seed


## SHORTCUT
1. Creazione model, controller risorsa, migration, seeder con shortcut
    ``` php artisan make:model Models/NomeModello -msr

2. Creazione model, controller normale, migration, seeder con shortcut
    ``` php artisan make:model Models/Pasta -msc



## Yeld, @section, Extends
1. Extends, serve quando si vogliono usare pezzi di codice come i componenti, fatti e finiti che saranno tutti uguali in ognu pagina dove li importo con Extends

2. YELD, viene usato in specifiche sezioni del LAYOUT, cosi che quando si richiama quel layoutsi possa modificare dinamicamente quella parte di yeld, e farla diversa ogni volta che si richiama quel layout
    ``` @yield('nomeYeld')

3. SECTION serve a richiamare gli yeld dinamici
    ```
    @section('nomeYeld')
        Contenuto

    @endsection