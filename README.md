# Readme
This repository contains a simple hello world application written in Go. The intention is to show different options of 
using base images. One of the great benefits of Go is that it statically links all dependencies (as long as no CGO is involved).
Therefore it is possible to use an empty base image (FROM scratch). This is done in the master branch. The other 
branches use other base images in order to show the difference in size and possible security issues the larger
images may entail.

_Base images used per branch:_
* master: scratch
* alpine: alpine:3.10
* ubuntu: ubuntu:18.04