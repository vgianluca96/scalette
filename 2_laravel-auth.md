
# Creazione web app con autenticazione

## Operazioni preliminari
- laravel new nome-cartella-progetto
- configurazione database nel file .env
- installazione breeze
    - composer require laravel/breeze --dev
    - php artisan breeze:install
    - seleziono le seguenti opzioni
        - blade with alpine
        - no
        - pest
- installazione pacchetto bootstrap & npm
    - composer require pacificdev/laravel_9_preset
    - php artisan preset:ui bootstrap --auth
    - eliminazione stringa `type:module` dal file package.json
    - se necessario: cambio l'estensione di viteconfig.js in .cjs

## refactoring codice - spostamento di dashboard.blade

Creo controller della dashboard
```bash
php artisan make:controller Admin/DashobardController
```

- dashboard.blade lo sposto in view\admin
- aggiorno la rotta corrispondente in web.php

```php
Route::middleware(['auth', 'verified'])->prefix('admin')->name('admin.')->group(function () {
    Route::get('/', [DashboardController::class, 'index'])->name('dashboard');
});
```

- NOTA: in app.blade occorrera sostituire `url('dashboard')` con `route('admin.dashboard')`

## Aggiornamento RouteServiceProvider

- in RouteServiceProvider.php assegno alla variabile HOME il valore `admin`, così all'autenticazione avrò la url corretta

## Creazione modello
Creo il modello con tutto (con -a gli faccio creare migration, seeder, controller, form request e altro)
```bash
php artisan make:model Nomemodello -a
```

- elimino Policies che non servono
- NomemodelloController lo sposto in Admin (NOTA: devo cambiargli namespace)
- imposto migration e seeder
- imposto la rotta che punta a NomemodelloController in web.php
- imposto le CRUD
    - NOTA: i metodi store e update dovranno avere le validazioni

### impostare la pagination
- php artisan vendor:publish

