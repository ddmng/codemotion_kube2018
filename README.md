Codemotion 2018; KUBEPRIMER, Deploying microservices to Kubernetes
---
Wifi: Aula6 / Codemotion

## Appunti del corso
Quando si superano i limiti designati per il container, il kernel killa il processo.

* **Kubelet** --> gira su ogni nodo ed è quello che lancia `docker run`
* **pod** --> collezione di container che condividono storage e network --> si parlano su localhost. Ogni POD ha un suo virtual IP
il routing del cluster avviene tramite iptables (sta cambiando...)

## da approfondire
* "workload statico" per girare anche in nodo singolo

## Links
* https://www.cncf.io/
* https://github.com/kubernetes/kompose
