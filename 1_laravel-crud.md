
# Procedimento di creazione progetto laravel che usa CRUD operations

1. creazione e connessione db (modificando file .env)
2. link al file system
3. scaffolding
4. creazione di un Model
5. creazione della migrazione
6. creazione resource controller per il Model dato (e.g. se il Model è Comic, il resource controller è ComicController)
7. creazione routes in web.php
8. Creazione layout
9. implementazione delle CRUD nel resource controller

## 1. Creazione e connessione db

Creare il db in phpmyadmin

modificare il file .env
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_dc_comics
DB_USERNAME=root
DB_PASSWORD=root
```

## 2. link al file system

modificare il file .env
```env
FILESYSTEM_DISK=public
```

modificare il file config\filesystems.php
```php
'default' => env('FILESYSTEM_DISK','public')
```

eseguire il comando artisan storage link
```bash
php artisan storage:link
```

## 3. scaffolding

Installazione di laravel 9 preset

## 4. e 5. Creazione Model + migrazione

```bash
#creazione modello, migrazione e seed
php artisan make:model Comic -ms
```

NOTA: esiste anche il seguente comando
```bash
#creazione modello, migrazione e seed
php artisan make:model Comic -a
```
Questo comando crea migrazione, seeder, controller e pure le form request. Notare che con questo comando, se voglio cambiare cartella al controller e metterlo ad esempio in admin, dovrò cambiargli namespace.

Altrimenti posso cancellare il controller e rigenerarlo tramite il comando
```bash
#creazione modello, migrazione e seed
php artisan make:controller --model=Comic
```

### comandi terminale per migrazione e seeder
Se in DatabaseSeeder si scrive il seguente codice

```php
$this->call([
    ComicsSeeder::class,
]);
```

Allora si può eseguire il comando seguente
```bash
php artisan migrate --seed
```

Altrimenti è necessario eseguire il comando specificando la classe
```bash
php artisan db:seed --class=ComicsSeeder
```

## 6. Creazione resource controller

```bash
# notare che per convenzione si crea nella cartella Admin
# viene abbinato al model Comic
php artisan make:controller Admin/ComicController --resource --model=Comic
```

## 7. Creazione routes

In web.php
```php
Route::resource('admin/comics', ComicController::class);
```

### Attenzione

ricordarsi di importare il controller in web.php
```php
use App\Http\Controllers\Admin\ComicsController;
```

## 8. Creazione layout

Si avrà la seguente struttura
```
/layouts
    - app.blade
    - admin.blade
```

## 9. implementazione delle CRUD nel resource controller

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

successivamente in admin/comics/create.blade.php si creerà il form.

```php
<form action="route{{'comics.store'}}", method='POST' enctype="multipart/form-data">

@csrf
...

</form>
```

### store

```php
public function store(Request $request) {

    $data = request->all();
    Comic::create($data);

    /* metodo alternativo
    $comic = new Comic();
    $comic->title = $request->title;
    $comic->price = $request->price;
    ...
    $comic->save();
    */

    return to_route('comics.show')->with('message','Operation successfull');

}
```

### Attenzione

Attenzione al mass assingment error. Quando si usa il create method sul Model è necessario aggiungere una fillable property nel modello

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

successivamente in admin/comics/edit.blade.php si creerà il form.

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

    $data = request->all();
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