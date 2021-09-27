# Deploy Springboot Application to OKE Cluster

## Description

The purpose of this guide is to describe the steps necessary to configure and deploy the springboot application on an OKE cluster.

> **NOTE**: The guide assumes the user has at least basic knowledge and understanding of **OCI**, **OKE**, **Docker**, **Authorization Token**, **OCID** ,**Service Limits**, **Compartments** and is familiar with the tools. 
> **NOTE**: This guide uses the Windows commands, please be aware if you are using a MacOS or Linux operating system.


## Requirements

* For Container Registry, Kubernetes and Load Balancers:
  * A paid Oracle Cloud Infrastructure account.
  * See Signing Up for Oracle Cloud Infrastructure.
* For building applications and Docker images:
   * One of the following local environments:
     * A MacOS or Linux machine.
     * A Windows machine with Linux support. For example:
       * Windows Subsystem for Linux
       * Oracle Virtual Box
   * The following applications on your local environment:
     * JDK 11 and set JAVA_HOME in .bashrc.
     * Python 3.6.8+ and pip installer for Python 3
     * Kubernetes Client 1.11.9+
     * Apache Maven 3.0+
     * Docker 19.0.3+
     * Git 1.8+
     
     
## 1. Prepare

#### 1. Check your service limits
>**NOTE:** Not covered in this guide.

#### 2. Create an Authorization Token
>**NOTE:** Not covered in this guide.

#### 3. Gather Required Information
Like Object Storage Namespace, Tenancy OCID, Username, User OCID, Region, Region Key, Auth Token

#### 4. Setup OCI Command Line Interface
>**NOTE:** Not covered in this guide.

## 2. Set Up a Cluster

#### 1. Add Compartment policy

If your user is in Administrator's group , you may skip this step

```` bash
allow group <the-group-your-username-belongs> to manage compartments in tenancy
````
#### 2. Create a Compartment

>**NOTE:** Not covered in this guide.

#### 3. Add Resource Policy

If your user is in Administrator's group , you may skip this step

```` bash
allow group <the-group-your-username-belongs> to manage all-resources in compartment <your-compartment-name>
````

#### 4. Create a OKE Cluster with 'Quick Create'

1. Sign in to the Oracle Cloud Infrastructure Console.
2. Open the navigation menu and click Developer Services. Under Containers & Artifacts, click Kubernetes Clusters (OKE).
3. Click Create Cluster.
4. Select Quick Create.
5. Click Launch Workflow.
   The Create Cluster dialog is displayed.

6. Fill in the following information.
  * Name: 'your-cluster-name'
  * Compartment: 'your-compartment-name'
  * Kubernetes Version: 'take-default'
  * Kubernetes API Endpoint: Public Endpoint
    The Kubernetes cluster is hosted in a public subnet with an auto-assigned public IP address.

 * Kubernetes Worker Nodes: Private Workers
   The Kubernetes worker nodes are hosted in a private subnet.

 * Shape: VM.Standard.E3.Flex
 * Number of Nodes: 3
 * Specify a custom boot volume size: Clear the check box.
7. Click Next.
   All your choices are displayed. Review them to ensure that everything is configured correctly.

8. Click Create Cluster.
   The services set up for your cluster are displayed.

9. Click Close.


#### 5. Set Up Local Access to CLuster

* Create .kube directory and run the commands given under "Access Cluster" Local Access tab

![image](https://user-images.githubusercontent.com/57708209/134658515-fd106e70-322c-4d2c-a457-f9867445502f.png)

![image](https://user-images.githubusercontent.com/57708209/134658649-8e4570fe-7d86-4606-8e90-aee70f020577.png)

* Test your cluster configuration with the following commands

```` bash
kubectl describe deployment
````

```` bash
kubectl get pods
````

Since no application is deployed, the last two commands produce: "No resources found in default namespace."

  
## 3. Build a Local Application
 
#### 1. Create a Local Application
   
  1. Make a copy of the Spring Boot Docker guide with Git:
   ```bash
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```
   ![image](https://user-images.githubusercontent.com/57708209/134642325-161b4825-1746-4792-b632-1566e9889dad.png)
  
  2. Change into the gs-spring-boot-docker/initial directory
     ```` bash
     cd gs-spring-boot-docker/initial
     ````
  
  3. Change into the Java source directory: src/main/java/hello
     ````bash
     cd src/main/java/hello
     ````
  4. Update Application.java with the following code
     ```
     package hello;
     import org.springframework.boot.SpringApplication;
     import org.springframework.boot.autoconfigure.SpringBootApplication;
     import org.springframework.web.bind.annotation.RequestMapping;
     import org.springframework.web.bind.annotation.RestController;

     @SpringBootApplication
     @RestController
     public class Application {
	
	    @RequestMapping("/")
	    public String home() {
		   return "<h1>Spring Boot Hello World!</h1>";
	    }

	    public static void main(String[] args) {
		   SpringApplication.run(Application.class, args);
	    }

     }
     ```
                        
  5. Save the file.

   
#### 2. Run the Local Application
 
 1. Change into the gs-spring-boot-docker/initial directory
 
 2. Package the app:
 ````bash
 mvn package
 ````
 ![image](https://user-images.githubusercontent.com/57708209/134659301-3b776be8-dea0-4073-921a-e88909959f71.png)

 3. Run the app:
 ````bash
 java -jar target/spring-boot-docker-0.0.1-SNAPSHOT.jar
 ````
 
 ![image](https://user-images.githubusercontent.com/57708209/134659381-a80722c0-cb2b-4e26-ab90-867ad6f394c5.png)


 4. Keep the code running and test the app in one of the following ways:
    * In a new terminal connected to your instance, enter the following code:
      ```bash
      curl -X GET http://localhost:8080
      ````
    * Load the following address in a browser:
      ````bash
      localhost:8080
      ````
      ![image](https://user-images.githubusercontent.com/57708209/134659523-d96b08ad-a72c-4386-bcf5-4a082dbe0e4c.png)

 
 5. Stop the running application. Example: Ctrl + C



#### 3. Build a Docker Image
 
 1. Change into the gs-spring-boot-docker/initial directory.
 
 2. Create a file named Dockerfile.
 ````
FROM openjdk:8-jdk
RUN addgroup --system spring && adduser --system spring -ingroup spring
USER spring:spring
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
````

 3. Build the Docker image:
 
 Make sure docker Desktop is running before you build the image
 ````bash
 docker build -t spring-boot-hello .
 ````
 
 ![image](https://user-images.githubusercontent.com/57708209/134659855-eaf02b85-904e-4243-8fc2-8616994c01df.png)
 
 4. Check the docker image
 ````bash
 docker images
 ````
 ![image](https://user-images.githubusercontent.com/57708209/134660146-e1015645-3c14-46f8-bbb7-b3ba7c8df554.png)

 5. Run the Docker image:
 ````bash
 docker run -p 8080:8080 -t spring-boot-hello
 ````
 
 ![image](https://user-images.githubusercontent.com/57708209/134660249-7aed25b2-6dd3-4f56-8d4c-627dd004f4f0.png)
 
 6. Load the following address in a browser :
 ````bash
 localhost:8080
 ````
 
 ![image](https://user-images.githubusercontent.com/57708209/134660363-f617f0ab-d171-4c18-8aef-3a87f1cfaedf.png)


 7. In a new terminal ,  run the below command to see container created
 
 ````bash
docker ps
````
![image](https://user-images.githubusercontent.com/57708209/134660622-a4bf79b4-1d91-4866-bafd-a75bf880ccf9.png)

8. Stop the running application.
 ````bash
 docker stop <container_id>
 ````
  
## 4. Deploy Your Docker Image

#### 1. Create a Docker Repository
     
 1. Create a repo with the your choice of name with 'repo-name' = 'repo-prefix'/'image-name'

    ![image](https://user-images.githubusercontent.com/57708209/134667116-282c9751-653b-45ba-886b-7b1e30d461ed.png)


 
#### 2. Push your Local Image
 
 1. Continue working on the new terminal window.
 2. Log in to OCI Container Registry:
    ````bash
    docker login <region-key>.ocir.io
    ````
    You are prompted for your login name and password.

	* Username: 'tenancy-namespace'/'user-name'
	* Password: 'auth-token'

  ![image](https://user-images.githubusercontent.com/57708209/134666183-39ac89a3-51f0-406e-a813-b7ce02cecb2a.png)

 3. Tag your local image with the URL for the registry plus the repo name, so you can push it to that repo.
   ````bash
    docker tag <your-local-image> <repo-url>/<repo-name>
   ````
   
        *  repo-url ='region-key'.ocir.io/'tenancy-namespace'
        *  repo-name ='repo-prefix'/'image-name' from step 1 of previous section
	   
  ![image](https://user-images.githubusercontent.com/57708209/134668039-74746ab8-0755-42fe-9ed2-2aabaad7e038.png)


 4. Check your Docker images to see if the image is tagged.
 
    ````bash
    docker images
    ````
   
  ![image](https://user-images.githubusercontent.com/57708209/134666728-a54dbd5f-3c66-4dc3-9833-bad734e772b1.png)
	   
 5. Push the image to Container Registry
    
    ````bash
    docker push <tagged-image-name>:latest
    ````
    
  ![image](https://user-images.githubusercontent.com/57708209/134666764-6f71f3df-dcb8-4d34-8b35-0756025cbb0c.png)

 6. Open the navigation menu and click Developer Services. Under Containers & Artifacts, click Container Registry.
 7. Find your image in Container Registry after the push command is complete.
	   ![image](https://user-images.githubusercontent.com/57708209/134666915-b234219c-0396-4a51-82b0-ff8fbe8088cd.png)



#### 3. Deploy the Image
 
  1. Create a registry secret for your application. This secret authenticates your image when you deploy it to your cluster.
     To create your secret, fill in the information in this template .
     ````bash
     kubectl create secret docker-registry ocirsecret --docker-server=<region-key>.ocir.io  --docker-username='<tenancy-namespace>/<user-name>' --docker-password='<auth-token>'  --docker-email='<email-address>'
     ````
     
     ![image](https://user-images.githubusercontent.com/57708209/134668286-4c305e29-5b5e-45fc-ba37-10ce020a65ea.png)

  2. Verify that the secret is created. Issue the following command:
     ````bash
     kubectl get secret ocirsecret --output=yaml
     ````
  3. Determine the host URL to your registry image using the following template:
     ````bash
     <region-code>.ocir.io/<tenancy-namespace>/<repo-prefix>/<image-name>:<tag>
     ````
  6. On your system, create a file called deployment.yaml with the following text:
     Replace the following place holders:
       * your-image-url as per format given in last step
       * your-secret-name as per used in step2

	
     
  8. Deploy your application with the following command.
     ````bash
     kubectl create -f sb-app.yaml
     ````
     
     ![image](https://user-images.githubusercontent.com/57708209/134669109-ad15ffa8-c6f3-470e-82d1-9e14df2fd5fc.png)





#### 4. Test your App
  1. Check if the load balancer is live:
  
     ````bash
     kubectl get service
     ````
     
     ![image](https://user-images.githubusercontent.com/57708209/134669354-daa0e39a-66e5-4712-93d6-001e44bf771c.png)
     
     
     ![image](https://user-images.githubusercontent.com/57708209/134669454-e1e79a15-3a63-4417-8091-d01b5bb2bb48.png)


  2. Use the load balancer IP address to connect to your app in a browser:
  
   ````bash
   http://<load-balancer-IP-address>:8080
   ````
    
    
  ![image](https://user-images.githubusercontent.com/57708209/134669466-229f98d6-f0ab-4ed1-a40a-db4ff779bc1b.png)

  3. (Optional) To undeploy your application from the cluster, run the following command:
  
  
  ````bash
  kubectl delete -f sb-app.yaml
  ````
    
 ![image](https://user-images.githubusercontent.com/57708209/134669499-bb78fe6d-0a6f-4cb0-972e-9ee5cf3c582a.png)





  




     








