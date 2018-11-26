# Promise

## 1. À quoi sert une Promise ?

Les promises ont été créées pour gérer du traitement asynchrone. Les callbacks existaient déjà pour faire ce travail, mais les callbacks posent un problème reccurent. Lorsque l'on utilise des callbacks et que l'on doit chainer les traitements asynchrones, on est obligé d'imbriquer les callbacks, ce qui rend le code peu comprehensible et difficile à maintenir:

```js
asyncFunction1('param', function(err1, result1) {
  if (err1) {
    return handleError(err1)
  }
  // do something
  asyncFunction2(result1, function (err2, result2)) {
    if (err1) {
      return handleError(err1)
    }
    // do something again
    asyncFunction3(result2, function (err3, result3)) {
      if (err3) {
        return handleError(err3)
      }
      // do something again
      asyncFunction4(result3, function (err4, result4)) {
        if (err4) {
          return handleError(err4)
        }
        // do something again
      }
    }
  }
})
```

Si ces fonctions utilisaient des promises plutôt que des callbacks, leur utilisation ressembleraient plus à:

```js
asyncFunction('param')
  .then(asyncFunction2)
  .then(asyncFunction3)
  .then(asyncFunction4)
  .catch(handleError)
```

L'utilisation des promises permet d'avoir un code plus simple et plus facile à maintenir. Les promises sont aussi souvent plus compliquées à comprendre que les callbacks, mais c'est un outil que tout développeur javascript doit connaître.

## 2. Les mains dans le cambouis

Commençons par un exercice. Le but est de comprendre comment une Promise s'utilise. C'est la partie la plus simple.

Dans un fichier `text.txt`, mettez un texte quelconque, puis créez un fichier `index.js`.

1. Grâce à la documentation LINK, récupérez le contenu du fichier `text.txt` et affichez le dans la console en utilisant la callback. 

Une promise est un objet. Je répète cette partie parce que c'est très important. Une promise est un **objet**. Cette objet représente l'état de notre traitement asynchrone (en cours, terminé, en erreur ...). On peut ensuite utiliser `then` pour pour demander à déclancher une fonction quand le traitement sera terminé. On peut utiliser des fonctions déclarés ou des fonctions anonymes.

```js
// 1. with declared function

function handleResult(apiResult) {
    // do something with the API result
}

apiCall('param').then(handleResult)

// 2. with anonymous function

apiCall('param').then(apiResult => {
  // do something with the API result
})
```

La fonction util.promisify() LINK permet de transformer une fonction qui utilise une callback en une fonction qui retourne une promise de la façon suivante:

```js
const util = require('util')

const functionWithPromise = util.promisify(functionWithCallBack)

functionWithPromise('param').then(result => {
  // do something
})
```

2. Grâce à la fonction util.promisify() LINK, transformez la fonction `fs.readFile` en une autre fonction qui utilise une promise au lieu d'une callback. Récupérez ensuite le contenu du fichier `text.txt` avec cette nouvelle fonction.

## 3. La gestion des erreurs

La fonction `then` des promises acceptent en fait deux fonctions en paramètres. La première sert à gérer le traitement s'il réussi, la deuxième sert à gérer le traitement s'il a échoué. 

```js
function handleSuccess(result) {
  // do something
}

function handlerError(error) {
  // handle error
}

myPromise.then(handleSuccess, handleError)
```

Ajoutez une fonction pour gérer l'erreur de votre lecture de fichier. Déclanchez là en lui donnant un nom de fichier qui n'existe pas.

TODO: .catch(handlerrror) et le lien avec .then(null, handleerror)

## 4. Le chaînage

L'intêret principal des promises vient du chaînage. Pour rappel, une promise est un **objet**. Cet objet représente l'état du traitement de la promise. La fonction `then` permet d'effectuer un traitement après que le traitement de la promise soit terminé. 

La fonction `then` ne fait pas que ça. **Elle renvoie elle aussi une promise**, différente de la première. Elle représente l'état du traitement de la fonction qui est passé en paramètre à `then`.

```js
// call is a promise that represent the api call processing state
const call = apiCall('param')

// write is a promise that represent the file writing processing state 
const write = call.then(writeResultSomewhere)

// we can use this promise with the then and catch functions
write.then(result => {
  console.log('file is written with the api call result')
}).catch(error => {
  console.log('something went wrong')
})
```

En utilisant le chaînage, récupérez le contenu du fichier `text.txt`, retourner le contenu (contenu du fichier => reihcif ud unetnoc) et écrivez ce resultat dans un fichier `reverse.txt`. N'oubliez pas de conserver la gestion des erreurs.




