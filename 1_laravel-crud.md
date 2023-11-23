
# Procedimento di creazione progetto laravel con autenticazione e CRUD operations

1. Operazioni preliminari
2. Creazione e connessione db
3. Link al file system
4. Creazione Model, resource controller associato, migration, seeder
5. Creazione routes in web.php
6. Creazione layout
7. Implementazione delle CRUD nel resource controller


## 1. Operazioni preliminari

- Creare nuovo progetto (assumo nome folder sarà `laravel-dc-comics`)
```bash
laravel new laravel-dc-comics
```

- [opzionale] Installare pacchetto di autenticazione (breeze)
```bash
composer require laravel/breeze --dev
```

```bash
php artisan breeze:install
# selezionare le seguenti opzioni
#    - blade with alpine
#    - no
#    - pest
```

- Installare bootstrap & npm (https://packagist.org/packages/pacificdev/laravel_9_preset)
```bash
composer require pacificdev/laravel_9_preset
```

```bash
# se NON si è installato breeze (guardare link laravel 9 preset per approfondimenti)
php artisan preset:ui bootstrap

# altrimenti
php artisan preset:ui bootstrap --auth
```

```bash
npm i
```

- Aprire file `package.json` ed eliminare stringa `type:module`

- [se necessario] Cambiare l'estensione di `viteconfig.js` in `.cjs`

## 2. Creazione e connessione db

- Modificare il file `.env` (assumo che il nome del database sarà `laravel_dc_comics`)
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_dc_comics
DB_USERNAME=root
DB_PASSWORD=root
```

- Creare il db in phpmyadmin dandogli lo stesso nome che si è messo in `DB_DATABASE` oppure far girare una prima migrazione
```bash
php artisan migrate
```

- Se si fa girare una prima migrazione, il terminale chiederà se si vuole creare il database `laravel_dc_comics`, basta rispondere `yes`

## 3. Link al file system

- Modificare il file `.env`
```env
FILESYSTEM_DISK=public
```

- Modificare il file `config\filesystems.php`
```php
'default' => env('FILESYSTEM_DISK','public')
```

- Eseguire il comando artisan storage link
```bash
php artisan storage:link
```

## 4. Creazione Model, resource controller associato, migration, seeder

### 1. Creazione model e separatamente resource controller
```bash
php artisan make:model Comic
```

```bash
php artisan make:controller Admin/ComicController --resource --model=Comic
```

### 2. Creazione model, resource controller, in aggiunta migrazione e seed
```bash
php artisan make:model Comic -ms
```

### 3. Creazione model, resource controller, in aggiunta  migrazione, seeder e form request
```bash
php artisan make:model Comic -a
```

### NOTA:
Seguendo i punti 2 oppure 3, il controller sarà nella cartella `Controllers`. Se voglio averlo in `Controllers\Admin` come fatto al punto 1, dovrò cambiargli namespace e importare Controller tramite `use`
```php
namespace App\Http\Controllers\Admin;

use App\Http\Controllers\Controller;
```

### Comandi terminale per  eseguire migrazione e seeder

Per eseguire solo la migrazione
```bash
php artisan migrate
```

Ci sono due modi per eseguire uno o più seeder

#### Modo 1
- Aggiungere uno o più seeder in `DatabaseSeeder`
```php
$this->call([
    ComicsSeeder::class,
    OtherSeeder::class,
]);
```

- Eseguire Il comando bash
```bash
php artisan migrate --seed
```

#### Modo 2
- Eseguire un seeder alla volta (specificando il nome in `class`)
```bash
php artisan db:seed --class=ComicsSeeder
```

## 5. Creazione routes

- Aprire il file `web.php` e aggiungere il seguente codice
```php
Route::resource('admin/comics', ComicController::class);
```

### Attenzione

Ricordarsi di importare il controller in `web.php`
```php
use App\Http\Controllers\Admin\ComicsController;
```

## 6. implementazione delle CRUD nel resource controller

### index

```php
public function index() {

    $comics = Comic::all();
    /* se voglio ordinare per id decrescente
    $comics = Comic::orderByDesc(id)->get();
     */

    return view('admin.comics.index', compact('comics'));
}
```

### create

```php
public function create() {
    return view('admin.comics.create');
}
```

In `admin/comics/create.blade.php` si creerà il form.

```php
<form action="route{{'comics.store'}}", method='POST' enctype="multipart/form-data">

@csrf

...

</form>
```

### store

```php
public function store(Request $request) {

    $data = $request->all();
    Comic::create($data);

    /* metodo alternativo
    $comic = new Comic();
    $comic->title = $request->title;
    $comic->price = $request->price;
    ...
    $comic->save();
    */

    return to_route('comics.index')->with('message','Operation successfull');

}
```

### Attenzione

Attenzione al `mass assignment error`. Quando si usa il create method sul Model è necessario aggiungere una fillable property nel modello in cui si elencano tutti i campi che si aggiungono tramite la `request`
```php
class Comic extends Model
{
    use HasFactory;

    protected $fillable = ['title', 'description', 'price', '...'],
}
```

### show

```php
public function show(Comic $comic) {

    return view('admin.comics.show', compact('comic'));
}
```

### edit

```php
public function edit(Comic $comic) {

    return view('admin.comics.edit', compact('comic'));
}
```

successivamente in `admin/comics/edit.blade.php` si creerà il form.

```php
<form action="route{{'comics.update'}}, $comic->id", method='POST'>

@csrf
@method('PUT')

...

</form>
```

### update

```php
public function update(Request $request Comic $comic) {

    $data = $request->all();
    $comic->update($data);
    return to_route('comics.index')->with('message','Operation successfull');
}
```

### delete

```php
public function destroy(Comic $comic) {

    $comic->delete();
    return to_route('comics.index')->with('message','Operation successfull');
}
```