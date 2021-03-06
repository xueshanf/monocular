# Monocular
[![Build
Status](https://travis-ci.org/kubernetes-helm/monocular.svg?branch=master)](https://travis-ci.org/kubernetes-helm/monocular)

Monocular is web-based UI for managing Kubernetes applications packaged as Helm
Charts. It allows you to search and discover available charts from multiple
repositories, and install them in your cluster with one click.

![Monocular Screenshot](docs/MonocularScreenshot.gif)

See Monocular in action at [KubeApps.com](https://kubeapps.com) or click [here](docs/about.md) to learn more about Helm, Charts and Kubernetes.

##### Video links
- [Screencast](https://www.youtube.com/watch?v=YoEbvDrI5ng)
- [Helm and Monocular Webinar](https://www.youtube.com/watch?v=u8kDkHgRbWQ)

## Install

You can use the chart in this repository to install Monocular in your cluster.

### Prerequisites
- [Helm and Tiller installed](https://github.com/kubernetes/helm/blob/master/docs/quickstart.md)
- [Nginx Ingress controller](https://kubeapps.com/charts/stable/nginx-ingress)
  - Install with Helm: `helm install stable/nginx-ingress`

  - **Minikube/Kubeadm**: `helm install stable/nginx-ingress --set controller.hostNetwork=true`
  
  - **AWS**: For testing purpose, the install example below will set ELB security rule to allow connections from $MY_IP only.
  	```console
    $ MY_IP=$(curl -s http://httpbin.org/ip | jq -r '.origin')/32
    $ helm install stable/nginx-ingress --set controller.service.loadBalancerSourceRanges={${MY_IP}} --set controller.publishService.enabled=true
    ```

### Install monocular

```console
$ helm repo add monocular https://kubernetes-helm.github.io/monocular
$ helm install monocular/monocular
```
### Access Monocular

Use the Ingress endpoint to access your Monocular instance:

```console
# Wait for all pods to be running (this can take a few minutes)
$ kubectl get pods --watch

$ kubectl get ingress
NAME                        HOSTS     ADDRESS         PORTS     AGE
tailored-alpaca-monocular   *         192.168.64.30   80        11h
```

Visit the address specified in the Ingress object in your browser, e.g. http://192.168.64.30.

**Note for AWS**, because of the setting `controller.publishService.enabled=true` when the nginx-ingress controller is created, the Ingress object will be bound to ELB DNS name, instead of controller's node IP.

```console
$ kuberctl  get ingress -o wide
NAME                           HOSTS     ADDRESS                                                                  PORTS     AGE
ailored-alpaca-monocular-monocular   *         a134e1a1082ff11e7a6c102e36c6d789-123456789.us-west-2.elb.amazonaws.com   80        14m
```

You can visit UI at https://a134e1a1082ff11e7a6c102e36c6d789-123456789.us-west-2.elb.amazonaws.com.

Read more on how to deploy Monocular [here](deployment/monocular/README.md).

## Documentation

- [Configuration](deployment/monocular/README.md#configuration)
- [Deployment](deployment/monocular/README.md)
- [Development](docs/development.md)

## Roadmap

The [Monocular roadmap is currently located in the wiki](https://github.com/kubernetes-helm/monocular/wiki/Roadmap).

## Contribute

This project is still under active development, so you'll likely encounter
[issues](https://github.com/kubernetes-helm/monocular/issues).

Interested in contributing? Check out the [documentation](CONTRIBUTING.md).

Also see [developer's guide](docs/development.md) for information on how to
build and test the code.
