
??Chiedere stampa del with collegamento Post category, perchè ho docuto aggiungere ?? ''
??Chiedere come vanno scritti i nomi nella tabella di aggiornamento
?? Chiedere per il collegamento con user table, come avere l'id del user loggato, e stamparlo in post, nella colonna creata
## ONE_TO_MANY
Prendiamo in considerazione deu tabelle Post e Category
1. Partire dal avere le due tabelle da mettere in riferimento

2. Mettiamo in relazione le due tabelle, creiamo due funzione, da inserire nei relativi Model delle tabelle che andranno in relazione
3. Cerchiamo di capire, in basse al ragionamento che vogliamo fare, quale delle due tabelle avrà la Fk, quindi quale avra relazione UNA e quale MOLTI
4. La Fk va nella tabella con la parte della relazione MOLTI
per esempio una categoria puo essere legata a molti Posts, quindi posts sarà dalla parte Molti e avrà la fk
5. IMPORTANTE. La funzione che creiamo deve avere il nome del modello di riferimento,
Nel modello Post, la funzione sara <!-- pubblic function category(){}--> ... nel modello Category la funzione sarà <!--  pubblic function post(){} -->

6. Il modello dalla parte della relazione molti:
    ```
    // Funzione di relazione
    public function category(){
        //Il post ha una appartiene ad una sola cattegoria associata
        return $this->belongsTo('App\Models\Category');
    }

7. Il modello dalla parte della relazione ONE:
    ```
    // Funzione di relazione
    public function post(){
        
        //La categoria ha molti record associati
        return $this->hasMany('App\Models\Post');
    }

8. Fatto questo dobbiamo capire che nella tabella a cui ora si aggiungerà la FK, in questo caso Posts, va aggiunta una colonna <!--2Metodi-->
1. Andare a mano nella migration a cu devo aggiungere la colonna in piu per la Fk, e aggiungere la colonna, e lanciare il comando <!-- php artisan migrate:refresh -->
2. Creare una tabella nuova di Aggiornamento, sara una tabella cha andra ad aggiornare la tabella a cui vogliamo aggiungere la colonna

<!--! Creare una tabella di AGGIORNAMENTO per aggiungere la fk alla tabella originale -->
1. Lanciare il comando per creare una migration aggiornamento: con il --table= gli diciamo a quale tabella si riferira, quindi quale aggiornerà
    ``` php artisan make:migration update_addFKPosts_table --table=posts 

2. Compiliamo la migration di Aggiornamento
    ```
    public function up()
    {
        Schema::table('posts', function (Blueprint $table) {
            <!--? Creiamo la colonna che avra la Fk, -->
            $table->unsignedBigInteger('category_id')->nullable();
            <!--? Diciamo che la tabella che si aggiornera, in questo caso dei post, ha la colonna appena creata come fk, e ha un collegamento di riferimetno con la colonna id della tabella categories -->
            $table->foreign('category_id')->references('id')->on('categories')->onDelete('set null');
        });
    }
3. Devo aggiornare la funzione down() della tabella di aggiornamento, se si dimenti e si fa un refresh o qualcosa di simile non cancellerà mai la tabella
    ```
    public function down()
    {
        Schema::table('posts', function (Blueprint $table) {
            <!--? Elimino il collegamento con la tabella  -->
            $table->dropForeign('posts_category_id_foreign');

            <!--? Elimino la colonna stessa  -->
            $table->dropColumn('category_id');
        });
    }
9. php artisan:migrate

## Create e Edit
## Create
1. Alla creazione di un post e alla sua modifica, l'utente deve essere in grado di associargli una categoria, quindi dobbiamo passargli l'informazione di un'altra tabella e non solo dei post



2. Alla create nella function del controller risorsa, andiamo a passare le informazioni di categories, tramite il compact
3. Nella view create creiamo una select a cui passiamo dinamicamente tutte le cattegorie
Il name della select prende il nome della colonna, e la option deve avere il value che andra a riempire la colonna della fk
    
    ```
    <label>Categorie</label>
    <select name="category_id">
        <option value="">Seleziona la categoria</option>
        @foreach ($categories as $elem)
            <option value="{{ $elem['id'] }}">{{ $elem['name'] }}</option>
        @endforeach
    </select>

4. Devo aggirnare la store
5. Nella function store, ricordarsi di aggiornare la funzione fill (all'interno del model)

6. IMPORTANTE !! 
7. Ora nella tabella principale abbiamo la possibilita di stamparci a schermo l'id della fk, quindi l'id della tabella legata a Post, ma siccome all'utente non interessa vedere un id, ma un nome che è legato a quel id (in questo caso il nome della category table, legato al id di ogni riga), abbiamo l'ausilio di laravel, con una funzione specifica ALL() ci permette di estrappolare tutti i dati della tabella, ma nel caso in cui questa tabella abbiam una fk, come per i Posts hanno la fk di Category, laravel ci mette a disposizione una funzione with(), da usare al posto di all(), che prende tutte le informazioni della tabella principale, e in aggiunta, sottobanco, capisce che c'è un collegamento grazie alla fk, e si prende tutte le colonne della tabella associata.

8. Nella funzione index del controller risorsa: come parametro di with(), inserisco il nome della funzione che crea il legame, alll'interno del Model della tabella che ha il fk
    ```
    public function index()
    {
        $posts = Post::with('category')->paginate(10);

        return view('admin.post.index', compact('posts'));
    }

9. Nella pagina per stampare il valore name, associato alla fk, diventa un array dentro un array, A laravel non piace dover stampare una colonna se è Null e si hanno problemi, allora aggiungere ?? '' ala fine
    ```
    <td>{{ $post['category']['name']?? ''}}</td>
Oppure alternativa 2, giocare con l'if, se esiste quell'array all'interno allora stampa il suo valore se è null non fa niente
    
    @if ($post->category)
        {{ $post['category']['name']}}
    @endif



## Edit
1. Nella function edit del controller risorsa, passare tutti i dati delle category, che devo aggiungere alla tabela posts

2. Nella view creare la select come nella create, identica
    
    <div class="mb-3">
        <label>Categorie</label>
        <select class="form-controll" name="category_id">
            <option value="">Seleziona la categoria</option>

            @foreach ($categories as $elem)
                <option value="{{ $elem['id'] }}">{{ $elem['name'] }}</option>
            @endforeach
        </select>
    </div>

3. Per avere nella select gia selezionata, l'opzione originale, quindi quella che gia aveva selezionato in precedenza e che ora noi vogliamo modificare , si usa un ternario
    Se l'id dell'elento che voglio stampare è uguale all'id presente dentro alla tabella con la fk, aggingo selected altrimenti niente
    ```
    @foreach ($categories as $elem)
        <option value="{{ $elem['id'] }}"  {{ $elem['id'] == old('category_id' , $post['category_id']) ? 'selected' : ''  }}>{{ $elem['name'] }}</option>
    @endforeach

3. Update è gia apposto




















