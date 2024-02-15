
# OMS Docker Image collection

* Create image: ```docker build -t {tag_name} ./```.
* Docker Images [Docker Images on hub.docker.com](https://hub.docker.com/r/orbitmedia/php/).
* SSL selfsigned certificate for php 7.x is set for ```*.orbit``` domains.   
* - 
* Selfsigned key may report in GitHub
* - 
* Build for multiple platforms and push to docker hub: ``` docker buildx build --platform linux/amd64,linux/arm64 -t villav/php:bitbucket-pipiline-php8 ./ --push ```
* [Official doc for multiplatform builds](https://docs.docker.com/build/building/multi-platform/)
* [Tutorial (buildx already installed with Docker app](https://www.smartling.com/resources/product/building-multi-architecture-docker-images-on-arm-64-bit-aws-graviton2/)
* -
* Bitbucket pipeline requires linux/amd64 - didn't work linux/arm64