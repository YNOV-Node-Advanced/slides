# Websockets

---

## Objectifs du projet

Développer une application de dessin collaborative

- Utiliser une connexion websocket pour le temps réel
- Concevoir une application qui peut scaler pour des millions d'utilisateurs

---

## Websocket, c'est quoi ?

Connexion duplexée entre un browser et un serveur. Le serveur peut envoyer des datas au client sans répondre à une requête du browser.

---

## Oubliez Socket-io !!!

Socket-io est une librairie qui utilise du websocket et fallback sur du HTTP polling.

---

## Websocket: côté client

```js
const socket = new WebSocket(
  'ws://localhost:5000/',
  'protocolOne'
);

// Callback lorsque la connexion est etablie
socket.addEventListener('open', (event) => {
  
});

// Callback lorsqu'on recoit un message
socket.addEventListener('message', (event) => {
  console.log('Received', event.data);
});

// Envoyer un message (string ou buffer)
socket.send('Hello world');

// Clore la connexion
socket.close();
```

---

## Websocket: côté serveur

```
$ npm install ws
```

```js
const http = require('http');
const WebSocket = require('ws');

const server = http.createServer();
const wsServer = new WebSocket.Server({ server });

wsServer.on('connection', (ws) => {
  ws.on('message', (buffer) => {
    console.log('Received', buffer.toString());
  });
  
  ws.send(message);
});
```

---

## 👨‍💻 Exercice (45min):

https://classroom.github.com/a/axUcvLPD

Écrire un serveur de broadcast websocket: redistribue les messages reçus d'un client à tous les autres clients.

Module à utiliser: `ws`, `http`, `express`

---

## 👨‍💻 Exercice (1h30):

Modifier le serveur de broadcast pour seulement broadcast vers les clients qui partagent la même channel.