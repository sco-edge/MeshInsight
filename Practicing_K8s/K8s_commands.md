### CREATE IMAGE named 'simple-node'

-   docker build -t simple-node

### CONNECT to 'http://localhost:3000' & ATTACH to running prgm

-   docker run --rm -p 3000:3000 simple-node

### RUN CONTAINER with docker named 'simple-node'

-   docker run -d --name simple-node

### STOP docker CONTAINER 'simple-node'

-   docker stop simple-node

### REMOVE docker CONTAINER 'simple-node'

-   docker rm 'simple-node'
-   docker rmi <태그이름>
-   docker rmi <이미지ID>

------------------LABEL------------------

### CHECK LABEL

-   kubectl label pod my-pod --show-labels

### ADD LABEL named 'app=backend'

-   kubectl label pod my-pod app=backend

### EDIT LABEL NAME 'app=backend' -> 'app=frontend'

-   kubectl label pod my-pod app=frontend --overwrite

### CHECK LABEL NAME CONTAINS 'app', 'env'

-   kubectl label pod/my-pod --label-columns app, env
-   kubectl get pod/my-pod -L app,env

### DELETE LABEL 'app'

-   kubectl label pod/my-pod app-

### Look Up pod

-   kubectl get pod

### Look Up types of pods

-   kubectl get pod,service

### Look Up all types

-   kubectl get all
