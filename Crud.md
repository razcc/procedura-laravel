DA CHIEDERE A COSA SERVED il with nel update function durante il redirect

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

## PAGINATE
1. Nella function index del controller risorsa
    ```
    public function index()
    {
        $posts = Post::paginate(10);

        return view('admin.post.index', compact('posts'));
    }
2. Nella pagina view dovre avro la pagination in fondo a tutto il contentuo che sara inpaginato:
    ``` {{ $nomeVariabilePassataConCompact->links() }}


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

## CREATE
1. Creare link per andare alla pagine di creazione post
    ``` <a href="{{ route('admin.posts.create') }}">Create Post</a>

2. Andare nella view create.blade.php, estendere il layoute e creare un Form per il riempimento dei dati da parte dell'utente per creare un buovo post
    a- Da inserire nel tag form: <!--!    method="POST" action="{{route('admin.posts.store')}}"     -->    
    b- Primo elemento suito dopo l'apertura del tag form: <!--!   @csrf     -->
    C- Ogni input deve avere l'attributo name che corrispode al nome della colonna che andra a riempire nel Db:           <!--!  name="NomeColonna" -->
3. Nel controller risorse, nella function create, fare la return della view create
4. Compilare la SOTORE

## STORE
1. Nel controller risorsa, fuunction store, Salvare in una variabile tutti i valori inviati dal form, tramite parameto request della funzione store, abbinaa a funzione propria di laravel all();
    ```
    public function store(Request $request)
    {
        $var = $request->all();
    }

2. Metodo1:     Creo nuovo record, e compilo una per una le colonne della tabella, direttamente nella function store.
    ```
    public function store(Request $request)
    {
        $var = $request->all();

        $new_record = new Post();

        $new_record->NomeColonna = $var['nomeColonnaPresaDalnameDelTaginputPassatoDallaRquestSopraAllaVar'];
        $new_record->NomeColonna = $var['nomeColonnaPresaDalnameDelTaginputPassatoDallaRquestSopraAllaVar'];
        $new_record->save();
    }

3. Metodo 2:    Creo nuovo record, e uso la funzione fill con parametro la variabile con i dati. Questo è possibile se prima vado nel modello che controlla questa specifica tabella su cui sto lavorando e gli aggiungo il protected fillable: 
    
    ```
    <!--!  Espressione da mettere nel modello:               -->
             protected $fillable = [
                'NomeColonna1',
                'NomeColonna2'
            ];
    <!--! Espressione da mettere nel controllere store:     -->
            public function store(Request $request)
            {
                $var = $request->all();

                $new_record = new Post();
                $new_record->fill($var);
                $new_record->save();
            }

4. FAccio il redirect, quando l'utente clicca submit, non andra ad una efetiva pagina store, ma verra diretto a una pagina a mia scelta per esempio la index con tutti i post
    ``` return redirect()->route('admin.posts.index');

## VALIDATION (durante la fase di creazione)
1. Nella function store, subito dopo la request all, quindi dopo che ho i dati:
    ```
    $request->validate(
    [
        'title' => 'required|max:50',
        'description' => 'required|max:250'
    ],
    //IN QUESTA PARTE SI POSSONO DARE MESAGGI PERSONALIZZATI IN BASE ALLA VALIDAZIONE => .required -- .max -- etc...
    [
        'title.required' => 'Attenzione il campo title è obbligatorio',
        'title.max' => 'Attenzione il campo non deve superare i 50 caratteri',
        'description.max' => 'Non si possono avere piu di 250 caratteri'
    ]
    );
2. Nwlla pagina di creazione 3 metodi
-Metodo 1: Sopra a tutto facciamo comparire un messaggio, con tutti i mesaggi di errori:
Inseriamo questo prima del form
    
    @if ( $errors->any() )
    <div class="alert alert-danger my-3">
        <ul>
            @foreach ($errors->all() as $error )
               <li>{{$error}}</li>
            @endforeach
        </ul>
    </div>
    @endif

-Metodo 2: Sotto ogni campo input, facciamo comparire il mesaggio di errore specifico
Sotto all'input inserisco:

    <input name="title" type="string" class="form-control">
    @error('nameInoutDiRiferimento')
        <div class="alert alert-danger">{{ $message }}</div>
    @enderror

-Metodo 3: Faccio diventare rosso i contorni dell'input e faccio comparire il meaggi di errore dentro all'input stesso
Da inserire dentro alla classe dell'input stesso:

    <input name="title" type="string" class="form-control @error('title') is-invalid @enderror">


## EDIT
1. Dove si vuole, creare un link con href che porta il name della funzione relativa all'edit. Passare anche l'id specifico di riferimento dell'elemento che si intende editare

2. Nella function edit, del controller Risorsa, salvarsi in una variabile il dato tramite il findOrfail fare il return dell view edit.blade.php
    ```
    public function edit($id)
    {
        $post = NomeModello::findOrFail($id);

        return view('admin.post.edit', compact('post'));
    }
3. Nella view edit.blade.php, estendere il layout e creare un Form, ch avra gli input precompilati dai valori preesistenti nel dataBase
4. Il form deve avere i seguenti attributi, la action porta alla rotta aupdate, e gli passiamo l'id per sapere cosa aggiornare
    ``` <form method="POST" action="{{ route('admin.posts.update', $post['id']) }}">
5. Subito dopo l'appertura del tag form
    ```
    @csrf
    @method('PUT')

6. Nella Function Update, ci salviamo i valori inviati dal form edit, tramite il paramentro $request
    ```
    public function update(Request $request, $id)
    {
        $data = $request->all();
    }

7. Ci salviamo in una variabile i dati del singolo post originali che stiamo editando.
    ```
    public function update(Request $request, $id)
    {
        $data = $request->all();
        $singolo_post = Post::findOrFail($id);
    }

8. Subito dopo inseriamola validation, come per la create:
    ```
    public function update(Request $request, $id)
    {
        $data = $request->all();
        $singolo_post = Post::findOrFail($id);
        $request->validate(
            [
                'title' => 'required|max:50',
                'description' => 'required|max:250'
            ],
            [
                'title.max' => 'Attenzione il campo non deve superare i 50 caratteri',
                'description.max' => 'Non si possono avere piu di 250 caratteri'
            ]
        );
    }

7. Fare l'update dei dati con la funzione specifica update, come paramentro la var con i dati del form della edit, subito sotto la validation:
    ``` $singolo_post->update($data);
8. Faccio il redirect subito dopo:
    ```return redirect()->route('admin.posts.index');

## DESTROY
1. Creare il link passando oltre la rotta anche l'id dell'elemento specifico che voglio cancellare definitivamente. Si deve fare un form, con dentro un Button submit, metodo post, e gli passiamo la route destroy con l'id specifico dell'elemento
    ```
    <form method="POST" action="{{ route('admin.posts.destroy', $post['id']) }}">
        @csrf
        @method('DELETE')
        <button type="submit">Destroy</button>
    </form>

2. Nella functiondestroy, salviamo i dati dell'elemento specifico tramite il findOrfail, poi usiamo la funzione specifica delete, per cancellare il dato, e poi facciamo il redirect
    ```
    public function destroy($id)
    {
        $data = NomeModello::findOrFail($id);
        $data->delete();
        return redirect()->route('admin.posts.index');
    }

2. 



































