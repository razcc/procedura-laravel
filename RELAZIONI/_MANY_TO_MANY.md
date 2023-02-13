## Many To Many
1. Partire con le due migration gia relalizzate e i due Model pronti

2. Mettiamo in relazione le due tabelle tramite la creazione di due funzioni, all'interno dei Model

3. Ricordarsi che i nomi delle funzioni devono prendere il nome del model che mettiamo in relazione- 
4. Es: nella tabella Tag
    <!--  
    public function posts(){
        return $this->belongsToMany('App\Models\Post');
    }
    -->
5.  Es: nella tabella Post
    <!--  
    public function tags(){
        return $this->belongsToMany('App\Models\Tag');
    }
    -->
6. Creiamo la tabella Pivot
    <!-- php artisan make:migration create_post_tag_table --create=post_tag -->

7. Compilo la tabella pivot, creo le due collonne e le metto in relazione con le rispettive tabelle
    <!--  
    public function up()
    {
        Schema::create('post_tag', function (Blueprint $table) {
            $table->unsignedBigInteger('post_id');
            $table->foreign('post_id')->references('id')->on('posts');

            $table->unsignedBigInteger('tag_id');
            $table->foreign('tag_id')->references('id')->on('tags');
        });
    }
    -->
8. Lanciare il comdando per migrare
    <!-- php artisan migrate -->

10. Ora possiamo lavorare con la create e la edit


## Create
1. Nella funzione create del controller risorsa passare tutti i dati dellla tabella in relazione con il compact

2. Andare nella view della create, e ciclare le informazioni, stampandole, per esempio con un label e checkbox
Il name della checkbox, andra a creare un array che conterra tutti i value delle checkbox selezionate, nel value passiamo l'id
    <!-- 
        <div class="my-3">
            <label> Tags: </label>
            @foreach ( $tags as $tag)
                <label>
                    <input type="checkbox" name="tags[]" value="{{ $tag->id }}">
                    {{ $tag->name }}
                </label>
            @endforeach
        </div>
     -->

3.  Aggiorniamo la fumzione store nel controller risorsa

    public function store(Request $request)
    {   
        <!-- Recupero tutte le informazioni passate dal form -->
        $data = $request->all();
        dd($data);

        $request->validate([
            'title' => 'required',
            'body' => 'required'
        ]);

        $newPost = new Post();
        //controllo se l'iimagine è stata caricata nel input
        if( array_key_exists('image', $data) ){
            $cover_url = Storage::put('post_covers', $data['image']);
            $data['cover'] = $cover_url;
        }
        $newPost->fill($data);
        $newPost->save();

        <!-- AGGIORNAMENTO per la relazione many to many dopo il save -->
        // Tramite la funzione di laravel array_key_exists, gli passo come attributi cosa cerco, e dove lo cerco, se esiste allora, feseguirà il codice 
        if( array_key_exists( 'tags', $data ) ){

           // Se esiste la condizione, andra ad aggiornare la tabella PIVOT
           // instanta $newPost-> che è in relazioe con la tabella tags (sotto forma di funzione)-> sincrozizza (con paramentro il valore che vogliamo aggiungere nella tabella)
            $newPost->tags()->sync( $data['tags'] );
        }

        return redirect()->route('admin.posts.index');
    }


## EDIT
1. Nella funzione edit del controller risorsa passare tutti i dati dellla tabella in relazione con il compact

2. Creare le checkbox, all'interno del form edit, nella view edit, uguale alla create. Unica differenza è la condizione per avere le checkbox gia selezionate di partenza in base a cio che era stato creato
    <div class="my-3">
        <label>Tags:</label>

        @foreach ( $tags as $tag)
           <label>
                <input
                    class="form-check-input"
                    type="checkbox"
                    name="tags[]"
                    value="{{ $tag->id }}"

                    CONDIZIONE CHECKED- se l'array $post['tags']-> contiene il valore del ciclo attuale allora mettigli checked
                    {{ $post->tags->contains($tag) ? 'checked' : '' }}>

                {{ $tag->name }}

            </label>
        @endforeach
    </div>

3.  Aggiornare la update

     public function update(Request $request, $id)
    {
        $data = $request->all();
        $singoloPost = Post::findOrFail($id);

        $singoloPost->update($data);

        //controlla se l'utente ha cliccato o erano gia selezionate delle checkbox- UGuale alla create con l'aggiunta del else, nel caso l'utente deselezioni tutto
        if(array_key_exists('tags', $data)){
            $singoloPost->tags()->sync( $data['tags'] );
        } else{
            //non ci sono checkbox selezionate
            $singoloPost->tags()->sync([]);
        }

        return redirect()->route('admin.posts.show', $singoloPost->id);
    }










## Destroy
1. Nella funzione destroy del controller risorsa
     public function destroy($id)
    {
        $singoloPost = Post::findOrFail($id);

        <!-- Eliminazione delle informazioni della abella pivot dell'elemento che sto eliminando -->
        $singoloPost->tags()->sync([]);
        
        $singoloPost->delete();

        return redirect()->route('admin.posts.index');
    }