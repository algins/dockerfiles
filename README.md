# dockerfiles
Collection of my personally made Dockerfiles

### Requirements:
* Git
* Docker

### Setup:
```sh
$ git clone https://github.com/algins/dockerfiles.git && cd dockerfiles
$ cd php8.2.0-fpm-oci-nodejs19 # Navigate to directory where Dockerfile is located
$ docker build -t php8.2.0-fpm-oci-nodejs19 . # Build an image from a Dockerfile
```

### Usage:
```sh
$ docker run -d php8.2.0-fpm-oci-nodejs19 # Create and run a new container from an image
```