# API

- Le API si creano nel file `routes/api.php`
```php
Route::get('projects', function(){
    return response()->json([
        'status' => 'success',
        'result' => App\Models\Project::all()
        //'result' => App\Models\Project::paginate(5)
        //'result' => App\Models\Project::orderByDesc('id')->paginate(5)
    ]);
});
```

- Altrimenti le API si possono gestire tramite controller
```bash
php artisan make:controller Api/ProjectController
```

- In `routes/api.php` creo la rotta per il controller
```php
Route::get('projects', [ProjectController::class, 'index']);
```

- NOTA: in base a come è scritta la rotta, l'indirizzo della API sarà `my-address/api/projects` dove in locale `my-address = http:http://127.0.0.1:NumeroPorta`

- Nel controller `Api/ProjectController` scriverò
```php
public function index()
    {
        $projects = Project::all();
        //$projects = Project::orderByDesc('id')->paginate(5);
        return response()->json([
            'status' => 'success',
            'author' => 'gianluca',
            'result' => $projects
        ]);
    }
```

## App frontend

- creazione app vite
```bash
npm create vite@latest folder-name -- --template vue
```

- tasto destro sul workspace > add folder to workspace > apro la cartella dell'app frontend

- installazione axios
```bash
npm install axios
```

- installazione bootstrap
```bash
npm i --save bootstrap @popperjs/core
```

- installazione sass
```bash
npm i --save-dev sass
```

