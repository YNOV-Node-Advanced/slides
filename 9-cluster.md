# Cluster

---

Node JS utilise une seule thread d'execution JavaScript.

Mais il est possible d'en utiliser plusieurs avec le module `cluster`.

---

```js
const cluster = require('cluster');

if (cluster.isMaster) {
    console.log(`Master worker ${process.pid} is running`);
    cluster.fork();
} else {
    console.log(`Child worker ${process.pid} started`);
    process.exit(0);
}
```

---

Avantage: profiter au maximum du multi-core.

```js
const os = require('os');
const numCPUs = os.cpus().length;
```

et démarrer un worker pour chaque thread:

```js
// Fork workers.
for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
}

cluster.on('exit', (maworker, code, signal) => {
    console.log(`Worker ${worker.process.pid} died`);
});
```

---

Gros avantage: Les workers partagent les connections TCP.
