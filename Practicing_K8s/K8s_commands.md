### create image named 'simple-node'

-   docker build -t simple-node

### connect to 'http://localhost:3000' & attach to running prgm

-   docker run --rm -p 3000:3000 simple-node

### run container with docker named 'simple-node'

-   docker run -d --name simple-node

### stop docker container 'simple-node'

-   docker stop simple-node

### remove docker container 'simple-node'

-   docker rm 'simple-node'
-   docker rmi <태그이름>
-   docker rmi <이미지ID>
