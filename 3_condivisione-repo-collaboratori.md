# Scaletta condivisione repo tra collaboratori

## Per invitare collaboratori alla repo
`settings > collaborators > add people`

## Se sono invitato come collaboratore
- Clono la repo
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
- Avvio il server
```bash
php artisan serve e npm run dev
```

## Per fare modifiche su un branch diverso dal main
- Si crea nuovo branch 
```bash
git checkout -b nome-branch
```
- Si fanno le modifiche
- Si committa
- Si pubblica il branch
- Si fa compare & pull request

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

