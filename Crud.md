## CRUD

1. Per gestire le Crud ci serve un Controller Risorsa, all'interno di 📁app/Http/Controllers/Admin/ControllerRisorsa.php
    ``` php artisan make:controller Admin/PostsController -r  
2. In 📃web.php, aggiungere la Route per il controller risorsa, dentro al gruppo rotte degli Admin:
    ```
    Route::middleware('auth')->namespace('Admin')->prefix('admin')->name('admin.')->group(function () {
        Route::get('/', 'HomeController@index')->name('index');

        //CONTROLLER RISORSA
        Route::resource('/posts', postsController::class); 
    });
3. Creare la strutturain cartelle nelle views 📁
    ```
    resources/views/admin/post/ index.blade.php
                                show.blade.php
                                create.blade.php
                                edit.blade.php
4. Lanciare i comandi per creare il Model migration e seeder

5. Creare la struttura della tabella nella Migration

6. Se si vuole rieomire la tabaella nel DB, andare nell seeder, nella function run.
-Se si vogliono dati veloci e limitati in numero creiamo un array multidimensionale direttamente dentro la funzione run

    public function run()
    {
        $var = [
            'elem1',
            'elem2'
        ];

        foreach($var as $elem){
            $newVar = new NomeModel();
            $newVar->NomeColonnaTabella = $elem;
            $newVar->NomeColonnaTabella = $elem;
            $newVar->save();
        }
    }
-Se si vogliono gestire degli array con una gran quantità di dati si puo decidere di inserire questo array dentro un file .php all'interno della cartella config per poi richiamare i dati con una funzione specifica chiamata config
    ```
    public function run()
    {
        $var = config('fileDati');

        foreach($var as $elem){
            $newVar = new NomeModel();
            $newVar->NomeColonnaTabella = $elem['chiaveArray'];
            $newVar->NomeColonnaTabella = $elem['chiaveArray'];
            $newVar->save();
        };
    }
-Ultimo metodo su puo usare FAKER php, per chreare dati fittizi nella tabella

7. ``` php  artisan db:seed --class=Nomeseeder


## FAKER PHP
1. ``` composer remove fzaninotto/faker 
2. ``` composer require fakerphp/faker  
3. Nel file seed su cui si vuole usare il faker, usare l'importazione 
    ```use Faker\Generator as Faker;
4. Importare il modello
    ``` use App\Models\NomeModello;
5. Usare la seguente funzione run
    ```
    public function run(Faker $faker)
    {
        for ($i = 0; $i < 15; $i++){
            $newPost = new nomdeModello();
            $newPost->NomeColonna = $faker->name();
            $newPost->NomeColonna = $faker->paragraph();
            $newPost->save();
        }
    }

6. ``` php  artisan db:seed --class=Nomeseeder

## INDEX
1. Inserire nella homepage o navBar il link che porta alla pagina index, la route da inserire la si trova nella route:list
    ``` <a href="{{ route('admin.posts.index') }}">Posts</a>

2. Creare la struttura del index.blade.php, estendendo il layut
3. Andare nel Controller Risorse, nella funzione index, e creare la logica per passare i dati da ciclare alla pagina
e fare il return diella view index
    ```
    public function index()
    {
        $posts = Post::All();
        return view('admin.post.index', compact('posts'));
    }

4. Andare nella pagina index.blade.php e creare il ciclo per stapare a schermo tutti i dati ricevitu tramite il compact dal controller


## SHOW
1. Creare il link sul singol elemto, quindi dentro al ciclo,  nella pagina index, nella route del href, inserire, oltre al name indicatio della pagina show (riferirsi alla route.list), anche l'id del singolo elemto su cui si mette il link
    ```
    <a href=" {{ route('admin.posts.show', $post['id']) }}">
        {{ $post['title'] }}
    </a>
2. Nella Function Show del Controller Risorsa, riportarsi l'intero recordo col modello, associato alla funzione findOrFail($id), poi fare il return della pagina show, e il compact del singolo elemento salvato
    ```
    public function show($id)
    {

        $singolo_post = Post::findorFail($id);
        
        return view('admin.post.show', compact('singolo_post'));
    }
3. Nella pagina show stampare il dato ricevuto dal Controller Risorsa









