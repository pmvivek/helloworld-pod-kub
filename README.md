Hello World Service in GCP :

Step by step instruction to create a microservice image /container and deploy in GCP kubernetes engine.

Pre-requisite: 

Docker should exists in local machine
Docker hub user access.
GCP account access ( open a free account)

1. Create a simple hello world Rest Service using Spring Boot - Which just prints Hello world
2. Create a docker file 

	2.1 Below is a sample

		FROM openjdk:8-jdk-alpine
		ARG JAR_FILE=target/*.jar
		COPY ${JAR_FILE} app.jar
		ENTRYPOINT ["java","-jar","app.jar"]

3. Create a docker image & Container.
 
    3.1  Navigate to the Dockerfile location 
    3.2  Run the below command
        docker build -t <image_name>:version .
        (e.g.) docker build -t helloworld-app:latest .
    3.3  Ensure the image is created by running the below command
         docker images | grep helloworld
    3.4  Create a container in local maching to make sure it works

          docker run -d -p 8080:8080 -t helloword-app:latest
    3.5 Open a browser and access your app.
 
 4. pushing the image to Docker registry.

     4.1 Login to docker hub from terminal
          docker login -u <user_name>
     4.2  push the image to docker hub

          docker image push <username>/helloworld-app:latest
          e.g docker image push pmvivek/helloworld-app:latest

  5. Deploying in GCP

      5.1  Create a Kubernetes cluster.

           5.1.1. Login to https://console.cloud.google.com/
           5.1.2. Click on Kubernetes Engine -> Clusters ( Under Compute Menu)
           5.1.3. Click on create Cluster
           5.1.4  Provide a 'Name' and select a 'Zone', Click create.
           5.1.5  New cluster will get created.

      5.2. Pull the hello world image to GCP.
           Login to the GCP node
           docker login -u <user_name>
           docker pull pmvivek/helloworld-app:latest
      
      5.3 Create a pod 

           5.3.1. Create a Pod definition file, like the one below  ( Either you can download from Git to GCP or directly copy /paste the content to a file helloworld-pod.yml in GCP K8S machine)

           		https://github.com/pmvivek/helloworld-pod-kub/blob/master/helloworld-pod.yml

           5.3.2. Open GCP terminal , navigate to the folder where helloworld-pod.yml exists &  run the below command to create a pod.

           		kubectl apply -f helloworld-pod.yml

       5.4 Create Service definition ( this service provides external access to this app)

           5.4.1. Create a service definition file, like the one below

           		https://github.com/pmvivek/helloworld-pod-kub/blob/master/helloworld-service-def.yml

           5.4.2. In GCP terminal , navigate to the folder where helloworld-service-def.yml exists &  run the below command to create a service.

           		kubectl apply -f helloworld-service-def.yml
			
	with Public IP from GCP console, App should be accessible .
	
	Note: the above can also be done in different ways, such as implicit commands with fewer steps.
