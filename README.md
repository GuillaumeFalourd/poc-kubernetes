# poc-kubernetes

Proof of concept using kubernetes

## Deploying a Basic API with kubectl

[Kubernetes Concepts](https://gist.github.com/GuillaumeFalourd/c10c871e67dd93eae35797b336a5046d)

### Prerequisites

1. A Kubernetes cluster is up and running (e.g., Minikube, Kind, or a managed Kubernetes service like GKE, EKS, or AKS).
2. kubectl is installed and configured to interact with your cluster.
3. Docker is installed to build the API image (optional if using a pre-built image).

### Step by Step

1. Fork this repository.

2. At the repository root, build and push the Docker image:
```shell
docker login -u <your-dockerhub-username> -p <your-dockerhub-pass-or-access-token>
docker build -t <your-dockerhub-username>/basic-api:latest .
docker push <your-dockerhub-username>/basic-api:latest
```

3. Set <your-dockerhub-username> in the deployment.yaml file.

4. Apply the Deployment: `kubectl apply -f deployment.yaml`.

5. Apply the Service: `kubectl apply -f service.yaml`.

6. Check the status of the pods: `kubectl get pods`.

7. Describe a specific pod (replace <pod-name> with the name of one of the pods): `kubectl describe pod <pod-name>`.

8. View the logs of a pod: `kubectl logs <pod-name>`.

9. Access the application locally using port-forwarding: `kubectl port-forward service/basic-api-service 8080:80`.

10. Open your browser and navigate to http://localhost:8080
_You should see the message: `{"message": "Hello, Kubernetes!"}`._

11. Scale the deployment to 4 replicas: `kubectl scale deployment basic-api --replicas=4`.

12. Verify the new number of pods: `kubectl get pods`.

13. Delete the Deployment: `kubectl delete -f deployment.yaml`

14. Delete the Service: `kubectl delete -f service.yaml`.

## Summary

- `kubectl apply -f <file.yaml>`: Create or update resources.
- `kubectl get pods`: List all pods.
- `kubectl describe pod <pod-name>`: Get detailed information about a pod.
- `kubectl logs <pod-name>`: View logs of a pod.
- `kubectl port-forward service/<service-name> <local-port>:<service-port>`: Forward a local port to a service.
- `kubectl scale deployment <deployment-name> --replicas=<number>`: Scale a deployment.
- `kubectl delete -f <file.yaml>`: Delete resources.