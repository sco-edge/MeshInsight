# create image named 'simple-node'

-   docker build -t simple-node

# connect to 'http://localhost:3000' & attach to running prgm

-   docker run --rm -p 3000:3000 simple-node
