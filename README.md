# ionic-app-hosted-in-azure

*******
Tables of contents  
 1. [Project Architecture](#projectArchitecture )
 2. [Set up Pipeline](#SetupPipeline)

*******

<div id='projectArchitecture'/>

### 1. Project Architecture
a. Diagram
<br/>
<img src="./images/ionic-app-architecture-diagram.jpg" width="800" height="500">

b. Data flow
	
1. User submit request approval
2. Ets.api call Power Automate to send Teams notification in the flow
3. User visit ETS mobile app via link from Teams
4. User go to AAD login page to sign in with username & password (implicit flow & access token lifetime around 60-75 mins) with browser. Once authenticated, AAD Server return access token
5. Angular app send request to java middleware to get results  
6. Backend app service validate access token by audience & scope & use email decoding from token as a parameter
7. If token is valid, users can view the results


c. Use OAuth 2.0 implicit grant flow

d. Azure Cost Estimate
<br/>
<img src="./images/ionic-app-azure-cost-estimate.jpg" width="600" height="400">


<div id='SetupPipeline'/>  

### 2. Set up Pipeline
1. Pipeline Diagram
<br/>
<img src="./images/ionic-app-pipeline-design-diagram.jpg" width="800" height="500">

2. Data flow
Dataflow

1. Developers change application source code.
2. Application code including the configuration file is committed to the source code repository in Azure Repos.
3. Continuous integration (DEV) triggers unit tests, security scanning & application build.
4. Continuous deployment within Azure Pipelines triggers an deployment application artifacts for DEV environment manually.
5. The artifact with DEV configurations (War file) is deployed to Azure App Service & Blob Storage.
6. Continuous integration (UAT / PROD) triggers unit tests & application build.
7. Continuous deployment within Azure Pipelines triggers an deployment application artifacts for UAT / PROD environment manually.
8. The artifact with UAT Configurations (Jar file) is deployed to Azure App Service & Blob Storage
9. The deployment seeks approval before proceed.
10. When approved, the deployment with configuration injected from key vault (Jar file) is deployed to Azure App Service & Blob Storage
