# How to deploy Java applications using Docker

This is a simple Maven "Hello World" Java application that uses the Spotify plugin to build and deploy a Docker container with the settings in a separate docker file under the src/main/docker folder.


## Before you start

You need to have Docker installed AND running. If its not running, when the plugin tries to find the commands on the classpath it won't and it doesn't give you a nice error to tell you that!

## Also before you start

You also need a Docker Hub account. Its free and Google knows how to do it.

## Configuration, before you start

You then need to take the credentials that you created your Docker accout with and set up the configuration for Maven. Its the settings.xml under the config directory of your Maven installation. Add a setting thusly:-

<servers>
	<server>
		<id>docker-hub</id>
		<username>xxx</username>
		<password>xxx</password>
		<configuration>
			<email>
				xxx
			</email>
		</configuration>
	</server>
</servers>

## Now you can do it

mvn clean install will build the application image 
to push it to the Docker Hub using Maven issue a clean package docker:build -DpushImage
docker:build makes the image and pushImage as a property to this, follows that up with a push to Docker Hub. Nice.

## Pull it and run it in Docker

log into Docker on your target (probably AWS) and you can run it by SSHing to your EC2 public IP

* ssh -i my-ec2-key-pair.pem ec2-user@52.56.116.180

and issuing the following command which will go to Docker Hub and pull the latest image for the repository:

* [ec2-user@ip-172-31-2-175 ~]$ docker run -d -p 80:8080 petecknight/docker-app-in-docker-file

There's some stuff during the pull and then you'll get:

* Status: Downloaded newer image for petecknight/docker-app-in-docker-file:latest

## Look it up

Call your public IP of your EC2 container with the port you've bound the tomcat fired up on 8080 in the container 
(in the above example its 80 which is the default, so you don't need to add it in)

* http://52.56.116.180


## Finished!