keycloak
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: keycloak-deployment
  labels:
    app: keycloak
spec:
  replicas: 1
  selector:
    matchLabels:
      app: keycloak
  template:
    metadata:
      labels:
        app: keycloak
    spec:
      containers:
      - name: keycloak
        image: quay.io/keycloak/keycloak:legacy
        ports:
        - containerPort: 8080
        env:
          - name: DB_VENDOR
            value: "POSTGRES"
          - name: DB_ADDR
            value: "postgres-service"
          - name: DB_DATABASE
            value: {{ .Values.postgres.db }}
          - name: DB_USER
            value: {{ .Values.postgres.user }}
          - name: DB_SCHEMA
            value: "public"
          - name: DB_PASSWORD
            value: {{ .Values.postgres.password }}
          - name: KEYCLOAK_USER
            value: {{ .Values.keycloak.credential.user }}
          - name: KEYCLOAK_PASSWORD
            value: {{ .Values.keycloak.credential.password }}
---
apiVersion: v1
kind: Service
metadata:
  name: keycloak-service
spec:
  type: ClusterIP
  selector:
    app: keycloak
  ports:
    - port: 8080
      targetPort: 8080
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: keycloak-ingress
  # annotations:
  #   nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: {{ .Values.ingress.host }}
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: keycloak-service
            port:
              number: 8080
```

postgres
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres-deployment
  labels:
    app: postgres
spec:
  replicas: 1
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
      - name: postgres
        image: postgres
        ports:
        - containerPort: 5432
        volumeMounts:
          - name: postgres-data
            mountPath: /var/lib/postgresql/data
        env:
          - name: POSTGRES_DB
            value: {{ .Values.postgres.db }}
          - name: POSTGRES_USER
            value: {{ .Values.postgres.user }}
          - name: POSTGRES_PASSWORD
            value: {{ .Values.postgres.password }}
      volumes:
        - name: postgres-data
          persistentVolumeClaim:
            claimName: hostpath-pvc
       
---
apiVersion: v1
kind: Service
metadata:
  name: postgres-service
spec:
  type: ClusterIP
  selector:
    app: postgres
  ports:
    - port: 5432
      targetPort: 5432
  
```

values
```
keycloak:
  credential:
    user: admin
    password: gfkgvkgft
postgres:
  db: keycloak
  user: keycloak
  password: testttttt
ingress:
  host: tumjai.com
```