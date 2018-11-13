# Zero to Azure in 60 minutes

To give you a hint of the capabilities available to ASP.NET Core developers on Azure, let's take a quick tour through Azure App Service and Azure DevOps.

## Deploy an app to App Service

[Azure App Service](https://docs.microsoft.com/azure/app-service/) is Azure's web hosting platform. Deploying a web app to Azure App Service can be done manually or by an automated process. This section of the guide discusses deployment methods that can be triggered manually or by script using the command line, or triggered manually using Visual Studio.

In this section, you'll accomplish the following tasks:

* Download and build the sample app.
* Create an Azure App Service Web App using the Azure Cloud Shell.
* Deploy the sample app to Azure using Git.
* Deploy a change to the app using Visual Studio.
* Add a staging slot to the web app.
* Deploy an update to the staging slot.
* Swap the staging and production slots.

### Download and test the app

The app used in this guide is a pre-built ASP.NET Core app, [Simple Feed Reader](https://github.com/Azure-Samples/simple-feed-reader/). It's a Razor Pages app that uses the `Microsoft.SyndicationFeed.ReaderWriter` API to retrieve an RSS/Atom feed and display the news items in a list.

Feel free to review the code, but it's important to understand that there's nothing special about this app. It's just a simple ASP.NET Core app for illustrative purposes.

From a command shell, download the code, build the project, and run it as follows.

> *Note: Linux/macOS users should make appropriate changes for paths, e.g., using forward slash (`/`) rather than back slash (`\`).*

1. Clone the code to a folder on your local machine.

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. Change your working folder to the *simple-feed-reader* folder that was created.

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. Restore the packages, and build the solution.

    ```console
    dotnet build
    ```

4. Run the app.

    ```console
    dotnet run
    ```

    ![The dotnet run command is successful](./media/deploying-to-app-service/dotnet-run.png)

5. Open a browser and navigate to `http://localhost:5000`. The app allows you to type or paste a syndication feed URL and view a list of news items.

     ![The app displaying the contents of an RSS feed](./media/deploying-to-app-service/app-in-browser.png)

6. Once you're satisfied the app is working correctly, shut it down by pressing **Ctrl**+**C** in the command shell.

### Create the Azure App Service Web App

To deploy the app, you'll need to create an App Service [Web App](https://docs.microsoft.com/azure/app-service/app-service-web-overview). After creation of the Web App, you'll deploy to it from your local machine using Git.

1. Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash). Note: When you sign in for the first time, Cloud Shell prompts to create a storage account for configuration files. Accept the defaults or provide a unique name.

2. Use the Cloud Shell for the following steps.

    a. Declare a variable to store your web app's name. The name must be unique to be used in the default URL. Using the `$RANDOM` Bash function to construct the name guarantees uniqueness and results in the format `webappname99999`.

    ```console
    webappname=mywebapp$RANDOM
    ```

    b. Create a resource group. Resource groups provide a means to aggregate Azure resources to be managed as a group.

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    The `az` command invokes the [Azure CLI](https://docs.microsoft.com/cli/azure/). The CLI can be run locally, but using it in the Cloud Shell saves time and configuration.

    c. Create an App Service plan in the S1 tier. An App Service plan is a grouping of web apps that share the same pricing tier. The S1 tier isn't free, but it's required for the staging slots feature.

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    d. Create the web app resource using the App Service plan in the same resource group.

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    e. Set the deployment credentials. These deployment credentials apply to all the web apps in your subscription. Don't use special characters in the user name.

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    f. Configure the web app to accept deployments from local Git and display the *Git deployment URL*. **Note this URL for reference later**.

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    g. Display the *web app URL*. Browse to this URL to see the blank web app. **Note this URL for reference later**.

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. Using a command shell on your local machine, navigate to the web app's project folder (for example, `.\simple-feed-reader\SimpleFeedReader`). Execute the following commands to set up Git to push to the deployment URL:

    a. Add the remote URL to the local repository.

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    b. Push the local *master* branch to the *azure-prod* remote's *master* branch.

    ```console
    git push azure-prod master
    ```

    You'll be prompted for the deployment credentials you created earlier. Observe the output in the command shell. Azure builds the ASP.NET Core app remotely.

4. In a browser, navigate to the *Web app URL* and note the app has been built and deployed. Additional changes can be committed to the local Git repository with `git commit`. These changes are pushed to Azure with the preceding `git push` command.

### Deployment with Visual Studio

> *Note: This section applies to Windows only. Linux and macOS users should make the change described in step 2 below. Save the file, and commit the change to the local repository with `git commit`. Finally, push the change with `git push`, as in the first section.*

The app has already been deployed from the command shell. Let's use Visual Studio's integrated tools to deploy an update to the app. Behind the scenes, Visual Studio accomplishes the same thing as the command line tooling, but within Visual Studio's familiar UI.

1. Open *SimpleFeedReader.sln* in Visual Studio.
2. In Solution Explorer, open *Pages\Index.cshtml*. Change `<h2>Simple Feed Reader</h2>` to `<h2>Simple Feed Reader - V2</h2>`.
3. Press **Ctrl**+**Shift**+**B** to build the app.
4. In Solution Explorer, right-click on the project and click **Publish**.

    ![Right-click, Publish](./media/deploying-to-app-service/publish.png)
5. Visual Studio can create a new App Service resource, but this update will be published over the existing deployment. In the **Pick a publish target** dialog, select **App Service** from the list on the left, and then select **Select Existing**. Click **Publish**.
6. In the **App Service** dialog, confirm that the Microsoft or Organizational account used to create your Azure subscription is displayed in the upper right. If it's not, click the drop-down and add it.
7. Confirm that the correct Azure **Subscription** is selected. For **View**, select **Resource Group**. Expand the **AzureTutorial** resource group and then select the existing web app. Click **OK**.

    ![Publish App Service dialog](./media/deploying-to-app-service/publish-dialog.png)

Visual Studio builds and deploys the app to Azure. Browse to the web app URL. Validate that the `<h2>` element modification is live.

![The app with the changed title](./media/deploying-to-app-service/app-v2.png)

### Deployment slots

Deployment slots support the staging of changes without impacting the app running in production. Once the staged version of the app is validated by a quality assurance team, the production and staging slots can be swapped. The app in staging is promoted to production in this manner. The following steps create a staging slot, deploy some changes to it, and swap the staging slot with production after verification.

1. Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash), if not already signed in.
2. Create the staging slot.

    a. Create a deployment slot with the name *staging*.

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    b. Configure the staging slot to use deployment from local Git and get the **staging** deployment URL. **Note this URL for reference later**.

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    c. Display the staging slot's URL. Browse to the URL to see the empty staging slot. **Note this URL for reference later**.

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. In a text editor or Visual Studio, modify *Pages/Index.cshtml* again so that the `<h2>` element reads `<h2>Simple Feed Reader - V3</h2>` and save the file.

4. Commit the file to the local Git repository, using either the **Changes** page in Visual Studio's *Team Explorer* tab, or by entering the following using the local machine's command shell:

    ```console
    git commit -a -m "upgraded to V3"
    ```
5. Using the local machine's command shell, add the staging deployment URL as a Git remote and push the committed changes:

    a. Add the remote URL for staging to the local Git repository.

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    b. Push the local *master* branch to the *azure-staging* remote's *master* branch.

    ```console
    git push azure-staging master
    ```

    Wait while Azure builds and deploys the app.

6. To verify that V3 has been deployed to the staging slot, open two browser windows. In one window, navigate to the original web app URL. In the other window, navigate to the staging web app URL. The production URL serves V2 of the app. The staging URL serves V3 of the app.

    ![Comparing the browser windows](./media/deploying-to-app-service/ready-to-swap.png)

7. In the Cloud Shell, swap the verified/warmed-up staging slot into production.

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. Verify that the swap occurred by refreshing the two browser windows.

    ![Comparing the browser windows after the swap](./media/deploying-to-app-service/swapped.png)

### Summary

In this section, the following tasks were completed:

* Downloaded and built the sample app.
* Created an Azure App Service Web App using the Azure Cloud Shell.
* Deployed the sample app to Azure using Git.
* Deployed a change to the app using Visual Studio.
* Added a staging slot to the web app.
* Deployed an update to the staging slot.
* Swapped the staging and production slots.

In the next section, you'll learn how to build a DevOps pipeline with Azure Pipelines.

### Additional reading

* [Web Apps overview](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
* [Build a .NET Core and SQL Database web app in Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [Configure deployment credentials for Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-deployment-credentials)
* [Set up staging environments in Azure App Service](https://docs.microsoft.com/azure/app-service/web-sites-staged-publishing)

## Continuous integration and deployment

In the previous chapter, you created a local Git repository for the Simple Feed Reader app. In this chapter, you'll publish that code to a GitHub repository and construct an Azure DevOps Services pipeline using Azure Pipelines. The pipeline enables continuous builds and deployments of the app. Any commit to the GitHub repository triggers a build and a deployment to the Azure Web App's staging slot.

In this section, you'll complete the following tasks:

* Publish the app's code to GitHub
* Disconnect local Git deployment
* Create an Azure DevOps organization
* Create a team project in Azure DevOps Services
* Create a build definition
* Create a release pipeline
* Commit changes to GitHub and automatically deploy to Azure
* Examine the Azure Pipelines pipeline

### Publish the app's code to GitHub

1. Open a browser window, and navigate to `https://github.com`.
1. Click the **+** drop-down in the header, and select **New repository**:

    ![GitHub New Repository option](media/cicd/github-new-repo.png)

1. Select your account in the **Owner** drop-down, and enter *simple-feed-reader* in the **Repository name** textbox.
1. Click the **Create repository** button.
1. Open your local machine's command shell. Navigate to the directory in which the *simple-feed-reader* Git repository is stored.
1. Rename the existing *origin* remote to *upstream*. Execute the following command:
    ```console
    git remote rename origin upstream
    ```
1. Add a new *origin* remote pointing to your copy of the repository on GitHub. Execute the following command:
    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```
1. Publish your local Git repository to the newly created GitHub repository. Execute the following command:
    ```console
    git push -u origin master
    ```
1. Open a browser window, and navigate to `https://github.com/<GitHub_username>/simple-feed-reader/`. Validate that your code appears in the GitHub repository.

### Disconnect local Git deployment

Remove the local Git deployment with the following steps. Azure Pipelines (an Azure DevOps service) both replaces and augments that functionality.

1. Open the [Azure portal](https://portal.azure.com/), and navigate to the *staging (mywebapp\<unique_number\>/staging)* Web App. The Web App can be quickly located by entering *staging* in the portal's search box:

    ![staging Web App search term](media/cicd/portal-search-box.png)

1. Click **Deployment options**. A new panel appears. Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter. Confirm the removal operation by clicking the **Yes** button.
1. Navigate to the *mywebapp<unique_number>* App Service. As a reminder, the portal's search box can be used to quickly locate the App Service.
1. Click **Deployment options**. A new panel appears. Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter. Confirm the removal operation by clicking the **Yes** button.

### Create an Azure DevOps organization

1. Open a browser, and navigate to the [Azure DevOps organization creation page](https://go.microsoft.com/fwlink/?LinkId=307137).
1. Type a unique name into the **Pick a memorable name** textbox to form the URL for accessing your Azure DevOps organization.
1. Select the **Git** radio button, since the code is hosted in a GitHub repository.
1. Click the **Continue** button. After a short wait, an account and a team project, named *MyFirstProject*, are created.

    ![Azure DevOps organization creation page](media/cicd/vsts-account-creation.png)

1. Open the confirmation email indicating that the Azure DevOps organization and project are ready for use. Click the **Start your project** button:

    ![Start your project button](media/cicd/vsts-start-project.png)

1. A browser opens to *\<account_name\>.visualstudio.com*. Click the *MyFirstProject* link to begin configuring the project's DevOps pipeline.

### Configure the Azure Pipelines pipeline

There are three distinct steps to complete. Completing the steps in the following three sections results in an operational DevOps pipeline.

#### Grant Azure DevOps access to the GitHub repository

1. Expand the **or build code from an external repository** accordion. Click the **Setup Build** button:

    ![Setup Build button](media/cicd/vsts-setup-build.png)

1. Select the **GitHub** option from the **Select a source** section:

    ![Select a source - GitHub](media/cicd/vsts-select-source.png)

1. Authorization is required before Azure DevOps can access your GitHub repository. Enter *<GitHub_username> GitHub connection* in the **Connection name** textbox. For example:

    ![GitHub connection name](media/cicd/vsts-repo-authz.png)

1. If two-factor authentication is enabled on your GitHub account, a personal access token is required. In that case, click the **Authorize with a GitHub personal access token** link. See the [official GitHub personal access token creation instructions](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) for help. Only the *repo* scope of permissions is needed. Otherwise, click the **Authorize using OAuth** button.
1. When prompted, sign in to your GitHub account. Then select Authorize to grant access to your Azure DevOps organization. If successful, a new service endpoint is created.
1. Click the ellipsis button next to the **Repository** button. Select the *<GitHub_username>/simple-feed-reader* repository from the list. Click the **Select** button.
1. Select the *master* branch from the **Default branch for manual and scheduled builds** drop-down. Click the **Continue** button. The template selection page appears.

#### Create the build definition

1. From the template selection page, enter *ASP.NET Core* in the search box:

    ![ASP.NET Core search on template page](media/cicd/vsts-template-selection.png)

1. The template search results appear. Hover over the **ASP.NET Core** template, and click the **Apply** button.
1. The **Tasks** tab of the build definition appears. Click the **Triggers** tab.
1. Check the **Enable continuous integration** box. Under the **Branch filters** section, confirm that the **Type** drop-down is set to *Include*. Set the **Branch specification** drop-down to *master*.

    ![Enable continuous integration settings](media/cicd/vsts-enable-ci.png)

    These settings cause a build to trigger when any change is pushed to the *master* branch of the GitHub repository. Continuous integration is tested in the [Commit changes to GitHub and automatically deploy to Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) section.

1. Click the **Save & queue** button, and select the **Save** option:

    ![Save button](media/cicd/vsts-save-build.png)

1. The following modal dialog appears:

    ![Save build definition - modal dialog](media/cicd/vsts-save-modal.png)

    Use the default folder of *\\*, and click the **Save** button.

#### Create the release pipeline

1. Click the **Releases** tab of your team project. Click the **New pipeline** button.

    ![Releases tab - New definition button](media/cicd/vsts-new-release-definition.png)

    The template selection pane appears.

1. From the template selection page, enter *App Service* in the search box:

    ![Release pipeline template search box](media/cicd/vsts-release-template-search.png)

1. The template search results appear. Hover over the **Azure App Service Deployment with Slot** template, and click the **Apply** button. The **Pipeline** tab of the release pipeline appears.

    ![Release pipeline Pipeline tab](media/cicd/vsts-release-definition-pipeline.png)

1. Click the **Add** button in the **Artifacts** box. The **Add artifact** panel appears:

    ![Release pipeline - Add artifact panel](media/cicd/vsts-release-add-artifact.png)

1. Select the **Build** tile from the **Source type** section. This type allows for the linking of the release pipeline to the build definition.
1. Select *MyFirstProject* from the **Project** drop-down.
1. Select the build definition name, *MyFirstProject-ASP.NET Core-CI*, from the **Source (Build definition)** drop-down.
1. Select *Latest* from the **Default version** drop-down. This option builds the artifacts produced by the latest run of the build definition.
1. Replace the text in the **Source alias** textbox with *Drop*.
1. Click the **Add** button. The **Artifacts** section updates to display the changes.
1. Click the lightning bolt icon to enable continuous deployments:

    ![Release pipeline Artifacts - lightning bolt icon](media/cicd/vsts-artifacts-lightning-bolt.png)

    With this option enabled, a deployment occurs each time a new build is available.
1. A **Continuous deployment trigger** panel appears to the right. Click the toggle button to enable the feature. It isn't necessary to enable the **Pull request trigger**.
1. Click the **Add** drop-down in the **Build branch filters** section. Choose the **Build Definition's default branch** option. This filter causes the release to trigger only for a build from the GitHub repository's *master* branch.
1. Click the **Save** button. Click the **OK** button in the resulting **Save** modal dialog.
1. Click the **Environment 1** box. An **Environment** panel appears to the right. Change the *Environment 1* text in the **Environment name** textbox to *Production*.

   ![Release pipeline - Environment name textbox](media/cicd/vsts-environment-name-textbox.png)

1. Click the **1 phase, 2 tasks** link in the **Production** box:

    ![Release pipeline - Production environment link.png](media/cicd/vsts-production-link.png)

    The **Tasks** tab of the environment appears.
1. Click the **Deploy Azure App Service to Slot** task. Its settings appear in a panel to the right.
1. Select the Azure subscription associated with the App Service from the **Azure subscription** drop-down. Once selected, click the **Authorize** button.
1. Select *Web App* from the **App type** drop-down.
1. Select *mywebapp/<unique_number/>* from the **App service name** drop-down.
1. Select *AzureTutorial* from the **Resource group** drop-down.
1. Select *staging* from the **Slot** drop-down.
1. Click the **Save** button.
1. Hover over the default release pipeline name. Click the pencil icon to edit it. Use *MyFirstProject-ASP.NET Core-CD* as the name.

    ![Release pipeline name](media/cicd/vsts-release-definition-name.png)

1. Click the **Save** button.

### Commit changes to GitHub and automatically deploy to Azure

1. Open *SimpleFeedReader.sln* in Visual Studio.
1. In Solution Explorer, open *Pages\Index.cshtml*. Change `<h2>Simple Feed Reader - V3</h2>` to `<h2>Simple Feed Reader - V4</h2>`.
1. Press **Ctrl**+**Shift**+**B** to build the app.
1. Commit the file to the GitHub repository. Use either the **Changes** page in Visual Studio's *Team Explorer* tab, or execute the following using the local machine's command shell:

    ```console
    git commit -a -m "upgraded to V4"
    ```
1. Push the change in the *master* branch to the *origin* remote of your GitHub repository:

    ```console
    git push origin master
    ```

    The commit appears in the GitHub repository's *master* branch:

    ![GitHub commit in master branch](media/cicd/github-commit.png)

    The build is triggered, since continuous integration is enabled in the build definition's **Triggers** tab:

    ![enable continuous integration](media/cicd/enable-ci.png)

1. Navigate to the **Queued** tab of the **Azure Pipelines** > **Builds** page in Azure DevOps Services. The queued build shows the branch and commit that triggered the build:

    ![queued build](media/cicd/build-queued.png)

1. Once the build succeeds, a deployment to Azure occurs. Navigate to the app in the browser. Notice that the "V4" text appears in the heading:

    ![updated app](media/cicd/updated-app-v4.png)

### Examine the Azure Pipelines pipeline

#### Build definition

A build definition was created with the name *MyFirstProject-ASP.NET Core-CI*. Upon completion, the build produces a *.zip* file including the assets to be published. The release pipeline deploys those assets to Azure.

The build definition's **Tasks** tab lists the individual steps being used. There are five build tasks.

![build definition tasks](media/cicd/build-definition-tasks.png)

1. **Restore** &mdash; Executes the `dotnet restore` command to restore the app's NuGet packages. The default package feed used is nuget.org.
1. **Build** &mdash; Executes the `dotnet build --configuration release` command to compile the app's code. This `--configuration` option is used to produce an optimized version of the code, which is suitable for deployment to a production environment. Modify the *BuildConfiguration* variable on the build definition's **Variables** tab if, for example, a debug configuration is needed.
1. **Test** &mdash; Executes the `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` command to run the app's unit tests. Unit tests are executed within any C## project matching the `**/*Tests/*.csproj` glob pattern. Test results are saved in a *.trx* file at the location specified by the `--results-directory` option. If any tests fail, the build fails and isn't deployed.

    > [!NOTE]
    > To verify the unit tests work, modify *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* to purposefully break one of the tests. For example, change `Assert.True(result.Count > 0);` to `Assert.False(result.Count > 0);` in the `Returns_News_Stories_Given_Valid_Uri` method. Commit and push the change to GitHub. The build is triggered and fails. The build pipeline status changes to **failed**. Revert the change, commit, and push again. The build succeeds.

1. **Publish** &mdash; Executes the `dotnet publish --configuration release --output <local_path_on_build_agent>` command to produce a *.zip* file with the artifacts to be deployed. The `--output` option specifies the publish location of the *.zip* file. That location is specified by passing a [predefined variable](https://docs.microsoft.com/vsts/pipelines/build/variables) named `$(build.artifactstagingdirectory)`. That variable expands to a local path, such as *c:\agent\_work\1\a*, on the build agent.
1. **Publish Artifact** &mdash; Publishes the *.zip* file produced by the **Publish** task. The task accepts the *.zip* file location as a parameter, which is the predefined variable `$(build.artifactstagingdirectory)`. The *.zip* file is published as a folder named *drop*.

Click the build definition's **Summary** link to view a history of builds with the definition:

![build definition history](media/cicd/build-definition-summary.png)

On the resulting page, click the link corresponding to the unique build number:

![build definition summary page](media/cicd/build-definition-completed.png)

A summary of this specific build is displayed. Click the **Artifacts** tab, and notice the *drop* folder produced by the build is listed:

![build definition artifacts - drop folder](media/cicd/build-definition-artifacts.png)

Use the **Download** and **Explore** links to inspect the published artifacts.

#### Release pipeline

A release pipeline was created with the name *MyFirstProject-ASP.NET Core-CD*:

![release pipeline overview](media/cicd/release-definition-overview.png)

The two major components of the release pipeline are the **Artifacts** and the **Environments**. Clicking the box in the **Artifacts** section reveals the following panel:

![release pipeline artifacts](media/cicd/release-definition-artifacts.png)

The **Source (Build definition)** value represents the build definition to which this release pipeline is linked. The *.zip* file produced by a successful run of the build definition is provided to the *Production* environment for deployment to Azure. Click the *1 phase, 2 tasks* link in the *Production* environment box to view the release pipeline tasks:

![release pipeline tasks](media/cicd/release-definition-tasks.png)

The release pipeline consists of two tasks: *Deploy Azure App Service to Slot* and *Manage Azure App Service - Slot Swap*. Clicking the first task reveals the following task configuration:

![release pipeline deploy task](media/cicd/release-definition-task1.png)

The Azure subscription, service type, web app name, resource group, and deployment slot are defined in the deployment task. The **Package or folder** textbox holds the *.zip* file path to be extracted and deployed to the *staging* slot of the *mywebapp\<unique_number\>* web app.

Clicking the slot swap task reveals the following task configuration:

![release pipeline slot swap task](media/cicd/release-definition-task2.png)

The subscription, resource group, service type, web app name, and deployment slot details are provided. The **Swap with Production** checkbox is checked. Consequently, the bits deployed to the *staging* slot are swapped into the production environment.

### Additional reading

* [Create your first pipeline with Azure Pipelines](/azure/devops/pipelines/get-started-yaml)
* [Build and .NET Core project](/azure/devops/pipelines/languages/dotnet-core)
* [Deploy a web app with Azure Pipelines](/azure/devops/pipelines/targets/webapp)

## Monitor and debug

Having deployed the app and built a DevOps pipeline, it's important to understand how to monitor and troubleshoot the app.

In this section, you'll complete the following tasks:

* Find basic monitoring and troubleshooting data in the Azure portal
* Learn how Azure Monitor provides a deeper look at metrics across all Azure services
* Connect the web app with Application Insights for app profiling
* Turn on logging and learn where to download logs
* Stream logs in real time
* Learn where to set up alerts
* Learn about remote debugging Azure App Service web apps.

### Basic monitoring and troubleshooting

App Service web apps are easily monitored in real time. The Azure portal renders metrics in easy-to-understand charts and graphs.

1. Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.

1. The **Overview** tab displays useful "at-a-glance" information, including graphs displaying recent metrics.

    ![Overview panel](./media/monitoring/overview.png)

    * **Http 5xx**: Count of server-side errors, usually exceptions in ASP.NET Core code.
    * **Data In**: Data ingress coming into your web app.
    * **Data Out**: Data egress from your web app to clients.
    * **Requests**: Count of HTTP requests.
    * **Average Response Time**: Average time for the web app to respond to HTTP requests.

    Several self-service tools for troubleshooting and optimization are also found on this page.

    ![Self-service tools](./media/monitoring/wizards.png)

    * **Diagnose and solve problems** is a self-service troubleshooter.
    * **Application Insights** is for profiling performance and app behavior, and is discussed later in this section.
    * **App Service Advisor** makes recommendations to tune your app experience.

### Advanced monitoring

[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) is the centralized service for monitoring all metrics and setting alerts across Azure services. Within Azure Monitor, administrators can granularly track performance and identify trends. Each Azure service offers its own [set of metrics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) to Azure Monitor.

### Profile with Application Insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) is an Azure service for analyzing the performance and stability of web apps and how users use them. The data from Application Insights is broader and deeper than that of Azure Monitor. The data can provide developers and administrators with key information for improving apps. Application Insights can be added to an Azure App Service resource without code changes.

1. Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.
1. From the **Overview** tab, click the **Application Insights** tile.

    ![Application Insights tile](./media/monitoring/app-insights.png)

1. Select the **Create new resource** radio button. Use the default resource name, and select the location for the Application Insights resource. The location doesn't need to match that of your web app.

    ![Application Insights setup](./media/monitoring/new-app-insights.png)

1. For **Runtime/Framework**, select **ASP.NET Core**. Accept the default settings.
1. Select **OK**. If prompted to confirm, select **Continue**.
1. After the resource has been created, click the name of Application Insights resource to navigate directly to the Application Insights page.

    ![New Application Insights resource is ready](./media/monitoring/new-app-insights-done.png)

As the app is used, data accumulates. Select **Refresh** to reload the blade with new data.

![Application Insights overview tab](./media/monitoring/app-insights-overview.png)

Application Insights provides useful server-side information with no additional configuration. To get the most value from Application Insights, [instrument your app with the Application Insights SDK](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). When properly configured, the service provides end-to-end monitoring across the web server and browser, including client-side performance. For more information, see the [Application Insights documentation](https://docs.microsoft.com/azure/application-insights/app-insights-overview).

### Logging

Web server and app logs are disabled by default in Azure App Service. Enable the logs with the following steps:

1. Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.
1. In the menu to the left, scroll down to the **Monitoring** section. Select **Diagnostics logs**.

    ![Diagnostic logs link](./media/monitoring/logging.png)

1. Turn on **Application Logging (Filesystem)**. If prompted, click the box to install the extensions to enable app logging in the web app.
1. Set **Web server logging** to **File System**.
1. Enter the **Retention Period** in days. For example, 30.
1. Click **Save**.

ASP.NET Core and web server (App Service) logs are generated for the web app. They can be downloaded using the FTP/FTPS information displayed. The password is the same as the deployment credentials created earlier in this guide. The logs can be [streamed directly to your local machine with PowerShell or Azure CLI](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#download). Logs can also be [viewed in Application Insights](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).

### Log streaming

App and web server logs can be streamed in real time through the portal.

1. Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.
1. In the menu to the left, scroll down to the **Monitoring** section and select **Log stream**.

    ![Log stream link](./media/monitoring/log-stream.png)

Logs can also be [streamed via Azure CLI or Azure PowerShell](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), including through the Cloud Shell.

### Alerts

Azure Monitor also provides [real time alerts](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal) based on metrics, administrative events, and other criteria.

> *Note: Currently alerting on web app metrics is only available in the Alerts (classic) service.*

The [Alerts (classic) service](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) can be found in Azure Monitor or under the **Monitoring** section of the App Service settings.

![Alerts (classic) link](./media/monitoring/alerts.png)

### Live debugging

Azure App Service can be [debugged remotely with Visual Studio](https://docs.microsoft.com/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) when logs don't provide enough information. However, remote debugging requires the app to be compiled with debug symbols. Debugging shouldn't be done in production, except as a last resort.

### Conclusion

In this section, you completed the following tasks:

* Find basic monitoring and troubleshooting data in the Azure portal
* Learn how Azure Monitor provides a deeper look at metrics across all Azure services
* Connect the web app with Application Insights for app profiling
* Turn on logging and learn where to download logs
* Stream logs in real time
* Learn where to set up alerts
* Learn about remote debugging Azure App Service web apps.

### Additional reading

* [Troubleshoot ASP.NET Core on Azure App Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/azure-apps/troubleshoot)
* [Common errors reference for Azure App Service and IIS with ASP.NET Core](https://docs.microsoft.com/aspnet/core/host-and-deploy/azure-iis-errors-reference)
* [Monitor Azure web app performance with Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-azure-web-apps)
* [Enable diagnostics logging for web apps in Azure App Service](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log)
* [Troubleshoot a web app in Azure App Service using Visual Studio](https://docs.microsoft.com/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Create classic metric alerts in Azure Monitor for Azure services - Azure portal](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal)

## Chapter conclusion

With Azure App Service and Azure DevOps, ASP.NET Core developers have powerful, professional-grade tools to ...

<!-- TODO, obviously -->
