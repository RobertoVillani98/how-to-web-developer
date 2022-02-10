# LARAVEL
<p align="center">
  <img src="https://raw.githubusercontent.com/github/explore/56a826d05cf762b2b50ecbe7d492a839b04f3fbf/topics/laravel/laravel.png" height="150">
  <br/>
</p>

## Indice

* [Inizializzazione del progetto](#inizializzazione-del-progetto)

* [Database](#database)
  * [Collegare un database](#collegare-un-database)
  * [Popolare un database](#popolare-un-database)
    - [Migration](#migration)
    - [Seeder](#seeder)
      * [Faker](#faker)
* [Inizializzazione della struttura](#inzializzazione-struttura)
* [Altro e Bugfix ](#altro-e-bugfix)


## INIZIALIZZAZIONE DEL PROGETTO

1. Creare una cartella che conterrà il progetto

Aprire la cartella con VSCode, successivamente aprire il terminale e digirate:

```
composer create-project --prefer-dist laravel/laravel:^7.0 .
```
> Il punto alla fine della stringa in alto va inserito (indica la cartella corrente)

```
php artisan serve
```

<!-- 3. NPM

Aprire un nuovo terminale
```
npm install
npm run watch
``` -->

Ora la base del progetto è completa e il server è up.

<br>
<br>

# DATABASE
<br>

## COLLEGARE UN DATABASE
<br>

1. Avviare MAMP

2. Creare un database da collegare al progetto [(*Se non sai come fare visualizza la guida MySQL*)](sections/mysql.md)

2. Colleghiamo il DB appena creato al nostro progetto Laravel:

  - Aprire il file .env

  - Modifica i seguenti valori:

    * ```DB_PORT``` (**inserisci la porta di MySQL indicata da MAMP**)

    * ```DB_DATABASE``` (**nome del Database che hai creato al punto 2**) :point_up_2:

    * ```DB_USERNAME=root```

    * ```DB_PASSWORD=root```

  - Pulire la cache:

    ```
    php artisan config:clear
    ```

<br>
<br>

## POPOLARE UN DATABASE
<br>

Per popolare il nostro Databse abbiamo la possibilità di aggiungere nuove tabelle ( attraverso le [**MIGRATION**](#migration) ) e di aggiungere dati alle tabelle ( attraverso il [**SEEDER**](#seeder) ).

<br>
<br>

## MIGRATION
<br>

Dopo aver creato il DB su [phpmyadmin](sections/mysql.md) possiamo gestirlo direttamente da Laravel attraverso le Migrations. 

:point_down: ***I Comandi vanno lanciati da terminale***

* Creare un file migration
  ```
  php artisan make:migration nome_della_migration
  ```
  (*il suo percorso all'interno del nostro progetto sarà database>migrations*)

* Creare una tabella:
  ```
  php artisan make:migration create_nome-della-tabella_table
  ```    
* Caricare la tabella all'interno del Database:
  ```
  php artisan migrate
  ```
* Ripristinare le modifiche apportate 
  ```
  php artisan migrate:rollback
  ```
* Modificare una tabella presente nel Database:
  ```
  php artisan make:migration update_nome-della-tabella_table --table=nome-della-tabella
  ```

:firecracker: ATTENZIONE 	:firecracker:

Per qualsiasi modifica eseguita nell’up di una migration, bisogna sempre eseguire il contrario nel down della stessa migration

COMANDI PERICOLOSI :warning:

* Droppare il database
  ```
  php artisan migrate:fresh
  ```

* Droppare il database e lanciare tutte le migrate
  ```
  php artisan migrate:refresh
  ```


* Rollback di tutto il database
  ```
  php artisan migrate:reset
  ```
<!-- Inserire una colonna
Nel file creato con la migration aggiungere:
Schema::table('users', function (Blueprint $table) {
    $table->string('email');
});

Rimuovere una colonna
Nel file creato con la migration aggiungere:
Schema::table('users', function (Blueprint $table) {
    $table->dropColumn(‘lastname’);
});

Modificare una colonna
```composer require doctrine/dbal```
Schema::table('users', function (Blueprint $table) {
    $table->string(“country”, 150)->change();
}); -->
<br>
<br>

## SEEDER
<br>

I seeder servono a popolare le tabelle di un Database.

* Creare il Seeder nel progetto:

  ```
  php artisan make:seeder NomeTabellaTableSeeder
  ```
  (*il suo percorso all'interno del nostro progetto sarà database>seeds*)

Una volta creato il seeder, possimo popolare le tabelle richiamando i dati da un file/array esistente oppure attraverso un facker il quale inserire dati random.

* Aggiungiamo i dati da un file implementando il nostro seeder con: 

  ```php
  use App\Comic;
  ```

  ```php
  public function run()
    {
         //Richiamo i dati dal file comics.php all'interno della cartella config
         $comics = config("comics");

         // Cicliamo per popolare la tabella del Database
         foreach($comics as $comic){
             $newComic = new Comic();
             $newComic->title = $comic["title"];
             $newComic->description = $comic["description"];
             $newComic->thumb = $comic["thumb"];
             $newComic->price = $comic["price"];
             $newComic->series = $comic["series"];
             $newComic->sale_date = $comic["sale_date"];
             $newComic->type = $comic["type"];
             $newComic->save();
 
         }
    }
  ```
:firecracker: ATTENZIONE 	:firecracker:
```php
use App\Comic;
```

```php
$newComic = new Comic();
```
Queste porzioni di codice importano ed utilizzano il **model** se non l'hai ancora creato procedi attraverso la sezione [model](#)

<br>
<br>

## POPOLARE UNA TABELLA CON FAKER
<br>

* Cartella layouts
***resources>view>layouts***
Ci vanno i template delle pagine

Nome del file: **base.blade.php**
:warning: Controllare nel file **config.json** se stiamo usando fzaninotto o fakerphp, nel caso stessimo usando fzaninotto rimuoverla e installare fakerphp.
```
composer remove fzaninotto/faker
composer require fakerphp/faker
```

* Popolare la tabella con un faker
Nel file seeder appena creato aggiungere:
```php
use Faker\Generator as Faker;
use App\NomeModel;  // Il seeder è riferito a una tabella, che è collegata a un model
```

* Nel metodo run descrivere come popolare i dati:
```php
public function run(Faker $faker)
  {
    $variabile = new NomeModel(); (es:  $travel = new Travell();)
    $travel->name = $faker->sentence(3);
    $travel->destination = $faker->country();
    $travel->duration = $faker->randomNumber(2, false);
    $travel->flight = $faker->numberBetween(0, 1);
    $travel->price_adults = $faker->numberBetween(200, 1000);
    $travel->price_children = $faker->numberBetween(10, 200);
    $travel->save();
  }
```
* Eseguire il seeder per popolare il DB
```
php artisan db:seed --class=NomeTabellaTableSeeder
```

<br>
<br>

## POPOLARE UNA TABELLA CON UN FILE PHP
<br>

* Spostare nella cartella config il file php che contiene i dati

* Creare seeder

* In alto nel seeder: use App\NomeModel;

* Nel metodo run() del seeder:
```php
$comics = config("comics");

foreach ($comics as $comic) {
      $newComic = new Comic();
      //
      $variabile -> nomeColonna = $elemento[“proprietà”];
      $newComic->title = $comic["title"];
      $newComic->series = $comic["series"];
      $newComic->type = $comic["type"];
      $newComic->description =$comic["description"];
      $newComic->link = $comic["thumb"];
      $newComic->sale_date = $comic["sale_date"];
      $newComic->price = $comic["price"];
      //
      $newComic->save();
}
```
* Eseguire il seeder per popolare il DB
```
php artisan db:seed --class=NomeTabellaTableSeeder
```
<br>
<br>
<br>

# INZIALIZZAZIONE STRUTTURA
<br>

## 1. Creare Model

  - Lanciare questo comando: *[nome pascal case singolare]* (viene creato sparso in ***App***)
  ```
  php artisan make:model NomeModel
  ```
  Esempio: ```php artisan make:model Movie```

<br>
<br>

## 2. Creare Controller

- Lanciare questo comando: *[nome pascal case singolare]* (viene creato in ***App>Http>Controllers***)
```
php artisan make:controller NomeControllerController
``` 


Esempio:
```php artisan make:controller MovieController```

<br>

- Dichiarare l'uso del Model nel Controller inserendo in alto nel Controller: 
```php
use App\NomeModel;
```

- Creare metodo index che ritorna la view della pagina
```php
public function index()
{
  return view("home");
}
```
<br>
<br>

## 2.1. Creare Controller (Resource Controller) *[alternativa]*
<br>

Architettura REST: Associare a delle URI standard dei metodi standard.

- Creare Resource Controller: 
```
php artisan make:controller --resource NomeController
```

- Guardare la lista delle routes: 
```
php artisan route:list
```

- In alto nel nuovo controller: 
```
use App\NomeModel;
```

- Nella cartella views creare una sottocartella con il nome dell’entità su cui stiamo lavorando (es: ***comics***), in cui creare una pagina **index.blade.php**

<br>

### - Metodo index(): Mostra tutti i risultati
<br>

Nel metodo index() del controller:
```php
$variabile = NomeModel::all();
return view("nomecartella.index", compact("variabile"));
```

Esempio:
```php
$comics = Comic::all();
return view("comics.index", compact("comics"));
```
<br>

### - Metodo show(): Mostra il risultato dato un parametro dinamico (id)
<br>

Nel metodo show() del controller:
```php
$variabile = NomeModel::find($id);
return view("nomecartella.show", compact("variabile"));
```
Esempio:
```php
$comic = Comic::find($id);
return view("comics.show", compact("comic"));
```

<br>
<br>

## 3. Routes
<br>

***routes>web.php***<br>
* Contiene tutte le Routes che rimandano ai controller delle nostre pagine del sito

Esempio di una Route Generica:
```php
Route::get('/indirizzo_pagina', “PaginaController@index”);
```

Esempio della Route "Home":
```php
Route::get('/', “HomeController@index”);
```
<br>

## 3.1. Routes con Resource Controller *[alternativa]*
<br>

***routes>web.php***<br>
```php
Route::resource("comics", "ComicController");
```
<br>
<br>

## 4. Views
<br>

* Cartella layouts:
***resources>view>layouts***<br>
Contiene i template delle pagine

<br>

**base.blade.php**

```blade
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>@yield('pageTitle')</title>
  <link rel="stylesheet" href="{{asset('css/app.css')}}">
</head>

<body>
  @include('partials.header')
  <main>
    @yield('pageContent')
  </main>
  @include('partials.footer')
</body>
</html>
```
<br>

* Cartella partials:
***resources>view>partials***<br>
Contiene i pezzi da includere nei layout ed eventualmente in alcune pagine

<br>

```html
**header.blade.php**
```
<header>
  <h3>Header</h3>
</header>
```


* Pagine sito: 
***resources>view***<br>
Pagine del sito nella cartella view

<br>

**home.blade.php**

```blade
@extends('layouts.base')

@section('pageTitle')
Home
@endsection

@section('pageContent')
<div class="container">
  <h1>Home</h1>
</div>
@endsection
```



## Scss

* Creare cartella ***partials***<br>
***resources>sass>partials***

* Creare file scss: ***_reset.scss, _common.scss, _typography.scss***
* Se presenti creare anche: ***_header.scss, _footer.scss, _mainHome.scss***

* Importare tutto nel file ***app.scss***
```scss
@import "partials/reset";
@import "partials/common";
@import "partials/typography";
@import "partials/header";
@import "partials/mainHome";
@import "partials/footer";
```
<br>

### Esempi scss di reset, common e typography

Reset
```scss
* {
box-sizing: border-box;
margin: 0;
padding: 0;
}
```
Common
```scss
.container {
max-width: 1300px;
min-width: 950px;
margin: 0 auto;
}
```
Typography
```scss
body {
font-family: Arial, Helvetica, sans-serif;
}
```
<br>

### Installare Bootstrap su Laravel:
<br>

```
composer require laravel/ui:^2.4
```
```
php artisan ui bootstrap
```
```
npm install
```
```
npm run dev
```
```
npm run watch
```

Includere il css di bootstrap nei nostri layout.
Nel head dell’html aggiungere: 
```html
<link rel="stylesheet" href="{{ asset('css/app.css') }}">
```

<br>
<br>

# ALTRO e BUGFIX
<br>

## Query
<br>

### La prima query
- Chiediamo al database tutti i dati: nella funzione index del Controller dichiariamo una variabile in cui inseriamo tutti i dati del database, e poi la returniamo insieme alla view.

```php
public function index()
{
    $data = NomeModel::all();
    return view("home", compact(“data”));
}
```

Esempio con movies: 

```php
public function index()
{
    $movies = Movie::all();
    return view("home", compact(“movies”));
}
```

- A questo punto nella pagina blade.php di riferimento (Es: home.blade.php) possiamo usare la variabile (Es: $movies) come se fosse lì, e conterrà tutti i dati del DB.

Esempio:
```blade
<ul>
    @foreach ($movies as $movie)
        <li>{{ $movie[‘title’] }}</li>
    @endforeach
</ul>
```
<br>

### Alcune query
<br>

Sono da inserire nelle funzioni del controller, per esempio index.

```php
- $movies = App\NomeModel::where(‘active’, 1)->get();   // ACTIVE == 1

- $movies = App\NomeModel::where(‘active’, ‘!=’, 1)->get();   // ACTIVE != 1

- $movies = App\NomeModel::where(‘active’, 1)
->orderBy(‘title’, ‘desc’)->get();     //Ordinati decrescente

- $movies = App\NomeModel::where(‘active’, 1)
->orderBy(‘title’, ‘desc’)
->limit(10)->get();     //Solo i primi 10

- $movies = App\NomeModel::where(‘active’, 1)->first();   // ACTIVE == 1 Solo il primo risultato

- $users = User::all();
  $users = $users->find($id);    //Trovare per id
```
<br>
<br>

## webpack.mix.js
<br>

ATTENZIONE: Quando si modifica il webpack riavviare ```npm run watch```

* Aggiungere l’option alla fine per non modificare il percorso delle background image (../img/eccecc)

```js
mix.js('resources/js/app.js', 'public/js')
.sass('resources/sass/app.scss', 'public/css')
.options({
processCssUrls: false
});
```
<br>
<br>

## Problemi con nomi tabelle plurale
<br>

Si può specificare il nome “personalizzato” delle tabelle quando il plurale non coincide con il singolare + lettera ‘s’:
Nel Model, all’interno della classe aggiungere:
```
protected $table = ‘people’;
```
<br>
<br>

## Salvare un dato
<br>

Nel seeder, nel metodo run():

```php
$auto = new Auto();
$auto->marca = ‘Fiat’;
$auto->modello = ‘Punto’;
$auto->targa = ‘DA788CK’; 

$auto -> save();
```
