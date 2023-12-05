# Hello World Helm Chart

This Helm chart deploys a simple Hello World Pod with an init container for building the nginx image on-the-fly and a Service to expose it.

## What this chart does

1) Builds image using InitContainer

- add git package on the Podman container
- run `podman build`` to build image
- run `podman save` to save image tar file to shared folder. Folder shared between node and pod
- run `nsenter` command to import saved image (`ctr image import`) from shared folder to local Nodes image store

2) hello-world container will pull local saved image and use it to run `hello-world` nginx website page on the port 80

3) service `hello-world` will expose contaner running on the pod `hello-world` and port 80

## Prerequisites

- Kubernetes cluster
- Helm installed ([Install Helm](https://helm.sh/docs/intro/install/))

## Installing the Chart

Clone the repository:

```bash
git clone https://github.com/alexanderogorodnikov/tech-a
cd tech-a/helm

```

## Package build and deploy to k8s

```bash
helm package hello-world
helm install hello-world-release ./my-hello-world-chart-0.1.0.tgz
```

## Configuration

| Parameter         | Description                |Default
|-------------------|--------------------------------------------------------------------------
| image.repository  | Container image repository | localhost/hello-world
| image.tag         | Container image tag        | latest

To override a specific value during installation, use the --set key=value option with the helm install command.

## Uninstalling the Chart

```bash
helm uninstall hello-world-release
```

## Contributing

1. Fork the repository
2. Create a new branch: git checkout -b feature/new-feature
3. Commit your changes: git commit -am 'Add new feature'
4. Push the branch: git push origin feature/new-feature
5. Submit a pull request

## Notes

1) `podman build` used to build the nginx image. Ref: <https://docs.podman.io/en/stable/markdown/podman-build.1.html>
Other the podman, there are `kaniko`, `img`, `nerdctl`
2) This project is using `./docker/.Dockerfile` to build nginx server and `hello-world.html` as a main html page
3) There are some ways to build the image in k8s and then use it on a fly without pushing it to repository:

- If we use Minikube it can be done by  `minikube image load` for example
- If we use Cloud specific of vanila k8s, there is a isolation between k8s API, node host and pods and importing of images build inside pod is not suggested as it represents high security risk
 However we can do it by using:
- combination of bidirectionally shared volumes between Pod and Node
- allowPrivilegeEscalation security context of running pod
- `nsenter` command to run remote command FROM pod ONE the node
