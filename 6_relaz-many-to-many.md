# Scaletta relazioni many-to-many

- NOTA: assumo una relazione many-to-many tra i model `Technology` e `Project`

- Creare modello `Technology` con migration e seeder
```bash
php artisan make:model Technology -ms
```

- Impostare migrazione e Seeder di `Technology`

- Creare migration per tabella pivot
```bash
php artisan make:migration create_project_technology_table
```

- All'interno della tabella pivot
```php
public function up(): void
    {
        Schema::create('project_technology', function (Blueprint $table) {
            $table->unsignedBigInteger('project_id');
            $table->foreign('project_id')->references('id')->on('projects');


            $table->unsignedBigInteger('technology_id');
            $table->foreign('technology_id')->references('id')->on('technologies');

            $table->primary(['project_id', 'technology_id']);
        });
    }

    public function down(): void
    {
        Schema::dropForeign('project_technology_technology_id_foreign');
        Schema::dropIfExists('project_technology');
    }
```

- Aggiungere ai model `Project` e `Technology` i metodi per definire la relazione many to many

- in `Project`
```php
public function technologies(): BelongsToMany
    {
        return $this->belongsToMany(Technology::class);
    }
```

- in `Technology`
```php
public function projects(): BelongsToMany
    {
        return $this->belongsToMany(Project::class);
    }
```

- Eseguire migrazione e fare il seed di `Technology`
```bash
php artisan migrate
php artisan db:seed --class=TechnologySeeder
```