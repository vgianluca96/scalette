# scaletta relazioni one-to-many

- creazione modello con migrazione e seeder (ad esempio Type)
```bash
php artisan make:model Type -ms
```

- creare la migration di modifica per la tabella projects per aggiungere la chiave esterna
```bash
php artisan make:migration add_type_id_foreign_key_to_projects_table
```

- all'interno della migration si dovrà avere
```php
public function up(): void
    {
        Schema::table('projects', function (Blueprint $table) {
            $table->unsignedBigInteger('type_id')->nullable()->after('id');

            $table->foreign('type_id')
                ->references('id')
                ->on('types');
        });
    }

public function down(): void
    {
        Schema::table('projects', function (Blueprint $table) {
            $table->dropForeign('projects_type_id_foreign');
        });
    }
```

- aggiungere ai model Project e Type (ovvero i model da mettere in relazione) i metodi per definire la relazione one to many

- in Project
```php
 public function type(): BelongsTo
    {
        return $this->belongsTo(Type::class);
    }

```

- in Type
```php
public function projects(): HasMany
    {
        return $this->hasMany(Project::class);
    }
```

- migrare e fare il seed della tabella types
```bash
php artisan migrate
php artisan db:seed --class=TypeSeeder
```