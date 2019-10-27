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

Create a Docker file that exposes the basic webpage. 
echo "FROM nginx:alpine\nCOPY . /usr/share/nginx/html" >> Dockerfile
docker build -t webserver-image:v1 .
docker run -d -p 80:80 webserver-image:v1
curl docker

Log in on https://hub.docker.com/
Click on Create Repository.
Choose a name (e.g. webserver-image) and a description for your repository and click Create.

docker login --username=ravitejademolab
"docker images" to get the tag
docker tag 79a6c081b38b ravitejademolab/webserver-image:v1
docker push ravitejademolab/webserver-image:v1


Create a Jenkinsfile in your GitHub repo (use your own account) with two Jenkinsfiles.


sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
sudo docker run hello-world


docker build -t webserver-image .
docker run -dit --rm --name webserver -p 80:80 webserver-image