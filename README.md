# Helm Chart for Webapp
----

# Web Application Helm Chart
 
This Helm chart is designed to deploy a web application on a Kubernetes cluster with various features and best practices.
 
## Application Health Probes
 
This Helm chart includes HTTP-based Readiness and Liveness probes for the web application.
 
- **Readiness Probe**: Ensures that the application can connect to the database and is ready to serve traffic. It will fail if the application cannot connect to the database.
 
- **Liveness Probe**: Validates that the application is up and running. It will fail if the application crashes or an Out of Memory (OOM) error is detected.
 
## Application Configuration
 
The application must be configured using ConfigMap and Secrets. It does not hard code values, making it flexible and secure.
 
## Database
 
The PostgreSQL database is deployed using a StatefulSet with persistent storage.
 
## Application Helm Charts
 
The Helm chart is designed to deploy the web application on Kubernetes, It includes:
 
- Service
- ConfigMaps
- Secret
- Customizable values in `values.yaml`.
 
You can deploy multiple releases of the application on the same K8S cluster using this chart.
 
## Kubernetes Deployment
 
The application is deployed using a Kubernetes Deployment resource with the following settings:
 
- Deployment strategy: RollingUpdate
- maxSurge: 1
- maxUnavailable: 0
- Replica count: 3
- minReadySeconds: 30
- progressDeadlineSeconds: 180
 
All values are customizable in the Helm chart, avoiding hardcoding.
 
## Kubernetes Load Balancer Service
 
The application is served using a load balancer service, allowing endpoints to be accessible over the internet, ensuring availability and accessibility.
 
## CI/CD Pipeline for Helm Charts
 
A continuous deployment pipeline has been set up in Jenkins to create new releases of the Helm chart.
- Automated versioning of the Helm chart using semantic-release.
- Updating the version in `Chart.yaml`.
- Creating a GitHub release for the new version of the chart.
 
## Usage
 
Here are the steps to deploy the web application using the Helm chart:
 
1. Clone this repository:
    ```
     git clone
    ```
  - https://github.com/csye7125-fall2023-group07/webapp-helm-chart

 
Customize the Helm chart values in values.yaml as needed.
 
2. Deploy the Helm chart on your Kubernetes cluster:
    ```
     helm install chart-name .
    ```
 
3.  Verify the deployment
  - To verify that the release was successfully deployed,
    ```
      helm list
    ```
 
4. Uninstall the Chart
    ```
     helm uninstall chart-name .
    ```
Author: Souvik Dinda, Naga Vaishnavi Puppala