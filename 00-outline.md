# Holistic DevOps on Microsoft

The purpose of this book is to set the minimum bar to create a complete .NET development and devops environment on Azure. We assume that developers are experienced .NET developers with working knowledge of ASP.NET, SQL Server, and running these technologies on Windows Server. We don't assume that the reader has deep Azure experience. The emphasis is on providing .Net developers with the most  productive and high-velocity devops infrastructure and methods available. This book only covers the most common subset of Azure functionality required for implementing popular .NET architectures.  

1. Introduction/Overview
    * Purpose - small to mid-sized .Net teams
      * problems encountered by these teams
      * Stats on average age/experience of programmers
      * Stats on industry average productivity and quality of programmers
    * .Net and Azure - what parts are covered
      * List of application runtime types (web, job, database, mobile native, desktop, etc)
      * Arch. overview of example for the book
      * Walkthrough of deployment options for the application and some forces on why to choose what
    * DevOps - High level overview
      * Industry state of the art: Phoenix project, DevOps Handbook, Accelerate, state of devops report
      * DevOps environment architecture overview (diagram) w/ quick overview of the process
2. Zero to Azure in 60 Minutes (adapted existing content)
    * content adapted from other docs
3. The Professional-Grade DevOps Environment
    * Context of quality and productivity problems common in the industry
    * Methodologies that have contributed to the current state of the art
    * Sample app intro (ASP.NET Core & SQL database)
      * Azure subscription
      * Rules of thumb for how to set it up, level of isolation, etc. Security, network config
       * Visual Studio 2017
         * Options and why to choose Enterprise subscription
         * What types of add-ins to consider and encouraging to use the extension marketplace
       * SQL Server Express 2017
         * What benefits come from SQL Developer edition vs. express
         * How to set up SQL Express for development including TCP/IP network enablement
       * Azure DevOps Service organization
         * Setting up the organization and a project for working thro8ugh this book
         * Navigating the process templates
         * Naming projects, git repositories
         * Creating your board and hiding things until you need them
         * Encouragement about the marketplace and some interesting extensions
    * Fundamental requirements of a modern development environment (overview)
        - Tracking work
        - Tracking code
        - Building the code
        - Validating the code
        - Creating a release candidate
        - Provisioning/configuring server environment
        - Deploying the release
        - Operating/supporting the software release
     * Introduction to Azure DevOps Services & the different areas
4. Tracking work
    * Principles
      * DevOps principles like Make Work Visible
      * Create an assembly line of swim lanes where every column has an owner and a definition of done
      * Any column can be the source of a defect that is passed downstream, so each is a workcenter that needs to be managed
      * Each column needs to have explicit quality control steps to prevent defects from that workcenter to be passed down stream
      * Scrum-inherited process template and hiding unecessary work items
      * When to use PBI's, tasks, features, and epics
      * Metrics you should be getting from your project tracking
    * Tool setup (Azure Boards)
      * Implementation of each of the principles
      * Creating the development team
      * Creating the stakeholder team
    * Roles of the team & responsibilities
      * The consolidated engineering organization: stakeholders & engineers
      * Analysis - think up the thinks that might be done
      * Make decisions about how the implementation should be done (including test scenarios)
      * Develop
      * Release
      * Validation & customer service
    * 4+1 Architecture - how to identify and break down work
      * Iterative architecture
      * Process of creating and storing design artifacts in Azure Boards & Azure Repos
5. Tracking code
    * Principles
      * Everything is stored in source
      * One team per repository 
      * One versioned piece of software per repository
      * Linking to work items
      * Choosing branching
    * Tool setup (Azure Repos)
      * Walkthrough of the setup for our sample application
    * The .Net git repository structure
      * .Net Core example repo structure we/ directories for what purposes
      * .Net Framework example repo structure we/ directories for what purposes
    * How team members contribute code (branching/merging/pull request/review & work item tracability)
    * Automating release notes
6. Building the code
    * Principles
      * Entire repository builds together - if you don't want it to build together and get the same version number, it needs to be an a separate repository
      * Private build on dev workstation
      * Include the database from day 1
      * Include test suites from day 1
      * CI Build on ALL branches - and how to label them
      * Characteristics of a great build (fast, reliable, when it errors, it points to the problem)
    * Tool setup (Azure Pipelines & build script)
      * Powershell private build
      * CI Build configuration w/ variables (explanation of why to move away from the built-in step templates as fast as possible.
    * Using the private build
      * Steps required from the very first versions of the application
      * Creating database locally in the private build
    * Working with continuous integration
      * Hosted build vs. your own build agent
      * Pushing more logic into the git repository
      * Choosing between YAML and designer builds
    * Integrating database devops into CI
      * Planning ahead with the setup of your database
      * Marketplace options: Redgate ReadyRoll, AliaSQL, Roundhouse, etc.
7. Validating the code
    * Principles
      * No defects - no managing them - you squash them - if you aren't going to fix it, then call it expected, known behavior and a system limitation: Don't way "our product does this. . . but we have a bug so it really doesn't.  
      * Move test design ahead of coding - make it part of definition of done
      * Defect Removeal Efficiency as a measure of quality (Capers Jones research)
      * 3 QC activities: testing, inspections, static code analysis
      * Every stage of work needs validation
      * Measuring defects before release and after
    * Tool setup (test suites, code analysis, etc)
      * Where to track bugs
      * Setting up pull request inspections
      * Setting up 3+ levels of automated testing
      * Setting up levels of static code analysis
    * Levels of test automation (unit, integration, acceptance) 
      * deep dive and design considerations
      * Unit test - what's in, what's out
      * Integration tests - what's in, what's out
      * Full system tests - what's in, what's out, how to configure Selenium
      * Other test suits: security scanning, accessibility, performance, load (high scale), endurance (memory leaks)
    * Static code analysis
      * VS Code Analysis
      * ReSharper
      * NDepende
      * Sonarqube
      * Redgate SQL
    * Failing builds from validation problems
      * Invent ways to fail the build
   * Packaging test suites that need to be run in deployed environments
8. Creating a release candidate
    * Principles
    * Tool setup (Azure Artifacts)
    * Versioning
    * Designing packaging for application deployment
    * Packaging the web application
    * Packaging the database
9. Provisioning/configuring server environment
    * Principles
    * Tool setup (Azure subscription)
    * Infrastructure as Code and immutable infrastructure 
    * Azure ARM
    * Azure CLI
    * Provisioning environments for our application
10. Deploying the release
    * Principles
    * Tool setup (Azure Pipelines release hub)
    * Confinguring our pipeline (3 environments deep)
    * Deploying PaaS w/ config
    * Deploying IaaS w/ Azure VMs
    * Allowing stakeholders to approve production
    * Cold-start and smoke-testing tactics
11. Operating, monitoring & DevOps Diagnostics
    * Principles
    * Tool setup (AppInsights, OMS, etc)
    * Designed for "observability" 
    * Monitoring/alerts in the Azure portal
    * Logging and log centralization with OMS
    * Integrating an incident management workflow (Pick a partner, perhaps PagerDuty, who has VSTS integration.)
12. Modernizing a full framework app to DevOps   
    * Principles
    * Tool setup (.Net 4.6.2 (netstandard2))
    * Walkthrough of full framework solution template enabled for DevOps
    * Caveats and holes for reader to fill in
13. How to make an architectural change in a DevOps environment
    * Principles (design considerations) (add off-line job to application)
    * Architectural options overview
    * Scheduled job as webjob
    * Queue-triggered as Azure function
    * SQL-table polling as Windows service on VM
    * Designing queue-triggered services for scale (2 of them)
14. Advanced deployment options
    * Principles (design options)
    * Azure service fabric
    * Scaled app service
    * AKS
    * SQL Managed instances for complex databases
15. Conclusion
    * Review of architecture/build/testing loop
    * Review "rules of thumb" or 80/20 rule
    * Cover resources for further study
