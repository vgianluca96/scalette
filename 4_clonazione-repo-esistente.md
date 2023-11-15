# Clonazione repo esistente

- Comando git clone
```bash
git clone link-github-vecchia-repo indirizzo-locale-nuova-folder
```

- Rimozione cartella .git vecchia
```bash
rm -rf .git
```

- Inizializzazione, add e commit
```bash
git init
git add .
git commit -m"messaggio"
```

- Comandi di inizializzazione della repo
```bash
git remote add origin https://github.com/vgianluca96/nome-repo.git
git branch -M main
git push -u origin main
```

- Installazione composer
```bash
composer install
```

- Copia incolla `.env-example` e rinomina in `.env`

- Generazione chiave
```bash
php artisan key:generate
```

- Comando storage link
```bash
php artisan storage:link
```

- Installazione npm
```bash
- npm install
```