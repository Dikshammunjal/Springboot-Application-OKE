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

1. Check your service limits
>**NOTE:** Not covered in this guide.

2. Create an Authorization Token
>**NOTE:** Not covered in this guide.

3. Gather Required Information
Like Object Storage Namespace, Tenancy OCID, Username, User OCID, Region, Region Key, Auth Token

4. Setup OCI Command Line Interface
>**NOTE:** Not covered in this guide.

## 2. Set Up a Cluster

1. Add Compartment policy
allow group <the-group-your-username-belongs> to manage compartments in tenancy
2. Create a Compartment
3. Add Resource Policy
4. Create a OKE Cluster with 'Quick Create'
5. Set Up Local Access to CLuster
  
## 3. Build a Local Application
1. Create a Local Application
   Make a copy of the Spring Boot Docker guide with Git:
   ```bash
   $ git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```
   ![image](https://user-images.githubusercontent.com/57708209/134642325-161b4825-1746-4792-b632-1566e9889dad.png)
   
2. Run the Local Application
3. BUild a Docker Image
  
## 4. Deploy Your Docker Image
1. Create a Docker Repository
2. Push your Local Image
3. Deploy the Image
4. Test your App

  




     








