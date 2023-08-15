# Hello ck8s!

This is a fork of https://hub.docker.com/r/paulbouwer/hello-kubernetes/

This container image can be deployed on a ck8s cluster. It runs a web app, that displays the following:

- "A default 'Hello world!' message, or a custom message can be configured via secrets.yaml."
- namespace, pod, and node details
- container image details

## Quick start

## Build and push image

### Building a container image

You can build the `hello-ck8s` container image as follows:

```bash
export REGISTRY=harbor.$DOMAIN
export REPOSITORY=<demo> #change this to your own repository, remember to match it in github actions so it updates the manifest correctly
make build-image-linux
make push-image
```

## Deploy the app using an ArgoCD application

Adjust the values in the manifest as needed and run,

```bash
kubectl apply -f deploy/argocd/application.yaml
```

## Deploy the app using ArgoCD's applicationset and pull-request generator

Pull-request generator continuously monitors your git repo for pull requests with a label specified in the applicationset. As a work-around to github's ratelimiting, create a secret

```bash
apiVersion: v1
data:
  token: <base64 encoded github token with access to your repo>
kind: Secret
metadata:
  name: github-token
  namespace: argocd-system
type: Opaque
```

```bash
kubectl apply -f deploy/argocd/applicationset.yaml
```

## Setup docker credentials in the github repository for github actions

```
DOCKERHUB_SECRET=<create-your-own-in-harbor>
DOCKERHUB_USERNAME=<create-your-own-in-harbor>
```