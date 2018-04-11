# Intro

This is your final exam, don't worry you wont be graded but we hope you will have some fun executing and solving the problem below:

## Scenario

We have a simple multi tier app to deploy on kubernetes. The multi tier app is composed by a frontend service and a backend service.

To obtain the docker images you can pull:

```
docker pull sighup/kubeprimer_frontend:0.2
docker pull sighup/kubeprimer_backend:0.1
```

The two services are pretty trivial but you will need to apply all the knowledge learned today to complete the exercise.

## Suggested approach

I would start with deploying the frontend service, check if it works, and, if we think we are satisfied with the result we should continue with the backend app (you'll need the backend).

Remember you can use  `kubectl logs`, `kubectl exec`, and `kubectl port-forward` to help you debug if something goes wrong.

A ConfigMap might be needed.
