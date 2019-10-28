## Install java-1.8.0-openjdk-devel
Install Java

```
 sudo yum install -y java-1.8.0-openjdk-devel
```
![](/images/install_4.png)

## Install the repo and key, and then install Jenkinskeyboard_arrow_up
Install wget

```
sudo yum install -y wget
```

![](/images/install_5.png)

Download the repo.

```
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo
```

Import the required key.
```
sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
```

Install Jenkins.
```
sudo yum install -y jenkins
```
![](/images/install_6.png)
![](/images/install_7.png)

```
sudo systemctl enable jenkins
sudo systemctl start jenkins

```

Once you install Jenkins, you will need the temporary admin password to complete setup in the browser. You can get the temporary admin password with this command:

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Slave set up
geneterate a key in master server

```
su jenkins -s /bin/bash
bash-4.2$ 
bash-4.2$ ssh-keygen

```

## Install Docker

1. sudo yum install -y yum-utils device-mapper-persistent-data lvm2
2. sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
3. sudo yum install docker-ce docker-ce-cli containerd.io
4. sudo systemctl start docker
5. sudo docker run hello-world
![](/images/install_1.png)
![](/images/install_2.png)
![](/images/install_3.png)

## Create a GitRepo (in your own account) that creates a basic webpage.

Steps:
1. Log in to github.com
2. Create a new repo (Create "teja190827" repo)
![](/images/1.png)
3. Checkout git repo
```
echo "<h1>Welcome to Teja's website!!</h1>" >> index.html
git init
git add index.html
git commit -m "first commit"
git remote add origin https://github.com/teja190827/teja190827.git
git push -u origin master
```
![](/images/2.png)

## Create a Docker file that exposes the basic webpage. 

```
echo "FROM nginx:alpine\nCOPY . /usr/share/nginx/html" >> Dockerfile
docker build -t webserver-image:v1 .
docker run -d -p 80:80 webserver-image:v1
curl docker
````
![](/images/3.png)
![](/images/4.png)

## Create a DockerHub account so you can publish a Docker Image to it. 
1. Log in on https://hub.docker.com/
2. Click on Create Repository.
Choose a name (e.g. webserver-image) and a description for your repository and click Create.
![](/images/5.png)

```
docker login --username=ravitejademolab
"docker images" to get the tag
docker tag 79a6c081b38b ravitejademolab/webserver-image:v1
docker push ravitejademolab/webserver-image:v1
```
![](/images/6.png)
![](/images/7.png)

### Create a Jenkinsfile in your GitHub repo (use your own account) with two Jenkinsfiles.

[Jenkinsfile to publish docker image](/Jenkinsfile)

Create a new pipeline in the Jenkins
![](/images/8.png)
![](/images/9.png)
![](/images/10.png)
![](/images/11.png)
![](/images/12.png)

##Use a Public Helm chart to deploy the Jenkins Chart onto the MiniKube instance on your environment.
Instead of mini kube we are using kubernetes cluster:

```
helm install  stable/jenkins
```


## Write a script in Bash that checks pods running in your MiniKube environment and prints out the Pod Name and Namespace.
```
#!/bin/bash
kubectl get pods -o wide
```
