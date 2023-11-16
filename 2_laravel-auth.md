
# Creazione web app con autenticazione

- Nuovo progetto
```bash
laravel new nome-cartella-progetto
```

- Configurazione database nel file `.env`

- Installazione breeze
```bash
composer require laravel/breeze --dev
```

```bash
php artisan breeze:install
# seleziono le seguenti opzioni
#        blade with alpine
#        no
#        pest
```
    
- Installazione pacchetto bootstrap & npm
```bash
composer require pacificdev/laravel_9_preset
```

```bash
php artisan preset:ui bootstrap --auth
```

- Eliminazione stringa `type:module` dal file `package.json`

- [se necessario] Cambio l'estensione di `viteconfig.js` in `.cjs`

# Altre operazioni

## [opzionale] Refactoring codice - spostamento di dashboard.blade

- Creo controller della dashboard
```bash
php artisan make:controller Admin/DashboardController
```

- `dashboard.blade` lo sposto in `view/admin`
- Aggiorno la rotta corrispondente in `web.php`

```php
Route::middleware(['auth', 'verified'])->prefix('admin')->name('admin.')->group(function () {
    Route::get('/', [DashboardController::class, 'index'])->name('dashboard');
});
```

- NOTA: in `app.blade` occorrera sostituire `url('dashboard')` con `route('admin.dashboard')`

### Aggiornamento RouteServiceProvider

- in `app/Providers/RouteServiceProvider.php` assegno alla variabile `HOME` il valore `/admin`, così al login sarò rimandato alla url corretta

## Creazione modello
- Creo il modello con tutto (con -a gli faccio creare migration, seeder, resource controller, form request e altro)
```bash
php artisan make:model Nomemodello -a
```

- Elimino `Policies` che non servono
- `NomemodelloController` lo sposto in `Admin` (NOTA: devo cambiargli namespace e importare il controller)
- imposto migration e seeder
- imposto la rotta che punta a `NomemodelloController` in `web.php`
- imposto le CRUD
    - NOTA: i metodi `store` e `update` dovranno avere le validazioni

## Impostare la pagination
- Nel resource controller, nella crud `index`, aggiungo il metodo `paginate`
```php
public function index()
    {
        $projects = Project::orderByDesc('id')->paginate(10);

        return view('admin.projects.index', ['projects' => $projects]);
    }
```

- Nella pagina index (nella posizione dove voglio mi appaia la pagination)
```php
{{ $projects->links('pagination::bootstrap-5') }}
```