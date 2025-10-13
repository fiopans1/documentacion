# ☸️ Guía Completa de Kubernetes y Helm Charts

## 📋 Índice

### Kubernetes
1. [Introducción a Kubernetes](#1-introducción-a-kubernetes)
2. [Arquitectura de Kubernetes](#2-arquitectura-de-kubernetes)
3. [Instalación y Setup](#3-instalación-y-setup)
4. [Pods](#4-pods)
5. [Deployments](#5-deployments)
6. [Services](#6-services)
7. [ConfigMaps y Secrets](#7-configmaps-y-secrets)
8. [Volumes y Persistent Storage](#8-volumes-y-persistent-storage)
9. [Namespaces](#9-namespaces)
10. [Labels y Selectors](#10-labels-y-selectors)
11. [Ingress](#11-ingress)
12. [StatefulSets](#12-statefulsets)
13. [DaemonSets](#13-daemonsets)
14. [Jobs y CronJobs](#14-jobs-y-cronjobs)
15. [Health Checks](#15-health-checks)
16. [Resource Management](#16-resource-management)
17. [RBAC](#17-rbac)

### Helm
18. [Introducción a Helm](#18-introducción-a-helm)
19. [Instalación de Helm](#19-instalación-de-helm)
20. [Helm Charts](#20-helm-charts)
21. [Templates](#21-templates)
22. [Values](#22-values)
23. [Helm Hooks](#23-helm-hooks)
24. [Chart Dependencies](#24-chart-dependencies)
25. [Helm Best Practices](#25-helm-best-practices)
26. [Troubleshooting](#26-troubleshooting)

---

## PARTE 1: Kubernetes

## 1. Introducción a Kubernetes

Kubernetes (K8s) es una plataforma de orquestación de contenedores open-source que automatiza el despliegue, escalado y gestión de aplicaciones en contenedores.

### ¿Por qué Kubernetes?

- **Orquestación automática**: Gestiona contenedores automáticamente
- **Auto-scaling**: Escala aplicaciones según demanda
- **Self-healing**: Reinicia contenedores fallidos automáticamente
- **Service discovery**: Descubrimiento automático de servicios
- **Load balancing**: Balanceo de carga incorporado
- **Rolling updates**: Actualizaciones sin tiempo de inactividad
- **Portabilidad**: Funciona en cualquier infraestructura

---

## 2. Arquitectura de Kubernetes

### Componentes del Control Plane

```plaintext
┌─────────────────────────────────────────┐
│         Control Plane (Master)          │
├─────────────────────────────────────────┤
│ • kube-apiserver                        │
│ • etcd                                  │
│ • kube-scheduler                        │
│ • kube-controller-manager               │
│ • cloud-controller-manager (opcional)   │
└─────────────────────────────────────────┘
         │
         │ Comunicación
         ▼
┌─────────────────────────────────────────┐
│          Worker Nodes                   │
├─────────────────────────────────────────┤
│ • kubelet                               │
│ • kube-proxy                            │
│ • Container Runtime (Docker, containerd)│
│ • Pods (aplicaciones)                   │
└─────────────────────────────────────────┘
```

### Componentes clave:

- **kube-apiserver**: API principal de Kubernetes
- **etcd**: Almacén de datos distribuido
- **kube-scheduler**: Asigna pods a nodos
- **kube-controller-manager**: Ejecuta controladores
- **kubelet**: Agente en cada nodo
- **kube-proxy**: Gestiona red en cada nodo

---

## 3. Instalación y Setup

### Minikube (Local Development)

```bash
# Instalar Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Iniciar cluster
minikube start

# Verificar estado
minikube status

# Detener
minikube stop

# Eliminar
minikube delete
```

### kubectl (CLI)

```bash
# Instalar kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Verificar instalación
kubectl version --client

# Ver info del cluster
kubectl cluster-info

# Ver nodos
kubectl get nodes

# Configurar autocompletado
source <(kubectl completion bash)
```

### Comandos básicos de kubectl

```bash
# Obtener recursos
kubectl get pods
kubectl get services
kubectl get deployments
kubectl get namespaces

# Describir recurso
kubectl describe pod <pod-name>
kubectl describe service <service-name>

# Crear recursos desde archivo
kubectl apply -f deployment.yaml
kubectl create -f service.yaml

# Eliminar recursos
kubectl delete pod <pod-name>
kubectl delete -f deployment.yaml

# Logs
kubectl logs <pod-name>
kubectl logs <pod-name> -f  # Follow logs

# Ejecutar comando en pod
kubectl exec -it <pod-name> -- /bin/bash

# Port forwarding
kubectl port-forward <pod-name> 8080:80
```

---

## 4. Pods

Los Pods son la unidad más pequeña y básica en Kubernetes.

### Pod simple

```yaml
# simple-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.21
    ports:
    - containerPort: 80
```

```bash
kubectl apply -f simple-pod.yaml
kubectl get pods
kubectl describe pod nginx-pod
kubectl logs nginx-pod
kubectl delete pod nginx-pod
```

### Pod con múltiples contenedores

```yaml
# multi-container-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  containers:
  - name: nginx
    image: nginx:1.21
    ports:
    - containerPort: 80
    volumeMounts:
    - name: shared-data
      mountPath: /usr/share/nginx/html
  
  - name: sidecar
    image: busybox
    command: ['sh', '-c', 'while true; do echo "Hello from sidecar" > /data/index.html; sleep 10; done']
    volumeMounts:
    - name: shared-data
      mountPath: /data
  
  volumes:
  - name: shared-data
    emptyDir: {}
```

### Pod con recursos y límites

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resource-pod
spec:
  containers:
  - name: app
    image: nginx:1.21
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

---

## 5. Deployments

Los Deployments gestionan el despliegue y escalado de aplicaciones.

### Deployment básico

```yaml
# nginx-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
```

```bash
# Crear deployment
kubectl apply -f nginx-deployment.yaml

# Ver deployments
kubectl get deployments

# Ver pods creados
kubectl get pods -l app=nginx

# Escalar deployment
kubectl scale deployment nginx-deployment --replicas=5

# Ver estado del rollout
kubectl rollout status deployment nginx-deployment

# Ver historial de rollouts
kubectl rollout history deployment nginx-deployment
```

### Actualizar deployment

```yaml
# nginx-deployment-v2.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.22  # Versión actualizada
        ports:
        - containerPort: 80
```

```bash
# Actualizar
kubectl apply -f nginx-deployment-v2.yaml

# Ver progreso
kubectl rollout status deployment nginx-deployment

# Rollback
kubectl rollout undo deployment nginx-deployment

# Rollback a revisión específica
kubectl rollout undo deployment nginx-deployment --to-revision=2
```

### Estrategias de actualización

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
spec:
  replicas: 5
  strategy:
    type: RollingUpdate  # O Recreate
    rollingUpdate:
      maxSurge: 2        # Pods extra durante update
      maxUnavailable: 1  # Pods que pueden estar no disponibles
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: app
        image: myapp:v2
```

---

## 6. Services

Los Services exponen aplicaciones corriendo en Pods.

### ClusterIP (por defecto)

```yaml
# service-clusterip.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: ClusterIP  # Solo accesible dentro del cluster
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80       # Puerto del service
    targetPort: 80 # Puerto del pod
```

### NodePort

```yaml
# service-nodeport.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30080  # Puerto en cada nodo (30000-32767)
```

### LoadBalancer

```yaml
# service-loadbalancer.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-loadbalancer
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

```bash
# Ver services
kubectl get services
kubectl get svc

# Describir service
kubectl describe service nginx-service

# Acceder a service desde dentro del cluster
kubectl run test-pod --image=busybox -it --rm -- wget -O- nginx-service
```

---

## 7. ConfigMaps y Secrets

### ConfigMaps

```yaml
# configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  database_url: "postgres://db:5432/mydb"
  log_level: "info"
  config.json: |
    {
      "key": "value",
      "nested": {
        "data": "example"
      }
    }
```

```bash
# Crear ConfigMap desde archivo
kubectl create configmap app-config --from-file=config.properties

# Crear ConfigMap desde literales
kubectl create configmap app-config --from-literal=key1=value1 --from-literal=key2=value2

# Ver ConfigMaps
kubectl get configmaps
kubectl describe configmap app-config
```

### Usar ConfigMap en Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
  - name: app
    image: myapp:latest
    env:
    # Variables individuales
    - name: DATABASE_URL
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: database_url
    
    # Todas las keys como variables
    envFrom:
    - configMapRef:
        name: app-config
    
    # Montar como volumen
    volumeMounts:
    - name: config
      mountPath: /etc/config
  
  volumes:
  - name: config
    configMap:
      name: app-config
```

### Secrets

```yaml
# secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  username: YWRtaW4=  # base64 de "admin"
  password: cGFzc3dvcmQ=  # base64 de "password"
```

```bash
# Crear Secret
kubectl create secret generic app-secret \
  --from-literal=username=admin \
  --from-literal=password=password

# Crear Secret desde archivo
kubectl create secret generic ssh-key \
  --from-file=ssh-privatekey=~/.ssh/id_rsa

# Ver Secrets
kubectl get secrets
kubectl describe secret app-secret

# Ver contenido (decodificado)
kubectl get secret app-secret -o jsonpath='{.data.password}' | base64 --decode
```

### Usar Secret en Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
  - name: app
    image: myapp:latest
    env:
    - name: DB_USERNAME
      valueFrom:
        secretKeyRef:
          name: app-secret
          key: username
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: app-secret
          key: password
    
    volumeMounts:
    - name: secret
      mountPath: /etc/secrets
      readOnly: true
  
  volumes:
  - name: secret
    secret:
      secretName: app-secret
```

---

## 8. Volumes y Persistent Storage

### EmptyDir

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-emptydir
spec:
  containers:
  - name: writer
    image: busybox
    command: ['sh', '-c', 'echo "Hello" > /data/hello.txt; sleep 3600']
    volumeMounts:
    - name: shared-data
      mountPath: /data
  
  - name: reader
    image: busybox
    command: ['sh', '-c', 'cat /data/hello.txt; sleep 3600']
    volumeMounts:
    - name: shared-data
      mountPath: /data
  
  volumes:
  - name: shared-data
    emptyDir: {}
```

### PersistentVolume y PersistentVolumeClaim

```yaml
# persistent-volume.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-example
spec:
  capacity:
    storage: 5Gi
  accessModes:
  - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: standard
  hostPath:
    path: /data/pv-example

---
# persistent-volume-claim.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-example
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
  storageClassName: standard

---
# pod-with-pvc.yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-pvc
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - name: data
      mountPath: /usr/share/nginx/html
  
  volumes:
  - name: data
    persistentVolumeClaim:
      claimName: pvc-example
```

```bash
kubectl apply -f persistent-volume.yaml
kubectl apply -f persistent-volume-claim.yaml
kubectl apply -f pod-with-pvc.yaml

kubectl get pv
kubectl get pvc
```

---

## 9. Namespaces

Los Namespaces proporcionan aislamiento lógico dentro de un cluster.

```bash
# Ver namespaces
kubectl get namespaces

# Crear namespace
kubectl create namespace dev
kubectl create namespace prod

# Usar namespace específico
kubectl get pods -n dev
kubectl apply -f deployment.yaml -n dev

# Configurar namespace por defecto
kubectl config set-context --current --namespace=dev
```

```yaml
# namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: staging
  labels:
    environment: staging

---
# deployment-in-namespace.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app
  namespace: staging
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: app
        image: myapp:latest
```

---

## 10. Labels y Selectors

```yaml
# Labels en recursos
apiVersion: v1
kind: Pod
metadata:
  name: labeled-pod
  labels:
    environment: production
    tier: frontend
    version: v1.0
spec:
  containers:
  - name: nginx
    image: nginx:1.21
```

```bash
# Obtener recursos con labels específicas
kubectl get pods -l environment=production
kubectl get pods -l tier=frontend,version=v1.0

# Obtener recursos que NO tienen una label
kubectl get pods -l '!environment'

# Expresiones
kubectl get pods -l 'environment in (production, staging)'
kubectl get pods -l 'tier notin (backend)'

# Añadir label
kubectl label pod labeled-pod team=devops

# Eliminar label
kubectl label pod labeled-pod version-

# Ver labels
kubectl get pods --show-labels
```

---

## 11. Ingress

Ingress gestiona el acceso externo a servicios HTTP/HTTPS.

### Instalar Ingress Controller (Nginx)

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/cloud/deploy.yaml
```

### Ingress básico

```yaml
# ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app-service
            port:
              number: 80
```

### Ingress con múltiples paths

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: multi-path-ingress
spec:
  ingressClassName: nginx
  rules:
  - host: example.com
    http:
      paths:
      - path: /app1
        pathType: Prefix
        backend:
          service:
            name: app1-service
            port:
              number: 80
      - path: /app2
        pathType: Prefix
        backend:
          service:
            name: app2-service
            port:
              number: 80
```

### Ingress con TLS

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: tls-ingress
spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - myapp.example.com
    secretName: tls-secret
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app-service
            port:
              number: 80
```

---

## 12. StatefulSets

StatefulSets son para aplicaciones con estado que requieren identidad de red estable.

```yaml
# statefulset.yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mongodb
spec:
  serviceName: mongodb
  replicas: 3
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo:5.0
        ports:
        - containerPort: 27017
        volumeMounts:
        - name: data
          mountPath: /data/db
  
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 5Gi

---
# headless service
apiVersion: v1
kind: Service
metadata:
  name: mongodb
spec:
  clusterIP: None
  selector:
    app: mongodb
  ports:
  - port: 27017
```

---

## 13. DaemonSets

DaemonSets ejecutan un pod en cada nodo del cluster.

```yaml
# daemonset.yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: logging-daemon
spec:
  selector:
    matchLabels:
      app: logging
  template:
    metadata:
      labels:
        app: logging
    spec:
      containers:
      - name: fluentd
        image: fluentd:latest
        volumeMounts:
        - name: varlog
          mountPath: /var/log
      
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
```

---

## 14. Jobs y CronJobs

### Job

```yaml
# job.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: batch-job
spec:
  template:
    spec:
      containers:
      - name: worker
        image: busybox
        command: ['sh', '-c', 'echo Processing batch job; sleep 30; echo Done']
      restartPolicy: Never
  backoffLimit: 4  # Intentos máximos
```

### CronJob

```yaml
# cronjob.yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: backup-job
spec:
  schedule: "0 2 * * *"  # Cada día a las 2 AM
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backup
            image: backup-tool:latest
            command: ["/backup.sh"]
          restartPolicy: OnFailure
```

---

## 15. Health Checks

### Liveness Probe

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: liveness-pod
spec:
  containers:
  - name: app
    image: myapp:latest
    livenessProbe:
      httpGet:
        path: /health
        port: 8080
      initialDelaySeconds: 30
      periodSeconds: 10
      timeoutSeconds: 5
      failureThreshold: 3
```

### Readiness Probe

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: readiness-pod
spec:
  containers:
  - name: app
    image: myapp:latest
    readinessProbe:
      httpGet:
        path: /ready
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 5
```

### Startup Probe

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: startup-pod
spec:
  containers:
  - name: app
    image: slow-starting-app:latest
    startupProbe:
      httpGet:
        path: /startup
        port: 8080
      failureThreshold: 30
      periodSeconds: 10
```

---

## 16. Resource Management

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resource-managed-pod
spec:
  containers:
  - name: app
    image: myapp:latest
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

### HorizontalPodAutoscaler

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: app-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

---

## 17. RBAC

```yaml
# role.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: default
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]

---
# rolebinding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
- kind: User
  name: jane
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

---

## PARTE 2: Helm

## 18. Introducción a Helm

Helm es un gestor de paquetes para Kubernetes que simplifica el despliegue y gestión de aplicaciones.

### Conceptos clave:

- **Chart**: Paquete de Helm (contiene templates de Kubernetes)
- **Release**: Instancia de un chart desplegada
- **Repository**: Repositorio de charts
- **Values**: Configuración para personalizar un chart

---

## 19. Instalación de Helm

```bash
# Instalar Helm
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Verificar instalación
helm version

# Añadir repositorios
helm repo add stable https://charts.helm.sh/stable
helm repo add bitnami https://charts.bitnami.com/bitnami

# Actualizar repositorios
helm repo update

# Buscar charts
helm search repo nginx
helm search hub wordpress

# Ver repositorios
helm repo list
```

---

## 20. Helm Charts

### Estructura de un chart

```plaintext
mychart/
├── Chart.yaml          # Metadatos del chart
├── values.yaml         # Valores por defecto
├── charts/             # Charts dependientes
├── templates/          # Templates de Kubernetes
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── _helpers.tpl   # Helpers reutilizables
│   └── NOTES.txt      # Notas post-instalación
└── .helmignore        # Archivos a ignorar
```

### Crear un chart

```bash
# Crear nuevo chart
helm create mychart

# Ver estructura
tree mychart

# Validar chart
helm lint mychart

# Empaquetar chart
helm package mychart

# Ver templates renderizados
helm template mychart

# Instalar chart
helm install myrelease mychart

# Ver releases
helm list

# Actualizar release
helm upgrade myrelease mychart

# Eliminar release
helm uninstall myrelease
```

---

## 21. Templates

### Chart.yaml

```yaml
apiVersion: v2
name: myapp
description: A Helm chart for my application
type: application
version: 1.0.0
appVersion: "1.0"
keywords:
  - myapp
  - web
maintainers:
  - name: Your Name
    email: you@example.com
```

### deployment.yaml template

```yaml
# templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "mychart.fullname" . }}
  labels:
    {{- include "mychart.labels" . | nindent 4 }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      {{- include "mychart.selectorLabels" . | nindent 6 }}
  template:
    metadata:
      labels:
        {{- include "mychart.selectorLabels" . | nindent 8 }}
    spec:
      containers:
      - name: {{ .Chart.Name }}
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"
        imagePullPolicy: {{ .Values.image.pullPolicy }}
        ports:
        - name: http
          containerPort: {{ .Values.service.port }}
          protocol: TCP
        livenessProbe:
          httpGet:
            path: {{ .Values.livenessProbe.path }}
            port: http
        readinessProbe:
          httpGet:
            path: {{ .Values.readinessProbe.path }}
            port: http
        resources:
          {{- toYaml .Values.resources | nindent 10 }}
```

### service.yaml template

```yaml
# templates/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ include "mychart.fullname" . }}
  labels:
    {{- include "mychart.labels" . | nindent 4 }}
spec:
  type: {{ .Values.service.type }}
  ports:
  - port: {{ .Values.service.port }}
    targetPort: http
    protocol: TCP
    name: http
  selector:
    {{- include "mychart.selectorLabels" . | nindent 4 }}
```

### _helpers.tpl

```yaml
# templates/_helpers.tpl
{{/*
Expand the name of the chart.
*/}}
{{- define "mychart.name" -}}
{{- default .Chart.Name .Values.nameOverride | trunc 63 | trimSuffix "-" }}
{{- end }}

{{/*
Create a default fully qualified app name.
*/}}
{{- define "mychart.fullname" -}}
{{- if .Values.fullnameOverride }}
{{- .Values.fullnameOverride | trunc 63 | trimSuffix "-" }}
{{- else }}
{{- $name := default .Chart.Name .Values.nameOverride }}
{{- if contains $name .Release.Name }}
{{- .Release.Name | trunc 63 | trimSuffix "-" }}
{{- else }}
{{- printf "%s-%s" .Release.Name $name | trunc 63 | trimSuffix "-" }}
{{- end }}
{{- end }}
{{- end }}

{{/*
Common labels
*/}}
{{- define "mychart.labels" -}}
helm.sh/chart: {{ include "mychart.chart" . }}
{{ include "mychart.selectorLabels" . }}
{{- if .Chart.AppVersion }}
app.kubernetes.io/version: {{ .Chart.AppVersion | quote }}
{{- end }}
app.kubernetes.io/managed-by: {{ .Release.Service }}
{{- end }}

{{/*
Selector labels
*/}}
{{- define "mychart.selectorLabels" -}}
app.kubernetes.io/name: {{ include "mychart.name" . }}
app.kubernetes.io/instance: {{ .Release.Name }}
{{- end }}
```

---

## 22. Values

### values.yaml

```yaml
# Default values for mychart
replicaCount: 2

image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: "1.21"

service:
  type: ClusterIP
  port: 80

ingress:
  enabled: false
  className: "nginx"
  annotations: {}
  hosts:
    - host: chart-example.local
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls: []

resources:
  limits:
    cpu: 100m
    memory: 128Mi
  requests:
    cpu: 100m
    memory: 128Mi

livenessProbe:
  path: /
readinessProbe:
  path: /

autoscaling:
  enabled: false
  minReplicas: 1
  maxReplicas: 100
  targetCPUUtilizationPercentage: 80

nodeSelector: {}
tolerations: []
affinity: {}
```

### Usar values en templates

```yaml
# Acceder a values
{{ .Values.replicaCount }}
{{ .Values.image.repository }}

# Con valor por defecto
{{ .Values.image.tag | default .Chart.AppVersion }}

# Condicionales
{{- if .Values.ingress.enabled }}
# contenido
{{- end }}

# Loops
{{- range .Values.ingress.hosts }}
  - host: {{ .host | quote }}
{{- end }}
```

### Overriding values

```bash
# Override desde comando
helm install myrelease mychart --set replicaCount=5

# Override múltiples valores
helm install myrelease mychart \
  --set replicaCount=5 \
  --set image.tag=1.22

# Override desde archivo
helm install myrelease mychart -f custom-values.yaml

# Ver valores computados
helm get values myrelease
```

---

## 23. Helm Hooks

Los hooks ejecutan recursos en momentos específicos del ciclo de vida.

```yaml
# templates/pre-install-job.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: "{{ .Release.Name }}-pre-install"
  annotations:
    "helm.sh/hook": pre-install
    "helm.sh/hook-weight": "-5"
    "helm.sh/hook-delete-policy": hook-succeeded
spec:
  template:
    spec:
      containers:
      - name: pre-install-job
        image: busybox
        command: ['sh', '-c', 'echo Pre-install hook running']
      restartPolicy: Never
```

### Tipos de hooks:

- `pre-install`: Antes de instalar
- `post-install`: Después de instalar
- `pre-upgrade`: Antes de actualizar
- `post-upgrade`: Después de actualizar
- `pre-delete`: Antes de eliminar
- `post-delete`: Después de eliminar
- `pre-rollback`: Antes de rollback
- `post-rollback`: Después de rollback

---

## 24. Chart Dependencies

### Chart.yaml con dependencias

```yaml
apiVersion: v2
name: myapp
version: 1.0.0
dependencies:
  - name: postgresql
    version: 12.x.x
    repository: https://charts.bitnami.com/bitnami
    condition: postgresql.enabled
    tags:
      - database
  
  - name: redis
    version: 17.x.x
    repository: https://charts.bitnami.com/bitnami
    condition: redis.enabled
```

```bash
# Actualizar dependencias
helm dependency update mychart

# Listar dependencias
helm dependency list mychart

# Ver charts incluidos
ls mychart/charts/
```

### Configurar dependencias

```yaml
# values.yaml
postgresql:
  enabled: true
  auth:
    username: myuser
    password: mypassword
    database: mydb
  primary:
    persistence:
      size: 8Gi

redis:
  enabled: true
  auth:
    password: redispassword
  master:
    persistence:
      size: 8Gi
```

---

## 25. Helm Best Practices

### 1. Usar templates helpers

```yaml
# _helpers.tpl
{{- define "mychart.labels" -}}
app.kubernetes.io/name: {{ include "mychart.name" . }}
app.kubernetes.io/instance: {{ .Release.Name }}
{{- end }}

# Usar en templates
labels:
  {{- include "mychart.labels" . | nindent 4 }}
```

### 2. Validaciones

```yaml
{{- if not .Values.image.repository }}
{{- fail "image.repository is required" }}
{{- end }}

{{- if and .Values.ingress.enabled (not .Values.ingress.hosts) }}
{{- fail "ingress.hosts is required when ingress is enabled" }}
{{- end }}
```

### 3. NOTES.txt

```yaml
# templates/NOTES.txt
Thank you for installing {{ .Chart.Name }}.

Your release is named {{ .Release.Name }}.

To learn more about the release, try:

  $ helm status {{ .Release.Name }}
  $ helm get all {{ .Release.Name }}

{{- if .Values.ingress.enabled }}
Application URL:
  http://{{ index .Values.ingress.hosts 0 "host" }}
{{- else }}
Get the application URL by running:
  export POD_NAME=$(kubectl get pods --namespace {{ .Release.Namespace }} -l "app.kubernetes.io/name={{ include "mychart.name" . }}" -o jsonpath="{.items[0].metadata.name}")
  kubectl port-forward $POD_NAME 8080:{{ .Values.service.port }}
{{- end }}
```

### 4. Helm test

```yaml
# templates/tests/test-connection.yaml
apiVersion: v1
kind: Pod
metadata:
  name: "{{ include "mychart.fullname" . }}-test-connection"
  annotations:
    "helm.sh/hook": test
spec:
  containers:
  - name: wget
    image: busybox
    command: ['wget']
    args: ['{{ include "mychart.fullname" . }}:{{ .Values.service.port }}']
  restartPolicy: Never
```

```bash
# Ejecutar tests
helm test myrelease
```

---

## 26. Troubleshooting

### Debugging

```bash
# Ver templates renderizados sin instalar
helm template myrelease mychart

# Dry run
helm install myrelease mychart --dry-run --debug

# Ver valores computados
helm get values myrelease

# Ver manifest instalado
helm get manifest myrelease

# Ver hooks
helm get hooks myrelease

# Historial de releases
helm history myrelease

# Rollback
helm rollback myrelease 1
```

### Comandos útiles

```bash
# Ver chart info
helm show chart bitnami/nginx

# Ver values por defecto
helm show values bitnami/nginx

# Ver README
helm show readme bitnami/nginx

# Ver todo
helm show all bitnami/nginx

# Descargar chart
helm pull bitnami/nginx --untar

# Verificar chart
helm lint mychart

# Empaquetar
helm package mychart

# Listar releases en todos los namespaces
helm list --all-namespaces

# Ver estado de release
helm status myrelease
```

---

## 🎯 Ejemplos Completos

### Aplicación web completa

```yaml
# Chart.yaml
apiVersion: v2
name: webapp
version: 1.0.0
appVersion: "1.0"
dependencies:
  - name: postgresql
    version: 12.x.x
    repository: https://charts.bitnami.com/bitnami

---
# values.yaml
replicaCount: 3

image:
  repository: mywebapp
  tag: "1.0"
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80

ingress:
  enabled: true
  className: nginx
  hosts:
    - host: webapp.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: webapp-tls
      hosts:
        - webapp.example.com

resources:
  limits:
    cpu: 500m
    memory: 512Mi
  requests:
    cpu: 250m
    memory: 256Mi

autoscaling:
  enabled: true
  minReplicas: 3
  maxReplicas: 10
  targetCPUUtilizationPercentage: 70

postgresql:
  auth:
    username: webapp
    password: secret
    database: webapp_db
  primary:
    persistence:
      size: 10Gi
```

---

## 🚀 Mejores Prácticas Generales

1. **Usa namespaces** para organizar recursos
2. **Implementa health checks** siempre
3. **Define resource limits** para todos los pods
4. **Usa ConfigMaps y Secrets** para configuración
5. **Implementa HPA** para escalado automático
6. **Usa Ingress** para exponer servicios
7. **Implementa RBAC** para seguridad
8. **Usa Helm** para gestionar despliegues complejos
9. **Monitorea** con Prometheus y Grafana
10. **Implementa CI/CD** para automatizar despliegues

---

**¡Domina Kubernetes y Helm para orquestar aplicaciones en contenedores! ☸️**
