# poc-kubernetes

Proof of concept using kubernetes

## Deploying a Basic API with kubectl

[Kubernetes Concepts](https://gist.github.com/GuillaumeFalourd/c10c871e67dd93eae35797b336a5046d)

### Prerequisites

1. **A Kubernetes cluster is up and running** (e.g., Minikube, Kind, or a managed Kubernetes service like GKE, EKS, or AKS).

For Minikube (Ubuntu):
```shell
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube start
minikube status
```

2. **kubectl is installed and configured to interact with your cluster.**

With Minikube (Ubuntu):
```shell
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client
kubectl config use-context minikube
```

3. **[Docker is installed](https://docs.docker.com/engine/install/ubuntu/) to build the API image**.

### Step by Step

1. **Fork this repository.**

2. **At the repository root, build and push the Docker image:**
```shell
docker login -u <your-dockerhub-username> -p <your-dockerhub-pass-or-access-token>
docker build -t <your-dockerhub-username>/basic-api:latest .
docker push <your-dockerhub-username>/basic-api:latest
```

3. **Set `<your-dockerhub-username>` in the deployment.yaml file.**

4. **Apply the Deployment:**
```shell
kubectl apply -f deployment.yaml
```

5. **Apply the Service:**
```shell
kubectl apply -f service.yaml
```

6. **Check the status of the pods:**
```shell
kubectl get pods
```
The output should look like this:
```shell
kubectl get pods
NAME                         READY   STATUS    RESTARTS   AGE
basic-api-7867dfb5fb-xxxxx   1/1     Running   0          17s
basic-api-7867dfb5fb-yyyyy   1/1     Running   0          17s
```

7. **Describe a specific pod (replace <pod-name> with the name of one of the pods):**
```shell
kubectl describe pod <pod-name>
```

8. **View the logs of a pod:**
```shell
kubectl logs <pod-name>
```
The output should look like this:
```shell
 * Serving Flask app 'app'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:5000
 * Running on http://10.244.0.4:5000
Press CTRL+C to quit
```

9. **Access the application locally using port-forwarding:**
```shell
kubectl port-forward service/basic-api-service 8080:80
```
The output should look like this:
```shell
Forwarding from 127.0.0.1:8080 -> 5000
Forwarding from [::1]:8080 -> 5000
```

10. **Open your browser and navigate to [http://localhost:8080](http://localhost:8080)**

_You should see the message: `{"message": "Hello, Kubernetes!"}` as below:_

![](https://github.com/user-attachments/assets/945da275-af92-40ef-bd33-e1f632d68a29)

12. **Scale the deployment to 4 replicas:** 
```shell
kubectl scale deployment basic-api --replicas=4
```
The output should look like this:
```shell
deployment.apps/basic-api scaled
```

12. **Verify the new number of pods:**
```shell
kubectl get pods
```
The output should look like this:
```shell
NAME                         READY   STATUS    RESTARTS   AGE
basic-api-7867dfb5fb-xxxxx   1/1     Running   0          28s
basic-api-7867dfb5fb-yyyyy   1/1     Running   0          5m39s
basic-api-7867dfb5fb-aaaaa   1/1     Running   0          5m39s
basic-api-7867dfb5fb-bbbbb   1/1     Running   0          28s
```

13. **Delete the Deployment:**
```shell
kubectl delete -f deployment.yaml
```
The output should look like this:
```shell
deployment.apps "basic-api" deleted
```

14. **Delete the Service:** 
```shell
kubectl delete -f service.yaml
```
The output should look like this:
```shell
service "basic-api-service" deleted
```

## Summary

- `kubectl apply -f <file.yaml>`: Create or update resources.
- `kubectl get pods`: List all pods.
- `kubectl describe pod <pod-name>`: Get detailed information about a pod.
- `kubectl logs <pod-name>`: View logs of a pod.
- `kubectl port-forward service/<service-name> <local-port>:<service-port>`: Forward a local port to a service.
- `kubectl scale deployment <deployment-name> --replicas=<number>`: Scale a deployment.
- `kubectl delete -f <file.yaml>`: Delete resources.
