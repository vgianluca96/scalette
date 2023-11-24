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

## Processo di aggiornamento di un progetto tra più collaboratori
- [`Github`] L'amministratore della repo crea delle `issue` basate sulla suddivisione del lavoro; ogni `issue` sarà assegnata ad un collaboratore
- [`VS Code`] Ogni collaboratore crea un nuovo branch
```bash
git checkout -b nome-branch
```
- Dopo aver fatto le modifiche al codice si pubblica il branch con la solita procedura
```bash
git add .
git commit -m"messaggio"
git push
```
- [`Github`] Si crea `pull request` associandola alla `issue` (può essere fatto sia dal collaboratore sia dall'amministratore)
- L'amministratore farà il `merge` della `pull request` (gestendo eventuali conflitti)

## Per importare in locale modifiche pubblicate da altri
- Ritorno al branch `main`
```bash
git checkout main
```
- pull delle modifiche
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

