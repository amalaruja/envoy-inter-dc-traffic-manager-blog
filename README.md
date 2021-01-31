# Envoy as an Inter DC Traffic Manager [Blog Resource]

This repo contains resources that can be used to setup a mock environment for testing Envoy as an inter Data Center traffic manager. It has links to docker images of dummy apps that are used in the setup. It also provides kubernetes resources used for deploying these apps along with envoy to mock 2 differnt DCs, in a k8s cluster.

## Design

![Mock Environment](https://raw.githubusercontent.com/amalaruja/envoy-inter-dc-traffic-manager-blog/main/assets/mock-environment.jpg)

## External Resources

[Dummy REST App](https://github.com/amalaruja/go-hello-rest) <br/>
[Dummy gRPC App](https://github.com/amalaruja/go-hello-grpc) <br/>
[REST App Docker Image](https://hub.docker.com/repository/docker/kalip/hello-rest) <br/>
[gRPC App Docker Image](https://hub.docker.com/repository/docker/kalip/hello-grpc) <br/>

## Prerequisites

1. Local k8s cluster ([Docker Desktop](https://www.docker.com/products/docker-desktop) or [Minikube](https://minikube.sigs.k8s.io/docs/start/)), where the mock environment will be setup
2. [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) to create, edit and retrieve k8s resources
3. [cURL](https://curl.haxx.se/download.html) or [Postman](https://www.postman.com/) for hitting the REST endpoint
4. [grpcurl](https://github.com/fullstorydev/grpcurl) or [BloomRPC](https://github.com/uw-labs/bloomrpc) for hitting the gRPC service

## Setup

```console
foo@bar:~$ git clone https://github.com/amalaruja/envoy-inter-dc-traffic-manager-blog.git
foo@bar:~$ kubectl apply -f ./k8s
```

## Test

### DC 1
```console
foo@bar:~$ curl localhost:30081/hello
Hello from DC 1!
foo@bar:~$ grpcurl --plaintext --import-path ./proto/hello -proto hello.proto localhost:30081 HelloGrpcService/Hello
{
  "reply": "Hello from DC 1!"
}
```
### DC 2
```console
foo@bar:~$ curl localhost:30082/hello
Hello from DC 2!
foo@bar:~$ grpcurl --plaintext --import-path ./proto/hello -proto hello.proto localhost:30082 HelloGrpcService/Hello
{
  "reply": "Hello from DC 2!"
}
```