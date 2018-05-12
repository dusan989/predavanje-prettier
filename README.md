# Šta je Prettier?

Prettier je formater koda koji podržava razne jezike kao što su:
- JavaScript, uključujući i ES2017,
- JSX,
- Flow,
- TypeScript,
- CSS, SCSS,
- JSON,
- GraphQL.

U toku je i razvoj podrške i za druge jezike, kao što su PHP, Python, Ruby, Java, Elm itd.

Integriše se u mnoge editore:
- WebStorm i PhpStorm,
- Atom,
- Vim...

Takođe, ovaj formater koda je i konfigurabilan. Recimo može da se setuje željena maksimalna dužina linije koda.

Cilj Prettier-a je da sve promene koje unese u kod ne pokvare sam kod, tj. kod će isto da radi i pre i posle upotrebe Prettier-a. Zato, recimo, ne postoji mogućnost da Prettier automatski sortira propertije unutar objekata po abecedi jer sortiranje potencijalno može da dovede da kod prestane da radi.

# Zašto Prettier

Pomoću Prettier-a može da se definiše i lako kontroliše zajednički stil koda unutar projekta. Olakšava code review jer se onda onaj ko gleda kod ne bavi stilom koda (recimo "Ovde fali ;") već se fokusira na kvalitet koda i na ono što kod treba da radi. Takođe, olakšava pisanje koda jer programer ne mora da manuelno sređuje izgled koda, što donekle ubrzava rad.

# Prettier i linter

Važno je napomenuti da Prettier __nije__ alternativa linterima, već dopuna. Kod lintera (ESLint, TSLint) postoje 2 grupe pravila:
- _Formatting rules_, kao što su maksimalna dužina linije, mešanje spejsa i tabova itd. Ovde Prettier dolazi do izražaja jer automatski ispravlja greške ovog tipa.
- _Code-quality rules_, kao što su neiskorišćene variable itd. Ovde Prettier ne pomaže i na programeru je da ispravi ove greške. Važno je napomenuti da ove greške mnogo više dovode do bagova u kodu od formaterskih.

Postoje dodaci za ESLint, koji između ostalog isključuju neka ESLint pravila koja mogu da naprave problem Prettier-u.

Važno je napomenuti, kada se koristi kombinacija eslint i Prettier, da može da dođe do kolizije nekih pravila. Recimo, u eslint-u je namešteno pravilo da moraju da se koriste samo jednostruki navodnici, dok u Prettieru samo dvostruki. U tim slučajevima mogu da se orade sledeće neke od sledećih predloga:

- Da se isključe sva pravila u eslint-u koja mogu da naprave problem Prettier-u.
- Da se promene opcije u Prettier-u tako da budu u skladu sa eslint-om.

# Kako se instalira

Instalira se jednostavno kao dev dependency unutar projekta:

```bash
npm install --save-dev --save-exact prettier
```

# Pokretanje

Prettier se pokreće pomoći komande:
```
prettier [options] [filename ...]
```

Na primer:
```
prettier --single-quotes --trailing-comma es5 --print-width 80 --write "src/**/*.js"
```
će formatirati sve JS fajlove unutar `src` foldera, tako što će se koristiti jednostruki navodnici, dodaju se trailing commas u skladu sa ES5 (na kraju objekata i nizova, ali ne i na kraju definicije argumenata funkcije), a maksimalna dužina linije koda će biti 80 karaktera.

Takođe, Prettier se može namestiti da se pokreće automatski pre svakog komita i za to postoji više načina opisanih na sajtu Prettier-a.

Postoji paket `eslint-plugin-prettier` koji služi za integraciju Prettier-a i eslint-a, tako da se ispravljanje koda po prettier pravilima može postići i komandom `eslint --fix`. Više o tome ima na sledećem [linku](https://github.com/prettier/eslint-plugin-prettier).

# Opcije

Na stranici https://prettier.io/docs/en/options.html se nalazi spisak mogućih opcija kao i mogućih vrednosti datih opcija.

Umesto da se opcije proselđuju kroz terminal, može se kreirati konfiguracioni fajl i to na više načina:
- `.prettierrc` napisan kao YAML ili JSON sa opcionim ekstenzijama `.yaml/.yml/.json/.js`,
- `prettier.config.js`,
- `prettier` properti unutar `package.json`.

Moguće je takođe promeniti postojeća pravila za određene grupe fajlova.

## Primeri konfiguracionih fajlova

- [React](https://github.com/facebook/react/blob/master/.prettierrc.js).

# Primer kako da se uklone konflikti između eslint-a i Prettier-a

Jedan od načina da se uklone konflikti je da se usključe eslint pravila koja mogu da su u suprotnosti sa Prettier-om. Postoji oficijalni paket `eslint-config-prettier` koji služi upravo za to, da onemogući pravila koja mogu da prave problem Prettier-u. To se postiže jednostavnim podešavanje eslint konfiguracionog fajla. Uz pomoć komande `eslint --print-config .eslintrc | eslint-config-prettier-check`se može proveriti dal određena pravila prave problem Prettier-u ili ne.

# [IDE](https://en.wikipedia.org/wiki/Integrated_development_environment)

Jedan od načina korišćenja Prettier-a je da se pokreće automatski prilikom svakog čuvanja fajla. To se može postići pomoću integracije Prettier-a i editora.

## Atom

Paket `prettier-atom`.

## WebStorm, PHPStorm

Plugin `prettier` za verzije 2018.1 i više. Da se formitira fajl komanda je `Alt-Shift-Cmd-P`.

Da bi se kod automatski formatirao na _save_, treba odraditi sledeće:

- Idi na `Preferences/Tools/File Watchers` i klikni `+` da dodaš novi _watcher_ i daj mu ime `Prettier`.
  - `File Type`: `JavaScript`,
  - `Scope`: `Project Files`,
  - `Program`: puna putanja do `.bin/prettier`.
  - `Arguments`: `--write [other options] $FilePathRelativeToProjectRoot$`,
  - `Output paths to refresh`: `$FilePathRelativeToProjectRoot$`
  - `Working directory`: `$ProjectFileDir$`.
  - `Auto-save edited files to trigger the watcher`: isključiti.

