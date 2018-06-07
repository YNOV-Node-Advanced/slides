# Authentication et Session management

---

## Basic access authentication

Le client HTTP fournit un username et mot de passe dans la requête.

---

Demander au browser une authentification:

```http
WWW-Authenticate: Basic realm="User Visible Realm"
```

---

Format et convention du header d'authentication:

```http
Authorization: Basic <Base64(username + ':' + password)>
```

---

## 👨‍💻 Exercice

https://classroom.github.com/a/5wj7orZt

Écrire un middleware express pour protéger une route avec un username/mot de passe.

---

## Sessions management

---

### Cookies

Data définies par le serveur ou le code JS, stoquées dans le browser de l'utilisateur.

Le serveur définit un cookie en envoyant le header:

```http
Set-Cookie: <cookie-name>=<cookie-value>
```

Et obtient les cookies via le header:

```http
Cookie: <cookie-name>=<cookie-value>
```

---

Par defaut, les cookies expirent à la fin de la session (fermeture du browser).

Pour des cookies permanent et securisés (non-accessible coté Javascript):

```http
Set-Cookie: id=a3fWa;
  Expires=Wed, 21 Oct 2015 07:28:00 GMT;
  Secure; HttpOnly
```

---

### Avec express:

Nécessite le module `cookie-parser`:

```js
const express = require('express');
const cookieParser = require('cookie-parser');

const app = express()
app.use(cookieParser())
```

---

Le module parse les cookies:

```js
app.use(cookieParser());

app.get('/', function(req, res) {
  console.log('Cookies: ', req.cookies)
});
```

---

Écrire un cookie dans une `Response`:

```js
res.cookie(
  'cookieName',
  'cookieValue',
  {
    expires: new Date(Date.now() + 900000),
    httpOnly: true
  }
);
```

---

### Comment utiliser les cookies pour du session management ?

**Objectif:** Stoquer un identifiant représentant l'utilisateur.

---

### 1: Utiliser un store et un identifiant intermédiaire:

```
const sessionStore = {};

app.post('/login', (req, res, next) => {
  ...
  // ID qui identifie notre utilisateur
  const userID = 'JohnDoe';

  // ID (secret) qui identifie la session
  const sessionID = `${Math.random()}`;
  sessionStore[sessionID] = userID;

  res.cookie(
    'session',
    sessionID,
    {
      expires: new Date(Date.now() + 900000),
      httpOnly: true
    }
  );
  ...
});
```

---

Exemple de store pour session (Doît être assez rapide):

- Redis
- MemCached

---

### 2: Utiliser un cookie signé

L'identifiant de l'utilisateur est stoqué dans le cookie sans store de sessions.

Pourquoi le cookie doit être signé ?

---

### Signer un cookie

Une clé privée est gardée sur notre serveur, et utilisée pour signer les cookies:

```
SIGNATURE = HASH(COOKIE + PRIVATE_KEY)
SIGNED_COOKIE = COOKIE + SIGNATURE
```

---

### Vérifier un cookie

```
{ COOKIE, SIGNATURE } = SIGNED_COOKIE
EXPECTED_SIGNATURE = HASH(COOKIE + PRIVATE_KEY)

IF (EXPECTED_SIGNATURE != SIGNATURE) {
  INVALID
}
```

---

## 👨‍💻 Exercice

https://classroom.github.com/a/k_kd_bxI

Ajouter un systéme de session par cookies signés.
