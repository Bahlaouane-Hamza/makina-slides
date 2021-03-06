---
title: Formation Angular
theme: theme/makina.css
verticalSeparator: <!--v-->
revealOptions:
    transition: 'slide'
---

<!-- .slide: class="title logo" data-background="#b5b42c" -->

# Formation Angular
#### Éric Bréhault / Alexandra Janin


---

<!-- .slide: class="title" data-background="./_assets/theme/images/bg-rocks.jpg" -->

# 1. Présentation générale

<!--v-->

# La version 2 ?

* AngularJS est la version 1.
* À partir de la verison 2, le nom devient "Angular".
* La version 4 est parue en mars 2017.
* La version 5 est parue le 1er novembre 2017
* Depuis la version 2, pas de "gros" changement, continuité d'Angular 2.

Note: parler du sémantic versionning

<!--v-->

# Quel type de framework ?

- Spectre large
- Très structurant
- Fortement contraint


Note: expliquer les différences avec React

<!--v-->

<!-- .slide: data-background="./_assets/images/framework-library.svg" data-background-size="90% 90%" -->

<!--v-->

# Les grands principes

- Composants
- TypeScript
- build


---

<!-- .slide: class="title" data-background="./_assets/theme/images/bg-rocks.jpg" -->

# 2 - Installation et tooling

<!--v-->

# Installation

Installer NodeJS (6.9+) et NPM.

https://nodejs.org/en/download/

Installer Angular CLI:

```js
npm install -g @angular/cli
```

Accessoirement les extensions Chrome suivantes peuvent être utiles:

- Postman
- JSONViewer
- Allow-Control-Allow-Origin: *

<!--v-->

# Le CLI

Permet de générer un projet.

```js
ng new myproject # ET SI BESOIN --style=scss
cd myproject
ng serve
``` 

Fournit des facilités pour le développement.

```js
ng serve
```
    
Permet d'y ajouter des composants, des services, des sous-modules...

```js
ng generate component MyComponent
```

<!--v-->

<!-- .slide: class="alternate" -->

# Installation et premiers pas

- Installer NodeJS et Angular CLI
- Créer un projet
- Le lancer avec `ng serve` et ouvrir http://localhost:4200
- Observer la structure de fichiers générée

[Solution](https://github.com/makinacorpus/angular-training/commit/80e450d4fa2d580b4a7d3976ccb61ee5c9210571)

---

<!-- .slide: class="title" -->

# 3 - Les concepts

- TypeScript
- Component
- Module
- Injection

<!--v-->

# Typescript

- TypeScript superset d'ES6 superset d'ES5
- Un "compilateur" fait la conversion
- Apporte :
    - le typage
    - les classes
    - les décorateurs

<!--v-->

# Component

* Un composant est **une classe** avec un décorateur `@Component`.
* Il correspond à un **tag**.
* Il a un **template HTML**, éventuellement des fichiers de style.

<!--v-->

# Module

Le module est le point d'entrée de l'app Angular.

* Il déclare les composants.
* Il charge les modules tiers.
* Il définit les injections disponibles.

<!--v-->

# Injections

Les composants ont besoin de services globaux (persistence des données, accès au backend, routage, etc.).

Ils les obtiennent par **injection de dépendances** :
Angular va instancier les services dont on a besoin, et les fournir aux composants.

* Les services injectables sont des classes ayant le décorateur `@Injectable`.
* Ils sont déclarés dans le module (dans `providers`).
* Ils sont fournis aux composants dans leur constructeur.

---

# 4 - Créer un composant simple

Le CLI permet de générer de nouveaux composants dans l'app.

Nous allons créer un composant pour la page d'accueil :

```js
ng generate component Home
```

<!--v-->

# Utiliser un composant

Le décorateur `@Component` associe un sélecteur (`selector`) au composant.

C'est sous ce nom qu'on peut utiliser le composant dans nos templates HTML.

Dans le cas présent : 

```html
<app-home></app-home>
```

[Exemple](https://github.com/makinacorpus/angular-training/commit/b0a2943baf7a0a704770cc984095ff9e7ad41ad0)

Note:
Objectif: remplacer le contenu actuel de notre app component par notre composant home

On devrait voir "home works!" s'afficher

<!--v-->

# Créer un input

Le décorateur `@Input` permet de déclarer un nouvel attribut pour le composant :

```typescript
@Input() message: string;
```

déclare un attribut `message` qu'on peut utiliser de cette façon :

```html
<app-home message="Bonjour !"></app-home>
```

[Exemple](https://github.com/makinacorpus/angular-training/commit/46961b6ffe7838cd0857e75c5fa33fb5330896f1)

<!--v-->

# Lier (i.e. binding) un input à une propriété

Plutôt qu'un message écrit en dur, nous souhaitons passer la valeur d'une propriété du composant `App` :

```typescript
welcomeMessage: string = 'Bienvenue ici';
``` 

Pour cela, on doit noter l'input entre crochet :

```html
<app-home [message]="welcomeMessage"></app-home>
```

(sinon le message serait littéralement "welcomeMessage" et non pas "Bienvenue ici")

[Exemple](https://github.com/makinacorpus/angular-training/commit/97cb262cbb8187185427b9dcaa824281a2266666)

<!--v-->

# Lier un événement à une méthode

Ajoutons au composant Home une méthode qui modifie le message :

```typescript
changeMessage() {
    this.message = 'This is a new message';
}
```

On souhaite appeler cette méthode lorsqu'on clique sur un bouton.
Pour cela, on note l'événement entre parenthèses:

```html
<button (click)="changeMessage()">Change the message</button>
```

[Exemple](https://github.com/makinacorpus/angular-training/commit/56229b8566dcee1309b1a87e08e3b8b32fe2f17a)

<!--v-->

# Input et output

### input

* Les **inputs** sont les informations qui sont fournies en entrée au composant.
* Ils sont notés entre crochets.


### output

* Les **outputs** sont les informations qui sont émises par le composant.
* Ils sont notés entre parenthèses.

<!--v-->

# two-way data-binding

Pour les éléments pouvant être fournis en entrée et émis en sortie, on peut utiliser **les 2 notations ensemble : `[()]`**, par exemple pour un champ de formulaire. C'est le two-way data binding. 

En dehors de l'usage élémentaire pour des formulaires, on préfère l'éviter pour des raisons de performance.

<!--v-->

# Syntaxe des templates

Interpolation `{{ }}` : permet d'évaluer une expression.

Par exemple :

    {{ 1 + 2 }} // affiche 3
    {{ message }} // affiche la valeur de la propriété `message` du composant

<!--v-->

<!-- .slide: class="alternate" -->

# Syntaxe des templates

Boucle avec `*ngFor`

Note : les directives structurelles (celles qui utilisent un template propre) sont préfixées par `*`.

Créons une propriété contenant une liste de pokémons et affichons-les dans une liste :

    <ul>
      <li *ngFor="let pokemon of pokemons">{{pokemon.name}}</li>
    </ul>

[Exemple](https://github.com/makinacorpus/angular-training/commit/1497421547e9f87c53d751cb4021eb9a65fed15a)


<!--v-->

<!-- .slide: class="alternate" -->

# Syntaxe des templates

Condition avec `*ngIf` ou `*ngSwitch`

On va créer une méthode qui indique si un pokémon est le plus fort :

    isStronger(pokemon:any) {
        let max_pv = Math.max(...this.pokemons.map(pok => pok.pv));
        return (pokemon.pv == max_pv)
    }

Et on utilise cette méthode dans un `*ngIf` pour afficher une mention à côté du plus fort :

    <strong *ngIf="isStronger(pokemon)">Le plus fort !!</strong>

[Exemple](https://github.com/makinacorpus/angular-training/commit/3117c8f9dcd9391b4819ddddec34f9e60d4e7875)


<!--v-->

# Syntaxe des templates

Classes (et style) avec `ngClass`  (ou `ngStyle`)

On peut directement lier l'attribut normal `class` :

    private defaultClasses:string = 'btn btn-primary btn-special';

    <div [class]="defaultClasses"></div>

On peut aussi conditionner une classe avec un booléen:

    <div [class.alert]="isAlert"></div>

<!--v-->

# Syntaxe des templates

Mais la directive `ngClass` est souvent plus simple car elle permet de gérer plusieurs classes dans un dictionnaire :

    getClasses(pokemon:any) {
        return {
            grass: pokemon.type == 'grass',
            electric: pokemon.type == 'electric',
            fire: pokemon.type == 'fire',
            small: pokemon.size < 50,
            medium: pokemon.size < 100 && pokemon.size >= 50,
            big: pokemon.size >= 100
        }
    }

    <li [ngClass]="getClasses(pokemon)">

[Exemple](https://github.com/makinacorpus/angular-training/commit/5315799cab6638ac0b37507671fc7f034e2abd75)


<!--v-->

# Syntaxe des templates

**Les pipes** permettent d'appliquer des transformations simples au résultat d'une interpolation.

Exemples :

Formatter une date :

    {{ dateObj | date }}
    {{ dateObj | date:'shortTime'}}

Formatter des valeurs monétaires :

    {{ price | currency:'EUR':true }}


---

<!-- .slide: class="title" data-background="./_assets/theme/images/bg-rocks.jpg" -->

# 5 - Gérer le routage

<!--v-->

Le routage permet de naviguer de "pages" en "pages" dans notre application
(en fait, physiquement on reste sur la même page `index.html`).

Le principe est de mettre en correspondance des URLs avec des composants :

    { path: '', component: HomeComponent, pathMatch: 'full' },
    { path: 'about', component: AboutComponent, pathMatch: 'full' },

Le composant sera affiché dans un point d'insertion qu'on met généralement dans
le template de l'AppComponent :

    <router-outlet></router-outlet>

<!--v-->

ng generate module app-routing --flat --module=app


<!-- .slide: class="alternate" -->

# Exercice

Créons un composant About et utilisons des routes pour atteindre soit Home soit About.

Solution : [Mettre en place le routage](https://github.com/makinacorpus/angular-training/commit/8e6882cbd15c00782c00b4081d60134e2205c8af) [Ajouter About et sa route](https://github.com/makinacorpus/angular-training/commit/c1bd0305a0b33c390894d459ca378ba52054a728)

Note: Pour la mise en place, penser à :
- importer Routes et RouterModule de @angular/router
- déclarer une variable de type Routes
- ajouter RouterModule.forRoot(routes) dans les imports du module
- remplacer le contenu de app.component par router-outlet

<!--v-->

# Liens vers des routes

On peut très bien faire un lien normal vers une route :

    <a href="/about">About</a>

Mais cela va recharger la page en entier. [Exemple](https://github.com/makinacorpus/angular-training/commit/dd9f0ff32e3a1947a711dd75f4712a3c36f8ed9c)

Si on veut rendre le routage dynamique, on utilise la directive `routerLink`:

    <a routerLink="/about">About</a>

[Exemple](https://github.com/makinacorpus/angular-training/commit/e4727d2073d9c07345a7f468aff0081955dcc6ab)

<!--v-->

# Routes paramétrées

On peut déclarer des routes contenant des paramètres :

    { path: 'pokemon/:id', component: DetailComponent }

<!--v-->

### Routes paramétrées

Le module `Router` fournit des services **injectables** (c'est-à-dire appelables depuis n'importe quel composant).
Notamment le service `ActivatedRoute` qui permet d'obtenir des informations sur la route courante.

Le composant cible peut recevoir le (ou les) paramètre(s) d'une route en souscrivant (=subscribe)
à des **Observables** proposés par `ActivatedRoute`.

Dans le cas d'un paramètre, on utilise l'observable `params` :

    this.route.params.subscribe(params => {
        console.log(params['id']);
    }

<!--v-->

<!-- .slide: class="alternate" -->

# Exercice

On va créer [une page de présentation détaillée d'un pokémon](https://github.com/makinacorpus/angular-training/commit/cedc5088d9bfdf4c20c770ff36fc4916ce171dea) et faire [des liens depuis la page d'accueil](https://github.com/makinacorpus/angular-training/commit/4739011b62b8852a9181f61776856bf7d2970287).

Note : auparavant on va mettre les données sur les pokémons dans un [fichier de configuration](https://github.com/makinacorpus/angular-training/commit/9d771856bb09642bd34aa146d2bc4ed2ed255a6f)

Note: 
- créer le composant Detail
- importer ActivatedRoute dans DetailComponent et Location (@angular/common)
- déclarer dans le constructeur :
  - private route: ActivatedRoute,
  - private location: Location,
- prévoir un message d'erreur si pas de pokemon

---

# 6 - Appels au backend

Plutôt que gérer des informations localement, on souhaite utiliser l'API publique [http://pokeapi.co/](http://pokeapi.co/).

Pour cela on va utiliser le service [`HttpClient`](https://angular.io/guide/http) :

```js
this.http.get<any>('http://pokeapi.co/api/v2/pokemon/')
``` 

Les méthodes (get, post, etc.) de `http` renvoient des observables auquel on souscrit pour obtenir les données.

```js
this.http.get<any>('http://pokeapi.co/api/v2/pokemon/')
.subscribe(res => {
    this.pokemons = res.results
});
```


<!--v-->

<!-- .slide: class="alternate" -->

# Exercice

* Utiliser [http://pokeapi.co/api/v2/pokemon/](http://pokeapi.co/api/v2/pokemon/) pour avoir la liste des Pokémons sur la page d'accueil.
* Utiliser [http://pokeapi.co/api/v2/pokemon/:id](http://pokeapi.co/api/v2/pokemon/:id) pour avoir les informations d'un pokémon.

[Solution](https://github.com/makinacorpus/angular-training/commit/148f491a8719f52ae571b3b007632c0c40ea13ec)

L'API est lente, il faut faire un spinner (ou un message de chargement).

[Solution](https://github.com/makinacorpus/angular-training/commit/a96e4534e6e6b8a451edca21272abb7f192fdd57)

<!--v-->

# Créer un Injectable

Plutôt qu'appeler `http` directement dans nos composants, il est plus sain de déléguer
tous les appels à un **service injectable** spécifique.

Cela peut faciliter la maintenance (meilleure isolation), ou permettre de greffer
des améliorations (gérer l'authentification, le cache, le mode offline, etc.).

Pour cela, il faut créer un service :

    ng generate service MyService


- y injecter `http`,
- déclarer le service en tant que `provider` dans le module,
- et injecter notre service dans nos composants.

<!--v-->

# Observables

Grâce à l'opérateur `map` de **RxJS**, on peut appliquer une fonction sur un observable
comme si c'était une liste normale et le résultat reste un observable.

C'est ainsi qu'on peut **appliquer des traitements sur le résultat d'un appel HTTP**.

<!--v-->

<!-- .slide: class="alternate" -->

# Exercice

Créer un service fournissant une méthode `listAll()` et une méthode `get(id)`.

[Solution](https://github.com/makinacorpus/angular-training/commit/ebb28f8776b124cb0a66cb5fd42dba10cd6255df)

<!--v-->

<!-- .slide: class="alternate" -->

# Exercice

Stocker le résultat de `listAll()` en cache.

[Solution](https://github.com/makinacorpus/angular-training/commit/faa6d6c7511c7c2921d11c763de11e401ce9d31e)

---

# 7 - Gérer des formulaires

Le `FormsModule` fournit des directives disponibles par défaut sur tous les `<form>` :

- `ngForm`, l'objet form lui-même (avec notamment ses valeurs)
- `ngModel`, le binding avec les champs
- `ngSubmit`, l'action de submit

<!--v-->

# Exemple

    <form #f="ngForm" (ngSubmit)="doSomething(f.value)">
        <input type="text" name="email" ngModel />
        <input type="text" name="username" [ngModel]="currentUser" />
        <button (click)="ngSubmit">Go</button>
    </form>

Et dans le composant :

    doSomething(data) {
        console.log(data.email);
        console.log(data.username);
    }

<!--v-->

<!-- .slide: class="alternate" -->

# Exercice

Faire un formulaire dans la page About qui permet de signaler un pokémon manquant.

Note : comme on ne dispose pas d'un endpoint pour envoyer des mails, on va simplement écrire dans le log notre message.

[Solution](https://github.com/makinacorpus/angular-training/commit/7745d273a17065ed714a3a13585f3e4822d9ef6a)

<!--v-->

# Accèder au formulaire depuis le composant

    import { NgForm } from '@angular/forms';
    ...
        @ViewChild('f') form: NgForm;

        ngOnInit() {
          this.form.valueChanges.subscribe(v => console.log(v));
        }

<!--v-->

# Debounce sur la saisie

    import { NgForm } from '@angular/forms';
    ...
        @ViewChild('f') form: NgForm;

        ngOnInit() {
          this.form.valueChanges
            .debounceTime(1000)
            .subscribe(v => console.log(v));
        }

<!--v-->

# ViewChild

ViewChild permet d'accèder aux composants contenus dans le composant courant.

C'est un décorateur qui prend en paramètre soit une classe:

    @ViewChild(ContactComponent) contact: ContactComponent;

soit une référence de template:

    @ViewChild('f') form: NgForm;

<!--v-->

# RxJS et les observables

Faire un observable à partir d'une valeur simple:

    import { Observable } from 'rxjs/Observable';
    import 'rxjs/add/observable/of';

    Observable.of('Bonjour')

<!--v-->

<!-- .slide: style="font-size: 80%" -->

## Combiner 2 observables

`forkJoin` renvoie les données dès qu'il a obtenu une réponse de chacun.

`combineLatest` renvoie des données à partir du moment où il a obtenu au moins une réponse de chacun et à chaque nouvelle réponse d'un d'entre eux.

```
import { Observable } from 'rxjs/Observable';
import 'rxjs/add/operator/map';
import 'rxjs/add/observable/of';
import 'rxjs/add/observable/forkJoin';

Observable.forkJoin(
observable1,
observable2
).map(([result1, result2]) => ...)

Observable.combineLatest(
observable1,
observable2
).subscribe(([result1, result2]) => ...)
```

<!--v-->

## Chaîner 2 observables

`mergeMap` (ou `concatMap` pour des événements répétables)

```typescript
return this.http.get<any>(this.baseUrl + id + '/')
    .map(res => {
        this.status.next({ loading: false, error: null });
        return res;
    }).mergeMap(res => {
    return Observable.forkJoin(
        Observable.of(res),
        this.http.get<any>(res.abilities[0].ability.url).map(res2 => res2));
    });
```

<!--v-->

# RxJS et les subjects

Les subjects sont des observables et des observateurs à la fois, on peut s'y abonner et leur envoyer des données.

```typescript
const subject = new Subject<number>();

subject.subscribe((number) => {
    console.log(number);
});

subject.next(1);
subject.complete();
```

<!--v-->

# RxJS et les subjects

BehaviorSubject renvoie la dernière valeur même si elle a été émise avant le subscribe.
Il a forcément une valeur intiale.

<!--v-->

# RxJS et les subjects

**AsyncSubject** ne garde que sa dernière valeur et l'émet à partir du moment où on l'arrête.

```typescript
protected vocabulary: AsyncSubject = new AsyncSubject();

constructor(private api: HTTPService) {
    this.api.get('/vocabulary/')
        .map((response: Response) => {
            this.vocabulary.next(response.json());
            this.vocabulary.complete();
        });
}
```

---

# 8 - Tester une app

Par défaut, le CLI génère un fichier .spec.ts pour chaque composant.

Ce fichier contiendra tous les tests unitaires du composant.

Les tests sont lancés par la commande :

```
ng test
```


<!--v-->

# Test d'acceptance

## RobotFramework

- les tests sont rédigés en langage naturel,
- ils sont interprétés par Selenium.

<!--v-->

# Installation

Installer Python 2.7 puis :

```
easy_install pip
pip install robotframework
pip install robotframework-selenium2library
pip install robotframework-debuglibrary
```

ChromeDriver à ajouter dans le dossier bin de Python :
http://chromedriver.storage.googleapis.com/index.html

---

# 9 - Ajouter des dépendences externes

On ajoute des dépendences avec :

```
npm install nom-du-paquet
npm install nom-du-paquet@3.0.2
npm install nom-du-paquet --save
```

Note : `--save` sert à ajouter une entrée dans `package.json`.

<!--v-->

<!-- .slide: class="alternate" -->

# Exercice

Ajouter Material Design

[Voir la doc](https://material.angular.io/guide/getting-started)

[Solution](https://github.com/makinacorpus/angular-training/commit/365ea6bb474e7950392c3c2475f164bfa5b58e97)


---

# 10 - Déployer en prod

Pour créer un build :

    ng build

Pour un build minifié avec cache keys:

    ng build --prod

Si le déploiement n'est pas fait à la racine :

    ng build --base-href=/pokemon

<!--v-->

# Respecter le routage

Le routage de l'app Angular doit être accepté par le serveur web :

- si l'utilisateur est sur une URL donnée produite par le routage Angular, et qu'il rafraîchit la page, il ne doit pas obtenir un 404,
- idem pour les bookmarks ou les liens partagés par mail ou autre.

Il faut donc un vhost qui renvoie toutes les URLs sur index.html (sauf les ressources JS/CSS/assets).

Exemple Nginx:

```
location / {
    try_files   $uri $uri/ /index.html;
}
```

<!--v-->

# Angular Universal

Permet de faire tourner l'app Angular sur un serveur afin de servir la page demandée en HTML pré-généré :

- augmente les performances,
- gère le routage,
- améliore le référencement.

La page servie est ensuite ré-hydratée : le JS s'active et la suite de la navigation se fait en mode client-side.
