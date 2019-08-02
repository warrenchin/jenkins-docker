# jenkins-docker
Docker image for jenkins with docker support    
##Note-  Chnage the line `docker-ce=18.06.1~ce~3-0~debian` depending on ther version of docker client you need to use.

Ref:
https://medium.com/@manav503/how-to-build-docker-images-inside-a-jenkins-container-d59944102f30

Use vagrant to automatically provision a vm closest to aws with included vagrantfile

Ref: https://www.vagrantup.com
```
vagrant up
vagrant ssh
```

Browse to http://localhost:8080

initial password is in 
/home/jenkins/jenkins_home/secrets/initialAdminPassword

or retrieve it from log using
```
docker logs jenkins
```

```
    ...
    default: Successfully tagged jenkins-docker:latest
    default: e18b47f12ea17cc84c63546d8a7d49e4ba6dc212e5095be7554cea2ff53585b7
    default: ------------------------------------------------------------------------------
    default: Retrieve initial Admin Password from jenkins_home/secrets/initialAdminPassword
    default: ------------------------------------------------------------------------------
jenkins-docker · master± ⟩ vagrant ssh

       __|  __|_  )
       _|  (     /   Amazon Linux AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-ami/2018.03-release-notes/
-bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
[vagrant@localhost ~]$ sudo su - jenkins
Last login: Fri Aug  2 21:28:55 UTC 2019
[jenkins@localhost ~]$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                              NAMES
e18b47f12ea1        jenkins-docker      "/sbin/tini -- /usr/…"   6 minutes ago       Up 6 minutes        0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   jenkins
[jenkins@localhost ~]$ sudo cat jenkins_home/secrets/initialAdminPassword
2d36ca2ee20843f7a525cdeeb20181e9
```

If using EC2 or not testing on vagrant

```
mkdir jenkins_home
chmod a+rwx jenkins_home
git clone https://github.com/warrenchin/jenkins-docker.git

docker image build -t jenkins-docker jenkins-docker

docker run -d --mount type=bind,src=/home/jenkins/jenkins_home,dst=/var/jenkins_home -p 8080:8080 -p 50000:50000 -v /var/run/docker.sock:/var/run/docker.sock --name jenkins jenkins-docker
```


Check docker available inside container
```
[jenkins@localhost ~]$ docker exec -it jenkins bash
root@52036767679f:/# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                              NAMES
52036767679f        jenkins-docker      "/sbin/tini -- /usr/…"   6 minutes ago       Up About a minute   0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   jenkins
```