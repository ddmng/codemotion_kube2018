build:
	docker pull mongo:3.7.3
	docker tag mongo:3.7.3 sighup/kubeprimer-mongo

run:
	docker rm -f kubeprimer-mongo || true
	docker run -d --name kubeprimer-mongo --network=kubeprimer -p 27017:27017 sighup/kubeprimer-mongo
