# clonare repo esistente

- comando git clone
```bash
git clone link-github-vecchia-repo nome-nuova-repo-locale
```

- rimozione cartella .git vecchia
```bash
rm -rf .git
```

- inizializzazione, add e commit
```bash
git init
git add .
git commit -m"messaggio"
```

- comandi di inizializzazione della repo
```bash
git remote add origin https://github.com/vgianluca96/example-address.git
git branch -M main
git push -u origin main
```

- installazione composer
```bash
composer install
```

- copia incolla .env-example e rinomina in .env

- generazione chiave
```bash
php artisan key:generate
```

- comando storage link
```bash
php artisan storage:link
```

- installazione npm
```bash
- npm install
```