
# ONE_TO_MANY
- Prendiamo in considerazione deu tabelle Post e Category

1. Partire dal avere le due tabelle da mettere in riferimento, a livello di migration e Models

2. Mettiamo in relazione le due tabelle, creando due funzioni, da inserire nei relativi Model delle tabelle che andranno in relazione

3. Cerchiamo di capire, in basse al ragionamento che vogliamo fare, quale delle due tabelle avrà la Fk

4. La Fk va nella tabella con la parte della relazione MOLTI
    - Per esempio una categoria puo essere legata a molti Posts, quindi Posts sarà dalla parte Molti e avrà la FK

5. !IMPORTANTE. La funzione che creiamo deve avere il nome del modello di riferimento
    - Nel modello dalla parte 1, la funzione sara <br>
        ``` pubblic function nomeModelloDiCollegamentoAlPlurale(){} ```

    - Nel modello dalla parte N, la funzione sarà  <br>
        ```  pubblic function nomeModelloDiCollegamentoAlSingolare(){} ```

6. Il modello dalla parte della relazione MOLTI:

        public function category(){
            //Il post appartiene ad una sola cattegoria associata
            return $this->belongsTo('App\Models\Category');

            //Tabella dalla parte della N, Qui va la FK
        }

7. Il modello dalla parte della relazione ONE:

        public function posts(){
            
            // La categoria ha molti record associati
            return $this->hasMany('App\Models\Post');
        }

8. Creata la relazione, dobbiamo aggingere la colonna nuova della FK, nella tabella giusta (quella dalla parte di N) 

### Metodo 1
1. Andare a mano nella migration a cu devo aggiungere la colonna in piu per la Fk, e aggiungere la colonna, e lanciare il comando 
    ``` php artisan migrate:refresh ```

### Metodo 2
1. Creare una migration di Aggiornamento, sara una tabella cha andra ad aggiornare la tabella a cui vogliamo aggiungere la colonna

2. Con il ``` --table= ``` gli diciamo a quale tabella si riferira, quindi quale tabella aggiornerà <br>
    ``` php artisan make:migration update_addFKPosts_table --table=posts  ```

3. Compiliamo la migration di Aggiornamento
    
        public function up()
        {
            Schema::table('posts', function (Blueprint $table) {

                // Creiamo la colonna che avra la Fk
                $table->unsignedBigInteger('category_id')->nullable();

                // Creiamo il collegamento, la colonna foreign con quel nome, si riferisce al ID (on) nella tabella con quel nome, on delete set null
                $table->foreign('category_id')->references('id')->on('categories')->onDelete('set null');
            });
        }

4. SEMPRE compilare la funzione down() della tabella di aggiornamento, se si dimentica e si fa un refresh o qualcosa di simile non cancellerà mai la tabella

        public function down()
        {
            Schema::table('posts', function (Blueprint $table) {

                ///!  Elimino il collegamento con la tabella  nome tabella che uso per l'aggiornamento_colonna che ho aggiornato_legame foreign
                $table->dropForeign('posts_category_id_foreign');

                ///! Elimino la colonna stessa
                $table->dropColumn('category_id');
            });
        }

5. ``` php artisan:migrate ```

## Create
1. Nella funzione create del controller risorsa, andiamo a passare le informazioni della tabella in relazione, tramite il compact

2. Nella view create, creiamo una select a cui passiamo dinamicamente tutte le informazioni--Il name della select prende il nome della colonna, e la option deve avere il value che andra a riempire la colonna della fk

        <label>Categorie</label>
        <select name="category_id">
            <option value="">Seleziona la categoria</option>
            @foreach ($categories as $elem)
                <option value="{{ $elem['id'] }}">{{ $elem['name'] }}</option>
            @endforeach
        </select>

3. Contollare la store, ricordarsi di aggiornare la funzione fill (all'interno del model), se presente

4. 
5. IMPORTANTE !!  <br>
Ora nella tabella principale abbiamo la possibilita di stamparci a schermo l'id della fk, quindi l'id della tabella legata a Post, ma siccome all'utente non interessa vedere un id, ma un nome che è legato a quel id (in questo caso il nome della category table, legato al id di ogni riga), abbiamo l'ausilio di laravel, con una funzione specifica ALL() ci permette di estrappolare tutti i dati della tabella, ma nel caso in cui questa tabella abbiam una fk, come per i Posts hanno la fk di Category, laravel ci mette a disposizione una funzione with(), da usare al posto di all(), che prende tutte le informazioni della tabella principale, e in aggiunta, sottobanco, capisce che c'è un collegamento grazie alla fk, e si prende tutte le colonne della tabella associata.

6. Nella funzione index del controller risorsa: come parametro di with(), inserisco il nome della funzione che crea il legame, alll'interno del Model della tabella che ha il fk

        public function index()
        {
            $posts = Post::with('category')->paginate(10);
            return view('admin.post.index', compact('posts'));
        }

7. Nella pagina per stampare il valore name, associato alla fk, diventa un array dentro un array, A laravel non piace dover stampare una colonna se è <br>
    ``` <td>{{ $post['category']['name']?? ''}}</td> ```
8. Oppure:

        @if ($post->category)
            {{ $post['category']['name']}}
        @endif


## Edit
1. Nella function edit del controller risorsa, passare tutti i dati delle tabella in relazione

2. Nella view, creare la select come nella view create, identica

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

- Se l'id dell'elento che voglio stampare è uguale all'id presente dentro alla tabella con la fk, aggingo selected altrimenti niente

        @foreach ($categories as $elem)
            <option value="{{ $elem['id'] }}"  {{ $elem['id'] == old('category_id' , $post['category_id']) ? 'selected' : ''  }}>{{ $elem['name'] }}</option>
        @endforeach

4. Update è gia apposto cosi come è




















