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
- copia di `.env.example` e lo rinomino in `.env`
```bash
php artisan key:generate
```
```bash
php artisan serve e npm run dev
```

## Per fare modifiche su un branch diverso dal main
- si crea nuovo branch 
```bash
git checkout -b nome-branch
```
- si fanno le modifiche
- si committa
- si pubblica il branch
- si fa compare & pull request

## Per importare modifiche pubblicate da altri
```bash
git checkout main
```
```bash
git pull
```

## se sono state fatte modifiche ai pacchetti
```bash
composer install
```
```bash
npm install
```

