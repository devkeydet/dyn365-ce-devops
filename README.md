
# Introduction
Please watch the following video to get a high level overview of this project:

[![Video Introduction](https://img.youtube.com/vi/AorM792M8nY/0.jpg)](https://www.youtube.com/watch?v=AorM792M8nY)

The project provides reference examples of using [Visual Studio Team Service (VSTS)](https://www.visualstudio.com/team-services/) to build and deploy Dynamics 365 based applications.  The reference examples are based on various use cases, including:

* [Primary Release - Dynamics Only.json](https://github.com/devkeydet/dyn365-ce-devops/blob/master/Primary%20Release%20-%20Dynamics%20Only.json) - Deploying a Dynamics 365 solution package and initial data from source control.
* [Primary Release - Dynamics + Azure.json](https://github.com/devkeydet/dyn365-ce-devops/blob/master/Primary%20Release%20-%20Dynamics%20%2B%20Azure.json) - Builds on [Primary Release - Dynamics Only.json](https://github.com/devkeydet/dyn365-ce-devops/blob/master/Primary%20Release%20-%20Dynamics%20Only.json) and adds deployment of Azure components to the release to demonstrate deploying a release across both.

The intent of this project is to evolve (over time) to implement additional reference examples based on common and/or requested use cases.  Lessons learned from this project have resulted in contributions back to https://github.com/waelHamze/xrm-ci-framework/ and https://github.com/scottdurow/SparkleXrm/wiki/spkl.  Please feel free to suggest new scenarios / starter templates or other ideas via the [issues](https://github.com/devkeydet/dyn365-ce-devops/issues) section.  

These release templates point to a sample application that spans Dynamics 365 & Azure.  The source code that will be built and deployed by VSTS is available at:
https://github.com/devkeydet/CrmAsyncRequestResponseSampleV2

The source code in the repository above shows how to source control *both* code and configurations associated with a Dynamics 365 demo/poc/sample.

The reference build definitions show two different build scenarios:

* [Primary Build.json](https://github.com/devkeydet/dyn365-ce-devops/blob/master/Primary%20Build.json) - shows how to use VSTS to:
    * Build assets from a remote GitHub repository
        * This, of course, could be done from a VSTS repository as well depending on whether the source code should be public or private
    * Build all the assets from source control into their deployable form
        * Reports unit test pass/fail for both .net and javascript code as part of build reporting
        * Reports code coverage of unit tests 
        * Solution package zip files
            * Packages Dynamics 365 configuration xml files using [Solution Packager](https://msdn.microsoft.com/en-us/library/jj602987.aspx)
        * Starter data import zip files 
            * Zips up xml files produced by the [Configuration Migration](https://technet.microsoft.com/library/dn647421.aspx) tool to setup sample data
        * .NET dlls for both Dynamics 365 and an [Azure Function App](https://azure.microsoft.com/en-us/services/functions/)
        * etc.
* [Patch Build.json](https://github.com/devkeydet/dyn365-ce-devops/blob/master/Patch%20Build.json) - shows how to use VSTS to:
    * Build a simple [patch](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/create-patches-simplify-solution-updates) from source control

The reference release definitions show how to deploy assets from the build to the target environment(s).  Each does the following:
* Primary Release - Dynamics Only.json
    * Resets a Dynamics 365 instance by using the [Online Management API for Dynamics 365 Customer Engagement](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/online-management-api)
    * Deploys the Dynamics 365 package and sample/intiial data using [Package Deployer](https://msdn.microsoft.com/en-us/library/dn688182.aspx)
        * Demonstrates how to activate plugins *after* the sample/initial data is imported
    * Updates plugin [Secure Configuration](https://us.hitachi-solutions.com/blog/use-secure-vs-unsecure-configuration-plugins/) since it doesn't get stored in the solution package and can vary by environment
    * Backs up the Dynamics 365 instance using the  [Online Management API for Dynamics 365 Customer Engagement](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/online-management-api)
* Primary Release - Dynamics + Azure.json
    * Registers and Azure AD application so the code hosted in Azure has permissions to make web api calls to Dynamics 365
    * Resets a Dynamics 365 instance by using the [Online Management API for Dynamics 365 Customer Engagement](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/online-management-api)
    * Deploys an [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) [template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#template-deployment) to provision necessary Azure resources
    * Gets configuration data from the Azure deployment for use in latter parts of the deployment process
    * Deploys Azure Functions dlls from the build
    * Applies configuration settings to the Azure environment
    * Deploys the Dynamics 365 package and sample data using [Package Deployer](https://msdn.microsoft.com/en-us/library/dn688182.aspx)
    * Updates plugin [Secure Configuration](https://us.hitachi-solutions.com/blog/use-secure-vs-unsecure-configuration-plugins/) since it doesn't get stored in the solution package and can vary by environment
    * Backs up the Dynamics 365 instance using the [Online Management API for Dynamics 365 Customer Engagement](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/online-management-api)
* Patch Release - Dynamics Only.json
    * Restore a Dynamics 365 instances to test applying the patch to, using the [Online Management API for Dynamics 365 Customer Engagement](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/online-management-api)
    * Import the patch solution
    * Publish the customizations
    * Disable Admin Mode using the [Online Management API for Dynamics 365 Customer Engagement](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/online-management-api)

# UI Automation Testing
The source code of the sample application has a project called [UIAutomationTests](https://github.com/devkeydet/CrmAsyncRequestResponseSampleV2/tree/master/UIAutomationTests), which uses https://github.com/microsoft/easyrepro to demonstrate how perform automated UI testing for Dynamics 365.  However, the tests are not yet incorporated into a release as a predeployment quality gate.  There is a known issue when using https://github.com/microsoft/easyrepro with the phantomjs driver for headless test execution.  Therefore, we cannot run the UI automation tests using the hosted VSTS agent.  The hosted agent doesn't have browsers installed.  The workaround today is to deploy your own agent.  Doing so would allow you to install a browser and use one of the selenium browser drivers.  See https://docs.microsoft.com/en-us/vsts/build-release/test/continuous-test-selenium for more details.

# Getting Started
Please watch the following videos to help you understand how to get started:

*Part I* - This video walks through [Primary Build.json](https://github.com/devkeydet/dyn365-ce-devops/blob/master/Primary%20Build.json) and [Primary Release - Dynamics Only.json](https://github.com/devkeydet/dyn365-ce-devops/blob/master/Primary%20Release%20-%20Dynamics%20Only.json), with a goal of helping you understand how they work and how to import them into your own VSTS project.

[![Video walkthrough part one](https://img.youtube.com/vi/O_q3cSWAUVI/0.jpg)](https://www.youtube.com/watch?v=O_q3cSWAUVI)

*Part II* - This video assumes you watched *Part I*.  It builds on *Part I*, with a goal of helping you understand how to import and execute [Primary Release - Dynamics + Azure.json](https://github.com/devkeydet/dyn365-ce-devops/blob/master/Primary%20Release%20-%20Dynamics%20%2B%20Azure.json), which includes a more elaborate example, including provisioning, deploying and configuring Azure services and code that work as part of the overall application.

[![Video walkthrough part two](https://img.youtube.com/vi/hlAEMr4xlCY/0.jpg)](https://www.youtube.com/watch?v=hlAEMr4xlCY)
