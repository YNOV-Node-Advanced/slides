# Unit testing

---

## Jest

> Painless/Delightful JavaScript Testing

Librairie de testing gérée par Facebook. Contient l'ensemble des outils:

- Présentation des résultats
- Librairie d'assertion (`expect`)
- Mocking et Spy
- Code coverage

---

## Jest API

En variables globales:

- `describe(name: string, fn: () => void)`
- `it(name: string, fn: () => void | Promise<*>)`
- `expect(value: any): Expect`

---

https://facebook.github.io/jest/docs/en/expect.html

- `expect(value).toBe(expectedValue)`
- `expect(value).not.toBe(expectedValue)`


---

```
package.json
src
  math/
    sum.js
    __tests__
      sum.js
  server
    app.js
    something.js
    __tests__
      app.js
      something.js
```

---

```js
const sum = require('../sum');

describe('sum', () => {
    it('should add 2 positive numbers together and return the result', () => {
        expect(sum(2, 3)).toBe(5);
    });
});
```

---

## Jest Mocking

`src/runSomeStats.js`

```
const getDataFromServer = require('./getDataFromServer');

async function runSomeStats(url) {
   const data = await getDataFromServer(url)
   ...
}

module.exports = runSomeStats;
```

---

`src/__tests__/runSomeStats.js`

```js
jest.mock('../getDataFromServer');

const runSomeStats = require('../runSomeStats');


describe('runSomeStats', () => {
  ...
});
```

---

`src/__mocks__/getDataFromServer.js`

```js
async function getDataFromServer(url) {
  return Promise.resolve('Some fake data');
}

module.exports = getDataFromServer;
```

---

## 👨‍💻 Exercice

https://classroom.github.com/a/QGK8FsSD

Écrire un outil en ligne de commande pour GZIP tous les fichiers d'un dossier.
Écrire des tests avec `jest` pour cet outil.

---

### Express testing: Supertest

Supertest est une librairie facilitant les assertions de requêtes HTTPs.

---

```js
const request = require('supertest');
const express = require('express');

const app = express();

app.get('/user', function(req, res) {
  res.status(200).json({ name: 'john' });
});
```

---

### Avec supertest:

```js
const app = require('./app');

request(app)
  .get('/user')
  .expect('Content-Type', /json/)
  .expect('Content-Length', '15')
  .expect(200)
  .end((err, res) => {
    if (err) throw err;
  });
```

---

### Jest + supertest

```js
it('should return JSON', () => {
    return request(app)
      .get('/user')
      .expect('Content-Type', /json/)
      .expect(200);
});
```

---

### Continuous Integration (CI)

Exemple: Travis, Circle CI, Bitbucket, GitLab CI

---

## 👨‍💻 Exercice

https://classroom.github.com/a/yXR_gZLd

Écrire une API qui gzip un ou plusieurs buffers.
Écrire les tests pour cet API.
