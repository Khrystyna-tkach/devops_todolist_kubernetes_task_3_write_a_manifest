All Kubernetes manifests are located in the `.infrastructure` folder.

Create the namespace:

```bash
kubectl apply -f .infrastructure/namespace.yml
```

Create the ToDo application pod:

```bash
kubectl apply -f .infrastructure/todoapp-pod.yml
```

Create the BusyBox pod:

```bash
kubectl apply -f .infrastructure/busybox.yml
```

Check that all pods are running:

```bash
kubectl get pods -n todoapp
```

To display pod IP addresses:

```bash
kubectl get pods -n todoapp -o wide
```

## Test ToDo application using port-forward

Forward local port `8000` to port `8000` of the ToDo application pod:

```bash
kubectl port-forward -n todoapp pod/todoapp 8000:8000
```

Open the application:

```text
http://localhost:8000/
```

Test the readiness endpoint:

```text
http://localhost:8000/api/readiness/
```

Expected response:

```text
Ready
```

Test the liveness endpoint:

```text
http://localhost:8000/api/liveness/
```

Expected response:

```text
Live
```

## Test ToDo application using BusyBox

The BusyBox pod uses the following image:

```text
ikulykin404/busyboxplus:curl
```

First, get the IP address of the `todoapp` pod:

```bash
kubectl get pods -n todoapp -o wide
```

Test readiness endpoint:

```bash
kubectl exec -n todoapp busybox -- curl http://<TODOAPP_POD_IP>:8000/api/readiness/
```

Expected response:

```text
Ready
```

Test liveness endpoint:

```bash
kubectl exec -n todoapp busybox -- curl http://<TODOAPP_POD_IP>:8000/api/liveness/
```
Expected response:

```text
Live
```

Test the main ToDo application endpoint:

```bash
kubectl exec -n todoapp busybox -- curl http://<TODOAPP_POD_IP>:8000/
```