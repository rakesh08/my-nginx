kubectl apply -f- <<'EOF'
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-config
data:
  default.conf.template: |
    server {
      listen ${NGINX_PORT};
      location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
      }
    }
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    ports:
    - containerPort: 8000
    env:
    - name: NGINX_PORT
      value: "8000"
    volumeMounts:
    - name: config
      mountPath: /etc/nginx/templates
    readinessProbe:
      httpGet:
        path: /fail
        port: 8000
      periodSeconds: 3
  - image: nginx
    name: nginx2
    ports:
    - containerPort: 8001
    env:
    - name: NGINX_PORT
      value: "8001"
    volumeMounts:
    - name: config
      mountPath: /etc/nginx/templates
    readinessProbe:
      httpGet:
        path: /
        port: 8001
      periodSeconds: 3
  volumes:
  - name: config
    configMap:
      name: nginx-config
EOF
