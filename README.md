# java-mvn-docker
java 1.8, maven 3.3 docker image for building java applications in docker.
#Creating the image
```
$ sudo docker pull ubuntu
Create a container from the Ubuntu image:
$ sudo docker run -it --name javaimg ubuntu:latest
```
#Manually installing java in the container
```
On base machine download jdk (.tar.gz file) from Oracle (http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
Copy the downloaded file from host to docker container. From the directory where file is downloaded run:
sudo tar -cv jdk-8u91-linux-x64.tar.gz | sudo docker exec -i javaimg tar x -C /java
Inside container, extract the jdk file
tar -xvzf jdk-8u91-linux-x64.tar.gz
Move the extracted contents to /usr/lib/jvm folder
sudo mv /extracted/path/to/jdk /usr/lib/jvm/oracle_jdk8
Add new jdk alternatives
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/oracle_jdk8/jre/bin/java 2000
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/oracle_jdk8/bin/javac 2000
sudo update-alternatives --config java
sudo update-alternatives --config javac
Create a file /etc/profile.d/oraclejdk.sh, and replace the contents from the file in this repository
Set the path by running command: source /etc/profile.d/oraclejdk.sh
```
#Manually installing maven in the container
```
On base machine download maven binary (.tar.gz file) from Apache (https://maven.apache.org/download.cgi)
Copy the downloaded file from host to docker container. From the directory where file is downloaded run:
sudo tar -cv apache-maven-3.3.9-bin.tar.gz | sudo docker exec -i javaimg tar x -C /java
Inside container, extract the file
tar -xvzf apache-maven-3.3.9-bin.tar.gz
Move the extracted contents to /usr/lib/mvn folder
sudo mv /extracted/path/to/mvn/file /usr/lib/mvn
Set the maven path
export PATH=/usr/lib/mvn/bin:$PATH
```
Exit from the container and commit
docker commit -m "maven and java added" -a "{author name}" {container_id} {image_name}:{tag}
