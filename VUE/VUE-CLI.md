## Progetto VUE CLI
1. Creare una repository vuota su github
2. Clonarla sul pc con vs code

3. 
    ``` vue create . ```
4. 
    ``` npm run serve ```

## BOOTSTRAP
1. 
    ``` npm install bootstrap@5.2.3 ```

2. All'interno del file 📃"main.js" scrivo i due codici:

            import 'bootstrap/dist/css/bootstrap.min.css' 
            import 'bootstrap/dist/js/bootstrap.min.js' 
    

## AXIOS
1. 
    ``` npm install axios ```

2. All'interno del file .vue dove vogliamo utilizzare axios, inseriamo l'import:
    ``` import axios from 'axios' ```

3. utilizzo poi axios all'interno del componente dove l'ho importato,
    all'interno di mounted() created() se vogliamo fare un caricamnto delle api al' caricamento della pagina,
    oppure all'interno di method() se vogliamo richiamarla come funzione e poi utilizzarla in base alla necessità

## FONTAWESOME
1. 
    ``` npm i --save @fortawesome/fontawesome-svg-core ```

2. ```  CODICE  ```
        npm i --save @fortawesome/free-solid-svg-icons   
        npm i --save @fortawesome/free-regular-svg-icons 
        npm i --save @fortawesome/free-brands-svg-icons
    

3. ``` npm i --save @fortawesome/vue-fontawesome@latest-2 ```

4. Aprire poi il file main.js e scrivere all'interno:
        ```  CODICE  ```
        /* import the fontawesome core */
        import { library } from '@fortawesome/fontawesome-svg-core'

        /* import font awesome icon component */
        import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

        /* import specific icons */
        import { faUserSecret } from '@fortawesome/free-solid-svg-icons'
        /*Import icone regular */
        import { faFaceSmile } from '@fortawesome/free-regular-svg-icons'

        /* add icons to the library */
        library.add(faUserSecret)

        /* add font awesome icon component */
        Vue.component('font-awesome-icon', FontAwesomeIcon)
    

5. ATTENZIONE: se la stessa icona viene importata con due stili diversi ad esempio: regular e solid,
non possiamo usare lo stesso nome per le due icone ma dobbiamo usare un rieticchetamento con "as", il codice diverrà:
        ```  CODICE  ```
        /* import specific icons */
        import { faUserSecret as faUserSecretSolid } from '@fortawesome/free-solid-svg-icons'

        /*Import icone REGULAR */
        import { faUserSecret as faUserSecretRegular } from '@fortawesome/free-regular-svg-icons'

        /* add icons to the library */
        library.add(faUserSecretSolid, faUserSecretRegular)
    

6. Nelle righe di codice:
        ```  CODICE  ```
        /* import specific icons */
        import { faUserSecret } from '@fortawesome/free-solid-svg-icons'
        
        /* add icons to the library */
        library.add(faUserSecret)
        

7. Dobbiamo modificare il nome delle icone all'interno delle parentesi con il nome delle icone che vogliamo usare di fontawesome, con l'esempio dle codice sopra stiamousando l'icona "fa-user-secret" Se vogliamo usare l'icona della lente d'inngradimento scriveremo: 
        ```  CODICE  ```
        /* import specific icons */
        import { faUserSecret, faMagnifyingGlass } from '@fortawesome/free-solid-svg-icons' 
        /* add icons to the library */
        library.add(faUserSecret, faMagnifyingGlass)
    

8. Per utilizzare nel codice del componente l'icona dobiamo inserirle con il codice di fontawesome in stile componente
