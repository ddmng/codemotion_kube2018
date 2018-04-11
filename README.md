Codemotion 2018; KUBEPRIMER, Deploying microservices to Kubernetes
---
Wifi: Aula6 / Codemotion

## Appunti del corso
Quando si superano i limiti designati per il container, il kernel killa il processo.

* **Kubelet** --> gira su ogni nodo ed è quello che lancia `docker run`
* **pod** --> collezione di container che condividono storage e network --> si parlano su localhost - coschedulati e colocati. Ogni POD ha un suo virtual IP
il routing del cluster avviene tramite iptables (sta cambiando...)

La gestione del db è un problema, cmnq non condividono tutti il far girare in container. Il tizio lo fa girare sempre a parte su ferro.

* **ReplicaSet** --> Si assicura che i pod girino nel numero di repliche desiderato, si possono associare label e tag
* **Deployment** --> superset di ReplicaSet --> rolling updates and rollbacks
* Si possono customizzare le policy di gestione dei rolling update

### Esempio di deployment

```yaml
apiVersion: apps/v1beta1 # --> API di kubernetes (semantic versioning)
kind: Deployment # --> tipo di definizione che voglio fare
metadata:
  name: nginx-deployment # nome del deployment
spec: # --> flag relativi al replication controller
  replicas: 3 # Desired state ensured by rs
  template:
    metadata:
      labels: # Labels (simple key-values) associated with the pod
        app: nginx # --> questa è una label
    spec:
      containers: # The actual containers, a simple nginx exposing port 80
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

## da approfondire
* "workload statico" per girare anche in nodo singolo
* "stateful set" raccomandato per far girare i database
* "operator" gestione dei problemi di alta affidabilità, es. MySQL-operator esiste

## Links
* https://www.cncf.io/
* https://github.com/kubernetes/kompose
