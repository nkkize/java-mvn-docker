#java-mvn-docker
Learning - java 1.8, maven 3.3 docker image for building java applications in docker.
#Creating the image
```
$ sudo docker pull ubuntu
```
Create a container from the Ubuntu image:
```
$ sudo docker run -it --name javaimg ubuntu:latest
```
#Manually installing java in the container
On base machine download jdk (.tar.gz file) from Oracle (http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
Copy the downloaded file from base machine to docker container you just created. From the directory where file is downloaded run:
```
sudo tar -cv jdk-8u91-linux-x64.tar.gz | sudo docker exec -i javaimg tar x -C /java
```
Inside container go to /java and extract the jdk file
```
tar -xvzf jdk-8u91-linux-x64.tar.gz
```
Move the extracted contents to /usr/lib/jvm folder
```
sudo mv /extracted/path/to/jdk /usr/lib/jvm/oracle_jdk8
```
Add new jdk alternatives
```
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/oracle_jdk8/jre/bin/java 2000
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/oracle_jdk8/bin/javac 2000
sudo update-alternatives --config java
sudo update-alternatives --config javac
```
Create a file /etc/profile.d/oraclejdk.sh, and replace the contents from the file in this repository and set the paths by running
```
source /etc/profile.d/oraclejdk.sh
```

#Manually installing maven in the container
On base machine download apache maven binary (.tar.gz file) from Apache Maven official site (https://maven.apache.org/download.cgi)
Copy the downloaded file from base machine to the docker container. From the directory where file is downloaded run:
```
sudo tar -cv apache-maven-3.3.9-bin.tar.gz | sudo docker exec -i javaimg tar x -C /maven
```
Inside container, go to /maven and extract the file
```
tar -xvzf apache-maven-3.3.9-bin.tar.gz
```
Move the extracted contents to /usr/lib/maven folder
```
sudo mv /extracted/path/to/mvn/file /usr/lib/maven
```
Set the maven path
```
export PATH=/usr/lib/maven/bin:$PATH
```
Exit from the container and commit the changes in a new image
```
docker commit -m "maven and java added" -a "{author name}" {container_id} {new_image_name}:{tag}
```
You may push the image in docker hub
```
sudo docker push {your_dockerhub_account_name}/{new_image_name}:{tag}
```
