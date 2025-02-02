[![License](https://img.shields.io/badge/License-Apache2-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0) [![Community](https://img.shields.io/badge/Join-Community-blue.svg)](https://callforcode.org/slack) [![Hacktoberfest](https://img.shields.io/badge/Celebrate-Hacktoberfest-orange.svg)](https://call-for-code-for-racial-justice.github.io/Hacktoberfest/#/?id=main)

# Open Sentencing - Aggregator

Aggregator is the core module of the Open Sentencing Model which exposes REST endpoints to manage and maintain inventory of public attorney, clients, and case-related information. Aggregator is the point of articulation between the UI component and Data Prediction/Analytics modules. This is built using the IBM Starter Kit which comes pre-configured as a microservice with a Liberty server and uses the Cloudant database for storage. This is integrated with OpenAPI specifications to discover and understand the capabilities of the service without access to source code, documentation, or through network traffic inspection.

The Open Sentencing Aggregator is part of the overall Open Sentencing tool.  
Check out the main GitHub repo for more information: 
Call for Code for Racial Justice - Open Sentencing:  https://github.com/Call-for-Code-for-Racial-Justice/Open-Sentencing.

# Architecture
The architecture of the complete system is shown below.  

![Architecture Digaram](images/architecture.png)

# Create and deploy Aggregator application

To build the application you can run
```bash
docker build . -t <your-tag>
```
For the application to run, a Cloudant database is required. You can provision a free version from IBM Cloud or provide your own one. The HELM charts expect an IAMKey and the DB URL as parameters.
If you want to deploy the application to an OpenShift Cluster (ROKS) on IBM Cloud, perform the following steps:
```bash
ibmcloud login
ibmcloud cr login # logs you in to the Docker registry with your local Docker CLI
docker push <your-tag>
# Make sure your Container Registry is accessible by the ServiceAccount you are using, the default namespace and default ServiceAccount in an ROKS cluster are already integrated by default with the IBM Container Registry in your cloud account
helm package chart/base/
helm install --set image.repository=us.icr.io/emb-race-team/os-aggregator --set image.tag=helm-01 --set db.iamkey=<your-iam-key> --set db.url=https://<your-db-instance> aggregator os-aggregator-1.1.4.tgz --namespace deploy-test
```

If you want to install to a different namespace than default, you have to copy the secret first as described here (or create a new one): https://cloud.ibm.com/docs/openshift?topic=openshift-registry#copy_imagePullSecret.

## How to access the application

Once can use the [openapi spec](src/main/webapp/META-INF/openapi.yaml) or [openapi url](http://localhost:8090/openapi/ui/) to find the documentation of REST services exposed

![OpenAPI Specification](images/openapi.png)

### Building Locally

To get started building this application locally, you can either run the application natively or use the [IBM Cloud Developer Tools](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started) for containerization and easy deployment to IBM Cloud.

#### Native Application Development

* [Maven](https://maven.apache.org/install.html)
* Java 8: Any compliant JVM should work.
  * [Java 8 JDK from Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
  * [Java 8 JDK from IBM (AIX, Linux, z/OS, IBM i)](http://www.ibm.com/developerworks/java/jdk/),
    or [Download a Liberty server package](https://developer.ibm.com/assets/wasdev/#filter/assetTypeFilters=PRODUCT)
    that contains the IBM JDK (Windows, Linux)

To build and run the application:
1. Edit src/main/liberty/config/server.sample.env and save as src/main/liberty/config/server.env
2. mvn verify
3. mvn liberty:dev or mvn liberty:debug
4. Once completed, enter `http://localhost:9080/openapi/ui in browser to view the swagger ui.

To run an application in Docker use the Docker file called `Dockerfile`. If you do not want to install Maven locally you can use `Dockerfile-tools` to build a container with Maven installed.

You can verify the state of your locally running application using the Selenium UI test script included in the `scripts` directory.

## License

This sample application is licensed under the Apache License, Version 2. Separate third-party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1](https://developercertificate.org/) and the [Apache License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache License FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)

## Sentencing Data

This site provides applications using data that has been modified for use from its original source, www.ida.ussc.gov, an official website of the U.S. Sentencing Commission. The U.S. Sentencing Commission makes no claims as to the content, accuracy, timeliness, or completeness of any of the data provided at this site. The data provided at this site is subject to change at any time. It is understood that the data provided at this site is being used at one’s own risk.

## How to Help  *We'd love your involvement!*
Please visit our main repo here: https://github.com/Call-for-Code-for-Racial-Justice/Open-Sentencing.  More detailed areas on where we need assistance are provided.
If you would like to [Help](https://developer.ibm.com/callforcode/racial-justice/) with the cause to use technology to battle racism, we would love for you to get involved!  Please submit updates to us for review. 
Together we can use technology to fight systemic racism!

Please read [CONTRIBUTING.md](https://github.com/Call-for-Code-for-Racial-Justice/Open-Sentencing/blob/master/CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.
