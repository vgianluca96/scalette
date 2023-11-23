# Scaletta condivisione repo tra collaboratori

## Per invitare collaboratori alla repo
`settings > collaborators > add people`

## Se sono invitato come collaboratore
- Clono la repo
```bash
git clone link-github-repo nome-folder-locale
```
- Installo composer
```bash
composer install
```
- Installo npm
```bash
npm install
```
- Copia di `.env.example` e lo rinomino in `.env`
- Genero la chiave
```bash
php artisan key:generate
```
- Avvio i server
```bash
php artisan serve
npm run dev

# se c'è anche l'app frontend apro un nuovo terminale
npm run dev
```

## Per fare modifiche su un branch diverso dal main
- Si crea nuovo branch 
```bash
git checkout -b nome-branch
```
Dopo aver fatto le modifiche al codice
- Si mettono in stage le modifiche e si committa
```bash
git add .
git commit -m"messaggio"
```
- Si pubblica il branch
```bash
git push
```
- L'amministratore della repo farà compare & pull request (e gestirà eventuali conflitti)

## Per importare in locale modifiche pubblicate da altri
```bash
git checkout main
```
```bash
git pull
```

## Se sono state fatte modifiche ai pacchetti
```bash
composer install
```
```bash
npm install
```

