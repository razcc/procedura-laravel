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



































