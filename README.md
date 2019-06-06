# Kuberentes implementation of devops project

this is an extension of the project https://github.com/michealbmullin/devops-bobsproject , and uses the images and src files created in that project

below is the architecture of the image

[Diagram](https://github.com/michealbmullin/devops-project-kuberenetes/blob/master/archetecture%20diagram.png)

# Deployments

this directory contains all the processes required to run the kubberentes services of the project
the files are numbered to run in the correct order (due to dependancies)
the services can be started by run by using kubectl -f ./

note: enviroment variables are currently hard-coded , and the authentication link for activation required manaul changing once the ip of the load balancer is set.

# jenkins

folder contains the yaml files to run jenkins within the cluster that comes with a docker, docker-compose and the g-cloud sdk
also created a role group and applies it to the jenkins service.
note: current permissions are set to all but can be adjusted for security reasons.

# CI pipline

a simple diagram of the ci pipleine can be found below

[Diagram](https://github.com/michealbmullin/devops-project-kuberenetes/blob/master/CI-Pipeline.png)

the jenkins job has webhooks with the git repository form the earlier project. any pushed changes trigger a rebuild of the images, and then an updating of the images used for the running kubberentes services

sample job:
(auto-clone due to web hooks)
cd ci-project-dist
docker-compose build
docker-compose push
kubectl --record deployment.apps/authentication-client set image deployment.v1.apps/authentication-client authentication-client=docker.io/michealbmullin/projauthenticationcli:${BUILD-NUMBER}
etc
