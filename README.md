# OWASP ZAP Baseline Test via Azure
An Azure ARM template designed to enable continuous security workflows, such as running baseline security tests against a web-based service as part of a release process. The template:

   * Creates a storage account and blob container
   * Provisions the [OWASP Zed Attack Proxy](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project) docker image to an Azure Container Instance
   * Runs a baseline set of tests and outputs them to the blob store

To meet more specific requirements please fork and amend the template. If you enhance the functionality and think more people might benefit then I'd be more than happy to deal with pull requests.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fnathankitchen%2Fowasp-azure-arm%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fnathankitchen%2Fowasp-azure-arm%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

## About
For more information on how this template works and its development, see my blog posts:

   * [Continuous Security with OWASP ZAP and Azure ARM (part 1)](https://www.nathankitchen.com/owasp-zap-baseline-test-in-azure-devops/)
   * [Continuous Security with OWASP ZAP and Azure DevOps (part 2)](https://www.nathankitchen.com/owasp-zap-baseline-test-in-azure-devops-2/)

## Usage
### Azure DevOps
If you're using Azure DevOps, you can import a *Task Group* defining all the steps required to execute the ZAP run and publish the results. Find `az-devops-task-group.json` in the repository.

Note that this definition downloads the latest version of the ARM template, parameters, and XSLT file over the public internet. For a more stable solution, I recommend forking the repo and creating a simple build pipeline that publishes an artifact containing the relevant files.

### Any other scenario
Deploy the ARM template to your Azure subscription, specifying the following:

   * **Resource Group** - All resources get deployed to the same resource group, and to its location. *NB:* Make sure you deploy to one that supports [Azure Container Instances](https://azure.microsoft.com/en-gb/services/container-instances/) (check [here](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=container-instances)), or the template will fail!*

   * **Blob Container Name** - The ARM template will automatically provision an [Azure Storage account](https://azure.microsoft.com/en-gb/services/storage/) with a [Blob Container](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction) where the results of the ZAP run will be saved, in XML form. Defaults to `reports`.

   * **Target** - The name of the web resource to scan. Must begin with http/https.

   * **Spider Time** - How long (in minutes) ZAP should spend spidering the target for additional content pages. ZAP will only spider links to the same domain. Defaults to `1`. *(Possible bug that this is not being respected, seems to scan for way longer than this limit)*

   * **Report name** - The name of the output file. This will be an XML document containing the output of the ZAP baseline run. You can transform this to be compatible with Azure DevOps as part of a pipeline using [this XSLT file](https://dev.azure.com/francislacroix/_git/CodeShare?path=%2FOWASPBlog%2FOWASPToNUnit3.xslt&version=GBmaster).

## Credit
Based on the work of **Francis Lacroix** over at Microsoft's [Premier Developer Blog](https://devblogs.microsoft.com/premier-developer/azure-devops-pipelines-leveraging-owasp-zap-in-the-release-pipeline/), ported to an ARM template.

Template and task group created by [Nathan Kitchen](https://www.twitter.com/nathankitchen). I'm a Cloud Architect and lead delivery of our Digital Platform at [Trustmarque](https://www.trustmarque.com/).

## License
The MIT License (MIT)

Copyright (c) 2019 Nathan Kitchen

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
