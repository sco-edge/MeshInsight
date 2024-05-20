# Pod

junho@junho-System-Product-Name:~$ sudo kubectl run kuard --image=gcr.io/kuar-demo/kuard-amd64:blue
pod/kuard created

junho@junho-System-Product-Name:~$ sudo kubectl get pods
NAME READY STATUS RESTARTS AGE
kuard 1/1 Running 0 4m49s

junho@junho-System-Product-Name:~$ sudo kubectl delete pods/kuard
pod "kuard" deleted

-   kuard-pod.yaml
    apiVersion: v1
    kind: Pod
    metadata:
    name: kuard
    spec:
    containers: - image: gcr.io/kuar-demo/kuard-amd64:blue
    name: kuard
    ports: - containerPort: 8080
    name: http
    protocol: TCP

junho@junho-System-Product-Name:~$ sudo kubectl get pods
NAME READY STATUS RESTARTS AGE
kuard 1/1 Running 0 2m44s

junho@junho-System-Product-Name:~$ sudo kubectl logs kuard
2024/05/16 04:37:52 Starting kuard version: v0.10.0-blue
2024/05/16 04:37:52 **\*\***\*\*\*\***\*\***\*\***\*\***\*\*\*\***\*\***\*\***\*\***\*\*\*\***\*\***\*\***\*\***\*\*\*\***\*\***
2024/05/16 04:37:52 _ WARNING: This server may expose sensitive
2024/05/16 04:37:52 _ and secret information. Be careful.
2024/05/16 04:37:52 **\*\***\*\*\*\***\*\***\*\***\*\***\*\*\*\***\*\***\*\***\*\***\*\*\*\***\*\***\*\***\*\***\*\*\*\***\*\***
2024/05/16 04:37:52 Config:
{
"address": ":8080",
"debug": false,
"debug-sitedata-dir": "./sitedata",
"keygen": {
"enable": false,
"exit-code": 0,
"exit-on-complete": false,
"memq-queue": "",
"memq-server": "",
"num-to-gen": 0,
"time-to-run": 0
},
"liveness": {
"fail-next": 0
},
"readiness": {
"fail-next": 0
},
"tls-address": ":8443",
"tls-dir": "/tls"
}
2024/05/16 04:37:52 Could not find certificates to serve TLS
2024/05/16 04:37:52 Serving on HTTP on :8080

junho@junho-System-Product-Name:~$ sudo kubectl exec kuard -- date
Thu May 16 04:42:01 UTC 2024

junho@junho-System-Product-Name:~$ sudo kubectl exec -it kuard -- ash
~ $
~ $ exit

apiVersion: v1
kind: Pod
metadata:
name: kuard
spec:
containers: - image: gcr.io/kuar-demo/kuard-amd64:blue
name: kuard
livenessProbe:
httpGet:
path: /healthy
port: 8080
initialDelaySeconds: 5
timeoutSeconds: 1
periodSeconds: 10
failureThreshold: 3
ports: - containerPort: 8080
name: http
protocol: TCP

nano kuard-pod-resreq.yaml
apiVersion: v1
kind: Pod
metadata:
name: kuard
spec:
containers: - image: gcr.io/kuar-demo/kuard-amd64:blue
name: kuard
resources:
requests:
cpu: "500m"
memory: "128Mi"
ports: - containerPort: 8080
name: http
protocol: TCP

nano kuard-pod-reslim.yaml
apiVersion: v1
kind: Pod
metadata:
name: kuard
spec:
containers: - image: gcr.io/kuar-demo/kuard-amd64:blue
name: kuard
resources:
requests:
cpu: "500m"
memory: "128Mi"
limits:
cpu: "1000m"
memory: "256Mi"
ports: - containerPort: 8080
name: http
protocol: TCP

nano kuard-pod-vol.yaml
apiVersion: v1
kind: Pod
metadata:
name: kuard
spec:
volumes: - name: "kuard-data"
hostPath:
path: "/var/lib/kuard"
containers: - image: gcr.io/kuar-demo/kuard-amd64:blue
name: kuard
volumeMounts: - mountPath: "/data"
name: "kuard-data"
ports: - containerPort: 8080
name: http
protocol: TCP

nano kuard-pod-full.yaml
apiVersion: v1
kind: Pod
metadata:
name: kuard
spec:
volumes: - name: "kuard-data"
nfs:
server: my.nfs.server.local
path: "/exports"
containers: - image: gcr.io/kuar-demo/kuard-amd64:blue
name: kuard
ports: - containerPort: 8080
name: http
protocolL: TCP
resources:
requests:
cpu: "500m"
memory: "128Mi"
limits:
cpu: "1000m"
memory: "256Mi"
volumeMounts: - mountPath: "/data"
name: "kuard-data"
livenessProbe:
httpGet:
path: /healthy
port:8080
initialDelaySeconds: 5
timeoutSeconds: 1
periodSeconds: 10
failureThreshold: 3
readinessProbe:
httpGet:
path /ready
port:8080
initialDelaySeconds: 30
timeoutSeconds: 1
periodSeconds: 10
failureThreshold: 3

junho@junho-System-Product-Name:~$ sudo kubectl run alpaca-prod --image=gcr.io/kuar-demo/kuard-amd64:blue -- replicas=2 --labels="ver=1,app=alpaca,env-prod"
pod/alpaca-prod created

junho@junho-System-Product-Name:~$ sudo kubectl run alpaca-test --image=gcr.io/kuar-demo/kuard-amd64:green -- replicas=1 --labels="ver=2,app=alpaca,env=test"
pod/alpaca-test created

junho@junho-System-Product-Name:~$ sudo kubectl run bandicoot-prod --image=gcr.io/kuar-demo/kuard-amd64:green -- replicias=2 --labels="ver-2,app=bandicoot,env=prod"
pod/bandicoot-pord created

junho@junho-System-Product-Name:~$ sudo kubectl run bandicoot-staging --image=gcr.io/kuar-demo/kuard-amd64:green -- replicias=1 --labels="ver-2,app=bandicoot,env=staging"
pod/bandicoot-staging created

junho@junho-System-Product-Name:~$ sudo kubectl get pods --show-labels
NAME READY STATUS RESTARTS AGE LABELS
bandicoot-staging 0/1 CrashLoopBackOff 9 (4m21s ago) 25m run=bandicoot-staging
kuard 0/1 CrashLoopBackOff 6 (2m31s ago) 8m20s run=kuard
test 0/1 CrashLoopBackOff 12 (3m2s ago) 39m run=test

junho@junho-System-Product-Name:~$ sudo kubectl create deployment alpaca-prod --image=gcr.io/kuar-demo/kuard-amd64:blue --port=8080
deployment.apps/alpaca-prod created

junho@junho-System-Product-Name:~$ sudo kubectl scale deployment alpaca-prod --replicas 3
deployment.apps/alpaca-prod scaled

junho@junho-System-Product-Name:~$ sudo kubectl expose deployment alpaca-prod
service/alpaca-prod exposed

junho@junho-System-Product-Name:~$ sudo kubectl create deployment bandicoot-prod --image=gcr.io/kuar-demo/kuard-amd64:green --port=8080
deployment.apps/bandicoot-prod created

junho@junho-System-Product-Name:~$ sudo kubectl scale deployment bandicoot-prod --replicas 2
deployment.apps/bandicoot-prod scaled

junho@junho-System-Product-Name:~$ sudo kubectl expose deployment bandicoot-prod
service/bandicoot-prod exposed

junho@junho-System-Product-Name:~$ sudo kubectl get services -o wide
NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE SELECTOR
alpaca-prod ClusterIP 10.96.231.212 <none> 8080/TCP 7m11s app=alpaca-prod
bandicoot-prod ClusterIP 10.96.199.5 <none> 8080/TCP 96s app=bandicoot-prod
kubernetes ClusterIP 10.96.0.1 <none> 443/TCP 6d22h <none>

junho@junho-System-Product-Name:~$ sudo kubectl describe service alpaca-prod
Name: alpaca-prod
Namespace: default
Labels: app=alpaca-prod
Annotations: <none>
Selector: app=alpaca-prod
Type: NodePort
IP Family Policy: SingleStack
IP Families: IPv4
IP: 10.96.231.212
IPs: 10.96.231.212
Port: <unset> 8080/TCP
TargetPort: 8080/TCP
NodePort: <unset> 30945/TCP
Endpoints: 10.244.0.2:8080,10.244.0.3:8080,10.244.0.4:8080
Session Affinity: None
External Traffic Policy: Cluster
Events: <none>

junho@junho-System-Product-Name:~$ sudo kubectl get services
NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE
alpaca-prod LoadBalancer 10.96.231.212 <pending> 8080:30945/TCP 3h58m
bandicoot-prod ClusterIP 10.96.199.5 <none> 8080/TCP 3h52m
kubernetes ClusterIP 10.96.0.1 <none> 443/TCP 7d2h

junho@junho-System-Product-Name:~$ sudo kubectl describe endpoints alpaca-prod
Name: alpaca-prod
Namespace: default
Labels: app=alpaca-prod
Annotations: endpoints.kubernetes.io/last-change-trigger-time: 2024-05-17T01:45:25Z
Subsets:
Addresses: 10.244.0.2,10.244.0.3,10.244.0.4
NotReadyAddresses: <none>
Ports:
Name Port Protocol

---

<unset> 8080 TCP

Events: <none>

junho@junho-System-Product-Name:~$ sudo kubectl get endpoints alpaca-prod --watch
NAME ENDPOINTS AGE
alpaca-prod 10.244.0.2:8080,10.244.0.3:8080,10.244.0.4:8080 4h34m

junho@junho-System-Product-Name:~$ sudo kubectl get endpoints alpaca-prod --watch
NAME ENDPOINTS AGE
alpaca-prod 10.244.0.2:8080,10.244.0.3:8080,10.244.0.4:8080 4h34m
alpaca-prod 10.244.0.3:8080,10.244.0.4:8080 4h36m
alpaca-prod <none> 4h36m
alpaca-prod 10.244.0.15:8080 4h36m
alpaca-prod 10.244.0.15:8080,10.244.0.17:8080 4h37m
alpaca-prod 10.244.0.15:8080,10.244.0.16:8080,10.244.0.17:8080 4h37m

junho@junho-System-Product-Name:~$ sudo kubectl get pods -o wide --show-labels
NAME READY STATUS RESTARTS AGE IP NODE NOMINATED NODE READINESS GATES LABELS
alpaca-prod-7786f5894c-dbnfn 1/1 Running 0 4m24s 10.244.0.16 kind-control-plane <none> <none> app=alpaca-prod,pod-template-hash=7786f5894c
alpaca-prod-7786f5894c-ddrv8 1/1 Running 0 4m45s 10.244.0.15 kind-control-plane <none> <none> app=alpaca-prod,pod-template-hash=7786f5894c
alpaca-prod-7786f5894c-m5ltf 1/1 Running 0 4m24s 10.244.0.17 kind-control-plane <none> <none> app=alpaca-prod,pod-template-hash=7786f5894c
bandicoot-prod 0/1 CrashLoopBackOff 67 (16s ago) 4h57m 10.244.0.11 kind-control-plane <none> <none> run=bandicoot-prod
bandicoot-prod-79f7456fbc-9rnrd 1/1 Running 1 (98m ago) 4h39m 10.244.0.13 kind-control-plane <none> <none> app=bandicoot-prod,pod-template-hash=79f7456fbc
bandicoot-prod-79f7456fbc-pdtmf 1/1 Running 1 (98m ago) 4h37m 10.244.0.14 kind-control-plane <none> <none> app=bandicoot-prod,pod-template-hash=79f7456fbc
bandicoot-staging 0/1 CrashLoopBackOff 268 (16s ago) 22h 10.244.0.9 kind-control-plane <none> <none> run=bandicoot-staging
kuard 0/1 CrashLoopBackOff 264 (68s ago) 21h 10.244.0.12 kind-control-plane <none> <none> run=kuard
test 0/1 CrashLoopBackOff 270 (43s ago) 22h 10.244.0.10 kind-control-plane <none> <none> run=test

junho@junho-System-Product-Name:~$ sudo kubectl get -n projectcontour service envoy -o wide
NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE SELECTOR
envoy LoadBalancer 10.96.139.89 <pending> 80:30537/TCP,443:32329/TCP 3m57s app=envoy

junho@junho-System-Product-Name:~$ sudo kubectl get services -o wide
NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE SELECTOR
alpaca ClusterIP 10.96.18.15 <none> 8080/TCP 77s app=alpaca
alpaca-prod LoadBalancer 10.96.231.212 <pending> 8080:30945/TCP 8h app=alpaca-prod
bandicoot ClusterIP 10.96.172.29 <none> 8080/TCP 17s app=bandicoot
bandicoot-prod ClusterIP 10.96.199.5 <none> 8080/TCP 8h app=bandicoot-prod
be-default ClusterIP 10.96.77.24 <none> 8080/TCP 2m16s app=be-default
kubernetes ClusterIP 10.96.0.1 <none> 443/TCP 7d6h <none>

junho@junho-System-Product-Name:~$ sudo kubectl get ingress
NAME CLASS HOSTS ADDRESS PORTS AGE
simple-ingress <none> hello-world.example 80 2m41s

junho@junho-System-Product-Name:~$ sudo kubectl describe ingress simple-ingress
Name: simple-ingress
Labels: <none>
Namespace: default
Address:
Ingress Class: <none>
Default backend: <default>
Rules:
Host Path Backends

---

hello-world.example
/ alpaca:8080 (10.244.0.27:8080,10.244.0.28:8080,10.244.0.29:8080)
Annotations: nginx.ingress.kubernetes.io/rewrite-target: /$1
Events: <none>

junho@junho-System-Product-Name:~$ sudo kubectl apply -f host-ingress.yaml
ingress.networking.k8s.io/host-ingress created
