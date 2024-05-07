# Ecommerce App - CI/CD eith Github Actions and ArgoCD

## About the Microservices
The applications used for demonstrative purposes are Orders and Products. The applications are written in JavaScript (NodeJS based APIs). The Orders API returns a list of orders, whereas the products API returns a list of products.
![CI/CD and GitOps with Argo CD](image.png)


## Stage 1: Continuos Integration process
Updates in app code triggers on CI pipeline process to build/test/deploy the container image for application component.
The CI pipeline will update the app CD configuration files with the new image of the component of the application. Argo CD allows us to use Argo Rollouts.


![Updates to app code in CI pipeline](image-1.png)

## Stage 2: Continuos Delivery process:
ArgoCD allows the code repository (thats why the term GitOps) to be the source of truth. The app is redeployed on the K8s cluster with the new image tag per microservice.
In such a way, even if the developer/ops team update the state of the application with kubectl command, ArgoCD will detect the change and redeploy the app to the desired state, using Helm charts.

Alternatively, you can update the `Services` from a `ClusterIP` to `LoadBalancer`. 
*It's a best practice to use an Ingress controller like the Load Balancer Controller to expose your applications to external traffic.*

## GitHub Actions Deployment Config
The `.github/workflows` folder contains the *main.yaml* configuration file the is used to configure the build 


## Helm Charts

The `application-charts` folder contains the Helm charts for both microservices. Helm is a package manager for Kubernetes resources. Instead of managing multiple Kubernetes manifests, you can use Helm to manage the deployment lifecycle for your Kubernetes applications. It also has a templating engine to automatically generate the relevant resources for a given application.

```
.
|-- orders
|   |-- Chart.yaml
|   |-- charts
|   |-- orders.yaml
|   |-- templates
|   |   |-- NOTES.txt
|   |   |-- _helpers.tpl
|   |   |-- deployment.yaml
|   |   |-- hpa.yaml
|   |   |-- ingress.yaml
|   |   |-- service.yaml
|   |   |-- serviceaccount.yaml
|   |   `-- tests
|   `-- values.yaml
`-- products
    |-- Chart.yaml
    |-- charts
    |-- templates
    |   |-- NOTES.txt
    |   |-- _helpers.tpl
    |   |-- deployment.yaml
    |   |-- hpa.yaml
    |   |-- ingress.yaml
    |   |-- service.yaml
    |   |-- serviceaccount.yaml
    |   `-- tests
    `-- values.yaml
```

Example:
```
cd ./application-charts/orders
helm install -f values-dev.yaml orders .
```

### Endpoints for Microservices
When running the microservices locally or in your Amazon EKS cluster, you can use the following API endpoints for testing purposes.

#### Orders 
```
GET Orders - /v1/orders
GET Orders and Products - /v1/orders/products
``` 

#### Products
``` 
GET Products - /v1/products
``` 

### Container Image Repositories

#### Orders 
Repository: ghcr.io/alex00pep/ecommerce-orders
<br />Image tag: "0.0.1"

#### Products
Repository: ghcr.io/alex00pep/ecommerce-products
<br />Image tag: "0.0.1"

