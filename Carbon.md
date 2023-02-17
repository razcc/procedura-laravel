## Carbon 
1. Serve a sistemare le date, di base phpMyAdmin ha un fusoOrario prestabilito che è americano, carbon ci permette di gestire le date nel modo che preferiamo

2. 
    ``` composer require nesbot/carbon ```

3. Nel posto dove voglio usare carbon uso:
   ```  use Carbon\Carbon; ```

<!--! TIMEZONE fuso Orario -->
4. Andiamo nella cartella confic in app.php, cerchiamo timezone, e modifichiao da UTC a Europe/Rome 
    ``` 'timezone' => 'Europe/Rome', ```

5. Link per le time zone: 
        https://www.w3schools.com/php/php_ref_timezones.asp#europe

6. Dopo aver modificato un fiile nella cartella confiuc bisogna ripulire la cache
    <!-- php artisan config:clear -->

7. Secondo metodo di modifica della timeZone
8. Anziche modificare direttamente la timeZone del file app.php della cartella config, al suo interno creiamo un collegamento al file .env      

    <!-- 'timezone' => 'env('APP_TIMEZONE', 'UTC')', -->
    Primo parametro serve a dire cosa deve cercare nel file .env,il secondo è cosa deve applicare nel caso nel .env non ci sia niente

9.  Andare nel file .env e creare una nuova chiave il APP_TIMEZONE
    APP_TIMEZONE = 'Europe/Rome'



10. Comando per resettare le date preesistenti su php my admin: 
    <!-- php artisan migrate:refresh --seed  -->


## Usiamo carbon
1. Nel modello giusto, usiamo cartbon, importarlo

2. Creare una funzione custom, che useremo in pagina view, quando voremmo stampare una data

    public function getFormattedDate($nomeColonna, $tipoFormattazione){

        return Carbon::create( $this->$nomeColonna )-> format($tipoFormattazione)
    }

3. Quando voglio stampare nella index, richiamo la funzione passando come parametro, il nome della colonna che voglio formattare, e il tipo di formattazione, sotto forma di stringa ('d-m-y h:i:s')

const no[1] = 'cares';
    Work.harder();






















