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
2024/05/16 04:37:52 ****************\*\*****************\*\*****************\*\*****************
2024/05/16 04:37:52 _ WARNING: This server may expose sensitive
2024/05/16 04:37:52 _ and secret information. Be careful.
2024/05/16 04:37:52 ****************\*\*****************\*\*****************\*\*****************
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
