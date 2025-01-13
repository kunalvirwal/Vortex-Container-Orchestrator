# Vortex  

> Vortex is a CLI-driven, event based, container orchestrator built to handle the storm of dynamic workloads.

It handles the job of maintaining related services in the form of deployments, where each service can have multiple instances of the same container running. Vortex takes care of starting and maintaining these containers, replacing them if they terminate and overall maintaining the provided state of a deployment applied. This project is inspired by Kubelet which is a crucial component in a Kubernetes cluster.

## Features and Project Goals  

✅: Implemented  
🚧: Under active development  
⚒️: Under consideration  

### Phase I: MVP
✅ It should use the docker sdk : Done  
✅ It should be able to create & delete containers  : Done  
✅ It should be able to track if a container is deleted : Done  
⚒️ It should maintain its own data in an mongo container (we'll see)  
✅ It should detect if its containers are killed and restart them : Done   
✅ It should run schedulers to have CrashBackOff : Done  
✅ It should have a healthcheck to see if the service is running or not : Done  
  
  
### Phase II: Advanced Features   
✅ It must have gRPC connections within cli and main service for healthchecks and responses : Done  
✅ It should detect container Crash Loop and Implement Exponential CrashLoopBackOff : Done  
✅ It should have Exponential backoff reset : Done  
🚧 It should be dynamically modifiable  
⚒️ It should detect container health and restart if possible  : gRPC route added 

### Phase III: Prod level Features   
⚒️ It must integrate with Velocity Load Balancer  
🚧 It can implement microservices architecture to run on multiple nodes  
🚧 It should have an install script for global install of the commands  
  
## Command Line Interface 

Note: To use the commands globally, shift the `vortex` and `vortex-service` binaries after building to a directory which is present in your `PATH` environment variable like `/usr/local/bin`. If you want to skip this step then you can cd to the project directory and use `./vortex` in place of vortex in all the commands.  


- `vortex up`:  

  To start the vortex-service   
  
- `vortex apply deployment -f ./<file.yaml>`:   

  To Create a deployment. One deployment file must contain configuration of only one deployment. Each deployment can contain many services. The name of a deployment must be unique and the name of a service must be unique in its deployment. For more information look at the demo deployment.yml file provoided.

- `vortex show`:  
  
  To see the current state of all the services and their containers running on a machine.  

- `vortex delete -d <deployment-name>`:  
  
  To delete a deployment and all its services.

- `vortex delete -d <deployment-name> -s <service-name>`:  
  
  To delete a particular service in a deployment and thus stop all its containers.

- `vortex crashlog -d <deployment-name> -s <service-name> -u <ServiceUID>`:  
  
  To see the crashlog of any particular container of any service. ServiceUID is a whole number that identifies each replica container of a service. If a container dies, its successor  will carry the same ServiceUID. The ServiceUID of all containers can be viewed using `vortex show` command.

- `vortex down`:    

  Stops the running instance of vortex-service, any running containers will not be deleted. All running comntainers can be eiter deleted using the `vortex delete` command if vortex-service has not been stopped yet or manually if the then running instance of vortex-service was stopped.

## User's and Developer's Guide
Steps to setup the project

- Clone the project  

  `$ git clone https://github.com/kunalvirwal/Vortex-Container-Orchestrator.git`  

- Build the project using the provided makefile  

  `$ make build`  

- Now you can start Vortex using `vortex start` and experiment with the example deployment provided.  

- To have a deeper hold on vortex-service and its logs you can manually start its binary using `./vortex-service` in a separate terminal and running cli commands in a separate terminal will intract with the earlier instance (in that case you don't have to use `vortex up`).   

- To add any new gRPC route, update the protofile in `./proto/factory.proto` and update its autogenerated gRPC files using 

  `$ protoc --go_out=./proto/ --go-grpc_out=./proto/ ./proto/factory.proto`
