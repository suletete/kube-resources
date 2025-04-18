---

## 📘 Working with Kubernetes Resources

### 🧾 Introduction to YAML

A **Kubernetes YAML file** is a configuration file that defines the desired state of Kubernetes resources (such as Pods, Containers, Services, Deployments) within a Kubernetes cluster.

**YAML** stands for "**YAML Ain’t Markup Language**" and is widely used for configuration files because of its **human-readable format**. In Kubernetes, YAML files are used to specify the desired state of resources declaratively. This means you describe the "end state" you want for your resources, and Kubernetes will work to ensure that state is achieved.

---

### 🧱 Basic Structure of a YAML File

YAML relies on **indentation** to represent the hierarchy of data, where spaces (not tabs) are used for indentation. This is important to ensure correct parsing of data.

#### **Example YAML Syntax:**

```yaml
key1: value1
key2:
  subkey1: subvalue1
  subkey2: subvalue2
key3:
  - item1
  - item2
```

---

### 🔑 Data Types in YAML

1. **Scalars** (Single values)

   - **Strings:**

   ```yaml
   name: John Doe
   ```

   - **Numbers:**

   ```yaml
   age: 25
   ```

   - **Booleans:**

   ```yaml
   is_student: true
   ```

2. **Collections**

   - **Lists (Arrays):**

   ```yaml
   fruits:
     - apple
     - banana
     - orange
   ```

   - **Maps (Key-value pairs):**

   ```yaml
   person:
     name: Alice
     age: 30
   ```

3. **Nested Structures:**

   YAML allows for nesting of data structures.

   ```yaml
   employee:
     name: John Doe
     position: developer
     skills:
       - Python
       - JavaScript
   ```

4. **Comments:**

   Comments in YAML start with a `#` symbol.

   ```yaml
   # This is a comment
   key: value
   ```

5. **Multiline Strings:**

   Use `|` for multiline strings, preserving line breaks.

   ```yaml
   description: |
     This is a multiline
     string in YAML.
   ```

   Use `>` for folding multiline strings into a single line.

   ```yaml
   description: >
     This is a long description
     that will be wrapped into a single line.
   ```

6. **Anchors and Aliases:**

   YAML allows you to reuse data using **anchors** (`&`) and **aliases** (`*`).

   ```yaml
   first: &name John
   second: *name  # second will have the value of first
   ```

---

### 🚀 Deploying Applications in Kubernetes

Deploying applications is a core skill in Kubernetes. It involves deploying your code to the cluster and ensuring that it scales, manages resources, and remains resilient.

---

### 🏗️ Kubernetes Deployment

In Kubernetes, a **Deployment** defines the desired state for your application, including the number of replicas, the container images to use, and how to manage them.

#### Example Deployment Command:

```bash
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
```

This command creates a **Deployment** named `hello-minikube` using the `kicbase/echo-server:1.0` container image.

---

### 🛠️ Kubernetes Services

Once your application is deployed, a **Service** allows other components or external users to access it. In Kubernetes, a **Service** abstracts a set of Pods and provides a stable endpoint for accessing them.

#### Types of Services in Kubernetes:

1. **ClusterIP**: Exposes the Service on a cluster-internal IP. Accessible only within the cluster.
2. **NodePort**: Exposes the Service on each Node's IP at a static port. Accessible externally through `NodeIP`.
3. **LoadBalancer**: Exposes the Service externally using a cloud provider's load balancer. Accessible externally via the load balancer's IP.

---

### 🔧 Deploying a Minikube Sample Application

Follow these steps to deploy a sample app in **Minikube** using YAML files for both Deployment and Service.

1. **Create Deployment:**

   ```bash
   kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
   ```

2. **Expose Deployment as a Service:**

   ```bash
   kubectl expose deployment hello-minikube --type=NodePort --port=8080
   ```

3. **Check Services:**

   ```bash
   kubectl get services hello-minikube
   ```

4. **Access the Service:**

   Use Minikube to open the service in your web browser:

   ```bash
   minikube service hello-minikube
   ```

---

### 🛠️ Working With YAML Files in Kubernetes

Let's use an example to deploy a custom Nginx container using YAML files.

#### Step 1: Create a folder and YAML files

1. **Create a folder for your YAML files:**

   ```bash
   mkdir my-nginx-yaml
   cd my-nginx-yaml
   ```

2. **Create the Deployment YAML file (`nginx-deployment.yaml`):**

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: my-nginx-deployment
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: my-nginx
     template:
       metadata:
         labels:
           app: my-nginx
       spec:
         containers:
           - name: my-nginx
             image: dareyregistry/my-nginx:1.0
             ports:
               - containerPort: 80
   ```

#### Step 2: Create the Service YAML file (`nginx-service.yaml`):

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nginx-service
spec:
  selector:
    app: my-nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```

#### Step 3: Apply the YAML Files

```bash
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
```

#### Step 4: Verify Deployment and Service

```bash
kubectl get deployments
kubectl get services
```

#### Step 5: Access the Application

```bash
minikube service my-nginx-service --url
```

---

### 📌 Summary

- YAML files define and manage Kubernetes resources in a **declarative** manner.
- **Deployments** and **Services** are essential components for scaling and accessing applications.
- **Minikube** provides a local Kubernetes environment for testing deployments and services.

