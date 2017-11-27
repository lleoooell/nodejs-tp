# Node.js TP

## 1. Écrire votre première application Node.js

Dans cette première partie, nous allons créer une application très simple qui affiche `Hello world` en console.

- Se placer dans un terminal, dans un dossier créé pour ce TP, appelé `node-tp`.
- Créer un dossier qu’on appelle: `hello-world`
- Créer un fichier `index.js` dans ce dossier
- Écrire dans ce fichier
```js
console.log('Hello world');
```
- Dans le terminal (dans le dossier `hello-world`), lancer la commande:

```
node index.js
```

Vous devriez voir le résultat suivant:

```
hello world

```

Votre première application est terminée ! Bon elle ne fait pas grand chose alors on va l'améliorer un peu.

## 2. Une application calculatrice

Sur chacune des étapes, je vous conseille de **taper** le code et de le comprendre, et non de le copier/coller...

- Créer un nouveau dossier `calc` dans le dossier `node-tp`
- Créer un nouveau fichier `index.js` dans ce dossier:

```js
// index.js

var sum = function (a, b) {
  return a + b;
}

var result = sum(1, 3);
console.log('The result is ' + result);
```

Ici nous avons créé une application qui calcule la somme `1 + 3`.

On lance l'application (depuis le dossier `calc`):

```
node index.js
```

Et vous devriez voir le résultat suivant:

```
The result is 4
```

### 2.1 Création d'un module

Pour bien organiser son code, nous allons séparer certaines parties du code dans ce qu'on appelle des **Modules**. Un module va permettre de créer une brique réutilisable qui va fournir un certain nombre de fonctions ou de données.

Pour notre exemple, nous allons créer un module calculatrice.

En Node.js chaque fichier est un module, nous allons donc:

- Créer un nouveau fichier `calc.js`:

```js
// calc.js

var sum = function (a, b) {
  return a + b;
};

module.exports = {
  sum: sum
};
```

*... C'est quoi ce `module.exports` et ce JSON ? tu m'as perdu.*

Dans chacun des fichiers, et donc des modules, certaines variables et fonctions particulières sont disponibles.

`module` est une des variables disponibles dans chacun des modules. Cette variable possède une propriété `exports` qui contient un objet particulier. Cet objet est l'objet le plus important d'un module.

*La raison pour laquelle ces variables particulières sont disponibles dans les modules dépasse le scope de ce cours, mais pour les curieux, nous pourrons en discuter à la fin du cours.*

L'objet qui se trouve dans `module.exports` est l'objet que l'on pourra récupérer quand on voudra utiliser le module. Rien ne vaut un exemple.

- Créer un nouveau fichier `index.js`:

```js
// index.js

// retrieve the calc module
var calc = require('./calc.js');

var result = calc.sum(1, 3);
console.log(result);
```

Et on lance notre application:
```
node index.js
```

Et vous devriez voir le resultat:
```
4
```

Les modules sont essentiels quand on utilise Node.js. Il faut vraiment comprendre correctement ce que l'on vient de faire.

- Nous avons créé un nouveau fichier `calc.js`, et donc un nouveau module, car chaque fichier est un module.

Dans le fichier `calc.js`:

- Nous avons créé une fonction `sum` disponible dans le module.
- Nous avons créé un nouvel objet que nous avons mis dans la variable `module.exports`, et cet objet sera "récupérable" dans un autre fichier.
- Nous avons ajouté une propriété `sum` qui a pour valeur la fonction `sum`.

Dans le fichier `index.js`:

- Nous avons récupéré l'objet qui est dans `module.exports` du fichier grâce à la fonction `require`. (`require` est une fonction disponible dans tous les modules, tout comme la variable `module`) Nous avons mis cet objet dans la variable calc.
- En faisant `calc.sum()`, nous accédons à la fonction `sum` définie dans le module.

*Ça fait beaucoup de chose juste pour mettre un fichier ...*

Oui. Mais c'est nécessaire et nous reviendrons sur ce fonctionnement qui est assez complexe.

## 3. Création d'un serveur

*Bon moi par contre je suis venu pour faire du web alors ta calculatrice ...*

Dans le cadre de ce cours, nous allons essentiellement nous concentrer sur le développement back-end. Nous allons créer une API calculatrice. C'est à dire qu'une autre application pourra faire appel à notre API pour faire ses calculs.

Nous allons utiliser Node Package Manager (npm) pour télécharger un framework nommé `express`, qui va nous servir à créer cette API plus simplement. Nous allons reprendre le code de l'exercice précédent et nous allons l'améliorer.

Pour commencer nous allons "initialiser" notre projet. dans le répertoire `calc`:

```
npm init
```

Cette commande va vous poser une liste de questions. Vous pouvez prendre les valeurs par défaut de ces questions en appuyant directement sur `entrée`. La commande génère un fichier nommé `package.json`. Ce fichier décrit l'application, tout comme le `composer.json` pour les applications PHP. Il liste les différentes informations relatives au projet, et plus particulièrement, ses dépendances.

Nous allons maintenant ajouter une nouvelle dépendance à notre projet: le framework `express`.

```
npm install express --save
```

`npm install` est la commande pour installer un package dans le projet. Npm place ce package dans le dossier `node_modules`. L'option `--save` permet d'ajouter cette dépendance dans le fichier `package.json`.

Nous avons donc installé express et nous l'avons également défini comme dépendance du projet. Nous allons maintenant l'utiliser.

Nous avons vu précédemment comment utiliser un module que nous avons écrit avec le module `calc`. Il est également possible d'utiliser des modules écrits par d'autres développeurs. Express se présente sous la forme d'un module que l'on peut utiliser. Pour le récupérer, nous allons utiliser la fonction `require`.

```js
// index.js

var express = require('express');
```

Le framework `express` va nous permettre de créer facilement des routes et de répondre aux requêtes des utilisateurs (car oui, c'est quand même le but d'un serveur !). Nous allons définir une première route `/` qui va répondre le fameux `Hello world!`.

Ce code ne s'invente pas, il faut bien entendu regarder dans la documentation du module.

```js
// index.js

var express = require('express');
var app = express();

app.get('/', function (req, res) {
  res.send('Hello World!');
});
```

Ici, nous avons créé une nouvelle application `app` grâce au module `express`. Nous avons ensuite créé une route pour cette application (`/`) avec la méthode `GET` et nous lui avons associé une callback, qui prend en paramètre la requête `req` et la réponse `res`.

Nous avons enfin ajouté `Hello world` à la réponse.

*Ça y est, je peux appeler mon Hello world ???!*

Pas si vite. Pour l'instant nous avons créé une application express mais on ne "l'expose" pas encore.

Nous allons utiliser la fonction `listen` qui permet comme son nom l'indique d'écouter un port.

```js
// index.js

var express = require('express');
var app = express();

app.get('/', function (req, res) {
  res.send('Hello World!');
});

app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```

Et voilà! Vous pouvez maintenant lancer votre serveur avec la commande:

```
node index.js
```

Vous devriez maintenant pouvoir accéder à votre `Hello world` à l'url [suivante](http://localhost:3000).

### Résumé

- Créer une application en utilisant le framework `express`.
- Créer une route `/` à laquelle on répond `Hello world!`.
- Exposer l'application sur le port 3000.

## 4. Postman

Nous allons faire une courte pause pour apprendre à utiliser un outil important. Pour créer une API, il est pratique de pouvoir la tester, et le navigateur n'est pas forcément l'outil le plus adapté pour tester une API. Nous allons utiliser `Postman`.

Pour notre première utilisation, nous allons faire appel à notre route `/`.

Lancer Postman.

Vous devriez voir un bouton GET et une barre de saisie à côté. Le bouton GET permet de selectionner la méthode HTTP que l'on souhaite utiliser avec Postman. La barre de saisie permet de choisir l'url à laquelle on envoie cette requête.

Nous allons rester sur une requête GET et nous allons saisir l'url suivante: `http://localhost:3000`.

Cliquez ensuite sur le bouton SEND pour envoyer la requête. Vous devriez maintenant recevoir le résultat `Hello Wolrd!`.

À partir de maintenant, nous utiliserons Postman pour tester notre API.

## 5. API Calculatrice

Maintenant que nous savons comment créer une API, et comment créer un module calculatrice, il est temps de créer une API calculatrice.

Bien entendu, il n'est pas nécessaire de créer un module à part dans un cas aussi simple, mais ceci est un exemple pour comprendre comment il faut organiser son code, car dans le cas d'une application plus complexe, votre serveur fera bien plus qu'une simple addition pour répondre à son utilisateur.

### 5.1 les paramètres de route (Route parameters)

Nous allons reprendre notre application `express` et y ajouter une route pour faire une addition. Pour faire une addition, il faut que l'utilisateur puisse choisir quels nombres il veut additionner.

*Pour l'instant nous allons oublier notre module et faire le calcul directement dans la fonction callback de la route.*

Essayer de faire cet exercice sans regarder la correction.

Ajouter une nouvelle route avec 2 paramètres de route, le premier et le deuxième input. Pour ajouter un paramètre de route il faut ajouter `:` devant le paramètre. On peut ensuite recupérer le paramètre via la `req.params.paramName`.

Par exemple:
```js
app.get('/hello/:name', function (req, res) {
  res.send('Hello ' + req.params.name + '!');
});
```

répondra à `/hello/maxime` par `Hello maxime!`.

Attention: les paramètres passés sont des `string`. Veillez donc à ne pas renvoyer `2 + 3 = 23` :)

Prennez le temps de tester votre nouvelle route avec Postman et vérifier que les résultats sont cohérents.

Correction:
 ```js
 var express = require('express');
 var app = express();

 app.get('/', function (req, res) {
   res.send('Hello World!');
 });

 app.get('/add/:input1/:input2', function (req, res) {
   var input1 = parseInt(req.params.input1);
   var input2 = parseInt(req.params.input2);

   var result = input1 + input2;

   res.send(result.toString());
 });

 app.listen(3000, function () {
   console.log('Example app listening on port 3000!');
 });
 ```

- Nous avons donc 2 paramètres de route (input1, input2) et nous les récupérons via `req.params` (paramètres de la requête).
- Nous utilisons `parseInt()` pour transformer les paramètres en entier pour le calcul.

### 5.2 Ajout du module Calculatrice

Récupérer le fichier calc.js vu précedemment et ajouter le dans ce même dossier.

Nous allons maintenant utiliser notre module calculatrice pour le calcul.

À nouveau, essayer de faire cette exercice sans regarder la correction. Le but est de faire exactement la même chose sauf que le calcul doit être effectué par le module et sa méthode `sum` au lieu de faire directement le calcul dans la callback.

```js
var express = require('express');
var app = express();

var calc = require('./calc.js');

app.get('/', function (req, res) {
  res.send('Hello World!');
});

app.get('/add/:input1/:input2', function (req, res) {
  var input1 = parseInt(req.params.input1);
  var input2 = parseInt(req.params.input2);

  var result = calc.sum(input1, input2);

  res.send(result.toString());
});

app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```

## 6. Les middlewares d'Express

Express est un framework. Il fournit plusieurs petites briques appelées middlewares permettant de remplir différentes tâches.

Le but d'un middleware est d'intercepter la requête avant qu'elle soit traitée pour différentes raisons possibles:
- Exécuter du code
- Changer la requête et/ou la réponse
- Interrompre le cycle de vie requête/réponse
- Appeler le middleware suivant

### 6.1 Création d'un middleware

Nous allons créer notre premier middleware. Le but du middleware est de logger le timestamp des requêtes des utilisateurs de l'API. Avant les déclarations des routes, ajouter le code suivant:

```js
app.use(function (req, res, next) {
  console.log('Time:', Date.now());
});
```

Essayez maintenant d'appeler une route avec Postman et regarder votre console. Les timestamps des requêtes sont affichés.

*Mais je ne comprends pas, je n'ai plus de résultat ...*

Le middleware a intercepté la requête, il peut ensuite faire ce qu'il veut avec la requête. Dans notre cas, nous voulons rendre la main à Express, nous allons donc lui dire de continuer en appelant la fonction next.

```js
app.use(function (req, res, next) {
  console.log('Time:', Date.now());
  next();
});
```

Voilà, on a récupéré notre résultat.

### 6.2 Utilisation du middleware express-session

Express fournit une liste de middlewares pour différentes fonctionnalités. Nous allons utiliser le middleware nous permettant de gérer les sessions utilisateurs.

Partons du code suivant:

```js
// index.js

var express = require('express');
var app = express();

app.get('/', function (req, res) {
  res.send('Hello ! you already visited this route ' + 0 + ' time(s)');
});

app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```

Le but est de déterminer grâce à la session utilisateur, combien de fois l'utilisateur a visité la route `/`.

Nous allons installer le middleware `express-session`

```
npm install express-session --save
```

Nous allons ensuite utiliser les sessions utilisateurs pour compter le nombre de visites sur `/`.

```js
// index.js

var express = require('express');
var session = require('express-session');
var app = express();

app.use(session({ secret: 'mysecrettoken'}));

app.get('/', function (req, res) {
  if (!req.session.views) {
    req.session.views = 0;
  }

  req.session.views++;
  res.send('Hello ! you already visited this route ' + req.session.views + ' time(s)');
});

app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```


Le middleware `express-session` nous donne accès à `req.session`, un objet propre à chaque session utilisateur. Une session utilisateur est determinée grâce à un cookie de session contenant un identifiant de session. En appelant une route d'express qui utilise le middle `express-session`, vous recevez un cookie. Vous pouvez regarder ce cookie dans Postman en appelant la route `/`.

Si vous appelez la route `/` plusieurs fois, le compteur s'incrémente. Si vous supprimez votre cookie, le compteur se reinitialise, car vous avez perdu votre identifiant de session. Nous avons donc bien un compteur par session utilisateur.

### 6.3 Gestion des formulaires

Nous allons ici apprendre à récupérer des informations depuis un formulaire. Nous allons créer une route `/form` pour regarder ce que l'on reçoit à la soumission d'un formulaire en POST sur cette route.

Nous avons vu comment utiliser un middleware sur toutes les routes d'express, voici maintenant comment utiliser un middleware sur une route en particulier.

```js
// index.js

var express = require('express');
var bodyParser = require('body-parser');
var urlEncodedParser = bodyParser.urlencoded({ extended: false });

var app = express();

app.get('/', function (req, res) {
  res.send('Hello world !');
});

app.post('/form', urlEncodedParser, function (req, res) {
  console.log(req.body);
  res.send('form');
});


app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```

Le middleware va nous fournir l'objet `req.body` contenant les différents champs du formulaire urlencoded. Nous allons maintenant faire une requête POST depuis Postman:

- Changer GET en POST
- Dans l'onglet Body, choisir `x-www-form-urlencoded`
- Entrer une clé et une valeur (par exemple `testkey`, `testvalue`)
- Soumettre la requête

On peut voir dans la console:

```js
{ testkey: 'testvalue' }
```

Dans l'objet `req.body`, on retrouve bien notre `testkey`. C'est de cette façon que l'on récupère les champs depuis un formulaire.
