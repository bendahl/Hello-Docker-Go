# A simple hello world in Docker
This repository contains a simple hello world application written in Go. The intention of this project is to showcase 
basic aspects of Docker. These aspects are:

- The impact of base images on size and security
- The power and simplicity of multistage builds

In order to keep things as simple as possible, there are three branches, one for each base image used. Below you will find
an overview of the branches and the base images used there. Besides the base image, all branches share the same basic
structure. This approach has two major advantages:
 
1. The build instructions given below apply to all three branches
2. In order to try out a different base image, all you need to do is a `git checkout <branchname>`

_Base images used per branch:_
* master: scratch
* alpine: alpine:3.10
* ubuntu: ubuntu:18.04

## Prerequisites
- Mandatory
    - [Docker](https://docs.docker.com/v17.12/install/)
- Optional
    - [Go 1.13+](https://go.dev/)


## How to build the image
**TL;DR**

__If you do have Go installed and you want to build the hello world example yourself, continue reading. Otherwise either 
download the [released version from GitHub](https://github.com/bendahl/Hello-Docker-Go/releases/download/v2.0/hello_docker)
and skip to [step 2](#step-2-build-the-image) or use the multistage build, as explained in 
[the multistage section](#how-to-use-multistage-builds-in-docker).__

### Step 1: Build the binary
Assuming that you do have Go 1.13 or later installed on your linux box, you can simply use `go build` in order to build
the hello world example. The resulting binary will be called "hello_docker" and is referenced in both Dockerfiles. 

### Step 2: Build the image
Once you do have the "hello_docker" binary in your current working directory, all it takes to build the actual image 
is a simple `docker build -t hello-docker:<insert-tag> .` (don't forget the "." at the end). You should now have an 
image containing the hello_docker binary (you can verify that the image exists in your local repo using `docker images`). 
That's it! Done! Great job! 

## How to use multistage builds in Docker
Multistage builds are a great feature provided by Docker. It allows us to use an intermediate container to perform the
actual build and use the resulting artifact(s) in the final image. To find out more about the nitty gritty of multistage
builds, have a look at the [excellent documentation provided by Docker](https://docs.docker.com/develop/develop-images/multistage-build/).

In order to build the example provided here, enter `docker build -f Dockerfile_multistage -t hello-docker:1.0 .` in order
to build the image using a multistage build.


## Creating and running a container based on your image
Given that you've successfully created an image following the above instructions, a plain `docker run hello-docker:<your-tag>` 
will suffice. The expected result is a mind blowing "Hello World!" on the console. Congratulations! You just built and ran
an application using Docker.

## Inspecting the images
If you go the extra mile and build all three example images, you will be rewarded with the ability to compare the results.
Try `docker history hello-docker:<whatever-version-you-want-to-checkout>` and take a closer look. What you'll see are the
actual layers along with their respective sizes. You will note huge differences, especially between the basic "from scratch"
no frills image and the "all bells and whistles included" Ubuntu version. 

## A few words regarding base images in general
In conclusion, you should always pay attention to the base image you're using. Not just to minimize image size, but especially
to reduce the attack surface. In the end, the more stuff is in your image, the more stuff may be broken. However, to be fair,
keep in mind that "FROM scratch" is not always an option, since most applications will have certain runtime dependencies. 
In these cases Alpine based images may be an option, as they are usually pretty lightweight and secure. Due to the fact that 
Alpine makes certain design choices in order to be that lightweight, it may not fit the bill for you. So Ubuntu, 
Debian, or the like may be a good base image option for your use case after all. As always - it depends...
