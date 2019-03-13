# Install Jenkins for Swarm cluster in AWS

## configure

### `.env file`
* set `ebs_master` for the persistent volume
* set `time_zone`
* set `expose_port` to expose jenkins master to the user

## Setup
* install docker plugin [rexray/ebs](https://rexray.readthedocs.io/en/stable/user-guide/schedulers/docker/plug-ins/aws/#elastic-block-service) in all of worker nodes
* create network in swarm cluster
  * `docker network create -d overlay jenkins`
* run master.yml
  * `docker-compose -f master.yml config > deploy-master.yml`
  * `docker stack deploy -c deploy-master.yml jenkins-master`
* set ALB about exposed port
* copy `InitialAdminPassword` from master
* install default plugins you need
* install [Swarm plugin](https://wiki.jenkins.io/display/JENKINS/Swarm+Plugin) in master jenkins
* create account for agents
  * username, password
* set secrets in swarm cluster
  * `echo "your username" | docker create secrets jenkins-user -`
  * `echo "your password" | docker create secrets jenkins-pass -`
* run agent.ymn
  * `docker stack deploy -c agent.yml jenkins-agent`

## images
* master: jenkins/jenkins:lts
* swarm agent: neosapience/jenkins-swarm-agent, [Github](https://github.com/neosapience/jenkins-swarm-agent)
