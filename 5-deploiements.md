# Déploiements sur Heroku

---

Heroku est un Cloud PaaS (Platform as a Service).

---

1. **Baremetal**: (OVH)
2. **IaaS** (Infrastructure as a Service): Allocation dynamique de serveurs virtuels (Amazon EC2, Google Cloud Compute)
3. **PaaS** (Platform as a Service):  Allocation de resources et lancement/gestion de l'application (Heroku, Google App Engine)
4. **Serverless / FaaS** (Firebase, Amazon Lambda)

---


### Dynos:

Process tournant dans son propre environement virtuel.

- **web**: exposés au load balancer (lancé avec une env `PORT`)
- **worker** / **clock**: non exposés au load balancer

---

### Procfile

```
web: node ./server/index.js
worker: node ./server/worker.js
```

---

### Déploiement

```
$ git add .
$ git commit -m "Procfile"
$ git push heroku master
```