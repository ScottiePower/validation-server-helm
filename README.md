# Project: validation-server
Deploy simple spring boot application that has health probes enabled

# Docker commands
You can manually test the application being deployed by running the docker image.
- docker run --publish 80:80 zimmy71/validation-server:20232712.043825

# Get readiness state 
- curl --location 'localhost:80/actuator/health/readiness' --header 'Accept: application/json'
- curl --location 'localhost:80/spring/test' --header 'Accept: application/json' --header 'Content-Type: application/json'

# Post data to validation endpoint
- curl --location 'localhost:80/spring/validate' --header 'Accept: application/json' --header 'Content-Type: application/json' --data '{"rego": "some rego"}'

# Helm commands
- with in project root directory run: helm install validation-server-helm .
- Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services validation-server-helm-mainapp)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
- Use the ip address port in the curl commands.
  curl --location $NODE_IP:$NODE_PORT/actuator/health/readiness --header 'Accept: application/json'
  curl --location $NODE_IP:$NODE_PORT/spring/validate --header 'Accept: application/json' --header 'Content-Type: application/json' --data '{"rego": "some rego"}'