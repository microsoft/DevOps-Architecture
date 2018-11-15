# Professional DevOps on Azure for .NET

The purpose of this content is to guide Visual Studio developers in falling into the "pit of success" when developing and shipping software on Azure. Going beyond the quickstarts, this content will show developers how to pull all the technologies together to create and maintain a truly professional DevOps environment in Azure. We targeting an experienced .NET developer audience with working knowledge of ASP.NET, SQL Server, and Windows Server. We don't assume that the audience already has deep Azure experience, but will expect the audience to implement new resources in their Azure subscription using Powershell and other automating methods. The target audience will take away from this content a complete, professional-grade DevOps environment (from source control all the way through production diagnostics) that can be applied to any of their existing or future .NET software applications. 

Target publishing format will be docs.microsoft.com, as an MS Learn path if possible.  Localized.  It is also intended to be available in printed form as a book.

1. Introduction/Overview
    * Purpose - small to mid-sized .NET teams
      * problems encountered by these teams
      * Stats on average age/experience of programmers
      * Stats on industry average productivity and quality of programmers
    * .NET and Azure - what parts are covered
      * List of application runtime types (web, job, database, mobile native, desktop, etc)
      * Arch. overview of example for the book
      * Walkthrough of deployment options for the application and some forces on why to choose what
    * DevOps - High level overview
      * Industry state of the art: Phoenix project, DevOps Handbook, Accelerate, state of devops report
      * DevOps environment architecture overview (diagram) w/ quick overview of the process
2. Zero to Azure in 60 Minutes (adapted existing content)
    * Deploying to Azure App Service
    * Continuous Integration and Deployment
    * Monitoring and Debugging
3. The Professional-Grade DevOps Environment
    * Methodologies that have contributed to the current state of the art
        * Agile 2001
        * CI 2006 (Addision Wesley book)
            * CruiseControl.NET
        * CD 2009 (Addision Wesley book)
        * Working Effectively with Legacy Code (Michael Feathers)
        * Phoenix Project, DevOps Handbook (Jez Humble, etc)
        * DevOps (since 2010, but still misunderstood)
            * The ways of DevOps.
    * Sample app intro (ASP.NET Core & SQL database)
        * .NET Core 3. quick walkthrough of our application shell - setting ourselves up for the ability to write features.
        * Azure subscription - PaaS
          - rule of thumb is new Azure subscription for a new software system. May combine pre-production if multiple applications owned by same team. Could other mechanism work? Yes. Once you know why you are doing it.
    * Professional Tools
       * Visual Studio 2017 Ent. subscription.
         * Options and why to choose Enterprise subscription
         * What types of add-ins to consider and encouraging to use the extension marketplace
       * SQL Server Express 2017 min.  SQL Developer edition
         * What benefits come from SQL Developer edition vs. express
         * How to set up SQL Express for development including TCP/IP network enablement
       * Azure DevOps Service organization (Introduction to Azure DevOps Services & the different areas)
         * Setting up the organization and a project for working thro8ugh this book
         * Navigating the process templates - overview of process templates - doesn't matter which one you choose because you will be redoing it anyway.
         * Naming projects, git repositories - no spaces in the project name.
         * Encouragement about the marketplace and some interesting extensions
            * Octopus Deploy
            * Redgate (database)
            * DevLab
    * Fundamental requirements of a modern development environment (overview)
        * Principles: short lead time, short cycle time, defects prevented at every stage
        * Tracking work
        * Tracking code
        * Building the code
        * Validating the code
        * Creating a release candidate
        * Provisioning/configuring server environment
        * Deploying the release
        * Operating/supporting the software release
    
4. Tracking work
    * Principles
      * DevOps principles like Make Work Visible
      * Create an assembly line of swim lanes where every column has an owner and a definition of done
      * Any column can be the source of a defect that is passed downstream, so each is a workcenter that needs to be managed
      * Each column needs to have explicit quality control steps to prevent defects from that workcenter to be passed down stream
      * Scrum-inherited process template and hiding unnecessary work items
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
    * The .NET git repository structure
      * .NET Core example repo structure we/ directories for what purposes
      * .NET Framework example repo structure we/ directories for what purposes
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
    * Integrating database DevOps into CI
      * Planning ahead with the setup of your database
      * Marketplace options: Redgate ReadyRoll, AliaSQL, Roundhouse, etc.
7. Validating the code
    * Principles
      * No defects - no managing them - you squash them - if you aren't going to fix it, then call it expected, known behavior and a system limitation: Don't way "our product does this. . . but we have a bug so it really doesn't.  
      * Move test design ahead of coding - make it part of definition of done
      * Defect Removal Efficiency as a measure of quality (Capers Jones research)
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
    * Configuring our pipeline (3 environments deep)
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
    * Tool setup (.NET 4.6.2 (netstandard2))
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
    * AKS & Containers
    * SQL Managed instances for complex databases
15. Conclusion
    * Review of architecture/build/testing loop
    * Review "rules of thumb" or 80/20 rule
    * Cover resources for further study
