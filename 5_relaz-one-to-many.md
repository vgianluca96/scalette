# Scaletta relazioni one-to-many

- NOTA: assumo una relazione one-to-many tra i model `Type` e `Project`

- Creare model `Type` con migrazione e seeder
```bash
php artisan make:model Type -ms
```

- Impostare migrazione e Seeder di `Type`

- Creare la migration di modifica per la tabella `projects` per aggiungere la chiave esterna
```bash
php artisan make:migration add_type_id_foreign_key_to_projects_table
```

- All'interno della migration si dovrà avere
```php
public function up(): void
    {
        Schema::table('projects', function (Blueprint $table) {
            $table->unsignedBigInteger('type_id')->nullable()->after('id');

            $table->foreign('type_id')
                ->references('id')
                ->on('types')
                ->cascadeOnDelete();
                // cascadeOnDelete serve per propagare su projects la cancellazione di un type??
        });
    }

public function down(): void
    {
        Schema::table('projects', function (Blueprint $table) {
            $table->dropForeign('projects_type_id_foreign');
        });
    }
```

- Aggiungere ai model `Project` e `Type` i metodi per definire la relazione one to many

- in `Project`
```php
 public function type(): BelongsTo
    {
        return $this->belongsTo(Type::class);
    }

```

- in `Type`
```php
public function projects(): HasMany
    {
        return $this->hasMany(Project::class);
    }
```

- Migrare e fare il seed della tabella `types`
```bash
php artisan migrate
php artisan db:seed --class=TypeSeeder
```