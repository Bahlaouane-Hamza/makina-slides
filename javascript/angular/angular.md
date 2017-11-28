---
title: Formation Angular
theme: ./assets/theme-reveal/makina.css
revealOptions:
    transition: 'fade'
---
<!-- .slide: class="title" data-background="#b5b42c" -->

# Formation Angular
#### Éric Bréhault / Alexandra Janin

![](../../assets/theme-reveal/makina-corpus.svg)

---

<!-- .slide: class="title" data-background="./assets/images/bg-rocks.jpg" -->

# 1. Présentation générale

---

# La version 2 ?

* AngularJS est la version 1.
* À partir de la verison 2, le nom devient "Angular".
* La version 4 est parue le 23 mars 2017 (et reste dans la continuité d'Angular 2).

Note: parler du sémantic versionning

---

# Quel type de framework ?

- Spectre large
- Très structurant
- Fortement contraint


Note: expliquer les différences avec React

---

# Les grands principes

- Composants
- TypeScript
- build


---

<!-- .slide: class="title" data-background="./assets/images/bg-rocks.jpg" -->

# 2 - Installation et tooling

---

# Installation

Installer NodeJS (6.9+) et NPM.

https://nodejs.org/en/download/

Installer Angular CLI:

    npm install -g @angular/cli

Accessoirement les extensions Chrome suivantes peuvent être utiles:

- Postman
- JSONViewer
- Allow-Control-Allow-Origin: *

---

# Le CLI

Permet de générer un projet.

    ng new myproject # ET SI BESOIN --style=scss
    cd myproject
    ng serve

Fournit des facilités pour le développement.

    ng serve
    
Permet d'y ajouter des composants, des services, des sous-modules...

    ng generate component MyComponent

---

<!-- .slide: class="alternate" -->

# Installation et premiers pas

- Installer NodeJS et Angular CLI
- Créer un projet
- Le lancer avec `ng serve` et ouvrir http://localhost:4200
- Observer la structure de fichiers générée

[Solution](https://github.com/makinacorpus/angular-training/commit/f865cbf4660be2087da7e6925db7ff931bd833b6)


---

<!-- .slide: class="title" -->

# 3 - Les concepts

- TypeScript
- Component
- Module
- Injection

---

# Typescript

- TypeScript superset d'ES6 superset d'ES5
- Un "compilateur" fait la conversion
- Apporte :
    - le typage
    - les classes
    - les décorateurs

---

# Component

* Un composant est **une classe** avec un décorateur `@Component`.
* Il correspond à un **tag**.
* Il a un **template HTML**, éventuellement des fichiers de style.

---

# Module

Le module est le point d'entrée de l'app Angular.

* Il déclare les composants.
* Il charge les modules tiers.
* Il définit les injections disponibles.

---

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

    ng generate component Home

---

# Utiliser un composant

Le décorateur `@Component` associe un sélecteur (`selector`) au composant.

C'est sous ce nom qu'on peut utiliser le composant dans nos templates HTML.

Dans le cas présent : 

    <app-home></app-home>

[Exemple](https://github.com/makinacorpus/angular-training/commit/3c42e93cf619d0efacc8581db4dbd212a4606902)

---

# Créer un input

Le décorateur `@Input` permet de déclarer un nouvel attribut pour le composant :

    @Input() message: string;

déclare un attribut `message` qu'on peut utiliser de cette façon :

    <app-home message="Bonjour !"></app-home>

[Exemple](https://github.com/makinacorpus/angular-training/commit/75741b6aeb7b9338ebf80cb98ce2e4855ec3259c)

---

# Lier (i.e. binding) un input à une propriété

Plutôt qu'un message écrit en dur, nous souhaitons passer la valeur d'une propriété du composant `App` :

    welcomeMessage: string = 'Bienvenue ici';

Pour cela, on doit noter l'input entre crochet :

    <app-home [message]="welcomeMessage"></app-home>

(sinon le message serait littéralement "welcomeMessage" et non pas "Bienvenue ici")

[Exemple](https://github.com/makinacorpus/angular-training/commit/963495ca67318867b8b8d3ce67f0ccf63805e88e)

---

# Lier un événement à une méthode

Ajoutons au composant Hom une méthode qui modifie le message :

    changeMessage() {
       this.message = 'This is a new message';
    }

On souhaite appeler cette méthode lorsqu'on clique sur un bouton.
Pour cela, on note l'événement entre parenthèses:

    <button (click)="changeMessage()">Change the message</button>

[Exemple](https://github.com/makinacorpus/angular-training/commit/6682d3ace9ad190878db075880e1d0745116c520)

---

# Input et output

### input

* Les **inputs** sont les informations qui sont fournies en entrée au composant.
* Ils sont notés entre crochets.


### output

* Les **outputs** sont les informations qui sont émises par le composant.
* Ils sont notés entre parenthèses.

---

# two-way data-binding

Pour les éléments pouvant être fournis en entrée et émis en sortie, on peut utiliser **les 2 notations ensemble : `[()]`**, par exemple pour un champ de formulaire. C'est le two-way data binding. 

En dehors de l'usage élémentaire pour des formulaires, on préfère l'éviter pour des raisons de performance.

---

# Syntaxe des templates

Interpolation `{{ }}` : permet d'évaluer une expression.

Par exemple :

    {{ 1 + 2 }} // affiche 3
    {{ message }} // affiche la valeur de la propriété `message` du composant

---

<!-- .slide: class="alternate" -->

# Syntaxe des templates

Boucle avec `*ngFor`

Note : les directives structurelles (celles qui utilisent un template propre) sont préfixées par `*`.

Créons une propriété contenant une liste de pokémons et affichons-les dans une liste :

    <ul>
      <li *ngFor="let pokemon of pokemons">{{pokemon.name}}</li>
    </ul>

[Exemple](https://github.com/makinacorpus/angular-training/commit/f3f629d4d5781dcaee95c43733e042f527766768)


---

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

[Exemple](https://github.com/makinacorpus/angular-training/commit/c28624c8128f7fadba7e175958a963717e554e76)


---

# Syntaxe des templates

Classes (et style) avec `ngClass`  (ou `ngStyle`)

On peut directement lier l'attribut normal `class` :

    private defaultClasses:string = 'btn btn-primary btn-special';

    <div [class]="defaultClasses"></div>

On peut aussi conditionner une classe avec un booléen:

    <div [class.alert]="isAlert"></div>

---

# Syntaxe des templates

Mais la directive `ngClass` est souvent plus simple car elle permet de gérer plusieurs classes dans un dictionnaire :

    !typescript
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

[Exemple](https://github.com/makinacorpus/angular-training/commit/8431681ebd45a1de742b042259658fc9e32d8e43)


---

# Syntaxe des templates

**Les pipes** permettent d'appliquer des transformations simples au résultat d'une interpolation.

Exemples :

Formatter une date :

    {{ dateObj | date }}
    {{ dateObj | date:'shortTime'}}

Formatter des valeurs monétaires :

    {{ price | currency:'EUR':true }}


---

<!-- .slide: class="title" data-background="./assets/images/bg-rocks.jpg" -->

# 5 - Gérer le routage

---

Le routage permet de naviguer de "pages" en "pages" dans notre application
(en fait, physiquement on reste sur la même page `index.html`).

Le principe est de mettre en correspondance des URLs avec des composants :

    { path: '', component: HomeComponent },
    { path: 'about', component: AboutComponent },

Le composant sera affiché dans un point d'insertion qu'on met généralement dans
le template de l'AppComponent :

    <router-outlet></router-outlet>

---

<!-- .slide: class="alternate" -->

# Exercice

Créons un composant About et utilisons des routes pour atteindre soit Home soit About.

Solution : [Mettre en place le routage](https://github.com/makinacorpus/angular-training/commit/ad97a817a1e66819c21173bd4217f4d65985f803) [Ajouter About et sa route](https://github.com/makinacorpus/angular-training/commit/6715ca7fc3cd9a463bfc1aeb232d931788391efb)

.fx: alternate

---

# Liens vers des routes

On peut très bien faire un lien normal vers une route :

    <a href="/about">About</a>

Mais cela va recharger la page en entier. [Exemple](https://github.com/makinacorpus/angular-training/commit/21721389b3555f980d472f9f4035a787b6000d36)

Si on veut rendre le routage dynamique, on utilise la directive `routerLink`:

    <a routerLink="/about">About</a>

[Exemple](https://github.com/makinacorpus/angular-training/commit/323c37239af8ddc07b51bd68141475a8447c3cc3)

---

# Routes paramétrées

On peut déclarer des routes contenant des paramètres :

    { path: 'pokemon/:id', component: DetailComponent }

Le module `Router` fournit des services **injectables** (c'est-à-dire appelables depuis n'importe quel composant).
Notamment le service `ActivatedRoute` qui permet d'obtenir des informations sur la route courante.

Le composant cible peut recevoir le (ou les) paramètre(s) d'une route en souscrivant (=subscribe)
à des **Observables** proposés par `ActivatedRoute`.

Dans le cas d'un paramètre, on utilise l'observable `params` :

    this.route.params.subscribe(params => {
        console.log(params['id']);
    }

---

<!-- .slide: class="alternate" -->

# Exercice

On va créer [une page de présentation détaillée d'un pokémon](https://github.com/makinacorpus/angular-training/commit/3f74fcb655846f102aa782a693b2cc9f9323a429) et faire [des liens depuis la page d'accueil](https://github.com/makinacorpus/angular-training/commit/5d5ec1c2142722984346e17183cfffa80c6ce8aa).

Note: auparavant on va mettre les données sur les pokémons dans un [fichier de configuration](https://github.com/makinacorpus/angular-training/commit/a82ea22f7a53f08214a7a51411e8e4c2d42aa1f6)

---

# 6 - Appels au backend

Plutôt que gérer des informations localement, on souhaite utiliser l'API publique [http://pokeapi.co/](http://pokeapi.co/).

Pour cela on va utiliser le service `http` :

    this.http.get('http://pokeapi.co/api/v2/pokemon/')

Les méthodes (get, post, etc.) de `http` renvoient des observables auquel on souscrit pour obtenir les données.
La méthode `json()` permet de désérialiser les données reçues :

    this.http.get('http://pokeapi.co/api/v2/pokemon/')
    .subscribe(res => {
        this.pokemons = res.json().results
    });


---

<!-- .slide: class="alternate" -->

# Exercice

* Utiliser [http://pokeapi.co/api/v2/pokemon/](http://pokeapi.co/api/v2/pokemon/) pour avoir la liste des Pokémons sur la page d'accueil.
* Utiliser [http://pokeapi.co/api/v2/pokemon/:id](http://pokeapi.co/api/v2/pokemon/:id) pour avoir les informations d'un pokémon.

[Solution](https://github.com/makinacorpus/angular-training/commit/6376c0b3cb97dca531ef2eaf184aa5b015f44f7f)

L'API est lente, il faut faire un spinner (ou un message de chargement).

[Solution](https://github.com/makinacorpus/angular-training/commit/d7b02efba2cac3d5c6d36b4338677aaef9f7ea10)

---

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

---

# Observables

Grâce à l'opérateur `map` de **RxJS**, on peut appliquer une fonction sur un observable
comme si c'était une liste normale et le résultat reste un observable.

C'est ainsi qu'on peut **appliquer des traitements sur le résultat d'un appel HTTP**.

---

<!-- .slide: class="alternate" -->

# Exercice

Créer un service fournissant une méthode `listAll()` et une méthode `get(id)`.

[Solution](https://github.com/makinacorpus/angular-training/commit/9b9462fadb7f03661bf194fce84162b1b6e09809)

---

<!-- .slide: class="alternate" -->

# Exercice

Stocker le résultat de `listAll()` en cache.

[Solution](https://github.com/makinacorpus/angular-training/commit/3c216e2e53f2250d5d604e99fd640aec768fa114)

---

# 7 - Gérer des formulaires

Le FormModule fournit des directives disponibles par défaut sur tous les `<form>` :

- `ngForm`, l'objet form lui-même (avec notamment ses valeurs)
- `ngModel`, le binding avec les champs
- `ngSubmit`, l'action de submit

---

# Exemple

    !xml
    <form #f="ngForm" (ngSubmit)="doSomething(f.value)">
        <input type="text" name="email" ngModel />
        <input type="text" name="username" [ngModel]="currentUser" />
        <button (click)="ngSubmit">Go</button>
    </form>

Et dans le composant :

    !typescript
    doSomething(data) {
        console.log(data.email);
        console.log(data.username);
    }

---

# Exercice

Faire un formulaire dans la page About qui permet de signaler un pokémon manquant.

Note : comme on ne dispose pas d'un endpoint pour envoyer des mails, on va simplement écrire dans le log notre message.

.fx: alternate

---

# Accèder au formulaire depuis le composant

    !typescript
    import { NgForm } from '@angular/forms';
    ...
        @ViewChild('f') form: NgForm;

        ngOnInit() {
          this.form.valueChanges.subscribe(v => console.log(v));
        }

---

# Debounce sur la saisie

    !typescript
    import { NgForm } from '@angular/forms';
    ...
        @ViewChild('f') form: NgForm;

        ngOnInit() {
          this.form.valueChanges
            .debounceTime(1000)
            .subscribe(v => console.log(v));
        }

---

# ViewChild

ViewChild permet d'accèder aux composants contenus dans le composant courant.

C'est un décorateur qui prend en paramètre soit une classe:

    !typescript
    @ViewChild(ContactComponent) contact: ContactComponent;

soit une référence de template:

    !typescript
    @ViewChild('f') form: NgForm;

---

# RxJS et les observables

Faire un observable à partir d'une valeur simple:

    !typescript
    import { Observable } from 'rxjs/Observable';
    import 'rxjs/add/observable/of';

    Observable.of('Bonjour')

---

# RxJS et les observables

Combiner 2 observables:

`forkJoin` renvoie les données dès qu'il a obtenu une réponse de chacun.

`combineLatest` renvoie des données à partir du moment où il a obtenu au moins une réponse de chacun et à chaque nouvelle réponse d'un d'entre eux.


    !typescript
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

.fx: smaller

---

# RxJS et les observables

Chaîner 2 observables: mergeMap (ou concatMap pour des événements répétables)

    !typescript
    return this.http.get(this.baseUrl + id + '/')
      .map(res => {
        this.status.next({ loading: false, error: null });
        return res.json();
      }).mergeMap(res => {
        return Observable.forkJoin(
          Observable.of(res),
          this.http.get(res.abilities[0].ability.url).map(res2 => res2.json()));
      });

---

# RxJS et les subjects

Les subjects sont des observables et des observateurs à la fois, on peut s'y abonner et leur envoyer des données.

    !typescript
    const subject = new Subject<number>();

    subject.subscribe((number) => {
        console.log(number);
    });

    subject.next(1);
    subject.complete();

---

# RxJS et les subjects

BehaviorSubject renvoie la dernière valeur même si elle a été émise avant le subscribe.
Il a forcément une valeur intiale.

---

# RxJS et les subjects

**AsyncSubject** ne garde que sa dernière valeur et l'émet à partir du moment où on l'arrête.

    !typescript
    protected vocabulary: AsyncSubject = new AsyncSubject();

    constructor(private api: HTTPService) {
        this.api.get('/vocabulary/')
            .map((response: Response) => {
                this.vocabulary.next(response.json());
                this.vocabulary.complete();
            });
    }

---

# 8 - Tester une app

Par défaut, le CLI génère un fichier .spec.ts pour chaque composant.

Ce fichier contiendra tous les tests unitaires du composant.

Les tests sont lancés par la commande :

    !console
    ng test



---

# Test d'acceptance

RobotFramework

- les tests sont rédigés en langage naturel,
- ils sont interprétés par Selenium.

---

# Installation

Installer Python 2.7 puis :

    !console
    easy_install pip
    pip install robotframework
    pip install robotframework-selenium2library
    pip install robotframework-debuglibrary

ChromeDriver à ajouter dans le dossier bin de Python :
http://chromedriver.storage.googleapis.com/index.html

---

# 9 - Ajouter des dépendences externes

On ajoute des dépendences avec :

    !console
    npm install nom-du-paquet
    npm install nom-du-paquet@3.0.2
    npm install nom-du-paquet --save

Note: `--save` sert à ajouter une entrée dans `package.json`.

---

# Exercice

Ajouter Material Design

    !console
    npm install angular2-mdl material-design-lite --save


.fx: alternate

---

# 10 - Déployer en prod

Pour créer un build :

    !console
    ng build

Pour un build minifié avec cache keys:

    !console
    ng build --prod

Si le déploiement n'est pas fait à la racine :

    !console
    ng build --base-href=/pokemon

---

# Respecter le routage

Le routage de l'app Angular doit être accepté par le serveur web :

- si l'utilisateur est sur une URL donnée produite par le routage Angular, et qu'il rafraîchit la page, il ne doit pas obtenir un 404,
- idem pour les bookmarks ou les liens partagés par mail ou autre.

Il faut donc un vhost qui renvoie toutes les URLs sur index.html (sauf les ressources JS/CSS/assets).

Exemple Nginx:

    location / {
        try_files   $uri $uri/ /index.html;
    }

---

# Angular Universal

Permet de faire tourner l'app Angular sur un serveur afin de servir la page demandée en HTML pré-généré :

- augmente les performances,
- gère le routage,
- améliore le référencement.

La page servie est ensuite ré-hydratée : le JS s'active et la suite de la navigation se fait en mode client-side.
