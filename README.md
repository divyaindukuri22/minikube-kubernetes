# minikube-kubernetes

## ✅ Prerequisites

- Docker installed and running
- Minikube installed
- Kubectl installed
- Basic knowledge of pods, deployments, and services

---

## 🧱 Project Structure

```
minikube-kubernetes-flask/
│
├── app.py
├── Dockerfile
├── flask-deployment.yaml
└── flask-service.yaml
```

---

## 🔥 Step 1: Create Your Flask App

`app.py`:

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello, Divya! Running on Kubernetes 🎉"

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

---

## 🐳 Step 2: Create Dockerfile

`Dockerfile`:

```dockerfile
FROM python:3.9
WORKDIR /app
COPY . /app
RUN pip install flask
CMD ["python", "app.py"]
```

---

## 🔧 Step 3: Start Minikube

```bash
minikube start
```

✅ Minikube creates a local single-node Kubernetes cluster.

---

## 🏗️ Step 4: Build Docker Image inside Minikube

```bash
eval $(minikube docker-env)  # point Docker to Minikube
docker build -t flask-app .
```

---

## 📦 Step 5: Create Kubernetes Deployment YAML

`flask-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: flask-container
        image: flask-app
        ports:
        - containerPort: 5000
```

---

## 🌐 Step 6: Create Kubernetes Service YAML

`flask-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: flask-service
spec:
  type: NodePort
  selector:
    app: flask-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5000
      nodePort: 30036
```

---

## 🚀 Step 7: Apply Deployment & Service

```bash
kubectl apply -f flask-deployment.yaml
kubectl apply -f flask-service.yaml
```

---

## 🔍 Step 8: Check Everything is Running

```bash
kubectl get pods
kubectl get services
```

You should see:
```
flask-service   NodePort   ...   30036:<your-port> ...
```

---

## 🌐 Step 9: Access the App

```bash
minikube service flask-service
```

It will open in your browser! 🎉 Or you can manually visit:
```
http://localhost:30036/
```

---

## 🧼 Step 10: Cleanup

```bash
kubectl delete -f flask-deployment.yaml
kubectl delete -f flask-service.yaml
minikube stop
```

## 💡 Bonus

- You can also scale your app:
  ```bash
  kubectl scale deployment flask-deployment --replicas=5
  ```
- Watch rolling updates:
  ```bash
  kubectl rollout status deployment flask-deployment
  ```

---

📂 Save this with your folder like so:

```
minikube-kubernetes-flask/
├── README.md
├── app.py
├── Dockerfile
├── flask-deployment.yaml
└── flask-service.yaml
```
```
