Codemotion 2018; KUBEPRIMER, Deploying microservices to Kubernetes
---
* Wifi: Aula6 / Codemotion
* Società: SIGHUP

## Appunti del corso
Quando si superano i limiti designati per il container, il kernel killa il processo.

* **Kubelet** --> gira su ogni nodo ed è quello che lancia `docker run`
* **pod** --> collezione di container che condividono storage e network --> si parlano su localhost - coschedulati e colocati. Ogni POD ha un suo virtual IP
il routing del cluster avviene tramite iptables (sta cambiando...)

La gestione del db è un problema, cmnq non condividono tutti il far girare in container. Il tizio lo fa girare sempre a parte su ferro.

* **ReplicaSet** --> Si assicura che i pod girino nel numero di repliche desiderato, si possono associare label e tag
* **Deployment** --> superset di ReplicaSet --> rolling updates and rollbacks
* Si possono customizzare le policy di gestione dei rolling update

## Esempio di deployment 1

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

## Slides
* Gestione configurazioni e segreti --> configMaps e Secret. la diff è che i secret sono encoded base64. si tratta in entrambi i casi di etcd - non ideale, consigliato Vault, gestito in modo applicativo, non nativo di k8s, che sta diventando lo standard di fatto per **config, secret e certificati**.
* non gestire config con env o negli yaml

Secret e volumi possono poi essere mappati su variabili di ambiente o su files (cioè volumi).

Il VirtuaIP del service non cambia mai una volta creato ma è sempre meglio accedere ai servizi tramite il DNS interno di k8s. Di default non sono raggiungibili dall'esterno. Per pubblicare un pod NEL CLUSTER (non all'esterno), bisogna aprire la porta sul pod e sul servizio. A quel punto il pod è visibile nel cluster tramite il servizio. Per esporre all'esterno c'è un service di una tipologia diversa apposito.
* **ClusterIP** - default, solo interno al cluster
* **NodePort** - espone all'esterno una porta qsiasi sopra la 30000 (o specificata) e la pubblica SU TUTTI I nodi del cluster (magari può andare bene per una API di backend)
* **Ingress** - si usa al posto del LoadBalancer nelle infrastrutture private --> pod con nginx o haproxy (o quello che implementiamo noi) --> non è proprio un service, si crea un ClusterIP o NodePort e sopra di esso di fa l'ingress. --> `kind: service` + `kind: ingress`. Di default k8s non viene fornito con un ingress ma devi scaricartelo appositamente tra quelli che mette a disposizione.
* **LoadBalancer** - va bene solo sui cloud provider, quindi non va bene sull'infrastruttura privata

### Esercizio 2
* 2x backend
* 3x frontend
* 1x mongodb
* 1 Load balancer --> ce lo da k8s

NB annotation serve all'umano mentre le label sono gestite dai servizi

`resources: --> requests` sono i requisiti richiesti ai nodi per schedulare il servizio
`resources: --> limits` sono i limiti oltre i quali viene killato (ha una politica di bursting inclusa per gestire i picchi)

k8s può gestire lo storage come un servizio qualsiasi, lo fai vedere a k8s e gli dici quanto dedicarlo e a chi. NFS, glusterFS, ceph sono gestiti nativi da k8s.

Deployamo un servizio per BE, uno per FE e uno per Mongo.

Il type di default dei servizi è ClusterIP (quindi solo interna al cluster)

L'ingress va in tandem con un service

Non è l'ingress a mappare la porta da pubblicare ma il servizio, quindi per far vedere un servizio all'esterno bisogna dargli una porta NodePort e l'Ingress

## Slides
**DaemonSet**
* pod che gira con una replica per ogni nodo del cluster, es:
  - agent per le metriche dell'host
  - logging del nodo
**Job e Cronjob**
* Job: pod che **deve uscire bene**. Se non esce con `0` k8s se ne accorge e lo killa e riavvia
* Cronjob: job a tempo

**La parte critica è etcd perché perdi lo stato del cluster.**

Fail del master di k8s:
* `etcd`
* scheduler
* server

L'infrastruttura funziona ma non puoi applicare dei cambiamenti.

Standard per partire: `kubeadm`. Facile ma l'aggiornamento senza downtime del cluster ancora non funziona. "Adesso si può iniziare ad usare" e "tutti stanno convergendo su kubeadm".
kubespray sta migrando verso kubeadm. kops fa "terraforming" dei nodi.

`kubicorn`: non legato alla community k8s ma funziona.

`conjure-up`: vendor per ubuntu, rhel e fedora hanno le loro.

--> Port forwarding

---


## Da approfondire
* "workload statico" per girare anche in nodo singolo
* "stateful set" raccomandato per far girare i database
* "operator" gestione dei problemi di alta affidabilità, es. MySQL-operator esiste
* "vault" per gestire config, secret e cert
* "https://www.cncf.io/"
* elenco degli ingress messi a disposizione su github da k8s (https://github.com/kubernetes/ingress-nginx/tree/master)
* approfondire glusterFS e ceph
* flannel è un network flat
* calico supporta networking (ha pgp)
* weave forse è una via di mezzo?


## Links
* https://www.cncf.io/
* https://github.com/kubernetes/kompose
