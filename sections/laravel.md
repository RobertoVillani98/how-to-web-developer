# LARAVEL
<p align="center">
  <img src="https://raw.githubusercontent.com/github/explore/56a826d05cf762b2b50ecbe7d492a839b04f3fbf/topics/laravel/laravel.png" height="150">
  <br/>
</p>

## Indice

* [Inizzializzazione del progetto](#inizializzazione-del-progetto)
* [Inizzializzazione della struttura](#inzializzazione-struttura)
* [Database](#database)
* [Creare e gestire tabelle DB da Laravel](#creare-e-gestire-tabelle-db-da-laravel)
* [Seeder e Faker](#seeder-e-faker)
* [Altro e Bugfix ](#altro-e-bugfix)


## INIZIALIZZAZIONE DEL PROGETTO

1. Creare cartella progetto

2. Artisan

Aprirla con VSCode, aprire terminale
```
composer create-project --prefer-dist laravel/laravel:^7.0 .

php artisan serve
```

3. NPM

Aprire nuovo terminale
```
npm install
npm run watch
```

Ora la base del progetto è completa e il server è up.


## INZIALIZZAZIONE STRUTTURA

VIEWS

1. Cartella layouts
***resources>view>layouts***
Ci vanno i template delle pagine
```
base.blade.php
```
```php
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


2. Cartella partials
***resources>view>partials***
Ci vanno i pezzi da includere nei layout ed eventualmente in alcune pagine

header.blade.php

<header>
  <h3>Header</h3>
</header>







3) Pagine sito
resources>view
Pagine del sito nella cartella view

home.blade.php

```php
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



SCSS

1) Cartella partials
resources>sass>partials

- Creare file scss: _reset.scss, _common.scss, _typography.scss
- Se presenti creare anche: _header.scss, _footer.scss, _mainHome.scss

2) Importare tutto nel file app.scss
@import "partials/reset";
@import "partials/common";
@import "partials/typography";
@import "partials/header";
@import "partials/mainHome";
@import "partials/footer";


3) Esempio scss di reset, common e typography

Reset
* {
box-sizing: border-box;
margin: 0;
padding: 0;
}
Common
.container {
max-width: 1300px;
min-width: 950px;
margin: 0 auto;
}
Typography
body {
font-family: Arial, Helvetica, sans-serif;
}




## DATABASE

0) Accendere MAMP

1) Comunicare a Laravel i dati corretti del DB

  - Fermare php artisan (ctrl+c nel terminale dove è stato lanciato)
  - Aprire file .env
  - Cambiare il valore di:
    DB_PORT (mamp porta *vedi porta MySQL su MAMP*)
    DB_DATABASE (nome del database, es: movies)
    DB_USERNAME=root
    DB_PASSWORD=root
  - Pulire la cache: ```php artisan config:clear```
  - ```php artisan serve```


2) Creare Controller

  - ```php artisan make:controller NomeController``` [pascal case singolare]
(viene creato in App>Http>Controllers)

Esempio (DB movies):
  - ```php artisan make:controller MovieController```

  - Creare metodo index che ritorna la view della pagina
  public function index()
  {
    return view("home");
  }


3) Routes

routes>web.php
Contiene tutte le Routes che rimandano ai controller delle nostre pagine del sito
Esempio di una Route Generica:
Route::get('/indirizzo_pagina', “PaginaController@index”);

Esempio della Route "Home":
Route::get('/', “HomeController@index”);


4) Creare Model

  - Lanciare comando: ```php artisan make:model Movie``` [pascal case singolare] (viene creato sparso in App)
  - Dichiarare l'uso del Model nel Controller (inserire nel controller: use App\NomeModel;)



5) La prima Query

- Chiediamo al database tutti i dati: nella funzione index del Controller dichiariamo una variabile in cui inseriamo tutti i dati del database, e poi la returniamo insieme alla view.

public function index()
{
    $data = NomeModel::all();
    return view("home", compact(“data”));
}

ES con movies: 
public function index()
{
    $movies = Movie::all();
    return view("home", compact(“movies”));
}

- A questo punto nella pagina blade.php di riferimento (ES: home.blade.php) possiamo usare la variabile (ES: $movies) come se fosse lì, e conterrà tutti i dati del DB.

ES:
<ul>
    @foreach ($movies as $movie)
        <li>{{ $movie[‘title’] }}</li>
    @endforeach
</ul>



### CREARE E GESTIRE TABELLE DB DA LARAVEL

Dopo aver creato il DB su phpmyadmin possiamo gestirlo direttamente da Laravel attraverso le Migrations.
Si lancia un comando da terminale che crea il file della migration (database>migrations), poi posso aprire il file e definire nel particolare cosa voglio che succeda.

Sintassi generale:
```php artisan make:migration nome_della_migration```

Tutti i file creati con le migrations si trovano in (database>migrations).

Creare Tabella
Nel terminale lanciare: 
```php artisan make:migration create_users_table```    //Crea una tabella “users”

Aggiornare Tabella
Nel terminale lanciare:
```php artisan make:migration update_users_table --table=users```    //Aggiorna la tabella “users”

Eseguire le modifiche
Nel terminale lanciare:
```php artisan migrate```

Ripristinare le modifiche
Nel terminale lanciare:
```php artisan migrate:rollback```

Inserire una colonna
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
});



ATTENZIONE:
Per qualsiasi modifica eseguita nell’up di una migration, bisogna sempre eseguire il contrario nel down della stessa migration


Comandi utili in caso di problemi:

Droppare il database

```php artisan migrate:fresh```


Droppare il database e poi rifare tutte le migrate

```php artisan migrate:refresh```


Rollback di tutto il database

```php artisan migrate:reset```



## SEEDER e FAKER

Creare il Seeder nel progetto
```php artisan make:seeder NomeTabellaTableSeeder```
crea il file della migration (database>seeds)

Controllare nel file config.json se stiamo usando fzaninotto o fakerphp, nel caso stessimo usando fzaninotto rimuoverla e installare fakerphp.
```
composer remove fzaninotto/faker
composer require fakerphp/faker
```

Popolare la tabella con un faker
Nel file seeder appena creato aggiungere:
use Faker\Generator as Faker;
use App\NomeModel;  // Il seeder è riferito a una tabella, che è collegata a un model

Nel metodo run descrivere come popolare i dati:
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


Eseguire il faker per popolare il DB

```php artisan db:seed --class=NomeTabellaTableSeeder```
oppure
```php artisan db:seed```

## ALTRO e BUGFIX

WEBPACK.MIX.JS

ATTENZIONE: Quando si modifica il webpack riavviare ```npm run watch```

1) Aggiungere l’option alla fine per non modificare il percorso delle background image (../img/eccecc)

mix.js('resources/js/app.js', 'public/js')
.sass('resources/sass/app.scss', 'public/css')
.options({
processCssUrls: false
});


Problemi con nomi tabelle plurale

Si può specificare il nome “personalizzato” delle tabelle quando il plurale non coincide con il singolare + lettera ‘s’:
Nel Model, all’interno della classe aggiungere:
	protected $table = ‘people’;


Alcune query
Da inserire nelle funzioni del controller, per esempio index.

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



Salvare un dato
Nel seeder, nel metodo run():

$auto = new Auto();
$auto->marca = ‘Fiat’;
$auto->modello = ‘Punto’;
$auto->targa = ‘DA788CK’; 

$auto -> save();