# MeshInsight
Install 'MeshInsight' 

* 요구사항
  - Python (Version >= 3.7)
  - Kubernetes (Idealy version >=1.24
  - 
* 설치 이전 필요한 필수 프로그램들(requirements)
  -  numpy
  -  kubernetes
  -  docker
  -  scikit-learn
  -  pandas
  -  asydict
  -  pyyaml>=5.4.1
  -  Jinja2
  -  matplotlib
  -  rich

* 발생한 오류
-    The connection to the server localhost:8080 was refused - did you specify the right host or port?
=> Command 'docker' not found, but can be installed with:
sudo snap install docker         # version 24.0.5, or
sudo apt  install podman-docker  # version 3.4.4+ds1-1ubuntu1.22.04.2
sudo apt  install docker.io      # version 24.0.5-0ubuntu1~22.04.1
도커 미설치로 인한 문제(설치 후 정상작동)
