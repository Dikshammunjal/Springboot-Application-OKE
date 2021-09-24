# Deploy Springboot Application to OKE Cluster

## Description

The purpose of this guide is to describe the steps necessary to configure and deploy the springboot application on an OKE cluster.

> **NOTE**: The guide assumes the user has at least basic knowledge and understanding of **OCI**, **OKE**, **Docker**, **Authorization Token**, **OCID** ,**Service Limits** and is familiar with the tools. 



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
allow group <the-group-your-username-belongs> to manage compartments in tenancy
#### 2. Create a Compartment
#### 3. Add Resource Policy
#### 4. Create a OKE Cluster with 'Quick Create'
#### 5. Set Up Local Access to CLuster
  
## 3. Build a Local Application
 
#### 1. Create a Local Application
   
  1. Make a copy of the Spring Boot Docker guide with Git:
   ```bash
   $ git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```
   ![image](https://user-images.githubusercontent.com/57708209/134642325-161b4825-1746-4792-b632-1566e9889dad.png)
  
  2. Change into the gs-spring-boot-docker/initial directory
     
     cd gs-spring-boot-docker/initial
  
  3. Change into the Java source directory: src/main/java/hello
     
     cd src/main/java/hello
  
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
 
 3. Run the app:

 4. Keep the code running and test the app in one of the following ways:
    * In a new terminal connected to your instance, enter the following code:
    * Load the following address in a browser:
 
 5. Stop the running application. Example: Ctrl + C



#### 3. Build a Docker Image
 
 1. Change into the gs-spring-boot-docker/initial directory.
 
 2. Create a file named Dockerfile.

 3. Build the Docker image:

 4. Run the Docker image:

 5. Stop the running application.

  
## 4. Deploy Your Docker Image

#### 1. Create a Docker Repository
 
#### 2. Push your Local Image
 
 1. Open a terminal window.
 2. Log in to OCI Container Registry:
 3. List your local Docker images:
 4. Tag your local image with the URL for the registry plus the repo name, so you can push it to that repo.
 5. Check your Docker images to see if the image is tagged.
 6. Push the image to Container Registry.
 7. Open the navigation menu and click Developer Services. Under Containers & Artifacts, click Container Registry.
 8. Find your image in Container Registry after the push command is complete.


#### 3. Deploy the Image
 
  1. Create a registry secret for your application. This secret authenticates your image when you deploy it to your cluster.
     To create your secret, fill in the information in this template .
  2. Verify that the secret is created. Issue the following command:
  3. Determine the host URL to your registry image using the following template:
  4. On your system, create a file called sb-app.yaml with the following text:
  5. Deploy your application with the following command.




#### 4. Test your App
  1. Check if the load balancer is live:
  2. Use the load balancer IP address to connect to your app in a browser:
  3. (Optional) To undeploy your application from the cluster, run the following command:




  




     








