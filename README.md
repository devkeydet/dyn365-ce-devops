# Introduction
The purpose of this project is to serve as a reference implementation of using VSTS to build and deploy a sample applications that spans Dynamics 365 & Azure.
The source code that will be built by VSTS is available at:
https://github.com/devkeydet/CrmAsyncRequestResponseSampleV2

The source code in the repository above shows how to source control *both* code and configurations associated with a Dynamics 365 demo/poc/sample.  There is also a video walkhtrough (coming soon) of the sample. 


The reference build definition in this repository (Primary Build.json) shows how to use VSTS to:
* Build assets from a remote GitHub repository
    * This, of course, could be done from a VSTS repository as well depending on whether the source code should be public or private
* Build all the assets from source control into their deployable form
    * Solution package zip files
        * Packages Dynamics 365 configuration xml files using [Solution Packager](https://msdn.microsoft.com/en-us/library/jj602987.aspx)
    * Starter data import zip files
        * Zips up xml files produced by the [Configuration Migration](https://technet.microsoft.com/library/dn647421.aspx) tool to setup sample data
    * .NET dlls
    * etc.
The refernce release definition (Primary Release.json) shows how to deploy assets from the build to the target environment(s)
* Azure subscription(s)
* Dynamics 365 instance(s)
* etc.
* Specifically, it:
    * Deploys an [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) [template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#template-deployment) to provision necessary Azure resources
    * Gets configuration data from the Azure deployment for use in latter parts of the deployment process
    * Applies configuration settings to the Azure environment
    * Deploys the Dynamics 365 package and sample data using [Package Deployer](https://msdn.microsoft.com/en-us/library/dn688182.aspx)
    * Updates plugin [Secure Configuration](https://us.hitachi-solutions.com/blog/use-secure-vs-unsecure-configuration-plugins/) since it doesn't get stored in the solution package

# Getting Started
TODO: Video walkthrough of importing the json files into VSTS plus additional manual steps to get the build and release working.
