# Async, Promises, Streams, Networking and Events

---

## Async

JavaScript est single-threaded: une seule execution de code peut avoir lieu au même temps.

Code bloquant:

```
const contents = fs.readFileSync('/etc/hosts');
console.log(contents);
console.log('Doing something else');
```

Code async:

```
fs.readFile('/etc/hosts', (err, contents) => {
  console.log(contents);
});
console.log('Doing something else');
```

---

> “JavaScript has certain characteristics that make it very different than other dynamic languages, namely that it has no concept of threads. Its model of concurrency is completely based around events.”
> **Ryan Dahl**

---

## Promises

Une promesse est un objet (`Promise`) qui représente la complétion ou l'échec d'une opération asynchrone.

- Callback hell
- Flow plus propre de gestion des erreurs

---

```js
function wait(delay) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve();
    })
  }, delay);
}
```

```js
wait(1000)
.then(() => {
  ...
})
```

---

### `util.promisify`

Convertir des fonctions qui acceptent un callback en fonction qui retourne une promise:

```js
const util = require('util');

const stat = util.promisify(fs.stat);
stat('.').then((stats) => {
  // Do something with `stats`
}).catch((error) => {
  // Handle the error.
});
```

---

## 👨‍💻 Exercice (15min):

https://classroom.github.com/a/u7QZ2DuR

Adapter le code en utilisant des `Promise`.

---

## Syntaxe `async/await`

---

Node 10 and ES6+ ont introduit la syntaxe `async/await` pour gérer les flows de promises.

```js
const content = await readFile('something.txt');
```

est équivalent à:

```js
readFile('something.txt')
.then((content) => {

});
```

---


## 👨‍💻 Exercice (15min):

https://classroom.github.com/a/3_UcNROc

Adapter le code en utilisant `async/await`.


---

## Streams

---

⁉️ Quel est le problème avec ce code snippet ?

```js
const fs = require('fs');
const server = require('http').createServer();

server.on('request', (req, res) => {
  fs.readFile('./big.file', (err, data) => {
    if (err) throw err;

    res.end(data);
  });
});

server.listen(8000);
```

---

Structure de donnée basée sur les events. Comme une collection de donnée qui n'est pas entiérement disponible en mémoire.

Exemples de modules avec une Stream API:

- `net`
- `child_process`
- `http`
- `zlib`
- `fs`

---

Le concept de stream est basé sur les concepts Unix:

```
$ grep -R exports * | wc -l
```

---

Equivalent en Node.js:

```js
const childProcess = require('child_process');

const grep = childProcess.exec('grep -R exports *');
const wc = childProcess.exec('wc -l');

grep
  .pipe(wc)
  .pipe(process.stdout);
```

---

### Différents types de Streams:

- **Readable Stream**: source de donnée qui peut-être lue (un fichier avec `fs.createReadStream`)
- **Writable Stream**: destination pour des donnée (un fichier avec `fs.createWriteStream`).
- **Duplex Stream**: Un stream qui est readable et writable.
- **Transform Stream**: Un duplex stream avec une sortie qui correspond à une transformation de l'entrée

---

### `stream.pipe(destination)`


```js
const fs = require('fs');
const zlib = require('zlib');

fs.createReadStream('./input.txt')
  .pipe(
     zlib.createGzip()
   )
  .pipe(
     fs.createWriteStream('input.txt.gz')
  );
```

---

✅ Implémentation avec un faible coût mémoire:

```js
const fs = require('fs');
const server = require('http').createServer();

server.on('request', (req, res) => {
  const src = fs.createReadStream('./big.file');
  src.pipe(res);
});

server.listen(8000);
```

---

## Gérer les erreurs

```js
const net = require('net');

const client = new net.Socket();

client.on('data', (buffer) => {
  console.log(buffer.toString())
});

client.connect(1337, '127.0.0.1');
```

---

Si la connection n'est pas stable, le programme crash !

```
events.js:182
      throw er; // Unhandled 'error' event
```

---

Il faut toujours gérer les erreurs asynchrones potentielles:

```js
client.on('error', (error) => {
  console.error(error.message);
});
```

Et `.pipe` ne forward pas les erreurs:

```js
a
  .on('error', handleError)
  .pipe(b.on('error', handleError))
  .pipe(c.on('error', handleError))
```

---

## 👨‍💻 Exercice (15min):

https://classroom.github.com/a/zCvxX1H-

Écrire une fonction `compress` qui prend en entrée deux noms de fichiers `input` et  `output`, et ecrit le résultat gzippé de `input` dans `output`. La fonction doit retourner le nombre de bytes qui ont été ecrits.

```js
async function compress(
  input: string,
  output: string
): Promise<number>
```

---

## 👨‍💻 Exercice (45min):

https://classroom.github.com/a/JfZ2tP_g

Écrire un chat TCP:
- Un programme Node `server.js` qui broadcast les messages d'un client vers les autres.
- Un programme Node `client.js` qui affiche les messages reçus et envoie vers le serveur les messages entrés par l'utilisateur.

Module à utiliser: `net`, `readline`
