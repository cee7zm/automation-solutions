# Turbonomic Automation for AWS, Azure, and IBM Cloud

### Change Log

- **06/2022** - Readability revisions
- **05/2022** - Usability updates and Bug Fixes
- **05/2022** - Bug Fix Release
- **04/2022** - Initial Release

> This collection of Turbonomic terraform automation layers has been crafted from a set of  [Terraform modules](https://modules.cloudnativetoolkit.dev/) created by the IBM GSI Ecosystem Lab team part of the [IBM Partner Ecosystem organization](https://www.ibm.com/partnerworld/public?mhsrc=ibmsearch_a&mhq=partnerworld). Please contact **Matthew Perrins** _mjperrin@us.ibm.com_, **Vijay Sukthankar** _vksuktha@in.ibm.com_, **Sean Sundberg** _seansund@us.ibm.com_, **Tom Skill** _tskill@us.ibm.com_,  or **Andrew Trice** _amtrice@us.ibm.com_ for more details or raise an issue on the repository.

The automation will support the installation of Turbonomic on three cloud platforms **AWS**, **Azure**, and **IBM Cloud**.

### Target Infrastructure

The Turbonomic automation assumes you have an OpenShift cluster already configured on your cloud of choice. The supported managed options are [ROSA for AWS](https://aws.amazon.com/rosa/), [ARO for Azure](https://azure.microsoft.com/en-us/services/openshift/) or [ROKS for IBM Cloud ](https://www.ibm.com/cloud/openshift).

Before you start to install and configure Turbonomic, you will need to identify what your target infrastructure is going to be. You can start from scratch and use one of the pre-defined reference architectures from IBM or bring your own. It is recommended to install this on a `4cpu x 16gb x 2 worker nodes` **OpenShift Cluster**

### Reference Architectures

The reference architectures are provided in three different forms, with increasing security and associated sophistication to support production configuration. These three forms are as follows:

- **Quick Start** - a simple architecture to quickly get an OpenShift cluster provisioned
- **Standard** - a standard production deployment environment with typical security protections, private endpoints, VPN server, key management encryption, etc
- **Advanced** - a more advanced deployment that employs network isolation to securely route traffic between the different layers.

For each of these reference architecture, we have provided a detailed set of automation to create the environment for the software. If you do not have an OpenShift environment provisioned, please use one of these. They are optimized for the installation of this solution.

| Cloud Platform                                                                                                            | Automation and Documentation                                                                                                                                                                                  |   
|---------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [TechZone for IBMers and Partners](https://techzone.ibm.com/collection/turbonomic-automation-multicloud) | You can provision ARO, ROSA and ROKS through IBM TechZone. This is only supported for IBMers and IBM Partners
| [IBM Cloud](https://cloud.ibm.com)                                                                                        | [IBM Cloud Quick Start](https://github.com/IBM/automation-ibmcloud-infra-openshift/tree/initial-version) </br> [IBM Cloud Standard](https://github.com/IBM/automation-ibmcloud-infra-openshift/tree/standard) |  
| [AWS](https://aws.amazon.com/)                                                                                            | [AWS Quick Start](https://github.com/IBM/automation-aws-infra-openshift/tree/1-quick-start) </br> [AWS Standard - Coming soon]()                                                                              |
| [Azure](https://portal.azure.com/#home)                                                                                   | [Azure Quick Start - Coming soon]()                                                                                 |                                                                                             | 
| Bring You Own Infrastructure                                                                                              | You will need to setup GitOps and Storage details on the following steps                                                                                                                                        /

### Getting Started

Within this repository you will find a set of Terraform template bundles that embody best practices for provisioning Turbonomic in multiple cloud environments. This `README.md` describes the SRE steps required to provision the Turbonomic software.

This suite of automation can be used for a Proof of Technology environment, or used as a foundation for production workloads with a fully working end-to-end cloud-native environment. The software installs using **GitOps** best practices with [**Red Hat Open Shift GitOps**](https://docs.openshift.com/container-platform/4.8/cicd/gitops/understanding-openshift-gitops.html)

## Turbonomic Architecture

The following reference architecture represents the logical view of how Turbonomic works after it has been installed. After obtaining a license key you will need to register your data sources. They can range from other Kubernetes environment to VMWare and Virtual Machines.

![Reference Architecture](./turboArch.png)

## Deploying Turbonomic

The following instructions will help you install Turbonomic into AWS, Azure, and IBM Cloud OpenShift Kubernetes environment.

### Obtaining License Key

To use Turbonomic you are required to install a license key. For Proof of Concepts IBM Partners and IBMers can obtain it using the steps highlighted below.

#### For Partners

For Partners follow these steps:

1. Proof of Concepts (PoCs)/Proof of Technology (PoTs), Partners can download a license key from [Partner World Software Catalog](https://www.ibm.com/partnerworld/program/benefits/software-access-catalog)
2. You can search the software catalog for  **M05C4EN	IBM Turbonomic Application Resource Management On-Prem 8.4.6 for install on Kubernetes English**,
3. Download the package it is 11gb in size and unzip the contents. Look for the client license file for Turbonomic, with a name similar to `CP4MCM_IBM_ARM_OEM_Premier_License_July_2022.lic`
5. This file is covered by **Turbonomic ARM P/N are currently available under IBM PPA terms and conditions**

#### For IBMers

For IBMers you can download a license key using these steps:

1. Go to [XL Leverage](https://w3-03.ibm.com/software/xl/download/ticket.wss)
2. Search with keyword: turbonomic
3. Select the package **M05C4EN	IBM Turbonomic Application Resource Management On-Prem 8.4.6 for install on Kubernetes English** and download, this file is 11gb in size, once downloaded unzip to find the license key
4. Extract this download package to get the turbonomic license key
   This package contains license file for turbonomic, with a name similar to “CP4MCM_IBM_ARM_OEM_Premier_License_July_2022.lic

### Turbonomic for Multi Cloud

The turbonomic automation is broken into what we call layers of automation or bundles. The bundles enable SRE activities to be optimized. The automation is generic between clouds other than setting Storage for IBM Cloud. That was broken into a separate automation layer.

| BOM ID | Name                                                           | Description                                                                                                                                                                                                                                   | Run Time |
|--------|----------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| 200    | [200 - OpenShift Gitops](./200-openshift-gitops)               | Set up OpenShift GitOps in ROSA, ARO or ROKS, this is required to install the software using gitops only use this if you are bringing your own OpenShift Cluster which has not been provisioned from the reference architectures listed above | 10 Mins  |
| 202    | [202 - IBM Cloud Storage](./202-turbonomic-ibmcloud-storage-class)        | If you are installing into your own ROKS clusters on IBM Cloud you will need to use this automation bundle to configure IBM Cloud Storage class before installing Turbonomic                                                                  | 10 Mins |
| 250 | [250 - Turbonomic Multi Cloud](./250-turbonomic-multicloud) | Provision Turbonomic into Multi Cloud environment AWS, Azure and IBM Cloud supported                                                                                                                                                          | 10 Mins  |


> At this time the most reliable way of running this automation is with Terraform in your local machine either through a bootstrapped container image or with native tools installed. We provide a Container image that has all the common SRE tools installed. [CLI Tools Image,](https://quay.io/repository/ibmgaragecloud/cli-tools?tab=tags) [Source Code for CLI Tools](https://github.com/cloud-native-toolkit/image-cli-tools)

## Installation Steps

Before you start the installation please install the following pre-req tools on your machine.

> We have tested this on a modern Mac laptop. We are testing on M1 machines. You will need to setup the tools natively in your M1 Mac OS and not run the `launch.sh` script.

### Pre-Req Setup

Please install the following Pre-Req tools to help you get started with the SRE tasks for installing Turbonomic into an existing OpenShift Cluster on AWS, Azure, or IBM Cloud.

Pre-requisites:
- Check you have a valid GitHub ID that can be used to create a repository in your own organization [GitHub]( https://github.com/) or GitHub Enterprise account.
- Install a code editor, we recommend [Visual Studio Code](https://code.visualstudio.com/)
- Install [Brew](https://brew.sh/)
- Install a **Colima** a replacement for Docker Desktop , [Colima](https://github.com/abiosoft/colima)
   ```
   brew install colima
   ```

### Installing Turbonomic

The installation process will use a standard GitOps repository that has been built using the Modules to support Turbonomic installation. The automation is consistent across three cloud environments AWS, Azure, and IBM Cloud.

Steps:

1. Clone the automation code to your local machine by running this git command in your favorite command line shell.

     ```
     git clone https://github.com/IBM/automation-turbonomic
     ```
2. Navigate into the automation-turbonomic folder using your command line.	(Note: The README.md has a comprehensive instructions on how to install this into other cloud environments than TechZone. This document focuses on getting it running in a TechZone requested environment.)

3. Set-up your credentials.properties file. This will enable a secure access to your cluster:

    ```
    cp credentials.template credentials.properties
    code credential.properties
    ```

    ```
    ## Add the values for the Credentials to access the OpenShift Environment
    ## Instructions to access this information can be found in the README.MD
    ## This is a template file and the ./launch.sh script looks for a file based on this template named credentials.properties
    
    ## gitops_repo_host: The host for the git repository
    TF_VAR_gitops_repo_host=github.com
    ## gitops_repo_username: The username of the user with access to the repository
    TF_VAR_gitops_repo_username=
    ## gitops_repo_token: The personal access token used to access the repository
    TF_VAR_gitops_repo_token=
    
    ## TF_VAR_server_url: The url for the OpenShift api server
    TF_VAR_server_url=
    ## TF_VAR_cluster_login_token: Token used for authentication to the api server
    TF_VAR_cluster_login_token=

    ```

4. Add your Git Hub username and your Personal Access Token to `repo_username` and `repo_token` from Step 3. 
5. From your OpenShift console click on top right menu and select "Copy login command" and click on "Display Token." Copy the API Token value into the `login_token` value. Copy only the part of the Server URL starting with "https" into the `server_url` value.

6. Make sure you are **not** running Docker Desktop as this is not allowed under their new terms and conditions for corporate use. You need to install **Colima** as an alternative (see pre-reqs).

    ```
    brew install colima
    colima start
    ```

7. We are now ready to start installing Turbonomic. Run the `launch.sh` command and make sure you are in the root of the automation-turbonomic repository.  **launch.sh** will download a container image that contains all the command line tools to enable easy installation of the software. Once it has downloaded, it will mount the local file system and exec into the container for you to start running commands from within this custom container.

   ```
   ./launch.sh
   Cleaning up old container: cli-tools-WljCg
   Initializing container cli-tools-WljCg from quay.io/cloudnativetoolkit/terraform:v1.1
   Attaching to running container...
   /terraform $
   ```

> (We expect partners and clients will use their own specific **Continuous Integration** tools to support this.)

8. Create a workspace to run the Terraform automation. Run the command below for setup-workspace.sh. You will need to provide the `-p` platform value which can be `azure` | `aws` or `ibm` then supply a prefix name.

```
./setup-workspace.sh -p ibm -n turbo01
``` 

The default `terraform.tfvars` file is symbolically linked to the new `workspaces/current` folder, enabling you to edit the file in your native operating system using your editor of choice.

9. Edit the default `terraform.tfvars` file this will enable you to setup the GitOps parameters.

      ```
    ########################################################
    # Name: Turbonomic Terraform Variable File
    # Desc: Initial input variables to support installation of Turbonomic into the cloud provider of your choice
    ########################################################
    
    ## gitops-ocp-turbonomic_storage_class_name: Name of the block storage class to use - if multizone deployment then waitforfirstconsumer must be set on storageclass binding mode
    ## <your block storage on aws: gp2, on azure: managed-premium, on ibm: ibmc-vpc-block-10iops-tier>
    gitops-ocp-turbonomic_storage_class_name="ibmc-vpc-block-mzr"
    
    ## gitops-repo_host: The host for the git repository.
    gitops_repo_host="github.com"
    
    ## gitops-repo_type: The type of the hosted git repository (github or gitlab).
    gitops_repo_type="github"
    
    ## gitops-repo_org: The org/group where the git repository exists/will be provisioned.
    ## your gitorg - if left blank the value will default to your username
    gitops_repo_org="mjperrins"
    
    ## gitops-repo_repo: The short name of the repository (i.e. the part after the org/group name)
    gitops_repo_repo="turbo-gitops-01"
    
    ## gitops-cluster-config_banner_text: The text that will appear in the top banner in the cluster
    gitops-cluster-config_banner_text="Software Everywhere Turbonomic"
      ```

**Note the `repo_type` and `repo_host` are set to GitHub. You can change these to other Git Providers, like GitHub Enterprise or GitLab.**
10. For the `repo_org` value, set it to your default org name or specific a custom org value. This is the organization where the GitOps Repository will be created. Click on the top right menu and select "Your Profile" to take you to your default organization.
11. Set the `repo_repo` value to a unique name that you will recognize as the place where the GitOps configuration is going to be placed before Turbonomic is installed into the cluster. You can change the Banner text to something useful for your client project or demo. Save the `terraform.tfvars` file.
12. Navigate into the `/workspaces/current` folder. Navigate into the `200` folder and run the following commands:

      ```
      cd 200-openshift-gitops
      terraform init
      terraform apply --auto-approve
      ………
      Apply complete! Resources: 78 added, 0 changed, 0 destroyed.

      ```

This will kick off the automation for setting up the GitOps Operator into your cluster.

You can **check** the progress by looking at two places: first look in your github repository. You will see the git repository has been created based on the name you have provided. The Turbonomic install will populate this with information to let OpenShift GitOps install the software. Secondly look at the OpenShift console, Click Workloads->Pods and you will see the GitOps operator being installed.

12. **If you are using IBM Cloud,** set the `-p` flag to `ibm` and navigate into the `202-turbonomic-ibmcloud-storage-class` folder and run the following commands below. This will configure the storage correctly for IBM Cloud. If you are installing on AWS or Azure you can skip this step and move to the 250 installation of Turbonomic (Step 13).
`
     ```
     cd 202-turbonomic-ibmcloud-storage-class
     terraform init
     terraform apply --auto-approve
     ```

13. Now the GitOps is installed in the cluster and we have bound the git repository to OpenShift GitOps operator. Now we can populate this with some Software configuration that causes OpenShift GitOps to install the software into the cluster. Navigate into the `250` folder and run the following commands. This will install Turbonomic into the cluster.

     ```
     cd 250-turbonomic-multicloud
     terraform init
     terraform apply --auto-approve
     ………
     Apply complete! Resources: 38 added, 0 changed, 0 destroyed.
     ```

Once the installation has finished, you will see a message from  Terraform defining the state of the environment. The first change is indicated as a purple banner describing what was installed.

14. Validate if everything has installed correctly. Open your git repository where your git ops configuration was defined. Check if the payload folder has been created with the correct definitions for GitOps. Navigate to the `payload/2-services/namespace/turbonomic` folder and look at the content of the installation YAML files. You should see the Operator CR definitions

15. Open up Argo CD (OpenShift GitOps). Check it is correctly configured. Click on the Application menu 3x3 Icon on the header and select **Cluster Argo CD** menu item.

16. Complete the authorization with OpenShift, and then narrow the filters by selecting the **turbonomic namespace**. This will show you the GitOps dashboard of the software you have installed using GitOps techniques.

17. Click on the **turbonomic-turboinst** tile. You will see all the microservices that Turbonomic uses to install with their enablement state

### Setup Turbonomic after installation

Make sure you have downloaded the license key following the instructions in the pre-requisites section at the front of this document. 
Now that the installation process is complete,  it is time to configure Turbonomic and load your downloaded license key.

Steps:

1. In the OpenShift console navigate to the **Networking->Routes** and change the project from to **Turbonomic**. You will see the route to launch the dashboard for Turbonomic. Click on the Location URL to open Turbonomic.
2. The first time you will launch the dashboard it will ask you to define an Administration password. Enter your new password and confirm it.(Don’t forget to store it in your password manager!)
Once the account has been created, you will see the default screen.
3. Make sure you have downloaded the license key following the instructions in the pre-requisites section at the beginning of this tutorial.
4. Click on Settings on the left menu, then click on the **License** icon. Click **Import license**.
5. Drag your license key into the drop area. It should say it has been added.
6. Now we need to point Turbonomic at an environment for it to monitor. Click on the **Add Targets** button.
9. Click on `Kubernetes-Turbonomic` then the **Validate** button. Click on the **On** icon at the top of the left menu to see a monitor view of Turbonomic.

## Uninstalling & Troubleshooting

Please refer to the [Troubleshooting Guide](./TROUBLESHOOTING.md) for uninstallation instructions and instructions to correct common issues.

## Summary

This concludes the instructions for installing **Turbonomic** on AWS, Azure, and IBM Cloud

## How to Generate this repository from the source Bill of Materials.

This set of automation packages was generated using the open-source [`isacable`](https://github.com/cloud-native-toolkit/iascable) tool. This tool enables a [Bill of Material yaml](https://github.com/cloud-native-toolkit/automation-solutions/tree/main/boms/software/turbonomic) file to describe your software requirements. If you want up stream releases or versions you can use `iascable` to generate a new terraform module.

> The `iascable` tool is targeted for use by advanced SRE developers. It requires deep knowledge of how the modules plug together into a customized architecture. This repository is a fully tested output from that tool. This makes it ready to consume for projects.

